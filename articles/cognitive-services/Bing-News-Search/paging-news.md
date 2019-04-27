---
title: Bing haber arama sonuçları sayfasını nasıl
titlesuffix: Azure Cognitive Services
description: Bing haber arama API'si döndüren haber makalelerini sayfasında öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: scottwhi
ms.openlocfilehash: 1d344f388b03acb3a81fcfde0e214eb7d82dc9b9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60578734"
---
# <a name="how-to-page-through-news-search-results"></a>Haber arama sonuçları sayfasını nasıl

Haber arama API'si arama, Bing, sorgu ile ilgili sonuçların listesini döndürür. Kullanılabilir sonuçları tahmin edilen toplam sayısını almak için yanıt nesnenin erişim [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news-totalmatches) alan.  
  
Aşağıdaki örnekte gösterildiği `totalEstimatedMatches` haber yanıt içeren alan.  

```json
{  
    "_type" : "News",  
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",  
    "totalEstimatedMatches" : 88400,  
    "value" : [...]  
}  
```  
  
Makalelerde sayfasında kullanın [sayısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#count) ve [uzaklığı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#offset) sorgu parametreleri.  
 

|Parametre  |Açıklama  |
|---------|---------|
|`count`     | Yanıtta döndürülecek sonuç sayısını belirtir. Yanıtta isteyebilir sonuçları sayısının 100'dür. Varsayılan değer 10'dur. Teslim gerçek sayı istenenden daha az olabilir.        |
|`offset`     | Atlanacak sonuç sayısını belirtir. `offset` Sıfır tabanlıdır ve olması gereken küçüktür (`totalEstimatedMatches` - `count`).          |

Örneğin, sayfa başına 20 makaleleri görüntülemek istiyorsanız, ayarlarsınız `count` 20 ve `offset` ilk sayfasını almak için 0. Her bir sonraki sayfa için gerçekleştirilemediği `offset` 20 (örneğin, 20, 40) tarafından.  
  
Aşağıdaki örnek, 40 uzaklıkta başlayan 20 haber makalelerini ister.  

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
