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
ms.openlocfilehash: da4177fd54c0d8777f15175cea3a74a8b01c0954
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305342"
---
1. Azure hesabınızda oturum açmış ve bu Önizleme için eklemek istediğiniz aboneliği kullanarak emin olun. Kaydetmek için aşağıdaki örneği kullanın:

    ```azurepowershell-interactive
    Register-AzProviderFeature -FeatureName AllowBastionHost -ProviderNamespace Microsoft.Network
    ```
2.  Aboneliğiniz ile bir kez daha yeniden *Microsoft.Network* sağlayıcı ad alanı.

    ```azurepowershell-interactive
    Register-AzResourceProvider -ProviderNamespace Microsoft.Network
    ````
3. Doğrulamak için aşağıdaki komutu kullanın *AllowBastionHost* özellik, aboneliğiniz ile kayıtlı:

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.Network
    ````

    Bunu tamamlamak kayıt birkaç dakika sürebilir.
