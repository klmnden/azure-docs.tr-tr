---
title: Adlandırılmış varlık tanıma bilişsel arama beceri (Azure Search) | Microsoft Docs
description: Adlandırılmış varlıklar kişi, konum ve kuruluş için bir Azure Search bilişsel arama ardışık metinden ayıklayın.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 653a4675d546432eea8478ba6203be1df71ec4f4
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45731402"
---
#    <a name="named-entity-recognition-cognitive-skill"></a>Adlandırılmış varlık tanıma bilişsel beceri

**Adlandırılmış varlık tanıma** beceri adlandırılmış varlıklar metni ayıklar. Var olan varlıkları içeren türler `person`, `location`, ve `organization`.

> [!NOTE]
> Bilişsel Arama, genel önizleme aşamasındadır. Görüntü ayıklama ve normalleştirme ve beceri yürütmesi şu anda ücretsiz sunulmaktadır. Daha sonraki bir zamanda, bu özelliklerin fiyatlandırması duyurulacaktır. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.NamedEntityRecognitionSkill

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| kategoriler    | Ayıklanması gereken kategoriler dizisi.  Olası kategori türleri: `"Person"`, `"Location"`, `"Organization"`. Hiçbir kategori sağlanırsa, tüm türleri döndürülür.|
|defaultLanguageCode |  Giriş metni dil kodu. Aşağıdaki dillerde desteklenmektedir: `ar, cs, da, de, en, es, fi, fr, he, hu, it, ko, pt-br, pt`|
| minimumPrecision  | 0 ile 1 arasında bir sayı. Duyarlık bu değerden düşükse, varlık döndürülmez. Varsayılan değer 0'dır.|

## <a name="skill-inputs"></a>Beceri girişleri

| Adı girin      | Açıklama                   |
|---------------|-------------------------------|
| languageCode  | İsteğe bağlı. `"en"` varsayılan değerdir.  |
| metin          | Analiz edilecek metin.          |

## <a name="skill-outputs"></a>Beceri çıkışları

| Çıkış adı     | Açıklama                   |
|---------------|-------------------------------|
| Kişiler      | Her bir dizenin bir kişinin adını temsil ettiği bir dize dizisi. |
| konumlar  | Her bir dizenin bir konumu temsil ettiği bir dize dizisi. |
| organizations  | Bir kuruluş temsil ettiği her bir dizenin dize dizisi. |
| varlıklar | Karmaşık bir tür dizisi. Her bir karmaşık türü, aşağıdaki alanları içerir: <ul><li>Kategori (`"person"`, `"organization"`, veya `"location"`)</li> <li>değer (gerçek varlık adı)</li><li>uzaklık (Bu metnin bulunduğu konumu)</li><li>güvenle (0 ve 1 arasında bir değer güvenin değerin gerçek bir varlık olduğunu gösterir)</li></ul> |

##  <a name="sample-definition"></a>Örnek tanımı

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
    "categories": [ "Person"],
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
      }
    ]
  }
```
##  <a name="sample-input"></a>Örnek Giriş

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "This is a the loan application for Joe Romero, he is a Microsoft employee who was born in Chile and then moved to Australia… Ana Smith is provided as a reference.",
             "languageCode": "en"
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Örnek çıktı

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "persons": [ "Joe Romero", "Ana Smith"],
        "locations": ["Seattle"],
        "organizations":["Microsoft Corporation"],
        "entities":  
        [
          {
            "category":"person",
            "value": "Joe Romero",
            "offset": 45,
            "confidence": 0.87
          },
          {
            "category":"person",
            "value": "Ana Smith",
            "offset": 59,
            "confidence": 0.87
          },
          {
            "category":"location",
            "value": "Seattle",
            "offset": 5,
            "confidence": 0.99
          },
          {
            "category":"organization",
            "value": "Microsoft Corporation",
            "offset": 120,
            "confidence": 0.99
          }
        ]
      }
    }
  ]
}
```


## <a name="error-cases"></a>Hata durumları
Desteklenmeyen dil kodu belirtin veya belirtilen dil içeriği eşleşmiyorsa, bir hata dönüş ve varlık yok ayrıştırıcısıdır.

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)