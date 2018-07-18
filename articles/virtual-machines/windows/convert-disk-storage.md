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
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: 62cb906000999e6ed97be76c4fe9b15e9ec1cb91
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39092479"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Dönüştürme Azure yönetilen diskler depolaması standart, premium ve bunun tersi de geçerlidir

Yönetilen diskler, üç depolama seçeneği sunar: [Premium SSD](../windows/premium-storage.md), standart SSD(Preview) ve [standart HDD](../windows/standard-storage.md). Performans ihtiyaçlarınıza göre en düşük kapalı kalma süresi ile seçenekleri arasında kolayca geçiş yapmanızı sağlar. Yönetilmeyen diskler için desteklenmiyor. Ancak kolayca [yönetilen disklere dönüştürme](convert-unmanaged-to-managed-disks.md) disk türleri arasında kolayca geçiş için.

Bu makalede, Premium ve bunun tersi de Azure PowerShell kullanarak standart yönetilen diskleri dönüştürme gösterilmektedir. Gerekirse yüklemek veya yükseltmek bkz [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps.md).

## <a name="before-you-begin"></a>Başlamadan önce

* Dönüştürme sanal makinenin yeniden başlatılması gerekiyor, bu nedenle, diskleri depolama geçişini önceden var olan bir bakım penceresi sırasında zamanlayın. 
* İlk yönetilmeyen diskler kullanıyorsanız [yönetilen disklere dönüştürme](convert-unmanaged-to-managed-disks.md) için depolama seçenekleri arasında geçiş yapmak için bu makaleyi kullanın. 
* Bu makale Azure PowerShell modülü sürüm 6.0.0'dan gerekir veya üzeri. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). Ayrıca Azure ile bağlantı oluşturmak için `Connect-AzureRmAccount` komutunu çalıştırmanız gerekir.


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Bir VM'nin tüm yönetilen diskler için premium, standart dönüştürmek ve bunun tersi de geçerlidir

Aşağıdaki örnek, tüm diskleri VM standart katmandan premium depolamaya geçiş gösterilmektedir. Premium yönetilen diskleri kullanmak için sanal makinenizin kullanmalısınız bir [VM boyutu](sizes.md) , premium depolamayı destekler. Bu örnek ayrıca premium Depolama'yı destekleyen bir boyuta geçer.

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
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Yönetilen disk standart katmandan premium, dönüştürmek ve bunun tersi de geçerlidir

Geliştirme/test yükünüz için maliyetlerinizi azaltmak için standart ve premium diskleri karışımına sahip isteyebilirsiniz. Daha iyi performans gerektiren diskleri premium Depolama'ya yükselterek gerçekleştirebilirsiniz. Aşağıdaki örnek, tek bir disk, standart bir VM'den premium depolamaya geçiş işlemi gösterilmektedir ve bunun tersi de geçerlidir. Premium yönetilen diskleri kullanmak için sanal makinenizin kullanmalısınız bir [VM boyutu](sizes.md) , premium depolamayı destekler. Bu örnek ayrıca premium Depolama'yı destekleyen bir boyuta geçer.

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
$vmResource = Get-AzureRmResource -ResourceId $disk.diskId

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name -Force

$vm = Get-AzureRmVM $vmResource.ResourceGroupName -Name $vmResource.ResourceName 

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

## <a name="convert-a-managed-disk-from-standard-hdd-to-standard-ssd-and-vice-versa"></a>Yönetilen disk standart HDD standart SSD için dönüştürmek ve bunun tersi de geçerlidir

Aşağıdaki örnek, tek bir standart HDD bir VM'den için standart SSD disk geçiş işlemi gösterilmektedir ve bunun tersi de geçerlidir.

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

$vm = Get-AzureRmVM $vmResource.ResourceGroupName -Name $vmResource.ResourceName 

# Update the storage type
$diskUpdateConfig = New-AzureRmDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a>Sonraki adımlar

Kullanarak bir VM salt okunur bir kopyasını alın [anlık görüntüleri](snapshot-copy-managed-disk.md).

