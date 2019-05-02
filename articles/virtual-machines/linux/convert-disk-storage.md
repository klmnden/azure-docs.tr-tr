---
title: Dönüştürme Azure yönetilen diskler depolaması standart katmandan Premium katmana veya Premiumdan standarda | Microsoft Docs
description: Azure dönüştürmek diskleri depolama standart katmandan Premium katmana veya Premiumdan standarda Azure CLI kullanarak yönetilen.
services: virtual-machines-linux
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 2e7eb455a53abbe2df6ff72f091a599665732429
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64724904"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-or-premium-to-standard"></a>Dönüştürme Azure yönetilen diskler depolaması standart katmandan Premium katmana veya Premiumdan standarda

Dört vardır [disk türleri](disks-types.md) için Azure yönetilen diskler: Ultra Azure Disk Depolama, Premium SSD, standart bir SSD ve standart HDD. Premium SSD, standart bir SSD ve standart az kapalı kalma süresi ile performans ihtiyaçlarınıza göre HDD arasında kolayca geçiş yapabilirsiniz. Bu işlev, yönetilmeyen diskler veya Ultra Yüksek Disk depolama alanı için desteklenmiyor. Ancak kolayca [yönetilen disklere dönüştürme yönetilmeyen](convert-unmanaged-to-managed-disks.md) için disk türleri arasında geçiş yapabilirsiniz.

Bu makalede yönetilen diskler standart katmandan Premium katmana veya Premiumdan standarda Azure CLI'yi kullanarak nasıl dönüştürüleceğini gösterir. Aracı yükseltme veya yüklemek için bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="before-you-begin"></a>Başlamadan önce

* Disk dönüştürme sanal makinenin (VM) yeniden başlatılması gerekiyor, bu nedenle, disk depolama alanı geçişini önceden var olan bir bakım penceresi sırasında zamanlayın.
* Yönetilmeyen diskler için ilk [yönetilen disklere dönüştürme](convert-unmanaged-to-managed-disks.md) için depolama seçenekleri arasında geçiş yapabilirsiniz.


## <a name="switch-all-managed-disks-of-a-vm-between-premium-and-standard"></a>Premium ve standart arasında bir VM'nin tüm yönetilen disklere geçin

Bu örnek, Premium depolama için standart veya Premium standart depolama için bir sanal makinenin diskleri dönüştürme gösterilmektedir. Premium yönetilen diskleri kullanmak için sanal makinenizin kullanmalısınız bir [VM boyutu](sizes.md) , Premium depolamayı destekler. Bu örnek ayrıca Premium Depolama'yı destekleyen bir boyuta geçer.

 ```azurecli

#resource group that contains the virtual machine
rgName='yourResourceGroup'

#Name of the virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from Standard to Premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate the VM before changing the size of the VM
az vm deallocate --name $vmName --resource-group $rgName

#Change the VM size to a size that supports Premium storage 
#Skip this step if converting storage from Premium to Standard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update the SKU of all the data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update the SKU of the OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="switch-individual-managed-disks-between-standard-and-premium"></a>Standart ve Premium arasında bireysel yönetilen disklere geçin

Geliştirme/test yükünüz için maliyetlerinizi azaltmak için standart ve Premium diskler bir karışımını isteyebilirsiniz. Daha iyi performans gerektiren diskleri yükseltmeyi seçebilir. Bu örnek, Premium depolama için standart veya Premium standart depolama için tek bir VM disk dönüştürme gösterilmektedir. Premium yönetilen diskleri kullanmak için sanal makinenizin kullanmalısınız bir [VM boyutu](sizes.md) , Premium depolamayı destekler. Bu örnek ayrıca Premium Depolama'yı destekleyen bir boyuta geçer.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from Standard to Premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the size of the VM
az vm deallocate --ids $vmId 

#Change the VM size to a size that supports Premium storage 
#Skip this step if converting storage from Premium to Standard
az vm resize --ids $vmId --size $size

# Update the SKU
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="switch-managed-disks-between-standard-hdd-and-standard-ssd"></a>Standart HDD ve SSD standart arasında yönetilen disklere geçin

Bu örnek, tek bir sanal makine diski standart HDD'den SSD'ye standart ya da standart HDD'ye standart SSD nasıl dönüştürme yapılacağını gösterir.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Choose between Standard_LRS and StandardSSD_LRS based on your scenario
sku='StandardSSD_LRS'

#Get the parent VM ID 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the disk type
az vm deallocate --ids $vmId 

# Update the SKU
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="switch-managed-disks-between-standard-and-premium-in-azure-portal"></a>Azure portalında arasında standart ve Premium yönetilen disklere geçin

Şu adımları uygulayın:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. VM listesinden seçin **sanal makineler**.
3. VM durduruldu değil, seçin **Durdur** VM üst kısmındaki **genel bakış** bölmesi ve durdurmak sanal makine için bekleyin.
4. VM için bölmesinde seçin **diskleri** menüsünde.
5. Dönüştürmek istediğiniz diski seçin.
6. Seçin **yapılandırma** menüsünde.
7. Değişiklik **hesap türü** gelen **standart HDD** için **Premium SSD** veya **Premium SSD** için **standart HDD**.
8. Seçin **Kaydet**ve disk bölmesini kapatın.

Disk türünü anlık güncelleştirmesidir. Dönüştürme işleminden sonra sanal Makinenizin yeniden başlatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir VM salt okunur bir kopyasını kullanarak olun [anlık görüntüleri](snapshot-copy-managed-disk.md).
