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
origin.date: 02/11/2019
ms.date: 02/27/2019
ms.author: v-junlch
ms.custom: seodec18
ms.openlocfilehash: 91618b211fdd869daf74491b175d6359ffa3f30c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60368394"
---
# <a name="describe-images-with-human-readable-language"></a>Kullanıcı tarafından okunabilen bir dil ile görüntü açıklayın

Görüntü işleme, bir resmi çözümleme ve içeriğini açıklayan insanlar tarafından okunabilen bir cümle oluşturur. Farklı görsel özellikleri ve her açıklaması retruns birkaç açıklamaları gerçekten göre algoritması bir güven puanı verilir. Son çıkış açıklamaları en yüksek öncelikten en düşük güven için sıralanmış bir listesidir.

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

<!-- Update_Description: wording update -->