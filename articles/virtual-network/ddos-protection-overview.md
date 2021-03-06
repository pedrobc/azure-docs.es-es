---
title: "Introducción a Protección contra DDoS de Azure estándar| Microsoft Docs"
description: "Conozca el servicio Protección contra DDoS de Azure."
services: virtual-network
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2017
ms.author: kumud
ms.openlocfilehash: 0130823b6a6f5a4883ad640c9bcefc89b82b2cd1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="azure-ddos-protection-standard-overview"></a>Introducción a Protección contra DDoS de Azure estándar

>[!IMPORTANT]
>El servicio Protección contra DDoS de Azure estándar se encuentra actualmente en la versión preliminar. El servicio Protección contra DDoS estándar es compatible con un número limitado de recursos de Azure y en un determinado número de regiones. Necesita [registrarse en el servicio](http://aka.ms/ddosprotection) durante la versión preliminar limitada para habilitar Protección contra DDoS estándar para su suscripción. Después de registrarse, el equipo de DDoS de Azure se pondrá en contacto con usted para guiarle en el proceso de habilitación. El estándar de Protección contra DDoS está disponible en las regiones Este de EE. UU., Oeste de EE. UU. y Centro occidental de EE. UU. Durante la versión preliminar, no se le cobrará por usar el servicio.

Los ataques de denegación de servicio distribuido (DDoS) son uno de los problemas de seguridad y disponibilidad más extendidos a los que se enfrentan los clientes que mueven sus aplicaciones a la nube. Un ataque DDoS intenta agotar los recursos de una aplicación haciendo que esta no esté disponible para los usuarios legítimos. Los ataques DDoS pueden ir dirigidos a cualquier punto de conexión que esté públicamente accesible a través de Internet.

Protección contra DDoS de Azure, en combinación con el uso de procedimientos recomendados de diseño de aplicaciones, constituyen una defensa frente a estos ataques. Se proporcionan estos dos niveles de servicio: 

- **Protección contra DDoS de Azure básica**: está habilitada automáticamente como parte de la plataforma Microsoft Azure sin cargo adicional. La supervisión continua del tráfico y la reducción en tiempo real de los ataques a nivel de red más comunes ofrecen la misma defensa usada por los servicios en línea de Microsoft.  Puede usarse la escala completa de una red global de Azure para distribuir y reducir el tráfico de ataques en las distintas regiones. 
- **Protección contra DDoS de Azure estándar**: ofrece funciones adicionales de reducción de ataques, adaptadas específicamente a los recursos de la red virtual. Es fácil de habilitar y no requiere ningún cambio en la aplicación. Las directivas de protección se ajustan a través de algoritmos de aprendizaje automático y supervisión del tráfico dedicado, y se aplican a direcciones IP públicas asociadas a recursos de Virtual Network, como las instancias de Load Balancer, Application Gateway y Service Fabric.  La telemetría en tiempo real está disponible a través de las vistas de Azure Monitor durante un ataque y para el historial. Se pueden agregar protecciones en el nivel de aplicación mediante el [firewall de aplicaciones web de Application Gateway](https://azure.microsoft.com/services/application-gateway/). 

![Protección contra DDoS de Azure estándar](./media/ddos-protection-overview/ddos-protection-overview-fig2.png)

Para los escenarios de desarrollo y pruebas, le animamos a probar Protección contra DDoS estándar y usar estos recursos para enviar comentarios sobre su experiencia:
- [Protección contra DDoS de Azure en el foro de Microsoft Azure](https://feedback.azure.com/forums/905032-azure-ddos-protection). 
- [Protección contra DDoS de Azure en el foro de MSDN](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azureddosprotection).
- [Protección contra DDos de Azure en Stack Overflow](https://stackoverflow.com/tags/azure-ddos/info)

Para problemas de soporte técnico, puede [abrir una incidencia de soporte técnico de Azure](../azure-supportability/how-to-create-azure-support-request.md).

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>Tipos de ataques DDoS que mitiga Protección contra DDoS estándar

Protección contra DDoS estándar puede mitigar estos tipos de ataque:

- **Ataques volumétricos**: el objetivo del ataque es desbordar la capa de la red con una gran cantidad de tráfico aparentemente legítimo. Esto incluye ataques de tipo “flood” de UDP, de amplificación y otros ataques de tipo “flood” de paquetes falsificados. Protección contra DDoS estándar mitiga estos posibles ataques de varios gigabytes, ya que los absorbe y los limpia automáticamente aprovechando la escala de red global de Azure. 
- **Ataques de protocolo**: estos ataques representan un destino inaccesible aprovechándose de una vulnerabilidad en la pila de protocolo en los niveles 3 y 4. Esto incluye ataques de tipo “flood” de SYN, ataques de reflejo y otros ataques de protocolo. Protección contra DDoS estándar mitiga estos ataques diferenciando entre el tráfico malintencionado y el legítimo. Para ello, interactúa con el cliente y bloquea el tráfico malintencionado. 
- **Ataques de nivel de aplicación**: estos ataques van dirigidos a paquetes de aplicaciones web y su objetivo es interrumpir la transmisión de datos entre hosts. Algunos de ellos son las infracciones de protocolo HTTP, inyección de código SQL, scripts de sitios y otros ataques de nivel 7. Use Application Gateway WAF con Protección contra DDoS estándar para defenderse de estos ataques. 

Protección contra DDoS estándar protege los recursos de las redes virtuales, incluidas las direcciones IP públicas asociadas a máquinas virtuales, los equilibradores de carga internos y las puertas de enlace de aplicaciones. Cuando se combina con la SKU de Application Gateway WAF, Protección contra DDoS estándar puede proporcionar funciones de total mitigación en niveles del 3 al 7.

## <a name="ddos-protection-standard-features"></a>Características de Protección contra DDoS estándar

![Funciones de DDoS](./media/ddos-protection-overview/ddos-overview-fig1.png)

Entre las características de Protección contra DDoS estándar se incluyen: 

- **Integración de plataforma nativa:** Protección contra DDoS estándar se integra de forma nativa en Azure e incluye la configuración a través de Azure Portal y PowerShell. Protección contra DDoS estándar comprende sus recursos y la configuración de recursos.
- **Supervisión del tráfico en todo momento:** los patrones de tráfico de la aplicación se supervisan de forma ininterrumpida en busca de indicadores de ataques DDoS. La mitigación se realiza cuando se sobrepasan las directivas de protección.
- **Protección inmediata:** la configuración simplificada protege de inmediato todos los recursos de una red virtual desde el momento en que se habilita Protección contra DDoS estándar. No se requiere intervención ni configuración del usuario. Protección contra DDoS estándar mitiga el ataque de forma instantánea y automática en cuanto se detecta.
- **Optimización adaptable:** la generación de perfiles de tráfico inteligente va conociendo con el tiempo el tráfico de la aplicación y selecciona y actualiza el perfil que resulta más adecuado para el servicio. El perfil se ajusta a medida que el tráfico cambia con el tiempo.
- **Protección de nivel 3 al 7 con una puerta de enlace de aplicación:** las características de Application Gateway WAF proporcionan protección completa contra DDoS de la pila.
- **Escala de mitigación amplia:** se pueden mitigar más de 60 tipos de ataque diferentes con capacidad global para protegerse contra los ataques DDoS más conocidos. 
- **Métricas de ataque:** con Azure Monitor se puede acceder a un resumen de métricas de cada ataque.
- **Alertas de ataque:** las alertas pueden configurarse en el inicio y la detención de un ataque, y a lo largo de la duración del ataque mediante métricas de ataque integradas. Las alertas se integran en el software operativo como OMS, Splunk, Azure Storage, correo electrónico y Azure Portal.
- **Garantía de costo:** créditos para servicio de escalado horizontal de aplicaciones y transferencia de datos para ataques de DDoS documentados.

## <a name="ddos-protection-standard-mitigation"></a>Mitigación mediante Protección contra DDoS estándar

El servicio Protección contra DDoS de Microsoft supervisa el uso de tráfico real y lo compara constantemente con los umbrales definidos en la directiva de DDoS. Cuando se supera ese umbral de tráfico, se inicia automáticamente la mitigación de DDoS. Cuando el tráfico vuelve a estar por debajo del umbral, se quita la mitigación.

Durante la mitigación, el servicio Protección contra DDoS redirige el tráfico hacia el recurso protegido y realiza varias comprobaciones. Por lo general, estas comprobaciones realizan lo siguiente:

- Asegurarse de que los paquetes se ajustan a las especificaciones de Internet y no tienen un formato incorrecto.
- Interactuar con el cliente para determinar si es un posible paquete falsificado (por ejemplo, autorización de SYN o cookie de SYN, o colocando un paquete para que el origen lo retransmita).
- Limitar la velocidad de los paquetes si no se puede realizar ningún otro método de cumplimiento.

Protección contra DDoS bloquea el tráfico del ataque y reenvía el tráfico restante al destino previsto. En un intervalo de pocos minutos tras la detección del ataque, recibe una notificación mediante las métricas de Azure Monitor. Si configura el registro de datos de telemetría de Protección contra DDoS estándar, puede escribir los registros en opciones disponibles para su posterior análisis. Los datos métricos en Azure Monitor para Protección contra DDoS estándar se conservan actualmente durante 30 días.

No se recomienda a los usuarios que simulen sus propios ataques DDoS. Es preferible que usen el canal de soporte técnico para solicitar una simulación de ataque DDoS ejecutada por Azure Networking. Un ingeniero se pondrá en contacto con usted para organizar los detalles del ataque DDoS (puertos, protocolos, direcciones IP de destino) y concretar una hora para programar la prueba.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre cómo administrar Protección contra DDoS estándar mediante [Azure PowerShell](ddos-protection-manage-ps.md) o [Azure Portal](ddos-protection-manage-portal.md).