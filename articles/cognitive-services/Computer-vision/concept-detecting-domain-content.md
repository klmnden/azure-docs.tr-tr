---
title: Etki alanına özgü içerik - görüntü işleme algılayın
titleSuffix: Azure Cognitive Services
description: Bir görüntü ile ilgili daha ayrıntılı bilgi döndürmek için bir görüntü kategori etki alanı belirtirseniz öğrenin.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 76484a2340e527dc016f321dbafa29adb7c358b5
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55157543"
---
# <a name="detecting-domain-specific-content"></a>Etki alanına özgü içeriği algılama

Buna ek olarak etiketleme ve üst düzey kategori, görüntü işleme, özelleştirilmiş (veya etki alanına özgü) bilgi de destekler. Özelleştirilmiş bilgiler tek başına bir yöntem olarak uygulanabileceği gibi, üst düzey kategorilerle de uygulanabilir. Etki alanına özgü modellerin eklenmesiyle 86 kategori taksonomisini daha da geliştirmek için bir araç işlevi görür.

Etki alanına özgü modelleri kullanmak için iki seçenek vardır:

* Kapsamlı analiz  
  Seçilen bir model, bir HTTP POST çağrısına çağırarak analiz edin. Kullanmak istediğiniz hangi modelle biliyorsanız, modelin adı belirtin. Yalnızca bilgi ilgili Bu modele sahip olursunuz. Örneğin, bu seçeneği kullanarak yalnızca ünlü tanıma için arama yapabilirsiniz. Sonuç, eşleşen olası ünlülerin listesiyle birlikte bunların güvenilirlik puanını da içerir.
* Gelişmiş analiz  
  86 kategori taksonomisindeki kategorilerle ilgili ek ayrıntılar sağlamak için analiz edin. Bu seçenek bir veya daha fazla etki alanına özgü modelde yer alan ayrıntılara ek olarak kullanıcının genel görüntü analizi de almak istediği uygulamalarda kullanılmaya yöneliktir. Bu yöntem çağrıldığında, önce 86 kategori taksonomisinin sınıflandırıcısı çağrılır. Kategorilerden herhangi biri, bilinen ya da eşleşen modellerin eşleşirse, ikinci bir sınıflandırıcı çağrılarını geçişi izler. Örneğin, varsa `details` HTTP POST çağrısına parametresi ya da "tüm" ayarlı değil veya "ünlüleri" içerir, 86 kategori sınıflandırıcı çağrıldıktan sonra ünlü sınıflandırıcı yöntemini çağırır. Görüntü olarak sınıflandırılır varsa `people_` veya kategoriye ve ardından ünlü sınıflandırıcı kategorisidir çağrılır.

## <a name="listing-domain-specific-models"></a>Alana özgü modeller listeleme

Görüntü işleme tarafından desteklenen alana özgü modeller listeleyebilirsiniz. Şu anda görüntü işleme, etki alanına özgü içerik algılamak için aşağıdaki alana özgü modeller destekler:

| Ad | Açıklama |
|------|-------------|
| ünlüleri | Ünlü tanıma, sınıflandırılan görüntüleri için desteklenen `people_` kategorisi |
| Yer işareti | Önemli yer tanıma, sınıflandırılan görüntüleri için desteklenen `outdoor_` veya `building_` kategorileri |

### <a name="domain-model-list-example"></a>Etki alanı modeli listesi örneği

Etki alanına özgü modeller, görüntü işleme tarafından desteklenen aşağıdaki JSON yanıtı listeler.

```json
{
    "models": [
        {
            "name": "celebrities",
            "categories": ["people_", "人_", "pessoas_", "gente_"]
        },
        {
            "name": "landmarks",
            "categories": ["outdoor_", "户外_", "屋外_", "aoarlivre_", "alairelibre_",
                "building_", "建筑_", "建物_", "edifício_"]
        }
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [görüntüleri kategorilendirme](concept-categorizing-images.md).
