---
title: "Hızlı Başlangıç: Bing otomatik öneri REST API'si ve Java ile arama sorguları önerin"
titlesuffix: Azure Cognitive Services
description: Hızlı arama terimlerini önerme başlatmayı öğrenin Bing otomatik öneri API'si ile gerçek zamanlı.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: quickstart
ms.date: 02/20/2019
ms.author: aahi
ms.openlocfilehash: 64b6ed680ba0812322d5796debc5edada19bc926
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60547413"
---
# <a name="quickstart-suggest-search-queries-with-the-bing-autosuggest-rest-api-and-java"></a>Hızlı Başlangıç: Bing otomatik öneri REST API'si ve Java ile arama sorguları önerin


Bing otomatik öneri API'si ve JSON yanıtını alma yapmaya başlamak için bu Hızlı Başlangıç'ı çağırır kullanın. Bu basit bir Java uygulaması, API için bir kısmi arama sorgusu gönderir ve aramalar için öneriler döndürür. Bu uygulama Java ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir. Bu örnek için kaynak kodu kullanılabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/Search/BingAutosuggestv7.java)

## <a name="prerequisites"></a>Önkoşullar

* [Java geliştirme Kit(JDK)](https://www.oracle.com/technetwork/java/javase/downloads/)
* [Gson kitaplığı](https://github.com/google/gson)

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-autosuggest-signup-requirements.md)]

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
    ```

2. Abonelik anahtarınız, API konak ve yol değişkenlerinin, [pazara kod](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference#market-codes)ve bir arama sorgusu.
    
    ```java
    static String subscriptionKey = "enter key here";
    static String host = "https://api.cognitive.microsoft.com";
    static String path = "/bing/v7.0/Suggestions";
    static String mkt = "en-US";
    static String query = "sail";
    ```


## <a name="format-the-response"></a>Yanıtı biçimlendirme

Adlı bir yöntem oluşturma `prettify()` Bing Video API'den döndürülen yanıtı biçimlendirmek için. Gson kitaplığın kullanın `JsonParser` bir JSON dizesinde alıp bir nesnesine dönüştürmek için. Ardından `GsonBuilder()` ve `toJson()` biçimlendirilmiş dize oluşturmak için.

```java
// pretty-printer for JSON; uses GSON parser to parse and re-serialize
public static String prettify(String json_text) {
    JsonParser parser = new JsonParser();
    JsonObject json = parser.parse(json_text).getAsJsonObject();
    Gson gson = new GsonBuilder().setPrettyPrinting().create();
    return gson.toJson(json);
}
```

## <a name="construct-and-send-the-search-request"></a>Oluşturun ve arama isteği gönder

1. Adlı yeni bir yöntem oluşturma `get_suggestions()` ve aşağıdaki adımları gerçekleştirin:

   1. İsteğiniz URL'sini, API'nizi birleştirerek oluşturmak konak, yol ve arama sorgunuz kodlama. URL unutmayın-eklenmesinden önce sorgu kodlayın. Pazar koda ekleyerek parametreleri dize sorgunuz için oluşturma `mkt=` parametresi ve sorgunuzu `q=` parametresi.
    
      ```java
  
      public static String get_suggestions () throws Exception {
         String encoded_query = URLEncoder.encode (query, "UTF-8");
         String params = "?mkt=" + mkt + "&q=" + encoded_query;
         //...
      }
      ```
    
   2. Yeni bir istek URL'si, API ana bilgisayar, yol ve yukarıda oluşturulan parametreleri ile oluşturun. 
    
       ```java
       //...
       URL url = new URL (host + path + params);
       //...
       ```
    
   3. Oluşturma bir `HttpsURLConnection` nesne ve kullanmak `openConnection()` bir bağlantı oluşturmak için. İstek yöntemini ayarla `GET`, abonelik anahtarınızı ekleyin `Ocp-Apim-Subscription-Key` başlığı.

      ```java
       //...
       HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
       connection.setRequestMethod("GET");
       connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);
       connection.setDoOutput(true);
       //...
      ```

   4. API yanıtı okunan bir `StringBuilder`. Yanıt yakalandıktan sonra kapatmak `InputStreamReader` akışla aktarma ve yanıtı döndürür.

       ```java
       //...
       StringBuilder response = new StringBuilder ();
       BufferedReader in = new BufferedReader(
       new InputStreamReader(connection.getInputStream()));
       String line;
       while ((line = in.readLine()) != null) {
         response.append(line);
       }
       in.close();
    
       return response.toString();
       ```

2. Uygulamanızın ana işlev çağrısında `get_suggestions()`ve yanıt kullanarak yazdırma `prettify()`.
    
    ```java
    public static void main(String[] args) {
      try {
        String response = get_suggestions ();
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
  "_type": "Suggestions",
  "queryContext": {
    "originalQuery": "sail"
  },
  "suggestionGroups": [
    {
      "name": "Web",
      "searchSuggestions": [
        {
          "url": "https://www.bing.com/cr?IG\u003d2ACC4FE8B02F4AACB9182A6502B0E556\u0026CID\u003d1D546424A4CB64AF2D386F26A5CD6583\u0026rd\u003d1\u0026h\u003dgvtP9TS9NwhajSapY2Se6y1eCbP2fq_GiP2n-cxi6OY\u0026v\u003d1\u0026r\u003dhttps%3a%2f%2fwww.bing.com%2fsearch%3fq%3dsailrite%26FORM%3dUSBAPI\u0026p\u003dDevEx,5003.1",
          "displayText": "sailrite",
          "query": "sailrite",
          "searchKind": "WebSearch"
        },
        {
          "url": "https://www.bing.com/cr?IG\u003d2ACC4FE8B02F4AACB9182A6502B0E556\u0026CID\u003d1D546424A4CB64AF2D386F26A5CD6583\u0026rd\u003d1\u0026h\u003dBTS0G6AakxntIl9rmbDXtk1n6rQpsZZ99aQ7ClE7dTY\u0026v\u003d1\u0026r\u003dhttps%3a%2f%2fwww.bing.com%2fsearch%3fq%3dsail%2bsand%2bpoint%26FORM%3dUSBAPI\u0026p\u003dDevEx,5004.1",
          "displayText": "sail sand point",
          "query": "sail sand point",
          "searchKind": "WebSearch"
        },
        {
          "url": "https://www.bing.com/cr?IG\u003d2ACC4FE8B02F4AACB9182A6502B0E556\u0026CID\u003d1D546424A4CB64AF2D386F26A5CD6583\u0026rd\u003d1\u0026h\u003dc0QOA_j6swCZJy9FxqOwke2KslJE7ZRmMooGClAuCpY\u0026v\u003d1\u0026r\u003dhttps%3a%2f%2fwww.bing.com%2fsearch%3fq%3dsailboats%2bfor%2bsale%26FORM%3dUSBAPI\u0026p\u003dDevEx,5005.1",
          "displayText": "sailboats for sale",
          "query": "sailboats for sale",
          "searchKind": "WebSearch"
        },
        {
          "url": "https://www.bing.com/cr?IG\u003d2ACC4FE8B02F4AACB9182A6502B0E556\u0026CID\u003d1D546424A4CB64AF2D386F26A5CD6583\u0026rd\u003d1\u0026h\u003dmnMdREUH20SepmHQH1zlh9Hy_w7jpOlZFm3KG2R_BoA\u0026v\u003d1\u0026r\u003dhttps%3a%2f%2fwww.bing.com%2fsearch%3fq%3dsailing%2banarchy%26FORM%3dUSBAPI\u0026p\u003dDevEx,5006.1",
          "displayText": "sailing anarchy",
          "query": "sailing anarchy",
          "searchKind": "WebSearch"
        },
        {
          "url": "https://www.bing.com/cr?IG\u003d2ACC4FE8B02F4AACB9182A6502B0E556\u0026CID\u003d1D546424A4CB64AF2D386F26A5CD6583\u0026rd\u003d1\u0026h\u003dWLFO-B1GG5qtBGnoU1Bizz02YKkg5fgAQtHwhXn4z8I\u0026v\u003d1\u0026r\u003dhttps%3a%2f%2fwww.bing.com%2fsearch%3fq%3dsailpoint%26FORM%3dUSBAPI\u0026p\u003dDevEx,5007.1",
          "displayText": "sailpoint",
          "query": "sailpoint",
          "searchKind": "WebSearch"
        },
        {
          "url": "https://www.bing.com/cr?IG\u003d2ACC4FE8B02F4AACB9182A6502B0E556\u0026CID\u003d1D546424A4CB64AF2D386F26A5CD6583\u0026rd\u003d1\u0026h\u003dquBMwmKlGwqC5wAU0K7n416plhWcR8zQCi7r-Fw9Y0w\u0026v\u003d1\u0026r\u003dhttps%3a%2f%2fwww.bing.com%2fsearch%3fq%3dsailflow%26FORM%3dUSBAPI\u0026p\u003dDevEx,5008.1",
          "displayText": "sailflow",
          "query": "sailflow",
          "searchKind": "WebSearch"
        },
        {
          "url": "https://www.bing.com/cr?IG\u003d2ACC4FE8B02F4AACB9182A6502B0E556\u0026CID\u003d1D546424A4CB64AF2D386F26A5CD6583\u0026rd\u003d1\u0026h\u003d0udadFl0gCTKCp0QmzQTXS3_y08iO8FpwsoKPHPS6kw\u0026v\u003d1\u0026r\u003dhttps%3a%2f%2fwww.bing.com%2fsearch%3fq%3dsailboatdata%26FORM%3dUSBAPI\u0026p\u003dDevEx,5009.1",
          "displayText": "sailboatdata",
          "query": "sailboatdata",
          "searchKind": "WebSearch"
        },
        {
          "url": "https://www.bing.com/cr?IG\u003d2ACC4FE8B02F4AACB9182A6502B0E556\u0026CID\u003d1D546424A4CB64AF2D386F26A5CD6583\u0026rd\u003d1\u0026h\u003deSSt0MRSbl2V0RFPSuVd-gC7fGOT4717pz55EBUgPec\u0026v\u003d1\u0026r\u003dhttps%3a%2f%2fwww.bing.com%2fsearch%3fq%3dsailor%2b2025%26FORM%3dUSBAPI\u0026p\u003dDevEx,5010.1",
          "displayText": "sailor 2025",
          "query": "sailor 2025",
          "searchKind": "WebSearch"
        }
      ]
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../tutorials/autosuggest.md)

- [Bing Otomatik Öneri nedir?](../get-suggested-search-terms.md)
- [Bing Otomatik Öneri API’si v7 başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference)
