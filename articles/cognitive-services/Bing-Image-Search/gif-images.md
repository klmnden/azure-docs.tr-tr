---
title: Bing resim arama API'si kullanarak GIF resimler için arama yapın
titleSuffix: Azure Cognitive Services
description: Bing resim arama API'si, web üzerinden .gif görüntülerini aramak için kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: article
ms.date: 04/24/2018
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 0a2040424aa70f30831e214ce0b05d21414ff45c
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389403"
---
# <a name="search-for-gif-images"></a>GIF görüntülerinin arayın 

Bing resim arama API'si de en uygun .gif görüntüleri için tüm Web aramanızı sağlar.  Geliştiriciler, çeşitli konuşma senaryolarda ilgi çekici GIF'ler tümleştirebilirsiniz. 

Animasyonlu .gif görüntüleri için bir sorgu URL'dir.
```
https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=interesting&imageType=AnimatedGif&mkt=en-us
```
[q](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query) parametresi, arama koşulları belirtir.  Ayrıca önceki sorguyu belirtir `animatedGif` kullanarak [ImageType](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagetype) filtre parametresi.

Sonuçları örneklerini görmek için bing.com aramak için şu URL'yi kullanın.
```
https://www.bing.com/images/search?q=interesting&qft=%20filterui%3Aphoto-animatedgif

```
## <a name="query-parameters"></a>Sorgu parametreleri

Sorgu parametreleri ve seçenekleri hakkında daha fazla bilgi için bkz. [resim arama API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query-parameters). Başlığı altında bir örnek aşağıda verilmiştir [animasyonlu GIF Java kullanarak örnek arama](#gifExample).

## <a name="tips-and-suggestions"></a>İpuçları ve öneriler

- Belirtebileceğiniz [maxFileSize](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#maxfilesize) ve [minFileSize](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#minfilesize) parametreleri. MaxFileSize ayarı öneririz, dizinimizi GIF etkinleştirildiklerinde gibi altında 2 MB 2000000 =.  Bu ayrıca bant genişliği, gibi mobil hücresel senaryolarda önemliyse veri boyutunu denetlemek için yardımcı olur.
- Algılanan performansını geliştirmeye yardımcı olmak için ilk önce yükleme kaynak URL'si küçük resim yükleyin.  
- İlk çalıştırma veya yoksa sahip olduğunuz bir kullanıcı sorgu henüz giriş sayfası deneyimi için bizim popüler GIF aramaları Yardım almak için kullanmayı deneyin [popüler resimler API](trending-images.md).
- İçin üç seçenek vardır [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#safesearch) parametresi.  `strict` Seçeneğinin yetişkinlere yönelik içeriği engeller.
- Bkz: [mkt](supported-countries-markets.md) tam dil ve desteklenen konumların listesi.
- *AnimatedGifHttps* yalnızca döndürür animasyonlu GIF görüntüler bir https adresi. Güvenlik için çoğu uygulama, dış web bağlantılarını bağlantı https üzerinden gerektirir. Örneğin, Apple App Store, aktarım sırasında güvenli kullanıcı verileri şifreler, HTTPS üzerinden web hizmetlerine bağlantı gerektirir.

<a name="gifExample" />

## <a name="example-search-for-animated-gif-using-java"></a>Java kullanarak animasyonlu GIF örnek arayın

Aşağıdaki URL'yi animasyonlu .gif görüntülerde arar: `q=interesting`
```
https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=interesting&imageType=AnimatedGif&mkt=en-us

```
Aşağıdaki örnekte gösterildiği gibi URL sorgusu gerektirir [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#headers) başlığı.

Aşağıdaki Java örneğinde oluşturur ve istek gönderir.

```
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

```

## <a name="results"></a>Sonuçlar
Kod, aşağıdaki sonuçları JSON nesnesi alır:

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
- [C# hızlı başlangıcı](quickstarts/csharp.md)
- [Öğretici resim arama tek sayfalı uygulama](tutorial-bing-image-search-single-page-app.md)
