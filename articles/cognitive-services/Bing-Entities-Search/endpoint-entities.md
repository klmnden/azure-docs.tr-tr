---
title: Varlık arama endpoint | Microsoft Docs
description: Varlık arama API uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: a2557c6000445544b3b47a05d7d356ccaa9928b4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351401"
---
# <a name="entity-search-endpoint"></a>Varlık arama uç noktası
**Varlık arama API** sorgu temelli bir Web varlıklar döndürüyor bir uç nokta içerir.

## <a name="endpoint"></a>Uç Nokta
Varlık alma sonuçları kullanarak **Bing API'si**, gönderme bir `GET` şu uç nokta isteği. Üstbilgiler ve URL parametreleri daha ayrıntılı belirtimleri tanımlamak için kullanın.

**Uç noktası:** döndürür tarafından tanımlanan kullanıcının arama sorgusu için uygun olan varlıkları `?q=""`.
```
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

Üstbilgiler, parametreleri, Pazar kodları, yanıt nesneleri, hatalar, hakkındaki ayrıntılar için vs., bkz: [Bing varlık arama API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) başvuru.

## <a name="response-json"></a>Yanıt JSON
Bir varlık arama isteğine yanıt JSON nesnelerinin sonuçlarını içerir. Sonuçları bir örnekleri için bkz: [başlama](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start).

## <a name="next-steps"></a>Sonraki adımlar
**Bing** API'lerini destekleyen tipine göre sonuçları döndüren arama eylemler. Tüm arama uç noktaları sonuçları JSON yanıt nesneler olarak döndürür.  Tüm uç noktaları boylam, enlem ve arama RADIUS tarafından belirli bir dil ve/veya konum döndüren sorgular destekler.

Her bitiş noktası tarafından desteklenen parametreleri hakkında tam bilgi için her tür başvuru sayfalarına bakın.
Varlık arama'yı API kullanarak örnekler için bkz: [başlama](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start) ve [Resizing ve küçük resim görüntüleri kırpma](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/resize-and-crop-thumbnails).