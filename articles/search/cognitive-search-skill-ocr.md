---
title: OCR bilişsel arama nitelik (Azure Search) | Microsoft Docs
description: Bir Azure Search iyileştirmesini ardışık görüntü dosyaları metin Al.
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
ms.openlocfilehash: 478afe81ed739b98487973eb092ee9cad0aa17fd
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059194"
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
|defaultLanguageCode | <p>  Giriş metni dil kodu. Desteklenen diller: <br/> zh-atanır (ChineseSimplified) <br/> zh-Hant (ChineseTraditional) <br/>cs (Çekçe) <br/>da (Danimarka) <br/>NL (Hollanda dili) <br/>tr (İngilizce) <br/>Fi (Fince)  <br/>FR (Fransızca) <br/>  de (Almanca) <br/>el (Yunanca) <br/> hu (Macarca) <br/> Bunu (İtalyanca) <br/>  ja (Japonca) <br/> Ko (Korece) <br/> NB (Norveççe) <br/>   PL (Lehçe) <br/> PT (Portekiz) <br/>  RU (Rusça) <br/>  ES (İspanyolca) <br/>  sv (İsveççe) <br/>  tr (Türkçe) <br/> ar (Arapça) <br/> Ro (Rumence) <br/> SR-Cyrl (SerbianCyrillic) <br/> SR-Latn (SerbianLatin) <br/>  SK (Slovakça). <br/>  UNK (bilinmiyor) <br/><br/> Dil kodu belirtilmemiş veya null ise, autodetected dilidir. </p> |
| textExtractionAlgorithm | "yazdırılan" veya "el yazısı". "El yazısı" metin tanıma OCR algoritması şu anda önizlemede ve yalnızca İngilizce olarak desteklenir. |

## <a name="skill-inputs"></a>Yetenek girişleri

| Giriş adı      | Açıklama                                          |
|---------------|------------------------------------------------------|
| image         | Karmaşık türü. "/ Belge/normalized_images" alan şu anda yalnızca çalışır Azure Blob Oluşturucu tarafından üretilen zaman ```imageAction``` ayarlanır ```generateNormalizedImages```. Bkz: [örnek](#sample-output) daha fazla bilgi için.|


## <a name="skill-outputs"></a>Yetenek çıkışları
| Çıktı adı     | Açıklama                   |
|---------------|-------------------------------|
| metin          | Düz metin görüntüden ayıklanır.   |
| layoutText    | Karmaşık türü, metnin bulunduğu konumun yanı sıra ayıklanan metin açıklar.|


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
Yukarıdaki skillset örnek normalleştirilmiş görüntüleri alan var olduğunu varsayar. Bu alan oluşturmak üzere *imageAction* dizin oluşturucu tanımınızı yapılandırmasında *generateNormalizedImages* aşağıda gösterildiği gibi:

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
+ [Önceden tanımlanmış yetenekleri](cognitive-search-predefined-skills.md)
+ [TextMerger nitelik](cognitive-search-skill-textmerger.md)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)
+ [Dizin Oluşturucu (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
