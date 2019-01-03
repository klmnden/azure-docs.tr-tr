---
title: Resim türleri - görüntü işleme algılama
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'si, resim türü algılama özellikle ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 04062d5625126712c5f14c41d610d55caf4c28b5
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53583111"
---
# <a name="detecting-image-types-with-computer-vision"></a>Görüntü işleme olan algılama resim türleri

Görüntü işleme görüntüleri içerik türü tarafından bir görüntüyü küçük resim olup olmadığını belirten bir ölçeği veya çizim olasılığını derecelendirme çözümleyebilirsiniz.

## <a name="detecting-clip-art"></a>Küçük resim algılama

Görüntü işleme, görüntüye inceler ve aşağıdaki tabloda açıklandığı gibi küçük resim 0 ile 3 bir ölçekte sonra görüntünün olasılığını derecelendirir.

| Değer | Anlamı |
|-------|---------|
| 0 | Küçük resim değil |
| 1 | belirsiz |
| 2 | Normal küçük resim |
| 3 | iyi küçük resim |

### <a name="clip-art-detection-examples"></a>Küçük resim algılama örnekleri

Aşağıdaki JSON yanıtları gösterir görüntü işleme, küçük resim olan örnek görüntüleri olasılığını derecelendirme döndürür.

![Küçük resim görüntüsünü dilimin peynirlerine ayırıyor](./Images/cheese_clipart.png)

```json
{
    "imageType": {
        "clipArtType": 3,
        "lineDrawingType": 0
    },
    "requestId": "88c48d8c-80f3-449f-878f-6947f3b35a27",
    "metadata": {
        "height": 225,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Mavi bir ev ve ön yard](./Images/house_yard.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "a9c8490a-2740-4e04-923b-e8f4830d0e47",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="detecting-line-drawings"></a>Çizimlerde algılama

Görüntü işleme, görüntüye inceler ve resmin çizim olup olmadığını gösteren bir Boole değeri döndürür.

### <a name="line-drawing-detection-examples"></a>Çizim algılama örnekleri

Aşağıdaki JSON yanıtları gösterir görüntü işleme örnek görüntüleri çizimlerde olup olmadığını gösteren zaman döndürür.

![Bir lion çizim görüntüsü](./Images/lion_drawing.png)

```json
{
    "imageType": {
        "clipArtType": 2,
        "lineDrawingType": 1
    },
    "requestId": "6442dc22-476a-41c4-aa3d-9ceb15172f01",
    "metadata": {
        "height": 268,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Yeşil bir arka plan beyaz çiçek](./Images/flower.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "98437d65-1b05-4ab7-b439-7098b5dfdcbf",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [görüntüleri etiketleme](concept-tagging-images.md) ve [görüntüleri kategorilendirme](concept-categorizing-images.md).