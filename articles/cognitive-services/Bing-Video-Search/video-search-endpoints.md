---
title: Video arama uç noktaları - Bing Video arama
titlesuffix: Azure Cognitive Services
description: Bing Video arama API'si uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: conceptual
ms.date: 12/04/2017
ms.author: rosh
ms.openlocfilehash: c153f577f76944d22f9a1b0fb4b24d332d2a02c8
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220780"
---
# <a name="video-search-endpoints"></a>Video arama uç noktaları
**Video arama API'si** üç uç noktaları içerir.  Uç nokta 1 videoları bir sorguyu temel alan Web döndürür. Uç nokta 2 döndürür dayalı bir video öngörüleri `modules` URL parametresi.  Uç nokta 3 popüler resimlerden döndürür.

## <a name="endpoints"></a>Uç Noktalar
Bing API'si kullanarak video sonuçları elde etmek için Gönder bir `GET` aşağıdaki uç noktalarından birinin isteği. Daha fazla özellikleri tanımlamak için başlık ve URL parametrelerini kullanın.

**Bitiş noktası 1:** döndürür tarafından tanımlanan kullanıcı arama sorgusu için ilgili videoları `?q=""`.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search
```

**Uç nokta 2:** ilgili videolar gibi bir video hakkında bilgiler döndürür. Dahil `modules` [sorgu parametresi](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query-parameters), ınsights istemek için virgülle ayrılmış listesi verilmiştir.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/details
```

**Uç noktası 3:** başkaları tarafından yapılan arama istekleri dayanan popüler videoları döndürür. Videoları farklı kategorilerde gibi önemli kişiler veya olayları göre ayrılır.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending
```

Üstbilgileri, parametreleri, Pazar kodları, yanıt nesneleri, hata hakkındaki ayrıntılar için vb., bkz: [Bing Video arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) başvuru.
## <a name="response-json"></a>JSON yanıtı
Videoları arama isteğine yanıt JSON nesneleri sonuçlarını içerir. Ayrıştırma sonuçlarını örnekleri görmek için [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app) ve [kaynak kodu](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app-source).

## <a name="next-steps"></a>Sonraki adımlar
**Bing** türüne göre sonuçları arama eylemleri API'lerini desteklemektedir. Tüm arama uç noktaları sonuçlar JSON yanıtı nesneler olarak döndürür.  Tüm uç noktalar, belirli bir dil ve/veya konum boylam, enlem ve arama RADIUS döndüren sorgular destekler.

Her bir uç noktası tarafından desteklenen parametreleri hakkında tam bilgi için her türü için başvuru sayfalarına bakın.
Video arama API'si kullanarak temel istekleri örnekleri için bkz: [Video araması hızlı başlangıçları](https://docs.microsoft.com/azure/cognitive-services/bing-video-search).