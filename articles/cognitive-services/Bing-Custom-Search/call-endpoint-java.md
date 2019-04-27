---
title: 'Hızlı Başlangıç: Java kullanarak Bing özel arama uç noktanızı arayın | Microsoft Docs'
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Java Bing özel arama örneğinizin gelen arama sonuçlarını talep başlamak için kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: fc1c7d2730d68fb8e3b51a39e0cb89b4098418d6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60327386"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-java"></a>Hızlı Başlangıç: Java kullanarak Bing özel arama uç noktanızı arayın

Bing özel arama örneğinizin arama sonuçlarını talep başlamak için bu Hızlı Başlangıç'ı kullanın. Bu uygulama, Java dilinde yazılır, ancak Bing özel arama API'si bir RESTful web çoğu programlama dilleri ile uyumlu hizmetidir. Bu örneğin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/Search/BingCustomSearchv7.java)’da mevcuttur.

## <a name="prerequisites"></a>Önkoşullar

- Bing özel arama örneği için. Bkz: [hızlı başlangıç: İlk Bing özel arama örneğinizin oluşturma](quick-start.md) daha fazla bilgi için.

- En son [Java Geliştirme Seti](https://www.oracle.com/technetwork/java/javase/downloads/index.html)  

- [Gson kitaplığı](https://github.com/google/gson)

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.

    ```java
    import java.io.InputStream;
    import java.net.URL;
    import java.net.URLEncoder;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import java.util.Scanner;
    import javax.net.ssl.HttpsURLConnection;
    import com.google.gson.Gson;
    import com.google.gson.GsonBuilder;
    import com.google.gson.JsonObject;
    import com.google.gson.JsonParser;
    ```

2. adlı bir sınıf oluşturun `CustomSrchJava`ve abonelik anahtarınız, özel arama uç noktası ve yapılandırma özel arama örneğinizin kimliği için değişkenleri oluşturma 
    ```java
    public class CustomSrchJava {
        static String host = "https://api.cognitive.microsoft.com";
        static String path = "/bingcustomsearch/v7.0/search";
        static String subscriptionKey = "YOUR-SUBSCRIPTION-KEY"; 
        static String customConfigId = "YOUR-CUSTOM-CONFIG-ID";
        static String searchTerm = "Microsoft";  
    ...
    ```

3. Adlı başka bir sınıf oluşturun `SearchResults` Bing özel arama örneğinizin yanıttan içerecek.

    ```csharp
    class SearchResults{
        HashMap<String, String> relevantHeaders;
        String jsonResponse;
        SearchResults(HashMap<String, String> headers, String json) {
            relevantHeaders = headers;
            jsonResponse = json;
        }
    }
    ```

4. Adlı bir işlev oluşturma `prettify()` Bing özel arama API'si JSON yanıtı biçimlendirmek için.

    ```java
        // pretty-printer for JSON; uses GSON parser to parse and re-serialize
        public static String prettify(String json_text) {
            JsonParser parser = new JsonParser();
            JsonObject json = parser.parse(json_text).getAsJsonObject();
            Gson gson = new GsonBuilder().setPrettyPrinting().create();
            return gson.toJson(json);
        }
    ```

## <a name="send-and-receive-a-search-request"></a>Arama isteği gönderip 

1. Adlı bir işlev oluşturma `SearchWeb()` bir istek gönderir ve döndüren bir `SearchResults` nesne. İstek oluşturma özel yapılandırma kimliği, sorgu ve uç nokta bilgilerinizi birleştirerek URL'si. Abonelik anahtarınızı ekleme `Ocp-Apim-Subscription-Key` başlığı.

    ```java
    public class CustomSrchJava {
    ...
        public static SearchResults SearchWeb (String searchQuery) throws Exception {
            // construct the URL for your search request (endpoint + query string)
            URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchTerm, "UTF-8") + "&CustomConfig=" + customConfigId);
            HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
            connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);
    ...
    ```

2. Bir stream oluşturabilir ve JSON yanıt olarak depolamak bir `SearchResults` nesne.

    ```java
    public class CustomSrchJava {
    ...
        public static SearchResults SearchWeb (String searchQuery) throws Exception {
            ...
            // receive the JSON body
            InputStream stream = connection.getInputStream();
            String response = new Scanner(stream).useDelimiter("\\A").next();
        
            // construct result object for return
            SearchResults results = new SearchResults(new HashMap<String, String>(), response);
            
            stream.close();
            return results;
        }
    ```

3. Uygulamanızın ana yöntemi çağırın `SearchWeb()` arama teriminizi ile 

    ```java
    System.out.println("\nJSON Response:\n");
    System.out.println(prettify(result.jsonResponse));
    ```

4. Programı çalıştırın.
    
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir özel arama web uygulaması derleme](./tutorials/custom-search-web-page.md)
