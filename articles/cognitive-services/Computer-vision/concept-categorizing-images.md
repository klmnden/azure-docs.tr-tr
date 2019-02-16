---
title: Görüntüleri - görüntü işleme halinde kategorilere ayrılması
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'si görüntü kategori özelliğiyle ilgili kavramları öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: a85fb65ad971bfc70bdab0689e99921e616b56ac
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56308868"
---
# <a name="categorize-images"></a>Görüntüleri kategorilere ayırma

Etiketler ve bir açıklama ek olarak, görüntü işleme görüntüdeki algılanan sınıflandırma tabanlı kategorileri döndürür. Aksine, etiketler, kategoriler, üst/alt hereditary hiyerarşik olarak düzenlenir ve daha az (etiketler binlerce aksine, 86) vardır. Tüm kategori adları, İngilizce'dir. Kategori, tek başına ya da yeni etiketler modelin yanı sıra yapılabilir.

## <a name="the-86-category-concept"></a>86 kategori kavramı

Görüntü işleme görüntü problem veya özellikle, aşağıdaki diyagramda 86 categroies listesini kullanarak kategorilere ayırabilirsiniz. Metin biçiminde tam taksonomi için bkz. [Kategori Taksonomisi](category-taxonomy.md).

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
