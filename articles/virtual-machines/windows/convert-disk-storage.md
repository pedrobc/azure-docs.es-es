---
title: "Conversión de almacenamiento de Azure Managed Disks de estándar a premium, y viceversa | Microsoft Docs"
description: "Conversión de Azure Managed Disks de estándar a premium, y viceversa, mediante Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: ramankum
manager: kavithag
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: e9caa732526c4cf446e9c70ed0a030df81c172dd
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2017
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Conversión de almacenamiento de Azure Managed Disks de estándar a premium, y viceversa

Managed Disks ofrece dos opciones de almacenamiento: [Premium](premium-storage.md) (basado en SSD) y [Estándar](standard-storage.md) (basado en HDD). Permite cambiar fácilmente entre las dos opciones en función de sus necesidades de rendimiento, con el mínimo tiempo de inactividad. Esta funcionalidad no está disponible con discos no administrados. Pero resulta muy sencillo realizar la [conversión a Managed Disks](convert-unmanaged-to-managed-disks.md) para cambiar fácilmente entre las dos opciones.

En este artículo se muestra cómo convertir Managed Disks de estándar a premium, y viceversa, mediante Azure PowerShell. Si necesita instalarlas o actualizarlas, consulte [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps.md) (Instalación y configuración de Azure PowerShell).

## <a name="before-you-begin"></a>Antes de empezar

* La conversión requiere reiniciar la máquina virtual, por lo que debe programar la migración del almacenamiento de discos durante una ventana de mantenimiento existente previamente. 
* Si usa discos no administrados, debe primero realizar la [conversión a Managed Disks](convert-unmanaged-to-managed-disks.md) para usar este artículo para cambiar entre las dos opciones de almacenamiento. 


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Conversión de todos los discos Managed Disks de una máquina virtual de estándar a premium, y viceversa

En el ejemplo siguiente se muestra cómo cambiar todos los discos de una máquina virtual del almacenamiento estándar al premium. Para usar discos administrados premium, la máquina virtual debe usar un [Tamaño de máquina virtual](sizes.md) que admita Premium Storage. En este ejemplo también se pasa a un tamaño que admite Premium Storage.

```azurepowershell-interactive
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'

# Premium capable size
# Required only if converting storage from standard to premium
$size = 'Standard_DS2_v2'
$vm = Get-AzureRmVM -Name $vmName -resourceGroupName $rgName

# Stop and deallocate the VM before changing the size
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.OwnerId -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType $storageType
        Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Conversión de un disco de Managed Disks de estándar a premium, y viceversa

Puede que, en el caso de la carga de trabajo de desarrollo y pruebas, prefiera tener una combinación de discos estándar y premium para reducir el costo. Para ello, puede actualizar a Premium Storage solo los discos que requieren mejor rendimiento. En el ejemplo siguiente se muestra cómo cambiar un solo disco de una máquina virtual del almacenamiento estándar al premium, y viceversa. Para usar discos administrados premium, la máquina virtual debe usar un [Tamaño de máquina virtual](sizes.md) que admita Premium Storage. En este ejemplo también se pasa a un tamaño que admite Premium Storage.

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get the ARM resource to get name and resource group of the VM
$vmResource = Get-AzureRmResource -ResourceId $disk.OwnerId
$vm = Get-AzureRmVM $vmResource.ResourceGroupName -Name $vmResource.ResourceName 

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name -Force

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Update the storage type
$diskUpdateConfig = New-AzureRmDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a>Pasos siguientes

Realice una copia de solo lectura de una máquina virtual mediante [instantáneas](snapshot-copy-managed-disk.md).

