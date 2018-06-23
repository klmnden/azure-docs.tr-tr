---
title: C# - Bing özel arama - Microsoft Bilişsel hizmetler kullanarak uç nokta çağırma
description: Bu hızlı başlangıç Bing özel arama uç noktasını çağırmak için C# kullanarak özel arama örneğinden arama sonuçlarında istek gösterilmektedir.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: be4cc79d16b9a22124f16878b11ca04a916f98ae
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352858"
---
# <a name="call-bing-custom-search-endpoint-c"></a>Çağrı Bing özel arama uç noktası (C#)

Bu hızlı başlangıç Bing özel arama uç noktasını çağırmak için C# kullanarak özel arama örneğinden arama sonuçlarında istek gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar

-  Bir kullanıma hazır özel arama örneği. Bkz: [ilk Bing özel arama örneğinizi oluşturmak](quick-start.md).
-  [.NET core](https://www.microsoft.com/net/download/core) yüklü.
- A [Bilişsel Hizmetleri API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Bing arama API'leri**. [Ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) Bu Hızlı Başlangıç için yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirmek ya da Ücretli abonelik anahtarı Azure panonuza kullanabilir sağlanan erişim anahtarı gerekir.  

  >[!NOTE]  
  >Veya bu tarihten önce 15 Ekim 2017 sağlanan bir önizleme anahtara sahip varolan Bing özel arama müşteriler kendi anahtarları 30 Kasım 2017 kadar veya sayısı izin verilen sorgular tüketmiş kadar kullanmanız mümkün olacaktır. Daha sonra Azure ile ilgili genel olarak kullanılabilir sürümüne geçirmek gerekir. 
 
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

4. Program.cs için aşağıdaki kodu kopyalayın.
5. Değiştir **YOUR ABONELİK anahtarı** ve **YOUR-özel-CONFIG-ID** , anahtar ve yapılandırma kimliği

    ``` CSharp
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
6. Aşağıdaki komutu kullanarak uygulaması oluşturma. Komut çıktısı tarafından başvurulan dll yolu unutmayın.
    <pre>
    dotnet build 
    </pre>
7. Değiştirerek aşağıdaki komutu kullanarak uygulamayı çalıştırmak **yolu için çıktı** derleme adımı tarafından başvurulan yoluna sahip.
    <pre>    
    dotnet **PATH TO OUTPUT**
    </pre>

## <a name="next-steps"></a>Sonraki adımlar
- [Barındırılan UI deneyiminizi yapılandırın](./hosted-ui.md)
- [Metni vurgulama için decoration işaretlerini kullanın](./hit-highlighting.md)
- [Sayfa Web sayfaları](./page-webpages.md)
