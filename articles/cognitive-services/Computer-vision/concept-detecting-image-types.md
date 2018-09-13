---
title: Algılama resim türleri
titleSuffix: Computer Vision - Cognitive Services - Azure
description: Azure Bilişsel hizmetler görüntü işleme kullanarak görüntü türlerini algılamak için ilgili kavramları.
services: cognitive-services
author: deken
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: 6c0280959e82eaa2da4927b48af7ebc28db25a7d
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44725569"
---
# <a name="detecting-image-types"></a>Algılama resim türleri

Görüntü işleme görüntüleri içerik türü tarafından bir görüntüyü küçük resim olup olmadığını belirten bir ölçeği veya çizim olasılığını derecelendirme çözümleyebilirsiniz.

## <a name="detecting-clip-art"></a>Küçük resim algılama

Görüntü işleme, görüntüye inceler ve aşağıdaki tabloda açıklandığı gibi küçük resim 0 ile 3 bir ölçekte sonra görüntünün olasılığını derecelendirir.

| Değer | Anlamı |
|-------|---------|
| 0 | Olmayan küçük resim |
| 1 | belirsiz |
| 2 | Normal küçük resim |
| 3 | iyi küçük resim |

### <a name="clip-art-detection-examples"></a>Küçük resim algılama örnekleri

Aşağıdaki JSON yanıtları gösterir görüntü işleme, küçük resim olan örnek görüntüleri olasılığını derecelendirme döndürür.

![Görüntü işleme peynirlerine ayırıyor küçük resim analiz edin](./Images/cheese_clipart.png)

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

![Görüntü merkezi Yard analiz edin](./Images/house_yard.png)

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

![Görüntü işleme Lion çizim analiz edin](./Images/lion_drawing.png)

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

![İşleme çiçek analiz edin](./Images/flower.png)

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