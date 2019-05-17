---
title: Bing resim arama API'si için uç noktalar
titleSuffix: Azure Cognitive Services
description: Bing resim arama API'si için kullanılabilir uç noktalar listesi.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: article
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 9b3edd10d2928a512b94e9273000439f80cb8f33
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65777090"
---
# <a name="endpoints-for-the-bing-image-search-api"></a>Bing resim arama API'si için uç noktalar

**Resim arama API'si** üç uç noktaları içerir.  Uç nokta 1 görüntüleri bir sorguyu temel alan Web döndürür. Uç nokta 2 döndürür [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse).  Uç nokta 3 popüler resimlerden döndürür.

## <a name="endpoints"></a>Uç Noktalar

Bing API'si kullanarak görüntü sonuçlarını almak için aşağıdaki uç noktaların biri için bir istek gönderin. Daha fazla özellikleri tanımlamak için başlık ve URL parametrelerini kullanın.

**Uç nokta 1:** Tarafından tanımlanan kullanıcı arama sorgusu için uygun olan görüntüleri döndürür `?q=""`.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search
```

**Uç nokta 2:** Döndürür ya da kullanarak görüntü, ilgili Öngörüler `GET` veya `POST`.
```
 GET or POST https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
Görüntü içeren Web sayfaları gibi bir görüntü ile ilgili öngörüleri bir GET isteği döndürür. Dahil [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) parametresiyle birlikte bir `GET` istek.

Veya bir ikili görüntüsü gövdesinde dahil edebilirsiniz bir `POST` Ayarla ve isteği [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) parametresi `RecognizedEntities`. Bu döndürür bir [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v5-reference#insightstoken) sonraki parametre olarak kullanılacak `GET` isteği, görüntüde kişilerle ilgili bilgileri döndürür.  Ayarlama `modules` için `All` dışındaki tüm öngörüleri almak için `RecognizedEntities` sonuçlarında `POST` kullanarak başka bir çağrı yapmadan `insightsToken`.


**3 uç noktası:** Arama istekleri, başkaları tarafından yapılan mu döndürür görüntülerini temel. Görüntüleri farklı kategorilerde gibi önemli kişiler veya olayları göre ayrılır.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending
```

Popüler resimler destekleyen pazarlara listesi için bkz. [popüler resimler](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/trending-images).

Üstbilgileri, parametreleri, Pazar kodları, yanıt nesneleri, hata hakkındaki ayrıntılar için vb., bkz: [Bing resim arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) başvuru.
## <a name="response-json"></a>Response JSON
Görüntü arama isteğine yanıt JSON nesneleri sonuçlarını içerir. Ayrıştırma sonuçlarını örnekleri görmek için [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app) ve [kaynak kodu](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app-source).

## <a name="next-steps"></a>Sonraki adımlar
**Bing** türüne göre sonuçları arama eylemleri API'lerini desteklemektedir. Tüm arama uç noktaları sonuçlar JSON yanıtı nesneler olarak döndürür.  Tüm uç noktalar, belirli bir dil ve/veya konum boylam, enlem ve arama RADIUS döndüren sorgular destekler.

Her bir uç noktası tarafından desteklenen parametreleri hakkında tam bilgi için her türü için başvuru sayfalarına bakın.
Görüntü arama API'sini kullanarak temel istekleri örnekleri için bkz: [resim arama hızlı başlangıçları](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/search-the-web).
