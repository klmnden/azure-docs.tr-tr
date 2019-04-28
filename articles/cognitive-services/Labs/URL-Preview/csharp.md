---
title: 'Hızlı Başlangıç: Proje URL önizlemesiC#'
titlesuffix: Azure Cognitive Services
description: C# ile URL Önizleme Projesini kullanmaya başlayın.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: quickstart
ms.date: 03/16/2018
ms.author: rosh
ms.openlocfilehash: 0e2d4b783068cfd14385eb1712ea246c94dc9500
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61335492"
---
# <a name="quickstart-url-preview-query-in-c"></a>Hızlı Başlangıç: URL önizlemesi sorguC#

Aşağıdaki C# örneği SwiftKey Web sitesi için bir URL Önizlemesi oluşturur: https://swiftkey.com/en.

## <a name="prerequisites"></a>Önkoşullar

Bu kodu Windows’da çalıştırmak için [Visual Studio 2017](https://www.visualstudio.com/downloads/) gerekir. (Ücretsiz Community Edition’ı kullanabilirsiniz.)

[Bilişsel Hizmetler Laboratuvarları](https://aka.ms/answersearchsubscription) ücretsiz denemesi için erişim anahtarı alın

## <a name="code-scenario"></a>Kod senaryosu

Aşağıdaki C# kodu SwiftKey Web sitesi için bir URL Önizlemesi oluşturur: https://swiftkey.com/en. 

Aşağıdaki adımları izler:
1. Önizlemesi yapılacak uç noktayı ve sorgu URL'sini belirtmek için değişkenleri bildirme.  
2. İsteği oluşturma.
3. *Ocp-Apim-Subscription-Key* üst bilgisini ekleme. 
4. Web isteğini zaman uyumsuz olarak çalıştırma. 
5. Yanıtı okuma.
6. Üst bilgileri ve JSON sonuçlarını konsolda yazdırma.

**Kaynak kod**

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

1. Visual Studio'da yeni bir Konsol çözümü oluşturun.
2. `Program.cs` dosyasını verilen kodla değiştirin.
3. `YOUR-ACCESS-KEY` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar
- [Java hızlı başlangıcı](java-quickstart.md)
- [JavaScript hızlı başlangıcı](javascript.md)
- [Node hızlı başlangıcı](node-quickstart.md)
- [Python hızlı başlangıcı](python-quickstart.md)
