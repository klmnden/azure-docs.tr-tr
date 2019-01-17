---
title: Azure CLI ile - Azure Media Services hesabı oluşturma | Microsoft Docs
description: Azure Media Services hesabı oluşturmak için bu hızlı başlangıç adımlarını izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 01/15/2019
ms.author: juliako
ms.openlocfilehash: 9c026eb9a74cbba2ff188a3f08be625043ede474
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54352062"
---
# <a name="create-an-azure-media-services-account"></a>Azure Media Services hesabı oluşturma

Şifreleme, kodlama, çözümleme, yönetme ve azure'da medya içeriği akışı başlatmak için bir Media Services hesabı oluşturmanız gerekir. Zaman bir Media Services hesabı oluşturma, oluşturabilir, ilişkili bir depolama hesabı (veya var olanı kullanın).  

Media Services hesabı ve ilişkili depolama hesabı aynı veri merkezinde ve aynı kaynak grubunun parçası olması gerekir.

Bu makalede, Azure CLI kullanarak yeni bir Azure Media Services hesabı oluşturmaya yönelik adımlar açıklanmaktadır.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- Etkin bir Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- Yükleyin ve bu makalede Azure CLI 2.0 veya sonraki bir sürüm gerektirir, CLI'yı yerel olarak kullanın. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

    Şu anda tüm [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) komutlar Azure Cloud Shell içinde çalışır. CLI'yi yerel olarak kullanmak için önerilir.

## <a name="set-the-azure-subscription"></a>Azure aboneliğini ayarlama

Aşağıdaki komutta, Media Services hesabı için kullanmak istediğiniz Azure abonelik kimliğini sağlayın. [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)’e giderek erişimine sahip olduğunuz aboneliklerin listesini görebilirsiniz.

```azurecli
az account set --subscription mySubscriptionId
```
 
[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]
 
## <a name="next-steps"></a>Sonraki adımlar

[Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)

## <a name="see-also"></a>Ayrıca bkz.

[Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)

