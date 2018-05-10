---
title: Metin birleştirme bilişsel arama nitelik (Azure Search) | Microsoft Docs
description: Metin alanları koleksiyonundan bir birleştirilmiş alanına birleştirin. Bilişsel Bu yetenek, bir Azure Search iyileştirmesini ardışık düzeninde kullanın.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: e288748d7433f4b3c7da7db1ab1ef2ee487318df
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
#    <a name="text-merge-cognitive-skill"></a>Metin birleştirme bilişsel nitelik

**Metin birleştirme** yetenek koleksiyona tek bir alan alanlar metinden birleştirir. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.TextMerger

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreleri büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| insertPreTag  | Her ekleme önce eklenecek dize. Varsayılan değer `" "`. Alanı atlamak için değerine `""`.  |
| insertPostTag | Sonra her ekleme eklenecek dize. Varsayılan değer `" "`. Alanı atlamak için değerine `""`.  |


##  <a name="sample-input"></a>Örnek Giriş
Bu yetenek için kullanılabilir giriş sağlayan bir JSON belgesi aşağıdakilerden biri olabilir:

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "The brown fox jumps over the dog" ,
             "itemsToInsert": ["quick", "lazy"],
             "offsets": [3, 28],
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Örnek çıktı
Bu örnek olduğunu varsayarak önceki giriş çıktısını gösterir *insertPreTag* ayarlanır `" "`, ve *insertPostTag* ayarlanır `""`. 

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "mergedText": "The quick brown fox jumps over the lazy dog" 
           }
      }
    ]
}
```

## <a name="extended-sample-skillset-definition"></a>Genişletilmiş örnek skillset tanımı

Metin birleştirme kullanmak için yaygın bir senaryo görüntülerinin (OCR yetenek ya da görüntü başlık metni) değerinin metinsel gösterimini bir belge içerik alanına birleştirmektir. 

Aşağıdaki örnek skillset OCR yetenek metin belgede gömülü görüntüleri ayıklamak için kullanır. Ardından, oluşturduğu bir *merged_text* özgün ve her görüntü OCRed metinden içeren alan. 

```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "name": "OCR skill",
        "description": "Extract text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": "en",
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text"
          }
        ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Util.TextMerger",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " "
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetname" : "merged_text"
        }
      ]
    }
  ]
}
```
Yukarıdaki örnekte normalleştirilmiş görüntüleri alan bulunduğunu varsayar. Normalleştirilmiş görüntüleri alan almak üzere *imageAction* dizin oluşturucu tanımınızı yapılandırmasında *generateNormalizedImages* aşağıda gösterildiği gibi:

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "mergedText": "The quick brown fox jumps over the lazy dog" 
           }
      }
    ]
}
```

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış yetenekleri](cognitive-search-predefined-skills.md)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)