---
title: "Hızlı başlangıç: Bing Yazım Denetimi API'si, Java"
titlesuffix: Azure Cognitive Services
description: Bing Yazım Denetimi API'sini kısa sürede kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri alın.
services: cognitive-services
author: v-jaswel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: quickstart
ms.date: 09/14/2017
ms.author: v-jaswel
ms.openlocfilehash: 00e0b7db5bfc8b763d9b16524bd783601d1ec4d8
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801004"
---
# <a name="quickstart-for-bing-spell-check-api-with-java"></a>Hızlı başlangıç: Java ile Bing Yazım Denetimi API'si 

Bu makalede Java ile [Bing Yazım Denetimi API'si](https://azure.microsoft.com/services/cognitive-services/spell-check/) kullanma adımları gösterilmektedir. Yazım Denetimi API'si tanınmayan sözcüklere ek olarak değişiklik önerileri döndürür. Genellikle bu API'ye metin gönderip önerilen değişiklikleri metne uygular veya uygulamanızın kullanıcısına göstererek değişikliklerin yapılıp yapılmayacağına karar vermelerini sağlayabilirsiniz. Bu makalede "Hollo, wrld!" metnini içeren bir istek gönderme adımları gösterilmiştir. Önerilen değişiklikler "Hello" ve "world" olacaktır.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu derleyip çalıştırmak için [JDK 7 veya 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)’e ihtiyacınız olacak. Varsa, sık kullandığınız bir Java IDE’yi veya bir metin düzenleyicisini kullanabilirsiniz.

**Bing Yazım Denetimi API'si v7** sürümüne sahip bir [Bilişsel Hizmetler API hesabınız](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/#lang) bu hızlı başlangıç için yeterlidir. Ücretsiz denemenizi etkinleştirdiğinizde verilen erişim anahtarınız olması veya Azure panonuzdan ücretli bir abonelik anahtarı kullanmanız gerekir.

## <a name="get-spell-check-results"></a>Yazım Denetimi sonuçlarını alma

1. Sık kullandığınız IDE'de yeni bir Java projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `subscriptionKey` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```java
import java.io.*;
import java.net.*;
import javax.net.ssl.HttpsURLConnection;

public class HelloWorld {

    static String host = "https://api.cognitive.microsoft.com";
    static String path = "/bing/v7.0/spellcheck";

    // NOTE: Replace this example key with a valid subscription key.
    static String key = "ENTER KEY HERE";

    static String mkt = "en-US";
    static String mode = "proof";
    static String text = "Hollo, wrld!";

    public static void check () throws Exception {
        String params = "?mkt=" + mkt + "&mode=" + mode;
        URL url = new URL(host + path + params);
        HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
        connection.setRequestMethod("POST");
        connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
        connection.setRequestProperty("Content-Length", "" + text.length() + 5);
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", key);
        connection.setDoOutput(true);

        DataOutputStream wr = new DataOutputStream(connection.getOutputStream());
        wr.writeBytes("text=" + text);
        wr.flush();
        wr.close();

        BufferedReader in = new BufferedReader(
        new InputStreamReader(connection.getInputStream()));
        String line;
        while ((line = in.readLine()) != null) {
            System.out.println(line);
        }
        in.close();
    }

    public static void main(String[] args) {
        try {
            check ();
        }
        catch (Exception e) {
            System.out.println (e);
        }
    }
}
```

**Yanıt**

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
> [Bing Yazım Denetimi öğreticisi](../tutorials/spellcheck.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Bing Yazım Denetimi'ne genel bakış](../proof-text.md)
- [Bing Yazım Denetimi API’si v7 Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
