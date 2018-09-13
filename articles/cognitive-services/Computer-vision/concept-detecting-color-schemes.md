---
title: Renk düzenleri algılama
titleSuffix: Computer Vision - Cognitive Services - Azure
description: Azure Bilişsel hizmetler görüntü işleme kullanarak resimlerdeki renk şeması algılanıyor ilgili kavramları.
services: cognitive-services
author: deken
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: 66abab93ba9c1152d18428e66d648c6ba690aaa0
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44725453"
---
# <a name="detecting-color-schemes"></a>Renk düzenleri algılama

Görüntü işleme, bir resimden renkleri ayıklar. Renkleri, sonra üç farklı bağlamda analiz edilir: baskın ön plan rengi, baskın arka plan rengini ve bir bütün olarak görüntü için baskın renkler. Bunlar 12 baskın vurgu rengine ayrılarak gruplandırılır. Vurgu renklerdir siyah, mavi, brown, gri, yeşil, orange, pembe, mor, red, Deniz Mavisi, teknik ve sarı. Görüntü işleme, izleyiciler, birlikte baskın renk doygunluğu ve görüntü için en canlı rengi temsil eden bir Vurgu rengi döndürülecek bir görüntüden ayıklanan renkleri analiz eder. Görüntü renkleri bağlı olarak, basit siyah beyaz mı vurgu rengine ayrılarak onaltılık renk kodlarını döndürülebilir. Görüntü işleme, ayrıca bir resmin siyah olmadığını gösteren bir Boole değeri döndürür ve beyaz.

## <a name="color-scheme-detection-examples"></a>Renk şeması algılama örnekleri

Aşağıdaki örnek, örnek görüntüde renk düzenini tespit edilirken, görüntü işleme tarafından döndürülen JSON yanıtı gösterir. Bu durumda, örnek görüntüde Siyah & beyaz bir görüntü değil ancak baskın ön ve arka plan renkleri siyah olur ve baskın bir bütün olarak resmin siyah beyaz renklerdir.

![Dış Sıradağlar](./Images/mountain_vista.png)

```json
{
    "color": {
        "dominantColorForeground": "Black",
        "dominantColorBackground": "Black",
        "dominantColors": ["Black", "White"],
        "accentColor": "BB6D10",
        "isBwImg": false
    },
    "requestId": "0dc394bf-db50-4871-bdcc-13707d9405ea",
    "metadata": {
        "height": 202,
        "width": 300,
        "format": "Jpeg"
    }
}
```

### <a name="dominant-color-examples"></a>Baskın renk örnekleri

Aşağıdaki tabloda, baskın ön plan, arka plan ve görüntü işleme tarafından döndürülen her bir örnek görüntü için görüntü renkleri açıklanmaktadır.

| Görüntü | Baskın renkler |
|-------|-----------------|
|![İşleme çiçek analiz edin](./Images/flower.png)| Ön plan: siyah<br/>Arka plan: beyaz<br/>Renkler: Siyah, beyaz, yeşil|
![Görüntü işleme Train istasyon analiz edin](./Images/train_station.png) | Ön plan: siyah<br/>Arka plan: siyah<br/>Renkler: siyah |

### <a name="accent-color-examples"></a>Vurgu rengi örnekleri

 Aşağıdaki tabloda görüntü işleme tarafından döndürülen her bir örnek görüntü için onaltılık bir HTML renk değeri olarak bir Vurgu rengi açıklar.

| Görüntü | Vurgu rengi |
|-------|--------------|
|![Dış Sıradağlar](./Images/mountain_vista.png) | #BB6D10 |
|![İşleme çiçek analiz edin](./Images/flower.png) | #C6A205 |
|![Görüntü işleme Train istasyon analiz edin](./Images/train_station.png) | #474A84 |

### <a name="black--white-detection-examples"></a>Siyah & Beyaz algılama örnekleri

Aşağıdaki tabloda, her bir örnek görüntü siyah olup olmadığını belirtir ve görüntü işleme tarafından döndürülen beyaz.

| Görüntü | Siyah & Beyaz? |
|-------|----------------|
|![İşleme analiz oluşturma](./Images/bw_buildings.png) | true |
|![Görüntü merkezi Yard analiz edin](./Images/house_yard.png) | false |

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [resim türleri algılama](concept-detecting-image-types.md).