---
title: Program aracılığıyla Node.js kullanarak HALUK uygulaması oluşturma | Microsoft Docs
titleSuffix: Azure
description: Program aracılığıyla verilerden önceden var olan HALUK yazma API kullanarak CSV biçiminde HALUK uygulamasının nasıl oluşturulacağını öğrenin.
services: cognitive-services
author: DeniseMak
manager: rstand
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/21/2018
ms.author: v-geberr
ms.openlocfilehash: e97dc184266bc9518ee5f909891bd97f7c71804b
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37113066"
---
# <a name="build-a-luis-app-programmatically-using-nodejs"></a>Program aracılığıyla Node.js kullanarak HALUK uygulaması oluşturma

HALUK her şeyi yapan programlı bir API sağlar [HALUK] [ LUIS] Web sitesi yapar. Bu, ne zaman önceden var olan verilere sahip ve program aracılığıyla daha HALUK uygulama bilgilerini el ile girerek oluşturmak için hızlı zamandan tasarruf edebilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

* Oturum [HALUK] [ LUIS] Web sitesi ve bulma, [anahtar yazma](luis-concept-keys.md#authoring-key) hesap ayarları'nda. Geliştirme API'leri çağırmak için bu anahtarı kullanın.
* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* Bu öğretici, kullanıcı isteklerinin kuramsal şirketin günlük dosyaları için bir CSV başlar. Karşıdan [burada](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/IoT.csv).
* En son Node.js ile NPM yükleyin. İndirin [burada](https://nodejs.org/en/download/).
* **[Önerilen]**  IntelliSense ve hata ayıklama için visual Studio Code buradan indirin [burada](https://code.visualstudio.com/) ücretsiz.

## <a name="map-preexisting-data-to-intents-and-entities"></a>Hedefleri ve varlıkları önceden var olan verileri eşleme
Farklı işlemler kullanıcılara eşlemeleri yapmak istediğiniz metin veri içeriyorsa, ile HALUK göz önünde oluşturulmadıysa bir sistem olsa bile, kullanıcı giriş HALUK amaçları için varolan kategorilerden eşlemesi ile gündeme olabilir. Önemli kelimeler ve ifadeler hangi kullanıcıların kabul içinde belirleyebiliyorsanız, bu sözcükleri varlıklara eşleyebilir.

`IoT.csv` dosyasını açın. Günlük, kullanıcı sorgularının nasıl kategorilere ayrılan, hangi kullanıcı etti ve bazı sütunları bunları dışında çekilen yararlı bilgiler dahil olmak üzere bir kuramsal ev Otomasyon hizmetine içerir. 

![CSV dosyası](./media/luis-tutorial-node-import-utterances-csv/csv.png) 

Gördüğünüz **RequestType** sütun hedefleri, olabilir ve **isteği** sütun bir örnek utterance gösterir. Utterance oluşursa diğer alanları varlıklar olabilir. Hedefleri, varlıkları ve örnek utterances olduğundan, basit bir örnek uygulama için gereksinimleri vardır.

## <a name="steps-to-generate-a-luis-app-from-non-luis-data"></a>HALUK olmayan verileri HALUK uygulama oluşturmak için adımlar
Kaynak dosyasından yeni bir HALUK uygulaması oluşturmak için önce CSV dosyasından veriler ayrıştırma ve bu verileri yazma API kullanarak HALUK yükleyebilirsiniz bir biçime dönüştürün. Ayrıştırılmış verilerden ne hedefleri ve varlıkları vardır bilgi toplayın. Sonra bir uygulama oluşturmak için API çağrıları yapma ve amaçları ve ayrıştırılmış verilerden toplanan varlıklar ekleyin. HALUK uygulama oluşturduktan sonra örnek utterances ayrıştırılmış verileri ekleyebilirsiniz. Bu akış aşağıdaki kodu son bölümünde görebilirsiniz. Kopyalama veya [karşıdan](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/index.js) Bu kod ve içinde kaydedin `index.js`.

   [!code-javascript[Node.js code for calling the steps to build a LUIS app](~/samples-luis/examples/build-app-programmatically-csv/index.js)]


## <a name="parse-the-csv"></a>CSV ayrıştırılamıyor

CSV'ye utterances içeren sütun girişleri HALUK anlayabileceği bir JSON biçimine ayrıştırılması gerekir. Bu JSON biçimi içermesi gereken bir `intentName` utterance amacı tanımlayan alan. Ayrıca içermesi gereken bir `entityLabels` utterance varlık, boş alan. 

Örneğin, "ışık Aç" girişini bu JSON eşler:

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

Bu örnekte, `intentName` altında Kullanıcı isteği geldiği **isteği** CSV dosyasındaki sütun başlığını ve `entityName` anahtar bilgileri diğer sütunlarla alanından gelir. Örneğin, bir giriş için ise **işlemi** veya **aygıt**ve dize de gerçek istekte oluşur ve ardından bir varlık olarak etiketli. Aşağıdaki kod bu işlem ayrıştırma gösterir. Kopyalayabilir veya [karşıdan](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/_parse.js) onu ve kaydetmesi `_parse.js`.

   [!code-javascript[Node.js code for parsing a CSV file to extract intents, entities, and labeled utterances](~/samples-luis/examples/build-app-programmatically-csv/_parse.js)]



## <a name="create-the-luis-app"></a>HALUK uygulaması oluşturma
Verileri JSON içinde Ayrıştırılan sonra bir HALUK uygulamasına ekleyin. Aşağıdaki kod HALUK uygulamasını oluşturur. Kopyalama veya [karşıdan](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/_create.js) ve içine kaydedin `_create.js`.

   [!code-javascript[Node.js code for creating a LUIS app](~/samples-luis/examples/build-app-programmatically-csv/_create.js)]


## <a name="add-intents"></a>Hedef ekleme
Bir uygulama oluşturduktan sonra onu amaçları için gerekir. Aşağıdaki kod HALUK uygulamasını oluşturur. Kopyalama veya [karşıdan](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/_intents.js) ve içine kaydedin `_intents.js`.

   [!code-javascript[Node.js code for creating a series of intents](~/samples-luis/examples/build-app-programmatically-csv/_intents.js)]


## <a name="add-entities"></a>Varlık ekleme
Aşağıdaki kod HALUK uygulama varlıkları ekler. Kopyalama veya [karşıdan](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/_entities.js) ve içine kaydedin `_entities.js`.

   [!code-javascript[Node.js code for creating entities](~/samples-luis/examples/build-app-programmatically-csv/_entities.js)]
   


## <a name="add-utterances"></a>Konuşma ekleme
Hedefleri ve varlıkları HALUK uygulamasında tanımlanmış sonra utterances ekleyebilirsiniz. Aşağıdaki kod [Utterances_AddBatch](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09) API, aynı anda en fazla 100 utterances eklemenizi sağlar.  Kopyalama veya [karşıdan](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/_upload.js) ve içine kaydedin `_upload.js`.

   [!code-javascript[Node.js code for adding utterances](~/samples-luis/examples/build-app-programmatically-csv/_upload.js)]


## <a name="run-the-code"></a>Kodu çalıştırma


### <a name="install-nodejs-dependencies"></a>Node.js bağımlılıkları yükler
Node.js bağımlılıkları NPM terminal/komut satırında yükleyin.

````
> npm install
````

### <a name="change-configuration-settings"></a>Yapılandırma ayarlarını değiştirme
Bu uygulamayı kullanmak için index.js dosyasındaki değerleri kendi uç noktası anahtarı değiştirmek ve uygulamanızın sahip olmasını istediğiniz adı sağlamanız gerekir. Ayrıca, uygulamanın kültür ayarlayın veya sürüm numarasını değiştirin.

İndex.js dosyasını açın ve dosyanın üst bu değerleri değiştirin.


````JavaScript
// Change these values
const LUIS_programmaticKey = "YOUR_PROGRAMMATIC_KEY";
const LUIS_appName = "Sample App";
const LUIS_appCulture = "en-us"; 
const LUIS_versionId = "0.1";
````
### <a name="run-the-script"></a>Komut dosyasını çalıştır
Node.js ile terminal/komut satırından komut dosyasını çalıştırın.

````
> node index.js
````
or
````
> npm start
````

### <a name="application-progress"></a>Uygulama ilerleme durumu
Uygulama çalışırken, komut satırında ilerleme durumunu gösterir. Komut satırı çıkışı HALUK yanıtlarının biçimi içerir.

````
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
````




## <a name="open-the-luis-app"></a>HALUK uygulamasını açın
Komut dosyası tamamlandıktan sonra oturum açabildiğinizden [HALUK] [ LUIS] ve altında oluşturduğunuz HALUK uygulama görmek **My uygulamaları**. Eklediğiniz altında utterances görmeye olmalıdır **Sok**, **kapatma**, ve **hiçbiri** amaçlar.

![Sok hedefi](./media/luis-tutorial-node-import-utterances-csv/imported-utterances-661.png)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Test ve HALUK Web uygulamanızda eğitme](interactive-test.md)

## <a name="additional-resources"></a>Ek kaynaklar

Bu örnek uygulama aşağıdaki HALUK API'lerini kullanır:
- [uygulama oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36)
- [hedefleri Ekle](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0c)
- [Varlıklar ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0e) 
- [utterances Ekle](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09) 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions

