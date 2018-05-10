---
title: Anahtar tümcecik ayıklama bilişsel arama nitelik (Azure Search) | Microsoft Docs
description: Yapılandırılmamış metin değerlendirir ve her kayıt için bir Azure Search iyileştirmesini ardışık düzeninde anahtar tümcecikleri listesini döndürür.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: a12efaa020e626e4a10a0708c9b84d8fe125588c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
#   <a name="key-phrase-extraction-cognitive-skill"></a>Anahtar tümcecik ayıklama bilişsel nitelik

**Anahtar tümcecik ayıklama** yetenek yapılandırılmamış metin değerlendirir ve anahtar tümcecikleri listesini her bir kaydı döndürür.

Bu özellik hızla kaydındaki ana Konuşmayı noktalarını tanımlamak gerektiğinde kullanışlı olur. "Yemek Lezzetli ve harika personel vardı", "yemek" ve "harika personeli" hizmeti döndürür örnek için belirtilen giriş metni.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.KeyPhraseExtractionSkill 

## <a name="data-limits"></a>Veri sınırları
Bir kayıt en büyük boyutuna göre ölçülen 50.000 karakter olmalıdır `String.Length`. Anahtar tümcecik ayıklayıcısı göndermeden önce verilerinizi ayırmanız gerekiyorsa, kullanmayı [metin bölünmüş yetenek](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreleri büyük/küçük harfe duyarlıdır.
| Girişler                | Açıklama |
|---------------------|-------------|
| defaultLanguageCode | (İsteğe bağlı) Dil açıkça belirtmeyin belgelerine uygulamak için dil kodu.  Varsayılan dil kodu değilse belirtilen, İngilizce (TR) varsayılan dil kodu olarak kullanılır. <br/> Bkz: [tam desteklenen dillerin listesi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages). |
| maxKeyPhraseCount   | (İsteğe bağlı) Üretmek için anahtar tümcecikleri maksimum sayısı. |

## <a name="skill-inputs"></a>Yetenek girişleri
| Girişler     | Açıklama |
|--------------------|-------------|
| Metin | Çözümlenecek metin.|
| languageCode  |  Kayıtları dilinin gösteren bir dize. Bu parametre belirtilmezse, varsayılan dil kodu kayıtları çözümlemek için kullanılır. <br/>Bkz: [tam desteklenen dillerin listesi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)|

##  <a name="sample-definition"></a>Örnek tanımı

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      },
      {
        "name": "languageCode",
        "source": "/document/languagecode" 
      }
    ],
    "outputs": [
      {
        "name": "keyPhrases",
        "targetName": "myKeyPhrases"
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
             "text": "Glaciers are huge rivers of ice that ooze their way over land, powered by gravity and their own sheer weight. They accumulate ice from snowfall and lose it through melting. As global temperatures have risen, many of the world’s glaciers have already started to shrink and retreat. Continued warming could see many iconic landscapes – from the Canadian Rockies to the Mount Everest region of the Himalayas – lose almost all their glaciers by the end of the century.",
             "language": "en"
           }
      }
    ]
```


##  <a name="sample-output"></a>Örnek çıktı

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
            "keyPhrases": 
            [
              "world’s glaciers", 
              "huge rivers of ice", 
              "Canadian Rockies", 
              "iconic landscapes",
              "Mount Everest region",
              "Continued warming"
            ]
           }
      }
    ]
}
```


## <a name="errors-and-warnings"></a>Hatalar ve uyarılar
Desteklenmeyen dil kodu sağlarsanız, bir hata oluşturulur ve anahtar tümcecikleri değil ayıklanır.
Metni boş ise, bir uyarı oluşturulur.
Metninizi 50.000 karakterden büyükse, yalnızca ilk 50.000 karakter analiz ve bir uyarı verilir.

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış yetenekleri](cognitive-search-predefined-skills.md)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)