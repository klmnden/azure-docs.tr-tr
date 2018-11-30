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
ms.openlocfilehash: 7062d98d40c15f4e9e873038fc12fc1b104c996d
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52333488"
---
# <a name="categorizing-images"></a>Resimleri kategorilere ayırma

Görüntü işleme, etiketleme ve açıklamaları ek olarak, önceki sürümlerde tanımlanmış sınıflandırma tabanlı kategorileri döndürür. Bu kategoriler, üst/alt kalıtsal hiyerarşileriyle bir taksonomi olarak düzenlenmiştir. Tüm kategoriler İngilizcedir. Tek başına veya birlikte yeni modeli etiketleme kullanılabilir.

## <a name="the-86-category-concept"></a>86 kategori kavramı

Aşağıdaki diyagramda görüldüğü 86 kavramları listesini bağlı olarak, görüntü geniş özel arasında değişen sınıflandırılabilir. Metin biçiminde tam taksonomi için bkz. [Kategori Taksonomisi](category-taxonomy.md).

![Kategori sınıflandırma tüm kategorilerde gruplanmış listesi](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>Resmi kategori örnekleri

Görüntü işleme örnek görüntünün görsel özelliklerini halinde kategorilere ayrılması zaman döndürür aşağıdaki JSON yanıtı gösterilir.

![Çatıda Kadın](./Images/woman_roof.png)

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
| ![Aile Fotoğrafı](./Images/family_photo.png) | people_group |
| ![Şirin Köpek](./Images/cute_dog.png) | animal_dog |
| ![Dış Mekanda Dağ](./Images/mountain_vista.png) | outdoor_mountain |
| ![Görüntü Analizi Yiyecek Ekmek](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [görüntüleri etiketleme](concept-tagging-images.md) ve [görüntüleri açıklayan](concept-describing-images.md).