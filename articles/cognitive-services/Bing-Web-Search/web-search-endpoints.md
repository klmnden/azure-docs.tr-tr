---
title: Web araması uç noktası
titleSuffix: Azure Cognitive Services
description: Web araması API'si uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: article
ms.date: 11/14/2018
ms.author: v-gedod
ms.openlocfilehash: 8c3fd0fc091edbc4323315f636ed2f4fea7d822a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60499724"
---
# <a name="web-search-endpoint"></a>Web araması uç noktası

**Web araması API'si** döndüren Web sayfaları, Haberler, resimler, videolar ve [varlıkları](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Varlıklar bir kişi, yer veya konu ile ilgili özet bilgiler içerir.

## <a name="endpoint"></a>Uç Nokta

Bing API'si kullanarak Web araması sonuçlarını almak için gönderin bir `GET` aşağıdaki uç noktayı isteği. Daha fazla başlık ve URL parametrelerini belirtimleri tanımlayın.

**Uç nokta**: Tarafından tanımlanan kullanıcı arama sorgusu için uygun Web sonuçları döndüren `?q=""`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```

Uç noktası: Üst bilgiler, parametreleri, Pazar kodları, yanıt nesneleri, hataları ve diğer hakkında daha fazla ayrıntı için bkz: [Bing Web API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) başvuru.

## <a name="response-json"></a>Response JSON

Bir Web isteğine yanıt olarak arama JSON nesnesi tüm sonuçlarını içerir. Ayrıştırma sonucu, her türden öğelerini işlemek yordamları gerektirir. Bkz: [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app) ve [kaynak kodu](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/Tutorials/Bing-Web-Search) örnekler.

## <a name="next-steps"></a>Sonraki adımlar

**Bing** türüne göre sonuçları arama eylemleri API'lerini desteklemektedir. Tüm arama uç noktaları sonuçlar JSON yanıtı nesneler olarak döndürür.  Tüm uç noktalar, belirli bir dil ve konuma boylam, enlem ve arama RADIUS döndüren sorgular destekler.

Her bir uç noktası tarafından desteklenen parametreleri hakkında tam bilgi için her türü için başvuru sayfalarına bakın.
Web'de arama API'sini kullanarak temel istekleri örnekleri için bkz: [Web hızlı başlangıçları arama](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web).
