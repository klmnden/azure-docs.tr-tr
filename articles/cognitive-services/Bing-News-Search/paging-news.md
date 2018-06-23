---
title: Kullanılabilir haber makalelerini sayfadan nasıl | Microsoft Docs
description: Tüm Bing döndürebilir haber makalelerini sayfasında gösterilmektedir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: EA388F72-FA43-493B-967C-9560B3243C62
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 2c90d468536f0864d7deac073667e29e9a54692f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351466"
---
# <a name="paging-news"></a>Disk belleği Haberleri

Bing Haberler arama API çağırdığınızda, sonuçlarının bir listesini döndürür. Listeden bir sorgu ile ilgili sonuçları toplam sayısı alt kümesidir. Kullanılabilir sonuçları tahmini toplam sayısını almak üzere yanıt nesnenin erişim [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news-totalmatches) alan.  
  
Aşağıdaki örnekte gösterildiği `totalEstimatedMatches` haber yanıt içeren alan.  
  
```  
{  
    "_type" : "News",  
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",  
    "totalEstimatedMatches" : 88400,  
    "value" : [...]  
}  
```  
  
Kullanılabilir makalelerine sayfa için kullanmak [sayısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#count) ve [uzaklık](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#offset) sorgu parametreleri.  
  
`count` Parametresi, yanıtta döndürmek için sonuç sayısını belirtir. Yanıtta isteyebilir sonuçları sayısının 100'dür. Varsayılan değer 10'dur. Teslim gerçek sayı istenenden daha az olabilir.

`offset` Parametresi atlamak için sonuç sayısını belirtir. `offset` Sıfır tabanlı kullanılabilir olmalı ve küçüktür (`totalEstimatedMatches` - `count`).  

Sayfa başına 20 makaleleri görüntülemek istiyorsanız, ayarlamalısınız `count` 20 ve `offset` sonuçları'nın ilk sayfasında almak için 0. Her bir sonraki sayfa için gerçekleştirilemediği `offset` 20 (örneğin, 20, 40) tarafından.  
  
40 uzaklıkta başlayan 20 haber makaleleri ister bir örnek gösterilmektedir.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
Varsa varsayılan `count` değeri, uygulamanız için çalışır, yalnızca belirtin `offset` aşağıdaki örnekte gösterildiği gibi sorgu parametresi:  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
> [!NOTE]
> Disk belleği uygular yalnızca haber arama (/ haber/arama) ve popüler konuları (/ haber/trendingtopics) veya haber kategorileri (/ haber).