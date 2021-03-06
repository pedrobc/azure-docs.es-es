---
title: "Continuidad empresarial y recuperación ante desastres (BCDR): regiones emparejadas de Azure | Microsoft Docs"
description: "Conozca más sobre el emparejamiento de regiones de Azure para asegurarse de que las aplicaciones sean resistentes durante los errores del centro de datos."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: c2d0a21c-2564-4d42-991a-bc31723f61a4
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2017
ms.author: raynew
ms.openlocfilehash: 27491e34ad9e47aec2f424cfc439fad614f0e435
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>Continuidad empresarial y recuperación ante desastres (BCDR): regiones emparejadas de Azure

## <a name="what-are-paired-regions"></a>¿Qué son las regiones emparejadas?

Azure funciona en varias ubicaciones geográficas del mundo. Una ubicación geográfica de Azure es un área definida del mundo que contiene al menos una región de Azure. Una región de Azure es un área dentro de una ubicación geográfica que contiene uno o varios centros de datos.

Cada región de Azure se empareja con otra región de la misma zona geográfica, que juntas forman un emparejamiento regional. La excepción es el Sur de Brasil, ya que se trata de una región emparejada con otra que se encuentra fuera de su ubicación geográfica.

![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

Ilustración 1: Diagrama de pareja regional de Azure

| Geography | Regiones emparejadas |  |
|:--- |:--- |:--- |
| Asia |Asia oriental |Sudeste asiático |
| Australia |Australia Oriental |Sudeste de Australia |
| Canadá |Centro de Canadá |Este de Canadá |
| China |Norte de China |Este de China|
| India |India Central |Sur de la India |
| Japón |Este de Japón |Oeste de Japón |
| Corea |Corea Central |Corea del Sur |
| Norteamérica |Centro-Norte de EE. UU |Centro-Sur de EE. UU |
| Norteamérica |Este de EE. UU. |Oeste de EE. UU. |
| Norteamérica |Este de EE. UU. 2 |Central EE. UU.: |
| Norteamérica |Oeste de EE. UU. 2 |Centro occidental de EE.UU. |
| Europa |Europa del Norte |Europa occidental |
| Japón |Este de Japón |Oeste de Japón |
| Brasil |Sur de Brasil (1) |Centro-Sur de EE. UU |
| Gobierno de Estados Unidos |Gobierno de EE. UU. - Iowa |Gobierno de EE. UU. - Virginia |
| Gobierno de Estados Unidos |Gobierno de EE. UU.: Arizona |Gobierno de EE. UU.: Texas |
| Departamento de Defensa de Estados Unidos |Departamento de Defensa de EE. UU. Este |Departamento de Defensa de EE. UU. Centro |
| Reino Unido |Oeste de Reino Unido |Sur del Reino Unido 2 |
| Alemania |Centro de Alemania |Noreste de Alemania |

Tabla 1: Asignación de las parejas regionales de Azure

> Sur de Brasil (1) es un caso único porque se empareja con una región fuera de su propia ubicación geográfica. La región secundaria del Sur de Brasil es Centro y Sur de EE. UU., pero la región secundaria de esta última no es el Sur de Brasil.


Se recomienda que replique las cargas de trabajo entre las parejas regionales para beneficiarse de las directivas de aislamiento y disponibilidad de Azure. Por ejemplo, las actualizaciones planeadas del sistema de Azure se implementan de forma secuencial (no al mismo tiempo) entre regiones emparejadas. Esto significa que, incluso en el caso excepcional de una actualización defectuosa, ambas regiones no se verán afectadas al mismo tiempo. Además, en el improbable caso de una interrupción amplia, se da prioridad a la recuperación de al menos una región de cada pareja.

## <a name="an-example-of-paired-regions"></a>Un ejemplo de regiones emparejadas
La figura 2 muestra una aplicación hipotética que utiliza el par regional para la  recuperación ante desastres. Los números verdes resaltan las actividades entre regiones de tres servicios de Azure (Compute, Storage y Database) y cómo están configurados para replicar entre regiones. Con los números de color naranja se resaltan los beneficios exclusivos de la implementación en regiones emparejadas.

![Información general sobre las ventajas de las regiones emparejadas](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

Ilustración 2: Pareja regional de Azure hipotética

## <a name="cross-region-activities"></a>Actividades entre regiones
Como se indica en la ilustración 2.

![PaaS](./media/best-practices-availability-paired-regions/1Green.png)**Azure Compute (Paas)**: debe aprovisionar recursos de procesos adicionales de antemano para asegurarse de que haya recursos disponibles en otra región durante un desastre. Para obtener más información, consulte [Guía técnica sobre resistencia en Azure](resiliency/resiliency-technical-guidance.md).

![Storage](./media/best-practices-availability-paired-regions/2Green.png)**Azure Storage**: el almacenamiento con redundancia geográfica (GRS) se configura de manera predeterminada cuando se crea una cuenta de Azure Storage. Con GRS, los datos se replican automáticamente tres veces dentro de la región primaria y tres veces en la región emparejada. Para obtener más información, consulte [Opciones de redundancia de Almacenamiento de Azure](storage/common/storage-redundancy.md).

![Azure SQL](./media/best-practices-availability-paired-regions/3Green.png)**Azure SQL Databases**: con la replicación geográfica estándar de SQL de Azure puede configurar la replicación asincrónica de transacciones en una región emparejada. Con la replicación geográfica Premium, puede configurar la replicación en cualquier región del mundo; sin embargo, se recomienda implementar estos recursos en una región emparejada para la mayoría de los escenarios de recuperación ante desastres. Para más información, consulte [Replicación geográfica en Azure SQL Database](sql-database/sql-database-geo-replication-overview.md).

![Resource Manager](./media/best-practices-availability-paired-regions/4Green.png)**Azure Resource Manager**: Azure Resource Manager proporciona de forma inherente aislamiento lógico para los componentes de administración de servicios entre regiones. Esto significa que es menos probable que los errores lógicos de una región afecten a otra.

## <a name="benefits-of-paired-regions"></a>Ventajas de las regiones emparejadas
Como se indica en la ilustración 2.  

![Aislamiento](./media/best-practices-availability-paired-regions/5Orange.png)
**Aislamiento físico**: cuando sea posible, Azure prefiere por lo menos 300 millas de separación entre los centros de datos de un emparejamiento regional, aunque esto no será práctico o posible en todo el mundo. La separación del centro de datos físico reduce la probabilidad de que los desastres naturales, los disturbios civiles, los cortes del suministro eléctrico o las interrupciones de la red física afecten simultáneamente a ambas regiones. El aislamiento está sujeto a las restricciones geográficas (tamaño de la ubicación geográfica, disponibilidad de la infraestructura de red/energía, normativas, etc.).  

![Replicación](./media/best-practices-availability-paired-regions/6Orange.png)
**Replicación proporcionada por la plataforma**: algunos servicios, como el almacenamiento con redundancia geográfica ofrecen replicación automática a la región emparejada.

![Recuperación](./media/best-practices-availability-paired-regions/7Orange.png)
**Orden de recuperación de región**: en el caso de una interrupción amplia, tiene prioridad la recuperación de una región por cada pareja. Se garantiza que, si las aplicaciones se implementan en regiones emparejadas, se dará prioridad a la recuperación de una de las regiones. Si una aplicación se implementa en regiones que no están emparejadas, podría retrasarse la recuperación; en el peor de los casos, es posible que las regiones elegidas sean las dos últimas en recuperarse.

![Actualizaciones](./media/best-practices-availability-paired-regions/8Orange.png)
**Actualizaciones secuenciales**: las actualizaciones planeadas del sistema de Azure se implementan en regiones emparejadas de forma secuencial (no al mismo tiempo) para minimizar el tiempo de inactividad, el efecto de los errores y la aparición de errores lógicos en el infrecuente caso de una actualización incorrecta.

![Datos](./media/best-practices-availability-paired-regions/9Orange.png)
**Residencia de datos**: una región reside dentro de la misma ubicación geográfica que su pareja (a excepción del Sur de Brasil) con el objeto de cumplir los requisitos de residencia de datos para fines de jurisdicción de impuestos y aplicación de la ley.
