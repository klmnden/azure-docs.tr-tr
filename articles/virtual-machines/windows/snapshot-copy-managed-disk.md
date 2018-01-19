---
title: "VHD bir anlık görüntü oluşturma | Microsoft Docs"
description: "Bir Azure VM geri yukarı veya sorunlarını gidermek için kullanmak üzere bir kopyasını oluşturmayı öğrenin."
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/09/2017
ms.author: cynthn
ms.openlocfilehash: 10b5eb0062e4a029b0f233ee8af17d590d59c8d4
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-snapshot"></a>Bir anlık görüntü oluşturma

Yedekleme veya VM sorun giderme için VHD sorunları bir işletim sistemi veya veri diski bir anlık görüntüsünü. Bir VHD tam, salt okunur bir kopyasını bir anlık görüntüdür. 

## <a name="use-azure-portal-to-take-a-snapshot"></a>Bir anlık görüntüsünü için Azure portalını kullanma 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol üst başlayarak, tıklatın **yeni** arayın ve **anlık görüntü**.
3. Anlık görüntü dikey penceresinde tıklayın **oluşturma**.
4. Girin bir **adı** anlık görüntü için.
5. Varolan [Kaynak grubunu](../../azure-resource-manager/resource-group-overview.md#resource-groups) seçin veya yenisi için adı yazın. 
6. Azure veri merkezi konum seçin.  
7. İçin **kaynak disk**, anlık görüntü için yönetilen diski seçin.
8. Seçin **hesap türü** anlık görüntü deposu için kullanılacak. Öneririz **Standard_LRS** yüksek performanslı bir diskte depolanan gerekmedikçe.
9. **Oluştur**’a tıklayın.

## <a name="use-powershell-to-take-a-snapshot"></a>Bir anlık görüntüsünü için PowerShell kullanma
Aşağıdaki adımlar VHD diskin kopyalanması, anlık görüntü yapılandırmaları oluşturma ve kullanarak bir disk anlık görüntüsünü alma gösterir [yeni AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet'i. 

Yüklü AzureRM.Compute PowerShell modülü en son sürümüne sahip olduğunuzdan emin olun. Yüklemek için aşağıdaki komutu çalıştırın.

```
Install-Module AzureRM.Compute -MinimumVersion 2.6.0
```
Daha fazla bilgi için bkz: [Azure PowerShell sürüm](/powershell/azure/overview).


1. Bazı parametreler ayarlayın. 

 ```azurepowershell-interactive
$resourceGroupName = 'myResourceGroup' 
$location = 'eastus' 
$dataDiskName = 'myDisk' 
$snapshotName = 'mySnapshot'  
```

2. Kopyalanacak VHD disk alın.

 ```azurepowershell-interactive
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName 
```
3. Anlık görüntü yapılandırmaları oluşturun. 

 ```azurepowershell-interactive
$snapshot =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location 
```
4. Anlık görüntü alın.

 ```azurepowershell-interactive
New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName 
```
Yönetilen bir Disk oluşturmak ve yüksek performanslı olması gereken bir VM eklemek için anlık görüntü kullanmayı planlıyorsanız, parametresini kullanın `-AccountType Premium_LRS` yeni AzureRmSnapshot komutu. Parametre anlık görüntü oluşturur, böylece yönetilen bir Premium Disk depolanır. Premium yönetilen diskleri standart daha pahalıdır. Bu nedenle bu parametrenin kullanmadan önce Premium gerçekten ihtiyacınız emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Bir sanal makine bir anlık görüntüden bir anlık görüntüden yönetilen bir disk oluşturarak ve ardından yeni yönetilen işletim sistemi diski olarak diskindeki oluşturun. Daha fazla bilgi için bkz: [bir anlık görüntüden bir VM oluşturma](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) örnek.
