---
title: 'Bing özel arama: Sayfa kullanılabilir Web sayfaları | Microsoft Docs'
description: Tüm Bing döndürebilir Web sayfaları sayfasında gösterilmektedir.
services: cognitive-services
author: brapel
manager: ehansen
ms.assetid: 26CA595B-0866-43E8-93A2-F2B5E09D1F3B
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: f2f545a5a9195fc65515ea716f277723600cbb78
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351412"
---
# <a name="paging-webpages"></a>Disk belleği Web sayfaları 

Özel arama API çağırdığınızda, Bing sonuçlarının bir listesini döndürür. Listeden bir sorgu ile ilgili sonuçları toplam sayısı alt kümesidir. Kullanılabilir sonuçları tahmini toplam sayısını almak üzere yanıt nesnenin erişim [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#totalestimatedmatches) alan.  
  
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
  
Kullanılabilir Web sayfaları sayfa için kullanmak [sayısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#count) ve [uzaklık](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#offset) sorgu parametreleri.  
  
`count` Parametresi, yanıtta döndürmek için sonuç sayısını belirtir. En fazla yanıtta isteyebilir sonuç sayısı 50'dir. Varsayılan değer 10'dur. Teslim gerçek sayı istenenden daha az olabilir.

`offset` Parametresi atlamak için sonuç sayısını belirtir. `offset` Sıfır tabanlı kullanılabilir olmalı ve küçüktür (`totalEstimatedMatches` - `count`).  
  
Sayfa başına 15 Web sayfalarını görüntülemek istiyorsanız, ayarlamalısınız `count` 15 ve `offset` sonuçları'nın ilk sayfasında almak için 0. Her bir sonraki sayfa için gerçekleştirilemediği `offset` 15 (örneğin, 15, 30) tarafından.  
  
Aşağıdaki 45 uzaklıkta başlayan 15 Web sayfalarının ister bir örneği gösterir.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>
Host: api.cognitive.microsoft.com  
```  

Varsa varsayılan `count` değeri uygulamanız için çalışır, yalnızca belirtmeniz gerekir `offset` sorgu parametresi.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>  
Host: api.cognitive.microsoft.com  
```  

