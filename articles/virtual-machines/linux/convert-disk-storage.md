---
title: Dönüştürme Azure yönetilen diskler depolaması standart, premium ve tersi | Microsoft Docs
description: Azure dönüştürmek diskleri depolama standart katmandan Premium ve tersi, Azure CLI kullanarak yönetilen.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: 6b78027191d72c10b20c9d09a92c82be4a9e3e05
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56650810"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Dönüştürme Azure yönetilen diskler depolaması standart, premium ve bunun tersi de geçerlidir

Yönetilen diskler sunan dört [disk türü](disks-types.md) seçenekleri: Ultra yüksek katı hal sürücüsü (SSD), Premium SSD, standartları SSD ve standart sabit disk sürücüsü (HDD). Performans ihtiyaçlarınıza göre en düşük kapalı kalma süresi ile seçenekleri arasında kolayca geçiş yapmanızı sağlar. Yönetilmeyen diskler için desteklenmiyor. Ancak kolayca [yönetilen disklere dönüştürme](convert-unmanaged-to-managed-disks.md) disk türleri arasında kolayca geçiş için.

Bu makalede, Premium ve Azure CLI kullanarak tam tersi standart yönetilen diskleri dönüştürme gösterilmektedir. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme](/cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Başlamadan önce

* Dönüştürme sanal makinenin yeniden başlatılması gerekiyor, bu nedenle, diskleri depolama geçişini önceden var olan bir bakım penceresi sırasında zamanlayın. 
* İlk yönetilmeyen diskler kullanıyorsanız [yönetilen disklere dönüştürme](convert-unmanaged-to-managed-disks.md) için depolama seçenekleri arasında geçiş yapmak için bu makaleyi kullanın.


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Bir VM'nin tüm yönetilen diskler için premium, standart dönüştürmek ve bunun tersi de geçerlidir

Aşağıdaki örnek, tüm diskleri VM standart katmandan premium depolamaya geçiş gösterilmektedir. Premium yönetilen diskleri kullanmak için sanal makinenizin kullanmalısınız bir [VM boyutu](sizes.md) , premium depolamayı destekler. Bu örnek ayrıca premium Depolama'yı destekleyen bir boyuta geçer.

 ```azurecli

#resource group that contains the virtual machine
rgName='yourResourceGroup'

#Name of the virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate the VM before changing the size of the VM
az vm deallocate --name $vmName --resource-group $rgName

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update the sku of all the data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update the sku of the OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Yönetilen disk standart katmandan premium, dönüştürmek ve bunun tersi de geçerlidir

Geliştirme/test yükünüz için maliyetlerinizi azaltmak için standart ve premium diskleri karışımına sahip isteyebilirsiniz. Daha iyi performans gerektiren diskleri premium Depolama'ya yükselterek gerçekleştirebilirsiniz. Aşağıdaki örnek, tek bir disk, standart bir VM'den premium depolamaya geçiş işlemi gösterilmektedir ve bunun tersi de geçerlidir. Premium yönetilen diskleri kullanmak için sanal makinenizin kullanmalısınız bir [VM boyutu](sizes.md) , premium depolamayı destekler. Bu örnek ayrıca premium Depolama'yı destekleyen bir boyuta geçer.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the size of the VM
az vm deallocate --ids $vmId 

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --ids $vmId --size $size

# Update the sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="convert-a-managed-disk-from-standard-hdd-to-standard-ssd-and-vice-versa"></a>Yönetilen disk standart HDD standart SSD için dönüştürmek ve bunun tersi de geçerlidir

Aşağıdaki örnek, tek bir standart HDD bir VM'den için standart SSD disk geçiş gösterilmektedir.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Choose between Standard_LRS and StandardSSD_LRS based on your scenario
sku='StandardSSD_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the disk type
az vm deallocate --ids $vmId 

# Update the sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="convert-managed-disks-between-standard-and-premium-in-azure-portal"></a>Standart ve premium Azure portalında arasında yönetilen disklere dönüştürme

Yönetilen diskler standart ve premium Azure portalında arasında dönüştürme yapabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. VM listesinden seçin **sanal makineler** portalında.
3. VM durdurulmamışsa tıklayın **Durdur** üst kısmındaki VM'e genel bakış dikey penceresinde ve durdurmak sanal makine için bekleyin.
3. VM dikey penceresinde, seçin **diskleri** menüsünde.
4. Dönüştürmek istediğiniz diski seçin.
5. Seçin **yapılandırma** menüsünde.
6. Değişiklik **hesap türü** gelen **standart HDD** için **Premium SSD**ve bunun tersi de geçerlidir.
7. Tıklayın **Kaydet** ve disk dikey penceresini kapatın.

Disk türünü güncelleştirme etkilidir anlık. Dönüştürme işleminden sonra sanal Makinenizin yeniden başlatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Kullanarak bir VM salt okunur bir kopyasını alın [anlık görüntüleri](snapshot-copy-managed-disk.md).

