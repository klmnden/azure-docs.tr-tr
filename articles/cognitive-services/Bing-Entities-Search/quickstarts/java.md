---
title: "Hızlı Başlangıç: Java kullanarak Bing varlık arama REST API'si için bir arama isteği gönder"
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Java kullanarak Bing varlık arama REST API'si için bir istek göndermek için kullanın ve bir JSON yanıtı alırsınız.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: a8b25252e861d707568876f75aadd6f436441f8f
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389429"
---
# <a name="quickstart-send-a-search-request-to-the-bing-entity-search-rest-api-using-java"></a>Hızlı Başlangıç: Java kullanarak Bing varlık arama REST API'si için bir arama isteği gönder

Bu hızlı başlangıçta, Bing varlık arama API'si, ilk çağrı yapmak ve JSON yanıtı görüntülemek için kullanın. Bu basit bir Java uygulaması API için bir haber arama sorgu gönderir ve yanıtını görüntüler.

Bu uygulama Java ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

* [Java geliştirme Kit(JDK)](https://www.oracle.com/technetwork/java/javase/downloads/)
* [Gson kitaplığı](https://github.com/google/gson)


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.

   ```java
   import java.io.*;
   import java.net.*;
   import java.util.*;
   import javax.net.ssl.HttpsURLConnection;
   import com.google.gson.Gson;
   import com.google.gson.GsonBuilder;
   import com.google.gson.JsonObject;
   import com.google.gson.JsonParser;
   import com.google.gson.Gson;
   import com.google.gson.GsonBuilder;
   import com.google.gson.JsonObject;
   import com.google.gson.JsonParser;
   ```

2. Yeni bir sınıf içinde API uç noktası, abonelik anahtarınız ve bir arama sorgusu için değişkenleri oluşturun.

   ```java
   public class EntitySearch {

      static String subscriptionKey = "ENTER KEY HERE";
    
        static String host = "https://api.cognitive.microsoft.com";
        static String path = "/bing/v7.0/entities";
    
        static String mkt = "en-US";
        static String query = "italian restaurant near me";
   //...
    
   ```

## <a name="construct-a-search-request-string"></a>Bir arama isteği dizesi oluşturun

1. Çağrılan bir işlev oluşturma `search()` bir JSON döndüren `String`. URL-arama sorgunuz kodlamak ve parametreleri dizesiyle eklemek `&q=`. Dizenin pazarınızın eklemek `?mkt=`.
 
2. Konak ve yol parametreleri dizelerinizle bir URL oluşturur.
    
    ```java
    //...
    public static String search () throws Exception {
        String encoded_query = URLEncoder.encode (query, "UTF-8");
        String params = "?mkt=" + mkt + "&q=" + encoded_query;
        URL url = new URL (host + path + params);
    //...
    ```
      
## <a name="send-a-search-request-and-receive-a-response"></a>Arama isteği gönderme ve yanıt

1. İçinde `search()` yukarıda işlevi oluşturdunuz, yeni bir `HttpsURLConnection` nesnesi ile `url.openCOnnection()`. İstek yöntemini ayarla `GET`, abonelik anahtarınızı ekleyin `Ocp-Apim-Subscription-Key` başlığı.

    ```java
    //...
    HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
    connection.setRequestMethod("GET");
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);
    connection.setDoOutput(true);
    //...
    ```

2. Yeni bir `StringBuilder`. Yeni bir `InputStreamReader` örneği oluşturulurken bir parametre olarak `BufferedReader` API yanıtı okumak için.  
    
    ```java
    //...
    StringBuilder response = new StringBuilder ();
    BufferedReader in = new BufferedReader(
        new InputStreamReader(connection.getInputStream()));
    //...
    ```

3. Oluşturma bir `String` yanıttan depolamak için nesne `BufferedReader`. Üzerinden yinelemek ve dizeyi her bir satır ekleyin. Ardından okuyucu kapatın ve yanıtı döndürür. 
    
    ```java
    String line;
    
    while ((line = in.readLine()) != null) {
      response.append(line);
    }
    in.close();
    
    return response.toString();
    ```

## <a name="format-the-json-response"></a>JSON yanıtı biçimlendirme

1. Adlı yeni bir işlev oluşturma `prettify` JSON yanıtı biçimlendirmek için. Yeni bir `JsonParser`ve çağrı `parse()` json metni ve bir JSON nesnesi olarak depolar. 

2. Yeni bir Gson kitaplığını kullanma `GsonBuilder()`ve `setPrettyPrinting().create()` json biçimine. Ardından onu döndürür.    
  
   ```java
   //...
   public static String prettify (String json_text) {
    JsonParser parser = new JsonParser();
    JsonObject json = parser.parse(json_text).getAsJsonObject();
    Gson gson = new GsonBuilder().setPrettyPrinting().create();
    return gson.toJson(json);
   }
   //...
   ```

## <a name="call-the-search-function"></a>Arama işlevini çağırın

1. Projenizin ana yönteminden çağrı `search()`ve `prettify()` metni biçimlendirmek için.
    
    ```java
        public static void main(String[] args) {
            try {
                String response = search ();
                System.out.println (prettify (response));
            }
            catch (Exception e) {
                System.out.println (e);
            }
        }
    ```

## <a name="example-json-response"></a>Örnek JSON yanıtı

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "https://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfa uygulaması oluşturma](../tutorial-bing-entities-search-single-page-app.md)

* [Bing varlık arama API'si nedir?](../overview.md )
* [Bing varlık arama API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference)
