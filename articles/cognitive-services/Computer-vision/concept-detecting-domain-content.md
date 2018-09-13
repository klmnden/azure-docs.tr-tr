---
title: Etki alanına özgü içerik algılama
titleSuffix: Computer Vision - Cognitive Services - Azure
description: Azure Bilişsel hizmetler görüntü işleme kullanarak görüntüleri tanımlamaya ilgili kavramları.
services: cognitive-services
author: deken
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: 699698d8235d8fa53050a67491ee4f0e1c336d0e
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44725522"
---
# <a name="detecting-domain-specific-content"></a>Etki alanına özgü içerik algılama

Buna ek olarak etiketleme ve üst düzey kategori, görüntü işleme, özelleştirilmiş (veya etki alanına özgü) bilgi de destekler. Özel bilgiler, üst düzey kategori ile veya bir tek başına yöntem olarak uygulanabilir. Daha fazla alana özgü modeller aracılığıyla 86-kategori sınıflandırma iyileştirmek için bir yol olarak işlev görür.

Alana özgü modeller kullanarak için iki seçenek vardır:

* Kapsamlı analiz  
  Seçilen bir model, bir HTTP POST çağrısına çağırarak analiz edin. Kullanmak istediğiniz hangi modelle biliyorsanız, modelin adı belirtin. Yalnızca bilgi ilgili Bu modele sahip olursunuz. Örneğin, ünlü tanıma için yalnızca aramak için bu seçeneği kullanın. Yanıt, olası güven puanlarını eşlik ünlüleri, eşleşen bir listesini içerir.
* Gelişmiş analiz  
  Kategorileri 86 kategori Sınıflandırma, ilgili ek ayrıntılar sağlamak için analiz edin. Bu seçenek kullanıcıların bir veya daha fazla alana özgü modeller genel görüntü analizi ek ayrıntıları almak istediğiniz yere uygulamalarda kullanılabilir. Bu yöntem çağrıldığında 86 kategori sınıflandırma sınıflandırıcı önce çağrılır. Kategorilerden herhangi biri, bilinen ya da eşleşen modellerin eşleşirse, ikinci bir sınıflandırıcı çağrılarını geçişi izler. Örneğin, varsa `details` HTTP POST çağrısına parametresi ya da "tüm" ayarlı değil veya "ünlüleri" içerir, 86 kategori sınıflandırıcı çağrıldıktan sonra ünlü sınıflandırıcı yöntemini çağırır. Görüntü olarak sınıflandırılır varsa `people_` veya kategoriye ve ardından ünlü sınıflandırıcı kategorisidir çağrılır.

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