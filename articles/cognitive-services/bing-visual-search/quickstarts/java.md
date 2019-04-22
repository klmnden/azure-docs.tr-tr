---
title: "Hızlı Başlangıç: Bing görsel arama REST API'si ve Java kullanarak görüntü Öngörüler elde edin"
titleSuffix: Azure Cognitive Services
description: Bing görsel arama API'sine bir görüntüyü karşıya yükleme ve ilgili Öngörüler alma hakkında bilgi edinin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 4/02/2019
ms.author: scottwhi
ms.openlocfilehash: 2fe4e9dad0b198fe54e06ce07100d231f1f7d157
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59046453"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-java"></a>Hızlı Başlangıç: Bing görsel arama REST API'si ve Java kullanarak görüntü Öngörüler elde edin

Bu hızlı başlangıçta, ilk sonuçlarını görüntülemek ve Bing görsel arama API'sine çağrı yapmak için kullanın. Bu Java uygulaması, API için bir görüntüyü karşıya yükler ve döndürdüğü bilgileri görüntüler. Bu uygulama, Java dilinde yazılmış olsa, çoğu programlama dilleri ile uyumlu bir RESTful Web hizmeti API'dir.

Yerel bir görüntüyü karşıya yüklediğinizde, form verilerini içermelidir `Content-Disposition` başlığı. Ayarlamanız gerekir, `name` "image" ve parametre ayarlayabilirsiniz `filename` herhangi bir dize parametresi. Form içeriğini görüntünün ikili verileri içerir. Karşıya yüklediğiniz en yüksek görüntü boyutu 1 MB'dir.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Önkoşullar

* [Java Geliştirme Seti (JDK) 7 veya 8](https://aka.ms/azure-jdks)
* [Gson Java kitaplığı](https://github.com/google/gson)
* [Apache HttpComponents](https://hc.apache.org/downloads.cgi)

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Sık kullandığınız IDE veya düzenleyici yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarma:

    ```java
    import java.util.*;
    import java.io.*;
    import com.google.gson.Gson;
    import com.google.gson.GsonBuilder;
    import com.google.gson.JsonObject;
    import com.google.gson.JsonParser;
    
    // HttpClient libraries
    
    import org.apache.http.HttpEntity;
    import org.apache.http.HttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.ContentType;
    import org.apache.http.entity.mime.MultipartEntityBuilder;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClientBuilder;
    ```

2. API uç noktanız abonelik anahtarı ve görüntü yolu için değişkenler oluşturun:

    ```java
    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "your-key-here";
    static String imagePath = "path-to-your-image";
    ```

## <a name="create-the-json-parser"></a>JSON ayrıştırıcının oluşturma

Daha fazla API'sinden bir JSON yanıtı yapmak için bir yöntem oluşturma okunabilir kullanarak `JsonParser`:

    ```java
    public static String prettify(String json_text) {
            JsonParser parser = new JsonParser();
            JsonObject json = parser.parse(json_text).getAsJsonObject();
            Gson gson = new GsonBuilder().setPrettyPrinting().create();
            return gson.toJson(json);
        }
    ```

## <a name="construct-the-search-request-and-query"></a>Arama isteği ve sorgu oluşturma

1. Uygulamanızın ana yöntemi kullanarak bir HTTP istemci oluşturun `HttpClientBuilder.create().build();`:

    ```java
    CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    ```

2. Oluşturma bir `HttpEntity` nesne API'sine görüntünüzü karşıya yüklemek için:

    ```java
    HttpEntity entity = MultipartEntityBuilder
        .create()
        .addBinaryBody("image", new File(imagePath))
        .build();
    ```

3. Oluşturma bir `httpPost` nesne uç noktanız ile ve abonelik anahtarınızı kullanmayı üst bilgisini ayarlayın:

    ```java
    HttpPost httpPost = new HttpPost(endpoint);
    httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
    httpPost.setEntity(entity);
    ```

## <a name="receive-and-process-the-json-response"></a>JSON yanıtını alma ve işleme

1. Kullanım `HttpClient.execute()` API'sine bir istek gönderir ve yanıt olarak depolamak için yöntem bir `InputStream` nesnesi:
    
    ```java
    HttpResponse response = httpClient.execute(httpPost);
    InputStream stream = response.getEntity().getContent();
    ```

2. JSON dizesi Store ve yanıt yazdırın:

```java
String json = new Scanner(stream).useDelimiter("\\A").next();
System.out.println("\nJSON Response:\n");
System.out.println(prettify(json));
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir görsel arama tek sayfa web uygulaması derleme](../tutorial-bing-visual-search-single-page-app.md)
