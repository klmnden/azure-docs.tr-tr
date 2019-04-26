---
title: Node.js kullanarak konuşma alma
titleSuffix: Azure
description: Önceden var olan verileri CSV biçiminde yazma LUIS API'si kullanarak program aracılığıyla bir LUIS uygulaması oluşturmayı öğrenin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/30/2019
ms.author: diberry
ms.openlocfilehash: 314d121e8964ba1cdbb457260826d85bf8505fbc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60494906"
---
# <a name="build-a-luis-app-programmatically-using-nodejs"></a>Node.js kullanarak program aracılığıyla LUIS uygulaması oluşturma

LUIS, her şeyi yapan programlı bir API sağlar [LUIS](luis-reference-regions.md) Web sitesi yok. Bu, ne zaman önceden var olan verilere sahip olduğunuz ve bilgileri el ile girerek bir LUIS uygulaması daha programlı olarak oluşturmak için daha hızlı zamandan tasarruf edebilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

* Oturum [LUIS](luis-reference-regions.md) Web sitesi ve bulma, [anahtar yazma](luis-concept-keys.md#authoring-key) hesap ayarları'nda. Yazma API'leri çağırmak için bu anahtarı kullanırsınız.
* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* Bu öğretici, kullanıcı isteklerinin kuramsal bir şirketin günlük dosyaları için bir CSV ile başlar. İndirdiği [burada](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/build-app-programmatically-csv/IoT.csv).
* NPM ile en son Node.js yükleyin. İndirdiği [burada](https://nodejs.org/en/download/).
* **[Önerilen]**  Visual Studio Code için IntelliSense ve hata ayıklama, buradan indirin [burada](https://code.visualstudio.com/) ücretsiz.

## <a name="map-preexisting-data-to-intents-and-entities"></a>Amaç ve varlıkları için önceden var olan veri eşlemesi
Haritalar farklı şeyler kullanıcılara yapmak istediğiniz metin veri içeriyorsa, LUIS göz önünde bulundurun oluşturulmadıysa bir sistemde sahip olsanız bile, kullanıcı giriş LUIS içinde hedefleri için mevcut kategorilerden eşlemesi ile gündeme olabilir. Önemli bir sözcük ve tümcecikleri içinde hangi kullanıcıların kabul belirleyebiliyorsanız, bu sözcükler varlıklarına eşleyebilir.

`IoT.csv` dosyasını açın. Bu, kullanıcı sorgularının bir kuramsal ev Otomasyonu hizmetinde, bunları dışında çekilen yararlı bilgilerle nasıl kategorilere ayrılan, hangi kullanıcı etti ve bazı sütunlar gibi günlük içerir. 

![Önceden mevcut olan verileri CSV dosyası](./media/luis-tutorial-node-import-utterances-csv/csv.png) 

Gördüğünüz **RequestType** sütun amacı, olabilir ve **istek** sütun örnek utterance gösterir. Utterance içinde oluşursa diğer alanları varlıkları olabilir. Amacı, varlıkları ve örnek konuşma olduğundan, basit bir örnek uygulama için gereksinimleri vardır.

## <a name="steps-to-generate-a-luis-app-from-non-luis-data"></a>LUIS olmayan verilerden bir LUIS uygulaması oluşturmak için adımları
Kaynak dosyasından yeni bir LUIS uygulaması oluşturmak için ilk, CSV dosyasındaki verileri ayrıştırılamadı ve bu veriler için LUIS yazma API kullanarak karşıya yüklediğiniz bir biçimine dönüştürün. Ayrıştırılmış verilerden ne amaç ve varlıkları vardır bilgi toplayabilir. Ardından, uygulamayı oluşturmak için API çağrıları yapabilir ve hedefleri ve toplanan verileri ayrıştırılmış varlıklar ekleyin. LUIS uygulaması oluşturduktan sonra örnek konuşma ayrıştırılmış verileri ekleyebilirsiniz. Bu akış aşağıdaki kodun son bölümünde görebilirsiniz. Kopyalama veya [indirme](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/build-app-programmatically-csv/index.js) Bu kod ve kaydedin `index.js`.

   [!code-javascript[Node.js code for calling the steps to build a LUIS app](~/samples-luis/examples/build-app-programmatically-csv/index.js)]


## <a name="parse-the-csv"></a>CSV ayrıştırma

LUIS anlayabileceği bir JSON biçimine ayrıştırılacak konuşma csv içeren sütun girişleri sahip. Bu JSON biçiminde içermelidir bir `intentName` utterance amacını tanımlayan alan. Ayrıca içermelidir bir `entityLabels` alanı utterance hiçbir varlık değeri boş olabilir. 

Örneğin, "Işıkları Aç" girişini bu JSON olarak eşler:

```json
        {
            "text": "Turn on the lights",
            "intentName": "TurnOn",
            "entityLabels": [
                {
                    "entityName": "Operation",
                    "startCharIndex": 5,
                    "endCharIndex": 6
                },
                {
                    "entityName": "Device",
                    "startCharIndex": 12,
                    "endCharIndex": 17
                }
            ]
        }
```

Bu örnekte, `intentName` altında kullanıcı isteğin geldiği **isteği** sütun başlığı CSV dosyasındaki ve `entityName` anahtar bilgisi ile diğer sütunlardan gelir. Örneğin, bir giriş için ise **işlemi** veya **cihaz**ve dize de gerçek istekte oluşur ve ardından bir varlık olarak etiketlenmiş. Aşağıdaki kodu ayrıştırma işlemi gösterilmektedir. Kopyalayabilir veya [indirme](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/build-app-programmatically-csv/_parse.js) bunu ve kaydetmesi `_parse.js`.

   [!code-javascript[Node.js code for parsing a CSV file to extract intents, entities, and labeled utterances](~/samples-luis/examples/build-app-programmatically-csv/_parse.js)]



## <a name="create-the-luis-app"></a>LUIS uygulaması oluşturma
JSON'a veri ayrıştırıldıktan sonra bir LUIS uygulaması ekleyin. Aşağıdaki kod LUIS uygulaması oluşturur. Kopyalama veya [indirme](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/build-app-programmatically-csv/_create.js) ve içine kaydedin `_create.js`.

   [!code-javascript[Node.js code for creating a LUIS app](~/samples-luis/examples/build-app-programmatically-csv/_create.js)]


## <a name="add-intents"></a>Hedef ekleme
Bir uygulama oluşturduktan sonra ona hedefleri gerekir. Aşağıdaki kod LUIS uygulaması oluşturur. Kopyalama veya [indirme](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/build-app-programmatically-csv/_intents.js) ve içine kaydedin `_intents.js`.

   [!code-javascript[Node.js code for creating a series of intents](~/samples-luis/examples/build-app-programmatically-csv/_intents.js)]


## <a name="add-entities"></a>Varlık ekleme
Aşağıdaki kod varlıkları için LUIS uygulaması ekler. Kopyalama veya [indirme](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/build-app-programmatically-csv/_entities.js) ve içine kaydedin `_entities.js`.

   [!code-javascript[Node.js code for creating entities](~/samples-luis/examples/build-app-programmatically-csv/_entities.js)]
   


## <a name="add-utterances"></a>Konuşma ekleme
LUIS uygulama varlıkları ve hedefleri tanımlanmış sonra konuşma ekleyebilirsiniz. Aşağıdaki kod [Utterances_AddBatch](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09) aynı anda en fazla 100 Konuşma ekleme olanak tanıyan API.  Kopyalama veya [indirme](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/build-app-programmatically-csv/_upload.js) ve içine kaydedin `_upload.js`.

   [!code-javascript[Node.js code for adding utterances](~/samples-luis/examples/build-app-programmatically-csv/_upload.js)]


## <a name="run-the-code"></a>Kodu çalıştırma


### <a name="install-nodejs-dependencies"></a>Node.js bağımlılıklarını yükleyin
Node.js bağımlılıkları NPM'den terminal/komut satırında yükleyin.

```console
> npm install
```

### <a name="change-configuration-settings"></a>Yapılandırma ayarlarını değiştirme
Bu uygulamayı kullanmak için kendi uç noktası anahtarı için değerler index.js dosyasını değiştirin ve istediğiniz uygulamanın adını sağlayın gerekir. Ayrıca, uygulamanın kültüre ayarlayın veya sürüm numarasını değiştirin.

İndex.js dosyasını açın ve dosyanın üst bu değerleri değiştirin.


```javascript
// Change these values
const LUIS_programmaticKey = "YOUR_PROGRAMMATIC_KEY";
const LUIS_appName = "Sample App";
const LUIS_appCulture = "en-us"; 
const LUIS_versionId = "0.1";
```

### <a name="run-the-script"></a>Betiği çalıştırın
Node.js ile bir terminal/komut satırından betiği çalıştırın.

```console
> node index.js
```

or

```console
> npm start
```

### <a name="application-progress"></a>Uygulama ilerleme durumu
Komut satırı, uygulama çalışırken ilerleme durumunu gösterir. Komut satırı çıkışı, luıs'den yanıtları biçimini içerir.

```console
> node index.js
intents: ["TurnOn","TurnOff","Dim","Other"]
entities: ["Operation","Device","Room"]
parse done
JSON file should contain utterances. Next step is to create an app with the intents and entities it found.
Called createApp, created app with ID 314b306c-0033-4e09-92ab-94fe5ed158a2
Called addIntents for intent named TurnOn.
Called addIntents for intent named TurnOff.
Called addIntents for intent named Dim.
Called addIntents for intent named Other.
Results of all calls to addIntent = [{"response":"e7eaf224-8c61-44ed-a6b0-2ab4dc56f1d0"},{"response":"a8a17efd-f01c-488d-ad44-a31a818cf7d7"},{"response":"bc7c32fc-14a0-4b72-bad4-d345d807f965"},{"response":"727a8d73-cd3b-4096-bc8d-d7cfba12eb44"}]
called addEntity for entity named Operation.
called addEntity for entity named Device.
called addEntity for entity named Room.
Results of all calls to addEntity= [{"response":"6a7e914f-911d-4c6c-a5bc-377afdce4390"},{"response":"56c35237-593d-47f6-9d01-2912fa488760"},{"response":"f1dd440c-2ce3-4a20-a817-a57273f169f3"}]
retrying add examples...

Results of add utterances = [{"response":[{"value":{"UtteranceText":"turn on the lights","ExampleId":-67649},"hasError":false},{"value":{"UtteranceText":"turn the heat on","ExampleId":-69067},"hasError":false},{"value":{"UtteranceText":"switch on the kitchen fan","ExampleId":-3395901},"hasError":false},{"value":{"UtteranceText":"turn off bedroom lights","ExampleId":-85402},"hasError":false},{"value":{"UtteranceText":"turn off air conditioning","ExampleId":-8991572},"hasError":false},{"value":{"UtteranceText":"kill the lights","ExampleId":-70124},"hasError":false},{"value":{"UtteranceText":"dim the lights","ExampleId":-174358},"hasError":false},{"value":{"UtteranceText":"hi how are you","ExampleId":-143722},"hasError":false},{"value":{"UtteranceText":"answer the phone","ExampleId":-69939},"hasError":false},{"value":{"UtteranceText":"are you there","ExampleId":-149588},"hasError":false},{"value":{"UtteranceText":"help","ExampleId":-81949},"hasError":false},{"value":{"UtteranceText":"testing the circuit","ExampleId":-11548708},"hasError":false}]}]
upload done
```




## <a name="open-the-luis-app"></a>LUIS uygulaması açın
Betik tamamlandıktan sonra üzerinde oturum açabildiğinizden [LUIS](luis-reference-regions.md) ve altında oluşturduğunuz LUIS uygulaması **uygulamalarım**. Eklediğiniz altında konuşma görmeye olmalıdır **Sok**, **kapatma**, ve **hiçbiri** amacı.

![Sok hedefi](./media/luis-tutorial-node-import-utterances-csv/imported-utterances-661.png)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Test ve LUIS Web uygulamanızda eğitin](luis-interactive-test.md)

## <a name="additional-resources"></a>Ek kaynaklar

Bu örnek uygulama aşağıdaki LUIS API'leri kullanır:
- [uygulama oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36)
- [hedef ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0c)
- [Varlık Ekle](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0e) 
- [Konuşma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09)
