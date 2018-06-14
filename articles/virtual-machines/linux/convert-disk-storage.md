---
title: Azure Dönüştür yönetilen diskleri depolama standart, premium ve tersi yönde | Microsoft Docs
description: Azure dönüştürme diskleri depolama standart Premium'a ve tersi, Azure CLI kullanarak tarafından yönetilir.
services: virtual-machines-linux
documentationcenter: ''
author: ramankum
manager: kavithag
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: c22c2c194cb839c3ec9e3e851768ca19bc6fc443
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
ms.locfileid: "23950514"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Azure Dönüştür yönetilen diskleri depolama standart, premium ve tersi yönde

Yönetilen diskleri iki depolama seçenekleri sunar: [Premium](../windows/premium-storage.md) (SSD tabanlı) ve [standart](../windows/standard-storage.md) (HDD tabanlı). Performans ihtiyaçlarınıza göre en düşük kapalı kalma süresi ile iki seçenek arasında kolayca geçiş yapmanızı sağlar. Bu özellik, yönetilmeyen diskler için kullanılamaz. Ancak kolayca [yönetilen Diske Dönüştür](convert-unmanaged-to-managed-disks.md) kolayca iki seçenek arasında geçiş yapmak için.

Bu makalede yönetilen diskleri standart dönüştürme Premium'a ve bunun tersi de Azure CLI kullanarak gösterilmiştir. Gerekirse yüklemek veya yükseltmek için bkz: [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli.md). 

## <a name="before-you-begin"></a>Başlamadan önce

* Dönüştürme VM yeniden başlatma gerektiriyorsa, bu nedenle, diskleri depolama geçiş önceden var olan bir bakım penceresi sırasında zamanlayın. 
* Yönetilmeyen diskler, ilk kez kullanıyorsanız, [yönetilen Diske Dönüştür](convert-unmanaged-to-managed-disks.md) iki depolama seçenekleri arasında geçiş yapmak için bu makalede kullanılacak. 


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Yönetilen tüm diskleri bir VM, standart, premium dönüştürmek ve tersi yönde

Aşağıdaki örnekte, standart bir VM'den premium depolama için tüm disklerin geçiş yapma gösterir. Yönetilen premium diskleri kullanmak için VM kullanmanız gerekir bir [VM boyutu](sizes.md) premium storage destekler. Bu örnek ayrıca premium depolama destekleyen bir boyuta geçer.

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
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Premium için standart yönetilen disk dönüştürmek ve tersi yönde

Geliştirme ve test iş yükü için maliyetini azaltmak için standart ve premium disklerin karışımına sahip isteyebilirsiniz. Premium depolama alanına, daha iyi performans gerektiren disk yükselterek gerçekleştirebilirsiniz. Aşağıdaki örnekte, tek bir disk standardı VM premium Depolama'ya geçiş yapma gösteriyoruz tersi. Yönetilen premium diskleri kullanmak için VM kullanmanız gerekir bir [VM boyutu](sizes.md) premium storage destekler. Bu örnek ayrıca premium depolama destekleyen bir boyuta geçer.

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

## <a name="next-steps"></a>Sonraki adımlar

Kullanarak bir VM salt okunur bir kopyasını alın [anlık görüntüleri](snapshot-copy-managed-disk.md).

