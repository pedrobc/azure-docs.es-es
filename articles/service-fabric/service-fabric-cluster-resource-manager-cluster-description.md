---
title: "Descripción del clúster de Cluster Resource Manager | Microsoft Docs"
description: "Descripción de un clúster de Service Fabric mediante la especificación de dominios de error, dominios de actualización, propiedades de nodo y funcionalidades de nodo en Cluster Resource Manager."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 55f8ab37-9399-4c9a-9e6c-d2d859de6766
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: dde9d9b8be1faede7d2e9e45597070e6ce51ac02
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="describing-a-service-fabric-cluster"></a>Descripción de un clúster de Service Fabric
Service Fabric Cluster Resource Manager proporciona varios mecanismos para describir un clúster. Durante el tiempo de ejecución, Cluster Resource Manager usa esta información para garantizar la alta disponibilidad de los servicios que se ejecutan en el clúster. Al aplicar estas reglas importantes, también trata de optimizar el consumo de recursos del clúster.

## <a name="key-concepts"></a>Conceptos clave
Cluster Resource Manager es compatible con varias de las características que describen un clúster:

* Dominios de error
* Dominios de actualización
* Propiedades del nodo
* Capacidades de nodo

## <a name="fault-domains"></a>Dominios de error
Un dominio de error es cualquier área de error coordinado. Una única máquina constituye un dominio de error (ya que ella sola puede dejar de funcionar por muchas razones diferentes, desde errores en el sistema de alimentación hasta unidades averiadas o un firmware de NIC defectuoso). Varias máquinas conectadas al mismo conmutador Ethernet se encuentran en el mismo dominio de error, al igual que aquellas que estén conectadas a una sola fuente de energía o en una única ubicación. Como es natural que coincidan averías de hardware, los dominios de error son intrínsecamente jerárquicos y se representan como identificadores URI en Service Fabric.

Es importante que los dominios de error se configuren correctamente porque Service Fabric usa esta información para colocar servicios de forma segura. Service Fabric no quiere volver a colocar los servicios de forma que la pérdida de un dominio de error (causado por la avería de algún componente) provoque que un servicio deje de funcionar. En el entorno de Azure, Service Fabric aprovecha la información sobre dominios de error que proporciona dicho entorno para configurar automáticamente correctamente los nodos del clúster. Para Service Fabric independiente, los dominios de error se definen en el momento en que se configura el clúster. 

> [!WARNING]
> Es importante que la información de dominio de error proporcionada a Service Fabric sea precisa. Por ejemplo, supongamos que los nodos del clúster de Service Fabric se ejecutan dentro de 10 máquinas virtuales, en cinco hosts físicos. En este caso, a pesar de que hay 10 máquinas virtuales, hay solo 5 dominios de error (nivel superior) diferentes. Compartir el mismo host físico hace que las VM compartan el mismo dominio de error de raíz, dado que las máquinas virtuales experimentan un error coordinado si se produce un error en su host físico.  
>
> Service Fabric espera que el dominio de error de un nodo no cambie. Otros mecanismos de garantizar la alta disponibilidad de las VM, como [VM de alta disponibilidad](https://technet.microsoft.com/en-us/library/cc967323.aspx), usan la migración transparente de VM de un host a otro. Estos mecanismos no reconfiguran o notifican el código de ejecución dentro de la VM. Por lo tanto, **no se admiten** como entornos para ejecutar clústeres de Service Fabric. Service Fabric debe ser la única tecnología de alta disponibilidad empleada. Los mecanismos como la migración en vivo de VM, SAN u otros no son necesarios. Si se usa junto con Service Fabric, estos mecanismos _reducen_ la confiabilidad y la disponibilidad de las aplicaciones, ya que agregan una complejidad adicional, añaden orígenes centralizados de errores y utilizan estrategias de confiabilidad y disponibilidad que entran en conflicto con las de Service Fabric. 
>
>

En el siguiente gráfico, se muestran coloreadas todas las entidades que contribuyen a la generación de dominios de error y todos los que se crean. En este ejemplo, tenemos centros de datos (DC), bastidores (R) y servidores blade (B). Posiblemente, si cada servidor blade contiene más de una máquina virtual, podría haber otra capa en la jerarquía de dominios de error.

<center>
![Nodos organizados a través de dominios de error][Image1]
</center>

Durante el tiempo de ejecución, la utilidad Cluster Resource Manager de Service Fabric tiene en cuenta los dominios de error del clúster y planea diseños. Las réplicas con estado o las instancias sin estado de un servicio determinado se distribuyen de manera que estén en dominios de error independientes. La distribución del servicio en dominios de errores garantiza que la disponibilidad del servicio no se ve comprometida cuando se produce un fallo del dominio de error en cualquier nivel de la jerarquía.

La utilidad Cluster Resource Manager de Service Fabric no tiene en cuenta cuántos niveles hay en la jerarquía de dominios de error. Sin embargo, sí que trata de asegurarse de que la pérdida de cualquier parte de la jerarquía no afecte a los servicios que se ejecutan en ella. 

Es mejor si hay el mismo número de nodos en cada nivel de profundidad de la jerarquía de dominios de error. Si el "árbol" de dominios de error no está equilibrado en el clúster, Cluster Resource Manager tendrá más complicado determinar la mejor asignación de servicios. Los diseños de dominios de error desequilibrados provocan que la pérdida de algunos dominios afecte más a la disponibilidad de los servicios que otros dominios. Como resultado, Cluster Resource Manager se debate entre sus dos objetivos: usar las máquinas de ese dominio "pesado" colocando servicios en ellos, y colocando servicios en otros dominios de forma que la pérdida de un dominio no cause problemas. 

¿Qué aspecto tienen los dominios desequilibrados? En el diagrama siguiente, se muestran dos diseños de clúster diferentes. En el primero, los nodos se distribuyen uniformemente entre los dominios de error. En el segundo ejemplo, un dominio de error tiene muchos más nodos que los demás dominios de error. 

<center>
![Dos diseños de clúster distintos][Image2]
</center>

En Azure, será su responsabilidad elegir qué dominio de error contendrá un nodo. Sin embargo, dependiendo del número de nodos que se aprovisionen, puede seguir obteniendo dominios de error con más nodos que otros. Por ejemplo, supongamos que tiene cinco dominios de error en el clúster, pero aprovisiona siete nodos para un determinado objeto NodeType. En este caso, los dos primeros dominios de error acaban con más nodos. Si trata de implementar más objetos NodeType con solo dos instancias, el problema será más grave. Por esta razón, se recomienda que el número de nodos en cada tipo de nodo sea un múltiplo del número de dominios de error.

## <a name="upgrade-domains"></a>Dominios de actualización
Los dominios de actualización son otra característica que ayuda a la utilidad Resource Manager Cluster de Service Fabric a comprender el diseño del clúster. Estos definen conjuntos de nodos que se actualizan al mismo tiempo. Los dominios de actualización ayudan a Cluster Resource Manager a entender y coordinar las operaciones de administración como las actualizaciones.

Los dominios de actualización son muy similares a los dominios de error, pero con algunas diferencias clave. En primer lugar, las áreas de los errores de hardware coordinados definen los dominios de error. Por otro lado, los dominios de actualización se definen mediante la directiva. Podrá decidir cuántos desea, a diferencia de los de error, que el número se decida en función del entorno. Puede tener tantos dominios de actualización como ocurre con los nodos. Otra diferencia entre los dominios de error y los dominios de actualización es que los dominios de actualización no son jerárquicos. En su lugar, son más parecidos a una etiqueta sencilla. 

En el siguiente diagrama se muestran tres dominios de actualización divididos en tres dominios de error. También se muestra una posible selección de ubicación de tres réplicas diferentes de un servicio con estado, donde cada una de ellas terminan con diferentes dominios de error y de actualización. Esta selección de ubicación permite perder un dominio de error en el transcurso de una actualización de servicio y seguir teniendo una copia del código y los datos.  

<center>
![Selección de ubicación con dominios de error y de actualización][Image3]
</center>

Tener un gran número de dominios de actualización tiene ventajas y desventajas. Disponer de más dominios de actualización implica que cada paso de la actualización sea más granular y, por tanto, afecta a un número menor de nodos o servicios. Como consecuencia, hay que mover menos servicios a la vez, lo que reduce la actividad en el sistema. Esto suele mejorar la confiabilidad general (ya que los problemas afectarán a una proporción menor del servicio durante la actualización). Si tiene más dominios de actualización, también necesitará menos búfer disponible en otros nodos para controlar el impacto de la actualización. Por ejemplo, si dispone de cinco dominios de actualización, los nodos de cada uno de ellos controlarán aproximadamente el 20 % del tráfico. Si necesita desactivar ese dominio de actualización para realizar una actualización, esa carga normalmente debe ir a otro lado. Puesto que tiene cuatro dominios de actualización restantes, cada uno debe tener un espacio aproximado del 5 % del tráfico total. Disponer de más dominios de actualización implica que necesita menos búfer en los nodos en el clúster. Por ejemplo, considere si había 10 dominios de actualización en su lugar. En ese caso, cada UD solo puede controlar aproximadamente el 10 % del tráfico total. Cuando se realiza una actualización en el clúster, cada dominio solo tendría que tener espacio para aproximadamente un 1,1 % del tráfico total. Disponer de más dominios de actualización le permite normalmente ejecutar nodos con una utilización mayor, ya que necesita una capacidad de reserva menor. Lo mismo ocurre con los dominios de error.  

El inconveniente de tener muchos dominios de actualización es que las actualizaciones suelen tardar más tiempo. Service Fabric espera un breve período después de que se complete un dominio de actualización y realiza comprobaciones antes de iniciar la actualización del siguiente. Estos retrasos permiten detectar problemas introducidos por la actualización antes de continuar con la actualización. El compromiso es aceptable, ya que impide que los cambios incorrectos afecten a una proporción excesiva del servicio a la vez.

Usar demasiado pocos dominios de actualización también tiene muchos efectos secundarios: mientras que cada dominio de actualización esté inactivo y actualizándose, una gran parte de la capacidad general no estará disponible. Por ejemplo, si solo dispone de tres dominios de actualización, estará inactiva un tercio de la capacidad general del servicio o del clúster al mismo tiempo. Tener una gran parte del servicio sin actividad al mismo tiempo no es recomendable, ya que hay que contar con suficiente capacidad en el resto del clúster para controlar la carga de trabajo. Mantener el búfer implica que durante el funcionamiento normal, los nodos se carguen menos de lo que deberían. Esto aumenta el costo de ejecución del servicio.

No existe ningún límite real en el número total de dominios de error o de actualización en un entorno, ni tampoco restricciones a la forma en que se superponen. Es decir, hay varios patrones comunes:

- Asignación 1:1 de dominios de error y de actualización
- Un dominio de actualización por nodo (instancia del sistema operativo física o virtual)
- Un modelo "seccionado" o "de matriz", donde los dominios de error y de actualización forman una matriz con máquinas que se ejecutan abajo de las diagonales.

<center>
![Diseños de dominio de error y de actualización][Image4]
</center>

No existe un diseño ideal, sino que cada uno tiene sus ventajas e inconvenientes. Por ejemplo, el modelo 1FD:1UD es fácil de configurar. El modelo de 1 dominio de actualización por nodo es el más utilizado por los usuarios. Durante las actualizaciones, cada nodo se actualiza de forma independiente. Esto es similar a cómo se actualizaban manualmente los conjuntos pequeños de máquinas en el pasado. 

El modelo más habitual es la matriz FD/UD, en la que los FD (dominios de error) y los UD forman una tabla donde los nodos se colocan empezando por la diagonal. Este es el modelo que se utiliza de forma predeterminada en clústeres de Service Fabric en Azure. Para los clústeres con muchos nodos, todo tiene un aspecto parecido al patrón de matriz denso de arriba.

## <a name="fault-and-upgrade-domain-constraints-and-resulting-behavior"></a>Restricciones de dominio de error y de actualización y el comportamiento resultante
Cluster Resource Manager trata el deseo de mantener un servicio en equilibrio entre los dominios de error y de actualización como una restricción. Para más información sobre las restricciones, vea [este artículo](service-fabric-cluster-resource-manager-management-integration.md). Las restricciones de dominios de error y de actualización se definen de esta forma: "para una partición de servicio específica, nunca tiene que haber una diferencia *mayor que uno* en el número de réplicas (instancias de servicios sin estado o réplicas de servicios con estado) entre dos dominios". Esto evita determinados movimientos o disposiciones que infringen esta restricción.

Veamos un ejemplo. Supongamos que tenemos un clúster con 6 nodos, configurado con 5 dominios de error y 5 dominios de actualización.

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | | |N3 | | |
| **UD3** | | | |N4 | |
| **UD4** | | | | |N5 |

Ahora supongamos que creamos un servicio con un TargetReplicaSetSize (o, para un servicio sin estado un InstanceCount) de cinco. Las réplicas se dirigen a N1-N5. De hecho, nunca se usará N6, independientemente del número de servicios como este que cree. Pero ¿por qué? Echemos un vistazo a la diferencia entre el diseño actual y lo que sucedería si se elige N6.

Este es el diseño que obtuvimos y el número total de las réplicas por dominio de error y de actualización:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** | |R2 | | | |1 |
| **UD2** | | |R3 | | |1 |
| **UD3** | | | |R4 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

Este diseño se equilibra en términos de nodos por dominio de error y de actualización. También está equilibrado en cuanto al número de réplicas por dominios de error y de actualización. Cada dominio tiene el mismo número de nodos y el mismo número de réplicas.

Ahora, echemos un vistazo a lo que sucedería si hubiéramos utilizado N6 en lugar de N2. ¿Cómo se distribuirían las réplicas entonces?

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R5 | | | | |1 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |2 |0 |1 |1 |1 |- |

Este diseño no cumple la definición de la restricción de dominio de error. FD0 tiene 2 réplicas, mientras que FD1 tiene 0, por lo que la diferencia entre FD0 y FD1 es 2. Cluster Resource Manager no permite este tipo de disposición. De forma similar, si se ha seleccionado N2 y N6 (en lugar de N1 y N2), obtendríamos lo siguiente:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** | | | | | |0 |
| **UD1** |R5 |R1 | | | |2 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

Este diseño se equilibra en cuanto a dominios de error. Sin embargo, ahora infringe la restricción de actualización de dominio. Esto es porque UD0 tiene cero réplicas, mientras que UD1 tiene dos. Por lo tanto, este diseño tampoco es válido y Cluster Resource Manager no puede seleccionarlo. 

## <a name="configuring-fault-and-upgrade-domains"></a>Configuración de dominios de error y de actualización
La definición de dominios de error y de actualización se realiza automáticamente en las implementaciones de Service Fabric hospedadas en Azure. Service Fabric se limita a recopilar la información sobre el entorno de Azure.

Si va a crear su propio clúster (o quiere ejecutar una determinada topología en desarrollo), puede proporcionar la información de dominios de error y de actualización. En este ejemplo, definimos un clúster de desarrollo local de nueve nodos que abarca tres centros de datos (cada uno con tres bastidores). Este clúster también tiene tres dominios de actualización seccionados en esos tres centros de datos. A continuación se muestra un ejemplo de la configuración: 

ClusterManifest.xml

```xml
  <Infrastructure>
    <!-- IsScaleMin indicates that this cluster runs on one-box /one single server -->
    <WindowsServer IsScaleMin="true">
      <NodeList>
        <Node NodeName="Node01" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType01" FaultDomain="fd:/DC01/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node02" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType02" FaultDomain="fd:/DC01/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node03" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType03" FaultDomain="fd:/DC01/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node04" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType04" FaultDomain="fd:/DC02/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node05" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType05" FaultDomain="fd:/DC02/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node06" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType06" FaultDomain="fd:/DC02/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node07" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType07" FaultDomain="fd:/DC03/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node08" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType08" FaultDomain="fd:/DC03/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node09" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType09" FaultDomain="fd:/DC03/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
      </NodeList>
    </WindowsServer>
  </Infrastructure>
```

a través de ClusterConfig.json para las implementaciones independientes

```json
"nodes": [
  {
    "nodeName": "vm1",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm2",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm3",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm4",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm5",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm6",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm7",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm8",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm9",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD3"
  }
],
```

> [!NOTE]
> Al definir los clústeres a través de Azure Resource Manager, Azure asigna dominios de error y dominios de actualización. Por lo tanto, la definición de los tipos de nodos y conjuntos de escalado de máquinas virtuales en la plantilla de Azure Resource Manager no incluye información del dominio de error o de actualización.
>

## <a name="node-properties-and-placement-constraints"></a>Restricciones de ubicación y propiedades de nodo
A veces (de hecho, la mayor parte del tiempo) le va a interesar asegurarse de que ciertas cargas de trabajo solo se ejecuten en determinados tipos de nodos en el clúster. Por ejemplo, algunas cargas de trabajo pueden requerir GPU o SSD, al contrario que otras. Un buen ejemplo de esto es prácticamente cualquier arquitectura de n niveles, Determinadas máquinas sirven de front-end o lado de la aplicación a la API y se exponen a los clientes o a Internet. Otras máquinas, a menudo con recursos de hardware diferentes, se encargan del trabajo de las capas de proceso o almacenamiento. Estas _no_ se suelen exponer directamente a los clientes o a Internet. Service Fabric espera que haya casos en que se deban ejecutar cargas de trabajo concretas en configuraciones de hardware específicas. Por ejemplo:

* una aplicación de n niveles existente se ha transferido "tal cual" a un entorno de Service Fabric;
* se prefiere ejecutar una carga de trabajo en un hardware específico por motivos de rendimiento, escala o aislamiento de seguridad;
* una carga de trabajo debe estar aislada de otras por una directiva o a causa del consumo de recursos.

Para admitir estos tipos de configuraciones, Service Fabric parte de una noción de etiquetas de primera clase que pueden aplicarse a los nodos. Estas etiquetas se denominan **propiedades del nodo**. Las **restricciones de posición** son las instrucciones adjuntas a los servicios individuales que se seleccionan para una o más propiedades de nodo. Las restricciones de posición definen dónde deben ejecutarse los servicios. El conjunto de restricciones es extensible; puede funcionar cualquier par clave-valor. 

<center>
![Diferentes cargas de trabajo por diseño de clúster][Image5]
</center>

### <a name="built-in-node-properties"></a>Propiedades del nodo integradas
Además, Service Fabric define algunas propiedades predeterminadas que se pueden usar automáticamente sin que el usuario tenga que definirlas. Las propiedades predeterminadas definidas en cada nodo son **NodeType** y **NodeName**. Por ejemplo, podría escribir una restricción de selección de ubicación como `"(NodeType == NodeType03)"`. Por lo general, NodeType es una de las propiedades más usadas. Es útil, ya que corresponde a 1:1 con un tipo de una máquina. Cada tipo de máquina corresponde a un tipo de carga de trabajo en una aplicación de n niveles tradicional.

<center>
![Restricciones de selección de ubicación y propiedades de nodo][Image6]
</center>

## <a name="placement-constraint-and-node-property-syntax"></a>Restricciones de colocación y sintaxis de propiedades de nodo 
El valor especificado en la propiedad de nodo puede ser una cadena, un booleano o de tipo Long con signo. La instrucción en el servicio se denomina "*restricción* de selección de ubicación", puesto que limita el lugar del clúster donde puede ejecutarse el servicio. La restricción puede ser cualquier instrucción booleana que opera en las diferentes propiedades del nodo en el clúster. Los selectores válidos de estas instrucciones booleanas son los siguientes:

1) comprobaciones condicionales para crear instrucciones concretas

| Instrucción | Sintaxis |
| --- |:---:|
| "equal to" | "==" |
| "not equal to" | "!=" |
| "greater than" | ">" |
| "greater than or equal to" | ">=" |
| "less than" | "<" |
| "less than or equal to" | "<=" |

2) Instrucciones booleanas para operaciones lógicas y de agrupación

| Instrucción | Sintaxis |
| --- |:---:|
| "and" | "&&" |
| "or" | "&#124;&#124;" |
| "not" | "!" |
| "group as single statement" | "()" |

Estos son algunos ejemplos de instrucciones de restricción básicas.

  * `"Value >= 5"`
  * `"NodeColor != green"`
  * `"((OneProperty < 100) || ((AnotherProperty == false) && (OneProperty >= 100)))"`

El servicio se puede colocar solo en los nodos donde la instrucción de restricción de colocación general se evalúa como "True". Los nodos que no tengan definida ninguna propiedad no coinciden con ninguna restricción de selección de ubicación que contenga esa propiedad.

Supongamos que se definieron las siguientes propiedades de nodo para un tipo de nodo determinado:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType01">
      <PlacementProperties>
        <Property Name="HasSSD" Value="true"/>
        <Property Name="NodeColor" Value="green"/>
        <Property Name="SomeProperty" Value="5"/>
      </PlacementProperties>
    </NodeType>
```

a través de ClusterConfig.json para las implementaciones independientes o Template.json para los clústeres hospedados en Azure. 

> [!NOTE]
> En la plantilla de Azure Resource Manager se parametriza normalmente el tipo de nodo. Tendría el siguiente aspecto "[parameters('vmNodeType1Name')]", en lugar de "NodeType01".
>

```json
"nodeTypes": [
    {
        "name": "NodeType01",
        "placementProperties": {
            "HasSSD": "true",
            "NodeColor": "green",
            "SomeProperty": "5"
        },
    }
],
```

Puede crear *restricciones* de selección de ubicación para un servicio de forma parecida a esta:

C#

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
serviceDescription.PlacementConstraints = "(HasSSD == true && SomeProperty >= 4)";
// add other required servicedescription fields
//...
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceType -Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementConstraint "HasSSD == true && SomeProperty >= 4"
```

Si todos los nodos de NodeType01 son válidos, también puede seleccionar el tipo de nodo con la restricción "(NodeType == NodeType01)".

Una de las cosas interesantes sobre las restricciones de colocación de un servicio es que se pueden actualizar dinámicamente durante el tiempo de ejecución. Así que, si es necesario, puede mover un servicio por el clúster, agregar y quitar requisitos, etc. Service Fabric se encarga de garantizar que el servicio siempre esté en funcionamiento y disponible incluso cuando se estén produciendo estos tipos de cambios.

C#:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.PlacementConstraints = "NodeType == NodeType01";
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

Powershell:

```posh
Update-ServiceFabricService -Stateful -ServiceName $serviceName -PlacementConstraints "NodeType == NodeType01"
```

Las restricciones de colocación se especifican para cada instancia de servicio con nombre diferente. Las actualizaciones siempre asumen el lugar o sobrescriben lo que se especificó antes.

La definición del clúster define las propiedades en un nodo. Cambiar las propiedades de un nodo requiere una actualización de la configuración del clúster. Actualizar las propiedades de un nodo requiere que cada nodo afectado se reinicie para informar sobre sus nuevas propiedades. Service Fabric administra estas actualizaciones sucesivas.

## <a name="describing-and-managing-cluster-resources"></a>Descripción y administración de recursos de clúster
Uno de los trabajos más importantes de cualquier organizador es ayudar a administrar el consumo de recursos del clúster. La administración de recursos de clúster puede significar un par de cosas diferentes. En primer lugar, hay es asegurarse de que las máquinas no estén sobrecargadas. Esto significa asegurarse de que las máquinas no estén ejecutando más servicios de los que puede administrar. En segundo lugar, no hay equilibrio y la optimización que es esencial para la ejecución funciona de forma eficaz. Las ofertas de servicios que tienen en cuenta en rendimiento o la rentabilidad no pueden permitir que algunos nodos se activen cuando otros estén inactivos. Los activos provocan la contención de recursos y un rendimiento bajo, y los inactivos están relacionados con el aumento de los costos y el desperdicio de los recursos. 

Service Fabric representa los recursos como `Metrics`. Las métricas son cualquier recurso físico o lógico que desee describir a Service Fabric. Algunos ejemplos de métricas son elementos como "WorkQueueDepth" o "MemoryInMb". Para obtener información acerca de los recursos físicos que puede administrar Service Fabric en los nodos, vea el [regulador de recursos](service-fabric-resource-governance.md). Para obtener información sobre la configuración de métricas personalizadas y sus usos, vea [este artículo](service-fabric-cluster-resource-manager-metrics.md).

Las métricas se diferencian de las restricciones de selección ubicación y de las propiedades de nodo Las propiedades de nodo son descriptores estáticos de los nodos por sí mismos. Las métricas describen recursos que tienen nodos y que los servicios usan cuando se ejecutan en un nodo. Una propiedad de nodo podría ser "HasSSD" y tener un valor de True o False. La cantidad de espacio disponible en ese SSD y la cantidad consumida por los servicios sería una métrica como "DriveSpaceInMb". 

Es importante tener en cuenta que, al igual que para las restricciones de selección de ubicación y las propiedades de nodo, la utilidad Cluster Resource Manager de Service Fabric no comprende lo que significan los nombres de las métricas, ya que son simplemente cadenas. Se recomienda declarar las unidades como parte de los nombres de métricas que cree cuando puedan ser ambiguos.

## <a name="capacity"></a>Capacity
Si desactivó todo el *equilibrio*de recursos, la utilidad Cluster Resource Manager de Service Fabric aún podría asegurarse de que ningún nodo terminara superando su capacidad. Es posible administrar saturaciones de capacidad, a no ser que el clúster esté demasiado lleno o la carga de trabajo sea mayor que cualquier nodo. La capacidad es otra *restricción* que utiliza Cluster Resource Manager para entender cuánto de un recurso tiene un nodo. También se realiza un seguimiento de la capacidad restante en el clúster. Tanto la capacidad como el consumo en el nivel de servicio se expresan con métricas. Por ejemplo, la métrica podría ser "ClientConnections", y un nodo determinado puede tener una capacidad "ClientConnections" de 32768. Otros nodos pueden tener otros límites. Algún servicio que se ejecuta en ese nodo puede decir que está actualmente consumiendo 32256 de la métrica "ClientConnections".

Durante el tiempo de ejecución, Cluster Resource Manager realiza un seguimiento de capacidad restante del clúster y en los nodos. Para realizar un seguimiento de la capacidad, Cluster Resource Manager resta el uso de cada servicio de la capacidad del nodo donde se ejecuta el servicio. Con esta información, la utilidad Resource Manager de Service Fabric puede averiguar dónde colocar o mover las réplicas para que los nodos no superen su capacidad.

<center>
![Nodos del clúster y capacidad][Image7]
</center>

C#:

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
ServiceLoadMetricDescription metric = new ServiceLoadMetricDescription();
metric.Name = "ClientConnections";
metric.PrimaryDefaultLoad = 1024;
metric.SecondaryDefaultLoad = 0;
metric.Weight = ServiceLoadMetricWeight.High;
serviceDescription.Metrics.Add(metric);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ClientConnections,High,1024,0)
```

Puede ver las capacidades definidas en el manifiesto de clúster:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType03">
      <Capacities>
        <Capacity Name="ClientConnections" Value="65536"/>
      </Capacities>
    </NodeType>
```

a través de ClusterConfig.json para las implementaciones independientes o Template.json para los clústeres hospedados en Azure. 

```json
"nodeTypes": [
    {
        "name": "NodeType03",
        "capacities": {
            "ClientConnections": "65536",
        }
    }
],
```

Normalmente, una carga de un servicio cambia de forma dinámica. Pongamos que la carga de la réplica de "ClientConnections" cambió de 1024 a 2048, pero el nodo que se estaba ejecutando en ese momento solo tenía una capacidad restante de 512 para esa métrica. Ahora, esa selección de ubicación de la instancia o réplica no es válida porque no hay suficiente espacio en ese nodo. Cluster Resource Manager tiene que iniciar y reducir el uso de capacidad del nodo. Reduce la carga en el nodo que está por encima de la capacidad moviendo una o más réplicas o instancias de ese nodo a otros nodos. Al mover las réplicas, Cluster Resource Manager trata de minimizar el costo de los movimientos. El costo del movimiento se explica en [este artículo](service-fabric-cluster-resource-manager-movement-cost.md) y [aquí](service-fabric-cluster-resource-manager-metrics.md) se describe más sobre reglas y estrategias de reequilibrio de Cluster Resource Manager.

## <a name="cluster-capacity"></a>Capacidad del clúster
Entonces, ¿cómo Cluster Resource Manager de Service Fabric evita que el clúster general se llene demasiado? Con la carga dinámica no hay mucho que pueda hacer. ya que los servicios pueden tener un pico de carga con independencia de las acciones que realice Resource Manager. Como resultado, un clúster con espacio de sobra hoy puede quedarse corto cuando se haga famoso mañana, pero existen algunos controles preparados para evitar problemas. Lo primero que se puede hacer es evitar la creación de nuevas cargas de trabajo que harían que el clúster se llenara.

Supongamos que crea un servicio sin estado y tiene cierta carga asociado a él, y que Supongamos que el servicio se ocupa de la métrica "DiskSpaceInMb" y que, de forma predeterminada, va a consumir 5 unidades de "DiskSpaceInMb" para cada instancia del servicio. Quiere crear 3 instancias del servicio. Estupendo. Eso significa que necesitamos 15 unidades de "DiskSpaceInMb" en el clúster para poder crear estas instancias de servicio. Cluster Resource Manager calcula continuamente la capacidad y el consumo de cada métrica, por lo que puede determinar la capacidad restante del clúster. Si no hay espacio suficiente, Cluster Resource Manager rechaza la llamada para crear el servicio.

Dado que el requisito es solo que haya 15 unidades disponibles, este espacio se podría asignar de muchas maneras distintas; por ejemplo, podría ser una unidad de capacidad restante en 15 nodos diferentes o las tres unidades de capacidad que quedan en 5 nodos diferentes. Si Cluster Resource Manager puede reorganizarlo todo para que haya cinco unidades disponibles en tres nodos, podrá colocar el servicio. La reorganización del clúster es normalmente posible a no ser que el clúster esté casi lleno o los servicios existentes no puedan consolidarse por alguna razón.

## <a name="buffered-capacity"></a>Capacidad de búfer
Capacidad de búfer es otra característica de Cluster Resource Manager. Permite una reserva de alguna parte de la capacidad total del nodo. Este búfer de capacidad solo se utiliza para colocar los servicios durante las actualizaciones y los errores de nodo. Capacidad de búfer se especifica globalmente por métrica para todos los nodos. El valor que elija para la capacidad reservada es una función del número de dominios de actualización y de error que hay en el clúster. Más dominios de error y de actualización significa que puede elegir un número menor para la capacidad de búfer. Si tiene varios dominios, puede esperar que cantidades más reducidas de su clúster no estarán disponibles durante las actualizaciones y los errores. La especificación de la capacidad de búfer solo tiene sentido si también especifica la capacidad de nodo de una métrica.

Este es un ejemplo de cómo especificar la capacidad de búfer:

ClusterManifest.xml

```xml
        <Section Name="NodeBufferPercentage">
            <Parameter Name="SomeMetric" Value="0.15" />
            <Parameter Name="SomeOtherMetric" Value="0.20" />
        </Section>
```

a través de ClusterConfig.json para las implementaciones independientes o Template.json para los clústeres hospedados en Azure:

```json
"fabricSettings": [
  {
    "name": "NodeBufferPercentage",
    "parameters": [
      {
          "name": "SomeMetric",
          "value": "0.15"
      },
      {
          "name": "SomeOtherMetric",
          "value": "0.20"
      }
    ]
  }
]
```

La creación de nuevos servicios generará un error cuando el clúster carece de capacidad de búfer en una métrica, Evitar la creación de nuevos servicios para conservar el búfer garantiza que las actualizaciones y los errores no provocan que los nodos sobrepasen la capacidad. La capacidad de búfer es opcional, pero se recomienda en los clústeres que definen una capacidad de una métrica.

Cluster Resource Manager muestra esta información de carga. Para cada métrica, esta información incluye: 
  - la configuración de capacidad de búfer
  - la capacidad total
  - el consumo actual
  - si cada métrica se considera equilibrada o no
  - estadísticas acerca de la desviación estándar
  - los nodos que tienen más y menos carga  
  
A continuación podemos ver un ejemplo del resultado:

```posh
PS C:\Users\user> Get-ServiceFabricClusterLoadInformation
LastBalancingStartTimeUtc : 9/1/2016 12:54:59 AM
LastBalancingEndTimeUtc   : 9/1/2016 12:54:59 AM
LoadMetricInformation     :
                            LoadMetricName        : Metric1
                            IsBalancedBefore      : False
                            IsBalancedAfter       : False
                            DeviationBefore       : 0.192450089729875
                            DeviationAfter        : 0.192450089729875
                            BalancingThreshold    : 1
                            Action                : NoActionNeeded
                            ActivityThreshold     : 0
                            ClusterCapacity       : 189
                            ClusterLoad           : 45
                            ClusterRemainingCapacity : 144
                            NodeBufferPercentage  : 10
                            ClusterBufferedCapacity : 170
                            ClusterRemainingBufferedCapacity : 125
                            ClusterCapacityViolation : False
                            MinNodeLoadValue      : 0
                            MinNodeLoadNodeId     : 3ea71e8e01f4b0999b121abcbf27d74d
                            MaxNodeLoadValue      : 15
                            MaxNodeLoadNodeId     : 2cc648b6770be1bc9824fa995d5b68b1
```

## <a name="next-steps"></a>Pasos siguientes
* Para obtener más información sobre el flujo de información y la arquitectura de Cluster Resource Manager, consulte [este artículo](service-fabric-cluster-resource-manager-architecture.md).
* Definir las métricas de desfragmentación es una manera de consolidar la carga en los nodos en lugar de distribuirla. Para saber cómo configurar la desfragmentación, consulte [este artículo](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
* Empiece desde el principio y [obtenga una introducción a Cluster Resource Manager de Service Fabric](service-fabric-cluster-resource-manager-introduction.md)
* Para más información sobre cómo Cluster Resource Manager administra y equilibra la carga en el clúster, consulte el artículo sobre el [equilibrio de carga](service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-domains.png
[Image2]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-uneven-fault-domain-layout.png
[Image3]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domains-with-placement.png
[Image4]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domain-layout-strategies.png
[Image5]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-layout-different-workloads.png
[Image6]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-placement-constraints-node-properties.png
[Image7]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-nodes-and-capacity.png
