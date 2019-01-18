---
title: Metin birleştirme bilişsel arama beceri - Azure Search
description: Alanlar koleksiyonu metinden birleştirilmiş tek bir alanda birleştirin. Bilişsel Bu yetenek, bir Azure Search zenginleştirme hattında kullanın.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 01/17/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 2ef5d285c19900fd2896279edde8841581d7e947
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54388174"
---
#    <a name="text-merge-cognitive-skill"></a>Metin birleştirme bilişsel beceri

**Metin birleştirme** beceri alanlar koleksiyonu tek bir alana metinden birleştirir. 

> [!NOTE]
> Bu yetenek bir Bilişsel hizmetler API'sine bağlı değil ve bu beceri ile ilişkili ücret yoktur. Bununla birlikte, [Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md) yine de ücretsiz resource seçeneği geçersiz kılmak için günlük zenginleştirmelerinin az sayıda için sınırlar.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.MergeSkill

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| insertPreTag  | Önce her bir ekleme eklenecek dize. Varsayılan değer `" "` şeklindedir. Değer alanı atlamak için kümesine `""`.  |
| insertPostTag | Sonra her bir ekleme eklenecek dize. Varsayılan değer `" "` şeklindedir. Değer alanı atlamak için kümesine `""`.  |


##  <a name="sample-input"></a>Örnek Giriş
Bu yetenek için kullanılabilir girişi sağlayan bir JSON belgesi olabilir:

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
Bu örnek olduğunu varsayarak önceki giriş çıkış gösterir *insertPreTag* ayarlanır `" "`, ve *insertPostTag* ayarlanır `""`. 

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

## <a name="extended-sample-skillset-definition"></a>Genişletilmiş örnek beceri kümesi tanımı

Metin birleştirme kullanmaya yönelik yaygın bir senaryo görüntüleri (OCR beceri veya görüntünün bir açıklamalı alt yazı metni) değerinin metinsel gösterimini birleştirmek için içerik bir belgenin bir alandır. 

Aşağıdaki örnek becerilerine OCR beceri belgeye gömülü görüntülerinden ayıklamak için kullanmaktadır. Ardından, oluşturur bir *merged_text* hem özgün hem de her görüntü OCRed metni içeren alan. 

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
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
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
          "name": "mergedText", "targetName" : "merged_text"
        }
      ]
    }
  ]
}
```
Yukarıdaki örnekte, bir normalleştirilmiş görüntüleri alan bulunduğunu varsayar. Normalleştirilmiş görüntüleri alan almak üzere *imageAction* yapılandırma için dizin oluşturucu Tanımınızda *generateNormalizedImages* aşağıda gösterildiği gibi:

```json
{  
   //...rest of your indexer definition goes here ... 
  "parameters":{  
      "configuration":{  
         "dataToExtract":"contentAndMetadata",
         "imageAction":"generateNormalizedImages"
      }
   }
}
```

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Dizin Oluşturucu (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
