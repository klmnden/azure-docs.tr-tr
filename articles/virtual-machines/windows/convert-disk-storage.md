---
title: Dönüştürme Azure yönetilen diskler depolaması standart, premium ve tersi | Microsoft Docs
description: Azure dönüştürmek standart katmandan Premium ve tersi, Azure PowerShell kullanarak yönetilen diskleri
services: virtual-machines-windows
documentationcenter: ''
author: ramankum
manager: kavithag
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/04/2018
ms.author: ramankum
ms.component: disks
ms.openlocfilehash: 4f9e3468cc8ec94eeb3ba936b828e9adfd9a3e6d
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54478527"
---
# <a name="update-the-storage-type-of-a-managed-disk"></a>Güncelleştirme yönetilen disk depolama türü

Azure yönetilen diskler, üç depolama türü seçenekleri sunar: [Premium SSD](../windows/premium-storage.md), [standart SSD](../windows/disks-standard-ssd.md), ve [standart HDD](../windows/standard-storage.md). Yönetilen disk performans ihtiyaçlarınıza göre en düşük kapalı kalma süresiyle depolama türleri arasında geçiş yapabilirsiniz. Depolama türleri arasında geçiş yapma, yönetilmeyen bir disk için desteklenmiyor; Ancak, kolayca [yönetilmeyen disk bir yönetilen diske dönüştürme](convert-unmanaged-to-managed-disks.md).

Bu makalede, Premium ve tersi, Azure PowerShell kullanarak standart yönetilen disk dönüştürme gösterilmektedir. Gerekirse yükleyin veya PowerShell yükseltmek için bkz [Azure PowerShell'i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps?view=azurermps-6.8.1).

## <a name="prerequisites"></a>Önkoşullar

* Dönüştürme, sanal makine (VM) yeniden başlatılması gerektiğinden, diskleri depolama geçişini önceden var olan bir bakım penceresi sırasında zamanlamanız gerekir. 
* İlk yönetilmeyen disk kullanıyorsanız, [yönetilen diske dönüştürme](convert-unmanaged-to-managed-disks.md) depolama türleri arasında geçiş yapmanıza izin vermek için. 
* Bu makaledeki örneklerde, Azure PowerShell modülü sürüm 6.0.0'dan gerektirir veya üzeri. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/azurerm/install-azurerm-ps). Çalıştırma [Connect-AzureRmAccount](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) Azure ile bir bağlantı oluşturmak için.


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium"></a>Bir VM'nin tüm yönetilen diskler için premium standart moddan Dönüştür

Aşağıdaki örnek, tüm diskleri VM standart katmandan premium depolamaya geçiş gösterilmektedir. Premium yönetilen diskler kullanmak için sanal makinenizin kullanmalısınız bir [VM boyutu](sizes.md) , premium depolamayı destekler. Bu örnek ayrıca premium Depolama'yı destekleyen bir boyuta geçer:

```azurepowershell-interactive
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'Premium_LRS'

# Premium capable size
# Required only if converting storage from standard to premium
$size = 'Standard_DS2_v2'

# Stop and deallocate the VM before changing the size
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force

$vm = Get-AzureRmVM -Name $vmName -resourceGroupName $rgName

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.ManagedBy -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType $storageType
        Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="convert-a-managed-disk-from-standard-to-premium"></a>Yönetilen disk standart katmandan premium Dönüştür.

Geliştirme/test yükünüz için maliyetlerinizi azaltmak için standart ve premium diskler bir karışımını isteyebilirsiniz. Bunu, premium depolama için yükseltme dos performans gerektiren diskleri iyi. Aşağıdaki örnek, tek bir disk, standart bir VM'den premium depolamaya geçiş işlemi gösterilmektedir ve bunun tersi de geçerlidir. Premium yönetilen diskler kullanmak için sanal makinenizin kullanmalısınız bir [VM boyutu](sizes.md) , premium depolamayı destekler. Bu örnek ayrıca premium Depolama'yı destekleyen bir boyut için geçiş işlemini gösterir:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'Premium_LRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzureRmResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzureRmVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

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

## <a name="convert-a-managed-disk-from-standard-hdd-to-standard-ssd"></a>Yönetilen disk için standart SSD standart HDD dönüştürme

Aşağıdaki örnek, tek bir standart HDD bir VM'den için standart SSD disk geçiş işlemi gösterilmektedir ve bunun tersi de geçerlidir:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between Standard_LRS and StandardSSD_LRS based on your scenario
$storageType = 'StandardSSD_LRS'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzureRmResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzureRmVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Update the storage type
$diskUpdateConfig = New-AzureRmDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a>Sonraki adımlar

Bir VM salt okunur bir kopyasını kullanarak olun bir [anlık görüntü](snapshot-copy-managed-disk.md).

