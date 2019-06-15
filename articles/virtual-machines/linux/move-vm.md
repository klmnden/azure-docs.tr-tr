---
title: Azure'da bir Linux VM'yi taşıma | Microsoft Docs
description: Bir Linux VM, Resource Manager dağıtım modelinde başka bir Azure abonelik veya kaynak grubuna taşıyın.
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
ms.date: 09/12/2018
ms.author: cynthn
ms.openlocfilehash: d2d3f36c9b4ee0557f9e060bec762877a94ea637
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61473946"
---
# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Bir Linux VM başka bir abonelik veya kaynak grubuna taşıma
Bu makalede, bir Linux sanal makinesini (VM), kaynak grubu veya abonelik arasında taşıma konusunda size yol gösterir. VM'yi abonelikler arasında taşıma kişisel bir abonelikte bir VM oluşturduysanız, kullanışlı ve şirketinizin aboneliği taşımak şimdi istiyorsunuz.

> [!IMPORTANT]
>Şu anda Azure yönetilen diskler taşıyamazsınız. 
>
>Yeni kaynak kimliklerini taşımanın bir parçası oluşturulur. VM taşındıktan sonra araçları ve betikleri, yeni kaynak kimliğini kullanacak şekilde güncelleştirmeniz gerekir. 
> 
> 

## <a name="use-the-azure-cli-to-move-a-vm"></a>Bir VM'yi taşıma için Azure CLI kullanma


Azure CLI kullanarak sanal makinenize taşımadan önce kaynak ve hedef abonelikler aynı kiracıda kayıtlı emin olmanız gerekir. Her iki aboneliğin aynı Kiracı Kimliğine sahip denetlemek için kullanmak [az hesabı show](/cli/azure/account).

```azurecli-interactive
az account show --subscription mySourceSubscription --query tenantId
az account show --subscription myDestinationSubscription --query tenantId
```
Kaynak ve hedef abonelikler için Kiracı kimlikleri aynı değilse, başvurmalıdır [Destek](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) kaynakları yeni bir kiracıya taşımak için.

Bir sanal makine başarıyla taşımak için VM'yi ve tüm destekleyici kaynakları taşımak gerekir. Kullanım [az kaynak listesi](/cli/azure/resource) tüm kaynaklar bir kaynak grubu ve kimliklerini listelemek için komutu. Bu komutun çıktısı, kopyalayın ve daha sonra komutlarına kimlikleri yapıştırın, bir dosyaya kanal oluşturarak için yardımcı olur.

```azurecli-interactive
az resource list --resource-group "mySourceResourceGroup" --query "[].{Id:id}" --output table
```

Bir sanal makine ve kaynaklarının başka bir kaynak grubuna taşımak için kullanın [az kaynak taşıma](/cli/azure/resource). Aşağıdaki örnek, bir sanal Makineyi ve gerektirdiği en yaygın kaynakları taşımak gösterilmektedir. Kullanım **-kimlikleri** parametre ve bir virgülle ayrılmış listesi (boşluksuz) taşımak için kaynaklar için kimlikleri geçirin.

```azurecli-interactive
vm=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
nic=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkInterfaces/myNIC
nsg=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG
pip=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIPAddress
vnet=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/virtualNetworks/myVNet
diag=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mydiagnosticstorageaccount
storage=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccountname    

az resource move \
    --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag \
    --destination-group "myDestinationResourceGroup"
```

Sanal makine ve kaynaklarının farklı bir aboneliğe taşımak istiyorsanız, ekleme **--hedef Subscriptionıd** hedef aboneliği belirtmek için parametre.

İstediğiniz belirli kaynakları taşıma, girin onaylamak için sorulduğunda **Y** onaylamak için.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Sonraki adımlar
Birçok farklı türdeki kaynakların kaynak grupları ve abonelikler arasında taşıyabilirsiniz. Daha fazla bilgi için [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../../resource-group-move-resources.md).    

