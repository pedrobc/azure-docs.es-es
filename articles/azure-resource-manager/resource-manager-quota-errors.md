---
title: Errores de cuota de Azure | Microsoft Docs
description: "Se describe cómo resolver los errores de cuota de recursos."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: 6a8024a12c4a79e92e37df0a56b6e6bd0cb6a8d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-resource-quotas"></a>Resolución de errores de cuotas de recursos

En este artículo se describen errores de cuota que pueden producirse al implementar recursos.

## <a name="symptom"></a>Síntoma

Si implementa una plantilla que crea recursos que exceden las cuotas de Azure, obtendrá un error de implementación similar al siguiente:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

O bien, es posible que vea:

```
Code=ResourceQuotaExceeded
Message=Creating the resource of type <resource-type> would exceed the quota of <number>
resources of type <resource-type> per resource group. The current resource count is <number>,
please delete some resources of this type before creating a new one.
```

## <a name="cause"></a>Causa

Las cuotas se aplican por grupo de recursos, suscripciones, cuentas y otros ámbitos. Por ejemplo, la suscripción puede configurarse para limitar el número de núcleos para una región. Si trata de implementar una máquina virtual con más núcleos que los permitidos, recibirá un error que indica que se ha superado la cuota.
Para obtener información completa de las cuotas, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../azure-subscription-service-limits.md).

## <a name="solution"></a>Solución

### <a name="solution-1"></a>Solución 1

Para la CLI de Azure, use el comando `az vm list-usage` para encontrar las cuotas de máquina virtual.

```azurecli
az vm list-usage --location "South Central US"
```

Que devuelve:

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

### <a name="solution-2"></a>Solución 2

Para PowerShell, use el comando **Get AzureRmVMUsage** para encontrar las cuotas de máquina virtual.

```powershell
Get-AzureRmVMUsage -Location "South Central US"
```

Que devuelve:

```powershell
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional Cores                         0   100 Count
Virtual Machines                             0 10000 Count
```

### <a name="solution-3"></a>Solución 3

Si necesita aumentar un límite de cuota, vaya al portal y presente una incidencia de soporte técnico para elevar la cuota de la región en la que quiere realizar la implementación.

> [!NOTE]
> Recuerde que para los grupos de recursos, la cuota para cada región individual, no para toda la suscripción. Si necesita implementar 30 núcleos en el oeste de Estados Unidos, debe pedir 30 núcleos de administrador de recursos en el oeste de Estados Unidos. Si necesita implementar 30 núcleos en cualquiera de las regiones para las que tiene acceso, debe pedir 30 núcleos de Resource Manager en todas las regiones.
>
>