---
title: 'Hızlı Başlangıç: Görsel arama sorgusu oluşturma, Java - Bing Görsel Arama'
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API’sine görüntü yükleme ve görüntü hakkında içgörü alma.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 56e1b943f03128fa6703a7b15bd0d6ade09089d6
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47222633"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-java"></a>Hızlı Başlangıç: Java’da ilk Bing Görsel Arama sorgunuz

Bing Görsel Arama API’si, verdiğiniz bir görüntü hakkında bilgi döndürür. Bir URL veya bir içgörü belirteci kullanarak ya da karşıya resim yükleyerek görüntüyü verebilirsiniz. Bu seçenekler hakkında bilgi için bkz. [Bing Görsel Arama API’si nedir?](../overview.md) Bu makalede, karşıya görüntü yükleme gösterilmektedir. Karşıya resim yüklemek, mobil bir cihazla tanınmış bir yerin resmini çekip bu yer hakkında bilgi almak istediğiniz bir durumda kullanışlı olabilir. Örneğin, içgörüler bu yer hakkındaki önemsiz küçük ayrıntıları içerebilir. 

Aşağıda, yerel bir görüntüyü karşıya yükleyeceğiniz zaman POST’un gövdesine dahil etmeniz gereken form verileri gösterilmektedir. Form verileri, Content-Disposition üst bilgisini içermelidir. `name` parametresi, "image" olarak, `filename` parametresi ise herhangi bir dize olarak ayarlanmalıdır. Formun içerikleri, görüntünün ikili verisidir. Karşıya yükleyebileceğiniz maksimum görüntü boyutu 1 MB’tır. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Bu makale, bir Bing Görsel Arama API’si isteği gönderen ve JSON arama sonuçlarını görüntüleyen basit bir konsol uygulamasını içermektedir. Bu uygulama Java ile yazılmış olmakla birlikte API, HTTP istekleri gönderebilen ve JSON ayrıştırabilen her programlama diliyle uyumlu bir RESTful Web hizmetidir. 


## <a name="prerequisites"></a>Ön koşullar

Bu kodu derleyip çalıştırmak için [JDK 7 veya 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)’e ihtiyacınız olacak. Varsa, sık kullandığınız bir Java IDE’yi veya bir metin düzenleyicisini kullanabilirsiniz.

Bu hızlı başlangıçta bir [ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonelik anahtarı veya ücretli abonelik anahtarı kullanabilirsiniz.

## <a name="running-the-application"></a>Uygulamayı çalıştırma

Aşağıda, Java’da MultipartEntityBuilder kullanarak görüntüyü karşıya yükleme işlemi gösterilmektedir.

Bu uygulamayı çalıştırmak için şu adımları izleyin:

1. [Gson kitaplığı](https://github.com/google/gson)’nı indirip yükleyin. Maven aracılığıyla da edinebilirsiniz.
2. Sık kullandığınız IDE veya düzenleyicide yeni bir Java projesi oluşturun.
3. Sağlanan kodu `VisualSearch.java` adlı bir dosyaya ekleyin.
4. `subscriptionKey` değerini, abonelik anahtarınızla değiştirin.
4. `imagePath` değerini, karşıya yüklenecek görüntünün yoluyla değiştirin.
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

[İçgörü belirteci kullanarak bir görüntü ile ilgili içgörüler elde edin](../use-insights-token.md)  
[Bing Görsel Arama görüntü karşıya yükleme öğreticisi](../tutorial-visual-search-image-upload.md)
[Bing Görsel Arama tek sayfalı uygulama öğreticisi](../tutorial-bing-visual-search-single-page-app.md)  
[Bing Görsel Arama’ya genel bakış](../overview.md)  
[Deneyin](https://aka.ms/bingvisualsearchtryforfree)  
[Ücretsiz deneme erişim anahtarı alın](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Görsel Arama API’si başvurusu](https://aka.ms/bingvisualsearchreferencedoc)

