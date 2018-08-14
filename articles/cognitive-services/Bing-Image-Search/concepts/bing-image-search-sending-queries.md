---
title: Bing resim arama API'si için sorguları gönderme | Microsoft Docs
description: Gönderme ve Bing resim arama API'si için gönderilen arama sorguları özelleştirme hakkında bilgi edinin.
services: cognitive-services
author: aahill
manager: cgronlun
ms.assetid: C2862E98-8BCC-423B-9C4A-AC79A287BE38
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 8/8/2018
ms.author: aahi
ms.openlocfilehash: d74f59ffcf095e639686a3ada3b09dac988fc544
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "40082617"
---
# <a name="sending-queries-to-the-bing-image-search-api"></a>Bing resim arama API'si için sorguları gönderme

Bing resim arama API'si, Bing için bir kullanıcı arama sorgusu gönderin ve ilgili görüntülerin listesini dönmek vererek Bing.com/Images için benzer bir deneyim sağlar.

## <a name="using-and-suggesting-search-terms"></a>Kullanarak ve arama terimlerini önerme

Bir arama terimi girildikten sonra URL-kodlama ayarlamadan önce terimi [ **q** ](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query) sorgu parametresi. Örneğin, kullanıcının girdiği *Yelkenli dinghies*ayarlayın `q` için `sailing+dinghies` veya `sailing%20dinghies`.

Uygulamanızın arama kutusuna arama terimlerini girmiş varsa kullanabileceğiniz [Bing otomatik öneri API'si](../../bing-autosuggest/get-suggested-search-terms.md) gerçek zamanlı olarak önerilen arama terimlerini görüntüleyerek deneyimini iyileştirmek üzere. API, önerilen sorgu dizeleri kısmi arama terimlerini ve Azure bilişsel hizmetler göre döndürür.

## <a name="pivoting-the-query"></a>Sorguyu özetleme

Bing özgün arama sorgusu, döndürülen segmente yerleştirebilir [görüntüleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) nesne içerecek `pivotSuggestions` hangi görüntülenebilir isteğe bağlı arama terimleri kullanıcı olarak. Örneğin, özgün sorgu *Microsoft Surface*, Bing, sorguyu segmentlere *Microsoft* ve *Surface* ve her biri için önerilen özetleri sağlar. Bu kullanıcı için isteğe bağlı bir sorgu terimleriyle olarak görüntülenebilir.

Aşağıdaki örnek, pivot önerileri gösterir *Microsoft Surface*:  

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

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bing resim arama API'si önce denemediniz deneyin bir [hızlı](../quickstarts/csharp.md). Daha karmaşık bir şey için arıyorsanız, oluşturmak için öğreticiyi deneyin bir [tek sayfa uygulaması](../tutorial-bing-image-search-single-page-app.md).
