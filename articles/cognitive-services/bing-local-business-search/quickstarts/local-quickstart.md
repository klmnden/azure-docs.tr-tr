---
title: Hızlı Başlangıç - Bing yerel iş arama API'si bir sorguya bir gönderme C# | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: İçinde Bing yerel iş arama API'sini kullanmaya başlamak için bu makaleyi kullanın C#.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 91ff4c8e6f8487a2b0e5849f881e0cdb9c10385f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60576146"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-in-c"></a>Hızlı Başlangıç: Bing yerel iş arama API'si bir sorgu gönderinC#

Bu hızlı başlangıçta, Azure Bilişsel hizmet olduğu Bing yerel iş arama API'si için istekleri göndermeye başlamak için kullanın. Bu basit uygulama yazıldığı sırada C#, tüm programlama dillerini HTTP isteğinde bulunan ve JSON ayrıştırma özelliğine sahip uyumlu bir RESTful Web hizmeti API'dir.

Bu örnek uygulama, arama sorgusu için API'sinden yerel yanıt verilerini alır. `hotel in Bellevue`.

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)’nin herhangi bir sürümü.
* Linux/MacOS kullanıyorsanız bu uygulama, [Mono](https://www.mono-project.com/) kullanılarak çalıştırılabilir.

Bing Arama API'lerine sahip bir [Bilişsel Hizmetler API hesabınız](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) bu hızlı başlangıç için yeterlidir.  Ayrıca bkz: [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="create-the-request"></a>İstek oluşturma 

Aşağıdaki kod oluşturur bir `WebRequest`erişim anahtar üstbilgisini ayarlar ve "Restoran Bellevue içinde" için bir sorgu dizesi ekler.  Ardından isteği gönderir ve yanıtı JSON metnini içeren bir dizeye atar.

```
    // Replace the accessKey string value with your valid access key.
    const string accessKey = "enter key here";

    const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search";   

    const string searchTerm = "restaurant in Bellevue";
    // Construct the URI of the search request
    var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(searchQuery) + mkt=en-us;

    // Run the Web request and get response.
    WebRequest request = HttpWebRequest.Create(uriQuery);
    request.Headers["Ocp-Apim-Subscription-Key"] = accessKey; 

    HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
    string json = new StreamReader(response.GetResponseStream()).ReadToEnd();
```

## <a name="run-the-complete-application"></a>Tam uygulama çalıştırma

Bing yerel iş arama API'si, Bing arama motorundan yerelleştirilmiş arama sonuçlarını döndürür.
1. Visual Studio'da (Community Edition uygundur) yeni bir Konsol çözümü oluşturun.
2. Program.cs dosyasını aşağıda sağlanan kod ile değiştirin.
3. AccessKey değeri, aboneliğiniz için geçerli bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```
using System;
using System.Collections.Generic;
using System.Text;
using System.Net;
using System.IO;

namespace localSearch
{
    class Program
    {
        // **********************************************
        // *** Update or verify the following values. ***
        // **********************************************

        // Replace the accessKey string value with your valid access key.
        const string accessKey = "enter key here";

        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search";   

        const string searchTerm = "restaurant in Bellevue";

        // Returns search results including relevant headers
        struct SearchResult
        {
            public String jsonResult;
            public Dictionary<String, String> relevantHeaders;
        }

        static void Main()
        {
            Console.OutputEncoding = System.Text.Encoding.UTF8;
            Console.WriteLine("Searching locally for: " + searchTerm);

            SearchResult result = BingLocalSearch(searchTerm, accessKey);

            Console.WriteLine("\nRelevant HTTP Headers:\n");
            foreach (var header in result.relevantHeaders)
                Console.WriteLine(header.Key + ": " + header.Value);

            Console.WriteLine("\nJSON Response:\n");
            Console.WriteLine(JsonPrettyPrint(result.jsonResult));

            Console.Write("\nPress Enter to exit ");
            Console.ReadLine();
        }

        /// <summary>
        /// Performs a Bing Local business search and return the results as a SearchResult.
        /// </summary>
        static SearchResult BingLocalSearch(string searchQuery, string key)
        {
            // Construct the URI of the search request
            var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(searchQuery) + "&mkt=en-us";

            // Perform the Web request and get the response
            WebRequest request = HttpWebRequest.Create(uriQuery);
            request.Headers["Ocp-Apim-Subscription-Key"] = key; 

            HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
            string json = new StreamReader(response.GetResponseStream()).ReadToEnd();

            // Create result object for return
            var searchResult = new SearchResult();
            searchResult.jsonResult = json;
            searchResult.relevantHeaders = new Dictionary<String, String>();

            // Extract Bing HTTP headers
            foreach (String header in response.Headers)
            {
                if (header.StartsWith("BingAPIs-") || header.StartsWith("X-MSEdge-"))
                    searchResult.relevantHeaders[header] = response.Headers[header];
            }

            return searchResult;
        }

        /// <summary>
        /// Formats the given JSON string by adding line breaks and indents.
        /// </summary>
        /// <param name="json">The raw JSON string to format.</param>
        /// <returns>The formatted JSON string.</returns>
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            StringBuilder sb = new StringBuilder();
            bool quote = false;
            bool ignore = false;
            int offset = 0;
            int indentLength = 3;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\'':
                        if (quote) ignore = !ignore;
                        break;
                }

                if (quote)
                    sb.Append(ch);
                else
                {
                    switch (ch)
                    {
                        case '{':
                        case '[':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', ++offset * indentLength));
                            break;
                        case '}':
                        case ']':
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', --offset * indentLength));
                            sb.Append(ch);
                            break;
                        case ',':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', offset * indentLength));
                            break;
                        case ':':
                            sb.Append(ch);
                            sb.Append(' ');
                            break;
                        default:
                            if (ch != ' ') sb.Append(ch);
                            break;
                    }
                }
            }

            return sb.ToString().Trim();
        }
    }
}

```

## <a name="next-steps"></a>Sonraki adımlar
- [Yerel iş arama Java hızlı başlangıç](local-search-java-quickstart.md)
- [Yerel iş arama düğümü hızlı başlangıç](local-search-node-quickstart.md)
- [Yerel iş arama Python hızlı başlangıç](local-search-python-quickstart.md)
