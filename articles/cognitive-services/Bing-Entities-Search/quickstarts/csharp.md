---
title: Bilişsel hizmetler Azure, Bing varlık arama API için C# hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get Bing varlık arama API Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki kullanmaya başlayın.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-jaswel
ms.openlocfilehash: 928cddf5017890bddd25b9da3584d230cc44483a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354869"
---
# <a name="quickstart-for-microsoft-bing-entity-search-api-with-c"></a>Microsoft Bing varlık arama C# ile API için hızlı başlangıç 
<a name="HOLTop"></a>

Bu makalede nasıl kullanılacağı gösterilmektedir [Bing varlık arama](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web) C# ile API.

## <a name="prerequisites"></a>Önkoşullar

İhtiyacınız olacak [Visual Studio 2017](https://www.visualstudio.com/downloads/) Windows bu kodu çalıştırmak için. (Ücretsiz Community sürümü çalışır.)

Sahip olmanız gerekir bir [Bilişsel Hizmetleri API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Bing varlık arama API**. [Ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-entity-search-api) Bu Hızlı Başlangıç için yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirmek ya da Ücretli abonelik anahtarı Azure panonuza kullanabilir sağlanan erişim anahtarı gerekir.

## <a name="search-entities"></a>Arama varlıkları

Bu uygulamayı çalıştırmak için aşağıdaki adımları izleyin.

1. Sık kullanılan IDE içinde yeni bir C# projesi oluşturun.
2. Aşağıda sunulan kodu ekleyin.
3. Değiştir `key` aboneliğiniz için geçerli bir erişim anahtarı ile değer.
4. Programını çalıştırın.

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

Başarılı yanıt JSON'da, aşağıdaki örnekte gösterildiği gibi verilir: 

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
> [Bing varlık arama öğretici](../tutorial-bing-entities-search-single-page-app.md)
> [Bing varlık arama genel bakış](../search-the-web.md )
> [API Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference)
