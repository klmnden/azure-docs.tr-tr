---
title: Yüz algılama
titleSuffix: Computer Vision - Cognitive Services - Azure
description: Azure Bilişsel hizmetler görüntü işleme kullanan yüz algılama için ilgili kavramları.
services: cognitive-services
author: deken
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: abf0dc0ea80685130f6566ae962d2bed124e0924
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44725498"
---
# <a name="detecting-faces"></a>Yüz algılama

Görüntü işleme, bir resimdeki İnsan yüzlerini algılar ve yaş, cinsiyet ve algılanan her yüz için dikdörtgen oluşturur. Görüntü İşleme, [Yüz Tanıma](/azure/cognitive-services/face/)'da bulunan işlevlerin bir alt kümesini sunar ve Yüz tanımanın yanı sıra poz algılama gibi daha ayrıntılı analiz işlemleri için Yüz Tanıma hizmetini kullanabilirsiniz.  

## <a name="face-detection-examples"></a>Yüz algılama örnekleri

İlk örnek, tek bir insan yüz içeren bir görüntü için görüntü işleme tarafından döndürülen JSON yanıtı gösterir.

![İşleme kadın tavan yüz analiz edin](./Images/woman_roof_face.png)

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

İkinci örnek, birden fazla insan yüzlerini içeren bir görüntü için döndürülen JSON yanıtı gösterir.

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

Kavramları hakkında bilgi edinin [etki alanına özgü içerik algılama](concept-detecting-domain-content.md).