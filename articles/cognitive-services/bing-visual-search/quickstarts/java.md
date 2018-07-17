---
title: Bing görsel arama API'si için Java hızlı başlangıç | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Bing görsel arama API'sine bir görüntüyü karşıya yükleme ve görüntü ile ilgili Öngörüler geri alma işlemi gösterilmektedir.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 41e0855b126ca6e54d0a487a88fe59a0be6f72f6
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072004"
---
# <a name="your-first-bing-visual-search-query-in-java"></a>İlk Bing görsel arama sorgunuzda Java

Bing görsel arama API'sine sağlayan bir görüntü ile ilgili bilgi döndürür. Belirteç, ya da bir görüntü yükleyerek bir ınsights görüntünün URL'sini kullanarak görüntü sağlayabilir. Bu seçenekler hakkında daha fazla bilgi için bkz: [Bing görsel arama API'si nedir?](../overview.md) Bu makalede, bir resim karşıya gösterilmektedir. Görüntüyü karşıya yükleme, iyi bilinen bir yer işareti resmini Al ve geri hakkında bilgi alın mobil senaryolarda yararlı olabilir. Örneğin, öngörüleri, yer işareti hakkında bilgi dahil olabilir. 

Yerel bir görüntüyü karşıya yükleme, aşağıdaki form verilerini POST gövdesinde içermelidir gösterir. Form verileri içerik düzeni üstbilgisini içermelidir. Kendi `name` parametresi ayarlanması gerekir "Görüntü" ve `filename` parametreyi bir dizeye ayarlayın. Form içeriğini ikili görüntünün olur. Karşıya yükleyebilirsiniz en yüksek görüntü boyutu 1 MB'dir. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Bu makale, Bing görsel arama API'sine bir istek gönderir ve JSON arama sonuçlarını görüntüleyen basit bir konsol uygulaması içerir. Bu uygulama, Java dilinde yazılmış olsa da, HTTP istekleri ve JSON Ayrıştır programlama dili ile uyumlu bir RESTful Web hizmeti API'dir. 


## <a name="prerequisites"></a>Önkoşullar

İhtiyacınız olacak [JDK 7 veya 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) derlemek ve bu kodu çalıştırmak için. Sık kullanılan varsa, ancak bir metin düzenleyicisi ucun yetip Java IDE kullanabilirsiniz.

Bu hızlı başlangıçta, kullanabileceğiniz bir [ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonelik anahtarı veya Ücretli abonelik anahtarı.

## <a name="running-the-application"></a>Uygulamayı çalıştırma

Java'da MultipartEntityBuilder kullanarak görüntüyü karşıya yükleme işlemini gösterir.

Bu uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

1. İndirmenize veya yüklemenize [gson Kitaplığı](https://github.com/google/gson). Ayrıca Maven alabilirsiniz.
2. Yeni bir Java projesi, sık kullandığınız IDE veya düzenleyici oluşturun.
3. Adlı bir dosyada sağlanan kod ekleme `VisualSearch.java`.
4. Değiştirin `subscriptionKey` abonelik anahtarınız ile değeri.
4. Değiştirin `imagePath` görüntünün karşıya yükleme yolunu içeren değer.
5. Programı çalıştırın.


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

[Insights belirteci kullanarak bir görüntü ile ilgili Öngörüler elde edin](../use-insights-token.md)  
[Bing görsel arama görüntüsünü karşıya yükleme Öğreticisi](../tutorial-visual-search-image-upload.md)
[Bing görsel arama tek sayfalı uygulama Öğreticisi](../tutorial-bing-visual-search-single-page-app.md)  
[Bing görsel arama genel bakış](../overview.md)  
[Deneyin](https://aka.ms/bingvisualsearchtryforfree)  
[Ücretsiz deneme erişim anahtarını alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing görsel arama API'si başvurusu](https://aka.ms/bingvisualsearchreferencedoc)

