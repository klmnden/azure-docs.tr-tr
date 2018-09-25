---
title: C# - Bing özel arama - Microsoft Bilişsel hizmetler kullanarak uç noktasını çağırmak
description: Bu hızlı başlangıçta, Bing özel arama uç noktasını çağırmak için C# kullanarak arama sonuçlarını özel arama örneğinizin isteği gösterilmektedir.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: ed00b75fa956d0197d3672d84b097f99ec3c35ec
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956399"
---
# <a name="call-bing-custom-search-endpoint-c"></a>Bing özel arama uç noktası çağrısı (C#)

Bu hızlı başlangıçta, arama sonuçlarını Bing özel arama uç noktasını çağırmak için C# kullanarak özel arama örneğinizin isteği gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Bir kullanıma hazır özel arama örneği. Bkz: [ilk Bing özel arama örneğinizin oluşturma](quick-start.md).
- [.Net Core](https://www.microsoft.com/net/download/core) yüklü.
- Bir abonelik anahtarı. Etkinleştirme sırasında bir abonelik anahtarı edinirler, [ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search), ya da Ücretli abonelik anahtarı, Azure panosundan kullanabilirsiniz (bkz [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).    


## <a name="run-the-code"></a>Kodu çalıştırma

Bu örneği çalıştırmak için aşağıdaki adımları izleyin:

1. Kodunuz için bir klasör oluşturun.  
  
2. Bir komut istemi veya terminal, az önce oluşturduğunuz klasöre gidin.  
  
3. Aşağıdaki komutları çalıştırın:
    ```
    dotnet new console -o BingCustomSearch
    cd BingCustomSearch
    dotnet add package Newtonsoft.Json
    dotnet restore
    ```
  
4. Program.cs'ye aşağıdaki kodu kopyalayın. Değiştirin **YOUR-SUBSCRIPTION-KEY** ve **YOUR-özel-CONFIG-ID** abonelik anahtarınızı ve yapılandırma kimliği

    ```csharp
    using System;
    using System.Net.Http;
    using System.Web;
    using Newtonsoft.Json;
    
    namespace bing_custom_search_example_dotnet
    {
        class Program
        {
            static void Main(string[] args)
            {
                var subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
                var customConfigId = "YOUR-CUSTOM-CONFIG-ID";
                var searchTerm = args.Length > 0 ? args[0]: "microsoft";            
    
                var url = "https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?" +
                    "q=" + searchTerm +
                    "&customconfig=" + customConfigId;
    
                var client = new HttpClient();
                client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
                var httpResponseMessage = client.GetAsync(url).Result;
                var responseContent = httpResponseMessage.Content.ReadAsStringAsync().Result;
                BingCustomSearchResponse response = JsonConvert.DeserializeObject<BingCustomSearchResponse>(responseContent);
                
                for(int i = 0; i < response.webPages.value.Length; i++)
                {                
                    var webPage = response.webPages.value[i];
                    
                    Console.WriteLine("name: " + webPage.name);
                    Console.WriteLine("url: " + webPage.url);                
                    Console.WriteLine("displayUrl: " + webPage.displayUrl);
                    Console.WriteLine("snippet: " + webPage.snippet);
                    Console.WriteLine("dateLastCrawled: " + webPage.dateLastCrawled);
                    Console.WriteLine();
                }            
            }
        }
    
        public class BingCustomSearchResponse
        {        
            public string _type{ get; set; }            
            public WebPages webPages { get; set; }
        }
    
        public class WebPages
        {
            public string webSearchUrl { get; set; }
            public int totalEstimatedMatches { get; set; }
            public WebPage[] value { get; set; }        
        }
    
        public class WebPage
        {
            public string name { get; set; }
            public string url { get; set; }
            public string displayUrl { get; set; }
            public string snippet { get; set; }
            public DateTime dateLastCrawled { get; set; }
            public string cachedPageUrl { get; set; }
            public OpenGraphImage openGraphImage { get; set; }        
        }
        
        public class OpenGraphImage
        {
            public string contentUrl { get; set; }
            public int width { get; set; }
            public int height { get; set; }
        }
    }
    ```
6. Aşağıdaki komutu kullanarak uygulama oluşturun. Komut çıktısı tarafından başvurulan DLL yolu unutmayın.

    <pre>
    dotnet build 
    </pre>
    
7. Değiştirerek aşağıdaki komutu kullanarak uygulamayı çalıştırın **yolu için çıkış** 6. adımda başvurulan DLL yolu ile.

    <pre>    
    dotnet **PATH TO OUTPUT**
    </pre>

## <a name="next-steps"></a>Sonraki adımlar
- [Barındırılan UI deneyiminizi yapılandırın](./hosted-ui.md)
- [Metni vurgulayacak şekilde decoration işaretçileri kullanma](./hit-highlighting.md)
- [Sayfa Web sayfaları](./page-webpages.md)
