---
title: Proje yanıt arama Java hızlı başlangıç - Microsoft Bilişsel hizmetler | Microsoft Docs
description: Proje yanıt arama Java kullanmaya başlayın.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 4e1f606e1564981589e638e0e51a8b42633ca7b0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353939"
---
# <a name="project-answer-search-query-in-java"></a>Java projesi yanıt arama sorgusu
Bu makalede, Microsoft Bilişsel hizmetler Azure üzerinde bir parçası Bing yanıt arama API göstermek için Java kullanır. API, HTTP isteklerini hale getirebilir ve JSON ayrıştırma programlama dili ile uyumlu bir REST Web hizmetidir.
 
Kod örneği, en az bir dış bağımlılıkları olan Java kullanır.  Ayrıca Linux veya Mac OS X Mono kullanarak çalıştırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Ücretsiz deneme sürümü için bir erişim anahtarı alma [Bilişsel hizmetler Laboratuvarları](https://aka.ms/answersearchsubscription)

## <a name="request"></a>İstek 

Aşağıdaki kod oluşturur bir `WebRequest`erişim anahtar üstbilgisini ayarlar ve "Cebelitarık" için bir sorgu dizesi ekler.  İsteği gönderir ve yanıt JSON metnini içeren bir dize olarak atar.

````
    static String host = "https://api.labs.cognitive.microsoft.com";
    static String path = "/answerSearch/v7.0/search";

    // construct URL of search request (endpoint + query string)

    URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8") + &mkt=en-us");
    HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

    // receive JSON body
    InputStream stream = connection.getInputStream();
    String response = new Scanner(stream).useDelimiter("\\A").next();

    // construct result object for return
    SearchResults results = new SearchResults(new HashMap<String, String>(), response);
````

## <a name="complete-code"></a>Tam kod

Bing yanıt arama API Bing arama altyapısı sonuçlar döndürür.
1. İndirme veya gson Kitaplığı yükleyin.
2. Sık kullanılan IDE veya Düzenleyicisi yeni bir Java projesi oluşturun.
3. Aşağıda sunulan kodu ekleyin.
4. Aboneliğiniz için geçerli bir erişim anahtarı ile subscriptionKey değeri değiştirin.
5. Programını çalıştırın.

````
package knowledgeAPI;
import java.io.InputStream;
import java.net.*;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Scanner;

import javax.net.ssl.HttpsURLConnection;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

public class KnowledgeSrch {

        // Replace the subscriptionKey string value with your valid subscription key.
        static String subscriptionKey = "YOUR-ACCESS-KEY";

        static String host = "https://api.labs.cognitive.microsoft.com";
        static String path = "/answerSearch/v7.0/search";

        static String searchTerm = "Gibraltar";

        public static SearchResults SearchKnowledge (String searchQuery) throws Exception {

            URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8") + "&mkt=en-us");
            
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
            try {
                System.out.println("Searching the Web for: " + searchTerm);

                SearchResults result = SearchKnowledge(searchTerm);

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

    // Container class for search results encapsulates relevant headers and JSON data
    class SearchResults{
        HashMap<String, String> relevantHeaders;
        String jsonResponse;
        SearchResults(HashMap<String, String> headers, String json) {
            relevantHeaders = headers;
            jsonResponse = json;
        }
}

````

## <a name="next-steps"></a>Sonraki adımlar
- [C# hızlı başlangıç](c-sharp-quickstart.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [Düğüm hızlı başlangıç](node-quickstart.md)