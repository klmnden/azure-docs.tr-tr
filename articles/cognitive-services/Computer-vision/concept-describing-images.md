---
title: Görüntüleri - görüntü işleme açıklayan
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'si görüntü açıklama özelliğiyle ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 933c4e4a6731a492a5aa461de7c782af422fb6fb
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55162710"
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
