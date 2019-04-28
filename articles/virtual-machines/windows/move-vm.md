---
title: Azure'da bir Windows VM kaynak taşıma | Microsoft Docs
description: Bir Windows VM, Resource Manager dağıtım modelinde başka bir Azure abonelik veya kaynak grubuna taşıyın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: cynthn
ms.openlocfilehash: ede2092be4e4eaf201e15307a7d9934ea267ae37
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61481522"
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Bir Windows VM için başka bir Azure abonelik veya kaynak grubunu Taşı
Bu makalede, bir Windows sanal makinesi (VM), kaynak grubu veya abonelik arasında taşıma konusunda size yol gösterir. Abonelikler arasında taşıma, kişisel bir abonelikte ilk olarak bir VM oluşturduysanız, kullanışlı ve çalışmaya devam etmek için şirketinizin aboneliğine taşımak şimdi istiyorsunuz.

> [!IMPORTANT]
>Yeni kaynak kimliklerini taşımanın bir parçası oluşturulur. VM taşındıktan sonra araçları ve betikleri, yeni kaynak kimliğini kullanacak şekilde güncelleştirmeniz gerekir. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>Bir VM'yi taşıma için Powershell kullanma

Bir sanal makineyi başka bir kaynak grubuna taşımak için ayrıca tüm bağımlı kaynakları taşıdığınızdan emin olmanız gerekir. Bu kaynakların her biri kaynak kimliği listesini almak için kullanın [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) cmdlet'i.

```azurepowershell-interactive
 Get-AzResource -ResourceGroupName <sourceResourceGroupName> | Format-table -Property ResourceId 
```

Önceki komutun çıktısındaki kaynak kimliklerinin virgülle ayrılmış bir listesi olarak kullanabileceğiniz [taşıma AzResource](https://docs.microsoft.com/powershell/module/az.resources/move-azresource) her kaynak hedef konuma taşımak için. 

```azurepowershell-interactive
Move-AzResource -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```
    
Kaynakları farklı aboneliğe taşımak dahil **- DestinationSubscriptionId** parametresi. 

```azurepowershell-interactive
Move-AzResource -DestinationSubscriptionId "<myDestinationSubscriptionID>" `
    -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```


İstediğiniz belirli kaynakları taşıma, girin onaylamak için sorulduğunda **Y** onaylamak için.

## <a name="next-steps"></a>Sonraki adımlar
Birçok farklı türdeki kaynakların kaynak grupları ve abonelikler arasında taşıyabilirsiniz. Daha fazla bilgi için [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../../resource-group-move-resources.md).    

