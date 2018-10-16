---
title: Görüntüleri - görüntü işleme halinde kategorilere ayrılması
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'sini kullanarak görüntüleri halinde kategorilere ayrılması için ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 602ea8028cf89b23df692d5c2fb9b781f64bcad4
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341693"
---
# <a name="categorizing-images"></a>Resimleri kategorilere ayırma

Görüntü işleme, etiketleme ve açıklamaları ek olarak, önceki sürümlerde tanımlanmış sınıflandırma tabanlı kategorileri döndürür. Bu kategoriler, üst/alt hereditary hiyerarşileri ile bir sınıflandırma olarak düzenlenir. Tüm kategoriler İngilizce'dir. Tek başına veya birlikte yeni modeli etiketleme kullanılabilir.

## <a name="the-86-category-concept"></a>Kategori 86 kavramı

Aşağıdaki diyagramda görüldüğü 86 kavramları listesini bağlı olarak, görüntü geniş özel arasında değişen sınıflandırılabilir. Metin biçimindeki tam sınıflandırma için bkz: [kategori sınıflandırma](category-taxonomy.md).

![Çözümleme kategorisi](./Images/analyze_categories.png)

## <a name="image-categorization-examples"></a>Resmi kategori örnekleri

Görüntü işleme örnek görüntünün görsel özelliklerini halinde kategorilere ayrılması zaman döndürür aşağıdaki JSON yanıtı gösterilir.

![Kadın tavan](./Images/woman_roof.png)

```json
{
    "categories": [
        {
            "name": "people_",
            "score": 0.81640625
        }
    ],
    "requestId": "bae7f76a-1cc7-4479-8d29-48a694974705",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

Aşağıdaki tabloda tipik görüntü kümesi ve görüntü işleme tarafından döndürülen her görüntü için kategori gösterilmektedir.

| Görüntü | Kategori |
|-------|----------|
| ![Aile fotoğrafı](./Images/family_photo.png) | people_group |
| ![Şirin köpek](./Images/cute_dog.png) | animal_dog |
| ![Dış Sıradağlar](./Images/mountain_vista.png) | outdoor_mountain |
| ![İşleme Gıda ekmek analiz edin](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [görüntüleri etiketleme](concept-tagging-images.md) ve [görüntüleri açıklayan](concept-describing-images.md).