---
title: 'Öğretici: Ruby kullanarak bir LUIS uygulamasına konuşma ekleme | Microsoft Docs'
description: Bu öğreticide Ruby kullanarak bir LUIS uygulamasını çağırmayı öğreneceksiniz.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 7a470fd551a58978e6f2be0450a2e2a6cd471fc4
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266065"
---
# <a name="tutorial-add-utterances-to-app-using-ruby"></a>Öğretici: Ruby kullanarak uygulamaya konuşma ekleme 
Bu öğreticide amaca konuşma eklemek için Ruby içindeki Authoring API'lerini kullanarak bir program yazacaksınız.

<!-- green checkmark -->
> [!div class="checklist"]
> * Visual Studio konsol projesi oluşturma 
> * Konuşma eklemek için LUIS API'sini çağırma yöntemi ekleme ve uygulamayı eğitme
> * BookFlight amacı için örnek konuşmaları içeren JSON dosyası ekleme
> * Konsolu çalıştırma ve konuşmalar için eğitim durumunu görme

Daha fazla bilgi için [amaca örnek konuşma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) ve [eğitim durumu](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46) API'lerinin teknik belgelerini inceleyin.

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS][LUIS] hesabına ihtiyacınız olacak.

## <a name="prerequisites"></a>Ön koşullar

* [Ruby](http://rubyinstaller.org/) 
* LUIS **[yazma anahtarınız](luis-concept-keys.md#authoring-key)**. Bu anahtarı [LUIS](luis-reference-regions.md) web sitesinin Account Settings (Hesap Ayarları) bölümünde bulabilirsiniz.
* Mevcut LUIS [**uygulama kimliğiniz**](./luis-get-started-create-app.md). Uygulama kimliği, uygulama panosunda gösterilir. `utterances.json` dosyasında kullanılan amaçlara ve varlıklara sahip LUIS uygulamasının, kod `add-utterances.rb` içinde çalıştırılmadan önce mevcut olması gerekir. Bu makaledeki kod, amaçları ve varlıkları oluşturmaz. Yalnızca konuşmaları var olan amaçlara ve varlıklara ekler. 
* Konuşmaları alan uygulamanın içindeki **sürüm kimliği**. Varsayılan kimlik: "0.1"
* VSCode içinde `add-utterances.rb` projesi adlı yeni bir dosya oluşturun.

> [!NOTE] 
> `add-utterances.cs` dosyasının tamamı ve örnek bir `utterances.json` dosyası [**LUIS-Samples** Github deposunda](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/authoring-api-samples/ruby/) mevcuttur.


## <a name="write-the-ruby-code"></a>Ruby kodunu yazma

Bağımlılıkları dosyaya ekleyin.

   [!code-ruby[Ruby and LUIS Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=1-28 "Ruby and LUIS Dependencies")]

Eğitim durumu için kullanılan GET isteğini ekleyin.

   [!code-ruby[SendGet](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=30-40 "SendGet")]

Konuşmaları oluşturmak veya eğitimi başlatmak için kullanılan POST isteğini ekleyin. 

   [!code-ruby[SendPost](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=42-54 "SendPost")]

`AddUtterances` işlevini ekleyin.

   [!code-ruby[AddUtterances method](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=56-61 "AddUtterances method")]


`Train` işlevini ekleyin. 

   [!code-ruby[Train](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=63-69 "Train")]

`Status` işlevini ekleyin.

   [!code-ruby[Status](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=71-75 "Status")]

Bağımsız değişkenleri yönetmek için ana kodu ekleyin.

   [!code-ruby[Main code](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=77-93 "Main code")]

## <a name="specify-utterances-to-add"></a>Eklenecek konuşmaları belirtme
LUIS uygulamasına eklemek istediğiniz **konuşma dizisini** belirtmek için `utterances.json` dosyasını oluşturup düzenleyin. Amacın ve varlıkların LUIS uygulamasında mevcut olması **gerekir**.

> [!NOTE]
> `utterances.json` dosyasında kullanılan amaçlara ve varlıklara sahip LUIS uygulamasının, kod `add-utterances.rb` içinde çalıştırılmadan önce mevcut olması gerekir. Bu makaledeki kod, amaçları ve varlıkları oluşturmaz. Yalnızca konuşmaları var olan amaçlara ve varlıklara ekler.

`text` alanı, konuşma metnini içerir. `intentName` alanı, LUIS uygulaması içindeki bir amacın adına karşılık gelmelidir. `entityLabels` alanı gereklidir. Varlıkları etiketlemek istemiyorsanız, aşağıdaki örnekte gösterildiği gibi boş bir liste sağlayın:

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

Uygulamayı Ruby ile komut satırından çalıştırın.

`add-utterances.rb` öğesini yalnızca utterance.json bağımsız değişkeniyle çağırmanız durumunda yeni konuşmalar eklenir ancak LUIS bu konuda eğitilmez.
````
> ruby add-utterances.rb ./utterances.json
````

Bu sonuç add utterances API'sini çağırmanın sonuçlarını gösterir. `response` alanı, eklenen konuşmalar için bu biçimdedir. `hasError` false değere sahiptir ve konuşmanın eklendiğini belirtir.  

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
Eğitim isteği göndermek için add-utterance öğesini `-train` bağımsız değişkeniyle birlikte çağırın.

````
> ruby add-utterances.rb ./utterances.json -train
````

> [!NOTE]
> Yinelenen konuşmalar tekrar eklenmez ancak hataya da neden olmaz. `response`, özgün konuşmanın kimliğini içerir.

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
Eğitim durumunu kontrol etmek için örneği `-status` bağımsız değişkeniyle çağırın.

````
> ruby add-utterances.rb ./utterances.json -status
````

```
Requested training status.
[
   {
      "modelId": "eb2f117c-e10a-463e-90ea-1a0176660acc",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "c1bdfbfc-e110-402e-b0cc-2af4112289fb",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "863023ec-2c96-4d68-9c44-34c1cbde8bc9",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "82702162-73ba-4ae9-a6f6-517b5244c555",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "37121f4c-4853-467f-a9f3-6dfc8cad2763",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "de421482-753e-42f5-a765-ad0a60f50d69",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "80f58a45-86f2-4e18-be3d-b60a2c88312e",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "c9eb9772-3b18-4d5f-a1e6-e0c31f91b390",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "2afec2ff-7c01-4423-bb0e-e5f6935afae8",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "95a81c87-0d7b-4251-8e07-f28d180886a1",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   }
]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu öğreticiyi tamamladıktan sonra ihtiyacınız yoksa Visual Studio'yu ve konsol uygulamasını kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Programlama yoluyla bir LUIS uygulaması oluşturma](luis-tutorial-node-import-utterances-csv.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
