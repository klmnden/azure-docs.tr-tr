---
title: Etki alanına özgü içerik - görüntü işleme algılayın
titleSuffix: Azure Cognitive Services
description: Bir görüntü ile ilgili daha ayrıntılı bilgi döndürmek için bir görüntü kategori etki alanı belirtirseniz öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: e4b64e00f71768a8821c83a73b019f77089e1b3a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60368088"
---
# <a name="detect-domain-specific-content"></a>Etki alanına özgü içerikleri algılama

Buna ek olarak etiketleme ve üst düzey kategori, görüntü işleme, ayrıca daha fazla veri eğitilen modelleri kullanarak etki alanına özgü analiz destekler.

Alana özgü modeller kullanmanın iki yolu vardır: tek başına (kapsamlı analizi) veya kategori özelliği için bir geliştirme olarak.

### <a name="scoped-analysis"></a>Kapsamlı analiz

Yalnızca seçilen etki alanına özgü modeli çağırarak kullanarak görüntü çözümleyebilirsiniz [modelleri /\<modeli\>/Analyze](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e200) API.

Bir örnek tarafından döndürülen JSON yanıtı aşağıdaki gibidir **modelleri/ünlüleri/analiz** API belirtilen görüntü için:

![Satya Nadella standing, smiling](./images/satya.jpeg)

```json
{
  "result": {
    "celebrities": [{
      "faceRectangle": {
        "top": 391,
        "left": 318,
        "width": 184,
        "height": 184
      },
      "name": "Satya Nadella",
      "confidence": 0.99999856948852539
    }]
  },
  "requestId": "8217262a-1a90-4498-a242-68376a4b956b",
  "metadata": {
    "width": 800,
    "height": 1200,
    "format": "Jpeg"
  }
}
```

### <a name="enhanced-categorization-analysis"></a>Kategori Gelişmiş analiz

Alana özgü modeller, genel görüntü analizi desteklemek için de kullanabilirsiniz. Bir parçası olarak bunu [üst düzey kategori](concept-categorizing-images.md) etki alanına özgü modeller belirterek *ayrıntıları* parametresinin [Çözümle](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API çağrısı.

Bu durumda, 86 kategori sınıflandırma sınıflandırıcı önce çağrılır. Eşleşen bir etki alanına özgü modeli algılanan kategorilerden herhangi biri varsa, görüntü de modeli ve sonuçları eklenir üzerinden geçirilir.

Etki alanına özgü analiz aşağıdaki JSON yanıtı gösterilir olarak dahil edilebilir `detail` daha geniş bir kategori analizi düğümü.

```json
"categories":[
  {
    "name":"abstract_",
    "score":0.00390625
  },
  {
    "name":"people_",
    "score":0.83984375,
    "detail":{
      "celebrities":[
        {
          "name":"Satya Nadella",
          "faceRectangle":{
            "left":597,
            "top":162,
            "width":248,
            "height":248
          },
          "confidence":0.999028444
        }
      ],
      "landmarks":[
        {
          "name":"Forbidden City",
          "confidence":0.9978346
        }
      ]
    }
  }
]
```

## <a name="list-the-domain-specific-models"></a>Alana özgü modeller listesi

Görüntü işleme şu anda aşağıdaki alana özgü modeller destekler:

| Ad | Açıklama |
|------|-------------|
| ünlüleri | Ünlü tanıma, sınıflandırılan görüntüleri için desteklenen `people_` kategorisi |
| Yer işareti | Önemli yer tanıma, sınıflandırılan görüntüleri için desteklenen `outdoor_` veya `building_` kategorileri |

Çağırma [modelleri](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fd) API her model uygulayabilir kategorileri birlikte bu bilgileri döndürür:

```json
{
  "models":[
    {
      "name":"celebrities",
      "categories":[
        "people_",
        "人_",
        "pessoas_",
        "gente_"
      ]
    },
    {
      "name":"landmarks",
      "categories":[
        "outdoor_",
        "户外_",
        "屋外_",
        "aoarlivre_",
        "alairelibre_",
        "building_",
        "建筑_",
        "建物_",
        "edifício_"
      ]
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [görüntüleri kategorilendirme](concept-categorizing-images.md).
