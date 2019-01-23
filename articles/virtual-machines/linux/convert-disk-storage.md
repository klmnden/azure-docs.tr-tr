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
ms.component: disks
ms.openlocfilehash: 62c9d01a4322daaa8cf7210f60c5ee42f55b093c
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54476368"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Dönüştürme Azure yönetilen diskler depolaması standart, premium ve bunun tersi de geçerlidir

Yönetilen diskler teklifler üç depolama seçenekleri [Premium SSD](../windows/premium-storage.md), standart SSD ve [standart HDD](../windows/standard-storage.md). Performans ihtiyaçlarınıza göre en düşük kapalı kalma süresi ile seçenekleri arasında kolayca geçiş yapmanızı sağlar. Yönetilmeyen diskler için desteklenmiyor. Ancak kolayca [yönetilen disklere dönüştürme](convert-unmanaged-to-managed-disks.md) disk türleri arasında kolayca geçiş için.

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

## <a name="convert-using-the-azure-portal"></a>Azure portalını kullanarak dönüştürme

Yönetilmeyen diskler için yönetilen diskler Azure portalını kullanarak da dönüştürebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Portalda sanal makinelerinin listeden VM'yi seçin.
3. VM dikey penceresinde, seçin **diskleri** menüsünde.
4. Üst kısmındaki **diskleri** dikey penceresinde **yönetilen disklere geçirme**.
5. Bir kullanılabilirlik kümesindeki sanal makinenizin ise olacaktır uyarı üzerinde **yönetilen disklere geçirme** dikey kullanılabilirlik kümesini önce dönüştürmeniz gerekir. Uyarı kullanılabilirlik kümesini dönüştürmek için tıklayabileceği bir bağlantı olması gerekir. Kullanılabilirlik kümesi dönüştürüldükten sonra veya sanal makinenize bir kullanılabilirlik kümesine değilse tıklayın **geçirme** , diskleri yönetilen disklere geçirme işlemini başlatmak için. 

Sanal makine durdurulacak ve geçiş tamamlandıktan sonra yeniden başlatılacak.

## <a name="next-steps"></a>Sonraki adımlar

Kullanarak bir VM salt okunur bir kopyasını alın [anlık görüntüleri](snapshot-copy-managed-disk.md).

