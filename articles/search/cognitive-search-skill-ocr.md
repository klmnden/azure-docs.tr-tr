---
title: OCR bilişsel arama nitelik (Azure Search) | Microsoft Docs
description: Bir Azure Search iyileştirmesini ardışık görüntü dosyaları metin Al.
services: search
manager: pablocas
author: luiscabrer
documentationcenter: ''
ms.assetid: ''
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 84988c815759a726abe93d931f73c284d771a5ba
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="ocr-cognitive-skill"></a>OCR bilişsel nitelik

**OCR** yetenek görüntü dosyaları metin ayıklar. Desteklenen dosya biçimleri şunlardır:

+ . JPEG
+ . JPG
+ . PNG
+ . BMP
+ . GIF


## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreleri büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| detectOrientation | Görüntü Yönü algılama sağlar. <br/> Geçerli değerler: true / false.|
|defaultLanguageCode |  Giriş metni dil kodu. Desteklenen diller şunlardır: `ar, cs, da, de, en, es, fi, fr, he, hu, it, ko, pt-br, pt`.  Dil kodu belirtilmemiş veya null ise, autodetected dilidir.|
| textExtractionAlgorithm | "yazdırılan" veya "el yazısı". "El yazısı" metin tanıma OCR algoritması şu anda önizlemede ve yalnızca İngilizce olarak desteklenir. |

## <a name="skill-inputs"></a>Yetenek girişleri

| Giriş adı      | Açıklama                                          |
|---------------|------------------------------------------------------|
| görüntü         | Karmaşık türü. "/ Belge/normalized_images" alan şu anda yalnızca çalışır Azure Blob Oluşturucu tarafından üretilen zaman ```imageAction``` ayarlanır ```generateNormalizedImages```. Bkz: [örnek](#sample-output) daha fazla bilgi için.|


## <a name="skill-outputs"></a>Yetenek çıkışları
| Çıktı adı     | Açıklama                   |
|---------------|-------------------------------|
| Metin          | Düz metin görüntüden ayıklanır.   |
| layoutText    | Karmaşık türü, metnin bulunduğu konumun yanı sıra ayıklanan metin açıklar.|


## <a name="sample-definition"></a>Örnek tanımı

```json
{
    "skills": [
      {
        "description": "Extracts text (plain and structured) from image."
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": null,
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text",
            "targetName": "myText"
          },
          {
            "name": "layoutText",
            "targetName": "myLayoutText"
          }
        ]
      }
    ]
 }
```
<a name="sample-output"></a>

## <a name="sample-text-and-layouttext-output"></a>Örnek metin ve layoutText çıkışı

```json
{
  "text": "Hello World. -John",
  "layoutText":
  {
    "language" : "en",
    "text" : "Hello World. -John",
    "lines" : [
      {
        "boundingBox":
        [ {"x":10, "y":10}, {"x":50, "y":10}, {"x":50, "y":30},{"x":10, "y":30}],
        "text":"Hello World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ],
    "words": [
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"Hello"
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ]
  }
}
```

## <a name="sample-merging-text-extracted-from-embedded-images-with-the-content-of-the-document"></a>Örnek: metin belgesinin içeriği katıştırılmış görüntülerle ayıklanan birleştiriliyor.

Metin birleşme için ortak bir kullanım örneği görüntülerinin (OCR yetenek ya da görüntü başlık metni) değerinin metinsel gösterimini birleştirme belgeye içerik alanına yeteneğidir. 

Aşağıdaki örnek skillset oluşturur bir *merged_text* OCRed metnin her görüntülerinin yanı sıra belgenizi metinsel içeriğini içerecek şekilde alan bu belgede katıştırılmış. 

#### <a name="request-body-syntax"></a>İstek gövdesi sözdizimi
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
          "name": "mergedText", "targetname" : "merged_text"
        }
      ]
    }
  ]
}
```
Yukarıdaki skillset örnek normalleştirilmiş görüntüleri alan var olduğunu varsayar. Bu alan oluşturmak üzere *imageAction* dizin oluşturucu tanımınızı yapılandırmasında *generateNormalizedImages* aşağıda gösterildiği gibi:

```json
{
  //...rest of your indexer definition goes here ...
  "parameters":
  {
    "configuration": 
    {
        "dataToExtract": "contentAndMetadata",
        "imageAction": "generateNormalizedImages"
        }
  }
}
```

## <a name="see-also"></a>Ayrıca bkz.
+ [Önceden tanımlanmış yetenekleri](cognitive-search-predefined-skills.md)
+ [TextMerger nitelik](cognitive-search-skill-textmerger.md)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)