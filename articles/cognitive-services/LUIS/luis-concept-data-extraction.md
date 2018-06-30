---
title: HALUK - Azure veri ayıklama kavramlarını anlama | Microsoft Docs
description: Ne tür veriler dil anlama (HALUK) ayıklanabilir öğrenin
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 8d8620a1c53037be6f1a33083f41964655a04921
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112125"
---
# <a name="data-extraction"></a>Veri ayıklama
HALUK, bir kullanıcının doğal dil utterances bilgi almak için olanak sağlar. Bilgi, programı, uygulama veya chatbot tarafından eyleme geçmek için kullanılabilmesi için bir biçimde ayıklanır.

Aşağıdaki bölümlerde, hangi verilerin hedefleri ve JSON örnekleri varlıklarıyla döndürülür öğrenin. Bir tam metin eşleşme olmadığından ayıklamak için en zor makine öğrenilen veri veridir. Makine öğrenilen veri ayıklanmasıyla [varlıklar](luis-concept-entity-types.md) parçası olması gerekiyor [döngüsü yazma](luis-concept-app-iteration.md) beklediğiniz verileri aldığınız emin olana kadar. 

## <a name="data-location-and-key-usage"></a>Veri konumu ve anahtar kullanımı
HALUK sağlar yayımlanan verilerden [endpoint](luis-glossary.md#endpoint). **HTTPS isteğini** (POST veya GET) utterance yanı sıra hazırlık veya üretim ortamları gibi bazı isteğe bağlı yapılandırmaları içerir. 

`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&q=book 2 tickets to paris`

`appID` Kullanılabilir **ayarları** , URL parçası yanı sıra, HALUK uygulama sayfası (sonra `/apps/`) ne zaman düzenlemekte bu HALUK uygulama. `subscription-key` Uygulamanızı sorgulamak için kullanılan endpoint anahtarıdır. HALUK öğrenme ederken boş yazma/başlangıç anahtarınızı kullanabilirsiniz, ancak destekleyen bir anahtar uç noktası anahtarı değiştirmek önemlidir, [HALUK kullanım beklenen](luis-boundaries.md#key-limits). `timezoneOffset` Dakika birimdir.

**HTTPS yanıt** tüm HALUK belirleyebilir amacını ve varlık bilgileri herhangi birinin geçerli yayımlanmış model tabanlı hazırlık veya üretim uç noktası içerir. URL bulundu uç nokta [HALUK] [ LUIS] Web sitesi **Yayımla** sayfası. 

## <a name="data-from-intents"></a>Hedefleri verileri
Üst birincil verilerdir Puanlama **hedefi adı**. Kullanarak `MyStore` [Hızlı Başlangıç](luis-quickstart-intents-only.md), uç nokta yanıt:

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
|Hedefi|Dize|topScoringIntent.intent|"GetStoreInfo"|

Sorgu dizesi parametresi ayarlanarak hedefleri puanları chatbot veya HALUK arama uygulaması üzerinde birden fazla hedefi puan dayalı bir karar yaparsa, dönüş `verbose=true`. Uç nokta yanıt şöyledir:

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

Hedefleri en yüksekten en düşük puanı sıralanır.

|Veri nesnesi|Veri Türü|Veri Konumu|Değer|Puan|
|--|--|--|--|:--|
|Hedefi|Dize|hedefleri [0] .intent|"GetStoreInfo"|0.984749258|
|Hedefi|Dize|hedefleri [1] .intent|"Hiçbiri"|0.0168218873|

Önceden oluşturulmuş etki alanları eklerseniz, hedefi adı etki alanı gibi gösterir `Utilties` veya `Communication` hedefi yanı sıra:

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
    
|Etki alanı|Veri nesnesi|Veri Türü|Veri Konumu|Değer|
|--|--|--|--|--|
|Altyapı Hizmetleri|Hedefi|Dize|hedefleri [0] .intent|"<b>Yardımcı programları</b>. ShowNext"|
|İletişim|Hedefi|Dize|hedefleri [1] .intent|<b>İletişim</b>. StartOver"|
||Hedefi|Dize|[2] hedefleri .intent|"Hiçbiri"|


## <a name="data-from-entities"></a>Varlıkları verileri
Çoğu chatbots ve uygulamaları birden çok hedefi adı gerekir. Bu ek, isteğe bağlı veriler utterance bulunan varlıklar gelir. Her varlık türü eşleşme hakkında farklı bilgi döndürür. 

Bir tek sözcük veya tümcecik bir utterance içinde birden fazla varlık eşleştirebilirsiniz. Bu durumda, her bir eşleşen varlık ile kendi puan döndürülür. 

Tüm varlıklar döndürülür **varlıklar** uç noktasından yanıt dizisi:

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

## <a name="tokenized-entity-returned"></a>Döndürülen parçalanmış varlık
Birkaç [kültürler](luis-supported-languages.md#tokenization) varlık nesnesiyle dönmek `entity` değeri [simgeleştirilmiş](luis-glossary.md#token). Varlık nesnesinde HALUK tarafından döndürülen endIndex ve startIndex yeni, parçalanmış değerine ancak bunun yerine, ham varlık program aracılığıyla ayıklamak sırayla özgün sorguya eşleyin değil. 

Örneğin, Almanca, sözcük içinde `das Bauernbrot` içine simgeleştirilmiş `das bauern brot`. Parçalanmış değeri `das bauern brot`, döndürülür ve özgün değeri program aracılığıyla startIndex ve endIndex, veren özgün sorgudaki belirlenebilir `das Bauernbrot`.

## <a name="simple-entity-data"></a>Basit bir varlığın verilerinin

A [Basit varlık](luis-concept-entity-types.md) bir makine öğrenilen değerdir. Bir sözcük veya tümcecik olabilir. 

`Bob Jones wants 3 meatball pho`

Önceki utterance içinde `Bob Jones` basit etiketli `Customer` varlık.

Uç noktadan döndürülen veriler, varlık adı, utterance bulunan metinden, bulunan metin ve puanı konumunu içerir:

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
|Basit varlık|"Müşteri"|"Can Kemal"|

## <a name="hierarchical-entity-data"></a>Hiyerarşik varlık veri

[Hiyerarşik](luis-concept-entity-types.md) varlıkları makine öğrenilen ve bir sözcük veya tümcecik içerebilir. Alt bağlam tarafından tanımlanır. Tam metin olarak eşleşen bir üst-alt ilişkisi arıyorsanız kullanmak bir [listesi](#list-entity-data) varlık. 

`book 2 tickets to paris`

Önceki utterance içinde `paris` etiketli bir `Location::ToLocation` alt `Location` hiyerarşik varlık. 

Uç noktadan döndürülen verileri, varlık adı ve alt adı, utterance bulunan metinden, bulunan metin ve puanı konumunu içerir: 

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
|--|--|--|--|--|
|Hiyerarşik varlık|Konum|ToLocation|"paris"|

## <a name="composite-entity-data"></a>Bileşik varlık veri
[Bileşik](luis-concept-entity-types.md) varlıkları makine öğrenilen ve bir sözcük veya tümcecik içerebilir. Örneğin, önceden oluşturulmuş bir bileşik varlığını göz önünde bulundurun `number` ve `Location::ToLocation` aşağıdaki utterance ile:

`book 2 tickets to paris`

Dikkat `2`, sayı ve `paris`, ToLocation sahip varlıkları parçası olmayan aralarında sözcükler. Etiketli utterance içinde kullanılan yeşil çizgi [HALUK] [ LUIS] Web sitesi, bileşik bir varlık gösterir.

![Bileşik varlık](./media/luis-concept-data-extraction/composite-entity.png)

Bileşik varlıklar döndürülür bir `compositeEntities` dizisinin ve tüm varlıkları bileşik de döndürülür içinde `entities` dizi:

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
|Önceden oluşturulmuş varlık - numarası|"builtin.number"|"2"|
|Hiyerarşik varlık - konumu|"Location::ToLocation"|"paris"|

## <a name="list-entity-data"></a>Liste varlık verilerini

A [listesi](luis-concept-entity-types.md) varlık değil makine-öğrenilen. Tam metin bir eşleşmedir. Liste öğeleri için eş anlamlı sözcükleri yanı sıra listedeki öğeleri temsil eder. HALUK herhangi bir listede bir öğeye eşleşme yanıtta bir varlık olarak işaretler. Bir eş anlamlı birden fazla listesinde olabilir. 

Uygulama adında bir listesine sahip olduğunu varsayın `Cities`, havaalanı (Sea-tac), havaalanı kodu (SEA), posta kodu (98101) ve telefon alan kodu (206) Şehir de dahil olmak üzere Şehir adları varyasyonları için izin verme. 

|Liste öğesi|Öğe anlamlıları|
|---|---|
|Seattle|Deniz tac, sea, 98101, 206 + 1 |
|Paris|cdg, roissy, geçmiş, 75001, 1, +33|

`book 2 tickets to paris`

Önceki utterance, sözcük içinde `paris` paris öğesine bir parçası olarak eşleştirilir `Cities` listesinde varlık. Liste varlık hem öğesi'nin normalleştirilmiş adı, hem de öğe eş anlamlıları eşleşir. 

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

Bir eş için Paris kullanarak başka bir örnek utterance:

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
[Önceden oluşturulmuş](luis-concept-entity-types.md) varlıklar bulunan açık kaynaklı kullanarak bir normal ifade eşleştirmesi üzerinde temel [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text) projesi. Önceden oluşturulmuş varlıkları varlıkları dizisinde döndürülür ve önekine sahip türü adı kullanmak `builtin::`. Aşağıdaki örnek utterance döndürülen önceden oluşturulmuş varlıklarla metindir:

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

## <a name="regular-expression-entity-data"></a>Normal ifade varlık verileri
[Normal ifade](luis-concept-entity-types.md) varlıklar bulunan varlık oluştururken sağladığınız bir ifade kullanarak bir normal ifade eşleştirmesi üzerinde temel. Kullanırken `kb[0-9]{6}` normal ifade varlık, aşağıdaki JSON yanıtı bir örnek utterance sorgu için döndürülen normal ifade varlıklarla tanımıdır `When was kb123456 published?`:

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

## <a name="extracting-names"></a>Adları ayıklanıyor
Ad harf ve sözcükleri neredeyse her bir kombinasyonunu olabileceğinden, bir utterance adları alınıyor zordur. Ne tür bir adın, ayıklama bağlı olarak, birkaç seçeneğiniz vardır. Bu kurallar ancak konusunda daha fazla yönerge olup olmadığı. 

### <a name="names-of-people"></a>Kişilerin adlarını
Kişilerin adını, dil ve kültür bağlı olarak bazı küçük biçimi olabilir. Hiyerarşik bir varlık ilk ve son adlarıyla alt öğesi olarak veya tek bir varlığın adı ve Soyadı rolleriyle kullanın. Hiçbiri de dahil olmak üzere tüm hedefleri arasında farklı uzunlukta utterances ve utterances utterance farklı bölümlerinde adı ve Soyadı kullanan örnekler vermek hedefi sağlayın. [Gözden geçirme](label-suggested-utterances.md) uç nokta utterances doğru şekilde tahmin olmayan herhangi bir ad etiketlemek için düzenli olarak. 

### <a name="names-of-places"></a>Basamak adları
Konum adları ayarlayın ve şehir, ilçe, durumları, İl ve ülke gibi bilinen. Uygulamanızı konumları bilinen kümesini kullanıyorsa, bir liste varlık göz önünde bulundurun. Tüm adlar yerleştirin bulmanız gerekiyorsa, basit bir varlık oluşturun ve örnekler, çeşitli sağlayın. Hangi yer adları görünümlü uygulamanızda pekiştirmek için yer adları tümcecik listesi ekleyin. [Gözden geçirme](label-suggested-utterances.md) uç nokta utterances doğru şekilde tahmin olmayan herhangi bir ad etiketlemek için düzenli olarak. 

### <a name="new-and-emerging-names"></a>Yeni ve ortaya çıkan adları
Bazı uygulamalar ürünler veya şirketler gibi yeni ve ortaya çıkan adlarını bulmak gerekir. Bu veri ayıklama en zor türüdür. Basit bir varlıkla başlamak ve tümcecik listesine ekleyin. [Gözden geçirme](label-suggested-utterances.md) uç nokta utterances doğru şekilde tahmin olmayan herhangi bir ad etiketlemek için düzenli olarak. 

## <a name="pattern-roles-data"></a>Desen rolleri veri
Varlık için kavramsal farklar rolleridir. 

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

## <a name="patternany-entity-data"></a>Pattern.Any varlık veri
Pattern.Any varlıklardır şablonu utterances içinde kullanılan değişken uzunlukta varlıkların bir [düzeni](luis-concept-patterns.md). 

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
Düşünceleri analysis yapılandırdıysanız, HALUK json yanıt düşünceleri analiz içerir. Düşünceleri çözümleme hakkında daha fazla bilgi [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) belgeleri.

### <a name="sentiment-data"></a>Düşünceleri veri
Düşünceleri verilerdir pozitif gösteren 0 ile 1 arasındaki bir puan (1 yakın) veya (0 yakın) negatif verilerin düşünceleri.

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


### <a name="key-phrase-extraction-entity-data"></a>Anahtar tümcecik ayıklama varlık veri
Anahtar tümcecik ayıklama varlık tarafından sağlanan utterance anahtar tümcecikleri döndürür [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).

<!-- TBD: verify JSON-->
```JSON
"keyPhrases": [
    "places",
    "beautiful views",
    "favorite trail"
]
```

## <a name="data-matching-multiple-entities"></a>Birden çok varlık eşleşen veri
HALUK utterance içinde bulunan tüm varlıkları döndürür. Sonuç olarak, chatbot sonuçlarına dayalı karar vermeniz gerekebilir. Bir utterance birçok varlıklar bir utterance sahip olabilir:

`book me 2 adult business tickets to paris tomorrow on air france`

HALUK endpoint farklı varlıkları aynı verileri bulabilir: 

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

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [varlıkları ekleyin](luis-how-to-add-entities.md) HALUK uygulamanıza varlıklar ekleme hakkında daha fazla bilgi için.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions