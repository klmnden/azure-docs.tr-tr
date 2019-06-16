---
title: 'Hızlı Başlangıç: Dijital Mürekkep mürekkep tanıyıcı REST API ve Node.js ile tanıması'
description: Dijital mürekkep vuruşlarını algılamayı başlatmak için mürekkep tanıyıcı API'sini kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: aahi
ms.openlocfilehash: 0dc672f0efc420ab73fd923191c2bd52fb571a4f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056838"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-javascript"></a>Hızlı Başlangıç: Dijital Mürekkep mürekkep tanıyıcı REST API ve JavaScript ile tanıması

Dijital mürekkep vuruşlarını üzerinde mürekkep tanıyıcı API'sini kullanmaya başlamak için bu Hızlı Başlangıç'ı kullanın. Bu JavaScript uygulama JSON biçimli bir mürekkep vuruşu verilerini içeren bir API isteği gönderir ve yanıtını görüntüler.

Bu uygulama JavaScript'te yazılmış ve web tarayıcısında çalışır ancak çoğu programlama dilleri ile uyumlu bir RESTful web hizmeti API'dir.

Genellikle bir dijital mürekkep uygulamasından API çağrısıyla sonlandırmalısınız. Bu hızlı başlangıçta, bir JSON dosyasından el yazısı aşağıdaki örnek mürekkep vuruşu verileri gönderir.

![Resimlerdeki el yazısı görüntüsü](../media/handwriting-sample.jpg)

Bu Hızlı Başlangıç için kaynak kodu bulunabilir [GitHub](https://go.microsoft.com/fwlink/?linkid=2089905).

## <a name="prerequisites"></a>Önkoşullar

- Bir web tarayıcısı
- Bu hızlı başlangıç örnek mürekkep vuruşu verilerini bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/javascript/InkRecognition/quickstart/example-ink-strokes.json).


[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]

## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Sık kullandığınız IDE veya Düzenleyici içinde yeni bir oluşturma `.html` dosya. Ardından temel HTML daha sonra ekleyeceğiz kodunu ekleyin.
    
    ```html
    <!DOCTYPE html>
    <html>
    
        <head>
            <script type="text/javascript">
            </script>
        </head>
        
        <body>
        </body>
    
    </html>
    ```

2. İçinde `<body>` etiketinde, aşağıdaki HTML'yi ekleyin:
    1. JSON istek ve yanıt görüntülemek için iki metin alanları.
    2. Arama düğmesi `recognizeInk()` daha sonra oluşturulacak işlevi.
    
    ```HTML
    <!-- <body>-->
        <h2>Send a request to the Ink Recognition API</h2>
        <p>Request:</p>
        <textarea id="request" style="width:800px;height:300px"></textarea>
        <p>Response:</p>
        <textarea id="response" style="width:800px;height:300px"></textarea>
        <br>
        <button type="button" onclick="recognizeInk()">Recognize Ink</button>
    <!--</body>-->
    ```

## <a name="load-the-example-json-data"></a>Örnek JSON veri yükleme

1. İçinde `<script>` etiketinde, sampleJson için bir değişken oluşturun. Adlı bir JavaScript işlevi oluşturup `openFile()` JSON dosyanızı seçebilmeniz için bir dosya Gezgini açılır. Zaman `Recognize ink` düğmesine tıklandığında, dosya okunurken başlamak ve bu işlevi çağırın.
2. Kullanım bir `FileReader` nesnenin `onload()` dosyası zaman uyumsuz olarak işlemek için işlevi. 
    1. Herhangi bir yerine `\n` veya `\r` karakter boş bir dize içeren dosya. 
    2. Kullanım `JSON.parse()` için geçerli bir JSON metni dönüştürmek için
    3. Güncelleştirme `request` metin kutusuna uygulama. Kullanım `JSON.stringify()` JSON dizesi biçimlendirmek için. 
    
    ```javascript
    var sampleJson = "";
    function openFile(event) {
        var input = event.target;
    
        var reader = new FileReader();
        reader.onload = function(){
            sampleJson = reader.result.replace(/(\\r\\n|\\n|\\r)/gm, "");
            sampleJson = JSON.parse(sampleJson);
            document.getElementById('request').innerHTML = JSON.stringify(sampleJson, null, 2);
        };
        reader.readAsText(input.files[0]);
    };
    ```

## <a name="send-a-request-to-the-ink-recognizer-api"></a>Mürekkep tanıyıcı API'sine bir istek gönderin

1. İçinde `<script>` etiketinde, çağrılan bir işlev oluşturma `recognizeInk()`. Bu işlev, daha sonra API'ye çağrı ve sayfa yanıtı güncelleştirin. Aşağıdaki adımlarda bu işlev içindeki kodu ekleyin. 
        
    ```javascript
    function recognizeInk() {
    // add the code from the below steps here 
    }
    ```

    1. Değişkenleri, uç nokta URL'nizi, abonelik anahtarı ve JSON örneği oluşturun. Oluşturup bir `XMLHttpRequest` API isteği göndermek için nesne. 
        
        ```javascript
        // Replace the below URL with the correct one for your subscription. 
        // Your endpoint can be found in the Azure portal. For example: https://westus2.api.cognitive.microsoft.com
        var SERVER_ADDRESS = "YOUR-SUBSCRIPTION-URL";
        var ENDPOINT_URL = SERVER_ADDRESS + "/inkrecognizer/v1.0-preview/recognize";
        // Replace the subscriptionKey string value with your valid subscription key.
        var SUBSCRIPTION_KEY = "YOUR-SUBSCRIPTION-KEY";
        var xhttp = new XMLHttpRequest();
        ```
    2. Dönüş işlevi oluşturma `XMLHttpRequest` nesne. Bu işlev başarılı bir istek API yanıtı ayrıştırılamıyor ve uygulamada görüntüler. 
            
        ```javascript
        function returnFunction(xhttp) {
            var response = JSON.parse(xhttp.responseText);
            console.log("Response: %s ", response);
            document.getElementById('response').innerHTML = JSON.stringify(response, null, 2);
        }
        ```
    3. İstek nesnesi hata işlevi oluşturun. Bu işlev, konsola hatayı günlüğe kaydeder. 
            
        ```javascript
        function errorFunction() {
            console.log("Error: %s, Detail: %s", xhttp.status, xhttp.responseText);
        }
        ```

    4. İstek nesnenin bir işlev oluşturma `onreadystatechange` özelliği. İstek nesnenin hazır olma durumu değiştiğinde, yukarıdaki döndürür ve hata işlevleri uygulanır.
            
        ```javascript
        xhttp.onreadystatechange = function () {
            if (this.readyState === 4) {
                if (this.status === 200) {
                    returnFunction(xhttp);
                } else {
                    errorFunction(xhttp);
                }
            }
        };
        ```
    
    5. API isteği gönderin. Abonelik anahtarınızı ekleme `Ocp-Apim-Subscription-Key` üst bilgi ve kümesi `content-type` için `application/json`
    
        ```javascript
        xhttp.open("PUT", ENDPOINT_URL, true);
        xhttp.setRequestHeader("Ocp-Apim-Subscription-Key", SUBSCRIPTION_KEY);
        xhttp.setRequestHeader("content-type", "application/json");
        xhttp.send(JSON.stringify(sampleJson));
        };

## Run the application and view the response

This application can be run within your web browser. A successful response is returned in JSON format. You can also find the JSON response on [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/javascript/InkRecognition/quickstart/example-response.json):

## Next steps

> [!div class="nextstepaction"]
> [REST API reference](https://go.microsoft.com/fwlink/?linkid=2089907)

To see how the Ink Recognition API works in a digital inking app, take a look at the following sample applications on GitHub:
* [C# and Universal Windows Platform(UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# and Windows Presentation Foundation(WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [Javascript web-browser app](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Java and Android mobile app](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Swift and iOS mobile app](https://go.microsoft.com/fwlink/?linkid=2089805)
