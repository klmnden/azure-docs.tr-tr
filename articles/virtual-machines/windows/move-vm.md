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
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 1db25a5d9ff5cb6aa2787a0cafa40cfb010e3b06
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
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
Bir sanal makineyi başka bir kaynak grubuna taşımak için ayrıca tüm bağımlı kaynakları taşıdığınızdan emin olmanız gerekir. Taşıma AzureRMResource cmdlet'ini kullanmak için kaynak adı ve kaynak türü gerekir. Bul AzureRMResource cmdlet'i her ikisini de alabilirsiniz.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"


Bir VM taşımak için şu birden fazla kaynak taşımanız gerekir. Yalnızca her kaynak için ayrı değişkeni oluşturun ve bunları listeleyin. Bu örnek, bir VM için temel kaynakların çoğunu içerir, ancak gerektiğinde daha fazla ekleyebilirsiniz.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"

    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"

    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

Farklı aboneliğe kaynakları taşımak için dahil **- DestinationSubscriptionId** parametresi. 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



Belirtilen kaynaklar taşımak istediğiniz onaylamanız istenir. Tür **Y** kaynakları taşımak istediğinizi onaylamak için.

## <a name="next-steps"></a>Sonraki adımlar
Birçok farklı türdeki kaynakların kaynak grupları ve abonelikler arasında taşıyabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../../resource-group-move-resources.md).    

