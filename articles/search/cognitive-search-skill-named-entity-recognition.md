---
title: Varlık tanıma bilişsel arama nitelik (Azure Search) adlı | Microsoft Docs
description: Adlandırılmış varlıklar kişi, konumu ve kuruluş için bir Azure Search bilişsel arama ardışık metinde Al.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 73ffcf5e2ced63fddaf0f5ef2ca7e72a7d94b966
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33791045"
---
#    <a name="named-entity-recognition-cognitive-skill"></a>Adlandırılmış varlık tanıma bilişsel nitelik

**Adlı varlık tanıma** yetenek metinden adlandırılmış varlıklar ayıklar. Kullanılabilir varlıklar dahil türleri `person`, `location`, ve `organization`.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.NamedEntityRecognitionSkill

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreleri büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| kategoriler    | Ayıklanması gereken kategoriler dizisi.  Olası kategori türleri: `"Person"`, `"Location"`, `"Organization"`. Hiçbir kategori sağlanırsa, tüm türleri döndürülür.|
|defaultLanguageCode |  Giriş metni dil kodu. Aşağıdaki dilleri desteklenir: `ar, cs, da, de, en, es, fi, fr, he, hu, it, ko, pt-br, pt`|
| minimumPrecision  | 0 ile 1 arasında bir sayı. Duyarlık bu değerden daha düşük ise, varlık döndürülmez. Varsayılan değer 0'dır.|

## <a name="skill-inputs"></a>Yetenek girişleri

| Giriş adı      | Açıklama                   |
|---------------|-------------------------------|
| languageCode  | İsteğe bağlı. `"en"` varsayılan değerdir.  |
| Metin          | Analiz etmek için metin.          |

## <a name="skill-outputs"></a>Yetenek çıkışları

| Çıktı adı     | Açıklama                   |
|---------------|-------------------------------|
| kişi      | Her bir dize bir kişinin adını temsil ettiği bir dizeler dizisi. |
| Konumları  | Her bir dize bir konumu temsil ettiği bir dizeler dizisi. |
| Kuruluşlar  | Her bir dize kuruluş temsil ettiği bir dizeler dizisi. |
| varlıklar | Karmaşık türleri dizisi. Her karmaşık türü aşağıdaki alanları içerir: <ul><li>Kategori (`"person"`, `"organization"`, veya `"location"`)</li> <li>değer (gerçek bir varlık adı)</li><li>uzaklık (burada metinde bulunamadı konum)</li><li>GÜVENİRLİK (0 ve 1 arasında bir değer güvenin değerin gerçek bir varlık olduğunu gösterir)</li></ul> |

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
Desteklenmeyen dil kodu belirtin ya da içerik belirtilen dil eşleşmiyorsa, bir dönüş bir hatadır ve varlık ayıklanır.

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış yetenekleri](cognitive-search-predefined-skills.md)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)