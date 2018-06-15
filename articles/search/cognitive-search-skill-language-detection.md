---
title: Dil algılama bilişsel arama nitelik (Azure Search) | Microsoft Docs
description: Yapılandırılmamış metin değerlendirir ve her kayıt için bir dil tanımlayıcısı bir Azure Search iyileştirmesini ardışık analiz gücünü gösteren bir puan döndürür.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 338d89b47ea451efcf8300d4ac016a6946a95259
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33791059"
---
#   <a name="language-detection-cognitive-skill"></a>Dil algılama bilişsel nitelik

En fazla 120 diller için **dil algılama** yetenek giriş metin dilini algılar ve isteği gönderilen her belge için bir tek dil kodu raporlar. Dil kodu analiz gücünü gösteren bir puan ile eşleştirilmiş.

Diğer becerileri giriş olarak metin dili sağlamanız gerektiğinde bu beceri özellikle yararlı olur (örneğin, [düşünceleri Snalysis yetenek](cognitive-search-skill-sentiment.md) veya [metin bölünmüş yetenek](cognitive-search-skill-textsplit.md)).

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.LanguageDetectionSkill

## <a name="data-limits"></a>Veri sınırları
Bir kayıt en büyük boyutuna göre ölçülen 50.000 karakter olmalıdır `String.Length`. Düşünceleri Çözümleyicisi göndermeden önce verilerinizi ayırmanız gerekiyorsa, kullanabilir [metin bölünmüş yetenek](cognitive-search-skill-textsplit.md).

## <a name="skill-inputs"></a>Yetenek girişleri

Parametreleri büyük/küçük harfe duyarlıdır.

| Girişler     | Açıklama |
|--------------------|-------------|
| Metin | Çözümlenecek metin.|

## <a name="skill-outputs"></a>Yetenek çıkışları

| Çıktı adı    | Açıklama |
|--------------------|-------------|
| languageCode | Tanımlanan dilde ISO 6391 dil kodu. Örneğin, "tr". |
| LanguageName | Dil adı. Örneğin "İngilizce". |
| Puan | 0 ile 1 arasında bir değer. Olasılığını dil doğru şekilde tanımlanır. Tümce dilleri karışık varsa puanı 1'den daha düşük olabilir.  |

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
Metin desteklenmeyen bir dille ifade edilen, bir hata oluşturulur ve hiçbir dil tanımlayıcısı döndürülür.

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış yetenekleri](cognitive-search-predefined-skills.md)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)