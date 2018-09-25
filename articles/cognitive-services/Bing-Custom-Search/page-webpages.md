---
title: 'Bing özel arama: Sayfa kullanılabilir Web sayfaları | Microsoft Docs'
description: Tüm Bing döndüren Web sayfalarındaki sayfa işlemi gösterilmektedir.
services: cognitive-services
author: brapel
manager: ehansen
ms.assetid: 26CA595B-0866-43E8-93A2-F2B5E09D1F3B
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 5692776736090c55547c2fe934e2c0aaf840d3c5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982367"
---
# <a name="paging-webpages"></a>Disk belleği Web sayfaları 

Özel arama API'si arama, Bing sonuçların listesini döndürür. Liste, sorgu ile ilgili sonuç toplam sayısı bir alt kümesidir. Kullanılabilir sonuçları tahmin edilen toplam sayısını almak için yanıt nesnenin erişim [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#totalestimatedmatches) alan.  
  
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
  
Kullanılabilir Web sayfası için kullanın [sayısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#count) ve [uzaklığı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#offset) sorgu parametreleri.  
  
`count` Parametre sonuçları yanıtta döndürülecek sayısını belirtir. Yanıtta isteyebilir sonuçları sayısı 50'dir. Varsayılan değer 10'dur. Teslim gerçek sayı istenenden daha az olabilir.

`offset` Parametre atlanacak sonuç sayısını belirtir. `offset` Sıfır tabanlıdır ve olması gereken küçüktür (`totalEstimatedMatches` - `count`).  
  
Sayfa başına 15 Web sayfalarında görüntülemek istiyorsanız, ayarlarsınız `count` 15 ve `offset` ilk sayfasını almak için 0. Her bir sonraki sayfa için gerçekleştirilemediği `offset` 15 (örneğin, 15, 30) tarafından.  
  
Aşağıda, 15 Web sayfalarını 45 uzaklıkta başlayan ister bir örnek gösterilmektedir.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us&customConfig=123456 HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>
Host: api.cognitive.microsoft.com  
```  

Varsayılan `count` değeri, uygulamanız için çalışır, yalnızca belirtmenize gerek `offset` sorgu parametresi.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us&customConfig=123456 HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>  
Host: api.cognitive.microsoft.com  
```  

