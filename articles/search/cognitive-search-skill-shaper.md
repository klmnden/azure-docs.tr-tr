---
title: Shaper bilişsel arama beceri (Azure Search) | Microsoft Docs
description: Meta veriler ve yapılandırılmış bilgiler yapılandırılmamış verileri ayıklayın ve bir Azure Search zenginleştirme işlem hattı karmaşık tür olarak şekil.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 286e1f9d6f6ae09d98aa87b447df7a7524642a1f
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729012"
---
#   <a name="shaper-cognitive-skill"></a>Shaper bilişsel beceri

**Shaper** beceri bileşik alanlarını (çok bölümlü alanlar olarak da bilinir) desteklemek için bir karmaşık türü oluşturur. Bir karmaşık tür alanı birden çok bölümden oluşur, ancak Azure Search dizini içinde tek bir öğe olarak kabul edilir. Birleştirilmiş alanlar arama senaryolarda yararlı bir tek bir alan, şehir ve tek alan veya adı durumuna ve doğum tarihi benzersiz kimliğini oluşturmak için tek bir alanına adı ve Soyadı birleştirme verilebilir.

Shaper yetenek, aslında bir yapı oluşturmak, yapı üyelerinin adını tanımlayın ve her üye için değerler atayın olanak tanır.

Varsayılan olarak, bu teknik bir düzey derin olan nesneleri destekler. Daha karmaşık nesneler için çeşitli Shaper adımları zincirleyebilirsiniz.

Yanıt olarak, çıkış adı her zaman "çıkış". Dahili olarak, işlem hattı farklı bir ad eşleyebilirsiniz, "çıkış" ancak Shaper aşağıdaki örneklerde "analyzedText" gibi beceri kendisini "çıkış" yanıt olarak döndürür. Özel bir yetenek oluşturun ve yanıt kendiniz yapılandırılması bu zenginleştirilmiş belgeleri hata ayıklaması yapıyorsanız ve adlandırma tutarsızlık dikkat edin veya önemli olabilir.

> [!NOTE]
> Bilişsel Arama, genel önizleme aşamasındadır. Görüntü ayıklama ve normalleştirme ve beceri yürütmesi şu anda ücretsiz sunulmaktadır. Daha sonraki bir zamanda, bu özelliklerin fiyatlandırması duyurulacaktır. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ShaperSkill

## <a name="sample-1-complex-types"></a>Örnek 1: karmaşık türler

Adlı bir yapı oluşturmak istediğiniz bir senaryo düşünün *analyzedText* iki üyesi olan: *metin* ve *yaklaşım*sırasıyla. Azure Search'te çok parçalı aranabilir bir alanı olarak adlandırılan bir *karmaşık tür*, ve kullanıma hazır henüz desteklenmiyor. Bu önizleme sürümünde Shaper beceri dizininizdeki bir karmaşık türü alanları oluşturmak için kullanılabilir. 

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
      "targetName": analyzedText"
    }
  ]
}
```

### <a name="sample-input"></a>Örnek Giriş
Bu Shaper yetenek için kullanılabilir girişi sağlayan bir JSON belgesi olabilir:

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
Shaper beceri adlı yeni bir öğe oluşturur *analyzedText* birleşik öğeleri *metin* ve *yaklaşım*. 

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
            "output": "titlesAndChapters",
            "targetName": "analyzedText"
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

