---
title: 'Hızlı Başlangıç: Metinden dili tanımlama, Java - Translator Metin Çevirisi API’si'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Java ile Translator Metin Çevirisi API’sini kullanarak kaynak metnin dilini tanımlayacaksınız.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/21/2018
ms.author: erhopf
ms.openlocfilehash: dd08280b0a4a4608888cf262957c51499412612c
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647128"
---
# <a name="quickstart-identify-language-from-text-with-the-translator-text-rest-api-java"></a>Hızlı Başlangıç: Translator Metin Çevirisi REST API’si (Java) ile metinden dil tanımlama

Bu hızlı başlangıçta, Translator Metin Çevirisi API'sini kullanarak kaynak metnin dilini tanımlayacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu derleyip çalıştırmak için [JDK 7 veya 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)’e ihtiyacınız olacak. Eğer varsa sık kullandığınız bir Java IDE’yi veya bir metin düzenleyicisini kullanabilirsiniz.

Translator Metin Çevirisi API'sini kullanmak için, ayrıca abonelik anahtarınızın olması gerekir; bkz. [Translator Metin Çevirisi API'sine kaydolma](translator-text-how-to-signup.md).

## <a name="detect-request"></a>Algılama isteği

Aşağıdaki kod, [Algılama](./reference/v3-0-detect.md) yöntemini kullanarak kaynak metnin dilini tanımlar.

1. Sık kullandığınız kod düzenleyicisinde yeni bir Java projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `subscriptionKey` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```java
import java.io.*;
import java.net.*;
import java.util.*;
import javax.net.ssl.HttpsURLConnection;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.1
 */
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonElement;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

/* NOTE: To compile and run this code:
1. Save this file as Detect.java.
2. Run:
    javac Detect.java -cp .;gson-2.8.1.jar -encoding UTF-8
3. Run:
    java -cp .;gson-2.8.1.jar Detect
*/

public class Detect {

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
    static String subscriptionKey = "ENTER KEY HERE";

    static String host = "https://api.cognitive.microsofttranslator.com";
    static String path = "/detect?api-version=3.0";

    static String text = "Salve, mondo!";

    public static class RequestBody {
        String Text;

        public RequestBody(String text) {
            this.Text = text;
        }
    }

    public static String Post (URL url, String content) throws Exception {
        HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
        connection.setRequestMethod("POST");
        connection.setRequestProperty("Content-Type", "application/json");
        connection.setRequestProperty("Content-Length", content.length() + "");
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);
        connection.setRequestProperty("X-ClientTraceId", java.util.UUID.randomUUID().toString());
        connection.setDoOutput(true);

        DataOutputStream wr = new DataOutputStream(connection.getOutputStream());
        byte[] encoded_content = content.getBytes("UTF-8");
        wr.write(encoded_content, 0, encoded_content.length);
        wr.flush();
        wr.close();

        StringBuilder response = new StringBuilder ();
        BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream(), "UTF-8"));
        String line;
        while ((line = in.readLine()) != null) {
            response.append(line);
        }
        in.close();

        return response.toString();
    }

    public static String Detect () throws Exception {
        URL url = new URL (host + path);

        List<RequestBody> objList = new ArrayList<RequestBody>();
        objList.add(new RequestBody(text));
        String content = new Gson().toJson(objList);

        return Post(url, content);
    }

    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main(String[] args) {
        try {
            String response = Detect ();
            System.out.println (prettify (response));
        }
        catch (Exception e) {
            System.out.println (e);
        }
    }
}
```

## <a name="detect-response"></a>Algılama yanıtı

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
[
  {
    "language": "it",
    "score": 1.0,
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "alternatives": [
      {
        "language": "pt",
        "score": 1.0,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      },
      {
        "language": "en",
        "score": 1.0,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      }
    ]
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Çeviri ve başka alfabeye çevirme gibi bu ve diğer hızlı başlangıçlardaki örnek kodlarla birlikte GitHub’daki diğer örnek Translator Metin Çevirisi projelerini keşfedin.

> [!div class="nextstepaction"]
> [GitHub’da Java örneklerini keşfedin](https://aka.ms/TranslatorGitHub?type=&language=java)
