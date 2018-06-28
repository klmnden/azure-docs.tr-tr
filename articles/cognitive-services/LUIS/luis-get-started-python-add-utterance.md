---
title: 'Öğretici: Python kullanarak bir LUIS uygulamasına konuşma ekleme | Microsoft Docs'
description: Bu öğreticide Python kullanarak bir LUIS uygulamasını çağırmayı öğreneceksiniz.
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 3e1ca2ea874b6b39ac8a1b1eaa94fe9ff1894ea3
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264310"
---
# <a name="tutorial-add-utterances-to-app-using-python"></a>Öğretici: Python kullanarak uygulamaya konuşma ekleme
Bu öğreticide amaca konuşma eklemek için Python'daki Authoring API'lerini kullanarak bir program yazacaksınız.

<!-- green checkmark -->
> [!div class="checklist"]
> * Visual Studio konsol projesi oluşturma 
> * Konuşma eklemek için LUIS API'sini çağırma yöntemi ekleme ve uygulamayı eğitme
> * BookFlight amacı için örnek konuşmaları içeren JSON dosyası ekleme
> * Konsolu çalıştırma ve konuşmalar için eğitim durumunu görme

Daha fazla bilgi için [amaca örnek konuşma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45), [eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) ve [eğitim durumu](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46) API'lerinin teknik belgelerine bakın.

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS][LUIS] hesabına ihtiyacınız olacak.

## <a name="prerequisites"></a>Ön koşullar

* [Python 3.6](https://www.python.org/downloads/) veya üzeri.
* **[Önerilen]** IntelliSense ve hata ayıklama için [Visual Studio Code](https://code.visualstudio.com/).
* LUIS **[yazma anahtarınız](luis-concept-keys.md#authoring-key)**. Bu anahtarı [LUIS](luis-reference-regions.md) web sitesinin Account Settings (Hesap Ayarları) bölümünde bulabilirsiniz.
* Mevcut LUIS [**uygulama kimliğiniz**](./luis-get-started-create-app.md). Uygulama kimliği, uygulama panosunda gösterilir. `utterances.json` dosyasında kullanılan amaçlara ve varlıklara sahip LUIS uygulamasının, kod `add-utterances.js` içinde çalıştırılmadan önce mevcut olması gerekir. Bu makaledeki kod, amaçları ve varlıkları oluşturmaz. Yalnızca konuşmaları var olan amaçlara ve varlıklara ekler. 
* Konuşmaları alan uygulamanın içindeki **sürüm kimliği**. Varsayılan kimlik: "0.1"
* VSCode içinde `add-utterances-3-6.py` projesi adlı yeni bir dosya oluşturun.

> [!NOTE] 
> `add-utterances-3-6.py` dosyasının tamamı [**LUIS-Samples** Github deposunda](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/authoring-api-samples/python) mevcuttur.


## <a name="write-the-python-code"></a>Python kodunu yazma

1. Aşağıdaki kod parçacıklarını kopyalayın:

   [!code-python[Console app code that adds an utterance Python 3.6](~/samples-luis/documentation-samples/authoring-api-samples/python/add-utterances-3-6.py)]

## <a name="specify-utterances-to-add"></a>Eklenecek konuşmaları belirtme
LUIS uygulamasına eklemek istediğiniz **konuşma dizisini** belirtmek için `utterances.json` dosyasını oluşturup düzenleyin. Amacın ve varlıkların LUIS uygulamasında mevcut olması **gerekir**.

> [!NOTE]
> `utterances.json` dosyasında kullanılan amaçlara ve varlıklara sahip LUIS uygulamasının, kod `add-utterances.js` içinde çalıştırılmadan önce mevcut olması gerekir. Bu makaledeki kod, amaçları ve varlıkları oluşturmaz. Yalnızca konuşmaları var olan amaçlara ve varlıklara ekler.

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

Uygulamayı Python 3.6 ile komut satırından çalıştırın.

Bağımsız değişken olmadan add-utterance çağrısını yaptığınızda uygulamaya konuşma eklenir ancak eğitim gerçekleştirilmez.

````
> python add-utterances-3-6.py
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
> python add-utterances-3-6.py -train
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
> python add-utterances-3-6.py -status
````
## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu öğreticiyi tamamladıktan sonra ihtiyacınız yoksa Visual Studio'yu ve konsol uygulamasını kaldırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Programlama yoluyla bir LUIS uygulaması oluşturma](luis-tutorial-node-import-utterances-csv.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website

