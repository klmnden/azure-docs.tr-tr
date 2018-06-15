---
title: Metin bölme bilişsel arama nitelik (Azure Search) | Microsoft Docs
description: Metin öbekleri veya bir Azure Search iyileştirmesini ardışık uzunluğu göre metin sayfalarının bölün.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: dbb9261cfce0a8437cfe76121fa16aa87c4b3393
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33791031"
---
#   <a name="text-split-cognitive-skill"></a>Bilişsel yetenek metnini bölme

**Metin bölünmüş** yetenek metin metin parçalara ayırır. Metin cümleleri veya belirli bir uzunlukta sayfa sonu isteyip istemediğinizi belirtebilirsiniz. Diğer becerileri aşağı uzunluk gereksinimlerini maksimum metin varsa bu beceri özellikle yararlıdır. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SplitSkill 

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreleri büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| textSplitMode      | "Sayfalar" veya "cümleleri" | 
| maximumPageLength | TextSplitMode "sayfalara" olarak ayarlanırsa, bu tarafından ölçülen en fazla sayfa ifade eder `String.Length`. En düşük değer 100'dür. | 
| defaultLanguageCode   | (isteğe bağlı) Aşağıdaki dil kodlarından birini: `da, de, en, es, fi, fr, it, ko, pt`. İngilizce (TR) varsayılandır. Birkaç göz önünde bulundurmanız gerekenler:<ul><li>Bir languagecode countrycode biçimi geçirirseniz, yalnızca languagecode parçası biçimi kullanılır.</li><li>Dil önceki listede değilse, bölünmüş yetenek karakter sınırlarında metni keser.</li><li>Bir dil kodu sağlama ikiye Çince, Japonca ve Kore dili gibi boşluk olmayan diller için bir sözcük kesme önlemek kullanışlıdır.</li></ul>  |


## <a name="skill-inputs"></a>Yetenek girişleri

| Parametre adı       | Açıklama      |
|----------------------|------------------|
| Metin  | Dizeye bölmek için metin. |
| languageCode  | (İsteğe bağlı) Belge için dil kodu.  |

## <a name="skill-outputs"></a>Yetenek çıkışları 

| Parametre adı     | Açıklama |
|--------------------|-------------|
| textItems | Ayıklanan alt dizeler dizisi. |


##  <a name="sample-definition"></a>Örnek tanımı

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
    "textSplitMode" : "pages", 
    "maximumPageLength": 1000,
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/language"
        }
    ],
    "outputs": [
        {
            "name": "textItems",
            "targetName": "mypages"
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
                "text": "This is a the loan application for Joe Romero, he is a Microsoft employee who was born in Chile and then moved to Australia…",
                "languageCode": "en"
            }
        },
        {
            "recordId": "2",
            "data": {
                "text": "This is the second document, which will be broken into several pages...",
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
                "textItems": [
                    "This is the loan…",
                    "On the second page we…"
                ]
            }
        },
        {
            "recordId": "2",
            "data": {
                "textItems": [
                    "This is the second document...",
                    "On the second page of the second doc…"
                ]
            }
        }
    ]
}
```

## <a name="error-cases"></a>Hata durumları
Bir dil desteklenmiyorsa, bir uyarı oluşturulur ve metin karakter sınırlarında ayrılır.

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış yetenekleri](cognitive-search-predefined-skills.md)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)
