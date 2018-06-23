---
title: Bing Visual arama API için Java hızlı başlangıç | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Bing Visual arama API görüntüyü karşıya yükleme ve görüntü ile ilgili Öngörüler ulaşırsınız gösterilmektedir.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 8160302faa373d69b65afe6b68a8efb44442850d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355133"
---
# <a name="your-first-bing-visual-search-query-in-java"></a>Java, ilk Bing Visual arama sorgusu

Bing Visual arama API sağlayan bir görüntü ile ilgili bilgileri döndürür. Belirteç, ya da görüntüyü karşıya yükleyerek bir Öngörüler resmin URL'sini kullanarak görüntü sağlayabilir. Bu seçenekler hakkında daha fazla bilgi için bkz: [Bing Visual arama API nedir?](../overview.md) Bu makalede, görüntüyü karşıya gösterilmektedir. Görüntüyü karşıya burada iyi bilinen bir yer işareti resmini alın ve ilgili bilgileri dönmek mobil senaryolarda yararlı olabilir. Örneğin, Öngörüler yer işareti hakkında trivia içerebilir. 

Yerel görüntü yüklerseniz, aşağıdaki form verilerini POST gövdesinde içermelidir gösterir. Form verileri içerik düzeni üstbilgisini eklemeniz gerekir. Kendi `name` parametresini ayarlayın, görüntü"için" ve `filename` parametresi için herhangi bir dize ayarlanmış olabilir. Form içeriğini ikili görüntünün olur. Karşıya yükleme en büyük görüntü boyutu 1 MB'tır. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Bu makalede bir Bing Visual arama API isteği gönderir ve JSON arama sonuçlarını görüntüleyen basit bir konsol uygulaması içerir. Bu uygulama, Java'da yazılmış olsa da, HTTP isteği yapmak ve JSON ayrıştırma programlama dili ile uyumlu bir RESTful Web hizmeti API'dir. 


## <a name="prerequisites"></a>Önkoşullar

İhtiyacınız olacak [JDK 7 veya 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) derlemek ve bu kodu çalıştırmak için. Sık kullanılan varsa, ancak bir metin düzenleyicisi yeterli bir Java IDE kullanabilir.

Bu Hızlı Başlangıç için kullandığınız bir [ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonelik anahtar veya Ücretli abonelik anahtarı.

## <a name="running-the-application"></a>Uygulamayı çalıştırma

Aşağıdaki Java MultipartEntityBuilder kullanarak görüntüyü karşıya nasıl yükleneceğini gösterir.

Bu uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

1. İndirme veya yükleme [gson Kitaplığı](https://github.com/google/gson). Ayrıca Maven alabilirsiniz.
2. Sık kullanılan IDE veya Düzenleyicisi yeni bir Java projesi oluşturun.
3. Sağlanan kod adındaki bir dosyada eklemek `VisualSearch.java`.
4. Değiştir `subscriptionKey` abonelik anahtarınızı değerle.
4. Değiştir `imagePath` karşıya yüklemek için resminin yolunu içeren değer.
5. Programını çalıştırın.


```java
package uploadimage;

import java.util.*;
import java.io.*;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.2
 *
 * Once you have compiled or downloaded gson-2.8.2.jar, assuming you have placed it in the
 * same folder as this file (BingImageSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac BingImageSearch.java -classpath .;gson-2.8.2.jar -encoding UTF-8
 * java -cp .;gson-2.8.2.jar BingImageSearch
 */

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

// http://hc.apache.org/downloads.cgi (HttpComponents Downloads) HttpClient 4.5.5

import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClientBuilder;


public class UploadImage2 {

    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "<yoursubscriptionkeygoeshere";
    static String imagePath = "<pathtoyourimagetouploadgoeshere>";

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        
        CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        
        try {
            HttpEntity entity = MultipartEntityBuilder
                .create()
                .addBinaryBody("image", new File(imagePath))
                .build();

            HttpPost httpPost = new HttpPost(endpoint);
            httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            httpPost.setEntity(entity);
            HttpResponse response = httpClient.execute(httpPost);

            InputStream stream = response.getEntity().getContent();
            String json = new Scanner(stream).useDelimiter("\\A").next();

            System.out.println("\nJSON Response:\n");
            System.out.println(prettify(json));
        }
        catch (IOException e)
        {
            e.printStackTrace(System.out);
            System.exit(1);
        }
        catch (Exception e) {
            e.printStackTrace(System.out);
            System.exit(1);
        }
    }
    
    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }
    
}
```

## <a name="next-steps"></a>Sonraki adımlar

[Öngörüler belirteci kullanarak bir görüntü ile ilgili Öngörüler alın](../use-insights-token.md)  
[Bing Visual arama tek sayfa uygulaması Öğreticisi](../tutorial-bing-visual-search-single-page-app.md)  
[Bing Visual arama genel bakış](../overview.md)  
[Deneyin](https://aka.ms/bingvisualsearchtryforfree)  
[Ücretsiz deneme erişim anahtarı alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Visual arama API Başvurusu](https://aka.ms/bingvisualsearchreferencedoc)

