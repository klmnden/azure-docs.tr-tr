---
title: "Hızlı başlangıç: Bing Varlık Arama API'si, C#"
titlesuffix: Azure Cognitive Services
description: Bing Varlık Arama API'sini kısa sürede kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri alın.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: quickstart
ms.date: 11/28/2017
ms.author: aahi
ms.openlocfilehash: 7c303516c9b455ce3477c348c59cbc88693e2297
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52313869"
---
# <a name="quickstart-for-bing-entity-search-api-with-c"></a>Hızlı başlangıç: C# ile Bing Varlık Arama API'si 

Bu makalede nasıl kullanılacağını gösterir [Bing varlık arama](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web) API'SİYLE C#.

## <a name="prerequisites"></a>Önkoşullar

Bu kodu Windows’da çalıştırmak için [Visual Studio 2017](https://www.visualstudio.com/downloads/) gerekir. (Ücretsiz Community Edition çalışmaz).

**Bing Varlık Arama API'sine** sahip bir [Bilişsel Hizmetler API hesabınız](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-entity-search-api) bu hızlı başlangıç için yeterlidir. Ücretsiz denemenizi etkinleştirdiğinizde verilen erişim anahtarınız olması veya Azure panonuzdan ücretli bir abonelik anahtarı kullanmanız gerekir.  Ayrıca bkz: [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="search-entities"></a>Varlık arama

Bu uygulamayı çalıştırmak için şu adımları izleyin.

1. Sık kullandığınız IDE'de yeni bir C# projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```csharp
using System;
using System.Net.Http;
using System.Text;

namespace EntitySearchSample
{
    class Program
    {
        static string host = "https://api.cognitive.microsoft.com";
        static string path = "/bing/v7.0/entities";

        static string market = "en-US";

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "ENTER KEY HERE";

        static string query = "italian restaurant near me";

        async static void Search()
        {
            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

            string uri = host + path + "?mkt=" + market + "&q=" + System.Net.WebUtility.UrlEncode(query);

            HttpResponseMessage response = await client.GetAsync(uri);

            string contentString = await response.Content.ReadAsStringAsync();
            Console.WriteLine(JsonPrettyPrint(contentString));
        }

        static void Main(string[] args)
        {
            Search();
            Console.ReadLine();
        }


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

**Yanıt**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "http://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

[Başa dön](#HOLTop)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Varlık Arama öğreticisi](../tutorial-bing-entities-search-single-page-app.md)
> [Bing Varlık Arama'ya genel bakış](../search-the-web.md )
> [API Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference)
