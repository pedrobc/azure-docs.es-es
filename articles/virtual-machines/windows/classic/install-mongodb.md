---
title: "Instalación de MongoDB en una máquina virtual Windows en Azure | Microsoft Docs"
description: "Obtenga información acerca de cómo instalar MongoDB en una máquina virtual de Azure creada con el modelo clásico de implementación con Windows Server."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4095df41-bb69-4bbe-9c1c-70923b0d84ba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: iainfou
ms.openlocfilehash: 6b5af18d02fd508a21cdc21b38b1c16e79f07ecb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="install-mongodb-on-a-windows-vm-in-azure"></a>Instalación de MongoDB en una máquina virtual de Windows en Azure
> [!IMPORTANT]
> Azure tiene dos modelos de implementación diferentes para crear recursos y trabajar con ellos: [Resource Manager y el clásico](../../../resource-manager-deployment-model.md).  En este artículo se trata el modelo de implementación clásico. Microsoft recomienda que las implementaciones más recientes usen el modelo del Administrador de recursos. Para instalar y configurar MongoDB mediante el modelo de implementación de Resource Manager, consulte [este artículo](../install-mongodb.md).

[MongoDB][MongoDB] es una conocida base de datos NoSQL de código abierto y alto rendimiento. Este artículo le guía en la creación de una máquina virtual (VM) de Windows Server con el [Azure Portal][AzurePortal]. A continuación, creará y conectará un disco de datos a la máquina virtual antes de instalar y configurar MongoDB. Si tiene una máquina virtual existente en Azure que le gustaría usar, puede pasar directamente a la [instalación y configuración de MongoDB](#install-and-run-mongodb-on-the-virtual-machine).

## <a name="create-a-virtual-machine-running-windows-server"></a>Creación de una máquina virtual que ejecuta Windows Server
Para crear una máquina virtual siga estas instrucciones.

[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

> [!NOTE]
> Al crear la máquina virtual, puede agregar un punto de conexión para MongoDB y configurarlo como se indica a continuación: asígnele el nombre **Mongo**, use el protocolo **TCP** y establezca los puertos públicos y privados en **27017**.
>
>

## <a name="attach-a-data-disk"></a>Acoplamiento de un disco de datos
Para proporcionar almacenamiento para la máquina virtual, acople un disco de datos e inicialícelo para que Windows pueda usarlo. Si ya tiene un disco de datos, puede conectar ese disco existente o conectar un disco vacío.

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-windows-linux.md)]

Para obtener instrucciones sobre la inicialización del disco, consulte "Inicialización de un nuevo disco de datos en Windows Server" en [Acoplamiento de un disco de datos a una máquina virtual de Windows](attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Instalación y ejecución de MongoDB en la máquina virtual
[!INCLUDE [install-and-run-mongo-on-win2k8-vm](../../../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Resumen
En este tutorial se ha descrito cómo crear una máquina virtual con Windows Server, conectarse a ella de forma remota y acoplar un disco de datos.  También se ha descrito cómo instalar y configurar MongoDB en la máquina virtual basada en Windows. Ahora puede tener acceso MongoDB en la máquina virtual basada en Windows, siguiendo los temas avanzados en la [documentación de MongoDB][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: https://portal.azure.com/

