---
title: Microsoft Bilişsel hizmetler .gif resimlerini - alma | Microsoft Docs
description: Bing görüntü arama API .gif görüntüleri hakkında daha fazla bilgi almak için nasıl kullanılacağını.
services: cognitive-services
author: MikeDodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/24/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 10e922b0cd15868bfe8f09b3846c76a368052e69
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355061"
---
# <a name="search-for-gif-images"></a>.Gif resimlerini arayın
Bing görüntü arama API de en uygun .gif görüntüleri için tüm Web üzerinden aramanıza olanak tanır.  Geliştiriciler, çeşitli konuşma senaryolarda kurmadığını GIF tümleştirebilirsiniz. 

Animasyonlu .gif görüntüleri için bir sorgu URL'dir.
````
https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=interesting&imageType=AnimatedGif&mkt=en-us
````
[q](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-images-api-v7-reference#query) parametresi, arama terimleri belirtir.  Önceki sorgu ayrıca belirtir `animatedGif` kullanarak [imageType](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype) filtre parametresi.

Sonuçları örnekleri görmek için aşağıdaki URL'yi aratıp aramak için kullanın.
````
https://www.bing.com/images/search?q=interesting&qft=%20filterui%3Aphoto-animatedgif 

````
## <a name="query-parameters"></a>Sorgu parametreleri

Sorgu parametreleri ve seçenekleri hakkında daha fazla bilgi için bkz: [görüntü arama API Başvurusu](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-images-api-v7-reference#query-parameters). Bir örnek başlığı altında aşağıdaki [Java kullanarak animasyonlu gif örnek arama](#gifExample).

## <a name="tips-and-suggestions"></a>İpuçları ve öneriler

- Belirleyebileceğiniz [maxFileSize](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-images-api-v7-reference#maxfilesize) ve [minFileSize](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-images-api-v7-reference#minfilesize) parametreleri. MaxFileSize ayarı öneririz dizinimizi GIF çoğunluğu olarak altında 2 MB 2000000 =.  Bu ayrıca bant genişliği, gibi mobil cep telefonu senaryolarda önemliyse veri boyutunu denetlemek için yardımcı olur.
- Algılanan performansını iyileştirmeye yardımcı olmak için ilk önce yükleme kaynağı URL'si küçük resim yüklenemiyor.  
- İlk çalıştırma veya mu olduğu bir kullanıcı sorgu henüz giriş sayfası deneyimi için bizim oluşturan eğilim GIF aramaları Yardım kullanmayı denemek [eğilim görüntüleri API](trending-images.md).
- İçin üç seçenek vardır [güvenli arama](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch) parametresi.  `strict` Seçeneği yetişkinlere yönelik içeriğe engeller. 
- Bkz: [mkt](supported-countries-markets.md) diller ve desteklenen konumların tam listesi için.
- *AnimatedGifHttps* yalnızca döndürür animasyonlu bir https adresi GIF görüntüler. Güvenlik için birçok uygulama, dış web bağlantıları bağlantı https üzerinden gerektirir. Örneğin, Apple App Store aktarım sırasında güvenli kullanıcı verileri şifreler, HTTPS üzerinden web hizmetlerine bağlantı gerektirir.

<a name="gifExample" />
## <a name="example-search-for-animated-gif-using-java"></a>Animasyonlu gif Java kullanarak örnek arayın

Aşağıdaki URL'yi animasyonlu .gif görüntülerde arar: `q=interesting`
````
https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=interesting&imageType=AnimatedGif&mkt=en-us

````
Aşağıdaki örnekte gösterildiği gibi URL sorgusu gerektirir [Apim abonelik anahtar Ocp](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-images-api-v7-reference#headers) üstbilgi.

Aşağıdaki Java örneğinde oluşturur ve isteği gönderir.

````
package gifSearch;
import java.net.*;
import java.util.*;
import java.io.*;
import javax.net.ssl.HttpsURLConnection;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.1
 *
 * Once you have compiled or downloaded gson-2.8.1.jar, assuming you have placed it in the
 * same folder as this file (BingImageSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac GIFsearch.java -classpath .;gson-2.8.1.jar -encoding UTF-8
 * java -cp .;gson-2.8.1.jar GIFsearch
 */


public class GIFsearch {

    // Replace the subscriptionKey string value with your valid subscription key.
    static String subscriptionKey = "YOUR-ACCESS-KEY";

    static String host = "https://api.cognitive.microsoft.com";
    static String path = "/bing/v7.0/images/search";

    static String searchTerm = "interesting";

    public static SearchResults SearchImages (String searchQuery) throws Exception {
        // construct URL of search request (endpoint + query string)
        URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8") + "&imageType=AnimatedGif&mkt=en-us");
        HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

        // receive JSON body
        InputStream stream = connection.getInputStream();
        String response = new Scanner(stream).useDelimiter("\\A").next();

        // construct result object for return
        SearchResults results = new SearchResults(new HashMap<String, String>(), response);

        // extract Bing-related HTTP headers
        Map<String, List<String>> headers = connection.getHeaderFields();
        for (String header : headers.keySet()) {
            if (header == null) continue;      // may have null key
            if (header.startsWith("BingAPIs-") || header.startsWith("X-MSEdge-")) {
                results.relevantHeaders.put(header, headers.get(header).get(0));
            }
        }

        stream.close();
        return results;
    }

    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main (String[] args) {
        if (subscriptionKey.length() != 32) {
            System.out.println("Invalid Bing Search API subscription key!");
            System.out.println("Please paste yours into the source code.");
            System.exit(1);
        }

        try {
            System.out.println("Searching the Web for: " + searchTerm);

            SearchResults result = SearchImages(searchTerm);

            System.out.println("\nRelevant HTTP Headers:\n");
            for (String header : result.relevantHeaders.keySet())
                System.out.println(header + ": " + result.relevantHeaders.get(header));

            System.out.println("\nJSON Response:\n");
            System.out.println(prettify(result.jsonResponse));
        }
        catch (Exception e) {
            e.printStackTrace(System.out);
            System.exit(1);
        }
    }

}

//Container class for search results encapsulates relevant headers and JSON data
class SearchResults{
 HashMap<String, String> relevantHeaders;
 String jsonResponse;
 SearchResults(HashMap<String, String> headers, String json) {
     relevantHeaders = headers;
     jsonResponse = json;
 }
}

````

## <a name="results"></a>Sonuçlar
Kod aşağıdaki sonuçları JSON nesneler olarak alır:

```json
    {
      "webSearchUrl": "https://www.bing.com/images/search?view\u003ddetai...",
      "name": "Very Interesting GIF - Thats Very Interesting - ...",
      "thumbnailUrl": "https://tse1.mm.bing.net/th?id\u003dOIP.yJX6Vz345JPK...",
      "datePublished": "2017-03-12T01:35:00.0000000Z",
      "contentUrl": "https://media.contoso.co/images/c895fa573df8e493ca8d0dec7d93b/raw",
      "hostPageUrl": "https://www.contoso.co/view/thats-very-interesting-christi...",
      "contentSize": "1295633 B",
      "encodingFormat": "animatedgif",
      "hostPageDisplayUrl": "https://www.contoso.co/view/thats-very-christian...",
      "width": 440,
      "height": 186,
      "thumbnail": {
        "width": 474,
        "height": 200
      },
      "imageInsightsToken": "ccid_yJX6Vz34*mid_9FF0FFA42AADA1357F042443D2103B40EA...",
      "insightsMetadata": {
        "recipeSourcesCount": 0,
        "bestRepresentativeQuery": {
          "text": "That\u0027s Very Interesting",
          "displayText": "That\u0027s Very Interesting",
          "webSearchUrl": "https://www.bing.com/images/search?q\u003dThat..."
        },
        "pagesIncludingCount": 19,
        "availableSizesCount": 2
      },
      "imageId": "9FF0FFA42AADA1357F042443D21030EAAA225F",
      "accentColor": "62452D"
    },

```

## <a name="next-steps"></a>Sonraki adımlar
- [C# hızlı başlangıç](quickstarts/csharp.md)
- [Eğitmen görüntü arama tek sayfalı uygulama](tutorial-bing-image-search-single-page-app.md)