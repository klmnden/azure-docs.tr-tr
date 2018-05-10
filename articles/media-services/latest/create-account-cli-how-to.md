---
title: CLI 2.0 ile Azure Media Services hesabı oluşturma | Microsoft Docs
description: Azure Media Services hesabı oluşturmak için bu hızlı başlangıç adımları izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: cflower
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 03/27/2018
ms.author: juliako
ms.openlocfilehash: a9660ac61bab9f8b9eb9563aab4cc584786b25ae
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="create-an-azure-media-services-account"></a>Azure Media Services hesabı oluşturma

Şifreleme, kodlama, çözümleme, yönetme ve Azure media içeriğinde streaming başlatmak için Media Services hesabı oluşturmanız gerekir. Bir Media Services hesabı oluşturduğunuzda Media Services hesabı ile aynı coğrafi bölgede ilişkili bir depolama hesabı da oluşturursunuz (ya da var olanı kullanırsınız).

Bu konuda, CLI 2.0 kullanarak yeni bir Azure Media Services hesabı oluşturma adımları açıklanmaktadır.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Oturum [Azure portal](http://portal.azure.com) ve başlatma **CloudShell** CLI komutları, sonraki adımlarda gösterildiği gibi yürütmek için.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="set-the-azure-subscription"></a>Azure aboneliği ayarlama

Aşağıdaki komutta, Media Services hesabı için kullanmak istediğiniz Azure abonelik kimliği sağlayın. Giderek erişiminiz Aboneliklerin listesini görebilirsiniz [abonelikleri](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

```azurecli-interactive
az account set --subscription mySubscriptionId
```
 
[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]
 
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir dosya akışı](stream-files-dotnet-quickstart.md)
