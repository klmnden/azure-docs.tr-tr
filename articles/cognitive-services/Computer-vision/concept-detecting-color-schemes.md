---
title: Renk düzenleri - görüntü işleme algılama
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'sini kullanarak resimlerdeki renk şeması algılama için ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: d882ad89e68936d07ae4d76218c6e3ac450185a8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55151337"
---
# <a name="detect-color-schemes-in-images"></a>Görüntüleri renk düzenleri algılayın

Görüntü işleme, bir resimden renkleri ayıklar. Renkleri, sonra üç farklı bağlamda analiz edilir: baskın ön plan rengi, baskın arka plan rengini ve bir bütün olarak görüntü için baskın renkler. Bunlar 12 baskın vurgu rengine ayrılarak gruplandırılır. Bu vurgu renkleri beyaz, deniz mavisi, gri, kahverengi, kırmızı, mavi, mor, pembe, sarı, siyah, turuncu ve yeşildir. Görüntü işleme, izleyiciler, birlikte baskın renk doygunluğu ve görüntü için en canlı rengi temsil eden bir Vurgu rengi döndürülecek bir görüntüden ayıklanan renkleri analiz eder. Görüntüdeki renklere bağlı olarak, basit siyah ve beyaz veya vurgu renkleri onaltılık renk kodlarıyla döndürülebilir. Görüntü işleme, ayrıca bir resmin siyah olmadığını gösteren bir Boole değeri döndürür ve beyaz.

## <a name="color-scheme-detection-examples"></a>Renk şeması algılama örnekleri

Aşağıdaki örnek, örnek görüntüde renk düzenini tespit edilirken, görüntü işleme tarafından döndürülen JSON yanıtı gösterir. Bu durumda, örnek görüntüde Siyah & beyaz bir görüntü değil ancak baskın ön ve arka plan renkleri siyah olur ve baskın bir bütün olarak resmin siyah beyaz renklerdir.

![Dış Mekanda Dağ](./Images/mountain_vista.png)

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
|![Yeşil bir arka plan beyaz çiçek](./Images/flower.png)| Ön plan: Siyah<br/>Arka planı: Beyaz<br/>Renkler: Siyah, beyaz-yeşil|
![İstasyonu çalışan bir eğitimi](./Images/train_station.png) | Ön plan: Siyah<br/>Arka planı: Siyah<br/>Renkler: Siyah |

### <a name="accent-color-examples"></a>Vurgu rengi örnekleri

 Aşağıdaki tabloda görüntü işleme tarafından döndürülen her bir örnek görüntü için onaltılık bir HTML renk değeri olarak bir Vurgu rengi açıklar.

| Görüntü | Vurgu rengi |
|-------|--------------|
|![Üzerinde bir Sıradağlar rock gün batımı duran bir kişi](./Images/mountain_vista.png) | #BB6D10 |
|![Yeşil bir arka plan beyaz çiçek](./Images/flower.png) | #C6A205 |
|![İstasyonu çalışan bir eğitimi](./Images/train_station.png) | #474A84 |

### <a name="black--white-detection-examples"></a>Siyah & Beyaz algılama örnekleri

Aşağıdaki tabloda, her bir örnek görüntü siyah olup olmadığını belirtir ve görüntü işleme tarafından döndürülen beyaz.

| Görüntü | Siyah & Beyaz? |
|-------|----------------|
|![Siyah beyaz Manhattan stockholm'deki resmi](./Images/bw_buildings.png) | true |
|![Mavi bir ev ve ön yard](./Images/house_yard.png) | false |

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [resim türleri algılama](concept-detecting-image-types.md).
