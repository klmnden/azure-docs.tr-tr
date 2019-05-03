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
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 5267f81c9886e2d1d8d62c134156aedb3b2b8763
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65023705"
---
#   <a name="shaper-cognitive-skill"></a>Shaper bilişsel beceri

**Shaper** beceri birleştirir içine birkaç girişten bir [karmaşık tür](search-howto-complex-data-types.md) daha sonra zenginleştirme işlem hattında başvurulabilir. **Shaper** beceri temelde bir yapı oluşturmak, bu yapının üyelerine adını tanımlayın ve her üye için değerler atayın olanak tanır. Birleştirilmiş alanlar arama senaryolarda yararlı bir tek yapısı, şehir ve tek yapısı veya adı durumuna ve doğum tarihi benzersiz kimliğini oluşturmak için tek bir yapıda adı ve Soyadı birleştirme verilebilir.

API sürümü, şekillendirme derinliği elde edebilirsiniz belirler. 

| API sürümü | Davranışlar şekillendirme | 
|-------------|-------------------|
| REST API 2019-05-06-önizleme sürümünü (.NET SDK'sı desteklenmiyor) | Karmaşık nesneler, birden çok düzey derinlikte, birinde **Shaper** beceri tanımı. |
| 2019-05-06 ** (genel kullanıma sunuldu) 2017-11-11-Önizleme| Karmaşık nesneler, derin bir düzey. Çok düzeyli bir şekil shaper adımları birbirine zincirleme gerektirir.|

Önizleme **Shaper** yetenek, gösterilen [Senaryo 3](#nested-complex-types), yeni bir ekler isteğe bağlı *sourceContext* giriş özelliği. *Kaynak* ve *sourceContext* özelliklerini karşılıklı olarak birbirini dışlar. Yetenek bağlamında Giriş ise kullanmanız yeterlidir *kaynak*. Giriş sırasında ise bir *farklı* bağlamını anlamaktan kullanın beceri bağlamı *sourceContext*. *SourceContext* kaynağı olarak ele alıyor belirli bir öğe ile iç içe geçmiş girişi tanımlamanızı gerektirir. 

Yanıtta, tüm API sürümleri için çıkış adı her zaman "çıkış". Dahili olarak, işlem hattı "analyzedText" gibi farklı bir ad aşağıdaki örnekte gösterildiği gibi eşleyebilirsiniz ancak **Shaper** yetenek kendi yanıtta "çıkış" döndürür. Özel bir yetenek oluşturun ve yanıt kendiniz yapılandırılması bu zenginleştirilmiş belgeleri hata ayıklaması yapıyorsanız ve adlandırma tutarsızlık dikkat edin veya önemli olabilir.

> [!NOTE]
> **Shaper** bir Bilişsel hizmetler API'sine beceri bağlı değil ve kullanmak için ücretlendirilmez. Hala [Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md), ancak geçersiz kılmak için **ücretsiz** , az sayıda gün başına günlük zenginleştirmelerinin sınırlar resource seçeneği.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ShaperSkill

## <a name="scenario-1-complex-types"></a>Senaryo 1: karmaşık türler

Adlı bir yapı oluşturmak istediğiniz bir senaryo düşünün *analyzedText* iki üyesi olan: *metin* ve *yaklaşım*sırasıyla. Azure Search dizini çok parçalı aranabilir bir alanı olarak adlandırılan bir *karmaşık tür* ve genellikle eşleyen kendisine karşılık gelen bir karmaşık yapısı kaynak verilere sahip olduğunda oluşturulur.

Ancak, aracılığıyla karmaşık türler oluşturmak için başka bir yaklaşım, **Shaper** yeteneği. Bu yetenek bir beceri kümesi dahil ederek, bellek içi işlemleri beceri kümesi işleme sırasında veri şekilleri dizininizdeki bir karmaşık türü eşlenebilir iç içe geçmiş yapılar ile çıkış sağlayabilir. 

Aşağıdaki örnek beceri tanımı üyesi adları giriş olarak sağlar. 


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

### <a name="sample-index"></a>Örnek dizini

Bir beceri kümesi bir dizin oluşturucu tarafından çağrılır ve bir dizin oluşturucu bir dizin gerektirir. Dizininizi karmaşık alan gösteriminde aşağıdaki örnekteki gibi görünebilir. 

```json

    "name": "my-index",
    "fields": [
        {   "name": "myId", "type": "Edm.String", "key": true, "filterable": true   },
        {   "name": "analyzedText", "type": "Edm.ComplexType",
            "fields": [{
                    "name": "text",
                    "type": "Edm.String",
                    "filterable": false,
                    "sortable": false,
                    "facetable": false,
                    "searchable": true  },
          {
                    "name": "sentiment",
                    "type": "Edm.Double",
                    "searchable": true,
                    "filterable": true,
                    "sortable": true,
                    "facetable": true
                },
```

### <a name="skill-input"></a>Beceri giriş

Bunun için kullanılabilir giriş sağlama gelen bir JSON belgesi **Shaper** beceri olabilir:

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


### <a name="skill-output"></a>Beceri çıkış

**Shaper** beceri adlı yeni bir öğe oluşturur *analyzedText* birleşik öğeleri *metin* ve *yaklaşım*. Bu çıktı dizini şemaya uygun. İçeri aktarılan ve Azure Search dizini dizinlenmiş.

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

## <a name="scenario-2-input-consolidation"></a>Senaryo 2: Giriş birleştirme

Başka bir örnekte, ardışık düzen işleme farklı aşamalarında, bir kitap başlığı ve bölüm başlıkları kitabın farklı sayfalarında ayıkladığınız olduğunu varsayın. Artık bu çeşitli girişleri oluşan tek bir yapı oluşturabilir.

**Shaper** beceri tanımı bu senaryo için aşağıdaki örnekteki gibi görünebilir:

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
            "source": "/document/content/pages/*/chapterTitles/*/title"
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

### <a name="skill-output"></a>Beceri çıkış
Bu durumda, **Shaper** tek bir dizi oluşturmak için tüm bölüm başlıkları düzleştirir. 

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

<a name="nested-complex-types"></a>

## <a name="scenario-3-input-consolidation-from-nested-contexts"></a>Senaryo 3: iç içe geçmiş bağlamları gelen giriş birleştirme

> [!NOTE]
> İç içe yapılar desteklenen api-version = 2019-05-06-Önizleme kullanılabilir bir [bilgi deposu](knowledge-store-concept-intro.md) veya bir Azure Search dizini.

Sütun başlık, bölümler ve bir kitap içeriğini, varlık tanıma ve anahtar tümcecikleri içeriklerine çalıştırdıktan ve şimdi toplama sonuçlarına bölüm adı, varlıklar ve anahtar ifadeleri tek bir şekil içine farklı becerileri gerekir düşünün.

**Shaper** beceri tanımı bu senaryo için aşağıdaki örnekteki gibi görünebilir:

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
            "sourceContext": "/document/content/pages/*/chapterTitles/*",
            "inputs": [
              {
                  "name": "title",
                  "source": "/document/content/pages/*/chapterTitles/*/title"
              },
              {
                  "name": "number",
                  "source": "/document/content/pages/*/chapterTitles/*/number"
              }
            ]
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

### <a name="skill-output"></a>Beceri çıkış
Bu durumda, **Shaper** bir karmaşık türü oluşturur. Bu yapı, bellek içi bulunmaktadır. Bir Bilgi Bankası deposuna kaydetmek istiyorsanız, bir projeksiyon depolama özelliklerini tanımlar, beceri kümesi oluşturmanız gerekir.

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "titlesAndChapters": {
                    "title": "How to be happy",
                    "chapterTitles": [
                      { "title": "Start young", "number": 1},
                      { "title": "Laugh often", "number": 2},
                      { "title": "Eat, sleep and exercise", "number: 3}
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
+ [Karmaşık türler kullanma](search-howto-complex-data-types.md)
+ [Bilgi Bankası deposuna genel bakış](knowledge-store-concept-intro.md)
+ [Nasıl bilgi Store ile çalışmaya başlama](knowledge-store-howto.md)