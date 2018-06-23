---
title: Video arama uç noktaları | Microsoft Docs
description: Video arama API uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 12/04/2017
ms.author: v-gedod
ms.openlocfilehash: 9836d9928362ab37b0a81ff5043d99f9bf353f22
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351497"
---
# <a name="video-search-endpoints"></a>Video arama uç noktaları
**Video arama API** üç uç noktaları içerir.  Sorgu temelli bir Web videolar uç nokta 1 döndürür. Uç nokta 2 döndürür dayalı bir video hakkında Öngörüler `modules` URL parametresi.  Uç nokta 3 oluşturan eğilim görüntüleri döndürür.

## <a name="endpoints"></a>Uç Noktalar
Bing API'si kullanarak video sonuçları almak için gönderin bir `GET` aşağıdaki uç noktalardan biri isteği. Üstbilgiler ve URL parametreleri daha ayrıntılı belirtimleri tanımlamak için kullanın.

**Bitiş noktası 1:** döndürür tarafından tanımlanan kullanıcının arama sorgusu ilgili videolar `?q=""`.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search
```

**Uç nokta 2:** ilgili videolar gibi bir video hakkında Öngörüler döndürür. Dahil `modules` [sorgu parametresi](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query-parameters), istemek için Öngörüler virgülle ayrılmış bir listesi verilmiştir.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/details
```

**Uç noktası 3:** başkaları tarafından yapılan arama istekleri temel eğilim döndürür videolar. Videolar farklı kategoride Örneğin, önemli kişiler veya olayları göre ayrılır.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending
```

Üstbilgiler, parametreleri, Pazar kodları, yanıt nesneleri, hatalar, hakkındaki ayrıntılar için vs., bkz: [Bing Video arama API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) başvuru.
## <a name="response-json"></a>Yanıt JSON
Bir video arama isteğine yanıt JSON nesnelerinin sonuçlarını içerir. Sonuçları ayrıştırma örnekleri görmek için [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app) ve [kaynak kodu](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app-source).

## <a name="next-steps"></a>Sonraki adımlar
**Bing** API'lerini destekleyen tipine göre sonuçları döndüren arama eylemler. Tüm arama uç noktaları sonuçları JSON yanıt nesneler olarak döndürür.  Tüm uç noktaları boylam, enlem ve arama RADIUS tarafından belirli bir dil ve/veya konum döndüren sorgular destekler.

Her bitiş noktası tarafından desteklenen parametreleri hakkında tam bilgi için her tür başvuru sayfalarına bakın.
Video arama API kullanan temel isteklerinin örnekleri için bkz: [Video arama hızlı-başlatır](https://docs.microsoft.com/azure/cognitive-services/bing-video-search).