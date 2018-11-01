---
title: Nasıl yapılır videoları kullanılabilir - Bing Video arama sayfasından
titlesuffix: Azure Cognitive Services
description: Tüm Bing döndürebilir videoları sayfa işlemi gösterilmektedir.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: cd5d6ef6a75088d02bc34558f66ed520c230e06b
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741904"
---
# <a name="paging-videos"></a>Disk belleği videoları

Video Arama API'sini çağırdığınızda Bing, sonuç listesini döndürür. Bu liste sorguyla ilgili tüm sonuçların alt kümesidir. Kullanılabilir sonuçları tahmin edilen toplam sayısını almak için yanıt nesnenin erişim [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-totalestimatedmatches) alan.  
  
Aşağıdaki örnekte gösterildiği `totalEstimatedMatches` Video yanıt içeren alan.  
  
```  
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
    "totalEstimatedMatches" : 1000,
    "value" : [...]
}  
```  
  
Mevcut videoları sayfası için kullanın [sayısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#count) ve [uzaklığı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#offset) sorgu parametreleri.  
  
`count` Parametre sonuçları yanıtta döndürülecek sayısını belirtir. En fazla yanıtta isteyebilir sonuçları 105 sayısıdır. 35 varsayılandır. Teslim gerçek sayı istenenden daha az olabilir.

`offset` Parametre atlanacak sonuç sayısını belirtir. `offset` Sıfır tabanlıdır ve olması gereken küçüktür (`totalEstimatedMatches` - `count`).  
  
Sayfa başına 20 videoları görüntülemek istiyorsanız, ayarlarsınız `count` 20 ve `offset` ilk sayfasını almak için 0. Her bir sonraki sayfa için gerçekleştirilemediği `offset` 20 (örneğin, 20, 40) tarafından.  

Aşağıda, 40 uzaklıkta başlayan 20 videoları istekleri bir örnek gösterilmektedir.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Varsayılan `count` değeri, uygulamanız için çalışır, yalnızca belirtmenize gerek `offset` sorgu parametresi.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Genellikle, bir kerede 35 video sayfasında, ayarlarsınız `offset` sorgu parametresi 0 olarak ilk isteğinizi ve ardından Artır `offset` 35 sonraki her istek tarafından. Ancak, bazı sonuçları sonraki yanıt önceki isteğin çoğaltmaları olabilir. Örneğin, ilk iki videoları yanıt önceki yanıtın son iki videolardan aynı olabilir.

Yinelenen sonuçları ortadan kaldırmak için kullanın [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-nextoffset) alanını `Videos` nesne.

Örneğin, bir kerede 30 video sayfasında istiyorsanız ayarlarsınız `count` 30 ve `offset` ilk isteğinizdeki 0. Sonraki isteğiniz ayarlarsınız `offset` sorgu parametresi için `nextOffset` değeri.


> [!NOTE]
> Disk belleği yalnızca videoları arama (arama/video /) ve video öngörüleri (/ video/ayrıntıları) veya (/ video/popüler) popüler videolar için geçerlidir.

> [!NOTE]
> `TotalEstimatedAnswers` Alandır ne tahmini toplam arama sonuçları almak için geçerli sorgu sayısı.  Ayarladığınızda `count` ve `offset` parametreleri `TotalEstimatedAnswers` numarası değişebilir. 