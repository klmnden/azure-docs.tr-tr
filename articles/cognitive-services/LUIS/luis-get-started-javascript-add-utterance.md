---
title: 'Öğretici: JavaScript kullanarak bir LUIS uygulamasına konuşma ekleme | Microsoft Docs'
description: Bu öğreticide JavaScript kullanarak bir LUIS uygulamasını çağırmayı öğreneceksiniz.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/18/2017
ms.author: v-geberr
ms.openlocfilehash: b6d021dcfdddb5449aa989c6aa06d7faf326befb
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265468"
---
# <a name="tutorial-add-utterances-to-app-using-javascript"></a>Öğretici: JavaScript kullanarak uygulamaya konuşma ekleme
Bu öğreticide amaca konuşma eklemek için JavaScript içindeki Authoring API'lerini kullanarak bir program yazacaksınız.

<!-- green checkmark -->
> [!div class="checklist"]
> * Visual Studio konsol projesi oluşturma 
> * Konuşma eklemek için LUIS API'sini çağırma yöntemi ekleme ve uygulamayı eğitme
> * BookFlight amacı için örnek konuşmaları içeren JSON dosyası ekleme
> * Konsolu çalıştırma ve konuşmalar için eğitim durumunu görme

Daha fazla bilgi için [amaca örnek konuşma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) ve [eğitim durumu](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46) API'lerinin teknik belgelerini inceleyin.

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS][LUIS] hesabına ihtiyacınız olacak.

## <a name="prerequisites"></a>Ön koşullar
* LUIS [**yazma anahtarınız**](luis-concept-keys.md#authoring-key). 
* Mevcut LUIS **uygulama kimliğiniz** ve **sürüm kimliğiniz**. 
* VSCode içinde `add-utterances.html` projesi adlı yeni bir dosya.

> [!NOTE] 
> `add-utterances.html` dosyasının tamamı [**LUIS-Samples** Github deposunda](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/authoring-api-samples/javascript/add-utterance.html) mevcuttur.


## <a name="write-the-code"></a>Kodu yazma
`add-utterances.html` oluşturup aşağıdaki kodu ekleyin:

   [!code-javascript[Java Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/javascript/add-utterance.html "Java Dependencies")]

## <a name="view-in-browser"></a>Tarayıcıda görüntüleme
1. Dosyayı herhangi bir tarayıcıda açın.

2. LUIS yazma kimliğinizi ve LUIS uygulama kimliğinizi ekleyip sürüm farklıysa `0.1` olarak değiştirin

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
Bu öğreticiyi tamamladıktan sonra ihtiyacınız yoksa Visual Studio'yu ve konsol uygulamasını kaldırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [LUIS’i bir bot ile tümleştirme](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website