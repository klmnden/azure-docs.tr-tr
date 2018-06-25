---
title: Etiket otomatik olarak Nodejs kullanarak bir liste varlık varlıklarıyla | Microsoft Docs
description: HALUK etiket varyasyonları bir sözcük veya tümcecik yardımcı olmak için bir liste varlık eklemeyi öğrenin.
services: cognitive-services
author: v-geberr
titleSuffix: Azure
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/21/2018
ms.author: v-geberr
ms.openlocfilehash: e8558ecf4a64dbccef6e6367c1447bdcdb005126
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "35351778"
---
# <a name="use-a-list-entity-to-increase-entity-detection"></a>Varlık algılama artırmak için bir liste varlık kullanın 
Bu öğretici kullanımını gösteren bir [listesi varlık](luis-concept-entity-types.md) varlık algılama artırmak için. Liste varlıkları terimlerin tam bir eşleşme olarak etiketli gerekmez.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
* Bir liste varlık oluştur 
* Normalleştirilmiş değerleri ve eş anlamlıları ekleme
* Geliştirilmiş varlık kimliği doğrula

## <a name="prerequisites"></a>Önkoşullar

> [!div class="checklist"]
> * En son [Node.js](https://nodejs.org)
> * [HomeAutomation HALUK uygulama](luis-get-started-create-app.md). Oluşturulan giriş Otomasyon uygulama yoksa yeni bir uygulama oluşturmak ve önceden oluşturulmuş etki alanı ekleme **HomeAutomation**. Eğitmek ve uygulamayı yayımlama. 
> * [AuthoringKey](luis-concept-keys.md#authoring-key), [EndpointKey](luis-concept-keys.md#endpoint-key) (birçok kez sorgulama değilse), uygulama kimliği, sürüm kimliği ve [bölge](luis-reference-regions.md) HALUK uygulaması.

> [!Tip]
> Bir abonelik zaten yoksa için kaydedebilirsiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).

Bu öğreticideki kod tümünün edinilebilir [HALUK-Samples github deposunu](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-list-entity). 

## <a name="use-homeautomation-app"></a>HomeAutomation uygulama kullanma
Aygıtları ışık, eğlence sistemleri ve ortam gibi denetimin Isıtma ve soğutma gibi denetimleri HomeAutomation uygulama sağlar. Bu sistemler üretici adları, takma adlar, kısaltmalar ve argo içerebilir birkaç farklı adlara sahip. 

Birçok adları farklı kültürler ve demografisine sahip bir thermostat sistemidir. Bir thermostat soğutma hem bir ev veya yapı için sistemleri ısıtma kontrol edebilirsiniz.

İdeal olarak, aşağıdaki utterances önceden oluşturulmuş varlığa çözümlenmelidir **HomeAutomation.Device**:

|#|utterance|tanımlanan varlığı|puan|
|--|--|--|--|
|1|üzerinde ac Aç|HomeAutomation.Device - "ac"|0.8748562|
|2|Isı Aç|HomeAutomation.Device - "ısı"|0.784990132|
|3|soğuk olun|||

İlk iki utterances farklı cihazlara eşleyin. Üçüncü utterance "soğuk make", bir aygıtın eşlenmesi etmez ancak bunun yerine bir sonuç ister. HALUK terimi, "soğuk", istenen aygıt thermostat olduğu anlamına gelir bilmiyor. İdeal olarak, HALUK bu utterances tümünün aynı cihaza çözümlenmelidir. 

## <a name="use-a-list-entity"></a>Bir liste varlık kullanın
HomeAutomation.Device varlık aygıtların veya birkaç Çeşitlemeler adları ile küçük bir sayı için harika bir seçenek değil. Bir ofis binasının veya kampüs için aygıt adlarını HomeAutomation.Device varlık yararlılığını artar. 

A **listesi varlık** kümesi için bir aygıt bir yapı veya kampüs bilinen kümesi çok büyük bir küme olsa bile bu senaryo için iyi bir seçimdir olduğundan. Bir liste varlık kullanarak HALUK herhangi bir olası değer thermostat kümesindeki alabilir ve yalnızca tek aygıt aşağıya doğru "thermostat" çözümleyin. 

Bu öğretici ile thermostat bir varlık listesi oluşturmak için geçiyor. Bu öğreticide thermostat için alternatif adları şunlardır: 

|thermostat için diğer adlar|
|--|
| AC |
| Hesap|
| a-c|
|ısıtıcı|
|Etkin|
|hotter|
|soğuk|
|soğuk|

Genellikle, yeni bir seçenek belirlemek HALUK gerekiyorsa sonra bir [tümcecik listesi](luis-concept-feature.md#how-to-use-phrase-lists) daha iyi bir yanıt.

## <a name="create-a-list-entity"></a>Bir liste varlık oluştur
Node.js dosyası oluşturun ve aşağıdaki kodu buraya kopyalayın. AuthoringKey, AppID, VersionID ve bölge değerlerini değiştirin.

   [!code-javascript[Create DevicesList List Entity](~/samples-luis/documentation-samples/tutorial-list-entity/add-entity-list.js "Create DevicesList List Entity")]

NPM bağımlılıkları yükler ve liste varlık oluşturmak için kodu çalıştırmak için aşağıdaki komutu kullanın:

```Javascript
npm install && node add-entity-list.js
```

Çalıştır çıktısını listesi varlık Kimliğini gösterir:

```Javascript
026e92b3-4834-484f-8608-6114a83b03a6
```
## <a name="train-the-model"></a>Modeli eğitme
Sorgu sonuçlarını etkilemek yeni liste sırayla HALUK eğitmek. Eğitim, eğitim eğitim yapıldığında durumunu denetleme, iki parçalı işlemidir. Bir uygulama birçok modellerle eğitmek için birkaç dakika sürebilir. Aşağıdaki kod, uygulamanın eğitir sonra eğitim başarılı olana kadar bekler. Kod bekleyin ve yeniden deneme stratejisini 429 önlemek için kullanır. "çok sayıda istek" hatası. 

Node.js dosyası oluşturun ve aşağıdaki kodu buraya kopyalayın. AuthoringKey, AppID, VersionID ve bölge değerlerini değiştirin.

   [!code-javascript[Train LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/train.js "Train LUIS")]

Uygulama eğitmek için kodu çalıştırmak için aşağıdaki komutu kullanın:

```Javascript
node train.js
```

Çalıştır çıktısını HALUK modelleri eğitim her bir yineleme durumudur. Aşağıdaki yürütme eğitim yalnızca bir onay gerekli:

```Javascript
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
## <a name="publish-the-model"></a>Modele yayımlama
Liste varlık uç noktasından kullanılabilir olacak şekilde yayımlayın.

Node.js dosyası oluşturun ve aşağıdaki kodu buraya kopyalayın. EndpointKey, AppID ve bölge değerlerini değiştirin. Bu dosya, kota sınırı aşan çağırmak düşünmüyorsanız, authoringKey kullanabilirsiniz.

   [!code-javascript[Publish LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/publish.js "Publish LUIS")]

Uygulama sorgulamak için kodu çalıştırmak için aşağıdaki komutu kullanın:

```Javascript
node publish.js
```

Aşağıdaki çıkış tüm sorgular için uç nokta URL'sini içerir. Gerçek JSON sonuçları gerçek AppID içerir. 

```JSON
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
Uygulama listesi varlığı aygıt türü belirlemek HALUK yardımcı kanıtlamak için uç noktasından sorgu.

Node.js dosyası oluşturun ve aşağıdaki kodu buraya kopyalayın. EndpointKey, AppID ve bölge değerlerini değiştirin. Bu dosya, kota sınırı aşan çağırmak düşünmüyorsanız, authoringKey kullanabilirsiniz.

   [!code-javascript[Query LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/query.js "Query LUIS")]

Kodu çalıştırın ve uygulama sorgulamak için aşağıdaki komutu kullanın:

```Javascript
node train.js
```

Çıktı, sorgu sonuçları şeklindedir. Kod eklenmiş olduğundan **ayrıntılı** tüm hedefleri ve puanlarını sorgu dizesi, çıktı ad/değer çifti içerir:

```JSON
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

Belirli aygıt **Thermostat** "Aç" ısı yukarı sonuç odaklı sorgusu ile tanımlanır. Özgün HomeAutomation.Device varlık hala uygulamada olduğundan sonuçlarını da görebilirsiniz. 

Bunlar ayrıca thermostat döndürülür görmek için diğer iki utterances deneyin. 

|#|utterance|varlık|type|değer|
|--|--|--|--|--|
|1|üzerinde ac Aç| AC | DevicesList | Thermostat|
|2|Isı Aç|Isı| DevicesList |Thermostat|
|3|soğuk olun|soğuk|DevicesList|Thermostat|

## <a name="next-steps"></a>Sonraki adımlar

Cihaz konumları odaları, Katlar veya binalar genişletmek için başka bir liste varlık oluşturabilirsiniz. 
