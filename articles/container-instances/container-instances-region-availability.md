---
title: Disponibilidad de recursos y regiones en Azure Container Instances | Azure Docs
description: "Descubra qué regiones de Azure admiten la implementación de instancias de contenedor y los límites de CPU y de memoria para esas instancias."
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 2b9b1b864bbfd73383759212dd7d91f8e4941544
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="region-availability-for-azure-container-instances"></a>Disponibilidad de regiones en Azure Container Instances

En la versión preliminar, Azure Container Instances está disponible en las siguientes regiones con los límites de memoria y de CPU especificados.

| Ubicación | SO | CPU | Memoria (GB) |
| -------- | -- | :---: | :-----------: |
| Europa occidental, Oeste de EE. UU., Este de EE. UU.  | Linux | 2 | 7 |
| Europa occidental, Oeste de EE. UU., Este de EE. UU.  | Windows | 2 | 3,5 |

## <a name="resource-availability"></a>Disponibilidad de recursos

Las instancias de contenedor creadas dentro de estos límites de recursos están sujetas a disponibilidad dentro de la región de implementación. Cuando una región está sometida a mucha carga, puede experimentar un error al implementar instancias.

Para mitigar este tipo de error de implementación, intente implementar las instancias con una configuración inferior de memoria y de CPU, o intente realizar la implementación en un momento posterior.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo solucionar problemas de la implementación de instancias de contenedor, consulte [Solución de problemas en las implementaciones en Azure Container Instances](container-instances-troubleshooting.md).