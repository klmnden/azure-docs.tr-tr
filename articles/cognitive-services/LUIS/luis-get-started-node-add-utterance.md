---
title: 'Öğretici: Node.js kullanarak bir LUIS uygulamasına konuşma ekleme | Microsoft Docs'
description: Bu öğreticide Node.js kullanarak bir LUIS uygulamasını çağırmayı öğreneceksiniz.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: a1e028395ce4b1679a367ce110c2ba120128c501
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264235"
---
# <a name="tutorial-add-utterances-app-using-nodejs"></a>Öğretici: Node.js kullanarak uygulamaya konuşma ekleme 
Bu öğreticide amaca konuşma eklemek için Node.js'deki Authoring API'lerini kullanarak bir program yazacaksınız.

<!-- green checkmark -->
> [!div class="checklist"]
> * Visual Studio konsol projesi oluşturma 
> * Konuşma eklemek için LUIS API'sini çağırma yöntemi ekleme ve uygulamayı eğitme
> * BookFlight amacı için örnek konuşmaları içeren JSON dosyası ekleme
> * Konsolu çalıştırma ve konuşmalar için eğitim durumunu görme

Daha fazla bilgi için [amaca örnek konuşma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) ve [eğitim durumu](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46) API'lerinin teknik belgelerine bakın.

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS][LUIS] hesabına ihtiyacınız olacak.

## <a name="prerequisites"></a>Ön koşullar

* En son [**Node.js**](https://nodejs.org/en/download/) ve NPM.
* Bu makale için NPM bağımlılıkları: [**request**](https://www.npmjs.com/package/request), [**request-promise**](https://www.npmjs.com/package/request-promise), [**fs-extra**](https://www.npmjs.com/package/fs-extra).  
* **[Önerilen]** IntelliSense ve hata ayıklama için [Visual Studio Code](https://code.visualstudio.com/).
* LUIS **[yazma anahtarınız](luis-concept-keys.md#authoring-key)**. Bu anahtarı [LUIS](luis-reference-regions.md) web sitesinin Account Settings (Hesap Ayarları) bölümünde bulabilirsiniz.
* Mevcut LUIS [**uygulama kimliğiniz**](./luis-get-started-create-app.md). Uygulama kimliği, uygulama panosunda gösterilir. `utterances.json` dosyasında kullanılan amaçlara ve varlıklara sahip LUIS uygulamasının, kod `add-utterances.js` içinde çalıştırılmadan önce mevcut olması gerekir. Bu makaledeki kod, amaçları ve varlıkları oluşturmayacak. Yalnızca konuşmaları var olan amaçlara ve varlıklara ekleyecek. 
* Konuşmaları alan uygulamanın içindeki **sürüm kimliği**. Varsayılan kimlik: "0.1"
* VSCode içinde `add-utterances.js` projesi adlı yeni bir dosya oluşturun.

> [!NOTE] 
> `add-utterances.js` dosyasının tamamı [**LUIS-Samples** Github deposunda](https://github.com/Microsoft/LUIS-Samples/tree/master/examples/add-utterances/nodejs) mevcuttur.


## <a name="write-the-nodejs-code"></a>Node.js kodunu yazma

NPM bağımlılıklarını dosyaya ekleyin.

   [!code-javascript[NPM Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=16-19 "NPM Dependencies")]

LUIS sabitlerini dosyaya ekleyin. Aşağıdaki kodu kopyalayıp yazma anahtarı, uygulama kimliği ve sürüm kimliğini kendi değerlerinizle değiştirin.

   [!code-javascript[LUIS key and IDs](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=22-29 "LUIS key and IDs")]

Konuşmalarınızın bulunduğu yüklenecek dosyanın adını ve konumunu ekleyin. 

   [!code-javascript[Add upload file](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=31-33 "Add upload file")]

Komut satırı değerlerini tutan değişkenleri ekleyin.

   [!code-javascript[Add upload file](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=35-49 "Add upload file")]


HTTP çağrısı göndermek ve almak için `sendUtteranceToApi` işlevini ekleyin. 

   [!code-javascript[Send the HTTP request](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=135-151 "Send the HTTP request")]

`addUtterance` işlevi tarafından kullanılan yapılandırma JSON nesnesini ekleyin.

   [!code-javascript[Add configuration information for adding utterance](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=52-59 "Add configuration information for adding utterance")]

`SendUtteranceToApp` tarafından kullanılan API isteğini ve yanıtını yönetmek için `addUtterance` işlevini ekleyin.

   [!code-javascript[Add configuration information for adding utterance](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=62-92 "Add configuration information for adding utterance")]

`train` işlevi tarafından kullanılan yapılandırma JSON nesnesini ekleyin.

   [!code-javascript[Add configuration information for training LUIS](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=94-101 "Add configuration information for training LUIS")]

Eğitim işlemini başlatmak için `train` işlevini ekleyin. 

   [!code-javascript[Train the application](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=103-133 "Train the application")]

Komut satırı değişkenlerine göre gerçekleştirilecek eylemi (konuşma ekleme veya eğitme) seçen kodu ekleyin.

   [!code-javascript[Train the application](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=153-184 "Train the application")]

## <a name="specify-utterances-to-add"></a>Eklenecek konuşmaları belirtme
LUIS uygulamasına eklemek istediğiniz **konuşma dizisini** belirtmek için `utterances.json` dosyasını oluşturup düzenleyin. Amacın ve varlıkların LUIS uygulamasında mevcut olması **gerekir**.

> [!NOTE]
> `utterances.json` dosyasında kullanılan amaçlara ve varlıklara sahip LUIS uygulamasının, kod `add-utterances.js` içinde çalıştırılmadan önce mevcut olması gerekir. Bu makaledeki kod, amaçları ve varlıkları oluşturmayacak. Yalnızca konuşmaları var olan amaçlara ve varlıklara ekleyecek.

`text` alanı, konuşma metnini içerir. `intentName` alanı, LUIS uygulaması içindeki bir amacın adına karşılık gelmelidir. `entityLabels` alanı gereklidir. Varlıkları etiketlemek istemiyorsanız, aşağıdaki örnekte gösterildiği gibi boş bir liste sağlayın.

entityLabels listesi boş değilse `startCharIndex` ve `endCharIndex` öğelerinin `entityName` alanında başvurulan varlığın işaretlenmesi gerekir. İki dizin de sıfırdan başlar. Başka bir deyişle üst örnekteki 6 rakamı büyük S harfinden önceki boşluğu değil Seattle kelimesinin "S" harfini niteler.

```json
[
    {
        "text": "go to Seattle",
        "intentName": "BookFlight",
        "entityLabels": [
            {
                "entityName": "Location::LocationTo",
                "startCharIndex": 6,
                "endCharIndex": 12
            }
        ]
    },
    {
        "text": "book a flight",
        "intentName": "BookFlight",
        "entityLabels": []
    }
]
```

## <a name="add-an-utterance-from-the-command-line"></a>Komut satırından konuşma ekleme

Uygulamayı Node.js ile komut satırından çalıştırın.

Bağımsız değişken olmadan add-utterance çağrısını yaptığınızda uygulamaya konuşma eklenir ancak eğitim gerçekleştirilmez.
````
> node add-utterances.js
````

Bu örnek dosya, konuşma ekleme API'si çağrısının sonucuna sahip `results.json` içeren bir dosya oluşturur. `response` alanı, eklenen konuşmalar için bu biçimdedir. `hasError` false değere sahiptir ve konuşmanın eklendiğini belirtir.  

```json
    "response": [
        {
            "value": {
                "UtteranceText": "go to seattle",
                "ExampleId": -5123383
            },
            "hasError": false
        },
        {
            "value": {
                "UtteranceText": "book a flight",
                "ExampleId": -169157
            },
            "hasError": false
        }
    ]
```

## <a name="add-an-utterance-and-train-from-the-command-line"></a>Komut satırından konuşma ekleme ve eğitme
Eğitim isteği göndermek ve sonrasında eğitim durumunu istemek için add-utterance çağrısını `-train` bağımsız değişkeniyle yapın. Eğitim başladıktan hemen sonra durum sıraya alındı olarak değişir. Durum ayrıntıları bir dosyaya yazılır.

````
> node add-utterances.js -train
````

> [!NOTE]
> Yinelenen konuşmalar tekrar eklenmez ancak hataya da neden olmaz. `response`, özgün konuşmanın kimliğini içerir.

Örneği `-train` bağımsız değişkeniyle çağırdığınızda LUIS uygulamasını eğitme isteğinin başarıyla sıraya alındığını belirten bir `training-results.json` dosyası oluşturulur. 

Aşağıda başarılı bir eğitim isteğinin sonucu gösterilmiştir:
```json
{
    "request": null,
    "response": {
        "statusId": 9,
        "status": "Queued"
    }
}
```

Eğitim isteğinin sıraya alındıktan sonra tamamlanması zaman alabilir.

## <a name="get-training-status-from-the-command-line"></a>Komut satırından eğitim durumunu alma
Eğitim durumunu kontrol etmek ve durumla ilgili ayrıntılı bilgileri dosyaya yazmak için örneği `-status` bağımsız değişkeniyle çağırın.

````
> node add-utterances.js -status
````

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu öğreticiyi tamamladıktan sonra ihtiyacınız yoksa Visual Studio'yu ve konsol uygulamasını kaldırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Programlama yoluyla bir LUIS uygulaması oluşturma](luis-tutorial-node-import-utterances-csv.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
