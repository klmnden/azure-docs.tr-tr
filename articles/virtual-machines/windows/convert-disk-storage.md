---
title: Dönüştürme Azure yönetilen diskler depolaması standart katmandan Premium katmana veya Premiumdan standarda | Microsoft Docs
description: Azure dönüştürmek standart katmandan Premium katmana veya Premiumdan standarda Azure PowerShell kullanarak yönetilen diskleri
services: virtual-machines-windows
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/22/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 5687e6d0094083a9ee58455cc72b0b2e4da32d65
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66417139"
---
# <a name="update-the-storage-type-of-a-managed-disk"></a>Güncelleştirme yönetilen disk depolama türü

Dört vardır disk türleri Azure yönetilen diskler: Azure ultra SSD (Önizleme), premium SSD, standart bir SSD ve HDD standart. Üç GA disk türleri arasında geçiş yapabilirsiniz (premium SSD, standart bir SSD ve HDD standart) performans ihtiyaçlarınıza göre. Kullanmıyorsanız, henüz bir ultra SSD veya yapılan geçiş yapabilir, yeni bir tane dağıtmanız gerekir.

Bu işlev, yönetilmeyen diskler için desteklenmiyor. Ancak kolayca [yönetilmeyen disk bir yönetilen diske dönüştürme](convert-unmanaged-to-managed-disks.md) için disk türleri arasında geçiş yapabilirsiniz.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

* Dönüştürme sanal makine (VM) yeniden başlatılması gerektiğinden, disk depolama alanı geçişini önceden var olan bir bakım penceresi sırasında zamanlamanız gerekir.
* İlk diskinizi yönetilmiyorsa, [yönetilen diske dönüştürme](convert-unmanaged-to-managed-disks.md) için depolama seçenekleri arasında geçiş yapabilirsiniz.

## <a name="switch-all-managed-disks-of-a-vm-between-premium-and-standard"></a>Premium ve standart arasında bir VM'nin tüm yönetilen disklere geçin

Bu örnek, Premium depolama için standart veya Premium standart depolama için bir sanal makinenin diskleri dönüştürme gösterilmektedir. Premium yönetilen diskleri kullanmak için sanal makinenizin kullanmalısınız bir [VM boyutu](sizes.md) , Premium depolamayı destekler. Bu örnek ayrıca premium Depolama'yı destekleyen bir boyuta geçer:

```azurepowershell-interactive
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between Standard_LRS and Premium_LRS based on your scenario
$storageType = 'Premium_LRS'

# Premium capable size
# Required only if converting storage from Standard to Premium
$size = 'Standard_DS2_v2'

# Stop and deallocate the VM before changing the size
Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force

$vm = Get-AzVM -Name $vmName -resourceGroupName $rgName

# Change the VM size to a size that supports Premium storage
# Skip this step if converting storage from Premium to Standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to Premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.ManagedBy -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzDiskUpdateConfig –AccountType $storageType
        Update-AzDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="switch-individual-managed-disks-between-standard-and-premium"></a>Standart ve Premium arasında bireysel yönetilen disklere geçin

Geliştirme/test yükünüz için maliyetlerinizi azaltmak için standart ve Premium diskler bir karışımını isteyebilirsiniz. Daha iyi performans gerektiren diskleri yükseltmeyi seçebilir. Bu örnek, Premium depolama için standart veya Premium standart depolama için tek bir VM disk dönüştürme gösterilmektedir. Premium yönetilen diskleri kullanmak için sanal makinenizin kullanmalısınız bir [VM boyutu](sizes.md) , Premium depolamayı destekler. Bu örnek ayrıca Premium Depolama'yı destekleyen bir boyut için geçiş işlemini gösterir:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between Standard_LRS and Premium_LRS based on your scenario
$storageType = 'Premium_LRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Change the VM size to a size that supports Premium storage
# Skip this step if converting storage from Premium to Standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Update the storage type
$diskUpdateConfig = New-AzDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="convert-managed-disks-from-standard-to-premium-in-the-azure-portal"></a>Yönetilen diskler standart katmandan Premium Azure Portalı'nda Dönüştür

Şu adımları uygulayın:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. VM listesinden seçin **sanal makineler** portalında.
3. VM durduruldu değil, seçin **Durdur** VM üst kısmındaki **genel bakış** bölmesi ve durdurmak sanal makine için bekleyin.
3. VM için bölmesinde seçin **diskleri** menüsünde.
4. Dönüştürmek istediğiniz diski seçin.
5. Seçin **yapılandırma** menüsünde.
6. Değişiklik **hesap türü** gelen **standart HDD** için **Premium SSD**.
7. Tıklayın **Kaydet**ve disk bölmesini kapatın.

Disk türü anlık dönüştürmedir. Dönüştürme işleminden sonra sanal Makinenizin yeniden başlatabilirsiniz.

## <a name="switch-managed-disks-between-standard-hdd-and-standard-ssd"></a>Standart HDD ve SSD standart arasında yönetilen disklere geçin 

Bu örnek, tek bir sanal makine diski standart HDD'den SSD'ye standart ya da standart HDD'ye standart SSD dönüştürme gösterilmektedir:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between Standard_LRS and StandardSSD_LRS based on your scenario
$storageType = 'StandardSSD_LRS'

$disk = Get-AzDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Update the storage type
$diskUpdateConfig = New-AzDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a>Sonraki adımlar

Bir VM salt okunur bir kopyasını kullanarak olun bir [anlık görüntü](snapshot-copy-managed-disk.md).
