---
title: Veri ayıklama
titleSuffix: Language Understanding - Azure Cognitive Services
description: Hedefleri ve varlıklar ile utterance metin verileri ayıklayın. Language Understanding (LUIS) ne tür veriler ayıklanabileceği öğrenin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 35f1521884de3a4a0971b6e1c00f92a9094a8550
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60812821"
---
# <a name="extract-data-from-utterance-text-with-intents-and-entities"></a>Utterance metinle amaç ve varlıkları veri ayıklamak
LUIS, bir kullanıcının doğal dil konuşma bilgi almak için sağlar. Bilgiler bir program, uygulama veya sohbet Robotu eyleme kullanılabilmesi için bir şekilde ayıklanır. Aşağıdaki bölümlerde, hangi verilerin hedefleri ve JSON örneklerini varlıklarla döndürülür öğrenin.

Tam metin eşleşmesi olmadığından uygulamalarınızdaki verileri ayıklamak için makine öğrenilen verilerdir. Makine öğrendiniz, veri ayıklama [varlıkları](luis-concept-entity-types.md) parçası olması gerekiyor [döngüsü yazma](luis-concept-app-iteration.md) beklediğiniz verileri aldığınız başarılara kadar.

## <a name="data-location-and-key-usage"></a>Veri konum ve anahtar kullanımı
LUIS, yayımlanan verilerden sağlar [uç nokta](luis-glossary.md#endpoint). **HTTPS isteğini** hazırlık veya üretim ortamları gibi bazı isteğe bağlı yapılandırmalar yanı sıra utterance (POST veya GET) içerir.

`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&q=book 2 tickets to paris`

`appID` Kullanılabilir **ayarları** sayfası URL'SİNİN bir parçası yanı sıra, LUIS uygulaması (sonra `/apps/`) ne zaman düzenlediğiniz bu LUIS uygulaması. `subscription-key` Uygulamanızı sorgulamak için kullanılan uç noktası anahtarı. LUIS öğrenme aşamasında ücretsiz geliştirme/başlangıç anahtarınızı kullanabilirsiniz, ancak uç noktası anahtarı destekleyen bir anahtarı değiştirmek önemli olan, [LUIS kullanım beklenen](luis-boundaries.md#key-limits). `timezoneOffset` Dakika birimidir.

**HTTPS yanıtı** LUIS belirleyebilir hedefi ve varlık bilgilerini herhangi birinin geçerli yayımlanan model tabanlı hazırlık veya üretim uç noktası içerir. Uç nokta URL'si bulundu [LUIS](luis-reference-regions.md) Web sitesi, **Yönet** üzerinde bölümünde **anahtarları ve uç noktaları** sayfası.

## <a name="data-from-intents"></a>Intents verileri
Üst birincil verilerdir Puanlama **hedefi adı**. Kullanarak `MyStore` [hızlı](luis-quickstart-intents-only.md), uç nokta yanıt:

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "entities": []
}
```

|Veri nesnesi|Veri Türü|Veri Konumu|Değer|
|--|--|--|--|
|Amaç|Dize|topScoringIntent.intent|"GetStoreInfo"|

Sohbet botu veya arama LUIS uygulama birden fazla hedefi puanına göre karar verir, sorgu dizesi parametresini ayarlayarak ıntents puanlarını döndürür `verbose=true`. Uç nokta yanıt şöyledir:

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "intents": [
    {
      "intent": "GetStoreInfo",
      "score": 0.984749258
    },
    {
      "intent": "None",
      "score": 0.2040639
    }
  ],
  "entities": []
}
```

Intents en yüksek öncelikten en düşük puan için sıralanır.

|Veri nesnesi|Veri Türü|Veri Konumu|Değer|Puan|
|--|--|--|--|:--|
|Amaç|Dize|ıntents [0] .intent|"GetStoreInfo"|0.984749258|
|Amaç|Dize|ıntents [1] .intent|"None"|0.0168218873|

Önceden oluşturulmuş etki alanları eklerseniz, hedefi adı etki alanı gibi gösterir `Utilties` veya `Communication` amaç yanı sıra:

```JSON
{
  "query": "Turn on the lights next monday at 9am",
  "topScoringIntent": {
    "intent": "Utilities.ShowNext",
    "score": 0.07842206
  },
  "intents": [
    {
      "intent": "Utilities.ShowNext",
      "score": 0.07842206
    },
    {
      "intent": "Communication.StartOver",
      "score": 0.0239675418
    },
    {
      "intent": "None",
      "score": 0.0168218873
    }],
  "entities": []
}
```

|Domain|Veri nesnesi|Veri Türü|Veri Konumu|Değer|
|--|--|--|--|--|
|Altyapı Hizmetleri|Amaç|Dize|ıntents [0] .intent|"<b>Yardımcı programları</b>. ShowNext"|
|İletişim|Amaç|Dize|ıntents [1] .intent|<b>İletişim</b>. StartOver"|
||Amaç|Dize|[2] hedefleri .intent|"None"|


## <a name="data-from-entities"></a>Veri varlıkları
Çoğu sohbet robotları ve uygulamaların birden çok hedefi adı gerekir. Bu ek, isteğe bağlı veri varlıkları utterance içinde bulunan gelir. Her varlık türü eşleşme ile ilgili farklı bilgileri döndürür.

Bir tek sözcük veya tümcecik bir utterance içinde birden fazla varlık eşleşebilir. Bu durumda, eşleşen her varlık kendi puanıyla döndürülür.

Tüm varlıklar döndürülür **varlıkları** uç noktasından yanıt bir dizi:

```JSON
"entities": [
  {
    "entity": "bob jones",
    "type": "Name",
    "startIndex": 0,
    "endIndex": 8,
    "score": 0.473899543
  },
  {
    "entity": "3",
    "type": "builtin.number",
    "startIndex": 16,
    "endIndex": 16,
    "resolution": {
      "value": "3"
    }
  }
]
```

## <a name="tokenized-entity-returned"></a>parçalanmış varlık döndürdü
Birkaç [kültürler](luis-language-support.md#tokenization) ile varlık nesnesi döndürür `entity` değer [simgeleştirilmiş](luis-glossary.md#token). LUIS ile varlık nesnesi içinde döndürülen endIndex ve startIndex yeni, parçalanmış değerine sırada, ham varlık program aracılığıyla ayıklamak özgün sorguya ancak bunun yerine eşlemeyin. 

Örneğin, Almanca, sözcük içinde `das Bauernbrot` içine simgeleştirilmiş `das bauern brot`. Parçalanmış değeri `das bauern brot`, döndürülür ve özgün değeri, program aracılığıyla startIndex ve özgün sorgunun, ayırabilir endIndex belirlenebilir `das Bauernbrot`.

## <a name="simple-entity-data"></a>Basit bir varlığın verilerinin

A [varlığın](luis-concept-entity-types.md) bir makine öğrenilen değerdir. Bir sözcük veya tümcecik olabilir.

`Bob Jones wants 3 meatball pho`

Önceki utterance içinde `Bob Jones` basit etiketlenmiş `Customer` varlık.

Uç noktadan döndürülen veriler, varlık adı, utterance bulunan metni, bulunan metin ve puan konumunu içerir:

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

|Veri nesnesi|Varlık adı|Değer|
|--|--|--|
|Varlığın|`Customer`|`bob jones`|

## <a name="hierarchical-entity-data"></a>Hiyerarşik bir varlığın verilerinin

**Hiyerarşik varlıkları sonunda kullanımdan kaldırılacaktır. Kullanım [varlık rolleri](luis-concept-roles.md) hiyerarşik varlıkları yerine varlık subtypes belirlemek için.**

[Hiyerarşik](luis-concept-entity-types.md) varlıkları makine hakkında bilgi edindiniz ve bir sözcük veya tümcecik ekleyebilirsiniz. Alt öğeleri bağlam tarafından tanımlanır. İle tam metin eşleşmesi için bir üst-alt ilişkisi arıyorsanız, kullanan bir [listesi](#list-entity-data) varlık.

`book 2 tickets to paris`

Önceki utterance içinde `paris` etiketli bir `Location::ToLocation` alt `Location` hiyerarşik varlık.

Uç noktadan döndürülen veriler, varlık adı ve alt adı utterance bulunan metni, bulunan metin ve puan konumunu içerir:

```JSON
"entities": [
  {
    "entity": "paris",
    "type": "Location::ToLocation",
    "startIndex": 18,
    "endIndex": 22,
    "score": 0.6866132
  }
]
```

|Veri nesnesi|Üst öğe|Alt|Değer|
|--|--|--|--|
|Hiyerarşik varlık|Konum|ToLocation|"paris"|

## <a name="composite-entity-data"></a>Bileşik bir varlığın verilerinin
[Bileşik](luis-concept-entity-types.md) varlıkları makine hakkında bilgi edindiniz ve bir sözcük veya tümcecik ekleyebilirsiniz. Örneğin, önceden oluşturulmuş bir bileşik varlığın düşünün `number` ve `Location::ToLocation` aşağıdaki utterance ile:

`book 2 tickets to paris`

Dikkat `2`, sayı ve `paris`, ToLocation varlıkları parçası olmayan aralarında sözcükler vardır. Etiketli bir utterance içinde kullanılan yeşil bir çizgi [LUIS](luis-reference-regions.md) Web sitesi, bileşik bir varlık gösterir.

![Bileşik varlık](./media/luis-concept-data-extraction/composite-entity.png)

Bileşik varlıkları döndürülür bir `compositeEntities` dizisi ve bileşik tüm varlıklarda da döndürülür içinde `entities` dizisi:

```JSON
  "entities": [
    {
      "entity": "paris",
      "type": "Location::ToLocation",
      "startIndex": 18,
      "endIndex": 22,
      "score": 0.956998169
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 5,
      "endIndex": 5,
      "resolution": {
        "value": "2"
      }
    },
    {
      "entity": "2 tickets to paris",
      "type": "Order",
      "startIndex": 5,
      "endIndex": 22,
      "score": 0.7714499
    }
  ],
  "compositeEntities": [
    {
      "parentType": "Order",
      "value": "2 tickets to paris",
      "children": [
        {
          "type": "builtin.number",
          "value": "2"
        },
        {
          "type": "Location::ToLocation",
          "value": "paris"
        }
      ]
    }
  ]
```    

|Veri nesnesi|Varlık adı|Değer|
|--|--|--|
|Önceden oluşturulmuş varlık - sayı|"builtin.number"|"2"|
|Hiyerarşik varlık - konum|"Location::ToLocation"|"paris"|

## <a name="list-entity-data"></a>Varlık verilerini listesi

A [listesi](luis-concept-entity-types.md) varlık olmayan makine öğrendiniz. Tam metin bir eşleşmedir. Liste öğeleri için eş anlamlı sözcükler birlikte listedeki öğeleri temsil eder. LUIS, herhangi bir listede bir öğe için herhangi bir eşleşme yanıtta bir varlık olarak işaretler. Birden fazla listesinde bir eş anlamlı olabilir.

Adlı bir listesi uygulamanın olduğu varsayalım `Cities`, şehir adları havaalanı (tac Sea), havaalanı kodu (SEA), posta kodu (98101) ve telefon alan kodunu (206) bulunduğu şehir de dahil olmak üzere çeşitleri için izin verme.

|Liste öğesi|Öğe eş anlamlı sözcükler|
|---|---|
|Seattle|Deniz tac, Deniz, 98101, 206 + 1 |
|Paris|cdg, geçmiş, roissy 75001, 1, +33|

`book 2 tickets to paris`

Önceki utterance, sözcük içinde `paris` paris öğesine bir parçası olarak eşleştirilir `Cities` varlık listesi. Liste varlığı hem normalleştirilmiş öğenin adı, hem de öğe eş anlamlılar eşleşir.

```JSON
"entities": [
  {
    "entity": "paris",
    "type": "Cities",
    "startIndex": 18,
    "endIndex": 22,
    "resolution": {
      "values": [
        "Paris"
      ]
    }
  }
]
```

Paris için bir eşanlamlı kullanarak başka bir örnek utterance:

`book 2 tickets to roissy`

```JSON
"entities": [
  {
    "entity": "roissy",
    "type": "Cities",
    "startIndex": 18,
    "endIndex": 23,
    "resolution": {
      "values": [
        "Paris"
      ]
    }
  }
]
```

## <a name="prebuilt-entity-data"></a>Önceden oluşturulmuş bir varlığın verilerinin
[Önceden oluşturulmuş](luis-concept-entity-types.md) varlıkları kullanarak açık kaynaklı bir normal ifade eşleştirmesi üzerinde temel saptandığında [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text) proje. Önceden oluşturulmuş varlıklarla varlık dizide döndürülen ve tür adı ön ekine sahip kullanın `builtin::`. Aşağıda bir örnek utterance döndürülen önceden oluşturulmuş varlıklarla gösterilmiştir:

`Dec 5th send to +1 360-555-1212`

```JSON
"entities": [
    {
      "entity": "dec 5th",
      "type": "builtin.datetimeV2.date",
      "startIndex": 0,
      "endIndex": 6,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2017-12-05"
          },
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2018-12-05"
          }
        ]
      }
    },
    {
      "entity": "1",
      "type": "builtin.number",
      "startIndex": 18,
      "endIndex": 18,
      "resolution": {
        "value": "1"
      }
    },
    {
      "entity": "360",
      "type": "builtin.number",
      "startIndex": 20,
      "endIndex": 22,
      "resolution": {
        "value": "360"
      }
    },
    {
      "entity": "555",
      "type": "builtin.number",
      "startIndex": 26,
      "endIndex": 28,
      "resolution": {
        "value": "555"
      }
    },
    {
      "entity": "1212",
      "type": "builtin.number",
      "startIndex": 32,
      "endIndex": 35,
      "resolution": {
        "value": "1212"
      }
    },
    {
      "entity": "5th",
      "type": "builtin.ordinal",
      "startIndex": 4,
      "endIndex": 6,
      "resolution": {
        "value": "5"
      }
    },
    {
      "entity": "1 360 - 555 - 1212",
      "type": "builtin.phonenumber",
      "startIndex": 18,
      "endIndex": 35,
      "resolution": {
        "value": "1 360 - 555 - 1212"
      }
    }
  ]
```

## <a name="regular-expression-entity-data"></a>Normal ifade varlık verilerini
[Normal ifade](luis-concept-entity-types.md) varlıkları bir varlık oluşturduğunuzda sağladığınız ifade bir normal ifade eşleştirmesi üzerinde temel saptandığında. Kullanırken `kb[0-9]{6}` normal ifade varlık, aşağıdaki JSON yanıtı ile döndürülen bir normal ifade varlıkları sorgu için bir örnek utterance tanımıdır `When was kb123456 published?`:

```JSON
{
  "query": "when was kb123456 published?",
  "topScoringIntent": {
    "intent": "FindKBArticle",
    "score": 0.933641255
  },
  "intents": [
    {
      "intent": "FindKBArticle",
      "score": 0.933641255
    },
    {
      "intent": "None",
      "score": 0.04397359
    }
  ],
  "entities": [
    {
      "entity": "kb123456",
      "type": "KB number",
      "startIndex": 9,
      "endIndex": 16
    }
  ]
}
```

## <a name="extracting-names"></a>Ayıklanan adları
Bir ad, harf ve sözcükler neredeyse her bir birleşimi olabilir çünkü bir utterance adları alınıyor zordur. Ne tür adı, ayıklama bağlı olarak, birkaç seçeneğiniz vardır. Aşağıdaki öneriler kuralları ancak daha fazla yönergeleri değildir.

### <a name="add-prebuilt-personname-and-geographyv2-entities"></a>Önceden oluşturulmuş PersonName ve GeographyV2 varlık ekleme

[PersonName](luis-reference-prebuilt-person.md) ve [GeographyV2](luis-reference-prebuilt-geographyV2.md) varlıklar bazı durumlarda kullanılabilir [dil kültür](luis-reference-prebuilt-entities.md). 

### <a name="names-of-people"></a>Kişilerin adları

Kişi adı, dil ve kültür bağlı olarak bazı küçük biçimi olabilir. Ya da bir önceden oluşturulmuş kullanın **[personName](luis-reference-prebuilt-person.md)** varlık veya **[varlığın](luis-concept-entity-types.md#simple-entity)** ile [rolleri](luis-concept-roles.md) ilk ve Soyadı. 

Varlığın kullanırsanız, hiçbiri dahil olmak üzere tüm hedefleri arasında farklı kısımlarını farklı uzunluktaki konuşma ve konuşma utterance içinde adı ve Soyadı kullanan örnekler vermeniz hedefi sağlayın. [Gözden geçirme](luis-how-to-review-endoint-utt.md) doğru şekilde tahmin değil herhangi bir adı etiketlemek için düzenli olarak konuşma uç noktası.

### <a name="names-of-places"></a>Basamak adları

Konum adlarını ayarlayın ve şehir, ilçeler, durumları, bölgeler ve ülke gibi bilinen. Önceden oluşturulmuş varlık **[geographyV2](luis-reference-prebuilt-geographyv2.md)** konum bilgileri ayıklamak için.

### <a name="new-and-emerging-names"></a>Yeni ve geliştirilmekte olan adları

Bazı uygulamalar, ürünleri veya şirketler gibi yeni ve geliştirilmekte olan adlarını bulmak gerekir. Bu tür adları, veri ayıklama en zor türüdür. İle başlayan bir **[varlığın](luis-concept-entity-types.md#simple-entity)** ve ekleme bir [tümcecik listesi](luis-concept-feature.md). [Gözden geçirme](luis-how-to-review-endoint-utt.md) doğru şekilde tahmin değil herhangi bir adı etiketlemek için düzenli olarak konuşma uç noktası.

## <a name="pattern-roles-data"></a>Desen rolleri veri
Roller, varlık bağlamsal fark vardır.

```JSON
{
  "query": "move bob jones from seattle to redmond",
  "topScoringIntent": {
    "intent": "MoveAssetsOrPeople",
    "score": 0.9999998
  },
  "intents": [
    {
      "intent": "MoveAssetsOrPeople",
      "score": 0.9999998
    },
    {
      "intent": "None",
      "score": 1.02040713E-06
    },
    {
      "intent": "GetEmployeeBenefits",
      "score": 6.12244548E-07
    },
    {
      "intent": "GetEmployeeOrgChart",
      "score": 6.12244548E-07
    },
    {
      "intent": "FindForm",
      "score": 1.1E-09
    }
  ],
  "entities": [
    {
      "entity": "bob jones",
      "type": "Employee",
      "startIndex": 5,
      "endIndex": 13,
      "score": 0.922820568,
      "role": ""
    },
    {
      "entity": "seattle",
      "type": "Location",
      "startIndex": 20,
      "endIndex": 26,
      "score": 0.948008537,
      "role": "Origin"
    },
    {
      "entity": "redmond",
      "type": "Location",
      "startIndex": 31,
      "endIndex": 37,
      "score": 0.7047979,
      "role": "Destination"
    }
  ]
}
```

## <a name="patternany-entity-data"></a>Varlık verilerini pattern.Any
Pattern.Any varlıklardır değişken uzunluklu varlıklar şablon konuşma kullanılan bir [deseni](luis-concept-patterns.md).

```JSON
{
  "query": "where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.999999464
  },
  "intents": [
    {
      "intent": "FindForm",
      "score": 0.999999464
    },
    {
      "intent": "GetEmployeeBenefits",
      "score": 4.883697E-06
    },
    {
      "intent": "None",
      "score": 1.02040713E-06
    },
    {
      "intent": "GetEmployeeOrgChart",
      "score": 9.278342E-07
    },
    {
      "intent": "MoveAssetsOrPeople",
      "score": 9.278342E-07
    }
  ],
  "entities": [
    {
      "entity": "understand your responsibilities as a member of the community",
      "type": "FormName",
      "startIndex": 18,
      "endIndex": 78,
      "role": ""
    }
  ]
}
```


## <a name="sentiment-analysis"></a>Yaklaşım analizi
Yaklaşım analizi yapılandırılmışsa, yaklaşım analizi LUIS json yanıtı içerir. Yaklaşım analizi hakkında daha fazla bilgi [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) belgeleri.

### <a name="sentiment-data"></a>Duygu verilerini
Yaklaşım verilerdir pozitif gösteren 0 ile 1 arasındaki bir puan (1 yakın) veya (0 yakın) negatif yaklaşım veri.

Kültür olduğunda `en-us`, yanıt:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

Diğer tüm kültürler için yanıt şöyledir:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```


### <a name="key-phrase-extraction-entity-data"></a>Anahtar ifade ayıklama varlık verilerini
Anahtar ifade ayıklama varlık tarafından sağlanan utterance, anahtar ifadeleri döndürür [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).

```JSON
{
  "query": "Is there a map of places with beautiful views on a favorite trail?",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.764368951
  },
  "intents": [
    ...
  ],
  "entities": [
    {
      "entity": "beautiful views",
      "type": "builtin.keyPhrase",
      "startIndex": 30,
      "endIndex": 44
    },
    {
      "entity": "map of places",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 23
    },
    {
      "entity": "favorite trail",
      "type": "builtin.keyPhrase",
      "startIndex": 51,
      "endIndex": 64
    }
  ]
}
```

## <a name="data-matching-multiple-entities"></a>Birden çok varlık eşleşen veri

LUIS utterance içinde bulunan tüm varlıkları döndürür. Sonuç olarak, sohbet botu sonuçlarına göre karar vermeniz gerekebilir. Bir utterance birçok varlığın bir utterance sahip olabilir:

`book me 2 adult business tickets to paris tomorrow on air france`

LUIS uç nokta farklı varlıklarda aynı verileri bulabilir:

```JSON
{
  "query": "book me 2 adult business tickets to paris tomorrow on air france",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 1.0
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 1.0
    },
    {
      "intent": "Concierge",
      "score": 0.04216196
    },
    {
      "intent": "None",
      "score": 0.03610297
    }
  ],
  "entities": [
    {
      "entity": "air france",
      "type": "Airline",
      "startIndex": 54,
      "endIndex": 63,
      "score": 0.8291798
    },
    {
      "entity": "adult",
      "type": "Category",
      "startIndex": 10,
      "endIndex": 14,
      "resolution": {
        "values": [
          "adult"
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Cities",
      "startIndex": 36,
      "endIndex": 40,
      "resolution": {
        "values": [
          "Paris"
        ]
      }
    },
    {
      "entity": "tomorrow",
      "type": "builtin.datetimeV2.date",
      "startIndex": 42,
      "endIndex": 49,
      "resolution": {
        "values": [
          {
            "timex": "2018-02-21",
            "type": "date",
            "value": "2018-02-21"
          }
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Location::ToLocation",
      "startIndex": 36,
      "endIndex": 40,
      "score": 0.9730773
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 8,
      "endIndex": 8,
      "resolution": {
        "value": "2"
      }
    },
    {
      "entity": "business",
      "type": "Seat",
      "startIndex": 16,
      "endIndex": 23,
      "resolution": {
        "values": [
          "business"
        ]
      }
    },
    {
      "entity": "2 adult business",
      "type": "TicketSeatOrder",
      "startIndex": 8,
      "endIndex": 23,
      "score": 0.8784727
    }
  ],
  "compositeEntities": [
    {
      "parentType": "TicketSeatOrder",
      "value": "2 adult business",
      "children": [
        {
          "type": "Category",
          "value": "adult"
        },
        {
          "type": "builtin.number",
          "value": "2"
        },
        {
          "type": "Seat",
          "value": "business"
        }
      ]
    }
  ]
}
```

## <a name="data-matching-multiple-list-entities"></a>Birden çok listesi varlık eşleşen veri

Bir sözcük veya tümcecik birden fazla liste varlığı eşleşirse, uç nokta sorgu her liste varlığı döndürür.

Sorgu için `when is the best time to go to red rock?`, ve uygulama word `red` LUIS birden fazla listesinde, tüm varlıkları tanıyan ve JSON bitiş noktası yanıtın bir parçası varlıkları bir dizi döndürür: 

```JSON
{
  "query": "when is the best time to go to red rock?",
  "topScoringIntent": {
    "intent": "Calendar.Find",
    "score": 0.06701678
  },
  "entities": [
    {
      "entity": "red",
      "type": "Colors",
      "startIndex": 31,
      "endIndex": 33,
      "resolution": {
        "values": [
          "Red"
        ]
      }
    },
    {
      "entity": "red rock",
      "type": "Cities",
      "startIndex": 31,
      "endIndex": 38,
      "resolution": {
        "values": [
          "Destinations"
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [varlık Ekle](luis-how-to-add-entities.md) LUIS uygulamanızı varlıklar ekleme hakkında daha fazla bilgi için.
