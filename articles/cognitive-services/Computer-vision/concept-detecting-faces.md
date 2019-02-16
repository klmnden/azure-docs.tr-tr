---
title: Yüzleri - görüntü işleme algılama
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'si, yüz algılama özelliğiyle ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 1056b8be113d56342aea8f83d5325737f7ecb93b
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56308460"
---
# <a name="face-detection-with-computer-vision"></a>Yüz algılama ile görüntü işleme

Görüntü işleme, görüntü İnsan yüzlerini algılayın ve yaş, cinsiyet ve algılanan her yüz için dikdörtgen oluşturur. 

> [!NOTE]
> Bu özellik ayrıca Azure tarafından sunulan [yüz](/azure/cognitive-services/face/) hizmeti. Yüz tanıma gibi analiz, yüz tanıma ve algılama konusunda sizi uyarmayı bu seçenek daha ayrıntılı bakın. 

## <a name="face-detection-examples"></a>Yüz algılama örnekleri

Aşağıdaki örnek, tek bir insan yüz içeren bir görüntü için görüntü işleme tarafından döndürülen JSON yanıtı gösterir.

![Görüntü Analizi Damdaki Kadının Yüzü](./Images/woman_roof_face.png)

```json
{
    "faces": [
        {
            "age": 23,
            "gender": "Female",
            "faceRectangle": {
                "top": 45,
                "left": 194,
                "width": 44,
                "height": 44
            }
        }
    ],
    "requestId": "8439ba87-de65-441b-a0f1-c85913157ecd",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Png"
    }
}
```

Sonraki örnek, birden fazla insan yüzlerini içeren bir görüntü için döndürülen JSON yanıtı gösterir.

![İşleme ailesi fotoğraf yüz analiz edin](./Images/family_photo_face.png)

```json
{
    "faces": [
        {
            "age": 11,
            "gender": "Male",
            "faceRectangle": {
                "top": 62,
                "left": 22,
                "width": 45,
                "height": 45
            }
        },
        {
            "age": 11,
            "gender": "Female",
            "faceRectangle": {
                "top": 127,
                "left": 240,
                "width": 42,
                "height": 42
            }
        },
        {
            "age": 37,
            "gender": "Female",
            "faceRectangle": {
                "top": 55,
                "left": 200,
                "width": 41,
                "height": 41
            }
        },
        {
            "age": 41,
            "gender": "Male",
            "faceRectangle": {
                "top": 45,
                "left": 103,
                "width": 39,
                "height": 39
            }
        }
    ],
    "requestId": "3a383cbe-1a05-4104-9ce7-1b5cf352b239",
    "metadata": {
        "height": 230,
        "width": 300,
        "format": "Png"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [analiz görüntü](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) başvuru yüz algılama özelliğinin nasıl kullanılacağı hakkında daha fazla bilgi edinmek için belgeleri.
