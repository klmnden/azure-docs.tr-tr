---
title: Görüntüleri - görüntü işleme etiketleme
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'sini kullanarak görüntüleri etiketleme için ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 7e9181b6e805a768ca5dd8ed558b8feea45f22d3
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52960916"
---
# <a name="tagging-images"></a>Resimleri etiketleme

Görüntü işleme tanınabilir nesne, canlı, manzara ve Eylemler binlerce alan etiketler döndürür. Belirsiz veya herkesçe bilinmeyen etiketler söz konusu olduğunda, API yanıtı, etiketin anlamının bilinen bir ortama ilişkin bağlamda açıklığa kavuşturulması için "ipuçları" sağlar. Etiketler taksonomi olarak tanınmaz ve hiçbir devralma hiyerarşisi yoktur. Bir içerik etiketi koleksiyonu, tam tümceler halinde biçimlendirilmiş insan tarafından okunabilir dilde görüntülenen bir görüntü 'açıklamasının' temelini oluşturur. Şu noktada görüntü açıklaması için desteklenen tek dilin İngilizce olduğunu unutmayın.

Görüntü işleme algoritmaları, bir görüntü yüklemek veya bir resim URL'si belirtme sonra nesneleri, canlı ve Eylemler görüntüde tanımlanmış temel alan etiketler çıktı. Etiketleme yalnızca temel konu ile sınırlı kalmayıp ortam (iç mekân veya dış mekân), mobilyalar, aletler, bitkiler, hayvanlar, aksesuarlar, araçlar ve benzeri öğeleri de kapsar.

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