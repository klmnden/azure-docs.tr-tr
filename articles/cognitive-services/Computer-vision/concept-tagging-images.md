---
title: Görüntüleri etiketleme
titleSuffix: Computer Vision - Cognitive Services - Azure
description: Azure Bilişsel hizmetler görüntü işleme kullanarak görüntüleri etiketleme için ilgili kavramları.
services: cognitive-services
author: deken
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: b06265bbdd5ba642c5395823e98a6a76171baff4
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44725538"
---
# <a name="tagging-images"></a>Görüntüleri etiketleme

Görüntü işleme, 2000'den fazla tanınabilir nesne, canlı, manzara ve Eylemler alan etiketler döndürür. Etiketlerin belirsiz olduğunda veya bilinmediği API yanıtı 'bilinen bir ayar bağlamında etiketin anlamını açıklamak için ipuçları' sağlar. Etiketleri bir sınıflandırma düzenlenmiş ve devralma hiyerarşi yok. İçerik etiket koleksiyonu, 'description' tam cümlelerden biçimlendirilmiş insan tarafından okunabilir dili olarak görüntülenen bir görüntü için temel oluşturur. Bu noktada İngilizce için görüntü açıklaması yalnızca desteklenen dil olduğunu unutmayın.

Görüntü işleme algoritmaları, bir görüntü yüklemek veya bir resim URL'si belirtme sonra nesneleri, canlı ve Eylemler görüntüde tanımlanmış temel alan etiketler çıktı. Etiketleme, ana konu, bir kişi, ön planda gibi sınırlı değildir, ancak ayrıca ayarı (iç veya dış) mobilyası, araçları, tesisin, hayvanlar, Donatılar, vb. araçları içerir.

## <a name="image-tagging-example"></a>Resim etiketleme örneği

Görüntü işleme algılandı örnek görüntüde visual özellikleri etiketlenirken döndürür aşağıdaki JSON yanıtı gösterilir.

![House_Yard](./Images/house_yard.png).

```json
{
    "tags": [
        {
            "name": "grass",
            "confidence": 0.9999995231628418
        },
        {
            "name": "outdoor",
            "confidence": 0.99992108345031738
        },
        {
            "name": "house",
            "confidence": 0.99685388803482056
        },
        {
            "name": "sky",
            "confidence": 0.99532157182693481
        },
        {
            "name": "building",
            "confidence": 0.99436837434768677
        },
        {
            "name": "tree",
            "confidence": 0.98880356550216675
        },
        {
            "name": "lawn",
            "confidence": 0.788884699344635
        },
        {
            "name": "green",
            "confidence": 0.71250593662261963
        },
        {
            "name": "residential",
            "confidence": 0.70859086513519287
        },
        {
            "name": "grassy",
            "confidence": 0.46624681353569031
        }
    ],
    "requestId": "06f39352-e445-42dc-96fb-0a1288ad9cf1",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [görüntüleri kategorilendirme](concept-categorizing-images.md) ve [görüntüleri açıklayan](concept-describing-images.md).