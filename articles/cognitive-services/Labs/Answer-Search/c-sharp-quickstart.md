---
title: Microsoft Bilişsel hizmetler, proje yanıt arama için C# hızlı başlangıç | Microsoft Docs
description: Microsoft Azure'da Bilişsel hizmetler proje yanıt arama kullanmaya başlamak için kod örneği.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: b05b7745bdb5020a4a0912635e040256415b2626
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353944"
---
# <a name="project-answer-search-query-in-c"></a>C# proje yanıt arama sorgusu

Aşağıdaki C# örnek oluşturur ve hesaplama üçüncü yasal hakkında bilgi için bir sorgu gönderir.

## <a name="prerequisites"></a>Önkoşullar

İhtiyacınız olacak [Visual Studio 2017](https://www.visualstudio.com/downloads/) Windows bu kodu çalıştırmak için. (Ücretsiz Community sürümü çalışır.)

Ücretsiz deneme sürümü için bir erişim anahtarı alma [Bilişsel hizmetler Laboratuvarları](https://aka.ms/answersearchsubscription)

## <a name="code-scenario"></a>Kod senaryosu

Aşağıdaki C# kod oluşturur ve sorgusu gönderir. 

Aşağıdaki adımlarda uygulanır:
1. Uç nokta ve önizleme için bir sorgu URL'sini belirtmek için değişkenleri bildirin.  
2. İsteği oluşturun.
3. Ekleme *Apim abonelik anahtar Ocp* üstbilgi. 
4. Web isteği zaman uyumsuz olarak çalıştırın. 
5. Yanıt okuyun.
6. Üstbilgiler ve JSON sonuçlarını konsola yazdırır.

**Kaynak kodu**

```
using System;
using System.Collections.Generic;
using System.Text;
using System.Net;
using System.IO;

namespace Answers_csharp
{
    class Program
    {
        // Replace the accessKey string value with your valid access key.
        const string accessKey = "YOUR-SUBSCRIPTION-KEY";

        const string uriBase = "https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search "; 

        const string searchTerm = "third law of calculus"; 

        // Used to return news search results including relevant headers
        struct SearchResult
        {
            public String jsonResult;
            public Dictionary<String, String> relevantHeaders;
        }

        static void Main()
        {
            Console.OutputEncoding = System.Text.Encoding.UTF8;
            Console.WriteLine("Searching locally for: " + searchTerm);

            SearchResult result = BingLocalSearch(searchTerm);

            Console.WriteLine("\nRelevant HTTP Headers:\n");
            foreach (var header in result.relevantHeaders)
                Console.WriteLine(header.Key + ": " + header.Value);

            Console.WriteLine("\nJSON Response:\n");
            Console.WriteLine(JsonPrettyPrint(result.jsonResult));

            Console.Write("\nPress Enter to exit ");
            Console.ReadLine();
        }

        /// <summary>
        /// Does Bing knowledge search and returns the results as a SearchResult.
        /// </summary>
        static SearchResult BingLocalSearch(string searchQuery)
        {
            // Construct the URI of the search request
            var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(searchQuery) + "&mkt=en-us";

            // Send the Web request and get the response.
            WebRequest request = HttpWebRequest.Create(uriQuery);
            request.Headers["Ocp-Apim-Subscription-Key"] = accessKey; 

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
## <a name="running-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırmak için:

1. Visual Studio'da yeni bir konsol çözümü oluşturun.
2. Değiştir `Program.cs` sağlanan koduna sahip.
3. Değiştir `YOUR-ACCESS-KEY` aboneliğiniz için geçerli erişim anahtarı ile değer.
4. Programını çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar
[Java hızlı başlangıç](java-quickstart.md)
