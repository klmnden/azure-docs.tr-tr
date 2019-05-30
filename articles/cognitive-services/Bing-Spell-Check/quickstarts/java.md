---
title: "Hızlı Başlangıç: Bing yazım denetimi REST API'si ve Java ile yazım denetimi"
titlesuffix: Azure Cognitive Services
description: Bing yazım denetimi REST API'si yazım ve dilbilgisi denetimini kullanmaya başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 04/11/2019
ms.author: aahi
ms.openlocfilehash: 0d6ea62f1d2c1e15dff13aa45c4923717dd9c2c4
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66390189"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-java"></a>Hızlı Başlangıç: Bing yazım denetimi REST API'si ve Java ile yazım denetimi

Bu hızlı başlangıçta, Bing yazım denetimi REST API'si, ilk çağrı yapmak için kullanın. Bu basit bir Java uygulaması API'sine bir istek gönderir ve önerilen düzeltmeler listesini döndürür. Bu uygulama, Java dilinde yazılır, ancak çoğu programlama dilleri ile uyumlu bir RESTful web hizmeti API'dir. Bu uygulama için kaynak kodu kullanılabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/Search/BingSpellCheckv7.java).

## <a name="prerequisites"></a>Önkoşullar

* Java geliştirme Kit(JDK) 7 veya üzeri.

* İçeri aktarma [gson 2.8.5.jar](https://libraries.io/maven/com.google.code.gson%3Agson) veya en güncel [Gson](https://github.com/google/gson) sürümü. Komut satırı yürütme için ekleme `.jar` ana sınıfı içeren Java klasörünüze.

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]

## <a name="create-and-initialize-an-application"></a>Oluşturma ve bir uygulama başlatma

1. Sık kullandığınız IDE veya düzenleyici, seçtiğiniz bir sınıf adı ile yeni bir Java projesi oluşturun ve ardından aşağıdaki paketler aktarın.

    ```java
    import java.io.*;
    import java.net.*;
    import com.google.gson.*;
    import javax.net.ssl.HttpsURLConnection;
    ```

2. API uç noktanın ana bilgisayar, yol ve abonelik anahtarınız için değişkenler oluşturun. Ardından yazım denetimi yapmak istediğiniz metin pazarınızın değişkenleri ve yazım denetimi modu için bir dize oluşturur.

    ```java
    static String host = "https://api.cognitive.microsoft.com";
    static String path = "/bing/v7.0/spellcheck";

    static String key = "<ENTER-KEY-HERE>";

    static String mkt = "en-US";
    static String mode = "proof";
    static String text = "Hollo, wrld!";
    ```

## <a name="create-and-send-an-api-request"></a>Oluşturma ve bir API isteği gönder

1. Çağrılan bir işlev oluşturma `check()` oluşturmak ve API isteği göndermek için. İçine aşağıdaki adımları izleyin. İstek parametreleri için bir dize oluşturun. Append `?mkt=` Pazar dizenizi parametresi ve `&mode=` yazım denetimi modunuzu parametresi.  

   ```java
   public static void check () throws Exception {
       String params = "?mkt=" + mkt + "&mode=" + mode;
      // add the rest of the code snippets here (except prettify() and main())...
   }
   ```

2. Uç nokta konak ve yol parametreleri dize birleştirerek bir URL oluşturun. Yeni bir `HttpsURLConnection` nesne.

    ```java
    URL url = new URL(host + path + params);
    HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
    ```

3. URL bağlantısı açın. İstek yöntemini ayarla `POST`. İstek parametrelerinizi ekleyin. Abonelik anahtarınızı eklediğinizden emin olun `Ocp-Apim-Subscription-Key` başlığı.

    ```java
    connection.setRequestMethod("POST");
    connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", key);
    connection.setDoOutput(true);
    ```

4. Yeni bir `DataOutputStream` nesne ve API için istek gönderin.

    ```java
        DataOutputStream wr = new DataOutputStream(connection.getOutputStream());
        wr.writeBytes("text=" + text);
        wr.flush();
        wr.close();
    ```

## <a name="format-and-read-the-api-response"></a>Biçimlendirme ve API yanıtı okuyun

1. Bu yöntem sınıfınıza ekleyin. Bu, daha okunabilir bir çıkış için JSON biçimlendirir.

    ``` java
    // This function prettifies the json response.
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

1. Create a `BufferedReader` and read the response from the API. Print it to the console.
    
    ```java
    BufferedReader in = new BufferedReader(
    new InputStreamReader(connection.getInputStream()));
    String line;
    while ((line = in.readLine()) != null) {
        System.out.println(prettify(line);
    }
    in.close();
    ```

## <a name="call-the-api"></a>API çağırma

Yukarıda oluşturulan check() yönteminizi uygulamanızın ana işlevi çağırın.

    ```java
    public static void main(String[] args) {
        try {
            check();
        }
        catch (Exception e) {
            System.out.println (e);
        }
    }
    ```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Derleme ve projeyi çalıştırın.

Komut satırı kullanıyorsanız, oluşturmak ve uygulamayı çalıştırmak için aşağıdaki komutları kullanın.

**Derleme:**
```bash
javac -classpath .;gson-2.2.2.jar\* <CLASS_NAME>.java
```

**Çalıştırın:**
```bash
java -cp .;gson-2.2.2.jar\* <CLASS_NAME>
```

## <a name="example-json-response"></a>Örnek JSON yanıtı

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../tutorials/spellcheck.md)

- [Bing yazım denetimi API'si nedir?](../overview.md)
- [Bing Yazım Denetimi API’si v7 Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)
