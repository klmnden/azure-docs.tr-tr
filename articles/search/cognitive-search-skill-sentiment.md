---
title: Düşünceleri bilişsel arama nitelik (Azure Search) | Microsoft Docs
description: Bir Azure Search iyileştirmesini ardışık düzeninde metinden düşünceleri ayıklayın.
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
ms.openlocfilehash: 1ddbba5b881cd05a997cd24a9396d5b722376e6f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33791003"
---
#   <a name="sentiment-cognitive-skill"></a>Düşünceleri bilişsel nitelik

**Düşünceleri** yetenek yapılandırılmamış metin pozitif negatif sürekliliği ve her kayıt için hesaplar, 0 ile 1 arasında bir sayısal puan döndürür. Puanın 1’e yakın olması yaklaşımın olumlu olduğunu, 0’a yakın olması ise olumsuz olduğunu gösterir.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SentimentSkill

## <a name="data-limits"></a>Veri sınırları
Bir kayıt en büyük boyutuna göre ölçülen 5000 karakter olmalıdır `String.Length`. Kullanım, düşünceleri Çözümleyicisi göndermeden önce verilerinizi bölün gerekiyorsa [metin bölünmüş yetenek](cognitive-search-skill-textsplit.md).


## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreleri büyük/küçük harfe duyarlıdır.

| Parametre Adı |                      |
|----------------|----------------------|
| defaultLanguageCode | (isteğe bağlı) Dil açıkça belirtmeyin belgelerine uygulamak için dil kodu. <br/> Bkz: [tam desteklenen dillerin listesi](../cognitive-services/text-analytics/text-analytics-supported-languages.md) |

## <a name="skill-inputs"></a>Yetenek girişleri 

| Giriş adı | Açıklama |
|--------------------|-------------|
| Metin | Çözümlenecek metin.|
| languageCode  |  (İsteğe bağlı) Kayıtları dilinin gösteren bir dize. Bu parametre belirtilmezse, varsayılan değer "tr" dir. <br/>Bkz: [tam desteklenen dillerin listesi](../cognitive-services/text-analytics/text-analytics-supported-languages.md).|

## <a name="skill-outputs"></a>Yetenek çıkışları

| Çıktı adı | Açıklama |
|--------------------|-------------|
| Puan | 0 ve 1 arasında bir değer, analiz edilen metin düşünceleri temsil eder. Negatif düşünceleri değerleri 0 yakın olan, nötr düşünceleri 0,5 yakın olması ve pozitif düşünceleri 1 yakın değerlere sahip.|


##  <a name="sample-definition"></a>Örnek tanımı

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/languagecode"
        }
    ],
    "outputs": [
        {
            "name": "score",
            "targetName": "mySentiment"
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
            "data": {
                "text": "I had a terrible time at the hotel. The staff was rude and the food was awful.",
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
            "data": {
                "score": 0.01
            }
        }
    ]
}
```

## <a name="notes"></a>Notlar
Boş ise, bir düşünceleri puan kayıtları için döndürülmez.

## <a name="error-cases"></a>Hata durumları
Bir dil desteklenmiyorsa, bir hata oluşturulur ve hiçbir düşünceleri puan döndürülür.

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış yetenekleri](cognitive-search-predefined-skills.md)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)