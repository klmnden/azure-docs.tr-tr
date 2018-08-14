---
title: Bing resim arama API'si ile Web'den görüntüleri alma | Microsoft Docs
description: Web'den ilgili görüntü alma ve Bing resim arama API'si aramak için kullanın.
services: cognitive-services
author: aahill
manager: cgronlun
ms.assetid: AB1B9898-C94A-4B59-91A1-8623C94BA3D4
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 8/8/2018
ms.author: aahi
ms.openlocfilehash: c379cc2dfbe53f83e4d79500cd947879407c8d55
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "40082616"
---
# <a name="getting-images-from-the-web-with-the-bing-image-search-api"></a>Bing resim arama API'si ile Web'den görüntüsü alma

Bing resim arama REST API'si kullanarak, aşağıdaki GET isteği göndererek kullanıcının arama terimi için ilgili Web görüntülerini alabilirsiniz:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

> [!IMPORTANT]
> * Tüm istekleri bir sunucudan ve bir istemciden yapılması gerekir.
> * Tüm Bing arama API'leri çağırma ilk kez ise, istemci kimliği üst bilgisi dahil değildir. Yalnızca kullanıcı ve cihaz bileşimi için bir istemci kimliği döndürülen bir Bing API'si daha önce çağrılırsa istemci Kimliğini içerir.
> * Görüntüleri yanıtta verildikleri sırada görüntülenmesi gerekir.

## <a name="getting-images-from-a-specific-web-domain"></a>Belirli web etki alanından görüntüsü alma

Belirli bir etki alanındaki görüntüleri almak için [site:](http://msdn.microsoft.com/library/ff795613.aspx) dize işlecini kullanın.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

> [!NOTE]
> Sorguları kullanarak yanıtlarını `site:` işleci bağımsız olarak, yetişkinlere yönelik içerik içerebilir [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch) ayarı. Yalnızca `site:` etki alanındaki içeriği farkında olması durumunda.

Aşağıdaki örnekte Bing'in geçen hafta keşfettiği ContosoSailing.com sitesindeki küçük görüntülerin nasıl alınacağı gösterilmektedir.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="filtering-images"></a>Görüntüleri filtreleme

 Varsayılan olarak, resim arama API'si sorguya uygun olan tüm görüntüleri döndürür. (Örneğin, yalnızca bir transparant arka plan ya da belirli bir boyutu ile görüntüleri döndürülecek) Bing döndürür görüntüleri filtrelemek istiyorsanız, aşağıdaki sorgu parametreleri kullanabilirsiniz:

* [aspect](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#aspect): Görüntüleri en boy oranına göre filtreleyin (standart veya geniş ekran görüntüler gibi)
* [color](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#color): Görüntüleri baskın renge göre veya siyah-beyaz olarak filtreleyin
* [freshness](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#freshness): Görüntüleri yaşa göre filtreleyin (Bing'in geçen hafta keşfettiği görüntüler gibi)
* [height](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#height), [width](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#width): Görüntüleri genişliğe ve yüksekliğe göre filtreleyin
* [imageContent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagecontent): Görüntüleri içeriğe göre filtreleyin (yalnızca bir kişinin yüzünü gösteren görüntüler gibi)
* [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype): Görüntüleri türe göre filtreleyin (küçük resim, animasyonlu GIF veya saydam arka plan gibi)
* [license](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#license): Görüntüleri sitenin sahip olduğu lisans türüne göre filtreleyin
* [size](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#size): Görüntüleri boyuta göre filtreleyin, 200x200 piksele kadar olan küçük görüntüler gibi

Belirli bir etki alanındaki görüntüleri almak için [site:](http://msdn.microsoft.com/library/ff795613.aspx) dize işlecini kullanın.

    > [!NOTE]
    > Responses to queries using the `site:` operator may include adult content regardless of the [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch) setting. Only use `site:` if you are aware of the content on the domain.

Aşağıdaki örnekte Bing'in geçen hafta keşfettiği ContosoSailing.com sitesindeki küçük görüntülerin nasıl alınacağı gösterilmektedir.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="bing-image-search-response-format"></a>Bing resim arama yanıt biçimi

Bing yanıt iletisini içeren bir [görüntüleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) Azure bilişsel hizmetler sorgu ile ilgili belirlenen görüntülerin bir listesini içeren bir yanıt. Her [görüntü](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) Nesne listesinde görüntü ile ilgili aşağıdaki bilgileri içerir: URL, boyutunu, Boyutlar, kodlama biçimi, görüntü ve küçük 's boyutları bir küçük resim URL'si.

```json
{
    "name": "Rich Passage Sailing Dinghy",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
    "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OIP.GNarK7m...",
    "datePublished": "2011-10-29T11:26:00",
    "contentUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3...",
    "hostPageUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3687...",
    "contentSize": "79239 B",
    "encodingFormat": "jpeg",
    "hostPageDisplayUrl": "en.contoso.org\/wiki\/File:Rich_Passage...",
    "width": 526,
    "height": 688,
    "thumbnail": {
        "width": 229,
        "height": 300
    },
    "imageInsightsToken": "ccid_GNarK7ma*mid_CCF85447ADA6...",
    "insightsSourcesSummary": {
        "shoppingSourcesCount": 0,
        "recipeSourcesCount": 0
    },
    "imageId": "CCF85447ADA6FFF9E96E7DF0B796F7A86E34593",
    "accentColor": "376094"
},
```

Bing Resim Arama API'sini çağırdığınızda Bing, sonuç listesini döndürür. Bu liste sorguyla ilgili tüm sonuçların alt kümesidir. Yanıtın `totalEstimatedMatches` alanı, görüntülenebilecek tahmini görüntü sayısını içerir. Kalan görüntüleri sayfalama hakkında ayrıntılı bilgi için bkz. [Görüntüleri Sayfalama](../paging-images.md).

## <a name="next-steps"></a>Sonraki adımlar

Bing resim arama API'si önce denemediniz deneyin bir [hızlı](../quickstarts/csharp.md). Daha karmaşık bir şey için arıyorsanız, oluşturmak için öğreticiyi deneyin bir [tek sayfa uygulaması](../tutorial-bing-image-search-single-page-app.md).
