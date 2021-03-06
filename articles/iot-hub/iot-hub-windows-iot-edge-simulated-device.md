---
title: "Simulación de un dispositivo con Azure IoT Edge (Windows) | Documentos de Microsoft"
description: "Describe cómo usar Azure IoT Edge en Windows para crear un dispositivo simulado que envíe datos de telemetría a través de una puerta de enlace de Azure IoT Edge a un centro de IoT Hub."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 6a2aeda0-696a-4732-90e1-595d2e2fadc6
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: andbuc
ms.openlocfilehash: 0aa1836ee1445894022b95fefc2338ef53698240
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-windows"></a>Uso de Azure IoT Edge para enviar mensajes de dispositivo a nube con un dispositivo simulado (Windows)

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="run-the-sample"></a>Ejecución del ejemplo

El script **build.cmd** genera su salida en la carpeta **build** de la copia local del repositorio **iot-edge**. Esta salida incluye los cuatro módulos de IoT Edge utilizados en este ejemplo.

El script de compilación crea los siguientes archivos:

* **logger.dll** en la carpeta **build\\modules\\logger\\Debug**.
* **iothub.dll** en la carpeta **build\\modules\\iothub\\Debug**.
* **identity\_map.dll** en la carpeta **build\\modules\\identitymap\\Debug**.
* **simulated\_device.dll** en la carpeta **build\\modules\\simulated\_device\\Debug**.

Utilice estas rutas de acceso para los valores de **ruta de acceso del módulo**, tal y como se muestra en el archivo de configuración JSON simulated\_device\_cloud\_upload\_win.

El proceso de ejemplo simulated\_device\_cloud\_upload considera la ruta de acceso a un archivo de configuración JSON como argumento de línea de comandos. El siguiente archivo JSON de ejemplo se proporciona en el repositorio de SDK en **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_win.json**. Este archivo de configuración funciona tal y como está, a menos que se modifique el script de compilación para colocar los módulos de IoT Edge o los ejecutables de ejemplo en ubicaciones diferentes de las predeterminadas.

> [!NOTE]
> Las rutas de acceso del módulo son relativas al directorio donde se encuentra simulated\_device\_cloud\_upload\_sample.exe. De manera predeterminada, el archivo de configuración JSON de ejemplo escribe en "deviceCloudUploadGatewaylog.log" en el directorio de trabajo actual.

En un editor de texto, abra el archivo **samples\\simulated\_device\_cloud\_upload\\src\\simulated\_device\_cloud\_upload\_win.json** en la copia local del repositorio **iot-edge**. Este archivo configura los módulos de IoT Edge en la puerta de enlace de ejemplo:

* El módulo **IoTHub** conecta con el centro de IoT. Debe configurarlo para enviar datos a IoT Hub. En concreto, establezca el valor **IoTHubName** en el nombre de IoT Hub y el valor **IoTHubSuffix** en **azure-devices.net**. Establezca el valor **Transport** en uno de los siguientes: **HTTP**, **AMQP** o **MQTT**. Actualmente, solo **HTTP** comparte una conexión TCP para todos los mensajes de dispositivo. Si establece el valor en **AMQP** o **MQTT**, la puerta de enlace mantendrá una conexión TCP independiente con IoT Hub para cada dispositivo.
* El módulo **mapping** asigna las direcciones MAC de los dispositivos simulados a los identificadores de dispositivo de IoT Hub. Defina los valores de **deviceId** con los identificadores de los dos dispositivos que ha agregado a IoT Hub. Establezca los valores de **deviceKey** con las claves de los dos dispositivos.
* Los módulos **BLE1** y **BLE2** son los dispositivos simulados. Observe cómo las direcciones MAC del módulo coinciden con las del módulo de **asignación**.
* El módulo **Registrador** registra la actividad de puerta de enlace en un archivo.
* Los valores **module.path** mostrados en el ejemplo siguiente son relativos al directorio donde se encuentra simulated\_device\_cloud\_upload\_sample.exe.
* La matriz **links** de la parte inferior del archivo JSON conecta los módulos **BLE1** y **BLE2** al módulo de **asignación** y el módulo de **asignación** al módulo **IoTHub**. También garantiza que el módulo **Registrador** registre todos los mensajes.

```json
{
    "modules" :
    [
      {
        "name": "IotHub",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\iothub\\Debug\\iothub.dll"
          }
          },
          "args": {
            "IoTHubName": "<<insert here IoTHubName>>",
            "IoTHubSuffix": "<<insert here IoTHubSuffix>>",
            "Transport": "HTTP"
          }
        },
      {
        "name": "mapping",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\identitymap\\Debug\\identity_map.dll"
          }
          },
          "args": [
            {
              "macAddress": "01:01:01:01:01:01",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            },
            {
              "macAddress": "02:02:02:02:02:02",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            }
          ]
        },
      {
        "name": "BLE1",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "01:01:01:01:01:01",
            "messagePeriod" : 2000
          }
        },
      {
        "name": "BLE2",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "02:02:02:02:02:02",
            "messagePeriod" : 2000
          }
        },
      {
        "name": "Logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
          }
        },
        "args": {
          "filename": "deviceCloudUploadGatewaylog.log"
        }
      }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IotHub" }
    ]
}
```

Guarde los cambios realizados en el archivo de configuración.

Para ejecutar el ejemplo:

1. En un símbolo del sistema, vaya a la carpeta **build** de la copia local del repositorio **iot-edge**.
2. Ejecute el siguiente comando:
   
    ```cmd
    samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe ..\samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. Puede usar la herramienta [Explorador de dispositivos][lnk-device-explorer] o [iothub-explorer][lnk-iothub-explorer] para supervisar los mensajes que la instancia de IoT Hub recibe de la puerta de enlace. Por ejemplo, mediante iothub-explorer, puede supervisar los mensajes del dispositivo a la nube con el comando siguiente:

    ```cmd
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Pasos siguientes

Para lograr una descripción más avanzada de IoT Edge y experimentar con algunos ejemplos de código, consulte los siguientes tutoriales y recursos para desarrolladores:

* [Envío de mensajes de un dispositivo físico a la nube con IoT Edge][lnk-physical-device]
* [Azure IoT Edge][lnk-iot-edge]

Para explorar aún más las funcionalidades de IoT Hub, consulte:

* [Guía para desarrolladores de IoT Hub][lnk-devguide]
* [Protección total de la solución de IoT][lnk-securing]

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
