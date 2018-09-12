---
title: 'Hızlı Başlangıç: Go kullanarak model değiştirme ve LUIS uygulamasını eğitme - Azure Bilişsel Hizmetler | Microsoft Docs'
description: Bu Go hızlı başlangıcında, bir Ev Otomasyonu uygulamasına örnek konuşmalar ekleyip uygulamayı eğitin. Örnek konuşmalar, bir amaçla eşleşmiş kullanıcı konuşma metinleridir. Amaçlar için örnek konuşmalar sağlayarak, LUIS’e kullanıcı tarafından sağlanan hangi tür metinlerin hangi amaca ait olduğunu öğretirsiniz.
titleSuffix: Microsoft Cognitive Services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/24/2018
ms.author: diberry
ms.openlocfilehash: 6fa95a31e3a5f81adc3092ba7f272cd282f45e48
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43772560"
---
# <a name="quickstart-change-model-using-go"></a>Hızlı Başlangıç: Go kullanarak model değiştirme

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Ön koşullar

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [Go](https://golang.org/) programlama dilini yükleyin.
* [VSCode](https://code.visualstudio.com) 

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Örnek konuşmalar JSON dosyası

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Hızlı başlangıç kodu oluşturma 

1. VSCode ile `add-utterances.go` öğesini oluşturun. 

2. Bağımlılıkları ekleyin. 

   [!code-go[Add dependencies.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=2-10 "Add dependencies.")]

3. Üst bilgideki yazar anahtarını ileten genel HTTP isteği işlevini ekleyin. 

   [!code-go[Add HTTP request function which includes passing authoring key in header. ](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=12-36 "Add HTTP request function, which includes passing authoring key in header. ")]

4. JSON dosyasından örnek konuşmaları ekleyin.

   [!code-go[Add example utterances from JSON file.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=62-76 "Add example utterances from JSON file.")]

5. Eğitim isteğinde bulunun. FİİLİ eğitim durumuyla aynı yolda ayarlamak için yardımcı bir işlev kullanın. 

   [!code-go[Request training. ](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=77-86 "Request training. ")]

6. Eğitim durumu isteğinde bulunun. FİİLİ istek eğitimiyle aynı yolda ayarlamak için yardımcı bir işlev kullanın. 

   [!code-go[Request training status. ](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=87-90 "Request training status. ")]

7. Komut satırı ayrıştırma için ana işlevi ekleyin.

   [!code-go[Add main function to handle command line parsing. ](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=38-60 "Add main function to handle command-line parsing.")]

## <a name="add-an-utterance-from-the-command-line-train-and-get-status"></a>Komut satırından konuşma ekleme, eğitme ve durumu alma

1. Go dosyasını oluşturduğunuz dizinde bir komut istemi ile, Go dosyasını derlemek için `go build add-utterances.go` girin. Komut istemi başarılı bir derleme için herhangi bir bilgi döndürmez.

2. Aşağıdaki metni komut istemine girerek komut satırından Go uygulamasını çalıştırın: 

    ```CMD
    add-utterances -appID <your-app-id> -authoringKey <add-your-authoring-key> -version <your-version-id> -region westus -utteranceFile utterances.json

    ```

    `<add-your-authoring-key>` yerine yazma anahtarınızın (başlangıç anahtarı olarak da bilinir) değerini yazın. `<your-app-id>` değerini uygulama kimliği değeriyle değiştirin. `<your-version-id>` değerini sürüm değeriyle değiştirin. Varsayılan sürüm: `0.1`.

    Bu komut sonuçları şu görüntüler:

    ```CMD
    add example utterances requested
    [
        {
            "text": "go lang 1",
            "intentName": "None",
            "entityLabels": []
        }
    ,
        {
            "text": "go lang 2",
            "intentName": "None",
            "entityLabels": []
        }
    ]
    201
    [
        {
            "value": {
                "ExampleId": 77783998,
                "UtteranceText": "go lang 1"
            },
            "hasError": false
        },
        {
            "value": {
                "ExampleId": 77783999,
                "UtteranceText": "go lang 2"
            },
            "hasError": false
        }
    ]
    training selected
    202
    {"statusId":9,"status":"Queued"}
    training status selected
    200
    [
        {
            "modelId": "c52d6509-9261-459e-90bc-b3c872ee4a4b",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "5119cbe8-97a1-4c1f-85e6-6449f3a38d77",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "01e6b6bc-9872-47f9-8a52-da510cddfafe",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "33b409b2-32b0-4b0c-9e91-31c6cfaf93fb",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "1fb210be-2a19-496d-bb72-e0c2dd35cbc1",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "3d098beb-a1aa-423f-a0ae-ce08ced216d6",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "cce854f8-8f8f-4ed9-a7df-44dfea562f62",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "4d97bf0d-5213-4502-9712-2d6e77c96045",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        }
    ]
    ```

    Bu yanıt üç HTTP yanıtı için HTTP durum kodunun yanı sıra yanıt gövdesinde döndürülen JSON yanıtlarını döndürür. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
Hızlı başlangıçla işiniz bittiğinde, bu hızlı başlangıçta oluşturulan tüm dosyaları kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Özel etki alanıyla bir uygulama derleme](luis-quickstart-intents-only.md) 