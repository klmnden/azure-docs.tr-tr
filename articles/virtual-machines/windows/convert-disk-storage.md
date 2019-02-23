---
title: Dönüştürme Azure yönetilen diskler depolaması standart, premium ve tersi | Microsoft Docs
description: Azure dönüştürmek standart katmandan Premium ve tersi, Azure PowerShell kullanarak yönetilen diskleri
services: virtual-machines-windows
documentationcenter: ''
author: ramankumarlive
manager: kavithag
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/22/2019
ms.author: ramankum
ms.subservice: disks
ms.openlocfilehash: b1230c033019d21228fe5283e3ee6cfa478bef4c
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56727035"
---
# <a name="update-the-storage-type-of-a-managed-disk"></a>Güncelleştirme yönetilen disk depolama türü

Azure diskleri teklifler dört depolama türü seçenekleri yönetilen: Ultra yüksek katı hal sürücüleri (SSD), Premium SSD, standart bir SSD ve standart sabit disk sürücülerinin (HDD). Yönetilen disk performans ihtiyaçlarınıza göre en düşük kapalı kalma süresiyle depolama türleri arasında geçiş yapabilirsiniz. Depolama türleri arasında geçiş yapma, yönetilmeyen bir disk için desteklenmiyor; Ancak, kolayca [yönetilmeyen disk bir yönetilen diske dönüştürme](convert-unmanaged-to-managed-disks.md).

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="prerequisites"></a>Önkoşullar

* Dönüştürme, sanal makine (VM) yeniden başlatılması gerektiğinden, diskleri depolama geçişini önceden var olan bir bakım penceresi sırasında zamanlamanız gerekir.
* İlk yönetilmeyen disk kullanıyorsanız, [yönetilen diske dönüştürme](convert-unmanaged-to-managed-disks.md) depolama türleri arasında geçiş yapmanıza izin vermek için.

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
Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force

$vm = Get-AzVM -Name $vmName -resourceGroupName $rgName

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
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

## <a name="convert-a-managed-disk-from-standard-to-premium"></a>Yönetilen disk standart katmandan premium Dönüştür.

Geliştirme/test yükünüz için maliyetlerinizi azaltmak için standart ve premium diskler bir karışımını isteyebilirsiniz. Bunu yapmak için daha iyi performans gerektiren diskler için premium depolama yükseltin. Aşağıdaki örnek, tek bir disk, standart bir VM'den premium depolamaya geçiş işlemi gösterilmektedir ve bunun tersi de geçerlidir. Premium yönetilen diskler kullanmak için sanal makinenizin kullanmalısınız bir [VM boyutu](sizes.md) , premium depolamayı destekler. Bu örnek ayrıca premium Depolama'yı destekleyen bir boyut için geçiş işlemini gösterir:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'Premium_LRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Update the storage type
$diskUpdateConfig = New-AzDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="convert-managed-disks-from-standard-to-premium-in-azure-portal"></a>Azure Portalı'nda premium yönetilen diskler standart Dönüştür

Yönetilen disk Standart'tan Azure portalında premium dönüştürebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. VM listesinden seçin **sanal makineler** portalında.
3. VM durdurulmamışsa tıklayın **Durdur** üst kısmındaki VM'e genel bakış dikey penceresinde ve durdurmak sanal makine için bekleyin.
3. VM dikey penceresinde, seçin **diskleri** menüsünde.
4. Dönüştürmek istediğiniz diski seçin.
5. Seçin **yapılandırma** menüsünde.
6. Değişiklik **hesap türü** gelen **standart HDD** için **Premium SSD**.
7. Tıklayın **Kaydet** ve disk dikey penceresini kapatın.

Disk türü güncelleştirme etkilidir anlık. Dönüştürme işleminden sonra sanal Makinenizin yeniden başlatabilirsiniz.

## <a name="convert-a-managed-disk-from-standard-hdd-to-standard-ssd"></a>Yönetilen disk için standart SSD standart HDD dönüştürme

Aşağıdaki örnek, tek bir standart HDD bir VM'den için standart SSD disk geçiş işlemi gösterilmektedir ve bunun tersi de geçerlidir:

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

