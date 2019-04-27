---
title: Nasıl yapılır videoları kullanılabilir - Bing Video arama sayfasından
titlesuffix: Azure Cognitive Services
description: Tüm videoların Bing Video arama API'si tarafından döndürülen sayfasında öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: scottwhi
ms.openlocfilehash: 0af36fa68b2d801eed52e6f081b040fb56929c91
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60613639"
---
# <a name="paging-through-video-search-results"></a>Video arama sonuçlarını sayfalandırma

Bing Video arama API'si, bir alt kümesini sorgunuz için bulunan tüm arama sonuçlarını döndürür. Bu sonuçları API'sine yapılan sonraki çağrılar aracılığıyla disk belleği tarafından alın ve bunları uygulamanızda görüntüler.

> [!NOTE]
> Disk belleği yalnızca videoları arama (arama/video /) ve video öngörüleri (/ video/ayrıntıları) veya (/ video/popüler) popüler videolar için geçerlidir.

## <a name="total-estimated-matches"></a>Tahmini Toplam eşleşmeleri

Tahmini bulundu arama sonucu sayısını almak için kullanın [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-totalestimatedmatches) JSON yanıtındaki alan.   
  
```json  
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
    "totalEstimatedMatches" : 1000,
    "value" : [...]
}  
```  
  
## <a name="paging-through-videos"></a>Disk belleği videoları

Mevcut videoları sayfası için kullanın [sayısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#count) ve [uzaklığı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#offset) isteğiniz gönderilirken sorgu parametreleri.  
  

|Parametre  |Açıklama  |
|---------|---------|
|`count`     | Yanıtta döndürülecek sonuç sayısını belirtir. Yanıtta isteyebilir sonuçları sayısının 100'dür. Varsayılan değer 10'dur. Teslim gerçek sayı istenenden daha az olabilir.        |
|`offset`     | Atlanacak sonuç sayısını belirtir. `offset` Sıfır tabanlıdır ve olması gereken küçüktür (`totalEstimatedMatches` - `count`).          |

Örneğin, sayfa başına 20 makaleleri görüntülemek istiyorsanız, ayarlarsınız `count` 20 ve `offset` ilk sayfasını almak için 0. Her bir sonraki sayfa için gerçekleştirilemediği `offset` 20 (örneğin, 20, 40) tarafından.  
  
Aşağıdaki örnekte, 20 videolar, 40 uzaklıkta başlayan ister.  
  
```cURL  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

İçin varsayılan değeri kullanırsanız [sayısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#count), yalnızca belirtmenize gerek `offset` sorgu parametresi, aşağıdaki örnekte olduğu gibi.  
  
```cURL  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Aynı anda 35 videoları sayfası, ayarlarsınız `offset` sorgu parametresi 0 olarak ilk isteğinizi ve ardından Artır `offset` 35 sonraki her istek tarafından. Ancak, bazı sonuçları sonraki yanıt önceki yanıtından video yinelenen sonuçlar içerebilir. Örneğin, bir yanıt ilk iki videoları önceki yanıtın son iki videolardan aynı olabilir.

Yinelenen sonuçları ortadan kaldırmak için kullanın [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-nextoffset) alanını `Videos` nesne.

Örneğin, bir kerede 30 video sayfasında istiyorsanız, ayarlayabileceğiniz `count` 30 ve `offset` ilk isteğinizdeki 0. Sonraki isteğiniz ayarlarsınız `offset` sorgu parametresi için `nextOffset` değeri.

> [!NOTE]
> `TotalEstimatedAnswers` Alandır ne tahmini toplam arama sonuçları almak için geçerli sorgu sayısı.  Ayarladığınızda `count` ve `offset` parametreleri `TotalEstimatedAnswers` numarası değişebilir. 
  
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Video Öngörüler elde edin](video-insights.md)
