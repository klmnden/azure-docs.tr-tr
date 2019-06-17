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
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: bbf2e524d626ac17596ded61746c26f20a6caf1b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65021834"
---
#    <a name="text-merge-cognitive-skill"></a>Metin birleştirme bilişsel beceri

**Metin birleştirme** beceri alanlar koleksiyonu tek bir alana metinden birleştirir. 

> [!NOTE]
> Bu yetenek bir Bilişsel hizmetler API'sine bağlı değil ve kullanmak için ücretlendirilmez. Hala [Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md), ancak geçersiz kılmak için **ücretsiz** , az sayıda gün başına günlük zenginleştirmelerinin sınırlar resource seçeneği.

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
        "text": "The brown fox jumps over the dog",
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

Aşağıdaki örnek becerilerine OCR beceri belgeye gömülü görüntülerinden ayıklamak için kullanmaktadır. Ardından, oluşturur bir *merged_text* hem özgün hem de her görüntü OCRed metni içeren alan. OCR yetenek hakkında daha fazla bilgi [burada](https://docs.microsoft.com/azure/search/cognitive-search-skill-ocr).

```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
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
      "insertPostTag": " ",
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
