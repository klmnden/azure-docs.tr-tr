---
title: OCR bilişsel arama beceri (Azure Search) | Microsoft Docs
description: Metin, görüntü dosyalarını bir Azure Search zenginleştirme ardışık ayıklayın.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 234651ad3672982e4de9617561a926712697945a
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45734042"
---
# <a name="ocr-cognitive-skill"></a>OCR bilişsel beceri

**OCR** beceri görüntü dosyalarından metin ayıklar. Desteklenen dosya biçimleri şunlardır:

+ . JPEG
+ . JPG
+ . PNG
+ . BMP
+ . GIF

> [!NOTE]
> Bilişsel Arama, genel önizleme aşamasındadır. Görüntü ayıklama ve normalleştirme ve beceri yürütmesi şu anda ücretsiz sunulmaktadır. Daha sonraki bir zamanda, bu özelliklerin fiyatlandırması duyurulacaktır. 

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| detectOrientation | Görüntü Yönü'nın intellisense sağlar. <br/> Geçerli değerler: true / false.|
|defaultLanguageCode | <p>  Giriş metni dil kodu. Desteklenen diller: <br/> zh-Hans (ChineseSimplified) <br/> zh-Hant (ChineseTraditional) <br/>cs (Çekçe) <br/>da (Danimarka) <br/>NL (Hollanda dili) <br/>tr (Türkçe) <br/>Fi (Fince)  <br/>FR (Fransızca) <br/>  de (Almanya) <br/>el (Yunanca) <br/> hu (Macarca) <br/> Bu (İtalyanca) <br/>  ja (Japonca) <br/> Ko (Korece) <br/> NB (Norveç dili) <br/>   PL (Lehçe) <br/> PT (Portekizce) <br/>  RU (Rusça) <br/>  ES (İspanyolca) <br/>  sv (İsveç dili) <br/>  tr (Türkçe) <br/> ar (Arapça) <br/> Ro (Rumence) <br/> SR-Cyrl (SerbianCyrillic) <br/> SR-Latn (SerbianLatin) <br/>  SK (Slovakya). <br/>  UNK (bilinmiyor) <br/><br/> Dil kodu belirtilmemiş veya null ise, autodetected dilidir. </p> |
| textExtractionAlgorithm | "yazılı" veya "el yazısı". "El yazısı" metin tanıma OCR algoritması, şu anda Önizleme aşamasındadır ve yalnızca İngilizce olarak desteklenmektedir. |

## <a name="skill-inputs"></a>Beceri girişleri

| Adı girin      | Açıklama                                          |
|---------------|------------------------------------------------------|
| image         | Karmaşık tür. "/ Belge/normalized_images" alan şu anda yalnızca çalışır, Azure Blob Dizin Oluşturucu tarafından üretilen olduğunda ```imageAction``` ayarlanır ```generateNormalizedImages```. Bkz: [örnek](#sample-output) daha fazla bilgi için.|


## <a name="skill-outputs"></a>Beceri çıkışları
| Çıkış adı     | Açıklama                   |
|---------------|-------------------------------|
| metin          | Düz metin görüntüden ayıklanır.   |
| layoutText    | Karmaşık tür, ayıklanan metin, hem de metnin bulunduğu konumu açıklar.|


## <a name="sample-definition"></a>Örnek tanımı

```json
{
    "skills": [
      {
        "description": "Extracts text (plain and structured) from image.",
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

## <a name="sample-text-and-layouttext-output"></a>Örnek metin ve layoutText çıktısı

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

## <a name="sample-merging-text-extracted-from-embedded-images-with-the-content-of-the-document"></a>Örnek: belge içeriğini ile katıştırılmış görüntüler ayıklanan metin birleştiriliyor.

Metin birleştirme için yaygın bir kullanım örneği görüntülerini (OCR beceri veya görüntünün bir açıklamalı alt yazı metni) değerinin metinsel gösterimini birleştirmek için bir belge içerik alanına olanağıdır. 

Aşağıdaki örnek becerilerine oluşturur bir *merged_text* alan OCRed metnin her görüntülerinin yanı sıra, belgenizi metinsel içeriğini içerecek şekilde, bu belgede katıştırılmış. 

#### <a name="request-body-syntax"></a>İstek Gövdesi Sözdizimi
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
Yukarıdaki standartlarındaki şu örnek, bir normalleştirilmiş görüntüleri alan olduğunu varsayar. Bu alan oluşturmak üzere *imageAction* yapılandırma için dizin oluşturucu Tanımınızda *generateNormalizedImages* aşağıda gösterildiği gibi:

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
+ [TextMerger beceri](cognitive-search-skill-textmerger.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Dizin Oluşturucu (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
