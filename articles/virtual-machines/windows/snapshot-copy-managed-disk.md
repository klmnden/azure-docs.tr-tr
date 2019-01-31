---
title: Azure'da bir VHD anlık görüntüsünü oluşturma | Microsoft Docs
description: Bir kopyasını yukarı veya ilgili sorunları gidermeye yönelik bir yedekleme kullanmak üzere bir Azure VM oluşturmayı öğrenin.
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: 059c90df16a46c2575635d7c729ec563ef1130c6
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55459732"
---
# <a name="create-a-snapshot"></a>Anlık görüntü oluşturma

Bir anlık görüntü sanal sabit disk (VHD), tam, salt okunur bir kopyasıdır. Bir yedek olarak kullanmak ya da sanal makine (VM) sorunlarını gidermek için VHD bir işletim sistemi veya veri diskinin anlık görüntüsünü alabilir.

Yeni bir VM oluşturmak için anlık görüntü kullanmak için kullanacaksanız, devam eden tüm işlemleri kullanıma temizlemek için bir anlık görüntüsünü almadan önce VM'yi temiz kapatma öneririz.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol menüden **kaynak Oluştur**, arayın ve seçin **anlık görüntü**.
3. İçinde **anlık görüntü** penceresinde **Oluştur**. **Oluşturma anlık görüntüsü** penceresi görüntülenir.
4. Girin bir **adı** anlık görüntü.
5. Mevcut bir seçin [kaynak grubu](../../azure-resource-manager/resource-group-overview.md#resource-groups) veya yeni bir ad girin. 
6. Azure veri merkezi **Konumu** seçin.  
7. İçin **kaynak disk**, yönetilen diskin anlık görüntüsünü seçin.
8. Seçin **hesap türü** anlık görüntü deposu için kullanılacak. Seçin **Standard_HDD**, yüksek performanslı bir diskte depolanacak anlık görüntü gerekmedikçe.
9. **Oluştur**’u seçin.

## <a name="use-powershell"></a>PowerShell kullanma

Aşağıdaki adımları kullanarak disk anlık VHD diski kopyalayın ve anlık görüntü yapılandırması oluşturma işlemini göstermektedir [yeni AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet'i. 

Başlamadan önce sürüm 5.7.0 olmalıdır AzureRM.Compute PowerShell modülünün en son sürüm olduğundan emin olun veya üzeri. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/azurerm/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız, çalıştırma [Connect-AzureRmAccount](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) Azure ile bir bağlantı oluşturmak için.

1. Bazı parametrelerini ayarla: 

 ```azurepowershell-interactive
$resourceGroupName = 'myResourceGroup' 
$location = 'eastus' 
$vmName = 'myVM'
$snapshotName = 'mySnapshot'  
```

2. VM Al:

 ```azurepowershell-interactive
$vm = get-azurermvm `
   -ResourceGroupName $resourceGroupName 
   -Name $vmName
```

3. Anlık görüntü yapılandırmasını oluşturun. Bu örnekte, işletim sistemi diskinin anlık görüntüsüdür:

 ```azurepowershell-interactive
$snapshot =  New-AzureRmSnapshotConfig 
   -SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id 
   -Location $location 
   -CreateOption copy
```
   
   > [!NOTE]
   > Bölge dayanıklı depolama, anlık görüntü depolamak istiyorsanız, desteklediği bir bölgede oluşturun [kullanılabilirlik](../../availability-zones/az-overview.md) ve `-SkuName Standard_ZRS` parametresi.   
   
4. Anlık görüntü alın:

 ```azurepowershell-interactive
New-AzureRmSnapshot 
   -Snapshot $snapshot 
   -SnapshotName $snapshotName 
   -ResourceGroupName $resourceGroupName 
```


## <a name="next-steps"></a>Sonraki adımlar

Bir sanal makinenin bir anlık görüntüden yönetilen disk oluşturma ve ardından yeni bir yönetilen diski işletim sistemi diski olarak ekleyerek bir anlık görüntüden oluşturun. Örnekte daha fazla bilgi için bkz. [PowerShell ile anlık görüntüden VM oluşturma](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json).
