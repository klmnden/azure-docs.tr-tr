---
title: Anahtar ifade ayıklama bilişsel arama beceri - Azure Search
description: Yapılandırılmamış metinleri değerlendirir ve her bir kayıt için bir Azure Search zenginleştirme işlem hattı, anahtar ifadeleri listesini döndürür.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 1d221e3bcdfd781da79c73e8f228b9e449a7f5bd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65021808"
---
#   <a name="key-phrase-extraction-cognitive-skill"></a>Anahtar ifade ayıklama bilişsel beceri

**Anahtar ifade ayıklama** beceri yapılandırılmamış metinleri değerlendirir ve her bir kayıt için anahtar ifadeleri listesini döndürür. Bu yetenek, makine öğrenimi modellerini tarafından sağlanan kullanan [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) Bilişsel Hizmetler'e gösterdiğiniz.

Hızla kaydındaki ana konuşma noktalarını tanımlamak gerekirse bu özellik yararlıdır. "Yemek tat ve harika personeli vardı." hizmet "yemek" ve "harika personel" döndürür. Örneğin, belirli bir giriş metni.

> [!NOTE]
> Kapsam işleme sıklığını artırarak daha fazla belgelerin eklenmesi genişletmeniz veya daha fazla yapay ZEKA algoritmalarının eklenmesi gerekir [Faturalanabilir bir Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md). API'leri, Bilişsel hizmetler ve Azure Search'te belge çözme aşamasının bir parçası olarak görüntü ayıklama çağırırken ücretler tahakkuk. Metin ayıklama belgelerden için ücretlendirme yoktur.
>
> Yerleşik yetenek yürütülmesi sırasında mevcut ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/). Görüntü ayıklama fiyatlandırma üzerinde açıklanmıştır [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.KeyPhraseExtractionSkill 

## <a name="data-limits"></a>Veri sınırları
Bir kaydın en büyük boyutu tarafından ölçülen 50.000 karakter arasında olmalıdır `String.Length`. Anahtar ifade ayıklayıcısı için göndermeden önce verileri bölün gerekiyorsa kullanmayı [metin bölme beceri](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Girişler                | Açıklama |
|---------------------|-------------|
| defaultLanguageCode | (İsteğe bağlı) Dil açıkça belirtmeyin belgelerine uygulamak için dil kodu.  Varsayılan dil kodu değil ise, belirtilen, İngilizce (TR) varsayılan dil kodu olarak kullanılır. <br/> Bkz: [desteklenen dillerin tam listesini](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages). |
| maxKeyPhraseCount   | (İsteğe bağlı) Anahtar ifadeler üretmek için maksimum sayısı. |

## <a name="skill-inputs"></a>Beceri girişleri

| Girişler     | Açıklama |
|--------------------|-------------|
| metin | Analiz edilecek metin.|
| languageCode  |  Kayıt dili belirten bir dize. Bu parametre belirtilmezse, varsayılan dil kodu kayıtları çözümlemek için kullanılacak. <br/>Bkz: [desteklenen dillerin tam listesini](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)|

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
Desteklenmeyen dil kodunu sağlarsanız, bir hata oluşturulur ve anahtar tümcecikleri ayıklanmaz.
Metni boş ise, bir uyarı oluşturulur.
Metninizi 50.000 karakterden daha büyük ise, yalnızca ilk 50.000 karakter analiz edilir ve bir uyarı verilir.

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
