---
title: "Introducción a la conexión de dispositivos físicos a Azure IoT Hub | Microsoft Docs"
description: "Aprenda a conectar dispositivos físicos y paneles a Azure IoT Hub. Los dispositivos pueden enviar datos de telemetría a IoT Hub y este servicio supervisa y administra los dispositivos."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: tutorial de azure iot hub
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: f4128b6b049aa876e170c56dcf2e40720644dc3d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="azure-iot-hub-get-started-with-physical-devices-tutorials"></a>Tutoriales de introducción a Azure IoT Hub con dispositivos físicos

Estos tutoriales le presentan Azure IoT Hub y los SDK de dispositivo. Los tutoriales cubren escenarios comunes de IoT para demostrar las funcionalidades de IoT Hub. Los tutoriales también muestran cómo combinar IoT Hub con otras herramientas y servicios de Azure para crear soluciones de IoT más eficaces. Los tutoriales de la tabla siguiente muestran cómo crear dispositivos de IoT físicos.

| Dispositivo IoT                       | Lenguaje de programación |
|---------------------------------|----------------------|
| Raspberry Pi                    | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| IoT DevKit                      | [Arduino en VSCode][DevKit]     |
| Intel Edison                    | [Node.js][Ed_Nd], [C][Ed_C]           |
| Adafruit Feather HUZZAH ESP8266 | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 Thing Dev      | [Arduino][Th_Ard]              |
| Adafruit Feather M0             | [Arduino][M0_Ard]              |

Además, puede usar una puerta de enlace de IoT Edge para habilitar dispositivos para conectarse a su IoT Hub.

| Dispositivo de puerta de enlace               | Lenguaje de programación | Plataforma         |
|------------------------------|----------------------|------------------|
| Intel NUC (modelo DE3815TYKE) | C                    | [Wind River Linux][NUC_Lnx] |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]


[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
