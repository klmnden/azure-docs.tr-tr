---
title: Görüntü arama uç noktaları | Microsoft Docs
description: Görüntü arama API uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 11/30/2017
ms.author: v-gedod
ms.openlocfilehash: 0d5f28bfdb45b27b04df068f75e8a20d0235d12b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351400"
---
# <a name="image-search-endpoints"></a>Görüntü arama uç noktaları
**Görüntü arama API** üç uç noktaları içerir.  Sorgu temelli bir Web görüntüleri uç nokta 1 döndürür. Uç nokta 2 döndürür [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse).  Uç nokta 3 oluşturan eğilim görüntüleri döndürür.
## <a name="endpoints"></a>Uç Noktalar
Bing API'si kullanarak görüntü sonuçları almak için aşağıdaki uç noktalardan biri için bir istek gönderin. Üstbilgiler ve URL parametreleri daha ayrıntılı belirtimleri tanımlamak için kullanın.

**Uç nokta 1:** döndürür tarafından tanımlanan kullanıcının arama sorgusu için uygun olan görüntüleri `?q=""`.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search
```

**Uç nokta 2:** döndürür ya da kullanarak bir görüntü ile ilgili Öngörüler `GET` veya `POST`.
``` 
 GET or POST https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
GET isteği görüntü içeren Web sayfaları gibi bir görüntü ile ilgili Öngörüler döndürür. Dahil [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) parametresiyle bir `GET` isteği.

Ya da ikili bir resim gövdesinde ekleyebilirsiniz bir `POST` istemek ve ayarlama [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) parametresi `RecognizedEntities`. Bu döndürülecek bir [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v5-reference#insightstoken) bir sonraki içindeki bir parametre olarak kullanılmak üzere `GET` görüntüde kişiler hakkındaki bilgileri döndürür isteği.  Ayarlama `modules` için `All` tüm bilgileri elde etmek için dışındaki `RecognizedEntities` sonuçlarında `POST` başka bir çağrı kullanılarak yapmadan `insightsToken`. 


**Uç noktası 3:** başkaları tarafından yapılan arama istekleri temel eğilim döndürür görüntüler. Görüntüleri farklı kategoride Örneğin, önemli kişiler veya olayları göre ayrılır.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending
```

Oluşturan eğilim görüntüleri desteklemek pazarda listesi için bkz: [eğilim görüntüleri](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/trending-images).

Üstbilgiler, parametreleri, Pazar kodları, yanıt nesneleri, hatalar, hakkındaki ayrıntılar için vs., bkz: [Bing görüntü arama API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) başvuru.
## <a name="response-json"></a>Yanıt JSON
Bir görüntü arama isteğine yanıt JSON nesnelerinin sonuçlarını içerir. Sonuçları ayrıştırma örnekleri görmek için [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app) ve [kaynak kodu](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app-source).

## <a name="next-steps"></a>Sonraki adımlar
**Bing** API'lerini destekleyen tipine göre sonuçları döndüren arama eylemler. Tüm arama uç noktaları sonuçları JSON yanıt nesneler olarak döndürür.  Tüm uç noktaları boylam, enlem ve arama RADIUS tarafından belirli bir dil ve/veya konum döndüren sorgular destekler.

Her bitiş noktası tarafından desteklenen parametreleri hakkında tam bilgi için her tür başvuru sayfalarına bakın.
Görüntü arama API kullanan temel isteklerinin örnekleri için bkz: [görüntü arama hızlı-başlatır](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/search-the-web).