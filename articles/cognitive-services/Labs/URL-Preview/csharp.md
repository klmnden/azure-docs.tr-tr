---
title: Proje URL'si Önizleme - Microsoft Bilişsel hizmetler için C# hızlı başlangıç | Microsoft Docs
description: Proje URL'si Önizleme Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki kullanmaya başlayın.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/16/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 17d44bd0c23d0a1e67da5a0e91248700d3166c1a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354028"
---
# <a name="url-preview-query-in-c"></a>C# URL önizleme sorgu

Aşağıdaki C# örnek SwiftKey Web sitesi için Url Önizleme oluşturur: https://swiftkey.com/en.

## <a name="prerequisites"></a>Önkoşullar

İhtiyacınız olacak [Visual Studio 2017](https://www.visualstudio.com/downloads/) Windows bu kodu çalıştırmak için. (Ücretsiz Community sürümü çalışır.)

Ücretsiz deneme sürümü için bir erişim anahtarı alma [Bilişsel hizmetler Laboratuvarları](https://aka.ms/answersearchsubscription)

## <a name="code-scenario"></a>Kod senaryosu

Aşağıdaki C# kod SwiftKey Web sitesi URL'si önizlemesini oluşturur: https://swiftkey.com/en. 

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
using System.IO;
using System.Net;
using System.Text;

namespace UrlPrevCshp
{
    class Program
    {
        static void Main(string[] args)
        {
            String uriBase = "https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search";
            String searchQuery = "https://swiftkey.com/en"; 
            var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(searchQuery);

            // Do the Web request and get response
            WebRequest request = HttpWebRequest.Create(uriQuery);
            request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR-SUBSCRIPTION-KEY";
            HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
            string json = new StreamReader(response.GetResponseStream()).ReadToEnd();

            Console.WriteLine("\nHTTP Headers:\n");
            foreach (String header in response.Headers)
            {
                if (header.StartsWith("BingAPIs-") || header.StartsWith("X-MSEdge-"))
                    Console.WriteLine(header + ": " + response.Headers[header]);
            }

            Console.WriteLine(JsonPrettyPrint(json));
            

            Console.ReadKey();
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
            char last = ' ';
            int offset = 0;
            int indentLength = 2;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\\':
                        if (quote && last != '\\') ignore = true;
                        break;
                }

                if (quote)
                {
                    sb.Append(ch);
                    if (last == '\\' && ignore) ignore = false;
                }
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
                            if (quote || ch != ' ') sb.Append(ch);
                            break;
                    }
                }
                last = ch;
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
- [Java hızlı başlangıç](java-quickstart.md)
- [JavaScript hızlı başlangıç](javascript.md)
- [Düğüm hızlı başlangıç](node-quickstart.md)
- [Python hızlı başlangıç](python-quickstart.md)