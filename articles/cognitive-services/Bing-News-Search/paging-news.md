---
title: Nasıl yapılır sayfası aracılığıyla kullanılabilir haber makalelerini - Bing haber arama
titlesuffix: Azure Cognitive Services
description: Tüm yeni Bing arama döndürebilir haber makalelerini sayfa işlemi gösterilmektedir.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 0507f2cfb1d75025d1b6aadccc442326a52ceebc
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739813"
---
# <a name="paging-news"></a>Disk belleği Haberleri

Bing, haber arama API'si çağırdığınızda, sonuçların listesini döndürür. Liste, sorgu ile ilgili sonuç toplam sayısı bir alt kümesidir. Kullanılabilir sonuçları tahmin edilen toplam sayısını almak için yanıt nesnenin erişim [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news-totalmatches) alan.  
  
Aşağıdaki örnekte gösterildiği `totalEstimatedMatches` haber yanıt içeren alan.  
  
```  
{  
    "_type" : "News",  
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",  
    "totalEstimatedMatches" : 88400,  
    "value" : [...]  
}  
```  
  
Makalelerde sayfasında kullanın [sayısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#count) ve [uzaklığı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#offset) sorgu parametreleri.  
  
`count` Parametre sonuçları yanıtta döndürülecek sayısını belirtir. Yanıtta isteyebilir sonuçları sayısının 100'dür. Varsayılan değer 10'dur. Teslim gerçek sayı istenenden daha az olabilir.

`offset` Parametre atlanacak sonuç sayısını belirtir. `offset` Sıfır tabanlıdır ve olması gereken küçüktür (`totalEstimatedMatches` - `count`).  

Sayfa başına 20 makaleleri görüntülemek istiyorsanız, ayarlarsınız `count` 20 ve `offset` ilk sayfasını almak için 0. Her bir sonraki sayfa için gerçekleştirilemediği `offset` 20 (örneğin, 20, 40) tarafından.  
  
Aşağıda, 40 uzaklıkta başlayan 20 haber makaleleri ister bir örnek gösterilmektedir.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
Varsayılan `count` değer uygulamanız için çalışır, yalnızca belirtin `offset` sorgu parametresi aşağıdaki örnekte gösterildiği gibi:  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
> [!NOTE]
> Disk belleği, yalnızca haber arama (arama/haber /) ve popüler konularını (/ haber/trendingtopics) veya haber kategorileri uygular (/ haber).

> [!NOTE]
> `TotalEstimatedAnswers` Alandır ne tahmini toplam arama sonuçları almak için geçerli sorgu sayısı.  Ayarladığınızda `count` ve `offset` parametreleri `TotalEstimatedAnswers` numarası değişebilir. 