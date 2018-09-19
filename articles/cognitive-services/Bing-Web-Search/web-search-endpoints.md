---
title: Web araması uç noktası
titleSuffix: Azure Cognitive Services
description: Web araması API'si uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: 3058ca6cf0eb99486dd4c269d43b274fb367f7a9
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125468"
---
# <a name="web-search-endpoint"></a>Web araması uç noktası
**Web araması API'si** döndüren Web sayfaları, Haberler, resimler, videolar ve [varlıkları](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Varlıklar bir kişi, yer veya konu ile ilgili özet bilgiler içerir.
## <a name="endpoint"></a>Uç Nokta
Bing API'si kullanarak Web araması sonuçlarını almak için gönderin bir `GET` aşağıdaki uç noktayı isteği. Daha fazla başlık ve URL parametrelerini belirtimleri tanımlayın.

**Uç nokta**: sorgu tarafından tanımlanan arama kullanıcının ilgili döndürür Web sonuçları `?q=""`.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```
Uç noktası: Üstbilgileri, parametreleri hakkında daha fazla ayrıntı için pazara kodları, yanıt nesneleri, hataları, vs. bkz [Bing Web API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) başvuru.

## <a name="response-json"></a>JSON yanıtı
Bir Web isteğine yanıt olarak arama JSON nesnesi tüm sonuçlarını içerir. Ayrıştırma sonucu, her türden öğelerini işlemek yordamları gerektirir. Bkz: [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app) ve [kaynak kodu](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app-source) örnekler.

## <a name="next-steps"></a>Sonraki adımlar
**Bing** türüne göre sonuçları arama eylemleri API'lerini desteklemektedir. Tüm arama uç noktaları sonuçlar JSON yanıtı nesneler olarak döndürür.  Tüm uç noktalar, belirli bir dil ve/veya konum boylam, enlem ve arama RADIUS döndüren sorgular destekler.

Her bir uç noktası tarafından desteklenen parametreleri hakkında tam bilgi için her türü için başvuru sayfalarına bakın.
Web'de arama API'sini kullanarak temel istekleri örnekleri için bkz: [Web hızlı başlangıçları arama](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web).
