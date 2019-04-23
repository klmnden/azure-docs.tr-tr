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
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 257da397e11843ee96e93f7b3e9bc5ada29822cf
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59995583"
---
# <a name="categorize-images-by-subject-matter"></a>Görüntüleri konuya göre kategorilere ayırın

Etiketler ve bir açıklama ek olarak, görüntü işleme görüntüdeki algılanan sınıflandırma tabanlı kategorileri döndürür. Aksine, etiketler, kategoriler, üst/alt hereditary hiyerarşik olarak düzenlenir ve daha az (etiketler binlerce aksine, 86) vardır. Tüm kategori adları, İngilizce'dir. Kategori, tek başına ya da yeni etiketler modelin yanı sıra yapılabilir.

## <a name="the-86-category-concept"></a>86 kategori kavramı

Görüntü işleme görüntü problem veya özellikle, aşağıdaki diyagramda 86 kategori listesi kullanarak kategorilere ayırabilirsiniz. Metin biçiminde tam taksonomi için bkz. [Kategori Taksonomisi](category-taxonomy.md).

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

| Image | Category |
|-------|----------|
| ![Dört kişilik ailesi birlikte teşkil](./Images/family_photo.png) | people_group |
| ![Çocukluğunuzda alanında oturan bir sevimli köpek](./Images/cute_dog.png) | animal_dog |
| ![Üzerinde bir Sıradağlar rock gün batımı duran bir kişi](./Images/mountain_vista.png) | outdoor_mountain |
| ![Bir tabloda ekmek rolleri yığını](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [görüntüleri etiketleme](concept-tagging-images.md) ve [görüntüleri açıklayan](concept-describing-images.md).
