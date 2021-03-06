---
title: "Solución de problemas de puerta de enlace y conexiones de Azure Virtual Network con la CLI de Azure 1.0 | Microsoft Docs"
description: "En esta página se explica cómo usar Azure Network Watcher para solucionar problemas con la CLI de Azure 1.0."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 2838bc61-b182-4da8-8533-27db8fdbd177
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: 2acbc47970acf0eb2aa1aea8535d7157bc73cbb6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-10"></a>Solución de problemas de puerta de enlace y conexiones de Virtual Network mediante la CLI de Azure 1.0 de Azure Network Watcher

> [!div class="op_single_selector"]
> - [Portal](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [API DE REST](network-watcher-troubleshoot-manage-rest.md)

Network Watcher proporciona numerosas funcionalidades con relación a los recursos de red de Azure. Una de estas funcionalidades es la solución de problemas de recursos. Se puede llamar a la solución de problemas de recursos mediante el portal, PowerShell, la CLI o la API de REST. Cuando se llama a Network Watcher, este inspecciona el estado de una puerta de enlace de Virtual Network o de una conexión y devuelve sus conclusiones.

En este artículo, se utiliza la multiplataforma Azure CLI 1.0, que está disponible para Windows, Mac y Linux. 

## <a name="before-you-begin"></a>Antes de empezar

En este escenario, se da por hecho que ya ha seguido los pasos descritos en [Create an Azure Network Watcher instance](network-watcher-create.md) (Creación de una instancia de Azure Network Watcher) para crear una instancia de Network Watcher.

Para obtener una lista de los tipos de puerta de enlace compatibles, vea el artículo sobre los [tipos de puerta de enlace compatibles](/network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Información general

La solución de problemas de recursos permite solucionar los problemas que surgen con las puertas de enlace y las conexiones de Virtual Network. Cuando se envía una solicitud para solucionar problemas de recursos, se consultan y se inspeccionan los registros. Una vez finalizada la inspección, se devuelven los resultados. Las solicitudes para solucionar problemas de recursos son de larga ejecución, y podrían tardar varios minutos en devolver un resultado. Los registros de solución de problemas se almacenan en un contenedor en una cuenta de almacenamiento especificada.

## <a name="retrieve-a-virtual-network-gateway-connection"></a>Recuperación de una conexión de puerta de enlace de Virtual Network

En este ejemplo, la solución de problemas de recursos se va a ejecutar en una conexión. También puede pasarla una puerta de enlace de Virtual Network. El cmdlet siguiente enumera las conexiones de VPN en un grupo de recursos.

```azurecli
azure network vpn-connection list -g resourceGroupName
```

También puede ejecutar el comando para ver las conexiones de una suscripción.

```azurecli
azure network vpn-connection list -s subscription
```

Cuando tenga el nombre de la conexión, puede ejecutar este comando para obtener su identificador de recurso:

```azurecli
azure network vpn-connection show -g resourceGroupName -n connectionName
```

## <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

La solución de problemas de recursos devuelve datos sobre el estado de mantenimiento del recurso; también guarda registros en una cuenta de almacenamiento para su revisión. En este paso, se crea una cuenta de almacenamiento; si ya existe una cuenta de almacenamiento, puede usarla.

1. Creación de la cuenta de almacenamiento

    ```azurecli
    azure storage account create -n storageAccountName -l location -g resourceGroupName
    ```

1. Obtenga las claves de cuenta de almacenamiento.

    ```azurecli
    azure storage account keys list storageAccountName -g resourcegroupName
    ```

1. Cree el contenedor.

    ```azurecli
    azure storage container create --account-name storageAccountName -g resourcegroupName --account-key {storageAccountKey} --container logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a>Ejecución de la solución de problemas de recursos Network Watcher

Los problemas de recursos se solucionan con el cmdlet `network watcher troubleshoot`. Pasamos al cmdlet el grupo de recursos, el nombre de la instancia de Network Watcher, el identificador de la conexión, el identificador de la cuenta de almacenamiento, y la ruta de acceso al blob donde se van a almacenar los resultados de la solución de problemas.

```azurecli
azure network watcher troubleshoot -g resourceGroupName -n networkWatcherName -t connectionId -i storageId -p storagePath
```

Después de ejecutar el cmdlet, Network Watcher revisa los recursos para comprobar el estado. Devuelve los resultados al shell y almacena los registros de los resultados en la cuenta de almacenamiento especificada.

## <a name="understanding-the-results"></a>Descripción de los resultados

El texto de la acción ofrece instrucciones generales sobre cómo resolver el problema. Si se puede realizar una acción para el problema, se proporciona un vínculo con instrucciones adicionales. Si no hay instrucciones adicionales, la respuesta proporciona la dirección URL para abrir un caso de soporte técnico.  Para más información acerca de las propiedades de la respuesta y lo que incluye, consulte la [introducción a la solución de problemas en Network Watcher](network-watcher-troubleshoot-overview.md)

Para más instrucciones acerca de cómo descargar archivos desde cuentas de Azure Storage, consulte [Introducción a Azure Blob Storage mediante .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Otra herramienta que se puede utilizar es el Explorador de Storage. Encontrará más información acerca del Explorador de Storage en el siguiente vínculo: [Explorador de Storage](http://storageexplorer.com/)

## <a name="next-steps"></a>Pasos siguientes

Si se cambió la configuración y la conectividad de VPN se ha detenido, consulte [Administración de grupos de seguridad de red](../virtual-network/virtual-network-manage-nsg-arm-portal.md) para realizar un seguimiento de los grupos de seguridad de red y las reglas de seguridad que pueden estar afectados.
