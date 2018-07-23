---
title: Dil algılama bilişsel arama beceri (Azure Search) | Microsoft Docs
description: Yapılandırılmamış metinleri değerlendirir ve her bir kayıt için bir dil tanımlayıcısı bir Azure Search zenginleştirme işlem hattı analiz gücünü gösteren bir puan döndürür.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 443ac895085053b7c4c876c3deecaa1943c9f506
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171067"
---
#   <a name="language-detection-cognitive-skill"></a>Dil algılama bilişsel beceri

En fazla 120 dil için **dil algılama** beceri giriş metin dilini algılar ve istekte gönderilen her belge için bir tek dil kodu bildirir. Dil kodu çözümleme gücünü gösteren bir puanı ile eşleştirilir.

Metnin dilini diğer becerileri giriş olarak sağlamak, ihtiyacınız olduğunda bu beceri özellikle yararlı olur (örneğin, [yaklaşım analizi beceri](cognitive-search-skill-sentiment.md) veya [metin bölme beceri](cognitive-search-skill-textsplit.md)).

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.LanguageDetectionSkill

## <a name="data-limits"></a>Veri sınırları
Bir kaydın en büyük boyutu tarafından ölçülen 50.000 karakter arasında olmalıdır `String.Length`. Yaklaşım Çözümleyicisi göndermeden önce verileri bölün gerekiyorsa kullanabilir [metin bölme beceri](cognitive-search-skill-textsplit.md).

## <a name="skill-inputs"></a>Beceri girişleri

Parametreler büyük/küçük harfe duyarlıdır.

| Girişler     | Açıklama |
|--------------------|-------------|
| metin | Analiz edilecek metin.|

## <a name="skill-outputs"></a>Beceri çıkışları

| Çıkış adı    | Açıklama |
|--------------------|-------------|
| languageCode | Tanımlanan dilin ISO 6391 dil kodu. Örneğin, "en". |
| LanguageName | Dil adı. Örneğin "İngilizce". |
| puan | 0 ile 1 arasında bir değer. Olasılığı dil doğru şekilde tanımlanır. Cümlenin dilleri karıştırıldığında puanın 1'den daha düşük olabilir.  |

##  <a name="sample-definition"></a>Örnek tanımı

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill ",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      }
    ],
    "outputs": [
      {
        "name": "languageCode",
        "targetName": "myLanguageCode"
      },
      {
        "name": "languageName",
        "targetName": "myLanguageName"
      },
      {
        "name": "score",
        "targetName": "myLanguageScore"
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
             "text": "Glaciers are huge rivers of ice that ooze their way over land, powered by gravity and their own sheer weight. "
           }
      },
      {
        "recordId": "2",
        "data":
           {
             "text": "Estamos muy felices de estar con ustedes."
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
              "languageCode": "en",
              "languageName": "English",
              "score": 1,
            }
      },
      {
        "recordId": "2",
        "data":
            {
              "languageCode": "es",
              "languageName": "Spanish",
              "score": 1,
            }
      }
    ]
}
```


## <a name="error-cases"></a>Hata durumları
Metin, desteklenmeyen bir dilde ifade, bir hata oluşturulur ve herhangi bir dil tanımlayıcısı döndürülür.

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
