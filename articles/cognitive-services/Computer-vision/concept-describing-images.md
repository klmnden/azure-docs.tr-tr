---
title: Görüntüleri - görüntü işleme açıklayan
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'si görüntü açıklama özelliğiyle ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 7919a84ffe948c9b6a8f68fc1372f1976c09bc79
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864290"
---
# <a name="describe-images-with-human-readable-language"></a>Kullanıcı tarafından okunabilen bir dil ile görüntü açıklayın

Görüntü işleme'nın algoritmaları, görüntü içeriği analiz edin. Bu analiz, tam tümceler halinde insan tarafından okunabilir dilde görüntülenen bir 'açıklamanın' temelini oluşturur. Açıklama, görüntüde nelerin bulunduğunu özetler. Görüntü işleme'nın algoritmaları görüntüde tanımlanmış görsel özelliklere bağlı olarak çeşitli açıklamaları oluşturur. Her açıklaması değerlendirilir ve bir güven puanı oluşturulur. Ardından, güvenilirlik puanı için azalan düzende sıralı bir liste döndürülür.

## <a name="image-description-example"></a>Resim Açıklama örneği

Görüntü işleme örnek görüntünün görsel özelliklerini anlatırken döndürür aşağıdaki JSON yanıtı gösterir.

![Siyah beyaz Manhattan stockholm'deki resmi](./Images/bw_buildings.png)

```json
{
    "description": {
        "tags": ["outdoor", "building", "photo", "city", "white", "black", "large", "sitting", "old", "water", "skyscraper", "many", "boat", "river", "group", "street", "people", "field", "tall", "bird", "standing"],
        "captions": [
            {
                "text": "a black and white photo of a city",
                "confidence": 0.95301952483304808
            },
            {
                "text": "a black and white photo of a large city",
                "confidence": 0.94085190563213816
            },
            {
                "text": "a large white building in a city",
                "confidence": 0.93108362931954824
            }
        ]
    },
    "requestId": "b20bfc83-fb25-4b8d-a3f8-b2a1f084b159",
    "metadata": {
        "height": 300,
        "width": 239,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [görüntüleri etiketleme](concept-tagging-images.md) ve [görüntüleri kategorilendirme](concept-categorizing-images.md).
