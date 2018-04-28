---
title: VHD bir anlık görüntü oluşturma | Microsoft Docs
description: Bir Azure VM geri yukarı veya sorunlarını gidermek için kullanmak üzere bir kopyasını oluşturmayı öğrenin.
documentationcenter: ''
author: cynthn
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: cynthn
ms.openlocfilehash: d7315d3fb7fc156beb85271d0e5aa19ec6baa7a9
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="create-a-snapshot"></a>Bir anlık görüntü oluşturma

Yedekleme veya VM sorun giderme için VHD sorunları bir işletim sistemi veya veri diski bir anlık görüntüsünü. Bir VHD tam, salt okunur bir kopyasını bir anlık görüntüdür. 

## <a name="use-azure-portal-to-take-a-snapshot"></a>Bir anlık görüntüsünü için Azure portalını kullanma 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol üst başlayarak, tıklatın **kaynak oluşturma** arayın ve **anlık görüntü**.
3. Anlık görüntü dikey penceresinde tıklayın **oluşturma**.
4. Girin bir **adı** anlık görüntü için.
5. Varolan [Kaynak grubunu](../../azure-resource-manager/resource-group-overview.md#resource-groups) seçin veya yenisi için adı yazın. 
6. Azure veri merkezi konum seçin.  
7. İçin **kaynak disk**, anlık görüntü için yönetilen diski seçin.
8. Seçin **hesap türü** anlık görüntü deposu için kullanılacak. Öneririz **Standard_LRS** yüksek performanslı bir diskte depolanan gerekmedikçe.
9. **Oluştur**’a tıklayın.

## <a name="use-powershell-to-take-a-snapshot"></a>Bir anlık görüntüsünü için PowerShell kullanma

Aşağıdaki adımlar VHD diskin kopyalanması, anlık görüntü yapılandırmaları oluşturma ve kullanarak bir disk anlık görüntüsünü alma gösterir [yeni AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet'i. 

Başlamadan önce AzureRM.Compute PowerShell modülü en son sürümüne sahip olduğunuzdan emin olun. Bu makale Modül sürümü 5.7.0 AzureRM gerektirir veya sonraki bir sürümü. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

Bazı parametreler ayarlayın. 

 ```azurepowershell-interactive
$resourceGroupName = 'myResourceGroup' 
$location = 'eastus' 
$vmName = 'myVM'
$snapshotName = 'mySnapshot'  
```

VM Al

 ```azurepowershell-interactive
$vm = get-azurermvm `
   -ResourceGroupName $resourceGroupName `
   -Name $vmName
```

Anlık görüntü yapılandırmasını oluşturun. Bu örnekte, biz anlık görüntü için işletim sistemi diski adımıdır.

 ```azurepowershell-interactive
$snapshot =  New-AzureRmSnapshotConfig `
   -SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id `
   -Location $location `
   -CreateOption copy
```
   
> [!NOTE]
> Bölge esnek depolama alanında, anlık görüntü deposu istiyorsanız destekleyen bir bölgede oluşturmanıza gerek [kullanılabilirlik bölgeleri](../../availability-zones/az-overview.md) ve dahil `-SkuName Standard_ZRS` parametresi.   

   
Anlık görüntü alın.

```azurepowershell-interactive
New-AzureRmSnapshot `
   -Snapshot $snapshot `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName 
```




## <a name="next-steps"></a>Sonraki adımlar

Bir sanal makine bir anlık görüntüden bir anlık görüntüden yönetilen bir disk oluşturarak ve ardından yeni yönetilen işletim sistemi diski olarak diskindeki oluşturun. Daha fazla bilgi için bkz: [bir anlık görüntüden bir VM oluşturma](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) örnek.
