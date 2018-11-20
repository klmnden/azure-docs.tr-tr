---
title: 'Hızlı başlangıç: C# kullanarak uç nokta çağırma - Bing Özel Arama'
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Bing Özel Arama uç noktasını çağırmak için C# kullanarak özel arama örneğinizden arama sonuçlarını isteme adımları gösterilmektedir.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: maheshb
ms.openlocfilehash: a04d2a2bcaaf4edcf03fac3e2242f94712ce8022
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52163053"
---
# <a name="quickstart-call-bing-custom-search-endpoint-c"></a>Hızlı başlangıç: Bing Özel Arama uç noktasını çağırma (C#)

Bu hızlı başlangıçta Bing Özel Arama uç noktasını çağırmak için C# kullanarak özel arama örneğinizden arama sonuçlarını isteme adımları gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Kullanıma hazır özel arama örneği. Bkz. [İlk Bing Özel Arama örneğinizi oluşturma](quick-start.md).
- [.Net Core](https://www.microsoft.com/net/download/core) uygulamasının yüklenmiş olması.
- Abonelik anahtarı. Abonelik anahtarını [ücretsiz denemenizi](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) etkinleştirdikten sonra alabilir veya Azure panonuzdan ücretli abonelik anahtarı (bkz. [Bilişsel Hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)) kullanabilirsiniz.    


## <a name="run-the-code"></a>Kodu çalıştırma

Bu örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Kodunuz için bir klasör oluşturun.  
  
2. Bir komut isteminden veya terminalden az önce oluşturduğunuz klasöre gidin.  
  
3. Aşağıdaki komutları çalıştırın:
    ```
    dotnet new console -o BingCustomSearch
    cd BingCustomSearch
    dotnet add package Newtonsoft.Json
    dotnet restore
    ```
  
4. Aşağıdaki kodu Program.cs dosyasına kopyalayın. **YOUR-SUBSCRIPTION-KEY** ve **YOUR-CUSTOM-CONFIG-ID** yerine abonelik anahtarınızı ve yapılandırma kimliğinizi yazın.

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
6. Aşağıdaki komutu kullanarak uygulamayı derleyin. Komut çıktısında başvurulan DLL yolunu not edin.

    <pre>
    dotnet build 
    </pre>
    
7. Aşağıdaki komutta **PATH TO OUTPUT** değerini 6. adımda başvurulan DLL yoluyla değiştirerek uygulamayı çalıştırın.

    <pre>    
    dotnet **PATH TO OUTPUT**
    </pre>

## <a name="next-steps"></a>Sonraki adımlar
- [Barındırılan kullanıcı arabirimi deneyiminizi yapılandırma](./hosted-ui.md)
- [Metni vurgulamak için süsleme işaretçilerini kullanma](./hit-highlighting.md)
- [Sayfa web sayfaları](./page-webpages.md)
