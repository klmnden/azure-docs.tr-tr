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
ms.date: 02/15/2019
ms.author: juliako
ms.openlocfilehash: f4ce64599aad2b2eebbef6ca8d81acfca2a7a702
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60733903"
---
# <a name="create-an-azure-media-services-account"></a>Azure Media Services hesabı oluşturma

Şifreleme, kodlama, çözümleme, yönetme ve azure'da medya içeriği akışı başlatmak için bir Media Services hesabı oluşturmanız gerekir. Media Services hesabı, bir veya daha fazla depolama hesapları ile ilişkili olması gerekiyor.

> [!NOTE]
> Media Services hesabı ve onunla ilişkili tüm depolama hesaplarının aynı Azure aboneliğinde olması gerekir. Gecikme süreleri ve veri çıkış maliyetleri ile karşı karşıya kalmamak için mutlaka Media Services ile aynı konumda bulunan depolama hesaplarını kullanmanız önerilir.

Bu makalede, Azure CLI kullanarak yeni bir Azure Media Services hesabı oluşturmaya yönelik adımlar açıklanmaktadır.  

## <a name="prerequisites"></a>Önkoşullar

Etkin bir Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

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

