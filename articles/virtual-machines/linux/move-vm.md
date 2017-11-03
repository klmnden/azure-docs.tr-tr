---
title: "Azure'da bir Linux VM Taşı | Microsoft Docs"
description: "Resource Manager dağıtım modelinde başka bir Azure abonelik veya kaynak grubu için bir Linux VM taşıyın."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d635f0a5-4458-4b95-a5f8-eed4f41eb4d4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 4695a9c934f97f2b2d448c4990e7ad5533e38e9f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
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
Başarıyla bir VM taşımak için VM ve tüm destekleyici kaynakları taşımanız gerekir. Kullanım **azure Grup Göster** tüm kaynakları bir kaynak grubu ve kimlikleri listelemek için komutu. Böylece kopyalayıp kimlikleri Yapıştır sonraki komutlarını bu komutun çıktısı bir dosyaya kanal yardımcı olabilir.

    azure group show <resourceGroupName>

Bir VM ve kaynaklarını başka bir kaynak grubuna taşımak için kullanın **azure kaynak taşıma** CLI komutu. Aşağıdaki örnek, bir VM ve gerektirdiği en yaygın kaynaklarını taşımak gösterilmiştir. Kullanırız **-i** parametre ve virgülle ayrılmış listesini (boşluksuz) taşımak için kaynaklar için kimlikleri geçirin.

    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      

    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"

Farklı bir aboneliğe VM ve kaynaklarını taşımak istiyorsanız, ekleme **--hedef-Subscriptionıd &#60; destinationSubscriptionID &#62;** parametresi hedef abonelik belirtin.

Bir Windows bilgisayarda komut isteminden çalışıyorsanız, eklemek gereken bir  **$**  bunları bildirirken değişken adlarının önünde. Bu Linux gerekli değildir.

Belirtilen kaynak taşımak istediğiniz onaylamanız istenir. Tür **Y** kaynakları taşımak istediğinizi onaylamak için.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Sonraki adımlar
Birçok farklı türdeki kaynakların kaynak grupları ve abonelikler arasında taşıyabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../../resource-group-move-resources.md).    

