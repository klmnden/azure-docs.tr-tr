---
title: Görüntüleri - görüntü işleme açıklayan
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'sini kullanarak görüntüleri tanımlamaya ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 423d1be57bc800108a08a81b72587ca2711bbc3d
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342424"
---
# <a name="describing-images"></a>Resimleri açıklama

Görüntü işleme'nın algoritmaları, görüntü içeriği analiz edin. Bu analiz, tam cümlelerden okunabilir dili olarak görüntülenen bir 'description' için temel oluşturur. Açıklama, görüntüde bulunan özetler. Görüntü işleme'nın algoritmaları görüntüde tanımlanmış görsel özelliklere bağlı olarak çeşitli açıklamaları oluşturur. Her açıklaması değerlendirilir ve bir güven puanı oluşturulur. Ardından, güvenilirlik puanı için azalan düzende sıralı bir liste döndürülür.

## <a name="image-description-example"></a>Resim Açıklama örneği

Görüntü işleme örnek görüntünün görsel özelliklerini anlatırken döndürür aşağıdaki JSON yanıtı gösterir.

![B & W binalar](./Images/bw_buildings.png)

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