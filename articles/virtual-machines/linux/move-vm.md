---
title: Azure'da bir Linux VM Taşı | Microsoft Docs
description: Resource Manager dağıtım modelinde başka bir Azure abonelik veya kaynak grubu için bir Linux VM taşıyın.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: d635f0a5-4458-4b95-a5f8-eed4f41eb4d4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 12/14/2017
ms.author: cynthn
ms.openlocfilehash: a4a7dd5541fe298675232ffa803f749e71f6a03f
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Bir Linux VM başka bir abonelik veya kaynak grubuna taşıma
Bu makalede, bir Linux VM kaynak grupları veya abonelikler arasında taşıma hakkında adım adım anlatılmaktadır. Bir VM abonelikler arasında taşıma kişisel bir abonelikte VM oluşturduysanız kullanışlı ve şirketinizin aboneliği taşımak şimdi istiyorsunuz.

> [!IMPORTANT]
>Şu anda yönetilen diskleri taşınamıyor. 
>
>Yeni kaynak kimlikleri taşıma bir parçası olarak oluşturulur. VM taşındıktan sonra yeni kaynak kimlikleri kullanmak için komut dosyaları ve araçları güncelleştirmeniz gerekir. 
> 
> 

## <a name="use-the-azure-cli-to-move-a-vm"></a>Bir VM taşımak için Azure CLI kullanın


CLI kullanarak VM taşımadan önce kaynak ve hedef abonelikler aynı Kiracı içinde var olduğundan emin olmanız gerekir. Her iki aboneliğin aynı Kiracı kimliği olduğunu denetlemek için kullanın [az hesabı Göster](/cli/azure/account#az_account_show).

```azurecli-interactive
az account show --subscription mySourceSubscription --query tenantId
az account show --subscription myDestinationSubscription --query tenantId
```
Kaynak ve hedef abonelikler için Kiracı kimlikleri aynı değilse, başvurmalıdır [Destek](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) kaynakları için yeni bir kiracı taşımak için.

Başarıyla bir VM taşımak için VM ve tüm destekleyici kaynakları taşımanız gerekir. Kullanım [az kaynak listesi](/cli/azure/resource#az_resource_list) tüm kaynakları bir kaynak grubu ve kimlikleri listelemek için komutu. Böylece kopyalayıp kimlikleri Yapıştır sonraki komutlarını bu komutun çıktısı bir dosyaya kanal yardımcı olabilir.

```azurecli-interactive
az resource list --resource-group "mySourceResourceGroup" --query "[].{Id:id}" --output table
```

Bir VM ve kaynaklarını başka bir kaynak grubuna taşımak için kullanın [az kaynak taşıma](/cli/azure/resource#az_resource_move). Aşağıdaki örnek, bir VM ve gerektirdiği en yaygın kaynaklarını taşımak gösterilmiştir. Kullanım **-kimlikleri** parametre ve virgülle ayrılmış listesini (boşluksuz) taşımak için kaynaklar için kimlikleri geçirin.

```azurecli-interactive
vm=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
nic=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkInterfaces/myNIC
nsg=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG
pip=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIPAddress
vnet=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/virtualNetworks/myVNet
diag=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mydiagnosticstorageaccount
storage=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageacountname    

az resource move \
    --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag \
    --destination-group "myDestinationResourceGroup"
```

Farklı bir aboneliğe VM ve kaynaklarını taşımak istiyorsanız, ekleme **--hedef-Subscriptionıd** parametresi hedef abonelik belirtin.

Belirtilen kaynak taşımak istediğiniz onaylamanız istenirse. Tür **Y** kaynakları taşımak istediğinizi onaylamak için.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Sonraki adımlar
Birçok farklı türdeki kaynakların kaynak grupları ve abonelikler arasında taşıyabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../../resource-group-move-resources.md).    

