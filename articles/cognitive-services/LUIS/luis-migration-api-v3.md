---
title: V2'ye V3 API geçişi
titleSuffix: Azure Cognitive Services
description: API sürüm 3 uç nokta değişti. Sürüm 3 uç nokta API'leri geçirme anlamak için bu kılavuzu kullanın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 6412f0a2e295a19f741c70e7870a4d198ee03b71
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233556"
---
# <a name="preview-migrate-to-api-version-3x--for-luis-apps"></a>Önizleme: API sürümüne geçirme 3.x LUIS uygulamalar için

Sorgu tahmin uç nokta API'leri değişti. Sürüm 3 uç nokta API'leri geçirme anlamak için bu kılavuzu kullanın. 

Bu V3 API önemli JSON istek ve/veya yanıt değişiklikler içeren aşağıdaki yeni özellikleri sağlar: 

* [Dış varlıklar](#external-entities-passed-in-at-prediction-time)
* [Dinamik listeler](#dynamic-lists-passed-in-at-prediction-time)
* [Önceden oluşturulmuş varlık JSON değişiklikleri](#prebuilt-entities-with-new-json)

<!--
* [Multi-intent detection of utterance](#detect-multiple-intents-within-single-utterance)
-->

Sorgu tahmin uç nokta [isteği](#request-changes) ve [yanıt](#response-changes) aşağıdakiler dahil olmak üzere, yukarıda listelenen yeni özellikler desteklemek için önemli değişiklikler vardır:

* [Yanıt nesnesi değişiklikleri](#top-level-json-changes)
* [Varlık adı yerine varlık rol adı başvuruları](#entity-role-name-instead-of-entity-name)
* [Konuşma varlıklarda işaretlemek için özellikleri](#marking-placement-of-entities-in-utterances)

Aşağıdaki LUIS özellikleri **desteklenmiyor** V3 API:

* Bing yazım denetimi V7

[Başvuru belgeleri](https://aka.ms/luis-api-v3) V3 için kullanılabilir.

## <a name="prebuilt-entities-with-new-json"></a>Önceden oluşturulmuş varlıklarla yeni JSON

V3 yanıt nesnesi değişiklikleri içeren [önceden oluşturulmuş varlıklarla](luis-reference-prebuilt-entities.md). 

## <a name="request-changes"></a>Değişiklikleri isteme 

### <a name="query-string-parameters"></a>Sorgu dizesi parametreleri

V3 API farklı bir sorgu dizesi parametresi içeriyor.

|Parametre adı|Tür|Version|Amaç|
|--|--|--|--|
|`query`|string|Yalnızca v3|**V2'de**, tahmin için utterance bulunduğu `q` parametresi. <br><br>**V3 içinde**, işlevselliği geçirilen `query` parametresi.|
|`show-all-intents`|boole|Yalnızca v3|Tüm hedefleri içinde karşılık gelen puanı döndürür **prediction.intents** nesne. Hedefleri, bir üst nesneleri olarak döndürülür `intents` nesne. Bu amaç bir dizide bulunacak gerek kalmadan programlı erişim sağlar: `prediction.intents.give`. V2'de, bunları bir dizide döndürülmedi. |
|`verbose`|boole|V2 VE V3|**V2'de**, true, tahmin edilen tüm hedefleri kümesine döndürüldü. Tahmin edilen tüm hedefleri gerekiyorsa, V3 param kullanın `show-all-intents`.<br><br>**V3 içinde**, bu parametre varlık meta verileri varlık öngörü ayrıntılarını yalnızca sağlar.  |

<!--
|`multiple-segments`|boolean|V3 only|Break utterance into segments and predict each segment for intents and entities.|
-->


### <a name="the-query-prediction-json-body-for-the-post-request"></a>Sorgu tahmin JSON gövdesi için `POST` isteği

```JSON
{
    "query":"your utterance here",
    "options":{
        "timezoneOffset": "-8:00"
    },
    "externalEntities":[],
    "dynamicLists":[]
}
```

## <a name="response-changes"></a>Yanıt değişiklikleri

Sorgu yanıtına JSON en sık kullanılan veriler için daha fazla programlı erişim izin verecek şekilde değiştirildi. 

### <a name="top-level-json-changes"></a>Üst düzey JSON değişiklikleri

V2 için üst JSON özellikleri, `verbose` tüm amaçlar ve bunların puanları döndüren true olarak ayarlanmış `intents` özelliği:

```JSON
{
    "query":"this is your utterance you want predicted",
    "topScoringIntent":{},
    "intents":[],
    "entities":[],
    "compositeEntities":[]
}
```

V3 üst JSON özellikleri şunlardır:

```JSON
{
    "query": "this is your utterance you want predicted",
    "prediction":{
        "normalizedQuery": "this is your utterance you want predicted - after normalization",
        "topIntent": "intent-name-1",
        "intents": {}, 
        "entities":{}
    }
}
```

`intents` Sırasız bir listesini nesnedir. İlk alt varsaymayın `intents` karşılık gelen `topIntent`. Bunun yerine, `topIntent` puanı bulunacak değer:

```nodejs
const topIntentName = response.prediction.topIntent;
const score = intents[topIntentName];
```

Yanıt JSON şema değişiklikleri için izin ver:

* Özgün utterance arasında ayrım Temizle `query`ve tahmin, döndürülen `prediction`.
* Programlı erişim tahmin edilen verilere daha kolay. V2'de bir dizi aracılığıyla numaralandırma yerine değerlerle erişebilirsiniz **adlı** ıntents hem varlıklar için. Uygulamanın tamamında arasında benzersiz olduğundan, tahmin edilen varlık için rolleri, rol adı döndürülür.
* Veri türleri belirlerse dikkate alınır. Sayısal dizeler olarak artık döndürülür.
* Birinci öncelik tahmin bilgileri ve ek meta veriler arasında ayrım döndürülen içinde `$instance` nesne. 

### <a name="access-instance-for-entity-metadata"></a>Erişim `$instance` için varlık meta verileri

Varlık meta verilerini gerekiyorsa, sorgu dizesi kullanmak gereken `verbose=true` bayrağı ve yanıt içeren meta verilerde `$instance` nesne. Aşağıdaki bölümlerde JSON yanıtlarındaki örnekler gösterilmektedir.

### <a name="each-predicted-entity-is-represented-as-an-array"></a>Tahmin edilen her varlık, bir dizi olarak temsil edilir

`prediction.entities.<entity-name>` Her varlığın birden çok kez utterance tahmin edilebilmesi için bir dizi nesne içerir. 

### <a name="list-entity-prediction-changes"></a>Varlık tahmin Listele

Bir liste varlık tahmin için JSON oluşan bir dizi olarak değişmiştir:

```JSON
"entities":{
    "my_list_entity":[
        ["canonical-form-1","canonical-form-2"],
        ["canonical-form-2"]
    ]
}
```
Her iç dizi utterance içindeki metni karşılık gelir. Aynı metni bir liste varlığı birden fazla alt liste içinde görünür olduğundan iç nesne bir dizidir. 

Arasında eşlerken `entities` nesnesini `$instance` nesnesi, nesnelerin sırasını listesi varlık tahminler elde etmek için korunur.

```nodejs
const item = 0; // order preserved, use same enumeration for both
const predictedCanonicalForm = entities.my_list_entity[item];
const associatedMetadata = entities.$instance.my_list_entity[item];
```

### <a name="entity-role-name-instead-of-entity-name"></a>Varlık adı yerine varlık rol adı 

V2'deki `entities` dizi olan benzersiz tanımlayıcı varlık adı ile tahmin edilen tüm varlıkları döndürdü. V3 sürümünde varlık rolleri kullanıyorsa ve tahmin varlık rol için birincil rol adı tanımlayıcısıdır. Varlık rol adları diğer model (amaç, varlığı) adları da dahil olmak üzere tüm uygulama arasında benzersiz olması gerektiğinden, bu mümkündür.

Aşağıdaki örnekte: metin içeren bir utterance göz önünde bulundurun `Yellow Bird Lane`. Bu metin, özel bir tahmin `Location` varlığın rolü `Destination`.

|Utterance metin|Varlık adı|Rol adı|
|--|--|--|
|`Yellow Bird Lane`|`Location`|`Destination`|

V2'de varlık tarafından tanımlanan _varlık adı_ nesnenin bir özellik olarak rolüyle:

```JSON
"entities":[
    {
        "entity": "Yellow Bird Lane",
        "type": "Location",
        "startIndex": 13,
        "endIndex": 20,
        "score": 0.786378264,
        "role": "Destination"
    }
]
```

V3 sürümünde varlık tarafından başvurulan _varlık rolü_, tahmin rol için ise:

```JSON
"entities":{
    "Destination":[
        "Yellow Bird Lane"
    ]
}
```

V3 sürümünde, aynı sonucu ile `verbose` varlık meta verileri döndürmek için bayrağı:

```JSON
"entities":{
    "Destination":[
        "Yellow Bird Lane"
    ],
    "$instance":{
        "Destination": [
            {
                "role": "Destination",
                "type": "Location",
                "text": "Yellow Bird Lane",
                "startIndex": 25,
                "length":16,
                "score": 0.9837309,
                "modelTypeId": 1,
                "modelType": "Entity Extractor"
            }
        ]
    }
}
```

## <a name="external-entities-passed-in-at-prediction-time"></a>Tahmin zaman geçirilen dış varlıklar

Dış varlıklar LUIS uygulamanızı tanımlayın ve var olan varlıkları için özellikler olarak kullanılan çalışma zamanı sırasında varlıklar olanağı sağlayacak. Bu sorguları tahmin uç noktanıza göndermeden önce kendi ayrı ve özel varlık ayıklayıcıları kullanmanıza olanak sağlar. Bu sorgu tahmin uç noktada yapıldığından, yeniden eğitme ve modelinizi yayımlama gerek yoktur.

İstemci uygulaması kendi varlık ayıklayıcı varlık eşleşen eşleşen söz konusu varlık içinde utterance konumunu belirleme ve sonra bu bilgileri kullanarak istek göndererek yöneterek sunuyor. 

Dış varlıklar hala rolleri, bileşik ve diğerleri gibi diğer modelleriyle sinyalleri olarak kullanılan sırasında herhangi bir varlık türü genişletmek için mekanizmasıdır.

Bu, yalnızca sorgu tahmin çalışma zamanında kullanılabilir verileri olan bir varlık için kullanışlıdır. Bu tür verilerin örnekleri veri ya da kullanıcı başına özel sürekli olarak değişir. Bir LUIS kişi varlığı bir kullanıcının kişi listesi dış bilgileriyle genişletebilirsiniz. 

### <a name="entity-already-exists-in-app"></a>Varlık uygulamada zaten mevcut.

Değerini `entityName` son nokta isteğinin POST gövdesini geçirilen Dış varlık zaten isteği yapıldığında zaman eğitilen ve yayımlanan uygulamada bulunmalıdır. Varlık türü olması fark etmez, tüm türleri desteklenir.

### <a name="first-turn-in-conversation"></a>Konuşmadaki ilk Aç

Burada aşağıdaki eksik bilgileri kullanıcının girdiği bir sohbet bot konuşmadaki ilk bir utterance göz önünde bulundurun:

`Send Hazem a new message`

LUIS için Sohbet Robotu istekten hakkında bilgi POST gövdesinde geçirebilirsiniz `Hazem` için doğrudan kullanıcının kişilerini biri olarak eşleştirilir.

```json
    "externalEntities": [
        {
            "entityName":"contacts",
            "startIndex": 5,
            "entityLength": 5,
            "resolution": {
                "employeeID": "05013",
                "preferredContactType": "TeamsChat"
            }
        }
    ]
```

İstekte tanımlandığından tahmini yanıt tüm diğer varlıklarla tahmin edilen, bu dış varlık içeriyor.  

### <a name="second-turn-in-conversation"></a>İkinci konuşmada Aç

Sohbet Robotu içine ileri kullanıcı utterance daha belirsiz bir terim kullanır:

`Send him a calendar reminder for the party.`

Önceki utterance içinde utterance kullanan `him` başvuru olarak `Hazem`. Konuşma sohbet Robotu, POST gövdesinde eşleyebilirsiniz `him` ilk utterance ayıklanan varlık değerine `Hazem`.

```json
    "externalEntities": [
        {
            "entityName":"contacts",
            "startIndex": 5,
            "entityLength": 3,
            "resolution": {
                "employeeID": "05013",
                "preferredContactType": "TeamsChat"
            }
        }
    ]
```

İstekte tanımlandığından tahmini yanıt tüm diğer varlıklarla tahmin edilen, bu dış varlık içeriyor.  

#### <a name="resolution"></a>Çözüm

_İsteğe bağlı_ `resolution` özelliği döndürür dış bir varlıkla ilişkilendirilen meta verilerin geçirin ve ardından alacak olanak tanıyan tahmin yanıt geriye yanıtta. 

Önceden oluşturulmuş varlıklarla genişletmek için birincil amaçtır ancak bu varlık türüne sınırlı değildir. 

`resolution` Özelliği, bir sayı, dize, nesne veya dizi olabilir:

* "Dallas"
* {"text": "value"}
* 12345 
* ["a", "b", "c"]


## <a name="dynamic-lists-passed-in-at-prediction-time"></a>Tahmin zaman geçirilen dinamik listeler

Dinamik listeleri LUIS uygulaması içinde zaten mevcut bir eğitilen ve yayımlanan listesi varlık genişletmenizi sağlar. 

Liste varlık değerlerinizi düzenli aralıklarla değiştirmeniz gerektiğinde bu özelliği kullanın. Bu özellik önceden eğitilmiş ve yayımlanan listesi varlık genişletmenizi sağlar:

* Tahmin son nokta isteğinin sorgu zamanında.
* Tek bir istek için.

Liste varlığı LUIS uygulamada boş olabilir ancak var olması. LUIS uygulaması liste varlıkta değiştirilmez, ancak tahmin özelliği uç noktasında en fazla 2 listeleriyle yaklaşık 1000 öğe eklemek için genişletilir.

### <a name="dynamic-list-json-request-body"></a>Dinamik liste JSON isteği gövdesi

Listeye yeni bir alt liste ile eş anlamlılar eklemek için aşağıdaki JSON gövdesinde göndermek ve liste varlığı metin tahmin `LUIS`, ile `POST` sorgu tahmin isteği:

```JSON
{
    "query": "Send Hazem a message to add an item to the meeting agenda about LUIS.",
    "options":{
        "timezoneOffset": "-8:00"
    },
    "dynamicLists": [
        {
            "listEntityName":"ProductList",
            "requestLists":[
                {
                    "name": "Azure Cognitive Services",
                    "canonicalForm": "Azure-Cognitive-Services",
                    "synonyms":[
                        "language understanding",
                        "luis",
                        "qna maker"
                    ]
                }
            ]
        }
    ]
}
```

İstekte tanımlandığından tahmini yanıt tüm diğer varlıklarla tahmin edilen, bu liste varlık içeriyor. 

## <a name="timezoneoffset-renamed-to-datetimereference"></a>TimezoneOffset datetimeReference için yeniden adlandırıldı

**V2'de**, `timezoneOffset` [parametre](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) isteği bir GET veya POST isteği gönderirse bir sorgu dizesi parametresi olarak tahmin isteği bakılmaksızın gönderilir. 

**V3 içinde**, POST gövde parametresi ile aynı işlevselliği sağlanan `datetimeReference`. 

## <a name="marking-placement-of-entities-in-utterances"></a>Konuşma varlık yerleştirme işaretleme

**V2'de**, bir varlık içinde bir utterance ile işaretlenmiş `startIndex` ve `endIndex`. 

**V3 içinde**, varlık ile işaretlenmiş `startIndex` ve `entityLength`.


## <a name="next-steps"></a>Sonraki adımlar

Var olan geri KALAN güncelleştirilecek V3 API belgeleri çağırır LUIS için kullanım [uç nokta](https://aka.ms/luis-api-v3) API'leri. 