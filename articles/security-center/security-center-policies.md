---
title: Establecimiento de directivas de seguridad en Azure Security Center | Microsoft Docs
description: Este documento le ha ayudado a configurar directivas de seguridad en el Centro de seguridad de Azure.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: 1cebb6edecd13c6ab32c6854bfd6fe908c1f71f4
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2017
---
# <a name="set-security-policies-in-security-center"></a>Establecimiento de directivas de seguridad en Security Center
Este documento le ayuda a configurar las directivas de seguridad en Security Center guiándole por los pasos necesarios para realizar esta tarea. 


## <a name="how-security-policies-work"></a>¿Cómo funcionan las directivas de seguridad?
Security Center crea automáticamente una directiva de seguridad predeterminada para cada una de las suscripciones de Azure. Puede editar la directiva en Security Center y supervisar el cumplimiento de directivas. 

> [!NOTE]
> Ahora puede ampliar las directivas de Security Center con Azure Policy, que está en la versión preliminar ilimitada. Haga clic [aquí](http://aka.ms/getpolicy) para unirse a la versión preliminar o consulte la documentación [aquí](security-center-azure-policy.md).

Por ejemplo, es posible que los recursos usados para el desarrollo o las pruebas tengan requisitos de seguridad diferentes a los de los recursos que se emplean para aplicaciones de producción. Del mismo modo, es posible que las aplicaciones que usan datos regulados, como la información de identificación personal, requieran un mayor nivel de seguridad. Las directivas de seguridad habilitadas en Azure Security Center controlan las recomendaciones de seguridad y la supervisión para ayudarle a identificar posibles vulnerabilidades y mitigar las amenazas. Consulte [Guía de planeamiento y operaciones de Azure Security Center](security-center-planning-and-operations-guide.md) para más información sobre cómo determinar la opción más adecuada para usted.

## <a name="edit-security-policies"></a>Edición de directivas de seguridad
Puede editar la directiva de seguridad predeterminada de cada una de las suscripciones de Azure en Security Center. Para modificar una directiva de seguridad, debe ser propietario, colaborador o administrador de seguridad de esa suscripción. Inicie sesión en Azure Portal y siga estos pasos para configurar directivas de seguridad en Security Center: 

1.  En el panel **Security Center**, en **General**, haga clic en **Directiva de seguridad**.
2.  Seleccione la suscripción en la que desea habilitar la directiva de seguridad.
3.  En la sección **COMPONENTES DE LA DIRECTIVA**, haga clic en **Directiva de seguridad**.
4.  Esta es la directiva predeterminada asignada a Security Center. Puede activar o desactivar las recomendaciones de seguridad disponibles.
5.  Cuando termine de realizar las modificaciones, haga clic en **Guardar**.

## <a name="available-security-policy-options"></a>Opciones de directivas de seguridad disponibles

Utilice la tabla siguiente como referencia para entender cada opción:

| Directiva | Cuando el estado es Activado |
| --- | --- |
| Actualizaciones del sistema |Recupera una lista diaria de las actualizaciones críticas y de seguridad disponibles en Windows Update o Windows Server Update Services. La lista recuperada depende del servicio que está configurado para esa máquina virtual y recomienda aplicar las actualizaciones que faltan. Para los sistemas Linux, la directiva utiliza el sistema de administración de paquetes proporcionado por la distribución para determinar los paquetes que tienen actualizaciones disponibles. También comprueba las actualizaciones de seguridad y críticas de las máquinas virtuales de [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure.md). |
| Vulnerabilidades del SO |Analiza las configuraciones del sistema operativo diariamente para determinar los problemas que podrían hacer que la máquina virtual sea vulnerable a ataques. La directiva también recomienda cambios en la configuración para afrontar estas vulnerabilidades. Consulte la [lista de líneas de base recomendadas](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) para más información sobre las configuraciones específicas que se están supervisando. (En este momento, Windows Server 2016 no es totalmente compatible). |
| Endpoint Protection |Recomienda que se aprovisione el servicio Endpoint Protection para todas las máquinas virtuales de Windows para ayudar a identificar y a quitar virus, spyware y otro software malintencionado. |
| Cifrado de discos |Se recomienda habilitar el cifrado de disco en todas las máquinas virtuales para mejorar la protección de datos en reposo. |
| Grupos de seguridad de red |Recomienda configurar los [grupos de seguridad de red](../virtual-network/virtual-networks-nsg.md) para controlar el tráfico entrante y saliente de máquinas virtuales que tienen puntos de conexión públicos. Todas las interfaces de red de máquina virtual heredan los grupos de seguridad de red configurados para una subred, a menos que se especifique lo contrario. Además de comprobar que se ha configurado un grupo de seguridad de red, esta directiva evalúa las reglas de seguridad de entrada para identificar las reglas que permiten el tráfico entrante. |
| Firewall de aplicaciones web |Recomienda aprovisionar un firewall de aplicaciones web en máquinas virtuales cuando se cumple una de las siguientes acciones: </br></br>[Dirección IP pública a nivel de instancia](../virtual-network/virtual-networks-instance-level-public-ip.md) (ILPIP) se utiliza y las reglas de seguridad de entrada para el grupo de seguridad de red asociado se configuran para permitir el acceso al puerto 80/443.</br></br>Se utiliza la IP de carga equilibrada y el equilibrio de carga asociado y las reglas de traducción de direcciones de red (NAT) entrantes están configuradas para permitir el acceso al puerto 80 o 443. Para más información, consulte [Compatibilidad de Azure Resource Manager con Load Balancer](../load-balancer/load-balancer-arm.md). |
| Firewall de próxima generación |Amplía las medidas de protección de la red más allá de los grupos de seguridad de red, que están integrados en Azure. Security Center detectará las implementaciones para las que se recomienda un firewall de próxima generación y permitirá aprovisionar una aplicación virtual. |
| Auditoría de SQL y detección de amenazas |Se recomienda que se habilite la auditoría del acceso a Azure Database, con fines de cumplimiento, así como la detección avanzada de amenazas, con fines de investigación. |
| Cifrado de SQL |Recomienda que el cifrado en reposo se habilite para Azure SQL Database, las copias de seguridad asociadas y los archivos de registro de transacciones. De esta forma, aunque haya infracción de datos, no se podrán leer. |
| Evaluación de vulnerabilidades |Se recomienda instalar una solución de evaluación de vulnerabilidades en la máquina virtual. |
| Cifrado de almacenamiento |Actualmente, esta característica está disponible para los archivos y blobs de Azure. Después de habilitar Cifrado del servicio Storage, solo se cifrarán los datos nuevos, mientras que los archivos existentes en esta cuenta de almacenamiento permanecerán sin cifrar. |
| Acceso de red JIT |Cuando se habilita Just-In-Time, Security Center bloquea el tráfico entrante a las máquinas virtuales de Azure mediante la creación de una regla de NSG. Seleccione los puertos de la máquina virtual a los que se debe bloquear el tráfico entrante. Para más información, consulte [Administrar el acceso a máquina virtual mediante Just-In-Time](https://docs.microsoft.com/azure/security-center/security-center-just-in-time). |


## <a name="next-step"></a>Paso siguiente
En este documento ha aprendido a configurar directivas de seguridad en el Centro de seguridad. Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Guía de planeamiento y operaciones de Azure Security Center](security-center-planning-and-operations-guide.md). Aprenda a planificar y entender las consideraciones de diseño para adoptar Azure Security Center.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md). Aprenda a supervisar el estado de los recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md). Aprenda a administrar y responder a las alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md). Aprenda cómo supervisar el estado de mantenimiento de las soluciones de sus asociados.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md). Preguntas más frecuentes acerca del uso del servicio.
* [Blog de seguridad de Azure](http://blogs.msdn.com/b/azuresecurity/). Encuentre artículos de blog sobre el cumplimiento y la seguridad de Azure.
