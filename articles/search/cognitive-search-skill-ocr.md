---
title: OCR bilişsel arama beceri - Azure Search
description: Metin, görüntü dosyalarını bir Azure Search zenginleştirme işlem hattı, optik karakter tanıma (OCR) kullanarak çıkarın.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 9bc4167134bb70fa938ecd37d81482dc4e1508dd
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65021804"
---
# <a name="ocr-cognitive-skill"></a>OCR bilişsel beceri

Optik karakter tanıma (OCR) beceri görüntü dosyaları yazdırılan ve el yazısı metinde tanır. Bu yetenek, makine öğrenimi modellerini tarafından sağlanan kullanan [görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) Bilişsel Hizmetler'e gösterdiğiniz. **OCR** yetenek aşağıdaki işlevsellik eşler:

+ TextExtractionAlgorithm için "yazısı", olarak ayarlandığında ["RecognizeText"](../cognitive-services/computer-vision/quickstarts-sdk/csharp-hand-text-sdk.md) işlevi kullanılır.
+ TextExtractionAlgorithm için "yazdırılan" olarak ayarlandığında ["OCR"](../cognitive-services/computer-vision/concept-extracting-text-ocr.md) işlevselliği, İngilizce dışındaki diller için kullanılır. İngilizce, yeni ["Metin tanıma"](../cognitive-services/computer-vision/concept-recognizing-text.md) işlevselliği yazdırılan metin için kullanılır.

**OCR** beceri görüntü dosyalarından metin ayıklar. Desteklenen dosya biçimleri şunlardır:

+ .JPEG
+ . JPG
+ .PNG
+ . BMP
+ .GIF
+ . TIFF

> [!NOTE]
> Kapsam işleme sıklığını artırarak daha fazla belgelerin eklenmesi genişletmeniz veya daha fazla yapay ZEKA algoritmalarının eklenmesi gerekir [Faturalanabilir bir Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md). API'leri, Bilişsel hizmetler ve Azure Search'te belge çözme aşamasının bir parçası olarak görüntü ayıklama çağırırken ücretler tahakkuk. Metin ayıklama belgelerden için ücretlendirme yoktur.
>
> Yerleşik yetenek yürütülmesi sırasında mevcut ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/). Görüntü ayıklama fiyatlandırma üzerinde açıklanmıştır [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| detectOrientation | Görüntü Yönü'nın intellisense sağlar. <br/> Geçerli değerler: true / false.|
|defaultLanguageCode | <p>  Giriş metni dil kodu. Desteklenen diller: <br/> zh-Hans (ChineseSimplified) <br/> zh-Hant (ChineseTraditional) <br/>cs (Çekçe) <br/>da (Danimarka) <br/>NL (Hollanda dili) <br/>tr (Türkçe) <br/>Fi (Fince)  <br/>FR (Fransızca) <br/>  de (Almanya) <br/>el (Yunanca) <br/> hu (Macarca) <br/> Bu (İtalyanca) <br/>  ja (Japonca) <br/> Ko (Korece) <br/> NB (Norveç dili) <br/>   PL (Lehçe) <br/> PT (Portekizce) <br/>  RU (Rusça) <br/>  ES (İspanyolca) <br/>  sv (İsveç dili) <br/>  tr (Türkçe) <br/> ar (Arapça) <br/> Ro (Rumence) <br/> SR-Cyrl (SerbianCyrillic) <br/> SR-Latn (SerbianLatin) <br/>  SK (Slovakya). <br/>  UNK (bilinmiyor) <br/><br/> Dil kodu belirtilmemiş veya null ise, dil İngilizce'ye ayarlanır. Dil "unk" için açıkça ayarlanmış ise, dil otomatik olarak algılanır. </p> |
| textExtractionAlgorithm | "yazılı" veya "el yazısı". "El yazısı" metin tanıma OCR algoritması, şu anda Önizleme aşamasındadır ve yalnızca İngilizce olarak desteklenmektedir. |

## <a name="skill-inputs"></a>Beceri girişleri

| Giriş adı      | Açıklama                                          |
|---------------|------------------------------------------------------|
| image         | Karmaşık tür. "/ Belge/normalized_images" alan şu anda yalnızca çalışır, Azure Blob Dizin Oluşturucu tarafından üretilen olduğunda ```imageAction``` dışında bir değere ayarlanmış ```none```. Bkz: [örnek](#sample-output) daha fazla bilgi için.|


## <a name="skill-outputs"></a>Beceri çıkışları
| Çıkış adı     | Açıklama                   |
|---------------|-------------------------------|
| metin          | Düz metin görüntüden ayıklanır.   |
| layoutText    | Karmaşık tür, ayıklanan metin ve metnin bulunduğu konumu açıklar.|


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

## <a name="sample-merging-text-extracted-from-embedded-images-with-the-content-of-the-document"></a>Örnek: Metin belgesinin içeriği ile katıştırılmış görüntüler ayıklanan birleştiriliyor.

Metin birleştirme için yaygın bir kullanım örneği görüntülerini (OCR beceri veya görüntünün bir açıklamalı alt yazı metni) değerinin metinsel gösterimini birleştirmek için bir belge içerik alanına olanağıdır.

Aşağıdaki örnek becerilerine oluşturur bir *merged_text* alan. Bu alan, belge ve bu belgeye görüntülerin her OCRed metin metinsel içeriği içerir.

#### <a name="request-body-syntax"></a>İstek Gövdesi Sözdizimi
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
Yukarıdaki standartlarındaki şu örnek, bir normalleştirilmiş görüntüleri alan olduğunu varsayar. Bu alan oluşturmak üzere *imageAction* yapılandırma için dizin oluşturucu Tanımınızda *generateNormalizedImages* aşağıda gösterildiği gibi:

```json
{
  //...rest of your indexer definition goes here ...
  "parameters": {
    "configuration": {
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
