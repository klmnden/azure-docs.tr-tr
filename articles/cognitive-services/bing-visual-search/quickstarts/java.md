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
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 6a3eb9a7b85f3cd50384389ee8b03681c097dc27
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55859883"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-java"></a>Hızlı Başlangıç: Bing görsel arama REST API'si ve Java kullanarak görüntü Öngörüler elde edin

Bu hızlı başlangıçta, ilk Bing görsel arama API'sine çağrı yapmak ve arama sonuçlarını görüntülemek için kullanın. Bu basit C# uygulama API için bir görüntüyü karşıya yükler ve bu konuda döndürülen bilgileri görüntüler. Bu uygulama Java ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir.

Form verileri, yerel bir görüntüyü karşıya yüklenirken içerik düzeni üstbilgisini içermesi gerekir. `name` parametresi, "image" olarak, `filename` parametresi ise herhangi bir dize olarak ayarlanmalıdır. Formun içerikleri, görüntünün ikili verisidir. Karşıya yükleyebileceğiniz maksimum görüntü boyutu 1 MB’tır.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Önkoşullar

* [Java Development Kit (JDK) 7 veya 8](https://aka.ms/azure-jdks)
* [Gson kitaplığı](https://github.com/google/gson)
* [Apache HttpComponents](http://hc.apache.org/downloads.cgi)


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.

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

2. API uç noktanız abonelik anahtarı ve görüntü yolu için değişkenler oluşturun. 

    ```java
    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "your-key-here";
    static String imagePath = "path-to-your-image";
    ```

## <a name="create-the-json-parser"></a>JSON ayrıştırıcının oluşturma

Daha fazla API'sinden bir JSON yanıtı yapmak için bir yöntem oluşturma okunabilir kullanarak `JsonParser`.

    ```java
    public static String prettify(String json_text) {
            JsonParser parser = new JsonParser();
            JsonObject json = parser.parse(json_text).getAsJsonObject();
            Gson gson = new GsonBuilder().setPrettyPrinting().create();
            return gson.toJson(json);
        }
    ```

## <a name="construct-the-search-request-and-query"></a>Arama isteği ve sorgu oluşturma

1. Http kullanarak istemci uygulamanızın ana yöntemde oluşturma `HttpClientBuilder.create().build();`.

    ```java
    CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    ```

2. Oluşturma bir `HttpEntity` API'sine görüntünüzü karşıya yüklenecek.

    ```java
    HttpEntity entity = MultipartEntityBuilder
        .create()
        .addBinaryBody("image", new File(imagePath))
        .build();
    ```

3. Oluşturma bir `httpPost` nesne uç noktanız ile ve abonelik anahtarınızı kullanmayı üst bilgisini ayarlayın.

    ```java
    HttpPost httpPost = new HttpPost(endpoint);
    httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
    httpPost.setEntity(entity);
    ```

## <a name="receive-and-process-the-json-response"></a>JSON yanıtını alma ve işleme

1. Kullanım `HttpClient.execute()` API'sine bir istek gönderir ve yanıt olarak depolamak için bir `InputStream` nesne.
    
    ```java
    HttpResponse response = httpClient.execute(httpPost);
    InputStream stream = response.getEntity().getContent();
    ```

2. JSON dizesi Store ve yanıt yazdırın.

```java
String json = new Scanner(stream).useDelimiter("\\A").next();
System.out.println("\nJSON Response:\n");
System.out.println(prettify(json));
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir özel arama web uygulaması derleme](../tutorial-bing-visual-search-single-page-app.md)
