---
title: 'Hızlı Başlangıç: Dijital Mürekkep mürekkep tanıyıcı REST API ve Java ile tanıması'
description: Dijital mürekkep vuruşlarını algılamayı başlatmak için mürekkep tanıyıcı API'sini kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: aahi
ms.openlocfilehash: 04f2ac17871bbaf0506fe18122507167b23869a7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67060945"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-java"></a>Hızlı Başlangıç: Dijital Mürekkep mürekkep tanıyıcı REST API ve Java ile tanıması

Dijital mürekkep vuruşlarını üzerinde mürekkep tanıyıcı API'sini kullanmaya başlamak için bu Hızlı Başlangıç'ı kullanın. Bu Java uygulaması JSON biçimli bir mürekkep vuruşu verilerini içeren bir API isteği gönderir ve yanıtı alır.

Bu uygulama, Java dilinde yazılır, ancak çoğu programlama dilleri ile uyumlu bir RESTful web hizmeti API'dir.

Genellikle bir dijital mürekkep uygulamasından API çağrısıyla sonlandırmalısınız. Bu hızlı başlangıçta, bir JSON dosyasından el yazısı aşağıdaki örnek mürekkep vuruşu verileri gönderir.

![Resimlerdeki el yazısı görüntüsü](../media/handwriting-sample.jpg)

Bu Hızlı Başlangıç için kaynak kodu bulunabilir [GitHub](https://go.microsoft.com/fwlink/?linkid=2089904).

## <a name="prerequisites"></a>Önkoşullar

- [Java&trade; geliştirme Kit(JDK) 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya üzeri.

- Bu kitaplıklar Maven deposundan içeri aktarma
    - [Java'da JSON](https://mvnrepository.com/artifact/org.json/json) paket
    - [Apache HttpClient](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) paket

- Bu hızlı başlangıç örnek mürekkep vuruşu verilerini bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/InkRecognition/quickstart/example-ink-strokes.json).

[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]

## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.

    ```java
    import org.apache.http.HttpEntity;
    import org.apache.http.client.methods.CloseableHttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.StringEntity;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClients;
    import org.apache.http.util.EntityUtils;
    import java.io.IOException;
    import java.nio.file.Files;
    import java.nio.file.Paths;
    ```

2. Abonelik anahtarınız ve uç noktanız için değişkenler oluşturun. Mürekkep tanıma için kullanabileceğiniz URI aşağıdadır. Daha sonra API istek URL'si oluşturmak için hizmet uç noktanıza eklenir.

    ```java
    // Replace the subscriptionKey string value with your valid subscription key.
    static final String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
    // Replace the dataPath string with a path to the JSON formatted ink stroke data file.
    static final String dataPath = "PATH_TO_INK_STROKE_DATA";
    
    static final String endpoint = "https://api.cognitive.microsoft.com";
    static final String inkRecognitionUrl = "/inkrecognizer/v1.0-preview/recognize";
    ```

## <a name="create-a-function-to-send-requests"></a>İstekleri göndermek için bir işlev oluşturma

1. Adlı yeni bir işlev oluşturma `sendRequest()` yukarıda oluşturulan değişkenleri alır. Ardından aşağıdaki adımları gerçekleştirin.

2. Oluşturma bir `CloseableHttpClient` nesnesini API için istek gönderebilirsiniz. İstek gönderin bir `HttpPut` uç noktanızı ve mürekkep tanıyıcı URL birleştirerek istek nesnesi.

3. İsteğin kullanın `setHeader()` ayarlamak için işlevi `Content-Type` başlığına `application/json`, abonelik anahtarınızı ekleyin `Ocp-Apim-Subscription-Key` başlığı.

4. İsteğin kullanın `setEntity()` işleve gönderilecek veri.   

5. İstemcinin kullanmak `execute()` isteği göndermek için işlev ve kaydetmesi bir `CloseableHttpResponse` nesne. 

6. Oluşturma bir `HttpEntity` yanıt içeriği depolamak için nesne. İçeriği ile `getEntity()`. Yanıt boş değilse, onu döndürür.
    
    ```java
    static String sendRequest(String apiAddress, String endpoint, String subscriptionKey, String requestData) {
        try (CloseableHttpClient client = HttpClients.createDefault()) {
            HttpPut request = new HttpPut(endpoint + apiAddress);
            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            request.setEntity(new StringEntity(requestData));
            try (CloseableHttpResponse response = client.execute(request)) {
                HttpEntity respEntity = response.getEntity();
                if (respEntity != null) {
                    return EntityUtils.toString(respEntity, "utf-8");
                }
            } catch (Exception respEx) {
                respEx.printStackTrace();
            }
        } catch (IOException ex) {
            System.err.println("Exception recognizing ink: " + ex.getMessage());
            ex.printStackTrace();
        }
        return null;
    }
    ```

## <a name="send-an-ink-recognition-request"></a>Bir mürekkep tanıma İsteği Gönder

Adlı bir yöntem oluşturma `recognizeInk()` mürekkep vuruşu verilerinizi tanımak için. Çağrı `sendRequest()` , uç noktası, url, abonelik anahtarı ve json verilerini yukarıda oluşturulan yöntemi. Sonucu alın ve konsola yazdırır.

```java
static void recognizeInk(String requestData) {
    System.out.println("Sending an ink recognition request");
    String result = sendRequest(inkRecognitionUrl, endpoint, subscriptionKey, requestData);
    System.out.println(result);
}
```

## <a name="load-your-digital-ink-data-and-send-the-request"></a>Dijital Mürekkep verilerinizi yüklemek ve isteği gönder

1. Uygulamanızın ana yöntemde isteklerine eklenen veriler içeren JSON dosyasındaki okuyun.

2. Yukarıda oluşturulan mürekkep tanıma işlevi çağırın.
    
    ```java
    public static void main(String[] args) throws Exception {
        String requestData = new String(Files.readAllBytes(Paths.get(dataPath)), "UTF-8");
        recognizeInk(requestData);
    }
    ```

## <a name="run-the-application-and-view-the-response"></a>Uygulamayı çalıştırmak ve yanıtı görüntüleyin

Uygulamayı çalıştırın. Başarılı bir yanıt JSON biçiminde döndürülür. Üzerinde JSON yanıt bulabilirsiniz [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/InkRecognition/quickstart/example-response.json).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://go.microsoft.com/fwlink/?linkid=2089907)


Mürekkep tanıma API'si dijital mürekkep bir uygulamada nasıl çalıştığını görmek için Github'da aşağıdaki örnek uygulamaları göz atın:
* [C# Evrensel Windows Platformu (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [JavaScript web tarayıcı uygulaması](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Java ve Android mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Swift ve iOS mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089805)
