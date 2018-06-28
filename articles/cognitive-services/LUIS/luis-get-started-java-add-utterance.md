---
title: 'Öğretici: Java kullanarak bir LUIS uygulamasına konuşma ekleme | Microsoft Docs'
description: Bu öğreticide Java kullanarak bir LUIS uygulamasını çağırmayı öğreneceksiniz.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 5b9c7b90ca96b23fbabfeb1e1d06e4124e65a0dc
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266049"
---
# <a name="tutorial-add-utterances-to-app-using-java"></a>Öğretici: Java kullanarak uygulamaya konuşma ekleme 
Bu öğreticide amaca konuşma eklemek için Java içindeki Authoring API'lerini kullanarak bir program yazacaksınız.

<!-- green checkmark -->
> [!div class="checklist"]
> * Visual Studio konsol projesi oluşturma 
> * Konuşma eklemek için LUIS API'sini çağırma yöntemi ekleme ve uygulamayı eğitme
> * BookFlight amacı için örnek konuşmaları içeren JSON dosyası ekleme
> * Konsolu çalıştırma ve konuşmalar için eğitim durumunu görme

Daha fazla bilgi için [amaca örnek konuşma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) ve [eğitim durumu](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46) API'lerinin teknik belgelerini inceleyin.

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS][LUIS] hesabına ihtiyacınız olacak.

## <a name="prerequisites"></a>Ön koşullar

* En son Oracle [**Java/JDK**](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Google GSON JSON kitaplığı](https://github.com/google/gson).
* LUIS **[yazma anahtarınız](luis-concept-keys.md#authoring-key)**. Bu anahtarı [LUIS](luis-reference-regions.md) web sitesinin Account Settings (Hesap Ayarları) bölümünde bulabilirsiniz.
* Mevcut LUIS [**uygulama kimliğiniz**](./luis-get-started-create-app.md). Uygulama kimliği, uygulama panosunda gösterilir. `utterances.json` dosyasında kullanılan amaçlara ve varlıklara sahip LUIS uygulamasının, kod `AddUtterances.java` içinde çalıştırılmadan önce mevcut olması gerekir. Bu makaledeki kod, amaçları ve varlıkları oluşturmayacak. Yalnızca konuşmaları var olan amaçlara ve varlıklara ekleyecek. 
* Konuşmaları alan uygulamanın içindeki **sürüm kimliği**. Varsayılan kimlik: "0.1"
* `AddUtterances.java` adlı yeni bir metin dosyası oluşturun.

> [!NOTE] 
> `add-utterances.cs` dosyasının tamamı ve örnek bir `utterances.json` dosyası [**LUIS-Samples** Github deposunda](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/authoring-api-samples/java) mevcuttur.


## <a name="write-the-java-code"></a>Java kodunu yazma

Java bağımlılıklarını dosyaya ekleyin.

   [!code-java[Java Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=31-34 "Java Dependencies")]

`AddUtterances` sınıfını oluşturma

```Java
public class AddUtterances {
    // Insert code here
}
```

Bu sınıf sonraki tüm kod parçacıklarını içerir.

LUIS sabitlerini sınıfa ekleyin. Aşağıdaki kodu kopyalayıp yazma anahtarı, uygulama kimliği ve sürüm kimliğini kendi değerlerinizle değiştirin.

   [!code-java[LUIS-based IDs](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=41-53 "LUIS-based IDs")]

LUIS API'sine çağrılacak metodu seçin. 

   [!code-java[HTTP request to LUIS](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=55-178 "HTTP request to LUIS")]

LUIS API'sinden HTTP yanıtı için kullanılacak metodu ekleyin.

   [!code-java[HTTP response from LUIS](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=180-226 "HTTP response from LUIS")]


Özel durum işlemeyi ekleyin. 

   [!code-java[Exception Handling](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=228-256 "Exception Handling")]

Çıktı/yazdırma işlemeyi ekleyin.

   [!code-java[Add output handling](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=258-267 "Add configuration information for adding utterance")]

Ana işlevi ekleyin.

   [!code-java[Add main function](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=269-345 "Add main function")]


## <a name="specify-utterances-to-add"></a>Eklenecek konuşmaları belirtme
LUIS uygulamasına eklemek istediğiniz **konuşma dizisini** belirtmek için `utterances.json` dosyasını oluşturup düzenleyin. Amacın ve varlıkların LUIS uygulamasında mevcut olması **gerekir**.

> [!NOTE]
> `utterances.json` dosyasında kullanılan amaçlara ve varlıklara sahip LUIS uygulamasının, kod `AddUtterances.java` içinde çalıştırılmadan önce mevcut olması gerekir. Bu makaledeki kod, amaçları ve varlıkları oluşturmayacak. Yalnızca konuşmaları var olan amaçlara ve varlıklara ekleyecek.

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

AddUtterance öğesini bağımlılıklarla derleme

```
> javac -classpath gson-2.8.2.jar AddUtterances.java
```

Bağımsız değişken olmadan `AddUtterance` çağrısını yaptığınızda uygulamaya LUIS konuşmaları eklenir ancak eğitim gerçekleştirilmez.
````
> java -classpath .;gson-2.8.2.jar AddUtterances
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
Eğitim isteği göndermek için `add-utterance` öğesini `-train` bağımsız değişkeniyle birlikte çağırın. 

````
> java -classpath .;gson-2.8.2.jar AddUtterances -train
````

> [!NOTE]
> Yinelenen konuşmalar tekrar eklenmez ancak hataya da neden olmaz. `response`, özgün konuşmanın kimliğini içerir.

Uygulamayı `-train` bağımsız değişkeniyle çağırdığınızda LUIS uygulamasını eğitme isteğinin başarıyla sıraya alındığını belirten bir `training-results.json` dosyası oluşturulur. 

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
Eğitim durumunu kontrol etmek ve durumla ilgili ayrıntılı bilgileri dosyaya yazmak için uygulamayı `-status` bağımsız değişkeniyle çağırın.

````
> java -classpath .;gson-2.8.2.jar AddUtterances -status
````

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu öğreticiyi tamamladıktan sonra ihtiyacınız yoksa Visual Studio'yu ve konsol uygulamasını kaldırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Programlama yoluyla bir LUIS uygulaması oluşturma](luis-tutorial-node-import-utterances-csv.md) 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website