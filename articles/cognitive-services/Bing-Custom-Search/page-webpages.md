---
title: Kullanılabilir Web sayfalarını - Bing özel arama aracılığıyla sayfası
titlesuffix: Azure Cognitive Services
description: Tüm Bing özel arama döndüren Web sayfalarındaki sayfa işlemi gösterilmektedir.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: maheshb
ms.openlocfilehash: b4e022d6237712d8281c6f4f9332a68821dfd5b1
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49470090"
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

