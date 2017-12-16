---
title: "Azure üzerinde bir Windows VM kaynak taşıma | Microsoft Docs"
description: "Resource Manager dağıtım modelinde başka bir Azure abonelik veya kaynak grubu için bir Windows VM taşıyın."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: cynthn
ms.openlocfilehash: f4b739fd34cc0c85d47b97b7b42a70eb7f5f5ac7
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Bir Windows VM başka bir Azure abonelik veya kaynak grubuna taşıma
Bu makalede Windows VM kaynak grupları veya abonelikler arasında taşıma konusunda size yol göstermektedir. Abonelikler arasında taşıma kişisel bir abonelikte ilk olarak bir VM oluşturduysanız, kullanışlı ve çalışmaya devam etmek için şirketinizin aboneliği taşımak şimdi istiyorsunuz.

> [!IMPORTANT]
>Şu anda yönetilen diskleri taşınamıyor. 
>
>Yeni kaynak kimlikleri taşıma bir parçası olarak oluşturulur. VM taşındıktan sonra yeni kaynak kimlikleri kullanmak için komut dosyaları ve araçları güncelleştirmeniz gerekir. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>Bir VM taşımak için PowerShell kullanın

Bir sanal makineyi başka bir kaynak grubuna taşımak için ayrıca tüm bağımlı kaynakları taşıdığınızdan emin olmanız gerekir. Taşıma AzureRMResource cmdlet'ini kullanmak için her kaynakların ResourceId gerekir. ResourceId's kullanarak listesini alabilirsiniz [Bul AzureRMResource](/powershell/module/azurerm.resources/find-azurermresource) cmdlet'i.

```azurepowershell-interactive
 Find-AzureRMResource -ResourceGroupNameContains <sourceResourceGroupName> | Format-table -Property ResourceId 
```

Bir VM taşımak için şu birden fazla kaynak taşımanız gerekir. Biz ResourceId virgülle ayrılmış listesini oluşturmak ve olarak geçirmek için Bul AzureRMResource çıkışını kullanabilirsiniz [taşıma AzureRMResource](/powershell/module/azurerm.resources/move-azurermresource) hedefe taşımak için. 

```azurepowershell-interactive

Move-AzureRmResource -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```
    
Farklı aboneliğe kaynakları taşımak için dahil **- DestinationSubscriptionId** parametresi. 

```azurepowershell-interactive
Move-AzureRmResource -DestinationSubscriptionId "<myDestinationSubscriptionID>" `
    -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```


Belirtilen kaynaklar taşımak istediğiniz onaylamanız istenir. 

## <a name="next-steps"></a>Sonraki adımlar
Birçok farklı türdeki kaynakların kaynak grupları ve abonelikler arasında taşıyabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../../resource-group-move-resources.md).    

