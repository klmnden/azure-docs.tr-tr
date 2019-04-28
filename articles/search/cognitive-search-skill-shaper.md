---
title: Shaper bilişsel arama beceri - Azure Search
description: Meta veriler ve yapılandırılmış bilgiler yapılandırılmamış verileri ayıklayın ve bir Azure Search zenginleştirme işlem hattı karmaşık tür olarak şekil.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: c55783e9b209a1280a21edca34b75e72481f4cb6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61127079"
---
#   <a name="shaper-cognitive-skill"></a>Shaper bilişsel beceri

**Shaper** beceri birkaç girişten sonra zenginleştirme işlem hattı, başvurulan bir karmaşık türü birleştirir. **Shaper** beceri temelde bir yapı oluşturmak, bu yapının üyelerine adını tanımlayın ve her üye için değerler atayın olanak tanır. Birleştirilmiş alanlar arama senaryolarda yararlı bir tek yapısı, şehir ve tek yapısı veya adı durumuna ve doğum tarihi benzersiz kimliğini oluşturmak için tek bir yapıda adı ve Soyadı birleştirme verilebilir.

Varsayılan olarak, bu teknik bir düzey derin olan nesneleri destekler. Daha karmaşık nesneler için birkaç bağlayabilirsiniz(ekleyebilirsiniz) **Shaper** adımları.

Yanıt olarak, çıkış adı her zaman "çıkış". Dahili olarak, işlem hattı "analyzedText" gibi farklı bir ad "çıkış için", aşağıdaki örneklerde eşleyebilirsiniz ancak **Shaper** yetenek kendi yanıtta "çıkış" döndürür. Özel bir yetenek oluşturun ve yanıt kendiniz yapılandırılması bu zenginleştirilmiş belgeleri hata ayıklaması yapıyorsanız ve adlandırma tutarsızlık dikkat edin veya önemli olabilir.

> [!NOTE]
> Bu yetenek bir Bilişsel hizmetler API'sine bağlı değil ve kullanmak için ücretlendirilmez. Hala [Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md), ancak geçersiz kılmak için **ücretsiz** , az sayıda gün başına günlük zenginleştirmelerinin sınırlar resource seçeneği.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ShaperSkill

## <a name="sample-1-complex-types"></a>Örnek 1: karmaşık türler

Adlı bir yapı oluşturmak istediğiniz bir senaryo düşünün *analyzedText* iki üyesi olan: *metin* ve *yaklaşım*sırasıyla. Azure Search'te çok parçalı aranabilir bir alanı olarak adlandırılan bir *karmaşık tür*, ve kullanıma hazır henüz desteklenmiyor. Bu önizleme sürümünde bir **Shaper** yetenek, dizininizdeki bir karmaşık türü alanları oluşturmak için kullanılabilir. 

Aşağıdaki örnekte, giriş olarak üye adları sağlar. Çıktı yapısını (Azure Search karmaşık, alan) aracılığıyla belirtilen *targetName*. 


```json
{
  "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
  "context": "/document/content/phrases/*",
  "inputs": [
    {
      "name": "text",
      "source": "/document/content/phrases/*"
    },
    {
      "name": "sentiment",
      "source": "/document/content/phrases/*/sentiment"
    }
  ],
  "outputs": [
    {
      "name": "output",
      "targetName": "analyzedText"
    }
  ]
}
```

### <a name="sample-input"></a>Örnek Giriş
Bu kullanışlı girişi sağlayan bir JSON belgesi **Shaper** beceri olabilir:

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "this movie is awesome",
                "sentiment": 0.9
            }
        }
    ]
}
```


### <a name="sample-output"></a>Örnek çıktı
**Shaper** beceri adlı yeni bir öğe oluşturur *analyzedText* birleşik öğeleri *metin* ve *yaklaşım*. 

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
            "analyzedText": 
              {
                "text": "this movie is awesome" ,
                "sentiment": 0.9
              }
           }
      }
    ]
}
```

## <a name="sample-2-input-consolidation"></a>Örnek 2: Giriş birleştirme

Başka bir örnekte, ardışık düzen işleme farklı aşamalarında, bir kitap başlığı ve bölüm başlıkları kitabın farklı sayfalarında ayıkladığınız olduğunu varsayın. Artık bu çeşitli girişleri oluşan tek bir yapı oluşturabilir.

Bu senaryo için Shaper beceri tanımı aşağıdaki örnekteki gibi görünebilir:

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "context": "/document",
    "inputs": [
        {
            "name": "title",
            "source": "/document/content/title"
        },
        {
            "name": "chapterTitles",
            "source": "/document/content/pages/*/chapterTitles/*"
        }
    ],
    "outputs": [
        {
            "name": "output",
            "targetName": "titlesAndChapters"
        }
    ]
}
```

### <a name="sample-output"></a>Örnek çıktı
Bu durumda, tek bir dizi oluşturmak için tüm bölüm başlıkları Shaper düzleştirir. 

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "titlesAndChapters": {
                    "title": "How to be happy",
                    "chapterTitles": [
                        "Start young",
                        "Laugh often",
                        "Eat, sleep and exercise"
                    ]
                }
            }
        }
    ]
}
```

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)

