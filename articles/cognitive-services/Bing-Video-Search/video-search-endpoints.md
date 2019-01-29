---
title: Video arama uç noktaları - Bing Video arama
titlesuffix: Azure Cognitive Services
description: Bing Video arama API'si uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 12/04/2017
ms.author: rosh
ms.openlocfilehash: db65f0ec857b2d69a2d36a22a1d9e3676af91683
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190658"
---
# <a name="video-search-endpoints"></a>Video arama uç noktaları
**Video arama API'si** üç uç noktaları içerir.  Uç nokta 1 videoları bir sorguyu temel alan Web döndürür. Uç nokta 2 döndürür dayalı bir video öngörüleri `modules` URL parametresi.  Uç nokta 3 popüler resimlerden döndürür.

## <a name="endpoints"></a>Uç Noktalar
Bing API'si kullanarak video sonuçları elde etmek için Gönder bir `GET` aşağıdaki uç noktalarından birinin isteği. Daha fazla özellikleri tanımlamak için başlık ve URL parametrelerini kullanın.

**Bitiş noktası 1:** Kullanıcı arama sorgu tarafından tanımlanan ilgili videoları döndürür `?q=""`.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search
```

**Uç nokta 2:** İlgili videolar gibi bir video hakkında bilgiler döndürür. Dahil `modules` [sorgu parametresi](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query-parameters), ınsights istemek için virgülle ayrılmış listesi verilmiştir.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/details
```

**3 uç noktası:** Arama istekleri, başkaları tarafından yapılan mu döndürür videoları temel. Videoları farklı kategorilerde gibi önemli kişiler veya olayları göre ayrılır.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending
```

Üstbilgileri, parametreleri, Pazar kodları, yanıt nesneleri, hata hakkındaki ayrıntılar için vb., bkz: [Bing Video arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) başvuru.
## <a name="response-json"></a>Response JSON
Videoları arama isteğine yanıt JSON nesneleri sonuçlarını içerir. Ayrıştırma sonuçlarını örnekleri görmek için [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app) ve [kaynak kodu](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app-source).

## <a name="next-steps"></a>Sonraki adımlar
**Bing** türüne göre sonuçları arama eylemleri API'lerini desteklemektedir. Tüm arama uç noktaları sonuçlar JSON yanıtı nesneler olarak döndürür.  Tüm uç noktalar, belirli bir dil ve/veya konum boylam, enlem ve arama RADIUS döndüren sorgular destekler.

Her bir uç noktası tarafından desteklenen parametreleri hakkında tam bilgi için her türü için başvuru sayfalarına bakın.
Video arama API'si kullanarak temel istekleri örnekleri için bkz: [Video araması hızlı başlangıçları](https://docs.microsoft.com/azure/cognitive-services/bing-video-search).
