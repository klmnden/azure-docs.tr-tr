---
title: Bir Windows sanal makine yönetilmeyen disklerden yönetilen disklere - Azure yönetilen diskleri dönüştürme | Microsoft Docs
description: Yönetilmeyen diskler için bir Windows VM dönüştürmek Resource Manager dağıtım modelinde PowerShell kullanarak yönetilen diskleri
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: cynthn
ms.openlocfilehash: 26ddc1df8353a8a8b0e0c54a0c906c8d8b6b45b9
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52495333"
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Bir Windows sanal makine yönetilmeyen disklerden yönetilen disklere dönüştürme

Mevcut Windows yönetilmeyen diskler kullanan sanal makineleri (VM'ler) varsa, VM'lerin üzerinden yönetilen diskleri kullanma dönüştürebilirsiniz [Azure yönetilen diskler](managed-disks-overview.md) hizmeti. Bu işlem, hem işletim sistemi diski hem de bağlı veri diskleri dönüştürür.

Bu makalede, Azure PowerShell kullanarak Vm'leri dönüştürme işlemini göstermektedir. Gerekirse yüklemek veya yükseltmek bkz [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps).

## <a name="before-you-begin"></a>Başlamadan önce


* Gözden geçirme [yönetilen Diskler'e geçiş planı](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).

* Gözden geçirme [yönetilen Diskler'e geçiş hakkında SSS](faq-for-disks.md#migrate-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a>Tek Örnekli VM'ler Dönüştür
Bu bölümde, tek örnek Azure Vm'leri yönetilmeyen disklerden yönetilen disklere dönüştürme ele alınmaktadır. (Bir kullanılabilirlik kümesindeki sanal makineleriniz varsa sonraki bölüme bakın.) 

1. Kullanarak VM'yi serbest bırakın [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet'i. Aşağıdaki örnekte adlı VM serbest bırakılır `myVM` adlı kaynak grubunda `myResourceGroup`: 

  ```azurepowershell-interactive
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. Kullanarak VM'yi yönetilen disklere dönüştürme [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) cmdlet'i. Aşağıdaki işlem, işletim sistemi diski ve varsa veri diskleri dahil olmak üzere önceki VM dönüştürür ve sanal makineyi başlatır:

  ```azurepowershell-interactive
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```



## <a name="convert-vms-in-an-availability-set"></a>Vm'leri bir kullanılabilirlik kümesine dönüştürme

Dönüştürmek istediğiniz Vm'leri yönetilen diskleri olan bir kullanılabilirlik kümesindeki, ilk kullanılabilirlik kümesini bir yönetilen kullanılabilirlik kümesine dönüştürmeniz gerekir.

1. Kullanılabilirlik kümesini kullanarak dönüştürme [güncelleştirme AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) cmdlet'i. Aşağıdaki örnekte adlı kullanılabilirlik kümesi güncelleştirmeleri `myAvailabilitySet` adlı kaynak grubunda `myResourceGroup`:

  ```azurepowershell-interactive
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  Burada, kullanılabilirlik kümesi bölge yer alıyorsa yalnızca 2 yönetilen hata etki alanları var ancak yönetilmeyen hata etki alanları sayısı 3, bu komut "3 belirtilen hata etki alanı sayısı 1-2 aralığında olmalıdır." benzer bir hata gösterir Hatayı gidermek için hata etki alanı 2 güncelleştirmesi güncelleştirin ve `Sku` için `Aligned` gibi:

  ```azurepowershell-interactive
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. Serbest bırakın ve kullanılabilirlik kümesindeki Vm'leri dönüştürün. Aşağıdaki betiği kullanarak, her VM serbest bırakılır [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet'ini dönüştürür, kullanarak [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk)ve otomatik olarak uzaklıkta dönüştürme işleminin yeniden başlatır :

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

Dönüştürme sırasında bir hata varsa veya bir VM, önceki bir dönüştürme sorunları nedeniyle başarısız bir durumda ise, çalıştırın `ConvertTo-AzureRmVMManagedDisk` cmdlet'ini yeniden. Basit bir yeniden deneme durum genellikle engellemesini kaldırır.
Dönüştürmeden önce dönüştürme 409 hata kodu ile başarısız olur ya da tüm VM Uzantıları 'sağlama başarılı' durumda olduğundan emin olun.


## <a name="convert-using-the-azure-portal"></a>Azure portalını kullanarak dönüştürme

Yönetilmeyen diskler için yönetilen diskler Azure portalını kullanarak da dönüştürebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Portalda sanal makinelerinin listeden VM'yi seçin.
3. VM dikey penceresinde, seçin **diskleri** menüsünde.
4. Üst kısmındaki **diskleri** dikey penceresinde **yönetilen disklere geçirme**.
5. Bir kullanılabilirlik kümesindeki sanal makinenizin ise olacaktır uyarı üzerinde **yönetilen disklere geçirme** dikey kullanılabilirlik kümesini önce dönüştürmeniz gerekir. Uyarı kullanılabilirlik kümesini dönüştürmek için tıklayabileceği bir bağlantı olması gerekir. Kullanılabilirlik kümesi dönüştürüldükten sonra veya sanal makinenize bir kullanılabilirlik kümesine değilse tıklayın **geçirme** , diskleri yönetilen disklere geçirme işlemini başlatmak için. 

Sanal makine durdurulacak ve geçiş tamamlandıktan sonra yeniden başlatılacak.

## <a name="next-steps"></a>Sonraki adımlar

[Standart yönetilen diskler için premium Dönüştür](convert-disk-storage.md)

Kullanarak bir VM salt okunur bir kopyasını alın [anlık görüntüleri](snapshot-copy-managed-disk.md).

