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
ms.openlocfilehash: 1daf04e3f878d0748bfa0904259c7b7187481843
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45580503"
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Bir Windows VM için başka bir Azure abonelik veya kaynak grubunu Taşı
Bu makalede, bir Windows sanal makinesi (VM), kaynak grubu veya abonelik arasında taşıma konusunda size yol gösterir. Abonelikler arasında taşıma, kişisel bir abonelikte ilk olarak bir VM oluşturduysanız, kullanışlı ve çalışmaya devam etmek için şirketinizin aboneliğine taşımak şimdi istiyorsunuz.

> [!IMPORTANT]
>Şu anda Azure yönetilen diskler taşıyamazsınız. 
>
>Yeni kaynak kimliklerini taşımanın bir parçası oluşturulur. VM taşındıktan sonra araçları ve betikleri, yeni kaynak kimliğini kullanacak şekilde güncelleştirmeniz gerekir. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>Bir VM'yi taşıma için Powershell kullanma

Bir sanal makineyi başka bir kaynak grubuna taşımak için ayrıca tüm bağımlı kaynakları taşıdığınızdan emin olmanız gerekir. Bu kaynakların her biri kaynak kimliği listesini almak için kullanın [Get-AzureRMResource](/powershell/module/azurerm.resources/get-azurermresource) cmdlet'i.

```azurepowershell-interactive
 Get-AzureRMResource -ResourceGroupName <sourceResourceGroupName> | Format-table -Property ResourceId 
```

Önceki komutun çıktısındaki kaynak kimliklerinin virgülle ayrılmış bir listesi olarak kullanabileceğiniz [Move-AzureRMResource](/powershell/module/azurerm.resources/move-azurermresource) her kaynak hedef konuma taşımak için. 

```azurepowershell-interactive
Move-AzureRmResource -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```
    
Kaynakları farklı aboneliğe taşımak dahil **- DestinationSubscriptionId** parametresi. 

```azurepowershell-interactive
Move-AzureRmResource -DestinationSubscriptionId "<myDestinationSubscriptionID>" `
    -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```


İstediğiniz belirli kaynakları taşıma, girin onaylamak için sorulduğunda **Y** onaylamak için.

## <a name="next-steps"></a>Sonraki adımlar
Birçok farklı türdeki kaynakların kaynak grupları ve abonelikler arasında taşıyabilirsiniz. Daha fazla bilgi için [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../../resource-group-move-resources.md).    

