---
title: Bing Resim Arama nedir? | Microsoft Docs
description: Web'de görüntü aramak için Bing Resim Arama API'sinin nasıl kullanılacağını öğrenin.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 1446AD8B-A685-4F5F-B4AA-74C8E9A40BE9
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: overview
ms.date: 10/11/2017
ms.author: scottwhi
ms.openlocfilehash: 457707065059b5cb83c9d2b81f5d073cf1c89b84
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36336528"
---
# <a name="what-is-bing-image-search"></a>Bing Resim Arama nedir?

[Bing Resimler](https://www.bing.com/images)'e benzer bir deneyim sunan Bing Resim Arama API'si, Bing'e kullanıcı arama sorgusu gönderip yanıt olarak ilgili görüntülerin listesini almanızı sağlar.

Kullanıcının arama sorgusuyla ilgili görüntüleri bulmak için yalnızca görüntü içeren bir arama sonuçları sayfası oluşturuyorsanız daha genel [Web Araması API'si](../bing-web-search/search-the-web.md) yerine bu API'yi çağırın. Görüntülerin yanı sıra web sayfaları, haberler ve videolar gibi diğer içerik türlerini de istiyorsanız Web Araması API'sini çağırın.

## <a name="suggesting--using-search-terms"></a>Arama terimlerini önerme ve kullanma

Kullanıcıların arama terimlerini gireceği bir arama kutusu sağlıyorsanız deneyimi geliştirmek için [Bing Otomatik Öneri API'sini](../bing-autosuggest/get-suggested-search-terms.md) kullanın. API, kullanıcı yazarken kısmi arama terimlerine dayalı önerilen sorgu dizelerini yönetin.

Kullanıcı arama terimini girdikten sonra [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query) sorgu parametresini ayarlamadan önce terimi URL ile kodlayın. Örneğin kullanıcı *sailing dinghies* terimini girerse `q` öğesini `sailing+dinghies` veya `sailing%20dinghies` olarak ayarlayın.

## <a name="getting-images"></a>Görüntüleri alma

Web'den kullanıcının arama terimiyle ilgili görüntüleri almak için aşağıdaki GET isteğini gönderin:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Tüm isteklerin bir sunucudan yapılması gerekir. İstemciden çağrı yapamazsınız.

Bing API'lerinden birini ilk kez çağırıyorsanız istemci kimliği üst bilgisini eklemeyin. İstemci kimliğini yalnızca önceden bir Bing API'sini çağırdıysanız ve Bing, kullanıcı ve cihaz birleşimi için bir istemci kimliği döndürdüyse dahil edin.

Belirli bir etki alanındaki görüntüleri almak için [site:](http://msdn.microsoft.com/library/ff795613.aspx) dize işlecini kullanın.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

Yanıt, Bing'in sorguyla ilişkili olduğunu düşündüğü görüntülerin bir listesini içeren [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) yanıtı içerir. Listedeki her [Image](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) nesnesi görüntünün URL'sini, dosya boyutunu, boyutlarını ve kodlama biçimini içerir. Görüntü nesnesi aynı zamanda görüntü küçük resminin URL'sini ve küçük resmin boyutlarını içerir.

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

Görüntülerin küçük resimlerinden oluşan bir kolaj veya küçük resimlerin alt kümesini görüntüleyebilirsiniz. Alt küme görüntülerseniz kullanıcıya kalan resimleri görüntüleme seçeneği sunun. Görüntüleri yanıtta iletilen sırayla görüntülemeniz gerekir.

Küçük resimlerin, kullanıcı imleci üzerine getirdiğinde büyütülmesini de sağlayabilirsiniz. Büyütmeniz halinde görüntüye bağlam sağlamayı unutmayın. Örneğin [hostPageDisplayUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-hostpagedisplayurl) parametresindeki ana bilgisayarı ayıklayıp görüntünün altına ekleyebilirsiniz. Küçük resmi yeniden boyutlandırma hakkında bilgi için bkz. [Küçük Resimleri Yeniden Boyutlandırma ve Kırpma](./resize-and-crop-thumbnails.md).

<!-- Removing image until we can replace it with a sanatized version.
![Expanded view of thumbnail image](../bing-web-search/media/cognitive-services-bing-web-api/bing-web-image-thumbnail-expansion.PNG)
-->

Kullanıcı küçük resme tıklarsa [contentUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-contenturl) alanını kullanarak tam boyutlu görüntüyü kullanıcıya gösterebilirsiniz. Görüntüye bağlam sağlamayı unutmayın.

`shoppingSourcesCount` veya `recipeSourcesCount` sıfırdan büyükse görüntüdeki öğe için alışveriş veya tarif seçeneklerinin mevcut olduğunu belirtmek için küçük resme alışveriş sepeti gibi simgeler ekleyin.

<!-- this is a sanitized version but we're removing all images for now until everything is sanitized.
![Shopping sources badge](./media/cognitive-services-bing-images-api/bing-images-shopping-source.PNG)
-->

Görüntüyü içeren web sayfaları veya görüntüde tanınan kişiler gibi görüntü hakkında içgörüler için [imageInsightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-imageinsightstoken) belirtecini kullanın. Ayrıntılar için bkz. [Görüntü İçgörüleri](./image-insights.md).

## <a name="filtering-images"></a>Görüntüleri filtreleme

 Resim Arama API'si varsayılan olarak sorguyla ilgili tüm görüntüleri döndürür. Yalnızca saydam arka plana sahip veya belirli bir boyutta olan görüntüleri almak istiyorsanız aşağıdaki sorgu parametrelerini kullanarak Bing'in döndürdüğü görüntüleri filtreleyebilirsiniz.  

- [aspect](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#aspect): Görüntüleri en boy oranına göre filtreleyin (standart veya geniş ekran görüntüler gibi)
- [color](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#color): Görüntüleri baskın renge göre veya siyah-beyaz olarak filtreleyin
- [freshness](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#freshness): Görüntüleri yaşa göre filtreleyin (Bing'in geçen hafta keşfettiği görüntüler gibi)
- [height](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#height), [width](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#width): Görüntüleri genişliğe ve yüksekliğe göre filtreleyin
- [imageContent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagecontent): Görüntüleri içeriğe göre filtreleyin (yalnızca bir kişinin yüzünü gösteren görüntüler gibi)
- [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype): Görüntüleri türe göre filtreleyin (küçük resim, animasyonlu GIF veya saydam arka plan gibi)
- [license](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#license): Görüntüleri sitenin sahip olduğu lisans türüne göre filtreleyin
- [size](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#size): Görüntüleri boyuta göre filtreleyin, 200x200 piksele kadar olan küçük görüntüler gibi

Belirli bir etki alanındaki görüntüleri almak için [site:](http://msdn.microsoft.com/library/ff795613.aspx) dize işlecini kullanın.

> [!NOTE]
> Sorguya bağlı olarak `site:` işlecini kullanmanız halinde [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch) ayarından bağımsız olarak yanıtta yetişkinlere yönelik içerik bulunabilir. `site:` işlecini yalnızca sitenin içeriği hakkında bilgi sahibiyseniz ve senaryonuz, yetişkinlere yönelik içeriğin mevcut olma ihtimalini destekliyorsa kullanın.

Aşağıdaki örnekte Bing'in geçen hafta keşfettiği ContosoSailing.com sitesindeki küçük görüntülerin nasıl alınacağı gösterilmektedir.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="pivoting-the-query"></a>Sorguyu özetleme

Bing özgün arama sorgusunu parçalara ayırabiliyorsa [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) nesnesi `pivotSuggestions` alanını içerir. Örneğin, özgün sorgu *Microsoft Surface* ise Bing bunu *Microsoft* ve *Surface* olmak üzere ikiye bölebilir.  

Aşağıdaki örnekte *Microsoft Surface* için öneri özetleri gösterilmektedir.  

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface&FORM=OIIARP",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions": [...],
    "pivotSuggestions": [{
        "pivot": "microsoft",
        "suggestions": [{
            "text": "Contoso Surface",
            "displayText": "Contoso",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=OtterBox+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Contoso...",
                    "searchLink": "https:\/\/api.cognitive.microsoft.com\/api...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Contoso+Surface..."
            }
        },
        {
            "text": "Adatum Surface",
            "displayText": "Adatum",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Adatum+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Adatum+Surface&pid=Ap..."
            }
        },
        ...
        ]
    },
    {
        "pivot": "surface",
        "suggestions": [{
            "text": "Microsoft Surface4",
            "displayText": "Surface4",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface...",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft..."
            }
        },
        {
            "text": "Microsoft Tablet",
            "displayText": "Tablet",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Tablet&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Microsoft+Tablet..."
            }
        },
        ...
    ],
    "nextOffsetAddCount": 0
}
```

Daha sonra kullanıcıya ilgilenme ihtimali olan isteğe bağlı sorgu terimlerini görüntüleyebilirsiniz.

`pivotSuggestions` alanında özgün sorgunun ayrıldığı parçaların (özetler) listesi bulunur. Her bir özet terim için gelen yanıtta önerilen sorguları içeren [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query_obj) nesnelerinin listesi yer alır. `text` alanı önerilen sorguyu, `displayText` alanı ise özgün sorguda özetin yerine kullanılan terimi içerir. Örneğin, Surface Çıkış Tarihi.

`text` ve `thumbnail` alanlarını kullanarak özet sorgu dizelerini kullanıcıya gösterebilir ve kullanıcının bu özet sorgu dizelerinden seçim yapmasını sağlayabilirsiniz. `webSearchUrl` URL veya `searchLink` URL kullanarak küçük resmi ve metni tıklanabilir hale getirebilirsiniz. `webSearchUrl` kullanarak kullanıcıyı Bing arama sonuçlarına gönderebilir veya `searchLink` ile kendi sonuç sayfanızı sunabilirsiniz.

<!-- Need a sanitized version of the image
The following shows an example of the pivot queries.

![Pivot suggestions](./media/cognitive-services-bing-images-api/bing-image-pivotsuggestion.GIF)
-->

## <a name="expanding-the-query"></a>Sorguyu genişletme

Bing özgün aramayı daraltmak için sorguyu genişletebiliyorsa [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) nesnesi `queryExpansions` alanını içerir. Örneğin sorgu *Microsoft Surface* ise genişletilmiş sorgular şunlar olabilir: Microsoft Surface **Pro 3**, Microsoft Surface **RT**, Microsoft Surface **Phone** ve Microsoft Surface **Hub**.

Aşağıdaki örnekte *Microsoft Surface* için genişletilmiş sorgular gösterilmektedir.

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface...",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions":  [{
        "text": "Microsoft Surface Pro 3",
        "displayText": "Pro 3",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Pro+3...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Microsoft...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Pro+3..."
        }
    },
    {
        "text": "Microsoft Surface RT",
        "displayText": "RT",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+RT...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+RT..."
        }
    },
    {
        "text": "Microsoft Surface Phone",
        "displayText": "Phone",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Phone",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Phone..."
        }
    }],
    "pivotSuggestions": [...],
    "nextOffsetAddCount": 0
}
```

`queryExpansions` alanı [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query_obj) nesnelerinin listesini içerir. `text` alanı genişletilmiş sorguyu, `displayText` alanı ise genişletme terimini içerir. `text` ve `thumbnail` alanlarını kullanarak genişletilmiş sorgu dizelerini kullanıcıya gösterebilir ve kullanıcının bu genişletilmiş sorgu dizelerinden seçim yapmasını sağlayabilirsiniz. `webSearchUrl` URL veya `searchLink` URL kullanarak küçük resmi ve metni tıklanabilir hale getirebilirsiniz. `webSearchUrl` kullanarak kullanıcıyı Bing arama sonuçlarına gönderebilir veya `searchLink` ile kendi sonuç sayfanızı sunabilirsiniz.

<!-- Removing until we can replace with a sanitized image.
The following shows an example Bing implementation that uses expanded queries. If the user clicks the Microsoft Surface Pro 3 link, they're taken to the Bing search results page, which shows them images of the Pro 3.

![Query expansion suggestions](./media/cognitive-services-bing-images-api/bing-image-queryexpansion.GIF)
-->

## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Sonraki adımlar

İlk isteğinizi hızlı bir şekilde başlatmak için bkz. [C# hızlı başlangıç](quickstarts/csharp.md).

[Bing Resim Arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) başvurusunu inceleyin. Başvuruda arama sonuçlarını istemek için kullanabileceğiniz uç noktaların, üst bilgilerin ve sorgu parametrelerinin listesi yer alır. Ayrıca yanıt nesnelerinin tanımları da bulunur.

Arama kutusu kullanıcı deneyiminizi geliştirmek için bkz. [Bing Otomatik Öneri API'si](../bing-autosuggest/get-suggested-search-terms.md). Kullanıcı sorgu terimini girerken bu API'yi çağırarak başkaları tarafından kullanılan ilgili sorgu terimlerini alabilirsiniz.

Arama sonuçlarını kullanma kurallarına uygun hareket ettiğinizden emin olmak için [Bing Kullanım ve Görüntüleme Gereksinimleri](./useanddisplayrequirements.md)'ni okumayı unutmayın.

Bing Resim Arama API'sini çağırdığınızda Bing, sonuç listesini döndürür. Bu liste sorguyla ilgili tüm sonuçların alt kümesidir. Yanıtın `totalEstimatedMatches` alanı, görüntülenebilecek tahmini görüntü sayısını içerir. Kalan görüntüleri sayfalama hakkında ayrıntılı bilgi için bkz. [Görüntüleri Sayfalama](./paging-images.md).