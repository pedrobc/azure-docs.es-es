---
title: "Implementación de OpenShift Origin en Azure | Microsoft Docs"
description: Implemente OpenShift Origin en Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 1a40c4cc064b32aced7e976f40f6ed6a57e62204
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2017
---
# <a name="deploy-openshift-origin-in-azure"></a>Implementación de OpenShift Origin en Azure

Existen varias maneras de implementar OpenShift Origin en Azure. Puede implementar manualmente todos los componentes de infraestructura de Azure necesarios y, a continuación, seguir la [documentación](https://docs.openshift.org/3.6/welcome/index.html) de OpenShift Origin.
También puede usar una plantilla de administrador de recursos que simplifique la implementación del clúster de OpenShift Origin. Cuando dicha plantilla se encuentre [aquí](https://github.com/Microsoft/openshift-origin).

## <a name="deploy-using-the-openshift-origin-template"></a>Implementación mediante la plantilla de OpenShift Origin

Use el valor `appId` de la entidad de servicio que creó anteriormente para el parámetro `aadClientId`.

En el ejemplo siguiente se crea un archivo de parámetros denominado **azuredeploy.parameters.json** con todas las entradas necesarias.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_E2s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_E2s_v3"
        },
        "openshiftClusterPrefix": {
            "value": "mycluster"
        },
        "masterInstanceCount": {
            "value": 3
        },
        "infraInstanceCount": {
            "value": 2
        },
        "nodeInstanceCount": {
            "value": 2
        },
        "dataDiskSize": {
            "value": 128
        },
        "adminUsername": {
            "value": "clusteradmin"
        },
        "openshiftPassword": {
            "value": "{Strong Password}"
        },
        "sshPublicKey": {
            "value": "{SSH Public Key}"
        },
        "keyVaultResourceGroup": {
            "value": "keyvaultrg"
        },
        "keyVaultName": {
            "value": "keyvault"
        },
        "keyVaultSecret": {
            "value": "keysecret"
        },
        "aadClientId": {
            "value": "11111111-abcd-1234-efgh-111111111111"
        },
        "aadClientSecret": {
            "value": "{Strong Password}"
        },
        "defaultSubDomainType": {
            "value": "nipio"
        }
    }
}
```

### <a name="deploy-using-azure-cli"></a>Implementación mediante la CLI de Azure


> [!NOTE] 
> El comando siguiente requiere CLI de Azure 2.0.8 o una versión posterior. Puede comprobar la versión de az CLI con el comando `az --version`. Para actualizar la versión de la CLI, consulte [Instalación de la CLI de Azure 2.0]( /cli/azure/install-azure-cli).

En el ejemplo siguiente se implementa el clúster de OpenShift y todos los recursos relacionados en un grupo de recursos denominado myResourceGroup con el nombre de implementación myOpenShiftCluster. Se hace referencia a la plantilla directamente desde el repositorio de GitHub y se usa un archivo de parámetros local denominado **azuredeploy.parameters.json**.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

La implementación tarda al menos 25 minutos en completarse, según el número total de nodos implementados. La dirección URL de la consola de OpenShift y el nombre DNS del maestro OpenShift se imprime en el terminal cuando se haya completado la implementación.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="connect-to-the-openshift-cluster"></a>Conexión con el clúster de OpenShift

Cuando se haya completado la implementación, conéctese a la consola de OpenShift con el explorador usando `OpenShift Console Uri`. También puede conectarse al maestro de OpenShift mediante el siguiente comando:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, puede usar el comando [az group delete](/cli/azure/group#delete) para quitar el grupo de recursos, el clúster de OpenShift y todos los recursos relacionados.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

- [Tareas posteriores a la implementación](./openshift-post-deployment.md)
- [Solución de problemas de implementación de OpenShift](./openshift-troubleshooting.md)
- [Introducción a OpenShift Origin](https://docs.openshift.org/latest/getting_started/index.html)
