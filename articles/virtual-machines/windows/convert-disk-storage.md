---
title: "Azure Dönüştür yönetilen diskleri depolama standart, premium ve tersi yönde | Microsoft Docs"
description: "Azure dönüştürme diskleri standart Premium'a ve tersi, Azure PowerShell kullanarak tarafından yönetilir."
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
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Azure Dönüştür yönetilen diskleri depolama standart, premium ve tersi yönde

Yönetilen diskleri iki depolama seçenekleri sunar: [Premium](premium-storage.md) (SSD tabanlı) ve [standart](standard-storage.md) (HDD tabanlı). Performans ihtiyaçlarınıza göre en düşük kapalı kalma süresi ile iki seçenek arasında kolayca geçiş yapmanızı sağlar. Bu özellik, yönetilmeyen diskler için kullanılamaz. Ancak kolayca [yönetilen Diske Dönüştür](convert-unmanaged-to-managed-disks.md) kolayca iki seçenek arasında geçiş yapmak için.

Bu makalede yönetilen diskleri standart dönüştürme Premium'a ve bunun tersi de Azure PowerShell kullanarak gösterilmiştir. Gerekirse yüklemek veya yükseltmek için bkz: [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps.md).

## <a name="before-you-begin"></a>Başlamadan önce

* Dönüştürme VM yeniden başlatma gerektiriyorsa, bu nedenle, diskleri depolama geçiş önceden var olan bir bakım penceresi sırasında zamanlayın. 
* Yönetilmeyen diskler, ilk kez kullanıyorsanız, [yönetilen Diske Dönüştür](convert-unmanaged-to-managed-disks.md) iki depolama seçenekleri arasında geçiş yapmak için bu makalede kullanılacak. 


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Yönetilen tüm diskleri bir VM, standart, premium dönüştürmek ve tersi yönde

Aşağıdaki örnekte, standart bir VM'den premium depolama için tüm disklerin geçiş yapma gösterir. Yönetilen premium diskleri kullanmak için VM kullanmanız gerekir bir [VM boyutu](sizes.md) premium storage destekler. Bu örnek ayrıca premium depolama destekleyen bir boyuta geçer.

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
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Premium için standart yönetilen disk dönüştürmek ve tersi yönde

Geliştirme ve test iş yükü için maliyetini azaltmak için standart ve premium disklerin karışımına sahip isteyebilirsiniz. Premium depolama alanına, daha iyi performans gerektiren disk yükselterek gerçekleştirebilirsiniz. Aşağıdaki örnekte, tek bir disk standardı VM premium Depolama'ya geçiş yapma gösteriyoruz tersi. Yönetilen premium diskleri kullanmak için VM kullanmanız gerekir bir [VM boyutu](sizes.md) premium storage destekler. Bu örnek ayrıca premium depolama destekleyen bir boyuta geçer.

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

## <a name="next-steps"></a>Sonraki adımlar

Kullanarak bir VM salt okunur bir kopyasını alın [anlık görüntüleri](snapshot-copy-managed-disk.md).

