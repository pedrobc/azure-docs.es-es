---
title: 'CLI de Azure Service Fabric: sfctl cluster | Microsoft Docs'
description: Describe los comandos de sfctl cluster de la CLI Service Fabric.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 09/22/2017
ms.author: ryanwi
ms.openlocfilehash: dc2ed59d6adaca97b23dddcb7ec968d90171b483
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="sfctl-cluster"></a>sfctl cluster
Seleccione, administre y opere clústeres de Service Fabric.

## <a name="commands"></a>Comandos:

|Comando|Descripción|
| --- | --- |
|    code-versions| Obtiene una lista de versiones de código de tejido que se aprovisionan en un clúster de Service Fabric.|
|    config-versions | Obtiene una lista de versiones de configuración de tejido que se aprovisionan en un clúster de Service Fabric.|
|    health       | Obtiene el estado de un clúster de Service Fabric.|
|    manifest     | Obtiene el manifiesto de clúster de Service Fabric.|
|    operation-cancel| Cancela una operación de error provocada por el usuario.|
|    operationgit | Obtiene una lista de operaciones de error provocada por el usuario, filtrada por la entrada proporcionada.|
|    provision     | Aprovisiona el código o los paquetes de configuración de un clúster de Service Fabric.|
|    recover-system  | Indica al clúster de Service Fabric que debería intentar recuperar los servicios del sistema que actualmente están atascados en pérdida de cuórum.|
|report-health   | Envía un informe de estado sobre el clúster de Service Fabric.|
|    select       | Se conecta a un punto de conexión del clúster de Service Fabric.|
| unprovision     | Deshace el aprovisionamiento del código o los paquetes de configuración de un clúster de Service Fabric.|
|    upgrade         | Comienza a actualizar la versión del código o de la configuración de un clúster de Service Fabric.|
|    upgrade-resume  | Hace que la actualización del clúster avance al siguiente dominio de actualización.|
|    upgrade-rollback| Revierte la actualización de un clúster de Service Fabric.|
|    upgrade-status  | Obtiene el progreso de la actualización del clúster actual.|
|upgrade-update  | Actualiza los parámetros de actualización de una actualización de clúster de Service Fabric.|


## <a name="sfctl-cluster-health"></a>sfctl cluster health
Obtiene el estado de un clúster de Service Fabric.

Obtiene el estado de un clúster de Service Fabric. Use EventsHealthStateFilter para filtrar la colección de eventos de estado notificados en el clúster en función del estado de mantenimiento. Asimismo, utilice NodesHealthStateFilter y ApplicationsHealthStateFilter para filtrar la colección de nodos y aplicaciones devueltas, según su estado de mantenimiento agregado. 

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --applications-health-state-filter| Permite filtrar el estado de mantenimiento de la aplicación.
                                                    objects returned in the result of cluster health
                                                    query based on their health state. The possible
                                                    values for this parameter include integer value
                                                    obtained from members or bitwise operations on
                                                    members of HealthStateFilter enumeration. Only
                                                    applications that match the filter are returned.
                                                    All applications are used to evaluate the
                                                    aggregated health state. If not specified, all
                                                    entries are returned. The state values are flag
                                                    based enumeration, so the value could be a
                                                    combination of these values obtained using
                                                    bitwise 'OR' operator. For example, if the
                                                    provided value is 6 then health state of
                                                    applications with HealthState value of OK (2)
                                                    and Warning (4) are returned. - Default -
                                                    Default value. Matches any HealthState. The
                                                    value is zero. - None - Filter that doesn't
                                                    match any HealthState value. Used in order to
                                                    return no results on a given collection of
                                                    states. The value is 1. - Ok - Filter that
                                                    matches input with HealthState value Ok. The
                                                    value is 2. - Warning - Filter that matches
                                                    input with HealthState value Warning. The value
                                                    is 4. - Error - Filter that matches input with
                                                    HealthState value Error. The value is 8. - All -
                                                    Filter that matches input with any HealthState value. The value is 65535.|
| --events-health-state-filter   | Permite filtrar la colección de objetos HealthEvent devueltos según el estado de mantenimiento. Los valores posibles para este parámetro incluyen el valor entero de uno de los siguientes estados de mantenimiento. Se devuelven únicamente los eventos que coinciden con el filtro. Todos los eventos se utilizan para evaluar el estado de mantenimiento agregado. Si no se especifica, se devuelven todas las entradas. Los valores de estado se marcan según la enumeración, por lo que el valor puede ser una combinación de estos valores obtenidos mediante el operador bit a bit 'OR'. Por ejemplo, si el valor proporcionado es 6, se devuelven todos los eventos con el valor HealthState de Ok (2) y Warning (4). - Default: valor predeterminado. Coincide con cualquier HealthState. El valor predeterminado es cero. - None: filtro que no coincide con ningún valor de HealthState. Se utiliza para no devolver ningún resultado en una determinada colección de estados. El valor es 1. - Ok: filtro que asocia la entrada con el valor de HealthState de Ok. El valor es 2. - Warning: filtro que asocia la entrada con el valor de HealthState de Warning.
El valor es 4. - Error: filtro que asocia la entrada con el valor de HealthState de Error. El valor es 8. - All - Filtro que asocia la entrada con cualquier valor de HealthState. El valor es 65535. .| |--exclude-health-statistics | Indica si las estadísticas de mantenimiento se deben devolver como parte del resultado de la consulta. El valor predeterminado es false. Las estadísticas muestran el número de entidades secundarias en estado Ok, Warning y Error. | |   --include-system-application-health-statistics| Indica si las estadísticas de estado deben incluir las estadísticas de estado de la aplicación fabric:/System. El valor predeterminado es false. Si IncludeSystemApplicationHealthStatistics se establece en true, las estadísticas de estado incluyen las entidades que pertenecen a la aplicación fabric:/System. En caso contrario, el resultado de la consulta incluye las estadísticas de estado solo para las aplicaciones de usuario. Las estadísticas de estado deben estar incluidas en el resultado de la consulta para que se aplique este parámetro. | | --nodes-health-state-filter    | Permite filtrar los objetos de estado de mantenimiento del nodo devueltos en el resultado de consulta de estado del clúster en función de su estado de mantenimiento. Los valores posibles para este parámetro incluyen el valor entero de uno de los siguientes estados de mantenimiento. Se devuelven únicamente los nodos que coinciden con el filtro. Todos los nodos se utilizan para evaluar el estado de mantenimiento agregado. Si no se especifica, se devuelven todas las entradas. Los valores de estado se marcan según la enumeración, por lo que el valor puede ser una combinación de estos valores obtenidos mediante el operador bit a bit 'OR'. Por ejemplo, si el valor proporcionado es “6”, se devuelve el estado de mantenimiento de los nodos con el valor HealthState de Ok (2) y Warning (4). - Default: valor predeterminado. Coincide con cualquier HealthState. El valor predeterminado es cero. - None: filtro que no coincide con ningún valor de HealthState. Se utiliza para no devolver ningún resultado en una determinada colección de estados. El valor es 1. - Ok: filtro que asocia la entrada con el valor de HealthState de Ok. El valor es 2. - Warning: filtro que asocia la entrada con el valor de HealthState de Warning.
El valor es 4. - Error: filtro que asocia la entrada con el valor de HealthState de Error. El valor es 8. - All: filtro que asocia la entrada con cualquier valor de HealthState. El valor es 65535. | | --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado: 60.|

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug                        | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración.|
| --help -h                      | Muestra este mensaje de ayuda y sale.|
| --output -o                    | Formato de salida.  Valores permitidos: json, jsonc, table y tsv.                    Valor predeterminado: json.|
| --query                        | Cadena de consulta de JMESPath. Consulte http://jmespath.org/ para obtener más información y ejemplos.|
| --verbose                      | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos.|

## <a name="sfctl-cluster-manifest"></a>sfctl cluster manifest
Obtiene el manifiesto de clúster de Service Fabric.

Obtiene el manifiesto de clúster de Service Fabric. El manifiesto de clúster contiene propiedades del clúster que incluyen distintos tipos de nodos en el clúster, las configuraciones de seguridad, los errores y las topologías de dominio de actualización, etc. Estas propiedades se especifican como parte del archivo ClusterConfig.JSON al implementar un clúster independiente. Sin embargo, el tejido del servicio genera internamente la mayor parte de la información en el manifiesto de clúster durante la implementación del clúster en otros escenarios de implementación (por ejemplo, cuando se usa [Azure Portal](https://portal.azure.com)). El contenido del manifiesto de clúster tiene solo fines informativos y no se espera que los usuarios tomen una dependencia en el formato del contenido del archivo ni su interpretación. 

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --timeout -t| Tiempo de espera del servidor en segundos.  Valor predeterminado: 60.|

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug  | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración.|
| --help -h| Muestra este mensaje de ayuda y sale.|
| --output -o | Formato de salida.  Valores permitidos: json, jsonc, table y tsv.  Valor predeterminado: json.|
| --query  | Cadena de consulta de JMESPath. Consulte http://jmespath.org/ para obtener más información y ejemplos.|
| --verbose| Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos.|

## <a name="sfctl-cluster-provision"></a>sfctl cluster provision
Aprovisiona el código o los paquetes de configuración de un clúster de Service Fabric.

        Validate and provision the code or configuration packages of a Service Fabric cluster.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
|--cluster-manifest-file-path| La ruta de acceso del archivo de manifiesto de clúster.|
|    --code-file-path            | La ruta del archivo del paquete de código del clúster.|
|    --timeout -t                | Tiempo de espera del servidor en segundos.  Valor predeterminado: 60.|

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración.|
| --help -h  | Muestra este mensaje de ayuda y sale.|
| --output -o| Formato de salida.  Valores permitidos: json, jsonc, table y tsv.  Valor predeterminado: json.|
| --query | Cadena de consulta de JMESPath. Consulte http://jmespath.org/ para obtener más información y ejemplos.|
| --verbose  | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos.|

## <a name="sfctl-cluster-select"></a>sfctl cluster select
Se conecta a un punto de conexión del clúster de Service Fabric.

Si se conecta al clúster segura, especifique un archivo de certificado (.crt) y de clave (.key) o un único archivo con ambos (.pem). No especifique ambos. Como alternativa, si se conecta a un clúster seguro, también especifique una ruta de acceso a un archivo de paquete de la entidad emisora de certificados o al directorio de certificados de confianza de la entidad emisora de certificados.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --endpoint [Obligatorio]| Dirección URL del punto de conexión del clúster, incluido el puerto y el prefijo HTTP o HTTPS.|
| --aad             | Utilice Azure Active Directory para la autenticación.|
| --ca              | Ruta de acceso al directorio de certificados de la entidad emisora de certificados que se tratará como válido o al archivo de paquete de entidades emisoras de certificados.|
| --cert            | Ruta de acceso a un archivo de certificados de cliente.|
| --key             | Ruta de acceso a un archivo de claves de certificados de cliente.|
| --no-verify       | Deshabilita la comprobación de certificados cuando se utiliza HTTPS. Tenga en cuenta: Esta es una opción insegura y no debe usarse para entornos de producción.|
| --pem             | Ruta de acceso a un certificado de cliente, como un archivo .pem.|

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug           | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración.|
| --help -h         | Muestra este mensaje de ayuda y sale.|
| --output -o       | Formato de salida.  Valores permitidos: json, jsonc, table y tsv.  Valor predeterminado: json.|
| --query           | Cadena de consulta de JMESPath. Consulte http://jmespath.org/ para obtener más información y ejemplos.|
| --verbose         | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos.|

## <a name="sfctl-cluster-unprovision"></a>sfctl cluster unprovision
Deshace el aprovisionamiento del código o los paquetes de configuración de un clúster de Service Fabric.

        Unprovision the code or configuration packages of a Service Fabric cluster.

### <a name="arguments"></a>Argumentos
|Argumento|Descripción|
| --- | --- |
|--code-version  | La versión del paquete de código del clúster.|
|    --config-version| La versión del manifiesto de clúster.|
|    --timeout -t    | Tiempo de espera del servidor en segundos.  Valor predeterminado: 60.|

### <a name="global-arguments"></a>Argumentos globales
|Argumento|Descripción|
| --- | --- |
|--debug         | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración.|
 |   --help -h       | Muestra este mensaje de ayuda y sale.|
 |   --output -o     | Formato de salida.  Valores permitidos: json, jsonc, table y tsv.  Valor predeterminado: json.|
 |   --query         | Cadena de consulta de JMESPath. Consulte http://jmespath.org/ para obtener más información y
                      ejemplos.|
 |   --verbose       | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos.|


## <a name="sfctl-cluster-upgrade"></a>sfctl cluster upgrade
Comienza a actualizar la versión del código o de la configuración de un clúster de Service Fabric.

        Validate the supplied upgrade parameters and start upgrading the code or configuration
        version of a Service Fabric cluster if the parameters are valid.

### <a name="arguments"></a>Argumentos
|Argumento|Descripción|
| --- | --- |
|    --app-health-map                      | Diccionario JSON codificado de pares de nombre de aplicación y
                                            porcentaje máximo de estado incorrecto antes de generar un error.|
 |   --app-type-health-map                 | Diccionario JSON codificado de pares de tipo de aplicación
                                            name and maximum percentage unhealthy before raising
                                            error.|
 |   --code-version | La versión de código del clúster. | |   --config-version | La versión de configuración del clúster. | |   --delta-health-evaluation | Permite la evaluación de mantenimiento de delta, en lugar de la evaluación de mantenimiento absoluta tras la finalización de cada dominio de actualización. | |   --delta-unhealthy-nodes | El porcentaje máximo permitido de nodos con degradación del estado permitida durante las actualizaciones de clúster.  Valor predeterminado: 10
El delta se mide entre el estado de los nodos al principio de la actualización y el estado de los nodos en el momento de la evaluación de mantenimiento. La comprobación se realiza después de cada finalización de actualización de dominio de actualización para asegurarse de que el estado global del clúster está dentro de los límites tolerados. | |   --failure-action | Los valores posibles son: 'Invalid', 'Rollback', 'Manual'.| |   --force-restart | Fuerza el reinicio. | |   --health-check-retry | Tiempo de espera de reintentos de comprobación de estado medido en milisegundos. | |   --health-check-stable | Duración de comprobación de estado estable medida en milisegundos. | |  --health-check-wait | Duración de espera de comprobación de estado medida en milisegundos. | |  --replica-set-check-timeout | Tiempo de espera de comprobación del conjunto de réplicas de actualización en segundos. | |   --rolling-upgrade-mode | Los valores posibles son: 'Invalid', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitored'.  Valor predeterminado: UnmonitoredAuto. | |  --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado: 60.| |  --unhealthy-applications | El porcentaje máximo permitido de aplicaciones en mal estado antes de informar de un error.
Por ejemplo, para permitir el 10 % de las aplicaciones en mal estado, este valor sería 10. El valor representa el porcentaje máximo tolerado de aplicaciones que pueden ser incorrectas antes de que el clúster se considere erróneo. Si se respeta el porcentaje, pero hay al menos una aplicación en mal estado, el estado se evalúa como Warning. Esto se calcula dividiendo el número de aplicaciones en mal estado sobre el número total de instancias de la aplicación en el clúster, excluidas las aplicaciones de los tipos de aplicación que se incluyen en ApplicationTypeHealthPolicyMap. El cálculo se redondea hacia arriba para tolerar un error en un número pequeño de aplicaciones. | |   --unhealthy-nodes | El porcentaje máximo permitido de nodos mal estado antes de informar de un error.
Por ejemplo, para permitir el 10 % de los nodos en mal estado, este valor sería 10. El valor representa el porcentaje máximo tolerado de nodos que pueden ser incorrectos antes de que el clúster se considere erróneo. Si se respeta el porcentaje, pero hay al menos un nodo en mal estado, el estado se evalúa como Warning. El porcentaje se calcula dividiendo el número de nodos en mal estado sobre el número total de nodos del clúster. El cálculo se redondea hacia arriba para tolerar un error en números reducidos de nodos. En los clústeres de gran tamaño, algunos nodos siempre estarán fuera de servicio o en reparación, por lo que este porcentaje debe configurarse para tolerar esto. | |   --upgrade-domain-delta-unhealthy-nodes| El porcentaje máximo permitido de degradación del estado de los nodos de dominio de actualización permitida durante las actualizaciones de clúster.
Valor predeterminado: 15.
El delta se mide entre el estado de los nodos del dominio de actualización al principio de la actualización y el estado de los nodos del dominio de actualización en el momento de la evaluación de mantenimiento. La comprobación se realiza después de cada finalización de actualización de dominio de actualización para todos los dominios de actualización completados, para asegurarse de que el estado de los dominios de actualización este dentro de los límites tolerados. | |   --upgrade-domain-timeout | Tiempo de espera del dominio de actualización medido en milisegundos. | |   --upgrade-timeout | Tiempo de espera de actualización medido en milisegundos. | |   --warning-as-error | Las advertencias se tratan con el mismo nivel de gravedad que los errores. |

### <a name="global-arguments"></a>Argumentos globales
    |Argumento|Descripción|
| --- | --- |
|--debug                               | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración.|
|    --help -h                             | Muestra este mensaje de ayuda y sale.|
|    --output -o                           | Formato de salida.  Valores permitidos: json, jsonc, table y tsv.
                                            Valor predeterminado: json.|
|    --query                               | Cadena de consulta de JMESPath. Consulte http://jmespath.org/ para obtener más
                                            información y ejemplos.|
|    --verbose                             | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración
                                            completos.|

## <a name="next-steps"></a>Pasos siguientes
- [Configuración](service-fabric-cli.md) de la CLI de Service Fabric.
- Obtenga información sobre cómo utilizar la CLI de Service Fabric con los [scripts de ejemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).