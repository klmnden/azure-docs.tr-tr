---
title: Video Indexer - kullanılabildiği bölgeler Azure
titlesuffix: Azure Media Services
description: Bu makalede, Video Indexer kullanılabildiği Azure bölgeleri hakkında konuşuyor.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: 404aaf91c0cb30df0a83353ef7397987ec3f8e80
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65799426"
---
# <a name="azure-regions-in-which-video-indexer-exists"></a>Video Indexer bulunduğu azure bölgeleri

Video Indexer API'ları içeren bir **konumu** olduğu çağrı yönlendirilir bir Azure bölgesine ayarlamalısınız parametresi. Bu olmalıdır bir [Video Indexer kullanılabildiği Azure bölgesi](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).

## <a name="locations"></a>Konumlar

**Konumu** parametre değeri olarak bir Azure bölgesine kod adı verilen gerekir. Video Indexer'ı önizleme modunda kullanıyorsanız, sizi koymalısınız *"Deneme"* değeri. Aksi takdirde, hesabınızın bulunduğu ve aramanız için yönlendirileceğini Azure bölgesinin kod adını almak için aşağıdaki satırı çalıştırabilirsiniz [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest):

```bash
az account list-locations
```

Bir kez yukarıda gösterilen tüm Azure bölgelerinde listesini alma satırı çalıştırın. Sahip Azure bölgesine gidin *displayName* aradığınız ve kullanmak, *adı* değerini **konumu** parametresi.

Örneğin, Azure bölgesinde Batı ABD 2 (aşağıda gösterilen), "westus2" için kullanacağınız **konumu** parametresi.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/35c2594a-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="next-steps"></a>Sonraki adımlar

- [Dil modeli API'lerini kullanarak özelleştirme](customize-language-model-with-api.md)
- [Markaları modeli API'lerini kullanarak özelleştirme](customize-brands-model-with-api.md)
- [Kişi modeli API'lerini kullanarak özelleştirme](customize-person-model-with-api.md)
