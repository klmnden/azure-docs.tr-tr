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
ms.date: 12/06/2017
ms.author: cynthn
ms.openlocfilehash: 168ba57399b2649af29820f7321dd0151618346e
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436489"
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Bir Windows VM için başka bir Azure abonelik veya kaynak grubunu Taşı
Bu makalede, bir Windows VM kaynak grubu veya abonelik arasında taşıma konusunda size yol gösterir. Abonelikler arasında taşıma, kişisel bir abonelikte ilk olarak bir VM oluşturduysanız, kullanışlı ve çalışmaya devam etmek için şirketinizin aboneliğine taşımak şimdi istiyorsunuz.

> [!IMPORTANT]
>Şu anda yönetilen diskleri taşıyamazsınız. 
>
>Yeni kaynak kimliklerini taşımanın bir parçası oluşturulur. Sanal makine taşındığında, araçları ve betikleri, yeni kaynak kimliğini kullanacak şekilde güncelleştirmeniz gerekir. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>Bir VM'yi taşıma için Powershell kullanma

Bir sanal makineyi başka bir kaynak grubuna taşımak için ayrıca tüm bağımlı kaynakları taşıdığınızdan emin olmanız gerekir. Move-AzureRMResource cmdlet'ini kullanmak için her kaynak ResourceId gerekir. ResourceId'ın kullanarak bir listesini alabilirsiniz [Get-AzureRMResource](/powershell/module/azurerm.resources/get-azurermresource) cmdlet'i.

```azurepowershell-interactive
 Get-AzureRMResource -ResourceGroupName <sourceResourceGroupName> | Format-table -Property ResourceId 
```

Bir VM'yi taşıma için birden fazla kaynak taşımak ihtiyacımız var. Get-AzureRMResource çıktısını Resourceıds virgülle ayrılmış bir listesini oluşturun ve geçirebilirsiniz kullanabiliriz [Move-AzureRMResource](/powershell/module/azurerm.resources/move-azurermresource) hedef konuma taşınır. 

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


Belirtilen kaynakları taşımak isteyip istemediğiniz sorulur. 

## <a name="next-steps"></a>Sonraki adımlar
Birçok farklı türdeki kaynakların kaynak grupları ve abonelikler arasında taşıyabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../../resource-group-move-resources.md).    

