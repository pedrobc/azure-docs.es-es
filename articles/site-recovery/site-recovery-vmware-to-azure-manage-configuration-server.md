---
title: " Administración de un servidor de configuración en Azure Site Recovery | Microsoft Docs"
description: "En este artículo se describe cómo configurar y administrar un servidor de configuración."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 10/06/2017
ms.author: anoopkv
ms.openlocfilehash: e4740c96383468713976e5a98881bec13b0c1921
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="manage-a-configuration-server"></a>Administración de un servidor de configuración

El servidor de configuración actúa como coordinador entre los servicios de Site Recovery y la infraestructura local. En este artículo se describe cómo instalar, configurar y administrar el servidor de configuración.

> [!NOTE]
> La [planificación de la capacidad](site-recovery-capacity-planner.md) es un paso importante para tener la seguridad de que implementa el servidor de configuración con una configuración que satisfaga sus requisitos de carga. Lea más sobre los [requisitos de tamaño de un servidor de configuración](#sizing-requirements-for-a-configuration-server).


## <a name="prerequisites"></a>Requisitos previos
A continuación se indican los requisitos mínimos de hardware, software y red necesarios para instalar un servidor de configuración.
> [!IMPORTANT]
> Al implementar un servidor de configuración para la protección de máquinas virtuales de VMware, se recomienda implementarlo como una máquina virtual con **alta disponibilidad (HA)**.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-the-configuration-server-software"></a>Descarga del software del servidor de configuración

1. Inicie sesión en Azure Portal y busque el almacén de Recovery Services.
2. Vaya a **Site Recovery Infrastructure** (Infraestructura de Site Recovery)  > **Servidores de configuración** (en For VMware & Physical Machines [Para máquinas físicas y VMware]).

  ![Página Agregar servidores](./media/site-recovery-vmware-to-azure-manage-configuration-server/AddServers.png)
3. Haga clic en el botón **+Servidores**.
4. En la página **Agregar servidor**, haga clic en el botón Descargar para descargar la clave de Registro. Necesitará esta clave durante la instalación del servidor de configuración para registrarla en el servicio Azure Site Recovery.
5. Haga clic en el vínculo **Download the Microsoft Azure Site Recovery Unified Setup** (Descargar la instalación unificada de Microsoft Azure Site Recovery) para descargar la versión más reciente del servidor de configuración.

  ![Página de descarga](./media/site-recovery-vmware-to-azure-manage-configuration-server/downloadcs.png)

  > [!TIP]
  La versión más reciente del servidor de configuración se puede descargar directamente desde [la página del Centro de descarga de Microsoft](http://aka.ms/unifiedsetup).

## <a name="installing-and-registering-a-configuration-server-from-gui"></a>Instalación y registro de un servidor de configuración desde la interfaz gráfica de usuario
[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

## <a name="installing-and-registering-a-configuration-server-using-command-line"></a>Instalación y registro de un servidor de configuración desde la línea de comandos

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a>Ejemplo de uso
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="configuration-server-installer-command-line-arguments"></a>Argumentos de línea de comandos del instalador del servidor de configuración
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-mysql-credentials-file"></a>Creación de un archivo de credenciales de MySql
El parámetro MySQLCredsFilePath toma como entrada un archivo. Cree el archivo con el siguiente formato y páselo como parámetro de entrada MySQLCredsFilePath.
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-a-proxy-settings-configuration-file"></a>Creación de un archivo de configuración de proxy
El parámetro ProxySettingsFilePath toma un archivo como entrada. Cree el archivo con el siguiente formato y páselo como parámetro de entrada ProxySettingsFilePath.

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-configuration-server"></a>Modificación de la configuración de proxy del servidor de configuración
1. Inicie sesión en el servidor de configuración.
2. Inicie el archivo cspsconfigtool.exe mediante el acceso directo del escritorio.
3. Haga clic en la pestaña **Registro de almacén**.
4. Descargue un nuevo archivo de registro de almacén desde el portal y proporciónelo como entrada a la herramienta.

  ![register-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
5. Proporcione los detalles del nuevo servidor proxy y haga clic en el botón **Registrar**.
6. Abra una ventana de comandos de administrador de PowerShell.
7. Ejecute el siguiente comando.
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  Si tiene conectados a este servidor de configuración de servidores de procesos de escalado horizontal, deberá [corregir la configuración del proxy de todos los servidores de proceso de escalado horizontal](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) de su implementación.

## <a name="modify-user-accounts-and-passwords"></a>Modificación de las cuentas de usuario y contraseñas

El archivo CSPSConfigTool.exe se utiliza para administrar las cuentas de usuario usadas para la **detección automática de máquinas virtuales de VMware** y para llevar a cabo la instalación de inserción de Mobility Service en máquinas protegidas. 

1. Inicie sesión en el servidor de configuración.
2. Inicie CSPSConfigtool.exe haciendo clic en el acceso directo disponible en el escritorio.
3. Haga clic en la pestaña **Administrar cuentas**.
4. Seleccione la cuenta para la que debe modificarse la contraseña y haga clic en el botón **Editar**.
5. Escriba la nueva contraseña y haga clic en **Aceptar**.


## <a name="re-register-a-configuration-server-with-the-same-recovery-services-vault"></a>Volver a registrar un servidor de configuración con el mismo almacén de Recovery Services
  1. Inicie sesión en el servidor de configuración.
  2. Inicie el archivo cspsconfigtool.exe mediante el acceso directo del escritorio.
  3. Haga clic en la pestaña **Registro de almacén**.
  4. Descargue un nuevo archivo de registro del portal y proporciónelo como entrada a la herramienta.
        ![register-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
  5. Proporcione los detalles del servidor proxy y haga clic en el botón **Registrar**.  
  6. Abra una ventana de comandos de administrador de PowerShell.
  7. Ejecute el siguiente comando.

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

  >[!WARNING]
  Si tiene conectados servidores de procesos de escalado horizontal a este servidor de configuración, debe [volver a registrar todos los servidores de proceso de escalado horizontal](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) de su implementación.

## <a name="registering-a-configuration-server-with-a-different-recovery-services-vault"></a>Registro de un servidor de configuración con un almacén de Recovery Services diferente

> [!WARNING]
> El siguiente paso desasocia la configuración del almacén actual y se detiene la replicación de todas las máquinas virtuales protegidas en el servidor de configuración.

1. Inicie sesión en el servidor de configuración.
2. En el símbolo del sistema de administrador, ejecute el comando

    ```
    reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
    net stop dra
    ```
3. Inicie el archivo cspsconfigtool.exe mediante el acceso directo del escritorio.
4. Haga clic en la pestaña **Registro de almacén**.
5. Descargue un nuevo archivo de registro del portal y proporciónelo como entrada a la herramienta.

    ![register-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
6. Proporcione los detalles del servidor proxy y haga clic en el botón **Registrar**.  
7. Abra una ventana de comandos de administrador de PowerShell.
8. Ejecute el siguiente comando.
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="upgrading-a-configuration-server"></a>Actualización de un servidor de configuración

> [!WARNING]
> Las actualizaciones solo se admiten hasta la versión N-4. Por ejemplo, si la versión más reciente en el mercado es la 9.11, a continuación, puede realizar la actualización de las versiones 9.10, 9,9, 9.8 o 9.7 directamente a la 9.11. Pero si su versión es la 9.6, o cualquier anterior, tendrá que actualizarla a la 9.7, como mínimo, para poder aplicar las últimas actualizaciones al servidor de configuración. Los vínculos descarga de la versión anterior se pueden encontrar en [Azure Site Recovery service updates](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx) (Actualizaciones del servicio Azure Site Recovery).

1. Descargue al instalador de actualizaciones en el servidor de configuración.
2. Para iniciar el instalador, haga doble clic en él.
3. El instalador detectará la versión de los componentes de Site Recovery presentes en el equipo y pedirá una confirmación. 
4. Haga clic en el botón Aceptar para realizar la confirmación y continuar con la actualización.


## <a name="delete-or-unregister-a-configuration-server"></a>Eliminación o anulación del registro de un servidor de configuración

> [!WARNING]
> Antes de comenzar la retirada de su servidor de configuración, lleve a cabo estos pasos:
> 1. [Deshabilite la protección](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) en todas las máquinas virtuales de este servidor de configuración.
> 2. [Desasocie](site-recovery-setup-replication-settings-vmware.md#dissociate-a-configuration-server-from-a-replication-policy) y [elimine](site-recovery-setup-replication-settings-vmware.md#delete-a-replication-policy) todas las directivas de replicación del servidor de configuración.
> 3. [Elimine](site-recovery-vmware-to-azure-manage-vCenter.md#delete-a-vcenter-in-azure-site-recovery) todos los servidores vCenter y hosts de vSphere que estén asociados con el servidor de configuración.


### <a name="delete-the-configuration-server-from-azure-portal"></a>Eliminación del servidor de configuración de Azure Portal
1. En Azure Portal, vaya a **Site Recovery Infrastructure** (Infraestructura de Site Recovery)  > **Servidores de configuración** en el menú Almacén.
2. Haga clic en el servidor de configuración que quiere retirar.
3. En la página de detalles del servidor de configuración, haga clic en el botón Eliminar.

  ![delete-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.PNG)
4. Haga clic en **Sí** para confirmar la eliminación del servidor.

### <a name="uninstall-the-configuration-server-software-and-its-dependencies"></a>Desinstalación del software de servidor de configuración y sus dependencias
  > [!TIP]
  Si tiene pensado volver a usar el servidor de configuración con Azure Site Recovery, puede ir al paso 4 directamente.

1. Inicie sesión como administrador en el servidor de configuración.
2. Abra Panel de Control > Programas > Desinstalar programas.
3. Desinstale los programas en el orden siguiente:
  * Agente de Microsoft Azure Recovery Services
  * Servicio de movilidad de Microsoft Azure Site Recovery/servidor de destino maestro
  * Proveedor de Microsoft Azure Site Recovery
  * Servidor de configuración/servidor de procesos de Microsoft Azure Site Recovery
  * Dependencias del servidor de configuración de Microsoft Azure Site Recovery
  * MySQL Server 5.5
4. Ejecute el siguiente comando desde un símbolo del sistema de administrador:
  ```
  reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
  ```

## <a name="delete-or-unregister-a-configuration-server-powershell"></a>Eliminación o anulación del registro de un servidor de configuración (PowerShell)

1. [Instale](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) el módulo de Azure PowerShell.
2. Inicie sesión en su cuenta de Azure mediante el comando
    
    `Login-AzureRmAccount`
3. Seleccione la suscripción en la que existe el almacén:

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Ahora, configure el contexto de almacén.
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Obtenga el valor seleccionado de su servidor de configuración.

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Eliminación del servidor de configuración

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> La opción **-Force** de Remove-AzureRmSiteRecoveryFabric se puede usar para forzar la eliminación del servidor de configuración.

## <a name="renew-configuration-server-secure-socket-layerssl-certificates"></a>Renovación de certificados de Capa de sockets seguros (SSL) del servidor de configuración
El servidor de configuración lleva integrado un servidor web, que coordina las actividades del servicio de movilidad, los servidores de procesos y los servidores de destino maestros conectados a él. El servidor web del servidor de configuración usa un certificado SSL para autenticar a sus clientes. Este certificado tiene una fecha de expiración de tres años y se puede renovar en cualquier momento mediante el siguiente método:

> [!WARNING]
La expiración del certificado solo se puede realizar en la versión 9.4.XXXX.X o superior. Actualice todos los componentes de Azure Site Recovery (servidor de configuración, servidor de procesos, servidor de destino maestro y servicio de movilidad) antes de comenzar el flujo de trabajo de renovación de certificados.

1. En Azure Portal, vaya a Almacén > Site Recovery Infrastructure (Infraestructura de Site Recovery) > Configuration Server (Servidor de configuración).
2. Haga clic en el servidor de configuración para el que necesita renovar el certificado SSL.
3. En el mantenimiento del servidor de configuración, puede ver la fecha de expiración del certificado SSL.
4. Para renovar el certificado, haga clic en la acción **Renovar certificados** tal y como se muestra en la imagen siguiente:

  ![delete-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/renew-cert-page.png)

### <a name="secure-socket-layer-certificate-expiry-warning"></a>Advertencia de expiración de certificado de Capa de sockets seguros

> [!NOTE]
La validez del certificado SSL para todas las instalaciones que han sucedido antes del mayo de 2016 se estableció en un año. Ha comenzado a ver notificaciones de expiración de certificados en Azure Portal.

1. Si el certificado SSL del servidor de configuración va a expirar en los próximos dos meses, el servicio comienza a notificarlo a los usuarios en Azure Portal y por correo electrónico (debe estar suscrito a las notificaciones de Azure Site Recovery). Comienza a ver un banner de notificación en la página de recursos del almacén.

  ![certificate-notification](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-renew-notification.png)
2. Haga clic en el banner para obtener detalles adicionales sobre la expiración del certificado.

  ![certificate-details](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-expiry-details.png)

  >[!TIP]
  Si, en lugar del botón **Renovar ahora**, ve un botón **Actualizar ahora**, El botón Actualizar ahora indica que hay algunos componentes en el entorno que aún no se han actualizado a la versión 9.4.xxxx.x o superior.

## <a name="revive-a-configuration-server-if-the-secure-socket-layer-ssl-certificate-expired"></a>Renovación de un servidor de configuración si el certificado de Capa de sockets seguros (SSL) ha expirado

1. Actualice el servidor de configuración a la [versión más reciente](http://aka.ms/unifiedinstaller).
2. Si dispone de servidores de proceso de escalado horizontal, servidores de destino maestro para la conmutación por recuperación y servidores de proceso de conmutación por recuperación, actualícelos a la versión más reciente.
3. Actualice Mobility Service en todas las máquinas virtuales protegidas a la versión más reciente.
4. Inicie sesión en el servidor de configuración y abra un símbolo del sistema con privilegios de administrador.
5. Vaya a la carpeta %ProgramData%\ASR\home\svsystems\bin.
6. Ejecute RenewCerts.exe para renovar el certificado SSL en el servidor de configuración.
7. Si el proceso es correcto, debe ver el mensaje "Certificate renewal is Success" (Renovación correcta del certificado).


## <a name="sizing-requirements-for-a-configuration-server"></a>Requisitos de tamaño de un servidor de configuración

| **CPU** | **Memoria** | **Tamaño del disco de caché** | **Frecuencia de cambio de datos** | **Máquinas protegidas** |
| --- | --- | --- | --- | --- |
| 8 vCPU (2 sockets * 4 núcleos @ 2,5 GHz) |16 GB |< 300 GB |500 GB o menos |Replicar menos de 100 máquinas. |
| 12 vCPUs (2 sockets * 6 núcleos @ 2,5 GHz) |18 GB |600 GB |500 GB a 1 TB |Replicar entre 100 y 150 máquinas. |
| 16 vCPUs (2 sockets * 8 núcleos @ 2,5 GHz) |32 GB |1 TB |1 TB a 2 TB |Replicar entre 150 y 200 máquinas. |

  >[!TIP]
  Si la actividad diaria de datos supera los 2 TB o planea replicar más de 200 máquinas virtuales, se recomienda implementar servidores de procesos adicionales para equilibrar el tráfico de replicación. Aprenda cómo implementar servidores de procesos de escalado horizontal.


## <a name="common-issues"></a>Problemas comunes
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]
