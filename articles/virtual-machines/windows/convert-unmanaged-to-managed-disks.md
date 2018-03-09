---
title: "Windows sanal makine yönetilmeyen disklerden yönetilen disklere - Azure yönetilen diskleri dönüştürün. | Microsoft Docs"
description: "Resource Manager dağıtım modelinde PowerShell kullanarak yönetilen disklerde nasıl Windows VM yönetilmeyen Diske Dönüştür"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: cynthn
ms.openlocfilehash: 92168ba5605e119d42ba40ee694cebb3ad116041
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Windows sanal makine yönetilmeyen disklerden yönetilen Diske Dönüştür

Yönetilmeyen diskleri kullanan bir mevcut Windows sanal makineleri (VM'ler) varsa, aracılığıyla yönetilen diskleri kullanmak için sanal makineleri dönüştürebilirsiniz [Azure yönetilen diskleri](managed-disks-overview.md) hizmet. Bu işlem, işletim sistemi diski ve her eklenen veri disklerini dönüştürür.

Bu makale Azure PowerShell kullanarak sanal makineleri dönüştürmek nasıl gösterir. Gerekirse yüklemek veya yükseltmek için bkz: [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps).

## <a name="before-you-begin"></a>Başlamadan önce


* Gözden geçirme [Plan yönetilen disklerin geçiş için](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).

* Gözden geçirme [yönetilen disklere geçişi hakkında SSS](faq-for-disks.md#migrate-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a>Tek Örnekli VM'ler Dönüştür
Bu bölümde tek örnekli Azure VM'ler yönetilmeyen disklerden yönetilen disklere dönüştürmek nasıl ele alınmaktadır. (Bir kullanılabilirlik kümesine Vm'leriniz varsa sonraki bölüme bakın.) 

1. Kullanarak VM serbest bırakma [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet'i. Aşağıdaki örnek adlı VM kaldırır `myVM` kaynak grubunda adlı `myResourceGroup`: 

  ```azurepowershell-interactive
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. VM kullanarak yönetilen Diske Dönüştür [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) cmdlet'i. Aşağıdaki işlem, işletim sistemi diski ve tüm veri diskleri dahil önceki VM dönüştürür ve sanal makineyi başlatır:

  ```azurepowershell-interactive
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```



## <a name="convert-vms-in-an-availability-set"></a>Sanal makineleri bir kullanılabilirlik kümesine Dönüştür

Dönüştürmek istediğiniz sanal makineleri yönetiliyorsa diskleri bir kullanılabilirlik kümesine, önce bir yönetilen kullanılabilirlik kümesi kullanılabilirlik dönüştürmeniz gerekir.

1. Kullanılabilirlik kümesini kullanarak Dönüştür [güncelleştirme AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) cmdlet'i. Aşağıdaki örnek kullanılabilirlik adlandırılmış kümesi güncelleştirmeleri `myAvailabilitySet` kaynak grubunda adlı `myResourceGroup`:

  ```azurepowershell-interactive
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  Burada, kullanılabilirlik kümesi bölge bulunuyorsa yalnızca 2 yönetilen hataya etki alanına sahip ancak yönetilmeyen hata etki alanlarının sayısı 3, bu komutu bir hata "3 belirtilen hata etki alanı sayısı 1-2 aralığında olmalıdır." benzer gösterir Hatayı gidermek için hata etki alanı 2 güncelleştirme güncelleştirin ve `Sku` için `Aligned` gibi:

  ```azurepowershell-interactive
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. Deallocate ve kullanılabilirlik kümesindeki sanal makineleri dönüştürün. Aşağıdaki komut dosyası kullanarak her bir VM kaldırır [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet'ini dönüştürür onu kullanarak [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk)ve otomatik olarak birbirinden dönüştürme işleminin yeniden başlatır :

  ```azurepowershell-interactive
  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

  foreach($vmInfo in $avSet.VirtualMachinesReferences)
  {
     $vm = Get-AzureRmVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
     Stop-AzureRmVM -ResourceGroupName $rgName -Name $vm.Name -Force
     ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
  }
  ```


## <a name="troubleshooting"></a>Sorun giderme

Dönüştürme sırasında bir hata varsa veya bir VM önceki dönüştürme sorunları nedeniyle başarısız bir durumda ise, çalıştırın `ConvertTo-AzureRmVMManagedDisk` cmdlet'ini yeniden. Basit bir yeniden deneme genellikle durum kaldırır.
Dönüştürmeden önce tüm VM Uzantıları 'başarılı sağlanıyor' durumda veya dönüştürme 409 hata kodu ile başarısız olur emin olun.


## <a name="next-steps"></a>Sonraki adımlar

[Standart yönetilen disk premium Dönüştür](convert-disk-storage.md)

Kullanarak bir VM salt okunur bir kopyasını alın [anlık görüntüleri](snapshot-copy-managed-disk.md).

