---
title: Görüntüleri açıklayan
titleSuffix: Computer Vision - Cognitive Services - Azure
description: Azure Bilişsel hizmetler görüntü işleme kullanarak görüntüleri tanımlamaya ilgili kavramları.
services: cognitive-services
author: deken
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: be59055a2c6cd1366c8c52370fa97158ab8d6c88
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44725471"
---
# <a name="describing-images"></a>Görüntüleri açıklayan

Görüntü işleme'nın algoritmaları, görüntü içeriği analiz edin. Bu analiz, tam cümlelerden okunabilir dili olarak görüntülenen bir 'description' için temel oluşturur. Açıklama, görüntüde bulunan özetler. Görüntü işleme'nın algoritmaları görüntüde tanımlanmış görsel özelliklere bağlı olarak çeşitli açıklamaları oluşturur. Açıklamaların her biri değerlendirilir ve bir güvenilirlik puanı oluşturulur. Ardından, güvenilirlik puanı için azalan düzende sıralı bir liste döndürülür.

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

Resim yazıları oluşturmak için bu teknolojiyi kullanan bir robotun örneği bulunabilir [burada](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption).  

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [görüntüleri etiketleme](concept-tagging-images.md) ve [görüntüleri kategorilendirme](concept-categorizing-images.md).