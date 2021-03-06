---
title: "Instalación de MongoDB en una máquina virtual Linux mediante la CLI de Azure 1.0 | Microsoft Docs"
description: "Aprenda a instalar y configurar MongoDB en una máquina virtual Linux en Azure con el modelo de implementación de Resource Manager."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: c97ade0a3d95824f723aad55776de861fe49441f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-install-and-configure-mongodb-on-a-linux-vm-using-the-azure-cli-10"></a>Procedimiento de instalación y configuración de MongoDB en una máquina virtual Linux con la CLI de Azure 1.0
[MongoDB](http://www.mongodb.org) es una conocida base de datos NoSQL de código abierto y alto rendimiento. En este artículo, se muestra cómo instalar y configurar MongoDB en una máquina virtual Linux en Azure con el modelo de implementación de Resource Manager. Se muestran algunos ejemplos detallados de:

* [Instalación y configuración manuales de una instancia básica de MongoDB](#manually-install-and-configure-mongodb-on-a-vm)
* [Creación de una instancia básica de MongoDB mediante una plantilla de Resource Manager](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Creación de un clúster complejo con particiones de MongoDB con conjuntos de réplicas mediante una plantilla de Resource Manager](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="cli-versions-to-complete-the-task"></a>Versiones de la CLI para completar la tarea
Puede completar la tarea mediante una de las siguientes versiones de la CLI:

- CLI de Azure 1.0: la CLI para los modelos de implementación clásico y de Resource Manager (este artículo)
- [CLI de Azure 2.0](create-cli-complete-nodejs.md): la CLI de última generación para el modelo de implementación de administración de recursos


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Instalación y configuración manuales de MongoDB en una máquina virtual
MongoDB [proporciona instrucciones de instalación](https://docs.mongodb.com/manual/administration/install-on-linux/) para distribuciones de Linux, incluidos Red Hat/CentOS, SUSE, Ubuntu y Debian. En el ejemplo siguiente se crea una máquina virtual *CentOS* con una clave SSH almacenada en *~/.ssh/id_rsa.pub*. Responda a los mensajes que solicitan el nombre de la cuenta de almacenamiento, el nombre DNS y las credenciales de administrador:

```azurecli
azure vm quick-create \
    --image-urn CentOS \
    --ssh-publickey-file ~/.ssh/id_rsa.pub 
```

Inicie sesión en la máquina virtual con la dirección IP pública que se muestra al final del anterior paso de creación de la máquina virtual:

```bash
ssh azureuser@40.78.23.145
```

Para agregar los orígenes de instalación para MongoDB, cree un archivo de repositorio **yum** de la manera siguiente:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

Abra el archivo de repositorio de MongoDB para editarlo. Agregue las siguientes líneas:

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

Instale MongoDB mediante **yum** como se indica a continuación:

```bash
sudo yum install -y mongodb-org
```

De forma predeterminada, SELinux está aplicado en las imágenes de CentOS, lo que impide el acceso de MongoDB. Instale herramientas de administración de directivas y configure SELinux para permitir que MongoDB funcione en su puerto TCP predeterminado, 27017, como sigue. 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Inicie el servicio de MongoDB como se muestra a continuación:

```bash
sudo service mongod start
```

Para comprobar la instalación de MongoDB, conéctese por medio del cliente `mongo` local:

```bash
mongo
```

Ahora pruebe la instancia de MongoDB agregando algunos datos y después buscando:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Si lo desea, configure MongoDB para que se inicie automáticamente durante un reinicio del sistema:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Creación de una instancia básica de MongoDB en CentOS mediante una plantilla
Puede crear una instancia básica de MongoDB una sola máquina virtual CentOS con la siguiente plantilla de inicio rápido de Azure en GitHub. Esta plantilla usa la extensión de script personalizado para Linux para agregar un repositorio `yum` a la máquina Virtual CentOS recién creada y después instalar MongoDB.

* [Instancia básica de MongoDB en CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos): https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

En el ejemplo siguiente, se crea un grupo de recursos denominado `myResourceGroup` en la región `eastus`. Escriba sus propios valores, como se indica a continuación:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [!NOTE]
> La CLI de Azure regresa a un símbolo del sistema a los pocos segundos de crearse la implementación, pero la instalación y la configuración tardan unos minutos en completarse. Para comprobar el estado de la implementación con `azure group deployment show myResourceGroup`, escriba el nombre de su grupo de recursos según corresponda. Espere a que **ProvisioningState** muestre un estado *correcto* antes de intentar conectarse mediante SSH a la máquina virtual.

Una vez finalizada la implementación, conéctese mediante SSH a la máquina virtual. Obtenga la dirección IP de la máquina virtual con el comando `azure vm show`, como en el ejemplo siguiente:

```azurecli
azure vm show --resource-group myResourceGroup --name myLinuxVM
```

Casi al final de la salida, se muestra la dirección IP pública. Conéctese mediante SSH a la máquina virtual con la dirección IP de su máquina virtual:

```bash
ssh azureuser@138.91.149.74
```

Para comprobar la instalación de MongoDB, conéctese por medio del cliente `mongo` local como sigue:

```bash
mongo
```

Ahora pruebe la instancia agregando algunos datos y después buscando:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Creación de un clúster complejo con particiones de MongoDB en CentOS mediante una plantilla
Puede crear un clúster complejo con particiones de MongoDB con la siguiente plantilla de inicio rápido de Azure en GitHub. Esta plantilla sigue los [procedimientos recomendados para clústeres con particiones de MongoDB](https://docs.mongodb.com/manual/core/sharded-cluster-components/) para proporcionar alta disponibilidad y redundancia. La plantilla crea dos particiones, con tres nodos en cada conjunto de réplicas. También se crea un conjunto de réplicas de servidor de configuración con tres nodos, más dos servidores de ruta **mongos** para proporcionar coherencia a las aplicaciones entre las particiones.

* [Clúster con particiones de MongoDB en CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos): https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [!WARNING]
> Para implementar este clúster complejo con particiones de MongoDB, se requieren más de 20 núcleos, lo que suele ser el número predeterminado por región para una suscripción. Abra una solicitud de soporte técnico de Azure para aumentar el número de núcleos.

En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la región *eastus*. Escriba sus propios valores, como se indica a continuación:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [!NOTE]
> La CLI de Azure regresa a un símbolo del sistema a los pocos segundos de crearse la implementación, pero la instalación y la configuración pueden tardar más de una hora en completarse. Para comprobar el estado de la implementación con `azure group deployment show myResourceGroup`, ajuste el nombre de su grupo de recursos según corresponda. Espere a que **ProvisioningState** muestre un estado *correcto* antes de conectarse a las máquinas virtuales.


## <a name="next-steps"></a>Pasos siguientes
En estos ejemplos, puede conectarse a la instancia de MongoDB localmente desde la máquina virtual. Si desea conectarse a la instancia de MongoDB desde otra máquina virtual o una red, asegúrese de que [se creen las reglas de grupo de seguridad de red](nsg-quickstart.md) correspondientes.

Para más información sobre la creación de plantillas, consulte [Información general de Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

Las plantillas de Azure Resource Manager usan la extensión de script personalizado para descargar y ejecutar scripts en las máquinas virtuales. Para más información, consulte [Uso de la extensión de script personalizado de Azure con máquinas virtuales Linux](extensions-customscript.md).

