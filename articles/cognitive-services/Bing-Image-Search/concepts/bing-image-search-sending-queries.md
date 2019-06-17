---
title: Bing resim arama API'si - görüntü sorguları gönderme
titleSuffix: Azure Cognitive Services
description: Bing resim arama API'si için gönderdiğiniz arama sorguları özelleştirme hakkında bilgi edinin.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: C2862E98-8BCC-423B-9C4A-AC79A287BE38
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 32ced1d06a10f33e9d71ef09ba51d22e9e406f73
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66384393"
---
# <a name="send-queries-to-the-bing-image-search-api"></a>Bing resim arama API'si için sorguları gönderme

Bing resim arama API'si için Bing.com/Images benzer bir deneyim sağlar. Bing arama sorgusu gönderin ve geri ilgili görüntülerin listesini almak için kullanabilirsiniz.

## <a name="use-and-suggest-search-terms"></a>Kullanın ve arama terimlerini önerin

URL kodlaması terim, önce bir arama terimi girildikten sonra kümesi [ **q** ](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query) sorgu parametresi. Örneğin, girerseniz *Yelkenli dinghies*ayarlayın `q` için `sailing+dinghies` veya `sailing%20dinghies`.

Uygulamanızın arama kutusuna arama terimlerini girmiş varsa kullanabileceğiniz [Bing otomatik öneri API'si](../../bing-autosuggest/get-suggested-search-terms.md) deneyimini iyileştirmek üzere. API önerilen arama terimlerini gerçek zamanlı olarak görüntüleyebilirsiniz. API, kısmi arama terimlerini ve Bilişsel hizmetler göre önerilen sorgu dizelerini döndürür.

## <a name="pivot-the-query"></a>Sorgu Özet

Bing özgün arama sorgusu, döndürülen segmente yerleştirebilir [görüntüleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) nesnesini içeren `pivotSuggestions`. Pivot öneriler, isteğe bağlı arama terimleri kullanıcı olarak görüntülenebilir. Örneğin, özgün sorgu *Microsoft Surface*, Bing, sorguyu segmentlere *Microsoft* ve *Surface* ve her biri için önerilen özetleri sağlar. Bu öneriler, isteğe bağlı bir sorgu terimleri kullanıcı olarak görüntülenebilir.

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

`pivotSuggestions` alanında özgün sorgunun ayrıldığı parçaların (özetler) listesi bulunur. Her bir özet terim için gelen yanıtta önerilen sorguları içeren [Query](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query_obj) nesnelerinin listesi yer alır. `text` Alan önerilen sorgu içerir. `displayText` Alan özgün sorguyu pivot'ta değiştirir terim içerir. Yayın tarihi Surface'ı, buna bir örnektir.

Kullanıcı Arama Özet sorgu dizesi ise kullanın `text` ve `thumbnail` pivot görüntülenecek alanlar sorgu dizeleri. Kullanarak küçük resim ve metin tıklanabilir hale `webSearchUrl` URL veya `searchLink` URL'si. Kullanım `webSearchUrl` Bing arama sonuçları kullanıcıya gönderilecek. Kendi sonuçları sayfası sağlarsanız, kullanın `searchLink`.

<!-- Need a sanitized version of the image
The following shows an example of the pivot queries.

![Pivot suggestions](./media/cognitive-services-bing-images-api/bing-image-pivotsuggestion.GIF)
-->

## <a name="expand-the-query"></a>Sorguyu Genişlet

Bing özgün aramayı daraltmak için sorguyu genişletebiliyorsa [Images](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) nesnesi `queryExpansions` alanını içerir. Örneğin, sorgu *Microsoft Surface*, genişletilmiş sorgular olabilir:
- Microsoft Surface **Pro 3**.
- Microsoft Surface **RT**.
- Microsoft Surface **telefon**.
- Microsoft Surface **Hub**.

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

`queryExpansions` alanı [Query](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query_obj) nesnelerinin listesini içerir. `text` Alan genişletilmiş sorgu içerir. `displayText` Genişletme terimi alan içerir. Kullanıcı Arama genişletilmiş sorgu dizesidir kullanırsanız `text` ve `thumbnail` genişletilmiş sorgu dizelerini görüntülenecek alanları. Kullanarak küçük resim ve metin tıklanabilir hale `webSearchUrl` URL veya `searchLink` URL'si. Kullanım `webSearchUrl` Bing arama sonuçları kullanıcıya gönderilecek. Kendi sonuçları sayfası sağlarsanız, kullanın `searchLink`.

<!-- Removing until we can replace with a sanitized image.
The following shows an example Bing implementation that uses expanded queries. If the user clicks the Microsoft Surface Pro 3 link, they're taken to the Bing search results page, which shows them images of the Pro 3.

![Query expansion suggestions](./media/cognitive-services-bing-images-api/bing-image-queryexpansion.GIF)
-->


## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bing resim arama API'si önce denemediniz deneyin bir [hızlı](../quickstarts/csharp.md). Daha karmaşık bir şey için arıyorsanız, oluşturmak için öğreticiyi deneyin bir [tek sayfa uygulaması](../tutorial-bing-image-search-single-page-app.md).
