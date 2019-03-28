---
title: Azure'da bir Windows sanal makinesi yeniden boyutlandırmak için PowerShell kullanma | Microsoft Docs
description: Azure Powershell kullanarak Resource Manager dağıtım modelinde oluşturulan bir Windows sanal makine yeniden boyutlandırın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 057ff274-6dad-415e-891c-58f8eea9ed78
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: cynthn
ms.openlocfilehash: f54ff738199d433308a8eaba6a643861c57b4abb
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540695"
---
# <a name="resize-a-windows-vm"></a>Bir Windows VM yeniden boyutlandırma

Bu makalede, bir VM'yi farklı bir taşıma işlemini göstermektedir [VM boyutu](sizes.md) Azure Powershell kullanarak.

Bir sanal makine (VM) oluşturduktan sonra VM boyutunu değiştirerek artırıp azaltın VM ölçeklendirebilirsiniz. Bazı durumlarda, ilk VM ayırması gerekir. Yeni boyut şu anda sanal Makineyi barındıran donanım kümesinde kullanılabilir değilse, bu durum oluşabilir.

Premium depolama, sanal Makinenizin kullanıyorsa, seçtiğinizden emin olun. bir **s** boyutu Premium depolama desteği almak için bir sürümü. Örneğin, Standard_E4 tercih**s**_v3 işler için standart_e4_v3 yerine.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Bir kullanılabilirlik kümesindeki Windows VM'yi yeniden boyutlandırma

Bazı değişkenleri ayarlayın. Değerleri kendi bilgilerinizle değiştirin.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Sanal Makinenin barındırıldığı donanım kümesinde kullanılabilir VM boyutlarını listeler. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

İstediğiniz boyuta listeleniyorsa, VM'yi yeniden boyutlandırma için aşağıdaki komutları çalıştırın. İstenen boyut listede yoksa, 3. adıma geçin.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMsize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

İstediğiniz boyuta, VM ayırması için aşağıdaki komutları çalıştırın listede yoksa, yeniden boyutlandırabilir ve VM'yi yeniden başlatın. Değiştirin  **\<newVMsize >** istediğiniz boyuta sahip.
   
```powershell
Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
```

> [!WARNING]
> VM serbest bırakılıyor, sanal Makineye atanan dinamik IP adreslerini serbest bırakır. İşletim sistemi ve veri diskleri etkilenmez. 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Bir kullanılabilirlik kümesinde Windows VM'yi yeniden boyutlandırma

Bir kullanılabilirlik kümesindeki bir sanal makine için yeni boyut şu anda VM'yi barındıran donanım kümesinde kullanılabilir durumda değilse, kullanılabilirlik kümesindeki tüm sanal makineler, sanal Makineyi yeniden boyutlandırmak için serbest bırakılması gerekir. Diğer sanal makinelerin boyutunu kullanılabilirlik sonra bir sanal makine yeniden boyutlandırıldı kümesinde güncelleştirme gerekebilir. Bir kullanılabilirlik kümesindeki bir VM'nin yeniden boyutlandırmak için aşağıdaki adımları gerçekleştirin.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Sanal Makinenin barındırıldığı donanım kümesinde kullanılabilir VM boyutlarını listeler. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

İstenen boyut listeleniyorsa, VM'yi yeniden boyutlandırma için aşağıdaki komutları çalıştırın. Bu listede yoksa, sonraki bölüme gidin.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName 
$vm.HardwareProfile.VmSize = "<newVmSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```
    
İstediğiniz boyuta listede yoksa kullanılabilirlik kümesindeki tüm sanal makineler serbest bırakın, Vm'leri yeniden boyutlandırma ve yeniden başlatmak için aşağıdaki adımlarla devam edin.

Kullanılabilirlik kümesindeki tüm sanal makineler durdurun.
   
```powershell
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
    } 
```

Yeniden boyutlandırma ve kullanılabilirlik kümesindeki Vm'leri yeniden başlatma.
   
```powershell
$newSize = "<newVmSize>"
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
  foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    $vm = Get-AzVM -ResourceGroupName $resourceGroup -Name $vmName
    $vm.HardwareProfile.VmSize = $newSize
    Update-AzVM -ResourceGroupName $resourceGroup -VM $vm
    Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
    }
```

## <a name="next-steps"></a>Sonraki adımlar

Ek ölçeklenebilirlik için birden fazla VM örneği çalıştırın ve ölçeği genişletme. Daha fazla bilgi için [bir sanal makine ölçek kümesi'nde Windows makineleri otomatik ölçeklendirme](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).

