---
title: Bing Otomatik Öneri nedir?
titlesuffix: Azure Cognitive Services
description: Bing Otomatik Öneri API’sini kullanmayı öğrenin.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: overview
ms.date: 09/12/2017
ms.author: scottwhi
ms.openlocfilehash: e1e1ec2a9f5329f5d7331b7ce3ced4924d001a49
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55174185"
---
# <a name="what-is-bing-autosuggest"></a>Bing Otomatik Öneri nedir?

Bing Arama API’lerinden herhangi birine sorgular gönderirseniz, arama kutusu deneyiminizi iyileştirmek için Bing Otomatik Öneri API’sini kullanabilirsiniz. Bing Otomatik Öneri API’si, kullanıcının arama kutusuna girdiği kısmi sorgu dizesine göre önerilen sorguların bir listesini döndürür. Arama kutusunun açılır listesinde öneriler görüntülenir. Önerilen terimler, diğer kullanıcıların aradığı ve kullanıcının amaçladığı önerilen sorguları temel alır.

Genellikle kullanıcı arama kutusuna her yeni karakter yazdığında bu API’yi çağırırsınız. Sorgu dizesinin eksiksiz olması, API’nin döndürdüğü önerilen sorgu terimlerinin alakasını etkiler. Sorgu dizesi ne kadar eksiksizse, önerilen sorgu terimlerinin listesi de o kadar alakalıdır. Örneğin, API’nin *s* için döndürebileceği öneriler, *sailing dinghies* için döndürdüğü sorgulardan daha az alakalıdır.

## <a name="getting-suggested-search-terms"></a>Önerilen arama terimlerini alma

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

Her öneri, `displayText`, `query` ve `url` alanını içerir. `displayText` alanı, arama kutunuzun açılır listesini doldurmak için kullandığınız önerilen sorguyu içerir. Yanıtın içerdiği tüm önerileri, verilen sırada görüntülemeniz gerekir.

Aşağıda, önerilen sorgu terimlerini içeren açılır arama kutusunun bir örneği gösterilmektedir.

![Otomatik öneri açılır arama kutusu listesi](./media/cognitive-services-bing-autosuggest-api/bing-autosuggest-drop-down-list.PNG)

Kullanıcı, açılır listeden bir önerilen sorgu seçerse, `query` alanındaki sorgu terimini kullanarak [Bing Web Araması API](../bing-web-search/search-the-web.md)’sini çağırır ve sonuçları kendiniz görüntülersiniz. Veya bunun yerine kullanıcıyı Bing arama sonuçları sayfasına göndermek için `url` alanındaki URL’yi kullanabilirsiniz.

## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Sonraki adımlar

İlk isteğinizi hızlı bir şekilde başlatmak için bkz. [İlk Sorgunuzu Yapma](quickstarts/csharp.md).

[Bing Otomatik Öneri API’si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference) başvurusunu inceleyin. Başvuruda, önerilen sorgu terimlerini istemek için kullanacağınız uç noktaların, üst bilgilerin ve sorgu parametrelerinin listesinin yanı sıra yanıt nesnelerinin tanımları yer alır.

[Bing Web Araması API](../bing-web-search/search-the-web.md)’sini kullanarak web’de arama yapmayı öğrenin.

Arama sonuçlarını kullanma kurallarına uygun hareket ettiğinizden emin olmak için [Bing Kullanım ve Görüntüleme Gereksinimleri](./useanddisplayrequirements.md)'ni okumayı unutmayın.
