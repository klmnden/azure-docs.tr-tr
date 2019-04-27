---
title: Bing otomatik öneri API'si ile arama terimlerini önerme
titlesuffix: Azure Cognitive Services
description: Bing Otomatik Öneri API’sini kullanmayı öğrenin.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: overview
ms.date: 02/20/2019
ms.author: aahi
ms.openlocfilehash: 293dcaadfc20116455983b3fc0069f9e9df3f843
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60549266"
---
# <a name="suggesting-query-terms"></a>Sorgu terimi önerme

Genellikle, Bing otomatik öneri API'si her zaman bir kullanıcı, uygulamanızın arama kutusuna yeni karakter türleri çağırırsınız. Sorgu dizesinin eksiksiz olması, API’nin döndürdüğü önerilen sorgu terimlerinin alakasını etkiler. Sorgu dizesi ne kadar eksiksizse, önerilen sorgu terimlerinin listesi de o kadar alakalıdır. Örneğin, API için döndürebileceği öneriler `s` döndürür için sorguları daha az ilgili olasılığı `sailing dinghies`.

## <a name="example-request"></a>Örnek istek

Aşağıdaki örnekte, *sail* için önerilen sorgu dizelerini döndüren bir istek gösterilmektedir. [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference#query) sorgu parametresini ayarladığınızda, kullanıcının kısmi sorgu terimini URL kodlamayı unutmayın. Örneğin, kullanıcı *sailing dinghies* terimini girdiyse, `q` öğesini `sailing+les` veya `sailing%20les` olarak ayarlayın.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Aşağıdaki yanıt, önerilen sorgu terimlerini içeren [SearchAction](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference#searchaction) nesnelerinin bir listesini içerir.

```json
{
    "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
    "displayText" : "sailing lessons seattle",
    "query" : "sailing lessons seattle",
    "searchKind" : "WebSearch"
}, ...
```

## <a name="using-suggested-query-terms"></a>Önerilen sorgu terimleri kullanmayı

Her öneri, `displayText`, `query` ve `url` alanını içerir. `displayText` alanı, arama kutunuzun açılır listesini doldurmak için kullandığınız önerilen sorguyu içerir. Yanıtın içerdiği tüm önerileri, verilen sırada görüntülemeniz gerekir.

Aşağıdaki örnek, Bing otomatik öneri API'si bir açılan arama kutusuna önerilen sorgu terimleri gösterir.

![Otomatik öneri açılır arama kutusu listesi](../media/cognitive-services-bing-autosuggest-api/bing-autosuggest-drop-down-list.PNG)

Kullanıcı, açılır listeden bir önerilen sorgu seçerse, `query` alanındaki sorgu terimini kullanarak [Bing Web Araması API](../../bing-web-search/search-the-web.md)’sini çağırır ve sonuçları kendiniz görüntülersiniz. Veya bunun yerine kullanıcıyı Bing arama sonuçları sayfasına göndermek için `url` alanındaki URL’yi kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Bing otomatik öneri API'si nedir?](../get-suggested-search-terms.md)