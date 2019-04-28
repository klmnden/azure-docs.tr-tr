---
title: Bing resim arama API'si tarafından döndürülen görüntüleri aracılığıyla sayfası
titleSuffix: Azure Cognitive Services
description: Bing resim arama API'si tarafından döndürülen görüntülerin farklı sayfalar arasında taşıyın.
services: cognitive-services
author: swhite-msft
manager: cgonlun
ms.assetid: 3C8423F8-41E0-4F89-86B6-697E840610A7
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: 38b2244d68de25f53d59dd4eb0a6beba03f0e51d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60916691"
---
# <a name="page-through-the-images-results"></a>Seçmenin yanı sıra görüntüleri sonuçları

Bing, resim arama API'si çağırdığınızda, sonuçların listesini döndürür. Bu liste sorguyla ilgili tüm sonuçların alt kümesidir. Kullanılabilir sonuçları tahmin edilen toplam sayısını almak için yanıt nesnenin erişim [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#totalestimatedmatches) alan.  

Aşağıdaki örnekte gösterildiği `totalEstimatedMatches` görüntüleri yanıt içeren alan.  

```json
{
    "_type" : "Images",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8...",
    "totalEstimatedMatches" : 838,
    "value" : [...]  
}  
```  

Kullanılabilir görüntüleri sayfasında kullanın [sayısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#count) ve [uzaklığı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offset) sorgu parametreleri.  

`count` Parametre sonuçları yanıtta döndürülecek sayısını belirtir. Yanıtta isteyebilir sonuçları sayısı 150'dir. 35 varsayılandır. Teslim gerçek sayı istenenden daha az olabilir.

`offset` Parametre atlanacak sonuç sayısını belirtir. `offset` Sıfır tabanlıdır ve olması gereken küçüktür (`totalEstimatedMatches` - `count`).  

Sayfa başına 20 görüntüleri görüntülemek istiyorsanız, ayarlarsınız `count` 20 ve `offset` ilk sayfasını almak için 0. Aşağıda, 40 uzaklıkta başlayan 20 görüntüleri istekleri bir örnek gösterilmektedir.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Varsayılan `count` değeri, uygulamanız için çalışır, yalnızca belirtmenize gerek `offset` sorgu parametresi.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

35 görüntüleri birer sayfa, ayarlamalı beklediğiniz `offset` sorgu parametresi 0 olarak ilk isteğinizi ve ardından Artır `offset` 35 sonraki her istek tarafından. Ancak, bazı sonuçları sonraki yanıt önceki isteğin çoğaltmaları olabilir. Örneğin, ilk iki görüntü yanıt önceki yanıtın son iki görüntülerden aynı olabilir.

Yinelenen sonuçları ortadan kaldırmak için kullanın [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#nextoffset) alanını `Images` nesne. `nextOffset` Alan bildirir `offset` sonraki isteğiniz için kullanılacak. Örneğin, bir kerede 30 görüntüleri sayfasında istiyorsanız ayarlarsınız `count` 30 ve `offset` ilk isteğinizdeki 0. Sonraki isteğiniz ayarlarsınız `count` 30 ve `offset` önceki yanıtın değerine `nextOffset`. Geriye doğru gezinmek için önceki uzaklıkları yığınını bakımını yapma ve en son pencerelerinin öneririz.

> [!NOTE]
> Disk belleği, yalnızca resim arama (arama/resimler /) ve resim öngörüleri veya (/ Resimler/popüler) popüler resimler için geçerlidir.

> [!NOTE]
> `TotalEstimatedAnswers` Alandır ne tahmini toplam arama sonuçları almak için geçerli sorgu sayısı.  Ayarladığınızda `count` ve `offset` parametreleri `TotalEstimatedAnswers` numarası değişebilir. 
