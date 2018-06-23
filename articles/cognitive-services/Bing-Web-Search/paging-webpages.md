---
title: Kullanılabilir Web sayfaları aracılığıyla sayfasına nasıl | Microsoft Docs
description: Tüm Bing döndürebilir Web sayfaları sayfasında gösterilmektedir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 26CA595B-0866-43E8-93A2-F2B5E09D1F3B
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: bf29783246c603270d59b20b63027fccdbd45b89
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351527"
---
# <a name="paging-webpages"></a>Disk belleği Web sayfaları 

Web ara API çağırdığınızda, Bing sonuçlarının bir listesini döndürür. Listeden bir sorgu ile ilgili sonuçları toplam sayısı alt kümesidir. Kullanılabilir sonuçları tahmini toplam sayısını almak üzere yanıt nesnenin erişim [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#totalestimatedmatches) alan.  
  
Aşağıdaki örnekte gösterildiği `totalEstimatedMatches` bir Web yanıtı içeren alan.  
  
```  
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    }
}  
```  
  
Kullanılabilir Web sayfaları sayfa için kullanmak [sayısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#count) ve [uzaklık](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#offset) sorgu parametreleri.  
  
`count` Parametresi, yanıtta döndürmek için sonuç sayısını belirtir. En fazla yanıtta isteyebilir sonuç sayısı 50'dir. Varsayılan değer 10'dur. Teslim gerçek sayı istenenden daha az olabilir.

`offset` Parametresi atlamak için sonuç sayısını belirtir. `offset` Sıfır tabanlı kullanılabilir olmalı ve küçüktür (`totalEstimatedMatches` - `count`).  
  
Sayfa başına 15 Web sayfalarını görüntülemek istiyorsanız, ayarlamalısınız `count` 15 ve `offset` sonuçları'nın ilk sayfasında almak için 0. Her bir sonraki sayfa için gerçekleştirilemediği `offset` 15 (örneğin, 15, 30) tarafından.  
  
Aşağıdaki örnek 45 uzaklıkta başlayan 15 Web sayfalarının ister.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

Varsa varsayılan `count` değeri uygulamanız için çalışır, yalnızca belirtmeniz gerekir `offset` sorgu parametresi.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

Web ara API, Web sayfaları ve görüntü, video ve haber öğeleri içerebilir sonuçlar döndürür. Arama sonuçları sayfası, disk belleği [WebAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#webanswer) yanıt ve değil diğer yanıtlar görüntü veya news gibi. Örneğin, ayarlarsanız `count` 50'ye 50 Web sayfası sonuçları ulaşırsınız, ancak yanıt sonuçları diğer yanıtlar için de içerebilir. Örneğin, yanıt 15 görüntüleri ve 4 haber makalelerini içerebilir. Ayrıca sonuçları ilk sayfa ancak ikinci sayfada değil, haber içerebilir mümkündür (veya tersi).   
    
Belirtirseniz `responseFilter` sorgu parametresi ve Web sayfalarını filtre listesinde dahil etmeyin, kullanmayan `count` ve `offset` parametreleri.  
