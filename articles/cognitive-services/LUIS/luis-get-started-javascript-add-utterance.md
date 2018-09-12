---
title: 'Hızlı başlangıç: JavaScript kullanarak bir LUIS uygulamasına konuşma eklemeyi öğrenme - Azure Bilişsel Hizmetler | Microsoft Docs'
description: Bu hızlı başlangıçta JavaScript kullanarak bir LUIS uygulamasını çağırmayı öğreneceksiniz.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/24/2018
ms.author: diberry
ms.openlocfilehash: 0920a194d3e9c93883b88b7131f7e81dc8fb3302
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159699"
---
# <a name="quickstart-change-model-using-javascript"></a>Hızlı Başlangıç: JavaScript kullanarak model değiştirme

[!INCLUDE [Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [Visual Studio Code](https://code.visualstudio.com/).

[!INCLUDE [Code is available in LUIS-Samples Github repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Örnek konuşmalar JSON dosyası

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]


## <a name="create-quickstart-code"></a>Hızlı başlangıç kodu oluşturma

`add-utterances.html` oluşturup aşağıdaki kodu ekleyin:

   [!code-html[Html code](~/samples-luis/documentation-samples/quickstarts/change-model/javascript/add-utterance.html "Javascript code")]

## <a name="run-code"></a>Kodu çalıştırma

1. Dosyayı herhangi bir tarayıcıda açın.

2. LUIS yazma kimliğinizi ve LUIS uygulama kimliğinizi ekleyin.

3. Uygulamanıza eklenecek **konuşma dizisini** değiştirin. Bunlar utteranceJSON değişkeninde depolanır. Bu değişkeni kendi alanınıza ve konuşma ihtiyaçlarınıza göre değiştirin. 

    ```json
    // example batch utterances
    var utteranceJSON = [
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
        }
    ,
        {
            "text": "book a flight",
            "intentName": "BookFlight",
            "entityLabels": []
        }
    ];
    ```

4. `Upload utterance` düğmesini seçin. LUIS sonuçları düğmelerin altında görüntülenir.

5. Uygulamanızı bu yeni konuşmalarla eğitmek için `Train model` düğmesini seçin.

6. Eğitim durumunu görmek için `Train Status` düğmesini seçin. 

    ![Add-utterances.html](./media/luis-quickstart-javascript-add-utterance/add-utterance.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Hızlı başlangıçla işiniz bittiğinde, bu hızlı başlangıçta oluşturulan tüm dosyaları kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [LUIS’i bir bot ile tümleştirme](luis-csharp-tutorial-build-bot-framework-sample.md)
