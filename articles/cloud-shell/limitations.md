---
title: "Limitaciones de Azure Cloud Shell (versión preliminar) | Microsoft Docs"
description: "Introducción a las limitaciones de Azure Cloud Shell"
services: azure
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: juluk
ms.openlocfilehash: 92c8e4c205043f6c5c2925d9197270fb720969a3
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="limitations-of-azure-cloud-shell"></a>Limitaciones de Azure Cloud Shell

Azure Cloud Shell tiene las limitaciones conocidas siguientes:

## <a name="general-limitations"></a>Limitaciones generales

### <a name="system-state-and-persistence"></a>Persistencia y estado del sistema

La máquina que proporciona la sesión de Cloud Shell es temporal y se recicla después de que la sesión esté inactiva durante 20 minutos. Cloud Shell requiere montar un recurso compartido de archivos. Como resultado, la suscripción debe poder configurar los recursos de almacenamiento para tener acceso a Cloud Shell. Otras consideraciones:

* Con el almacenamiento montado, solo se conservan las modificaciones dentro del directorio `clouddrive`. En Bash, también se conserva el directorio `$Home`.
* Solo se pueden montar recursos compartidos de archivos desde la [región asignada](persisting-shell-storage.md#mount-a-new-clouddrive).
  * En Bash, ejecute `env` para buscar la región establecida como `ACC_LOCATION`.
* Azure Files solo admite cuentas de almacenamiento con redundancia local o de almacenamiento con redundancia geográfica.

### <a name="browser-support"></a>Compatibilidad con exploradores

Cloud Shell es compatible con las versiones más recientes de Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox y Safari de Apple. Safari en modo privado no es compatible.

### <a name="copy-and-paste"></a>Copiar y pegar

[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Solo puede haber un shell activo para un usuario determinado

Los usuarios solo pueden iniciar un tipo de shell a la vez, ya sea **Bash** o **PowerShell**. Pero puede haber varias instancias de Bash o PowerShell en ejecución al mismo tiempo. Los cambios entre Bash y PowerShell reinician Cloud Shell, lo que finaliza las sesiones existentes.

### <a name="usage-limits"></a>Límites de uso

Cloud Shell está pensado para casos de uso interactivos. Por tanto, todas las sesiones que no sean de este tipo y que se prolonguen durante mucho tiempo se finalizarán sin previo aviso.

## <a name="bash-limitations"></a>Limitaciones de Bash

### <a name="user-permissions"></a>Permisos de usuario

Los permisos se establecen como usuarios normales sin acceso a sudo. No se conserva cualquier instalación fuera del directorio `$Home`.
Aunque algunos comandos del directorio `clouddrive`, como `git clone`, no tienen los permisos adecuados, el directorio `$Home` sí los tiene.

### <a name="editing-bashrc"></a>Editar .bashrc

Tenga cuidado al editar .bashrc, ya que puede producir errores inesperados en Cloud Shell.

### <a name="bashhistory"></a>.bash_history

El historial de comandos de Bash puede ser incoherente debido a una interrupción de la sesión de Cloud Shell o a sesiones simultáneas.

## <a name="powershell-limitations"></a>Limitaciones de PowerShell

### <a name="slow-startup-time"></a>Tiempo de inicio lento

PowerShell en Azure Cloud Shell podría tardar hasta 60 segundos en inicializarse durante la versión preliminar.

### <a name="no-home-directory-persistence"></a>El directorio $Home no persiste

Los datos que cualquier aplicación (como git, vim y otras) escriben en `$Home` no se conservan entre las sesiones de PowerShell. [Consulte aquí](troubleshooting.md#powershell-resolutions) para ver una solución alternativa.

## <a name="next-steps"></a>Pasos siguientes

[Solución de problemas de Cloud Shell](troubleshooting.md) <br>
[Guía de inicio rápido de Bash](quickstart.md) <br>
[Guía de inicio rápido de PowerShell](quickstart-powershell.md)
