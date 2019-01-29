---
title: Bing Varlık Arama uç noktası
titlesuffix: Azure Cognitive Services
description: Bing varlık arama API'si uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: 4013b1791e4c725469d3f180ae0fdecc9ae2ebba
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55167062"
---
# <a name="entity-search-endpoint"></a>Varlık arama uç noktası
**Varlık arama API'si** bir sorguyu temel alan Web varlık döndüren bir uç nokta içerir.

## <a name="endpoint"></a>Uç Nokta
Varlık alma sonuçlarını kullanarak **Bing API**, gönderme bir `GET` aşağıdaki uç noktayı isteği. Daha fazla özellikleri tanımlamak için başlık ve URL parametrelerini kullanın.

**Uç noktası:** Tarafından tanımlanan kullanıcı arama sorgusu için uygun olan varlıklar döndürüyor `?q=""`.
```
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

Üstbilgileri, parametreleri, Pazar kodları, yanıt nesneleri, hata hakkındaki ayrıntılar için vb., bkz: [Bing varlık arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) başvuru.

## <a name="response-json"></a>Response JSON
Bir varlık arama isteğine yanıt JSON nesneleri sonuçlarını içerir. Sonuçları örnekleri için bkz: [başlama](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start).

## <a name="next-steps"></a>Sonraki adımlar
**Bing** türüne göre sonuçları arama eylemleri API'lerini desteklemektedir. Tüm arama uç noktaları sonuçlar JSON yanıtı nesneler olarak döndürür.  Tüm uç noktalar, belirli bir dil ve/veya konum boylam, enlem ve arama RADIUS döndüren sorgular destekler.

Her bir uç noktası tarafından desteklenen parametreleri hakkında tam bilgi için her türü için başvuru sayfalarına bakın.
Varlık arama API'si kullanan örnekler için [başlama](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start) ve [yeniden boyutlandırma ve küçük resim görüntüleri kırpma](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/resize-and-crop-thumbnails).
