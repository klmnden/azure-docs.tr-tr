---
title: Kullanılabilir video sayfadan nasıl | Microsoft Docs
description: Tüm Bing döndürebilir videolar sayfasında gösterilmektedir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 910A485F-BCF3-42B9-958D-DD48BDEDA965
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 00476825eb3fc1008c3f2172b591d8b7a2f35884
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351431"
---
# <a name="paging-videos"></a>Disk belleği videolar

Video arama API çağırdığınızda, Bing sonuçlarının bir listesini döndürür. Listeden bir sorgu ile ilgili sonuçları toplam sayısı alt kümesidir. Kullanılabilir sonuçları tahmini toplam sayısını almak üzere yanıt nesnenin erişim [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-totalestimatedmatches) alan.  
  
Aşağıdaki örnekte gösterildiği `totalEstimatedMatches` Video yanıt içeren alan.  
  
```  
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
    "totalEstimatedMatches" : 1000,
    "value" : [...]
}  
```  
  
Kullanılabilir videolar sayfa için kullanmak [sayısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#count) ve [uzaklık](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#offset) sorgu parametreleri.  
  
`count` Parametresi, yanıtta döndürmek için sonuç sayısını belirtir. En fazla yanıtta isteyebilir sonuçları 105 sayısıdır. 35 varsayılandır. Teslim gerçek sayı istenenden daha az olabilir.

`offset` Parametresi atlamak için sonuç sayısını belirtir. `offset` Sıfır tabanlı kullanılabilir olmalı ve küçüktür (`totalEstimatedMatches` - `count`).  
  
Sayfa başına 20 videolar görüntülemek istiyorsanız, ayarlamalısınız `count` 20 ve `offset` sonuçları'nın ilk sayfasında almak için 0. Her bir sonraki sayfa için gerçekleştirilemediği `offset` 20 (örneğin, 20, 40) tarafından.  

40 uzaklıkta başlayan 20 videolar ister bir örnek gösterilmektedir.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Varsa varsayılan `count` değeri uygulamanız için çalışır, yalnızca belirtmeniz gerekir `offset` sorgu parametresi.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Genellikle, aynı anda 35 videolar sayfasında, ayarlamalısınız `offset` sorgu parametresi 0 olarak ilk isteğiniz ve ardından Artır `offset` sonraki her istek üzerinde 35 tarafından. Ancak, sonraki yanıt sonuçlarında bazıları önceki yanıtın çoğaltmaları olabilir. Örneğin, ilk iki videolar yanıt önceki yanıtın son iki videoların aynı olabilir.

Yinelenen sonuçları ortadan kaldırmak için kullanın [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-nextoffset) alanını `Videos` nesnesi.

Örneğin, aynı anda 30 videolar sayfasında istiyorsanız, ayarlamalısınız `count` 30 ve `offset` ilk isteğiniz 0. Sonraki İsteğinizde ayarlamalısınız `offset` sorgu parametresi için `nextOffset` değeri.


> [!NOTE]
> Disk belleği yalnızca videolar arama (/ video/arama) ve video Öngörüler (/ video/ayrıntıları) veya (/ video/eğilim) oluşturan eğilim videolar geçerlidir.