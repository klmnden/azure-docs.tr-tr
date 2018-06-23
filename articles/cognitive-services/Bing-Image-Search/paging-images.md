---
title: Kullanılabilir görüntüleri sayfadan nasıl | Microsoft Docs
description: Tüm Bing döndürebilir görüntüleri sayfasında gösterilmektedir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 3C8423F8-41E0-4F89-86B6-697E840610A7
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: a74ee817e84be5bb563c5fdaf25afc1dc14732e5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351388"
---
# <a name="paging-results"></a>Disk belleği sonuçları

Görüntü arama API çağırdığınızda, Bing sonuçlarının bir listesini döndürür. Listeden bir sorgu ile ilgili sonuçları toplam sayısı alt kümesidir. Kullanılabilir sonuçları tahmini toplam sayısını almak üzere yanıt nesnenin erişim [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#totalestimatedmatches) alan.  
  
Aşağıdaki örnekte gösterildiği `totalEstimatedMatches` görüntüleri yanıt içeren alan.  
  
```json
{
    "_type" : "Images",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8...",
    "totalEstimatedMatches" : 838,
    "value" : [...]  
}  
```  
  
Kullanılabilir Resimler arasında gezinmek için kullanmak [sayısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#count) ve [uzaklık](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offset) sorgu parametreleri.  
  
`count` Parametresi, yanıtta döndürmek için sonuç sayısını belirtir. En fazla yanıtta isteyebilir sonuçları 150 sayısıdır. 35 varsayılandır. Teslim gerçek sayı istenenden daha az olabilir.

`offset` Parametresi atlamak için sonuç sayısını belirtir. `offset` Sıfır tabanlı kullanılabilir olmalı ve küçüktür (`totalEstimatedMatches` - `count`).  
  
Sayfa başına 20 görüntüleri görüntülemek istiyorsanız, ayarlamalısınız `count` 20 ve `offset` sonuçları'nın ilk sayfasında almak için 0. 40 uzaklıkta başlayan 20 görüntüleri ister bir örnek gösterilmektedir.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Varsa varsayılan `count` değeri uygulamanız için çalışır, yalnızca belirtmeniz gerekir `offset` sorgu parametresi.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
Aynı anda 35 görüntüleri sayfasında, ayarlamak bekleyebilirsiniz `offset` sorgu parametresi 0 olarak ilk isteğiniz ve ardından Artır `offset` sonraki her istek üzerinde 35 tarafından. Ancak, sonraki yanıt sonuçlarında bazıları önceki yanıtın çoğaltmaları olabilir. Örneğin, ilk iki görüntü yanıt önceki yanıtın son iki görüntülerden aynı olabilir.

Yinelenen sonuçları ortadan kaldırmak için kullanın [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#nextoffset) alanını `Images` nesnesi. `nextOffset` Alanı bildirir `offset` sonraki istek için kullanılacak. Örneğin, aynı anda 30 görüntüleri sayfasında istiyorsanız, ayarlamalısınız `count` 30 ve `offset` ilk isteğiniz 0. Sonraki İsteğinizde ayarlamalısınız `count` 30 ve `offset` önceki yanıtın değerine `nextOffset`. Geriye doğru sayfa için önceki uzaklıkları yığınını koruma ve en son pencerelerinin öneririz.

> [!NOTE]
> Disk belleği yalnızca görüntü arama (/ görüntüleri/arama) ve görüntü Öngörüler veya oluşturan eğilim görüntüler (/ görüntüleri/eğilim) için geçerlidir.