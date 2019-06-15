---
title: Extact metin eşleşen varlıkları
description: LUIS etiket çeşitleri bir sözcük veya tümcecik yardımcı olmak için bir liste varlığı eklemeyi öğrenin.
services: cognitive-services
author: diberry
titleSuffix: Azure
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 929dc7a86d141446a2070b046c6febfda4a07f0f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62117457"
---
# <a name="use-a-list-entity-to-increase-entity-detection"></a>Varlık algılama artırmak için bir liste varlığı kullanın 
Bu öğretici, kullanımını gösterir. bir [varlık listesinde](luis-concept-entity-types.md) varlık algılama artırmak için. Liste varlıkları, koşulları'nın tam bir eşleşme olarak Etiketlenecek gerekmez.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir liste varlığı oluşturma 
> * Normalleştirilmiş değerleri ve eş anlamlı sözcükler ekleme
> * Geliştirilmiş varlık kimliği doğrula

## <a name="prerequisites"></a>Önkoşullar

> [!div class="checklist"]
> * En son [Node.js](https://nodejs.org)
> * [HomeAutomation LUIS uygulaması](luis-get-started-create-app.md). Oluşturulan giriş Otomasyon uygulama yoksa yeni bir uygulama oluşturma ve önceden oluşturulmuş etki alanı ekleme **HomeAutomation**. Uygulamayı eğitin ve yayımlayın. 
> * [AuthoringKey](luis-concept-keys.md#authoring-key), [EndpointKey](luis-concept-keys.md#endpoint-key) (birden çok kez sorgulama değilse), uygulama kimliği, sürüm kimliği ve [bölge](luis-reference-regions.md) LUIS uygulaması için.

> [!Tip]
> Zaten bir aboneliğiniz yoksa, kaydolabilirsiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).

Bu öğreticideki kod tüm kullanılabilir [Azure örnekleri GitHub deposunda](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-list-entity). 

## <a name="use-homeautomation-app"></a>HomeAutomation uygulamasını kullanma
Işıklar, eğlence sistemleri ve ortam gibi denetimin Isıtma ve soğutma gibi denetimleri HomeAutomation uygulama sağlar. Bu sistemler üretici adları, takma adlar, takma adlar ve argo kullanımlar ekleyebileceğiniz birçok farklı adlara sahip. 

Farklı kültürler ve demografik bilgilere arasında birçok adları olan bir sıcaklığının sistemidir. Bir termostatınız, soğutma hem bir ev veya yapı için sistemleri ısıtma kontrol edebilirsiniz.

İdeal olarak, aşağıdaki konuşma önceden oluşturulmuş varlığa çözümlenmelidir **HomeAutomation.Device**:

|#|Utterance|Belirtilen varlık|puan|
|--|--|--|--|
|1|ac üzerinde Aç|HomeAutomation.Device - "ac"|0.8748562|
|2|Isı Aç|HomeAutomation.Device - "ısı"|0.784990132|
|3|soğuk olun|||

İlk iki konuşma farklı cihazlara eşleyin. Üçüncü utterance "soğuk olun" bir aygıtın eşlenmesi değil ancak bunun yerine bir sonuç ister. LUIS, "soğuk" terimi, istenen cihaz sıcaklığının olduğu anlamına gelir bilmez. İdeal olarak, LUIS Bu konuşma tümünün aynı cihaza çözümlenmelidir. 

## <a name="use-a-list-entity"></a>Bir liste varlığı kullanın
HomeAutomation.Device varlık küçük birkaç cihaz ya da bazı farklılıklar nedeniyle adları için idealdir. Bir ofis binasındaki veya kampüs için cihaz adları HomeAutomation.Device varlık kullanışlılığını büyütün. 

A **varlık listesinde** kümesi için bir cihaz bir yapı veya kampüs bilinen birtakım büyük bir küme olsa bile, bu senaryo için iyi bir seçim olduğundan. Bir liste varlığı kullanarak LUIS herhangi bir olası değer sıcaklığının için kümedeki alabilir ve yalnızca tek cihaz aşağı "thermostat" çözün. 

Bu öğreticide bir varlık listesi termostatın oluşturma zordur. Bu öğreticide bir thermostat için alternatif adlar şunlardır: 

|thermostat için diğer adlar|
|--|
| AC |
| Hesap|
| a-c|
|heater|
|Sık erişimli|
|hotter|
|soğuk|
|soğuk|

LUIS, genellikle yeni bir seçenek belirlemek gerekiyorsa bir [tümcecik listesi](luis-concept-feature.md#how-to-use-phrase-lists) daha iyi bir yanıt.

## <a name="create-a-list-entity"></a>Bir liste varlığı oluşturma
Bir Node.js dosyası oluşturun ve içine aşağıdaki kodu kopyalayın. AuthoringKey, AppID VersionID ve bölge değerlerini değiştirin.

   [!code-javascript[Create DevicesList List Entity](~/samples-luis/documentation-samples/tutorial-list-entity/add-entity-list.js "Create DevicesList List Entity")]

NPM bağımlılıkları yükler ve liste varlığı oluşturmak için kodu çalıştırmak için aşağıdaki komutu kullanın:

```console
npm install && node add-entity-list.js
```

Çıktı çalıştırma listesi varlık kimliği.

```console
026e92b3-4834-484f-8608-6114a83b03a6
```

## <a name="train-the-model"></a>Modeli eğitme
Sorgu sonuçlarını etkileyecek şekilde yeni liste için sırayla LUIS eğitin. Eğitim, eğitim, eğitim işlem durumu kontrol ediliyor iki parçalı işlemidir. Çok sayıda model ile bir uygulama geliştirmek için birkaç dakika sürebilir. Aşağıdaki kod, uygulamanın eğitir ardından eğitim başarılı olana kadar bekler. Kod, 429 önlemek için bekleyin ve yeniden deneme stratejisi kullanır. "çok fazla istek var" hatası. 

Bir Node.js dosyası oluşturun ve içine aşağıdaki kodu kopyalayın. AuthoringKey, AppID VersionID ve bölge değerlerini değiştirin.

   [!code-javascript[Train LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/train.js "Train LUIS")]

Uygulama geliştirmek için kodu çalıştırmak için aşağıdaki komutu kullanın:

```console
node train.js
```

Çalıştırma çıktısı her bir yinelemesini LUIS modellerin eğitimi durumudur. Aşağıdaki yürütme eğitimlerini yalnızca bir onay gerekli:

```console
1 trained = true
[ { modelId: '2c549f95-867a-4189-9c35-44b95c78b70f',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '5530e900-571d-40ec-9c78-63e66b50c7d4',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '519faa39-ae1a-4d98-965c-abff6f743fe6',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '9671a485-36a9-46d5-aacd-b16d05115415',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '9ef7d891-54ab-48bf-8112-c34dcd75d5e2',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '8e16a660-8781-4abf-bf3d-f296ebe1bf2d',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } } ]

```
## <a name="publish-the-model"></a>Modeli yayımlayın
Liste varlığı uç noktasından kullanılabilir olacak şekilde yayımlayın.

Bir Node.js dosyası oluşturun ve içine aşağıdaki kodu kopyalayın. EndpointKey, AppID ve bölge değerlerini değiştirin. Bu dosya, kota sınırını aşan çağrı planlamıyorsanız, authoringKey kullanabilirsiniz.

   [!code-javascript[Publish LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/publish.js "Publish LUIS")]

Uygulama sorgulamak için kodu çalıştırmak için aşağıdaki komutu kullanın:

```console
node publish.js
```

Aşağıdaki çıktı, sorgular için uç nokta URL'sini içerir. Gerçek JSON sonuçları gerçek AppID verilebilir. 

```json
{ 
  versionId: null,
  isStaging: false,
  endpointUrl: 'https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>',
  region: null,
  assignedEndpointKey: null,
  endpointRegion: 'westus',
  publishedDateTime: '2018-01-29T22:17:38Z' }
}
```

## <a name="query-the-app"></a>Sorgu uygulama 
Liste varlığı cihaz türü belirlemek LUIS yardımcı kanıtlamak için uç nokta uygulamadan sorgu.

Bir Node.js dosyası oluşturun ve içine aşağıdaki kodu kopyalayın. EndpointKey, AppID ve bölge değerlerini değiştirin. Bu dosya, kota sınırını aşan çağrı planlamıyorsanız, authoringKey kullanabilirsiniz.

   [!code-javascript[Query LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/query.js "Query LUIS")]

Kodu çalıştırmak ve uygulamayı sorgulamak için aşağıdaki komutu kullanın:

```console
node train.js
```

Sorgu sonuçları çıkış alınır. Kodunu eklenmiş olduğunuzdan **ayrıntılı** tüm hedefleri ve puanlarını sorgu dizesi, çıkış adı/değer çifti içerir:

```json
{
  "query": "turn up the heat",
  "topScoringIntent": {
    "intent": "HomeAutomation.TurnOn",
    "score": 0.139018849
  },
  "intents": [
    {
      "intent": "HomeAutomation.TurnOn",
      "score": 0.139018849
    },
    {
      "intent": "None",
      "score": 0.120624863
    },
    {
      "intent": "HomeAutomation.TurnOff",
      "score": 0.06746891
    }
  ],
  "entities": [
    {
      "entity": "heat",
      "type": "HomeAutomation.Device",
      "startIndex": 12,
      "endIndex": 15,
      "score": 0.784990132
    },
    {
      "entity": "heat",
      "type": "DevicesList",
      "startIndex": 12,
      "endIndex": 15,
      "resolution": {
        "values": [
          "Thermostat"
        ]
      }
    }
  ]
}
```

Özel aygıt **Thermostat** "ısı'kurmak aç" sonucu odaklı bir sorgu ile tanımlanır. Orijinal HomeAutomation.Device varlık hala uygulamada olduğundan, sonuçları de görebilirsiniz. 

Bunlar ayrıca bir thermostat döndürülür görmek için diğer iki konuşma deneyin. 

|#|Utterance|varlık|type|değer|
|--|--|--|--|--|
|1|ac üzerinde Aç| AC | DevicesList | Thermostat|
|2|Isı Aç|Isı| DevicesList |Thermostat|
|3|soğuk olun|soğuk|DevicesList|Thermostat|

## <a name="next-steps"></a>Sonraki adımlar

Cihaz konumları odaları, Katlar veya binalar genişletmek için başka bir liste varlığı oluşturabilirsiniz. 
