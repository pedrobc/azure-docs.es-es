---
title: "Instalación de PowerShell para Azure Stack | Microsoft Docs"
description: Aprenda a instalar PowerShell para Azure Stack.
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: sngun
ms.openlocfilehash: 3014b8a424e554f3c46f1c7e76eab586d089e059
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2017
---
# <a name="install-powershell-for-azure-stack"></a>Instalación de PowerShell para Azure Stack  

*Se aplica a: Sistemas integrados de Azure Stack y Azure Stack Development Kit*

Los módulos de Azure PowerShell compatibles con Azure Stack se requieren para trabajar con Azure Stack. En esta guía, se le explican los pasos necesarios para instalar PowerShell para Azure Stack. Puede seguir los pasos descritos en este artículo ya sea desde Azure Stack Development Kit o desde un cliente externo basado en Windows, si se conecta a través de VPN.

Este artículo contiene instrucciones detalladas para instalar PowerShell para Azure Stack. Sin embargo, si desea instalar y configurar PowerShell rápidamente, puede usar el script que se proporciona en el tema [Póngase a trabajar con PowerShell](azure-stack-powershell-configure-quickstart.md). 

> [!NOTE]
> En los siguientes pasos se necesita PowerShell 5.0. Para comprobar su versión, ejecute $PSVersionTable.PSVersion y compare la versión "Major" (principal).

Los comandos de PowerShell para Azure Stack se instalan a través de la Galería de PowerShell. Para registrar el repositorio de PSGallery, abra una sesión de PowerShell con privilegios elevados desde el kit de desarrollo o desde un cliente externo basado en Windows, si está conectado a través de VPN, y ejecute el siguiente comando:

```powershell
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted
```

## <a name="uninstall-existing-versions-of-powershell"></a>Desinstalación de las versiones existentes de PowerShell

Antes de instalar la versión requerida, asegúrese de desinstalar los módulos de Azure PowerShell existentes. Puede desinstalarlos mediante uno de los dos métodos siguientes:

* Para desinstalar los módulos de PowerShell existentes, inicie sesión en el kit de desarrollo o en un cliente externo basado en Windows, si va a establecer una conexión VPN. Cierre todas las sesiones activas de PowerShell y ejecute el siguiente comando: 

   ```powershell
   Get-Module -ListAvailable | where-Object {$_.Name -like “Azure*”} | Uninstall-Module
   ```

* Inicie sesión en el kit de desarrollo o en un cliente externo basado en Windows, si va a establecer una conexión VPN. Elimine todas las carpetas que empiecen con "Azure" de las carpetas `C:\Program Files (x86)\WindowsPowerShell\Modules` y `C:\Users\AzureStackAdmin\Documents\WindowsPowerShell\Modules`. Al eliminar estas carpetas, se quitan los módulos de PowerShell existentes de los ámbitos de usuario "AzureStackAdmin" y "global". 

En las secciones siguientes se describen los pasos necesarios para instalar PowerShell para Azure Stack. PowerShell puede instalarse en Azure Stack que opere en un escenario conectado, parcialmente conectado o sin conexión. 

## <a name="install-powershell-in-a-connected-scenario"></a>Instalación de PowerShell en un escenario conectado 

Los módulos AzureRM compatibles con Azure Stack se instalan a través de perfiles de la versión de API. Azure Stack requiere el perfil de la versión de API **2017-03-09-profile**, que está disponible al instalar el módulo AzureRM.Bootstrapper. Para obtener información acerca de los perfiles de la versión de API y los cmdlets proporcionados por ellos, consulte el artículo sobre [administración de perfiles de la versión de API](azure-stack-version-profiles.md). Además de los módulos AzureRM, también debe instalar los módulos de PowerShell específicos de Azure Stack. Ejecute el siguiente script de PowerShell para instalar estos módulos en la estación de trabajo de desarrollo:

> [!IMPORTANT]
> La versión del módulo de PowerShell AzureRM 1.2.11 incluye una lista de los últimos cambios. Para actualizar la versión 1.2.10, consulte la guía de migración en [https://aka.ms/azspowershellmigration](https://aka.ms/azspowershellmigration).

  ```powershell
  # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet 
  Install-Module `
    -Name AzureRm.BootStrapper

  # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
  Use-AzureRmProfile `
    -Profile 2017-03-09-profile -Force

  Install-Module `
    -Name AzureStack `
    -RequiredVersion 1.2.11
  ```

Para confirmar la instalación, ejecute el siguiente comando:

  ```powershell
  Get-Module `
    -ListAvailable | where-Object {$_.Name -like “Azure*”}
  ```
  Si la instalación es correcta, los módulos AzureRM y AzureStack se muestran en la salida.

## <a name="install-powershell-in-a-disconnected-or-in-a-partially-connected-scenario"></a>Instalación de PowerShell en un escenario sin conexión o parcialmente conectado

En un escenario sin conexión, primero debe descargar los módulos de PowerShell en un equipo que tenga conexión a Internet y, a continuación, transferirlos a Azure Stack Development Kit para la instalación.

> [!IMPORTANT]
> La versión del módulo de PowerShell AzureRM 1.2.11 incluye una lista de los últimos cambios. Para actualizar la versión 1.2.10, consulte la guía de migración en [https://aka.ms/azspowershellmigration](https://aka.ms/azspowershellmigration).

1. Inicie sesión en un equipo que tenga conexión a Internet y use el script siguiente para descargar los paquetes AzureRM y AzureStack en el equipo local:

   ```powershell
   $Path = "<Path that is used to save the packages>"

   Save-Package `
     -ProviderName NuGet `
     -Source https://www.powershellgallery.com/api/v2 `
     -Name AzureRM `
     -Path $Path `
     -Force `
     -RequiredVersion 1.2.11

   Save-Package `
     -ProviderName NuGet `
     -Source https://www.powershellgallery.com/api/v2 `
     -Name AzureStack `
     -Path $Path `
     -Force `
     -RequiredVersion 1.2.11 
   ```

2. Copie los paquetes descargados a través en un dispositivo USB.

3. Inicie sesión en el kit de desarrollo y copie los paquetes desde el dispositivo USB a una ubicación de este. 

4. Ahora debe registrar esta ubicación como el repositorio predeterminado e instalar los módulos AzureRM y AzureStack desde este repositorio:

   ```powershell
   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository `
     -Name $RepoName `
     -SourceLocation $SourceLocation `
     -InstallationPolicy Trusted

   ```powershell
   Install-Module AzureRM `
     -Repository $RepoName

   Install-Module AzureStack `
     -Repository $RepoName 
   ```

## <a name="next-steps"></a>Pasos siguientes

* [Descarga de herramientas de Azure Stack desde GitHub](azure-stack-powershell-download.md)
* [Configuración del entorno de PowerShell del usuario de Azure Stack](user/azure-stack-powershell-configure-user.md)  
* [Configuración del entorno de PowerShell del operador de Azure Stack](azure-stack-powershell-configure-admin.md) 
* [Administración de perfiles de la versión de API en Azure Stack](azure-stack-version-profiles.md)  
