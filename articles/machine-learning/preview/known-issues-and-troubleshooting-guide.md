---
title: "Guía de solución de problemas conocidos | Microsoft Docs"
description: "Lista de problemas conocidos y una guía para ayudar a solucionar problemas"
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: f39faea6b7e0886d63085b752f9532a7010ea941
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2017
---
# <a name="azure-machine-learning-workbench---known-issues-and-troubleshooting-guide"></a>Azure Machine Learning Workbench: Guía de solución de problemas conocidos 
En este artículo se le ayuda a buscar y corregir los errores que se producen como parte del uso de la aplicación de Azure Machine Learning Workbench. 

> [!IMPORTANT]
> Cuando se comunica con el equipo de soporte técnico es importante tener el número de compilación. Puede averiguar el número de compilación de la aplicación si hace clic en el menú **Ayuda**. Al hacer clic en el número de compilación, este se copia en el portapapeles para que pueda pegarlo en mensajes de correo electrónico o foros de soporte técnico e informar de los problemas más fácilmente.

![comprobar número de versión](media/known-issues-and-troubleshooting-guide/buildno.png)

## <a name="machine-learning-msdn-forum"></a>Foro de MSDN de Machine Learning
Tenemos un foro de MSDN en el que puede publicar preguntas. El equipo del producto supervisa activamente el foro. La dirección URL del foro es [https://aka.ms/azureml-forum](https://aka.ms/azureml-forum). 

## <a name="gather-diagnostics-information"></a>Recopilación de información de diagnóstico
A veces puede resultar útil proporcionar información de diagnóstico al solicitar ayuda. Aquí es donde se encuentran los archivos de registro:

### <a name="installer"></a>Instalador
Si surge algún problema durante la instalación, los archivos de registro del instalador están aquí:

```
# Windows:
%TEMP%\amlinstaller\logs\*

# macOS:
/tmp/amlinstaller/logs/*
```
Puede comprimir el contenido de estos directorios y enviárnoslo para que podamos realizar un diagnóstico.

### <a name="workbench-desktop-app"></a>Aplicación de escritorio Workbench
Si el escritorio de Workbench se bloquea, aquí puede encontrar los archivos de registro:
```
# Windows
%APPDATA%\AmlWorkbench

# macOS
~/Library/Application Support/AmlWorkbench
``` 
Puede comprimir el contenido de estos directorios y enviárnoslo para que podamos realizar un diagnóstico.

### <a name="experiment-execution"></a>Realización de un experimento
Si se produce un error en un script determinado durante el envío desde la aplicación de escritorio, pruebe a volver a realizar el envío a través de la CLI mediante el comando `az ml experiment submit`. De esta forma aparecerá el mensaje de error completo en formato JSON y, lo más importante, contiene un valor de **Id. de operación**. Envíenos el archivo JSON que incluye el **Id. de operación** para que podamos ayudarle con el diagnóstico. 

Si un script concreto realiza correctamente el envío, pero se produce un error en la ejecución, debe imprimir el **
Id. de ejecución** para identificar dicha ejecución concreta. Los archivos de registro relevantes se pueden empaquetar mediante el siguiente comando:

```azurecli
# Create a ZIP file that contains all the diagnostics information
$ az ml experiment diagnostics -r <run_id> -t <target_name>
```

El comando `az ml experiment diagnostics` genera un archivo `diagnostics.zip` en la carpeta raíz del proyecto. El paquete ZIP contiene la carpeta de proyecto completa en el estado que tenía en el momento en que se ejecutó, además de información de registro. No olvide quitar cualquier información confidencial que no desee incluir antes de enviarnos el archivo de diagnóstico.

## <a name="send-us-a-frown-or-a-smile"></a>Envío de una desaprobación (o una sonrisa)

Cuando trabaja en Azure Machine Learning Workbench nos puede enviar también una desaprobación (o una sonrisa) haciendo clic en el emoticono de la esquina inferior izquierda del shell de la aplicación. Opcionalmente, puede incluir su dirección de correo electrónico (para que podamos responderle), y una captura de pantalla del estado actual. 

## <a name="known-service-limits"></a>Límites de servicio conocidos
- El tamaño máximo de la carpeta de proyecto es 25 MB.
    >[!NOTE]
    >Este límite no se aplica a las carpetas `.git`, `docs` ni `outputs`. Estos nombres de carpeta distinguen entre mayúsculas y minúsculas. Si está trabajando con archivos grandes, consulte [Persisting Changes and Deal with Large Files](how-to-read-write-files.md) (Cambios persistentes y manejo de archivos grandes).

- El tiempo máximo de ejecución permitido del experimento son 7 días.
- El tamaño máximo del archivo de seguimiento de la carpeta `outputs` después de una ejecución es 512 MB.
  - Esto significa que si el script genera un archivo de más de 512 MB en la carpeta outputs, este no se recopilará en esta carpeta. Si está trabajando con archivos grandes, consulte [Persisting Changes and Deal with Large Files](how-to-read-write-files.md) (Cambios persistentes y manejo de archivos grandes).

- No se admiten claves SSH al conectarse a una máquina remota o a un clúster de Spark mediante SSH. Actualmente, solo se admite el modo de usuario y contraseña.

- Las transformaciones de agrupaciones en clústeres de texto no son compatibles con Mac.

- La biblioteca de RevoScalePy solo es compatible con Windows y Linux (en contenedores de Docker). Tampoco es compatible con macOS.

## <a name="file-name-too-long-on-windows"></a>El nombre del archivo es demasiado largo en Windows
Si usa Workbench en Windows, es posible que se encuentre con el límite predeterminado de 260 caracteres de longitud máxima del nombre de archivo, que puede aparecer como un error "El sistema no encuentra la ruta de acceso especificada". Puede modificar una configuración de la clave del Registro para permitir que el nombre de ruta del archivo sea más largo. Revise [este artículo](https://msdn.microsoft.com/en-us/library/windows/desktop/aa365247%28v=vs.85%29.aspx?#maxpath) para obtener más información acerca de cómo establecer la clave del Registro _MAX_PATH_.

## <a name="docker-error-read-connection-refused"></a>Error de Docker "read: connection refused" ("lectura: se ha rechazado la conexión")
Cuando se ejecuta en un contenedor de Docker local, puede que, en algunas ocasiones, aparezca el siguiente error: 
```
Get https://registry-1.docker.io/v2/: 
dial tcp: 
lookup registry-1.docker.io on [::1]:53: read udp [::1]:49385->[::1]:53: 
read: connection refused
```

Se puede corregir cambiando el servidor DNS de Docker de `automatic` a un valor fijo de `8.8.8.8`.

## <a name="remove-vm-execution-error-no-tty-present"></a>Solución del error de ejecución de máquina virtual "no tty present" ("no hay tty presente")
Cuando se ejecuta en un contenedor de Docker de una máquina Linux remota, puede que aparezca el mensaje de error siguiente:
```
sudo: no tty present and no askpass program specified.
``` 
Esto puede suceder si usa Azure Portal para cambiar la contraseña raíz de una máquina virtual Ubuntu Linux. 

Azure Machine Learning Workbench requiere acceso mediante comandos sudo sin contraseña para ejecutarse en hosts remotos. La manera más sencilla de hacerlo consiste en usar _visudo_ para editar el archivo siguiente (puede crear el archivo si no existe):

```
$ sudo visudo -f /etc/sudoers
```

>[!IMPORTANT]
>Es importante editar el archivo con _visudo_ y no con otro comando. _visudo_ comprueba automáticamente la sintaxis de todos los archivos de configuración de sudo y, si no se puede producir un archivo sintácticamente correcto de comandos sudo, puede que se le bloquee el acceso al programa.

Inserte la siguiente línea al final del archivo:

```
username ALL=(ALL) NOPASSWD:ALL
```

Donde _username_ es el nombre que usará Azure Machine Learning Workbench para iniciar sesión en el host remoto.

Se debe colocar la línea después de #includedir "/etc/sudoers.d" ya que, de lo contrario, puede que otra regla la reemplace.

Si tiene una configuración de sudo más complicada, es posible que desee consultar la documentación sobre comandos sudo de Ubuntu que está disponible aquí: https://help.ubuntu.com/community/Sudoers

El error anteriormente mencionado también se puede producir si no usa una máquina virtual Linux basada en Ubuntu en Azure como destino de ejecución. Solo se admiten máquinas virtuales Linux basadas en Ubuntu para la ejecución remota. 

## <a name="vm-disk-is-full"></a>El disco de la máquina virtual está lleno
De forma predeterminada, cuando se crea una nueva máquina virtual Linux en Azure, se obtiene un disco de 30 GB para el sistema operativo. El motor de Docker usará de forma predeterminada el mismo disco para extraer imágenes y ejecutar contenedores. Por ello, puede que se llene el disco del sistema operativo y es posible que aparezca el error "VM Disk is Full" ("El disco de la máquina virtual está lleno") si esto sucede.

Una solución rápida es quitar todas las imágenes de Docker que ya no use. El siguiente comando de Docker hace justamente eso. (Naturalmente deberá iniciar sesión mediante SSH en la máquina virtual para ejecutar el comando de Docker desde un shell de bash).

```
$ docker system prune -a
```

También puede agregar un disco de datos y configurar el motor de Docker para que lo use para almacenar las imágenes. Aquí puede ver el procedimiento para [agregar un disco de datos](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/add-disk). Posteriormente, puede [cambiar la ubicación en la que Docker almacena las imágenes](https://forums.docker.com/t/how-do-i-change-the-docker-image-installation-directory/1169).

O bien, puede expandir el disco del sistema operativo sin tener que tocar la configuración del motor de Docker. Aquí puede ver el procedimiento para [expandir el disco del sistema operativo](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/add-disk).

## <a name="sharing-c-drive-on-windows"></a>Uso compartido de la unidad C en Windows
Si la ejecución se va a realizar en un contenedor de Docker local en Windows, si establece `sharedVolumes` en `true` en el archivo `docker.compute` de `aml_config` puede mejorar el rendimiento de la ejecución. Sin embargo, esto requiere que comparta la unidad C en la _herramienta Docker para Windows_. Si no puede compartir la unidad C, pruebe las siguientes sugerencias:

* Compruebe el uso compartido de la unidad C con el Explorador de archivos.
* Abra la configuración del adaptador de red y desinstale o reinstale "Compartir impresoras y archivos para redes Microsoft" para vEthernet.
* Abra la configuración de Docker y comparta la unidad C en la configuración de Docker.
* Los cambios en la contraseña de Windows afectan al uso compartido. Abra el Explorador de archivos, vuelva a compartir la unidad C y escriba la nueva contraseña.
* También puede producirse un error de firewall al intentar compartir la unidad C con Docker. Esta [publicación acerca de Stack Overflow](http://stackoverflow.com/questions/42203488/settings-to-windows-firewall-to-allow-docker-for-windows-to-share-drive/43904051) puede resultar útil.
* Al compartir la unidad C con credenciales de dominio, el uso compartido podría dejar de funcionar en las redes donde el controlador de dominio no es accesible (por ejemplo, una red doméstica, una red Wi-Fi pública, etc.). Para obtener más información, consulte [esta publicación](https://blogs.msdn.microsoft.com/stevelasker/2016/06/14/configuring-docker-for-windows-volumes/).

También puede evitar el problema del uso compartido, con un pequeño costo de rendimiento, estableciendo `sharedVolumne` en `false` en el archivo `docker.compute`.

## <a name="some-useful-docker-commands"></a>Algunos comandos útiles de Docker

Aquí puede ver algunos comandos útiles de Docker:

```sh
# display all running containers
$ docker ps

# dislplay all containers (running or stopped)
$ docke ps -a

# display all images
$ docker images

# show Docker logs of a container
$ docker logs <container_id>

# create a new container and launch into a bash shell
$ docker run <image_id> /bin/bash

# launch into a bash shell on a running container
$ docker exec -it <container_id> /bin/bash

# stop an running container
$ docker stop <container_id>

# delete a container
$ docker rm <container_id>

# delete an image
$ docker rmi <image_id>

# delete all unussed Docker images 
$ docker system prune -a

```
