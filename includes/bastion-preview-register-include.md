---
title: include dosyası
description: include dosyası
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 05/28/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d05d425cc9bfb206207801f15a25e17d60dc0aaf
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191752"
---
1. Azure hesabınızda oturum açmış ve bu Önizleme için eklemek istediğiniz aboneliği kullanarak emin olun. Kaydetmek için aşağıdaki örneği kullanın:

    ```azurepowershell-interactive
    Register-AzureRmProviderFeature -FeatureName AllowBastionHost -ProviderNamespace Microsoft.Network
    ```
2.  Aboneliğiniz ile bir kez daha yeniden *Microsoft.Network* sağlayıcı ad alanı.

    ```azurepowershell-interactive
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ````
3. Doğrulamak için aşağıdaki komutu kullanın *AllowBastionHost* özellik, aboneliğiniz ile kayıtlı:

    ```azurepowershell-interactive
    Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network
    ````

    Bunu tamamlamak kayıt birkaç dakika sürebilir.