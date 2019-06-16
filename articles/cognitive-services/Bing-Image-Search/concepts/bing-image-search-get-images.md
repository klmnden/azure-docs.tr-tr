---
title: Web - Bing resim arama API'si görüntü alma
titleSuffix: Azure Cognitive Services
description: Bing resim arama API'si için arama yapın ve ilgili görüntüleri Web'den almak için kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: AB1B9898-C94A-4B59-91A1-8623C94BA3D4
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: f169f969a1acf4cefc8cee27f74a99730491176a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66389424"
---
# <a name="get-images-from-the-web-with-the-bing-image-search-api"></a>Bing resim arama API'si ile web görüntülerini alın

Bing resim arama REST API'si kullandığınızda, aşağıdaki GET isteği göndererek arama teriminizi ilgili Web görüntülerini alabilirsiniz:

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

## <a name="get-images-from-a-specific-web-domain"></a>Belirli web etki alanından görüntü alma

Belirli bir etki alanındaki görüntüleri almak için [site:](https://msdn.microsoft.com/library/ff795613.aspx) dize işlecini kullanın.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

> [!NOTE]
> Sorguları kullanarak yanıtlarını `site:` işleci bağımsız olarak, yetişkinlere yönelik içerik içerebilir [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#safesearch) ayarı. Yalnızca `site:` etki alanındaki içeriği farkında değilseniz.

Aşağıdaki örnekte Bing'in geçen hafta keşfettiği ContosoSailing.com sitesindeki küçük görüntülerin nasıl alınacağı gösterilmektedir.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="filter-images"></a>Görüntüleri Filtrele

 Varsayılan olarak, resim arama API'si, sorgu ile ilgili tüm görüntüleri döndürür. Bing (örneğin, bir saydam arka plan ya da belirli bir boyutu ile yalnızca görüntü döndürülecek) döndüren görüntüleri filtrelemek istiyorsanız, aşağıdaki sorgu parametrelerini kullanın:

* [en boy](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#aspect)— en boy oranı (örneğin, standart veya geniş ekran görüntüleri için) tarafından görüntüleri Filtrele.
* [renk](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#color)— filtre baskın renk veya siyah beyaz görüntüler.
* [güncellik](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#freshness)— yaş (örneğin, Bing tarafından geçen hafta içinde bulunan görüntüleri) tarafından görüntüleri Filtrele.
* [Yükseklik](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#height), [genişliği](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#width)— filtre görüntülerden genişlik ve yükseklik.
* [imageContent](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagecontent)— tarafından içerik (örneğin, yalnızca bir kişinin yüzü gösteren görüntüleri) görüntüleri Filtrele.
* [ImageType](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagetype)— filtre (örneğin, küçük resim, animasyonlu GIF veya saydam arka planlar) türüne göre görüntüler.
* [Lisans](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#license)— görüntüleri sitesiyle ilişkili lisans türüne göre filtreleyin.
* [boyutu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#size)— filtre gibi küçük bir boyuta göre görüntüleri en fazla 200 x 200 piksel.

Belirli bir etki alanındaki görüntüleri almak için [site:](https://msdn.microsoft.com/library/ff795613.aspx) dize işlecini kullanın.

 > [!NOTE]
 > Sorguları kullanarak yanıtlarını `site:` işleci bağımsız olarak, yetişkinlere yönelik içerik içerebilir [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#safesearch) ayarı. Yalnızca `site:` etki alanındaki içeriği farkında değilseniz.

Aşağıdaki örnek, Bing geçen hafta içinde bulunan ContosoSailing.com küçük resimler elde gösterilmektedir.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="bing-image-search-response-format"></a>Bing resim arama yanıt biçimi

Bing yanıt iletisini içeren bir [görüntüleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) Bilişsel hizmetler sorgu ile ilgili belirlenen görüntülerin bir listesini içeren bir yanıt. Her [görüntü](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#image) Nesne listesinde görüntü ile ilgili aşağıdaki bilgileri içerir: URL, boyutunu, Boyutlar, kodlama biçimi, görüntü ve küçük 's boyutları bir küçük resim URL'si.

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

Bing Resim Arama API'sini çağırdığınızda Bing, sonuç listesini döndürür. Bu liste sorguyla ilgili tüm sonuçların alt kümesidir. Yanıtın `totalEstimatedMatches` alanı, görüntülenebilecek tahmini görüntü sayısını içerir. Görüntüleri kalanında sayfasına hakkında daha fazla ayrıntı için bkz. [sayfalama görüntüleri](../paging-images.md).

## <a name="next-steps"></a>Sonraki adımlar

Bing resim arama API'si önce denemediniz deneyin bir [hızlı](../quickstarts/csharp.md). Daha karmaşık bir şey için arıyorsanız, oluşturmak için öğreticiyi deneyin bir [tek sayfa uygulaması](../tutorial-bing-image-search-single-page-app.md).
