---
title: Shaper bilişsel arama nitelik (Azure Search) | Microsoft Docs
description: Meta veri ve yapılandırılmış bilgiler yapılandırılmamış verileri ayıklamak ve bir Azure Search iyileştirmesini ardışık düzeninde bir karmaşık tür olarak şekil.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 311f4bd67081de567763783a9d86540eda36d9f8
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
#   <a name="shaper-cognitive-skill"></a>Shaper bilişsel nitelik

**Shaper** yetenek bileşik alanlar (çok parçalı alanlar olarak da bilinir) desteklemek için karmaşık bir tür oluşturur. Karmaşık Tür alanında birden çok bölümlü ancak Azure Search dizini içinde tek bir öğe olarak kabul edilir. Tek bir alan, şehir ve tek bir alan veya adı durumuna ve doğum tarihi benzersiz kimliğini oluşturmak için tek bir alana adı ve Soyadı birleştirme birleştirilmiş alanları arama senaryolarda yararlı örnekleri içerir.

Shaper yetenek, aslında bir yapı oluşturmak, yapıyı üyeleri adını tanımlayın ve her üyesine değerleri atamak sağlar.

Varsayılan olarak, bu teknik bir düzey derin olan nesneleri destekler. Daha karmaşık nesneler için çeşitli Shaper adımları zincir.

Yanıtta, çıktı adı her zaman "çıkış". Dahili olarak, ardışık düzen farklı bir ad eşleyebilirsiniz, "çıktı" ancak Shaper aşağıdaki örneklerde "analyzedText" gibi yetenek kendisini "çıktı" yanıt olarak döndürür. Özel bir yetenek oluşturmak ve yanıt kendiniz yapılandırılması bu zenginleştirilmiş belgeleri hata ayıklama ve adlandırma tutarsızlık dikkat edin veya, önemli olabilir.


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ShaperSkill

## <a name="sample-1-complex-types"></a>Örnek 1: karmaşık türler

Adlı bir yapı oluşturmak istediğiniz bir senaryo düşünün *analyzedText* iki üyesi olan: *metin* ve *düşünceleri*sırasıyla. Azure Search'te çok parçalı bir aranabilir alan adı verilen bir *karmaşık tür*, ve kullanıma hazır henüz desteklenmiyor. Bu Önizleme'de, bir Shaper yetenek dizininizdeki alanları bir karmaşık tür oluşturmak için kullanılabilir. 

Aşağıdaki örnek, girdi olarak adları üye sağlar. Çıktı yapısı (Azure Search karmaşık kendi alanına) aracılığıyla belirtilen *targetName*. 


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
      "targetName": analyzedText"
    }
  ]
}
```

### <a name="sample-input"></a>Örnek Giriş
Bu Shaper yetenek için kullanılabilir giriş sağlayan bir JSON belgesi aşağıdakilerden biri olabilir:

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
Adlı yeni bir öğe Shaper yetenek oluşturur *analyzedText* birleşik öğeleri *metin* ve *düşünceleri*. 

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

Başka bir örnekte, ardışık düzen işleme farklı aşamalarında kitap başlığı ve bölüm başlıkları defterinin farklı sayfalarında ayıkladığınız olduğunu düşünün. Şimdi çeşitli bu girişleri oluşan tek bir yapı oluşturabilir.

Bu senaryo için Shaper yetenek tanımı aşağıdaki gibi görünebilir:

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
            "output": "titlesAndChapters",
            "targetName": "analyzedText"
        }
    ]
}
```

### <a name="sample-output"></a>Örnek çıktı
Bu durumda, tek bir dizi oluşturmak için tüm bölüm başlıklarını Shaper düzleştirir. 

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

+ [Önceden tanımlanmış yetenekleri](cognitive-search-predefined-skills.md)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)

