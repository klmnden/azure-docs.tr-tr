---
title: Görüntüleri - görüntü işleme halinde kategorilere ayrılması
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'si görüntü kategori özelliğiyle ilgili kavramları öğrenin.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: f182110d150583ee1c241c23a2e1924d9f3e3bd4
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55158052"
---
# <a name="image-categorization-with-computer-vision"></a>Görüntü işleme ile görüntü kategorilere ayırma

Görüntü işleme, etiketleme ve açıklamaları ek olarak, önceki sürümlerde tanımlanmış sınıflandırma tabanlı kategorileri döndürür. Bu kategoriler, üst/alt kalıtsal hiyerarşileriyle bir taksonomi olarak düzenlenmiştir. Tüm kategoriler İngilizcedir. Tek başına veya birlikte yeni modeli etiketleme kullanılabilir.

## <a name="the-86-category-concept"></a>86 kategori kavramı

Aşağıdaki diyagramda görüldüğü 86 kavramları listesini bağlı olarak, görüntü geniş özel arasında değişen sınıflandırılabilir. Metin biçiminde tam taksonomi için bkz. [Kategori Taksonomisi](category-taxonomy.md).

![Kategori sınıflandırma tüm kategorilerde gruplanmış listesi](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>Resmi kategori örnekleri

Görüntü işleme örnek görüntünün görsel özelliklerini halinde kategorilere ayrılması zaman döndürür aşağıdaki JSON yanıtı gösterilir.

![Bir grup oluşturma çatıyı üzerinde bir kadın](./Images/woman_roof.png)

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
| ![Dört kişilik ailesi birlikte teşkil](./Images/family_photo.png) | people_group |
| ![Çocukluğunuzda alanında oturan bir sevimli köpek](./Images/cute_dog.png) | animal_dog |
| ![Üzerinde bir Sıradağlar rock gün batımı duran bir kişi](./Images/mountain_vista.png) | outdoor_mountain |
| ![Bir tabloda ekmek rolleri yığını](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [görüntüleri etiketleme](concept-tagging-images.md) ve [görüntüleri açıklayan](concept-describing-images.md).
