---
title: Ruby Hızlı Başlangıç - modeli değiştirme ve LUIS uygulamasını eğitme
titleSuffix: Azure Cognitive Services
description: Bu Ruby hızlı başlangıcında, bir Ev Otomasyonu uygulamasına örnek konuşmalar ekleyip uygulamayı eğitin. Örnek konuşmalar, bir amaçla eşleşmiş kullanıcı konuşma metinleridir. Amaçlar için örnek konuşmalar sağlayarak, LUIS’e kullanıcı tarafından sağlanan hangi tür metinlerin hangi amaca ait olduğunu öğretirsiniz.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 2d4633592742bd81cc6972884a0c4f5141aa9943
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47030920"
---
# <a name="quickstart-change-model-using-ruby"></a>Hızlı Başlangıç: Ruby kullanarak model değiştirme

[!INCLUDE [Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [Ruby](http://rubyinstaller.org/) 
* [Visual Studio Code](https://code.visualstudio.com/)

[!INCLUDE [Code is available in LUIS-Samples Github repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Örnek konuşmalar JSON dosyası

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Hızlı başlangıç kodu oluşturma 

Bağımlılıkları `add-utterances.rb` adlı dosyaya ekleyin.

   [!code-ruby[Ruby and LUIS Dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/ruby/add-utterances.rb?range=1-21 "Ruby and LUIS Dependencies")]

Eğitim durumu için kullanılan GET isteğini ekleyin.

   [!code-ruby[SendGet](~/samples-luis/documentation-samples/quickstarts/change-model/ruby/add-utterances.rb?range=23-33 "SendGet")]

Konuşmaları oluşturmak veya eğitimi başlatmak için kullanılan POST isteğini ekleyin. 

   [!code-ruby[SendPost](~/samples-luis/documentation-samples/quickstarts/change-model/ruby/add-utterances.rb?range=35-47 "SendPost")]

`AddUtterances` işlevini ekleyin.

   [!code-ruby[AddUtterances method](~/samples-luis/documentation-samples/quickstarts/change-model/ruby/add-utterances.rb?range=49-54 "AddUtterances method")]


`Train` işlevini ekleyin. 

   [!code-ruby[Train](~/samples-luis/documentation-samples/quickstarts/change-model/ruby/add-utterances.rb?range=56-62 "Train")]

`Status` işlevini ekleyin.

   [!code-ruby[Status](~/samples-luis/documentation-samples/quickstarts/change-model/ruby/add-utterances.rb?range=64-68 "Status")]

Bağımsız değişkenleri yönetmek için ana kodu ekleyin.

   [!code-ruby[Main code](~/samples-luis/documentation-samples/quickstarts/change-model/ruby/add-utterances.rb?range=70-72 "Main code")]

## <a name="run-code"></a>Kodu çalıştırma

Uygulamayı Ruby ile komut satırından çalıştırın.

### <a name="add-an-utterance-from-the-command-line"></a>Komut satırından konuşma ekleme

`add-utterances.rb` komutunu çağırmak konuşmaları ekler, eğitir ve eğitim durumunu alır.

```CMD
> ruby add-utterances.rb 
```

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

Sonraki yanıt eğitimin kuyruğa alındığını gösterir. Ardından sonraki yanıt her amacı durumunu gösterir. 

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
Hızlı başlangıçla işiniz bittiğinde, bu hızlı başlangıçta oluşturulan tüm dosyaları kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Programlama yoluyla bir LUIS uygulaması oluşturma](luis-tutorial-node-import-utterances-csv.md)