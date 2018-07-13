---
title: Bing haber arama C# Öğreticisi | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Bilişsel hizmetler Bing haber arama, bir ASP.NET Core web uygulamasından bağlanın.
services: cognitive-services
author: ghogen
manager: douge
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: conceptual
ms.date: 03/01/2018
ms.author: ghogen
ms.openlocfilehash: 5cfa82067d28b05f32bd87e0e83d55a03da8d508
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38665223"
---
# <a name="connect-to-bing-news-search-api-by-using-connected-services-in-visual-studio"></a>Bing haber arama API'si için Visual Studio bağlı Hizmetler'i kullanarak bağlanma

Bing haber arama kullanarak, uygulamalarınız ve web için kapsamlı bir Reklamsız bir arama motoru gücünden etkinleştirebilirsiniz. Bing haber arama, Bilişsel hizmetler ile arama hizmetleri biridir.

Bu makalede, Visual Studio bağlı hizmeti özelliği için Bing haber arama kullanmak için Ayrıntılar sağlar. Özelliği, Visual Studio 2017 15.7 veya sonraki sürümlerde, Bilişsel hizmetler uzantısı yüklü ile kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
- Visual Studio 2017 sürüm 15.7, yüklü Web geliştirme iş yüküyle birlikte sağlanır. [Hemen indirin](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="add-support-to-your-project-for-bing-news-search-api"></a>Bing haber arama API'si için projenize desteği eklendi

1. MyWebApplication adlı yeni bir ASP.NET Core web projesi oluşturun. Kullanım **Web uygulaması (Model-View-Controller)** tüm varsayılan ayarlarla proje şablonu. Kod projesine kopyaladığınızda, ad alanı eşleşecek şekilde MyWebApplication, proje adı önemlidir. 

1. İçinde **Çözüm Gezgini**, seçin **Ekle** > **bağlı hizmet**.
   Projenize eklediğiniz Hizmetleri ile bağlı hizmet sayfasında görünür.

   ![Ekran görüntüsü, bağlı hizmet Ekle menü öğesi](../media/vs-common/Connected-Service-Menu.PNG)

1. Kullanılabilir hizmetler menüsünde **bilgisayarınızı uygulamaları için akıllı arama Getir**.

   ![Bağlı hizmetler listesinin ekran görüntüsü](./media/vs-bing-news-search-connected-service/Cog-Search-Connected-Service-0.PNG)

   Visual Studio'da oturum açıldıktan ve hesabınızla ilişkili bir Azure aboneliğiniz varsa, aboneliklerinizin ile bir açılan listedeki bir sayfa görünür. Kullanmak istediğiniz aboneliği seçin ve ardından Bing haber arama API'si için bir ad seçin. Ayrıca seçebilirsiniz **Düzenle** otomatik olarak oluşturulan adı değiştirmek için.

   ![Abonelik ve ad alanlarını ekran görüntüsü](media/vs-bing-news-search-connected-service/Cog-Search-Connected-Service-1.PNG)

1. Kaynak grubunu ve fiyatlandırma katmanını seçin.

   ![Kaynak grubu ve fiyatlandırma katmanı alanları ekran görüntüsü](media/vs-bing-news-search-connected-service/Cog-Search-Connected-Service-2.PNG)

   Fiyatlandırma katmanları hakkında daha fazla bilgi istiyorsanız belirleyin **gözden geçirme fiyatlandırma**.

1. Seçin **Ekle** bağlı hizmeti için destek eklemek için.
   Visual Studio projenizin NuGet paketlerini, yapılandırma dosyası girdisi ve Bing haber arama API'si bağlantısı desteklemek için başka değişiklikler eklemek için değiştirir. Çıkış projenize olup bitenleri günlük gösterir. Aşağıdaki gibi görmeniz gerekir:

   ```output
   [5/4/2018 12:41:21.084 PM] Adding Intelligent Search to the project.
   [5/4/2018 12:41:21.271 PM] Creating new Intelligent Search...
   [5/4/2018 12:41:24.128 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.ImageSearch' version 1.2.0...
   [5/4/2018 12:41:24.135 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.NewsSearch' version 1.2.0...
   [5/4/2018 12:41:24.154 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.WebSearch' version 1.2.0...
   [5/4/2018 12:41:24.168 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.CustomSearch' version 1.2.0...
   [5/4/2018 12:41:24.187 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.VideoSearch' version 1.2.0...
   [5/4/2018 12:42:07.287 PM] Retrieving keys...
   [5/4/2018 12:42:07.741 PM] Updating appsettings.json setting: 'ServiceKey' = 'c271412f3e4c4e1dacc7c4145fa0572a'
   [5/4/2018 12:42:07.745 PM] Updating appsettings.json setting: 'ServiceEndPoint' = 'https://api.cognitive.microsoft.com/bing/v7.0'
   [5/4/2018 12:42:07.749 PM] Updating appsettings.json setting: 'Name' = 'WebApplicationCore-Search_IntelligentSearch'
   [5/4/2018 12:42:10.217 PM] Successfully added Intelligent Search to the project.
   ```

   Appsettings.json dosyasını artık aşağıdaki yeni ayarları içerir:

   ```json
   "CognitiveServices": {
     "IntelligentSearch": {
       "ServiceKey": "<your service key>",
       "ServiceEndPoint": "https://api.cognitive.microsoft.com/bing/v7.0",
       "Name": "WebApplicationCore-Search_IntelligentSearch"
     }
   }
   ```
 
## <a name="use-the-bing-news-search-api-to-add-search-functionality-to-a-web-page"></a>Bing haber arama API'si arama işlevselliği bir web sayfasına eklemek için kullanın

Bing haber arama API'si için destek projenize ekledikten sonra API'SİNİN akıllı arama bir web sayfasına eklemek için nasıl kullanılacağı aşağıda verilmiştir.

1.  İçinde *Startup.cs*, `ConfigureServices` yöntemine bir çağrı ekleyin `IServiceCollection.AddSingleton`. Bu kod projenizde kullanılabilen anahtar ayarlarını içeren bir yapılandırma nesnesi getirir.
 
   ```csharp
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddMvc();
            services.AddSingleton<IConfiguration>(Configuration);
        }
   ```


1. Altında yeni bir sınıf dosyası ekleyin **modelleri** adlı klasörü *BingNewsModel.cs*. Projeniz farklı adlandırılan MyWebApplication yerine kendi projenin ad alanını kullanın. İçeriğini aşağıdaki kodla değiştirin:
 
    ```csharp
    using Microsoft.Azure.CognitiveServices.Search.NewsSearch.Models;
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    
    namespace MyWebApplication.Models
    {
        public class BingNewsModel
        {
            public News SearchResult { get; set; } 
            public string SearchText { get; set; }
        }
    }
    ```

   Bu model, Bing haber arama hizmeti çağrısı sonuçlarını depolamak için kullanılır.
 
1. İçinde **denetleyicileri** klasör adında yeni bir sınıf dosyası ekleyin *IntelligentSearchController.cs*. İçeriğini aşağıdaki kodla değiştirin:

   ```csharp
    using System.Net.Http;
    using System.Threading.Tasks;
    using MyWebApplication.Models;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Azure.CognitiveServices.Search.NewsSearch;
    using Microsoft.Extensions.Configuration;
    
    namespace MyWebApplication.Controllers
    {
        // A controller to handle News Search requests
        public class IntelligentSearchController : Controller
        {
            private IConfiguration configuration;
  
            // Set up the configuration that contains the keys
            // (from the appsettings.json file)
            // that you will use to access the service  
            public IntelligentSearchController(IConfiguration configuration)
            {
                this.configuration = configuration;
            }

            // Call the Bing News Search API and put the result in the model object.    
            public async Task<IActionResult> BingSearchResult(BingNewsModel model)
            {
                if (!string.IsNullOrWhiteSpace(model.SearchText))
                { 
                    INewsSearchAPI client = this.GetNewsSearchClient(new MyHandler());
                    model.SearchResult = await client.News.SearchAsync(model.SearchText);
                }
                return View(model);
            }
    
            // Forward requests to the Search endpoint to the BingSearchResult method
            [HttpPost("Search")]
            public IActionResult Search(BingNewsModel model)
            {
                return RedirectToAction("BingSearchResult", model);
            }

            // Get the search client object
            private INewsSearchAPI GetNewsSearchClient(DelegatingHandler handler)
            {
                string key =
                   configuration.GetSection("CognitiveServices")["IntelligentSearch:ServiceKey"];
    
                INewsSearchAPI client = new NewsSearchAPI(
                   new ApiKeyServiceClientCredentials(key), handlers: handler);
    
                return client;
            }
        }
    }
   ```

   Bu kodda Oluşturucu anahtarlarınızı içeren yapılandırma nesne ayarlar. Yöntemi için `Search` yoldur yalnızca bir yeniden yönlendirme `BingSearchResult` işlevi. Bu çağrı `GetNewsSearchClient` almak için yöntemi `NewsSearchAPI` istemci nesnesi.  `NewsSearchAPI` İstemci nesnesini içeren `SearchAsync` gerçekten hizmetini çağıran ve sonuçları yöntemi `SearchResult` yeni oluşturduğunuz modeli. 

1. Bir sınıf ekleyin `MyHandler`, önceki kodda başvuruldu. Bu zaman uyumsuz çağrı temel sınıfı olan arama hizmetinizin Temsilciler `DelegatingHandler`.

   ```csharp
    using System.Net.Http;
    using System.Threading.Tasks;
    using System.Threading;

    class MyHandler : DelegatingHandler
    {
        protected async override Task<HttpResponseMessage> SendAsync(
        HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Call the inner handler.
            var response = await base.SendAsync(request, cancellationToken);
            
            return response;
        }
    }
   ```

1. Arama göndermek ve sonuçları görüntüleme için destek eklemek üzere **görünümleri** klasöründe adlı yeni bir klasör oluşturun **IntelligentSearch**. Bu yeni klasör görünümü ekleme *BingSearchResult.cshtml*. Aşağıdaki kodu kopyalayın:

    ```cshtml
    @using System
    @model MyWebApplication.Models.BingNewsModel
    
    @{
        ViewData["Title"] = "BingSearchResult";
    }
    
    <h2>Search News</h2>
    
    <div class="row">
        <section>
            <form asp-controller="IntelligentSearch" asp-action="Search" method="POST"
                  class="form-horizontal" enctype="multipart/form-data">
                <table width ="90%">
                    <tr>
                        <td>
                            <input type="text" name="SearchText" class="form-control" />
                        </td>
                        <td>
                            <button type="submit" class="btn btn-default">Search</button>
                        </td>
                    </tr>
                </table>
            </form>
        </section>
    </div>
    <h2>Search Result</h2=
    <table>
    @if (!string.IsNullOrEmpty(Model.SearchText)) {
        foreach (var item in Model.SearchResult.Value) {
        <tr>
            <td rowspan="2" width="90">
               <img src=@item?.Image?.Thumbnail?.ContentUrl width="80" height="80" />
            </td>
            <td><a href=@item.Url>@item.Name</a></td>
        </tr>   
        <tr>
            <td>@item.Description</td>
        </tr>
        <tr height="10">
            <td/><td/>
         </tr>
        } }
     </table>
    <div>
        <hr />
        <p>
            <a asp-controller="Home" asp-action="Index">Return to Index</a>
        </p>
    </div>
    ```

1. Web uygulamasını yerel olarak başlatın, yalnızca oluşturulan (/ IntelligentSearch/BingSearchResult) ve arama isteği arama düğmesini kullanarak gönderin sayfanın URL'sini girin.

   ![Ekran Bing haber arama sonuçları](media/vs-bing-news-search-connected-service/Cog-News-Search-Results.PNG)
           
## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubu artık gerekli olmadığında silebilirsiniz. Bu bilişsel hizmet ve ilişkili kaynakları siler. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki arama kutusuna kaynak grubunuzun adını girin. Silmek istediğiniz kaynak grubunu seçin.
2. **Kaynak grubunu sil**'i seçin.
3. İçinde **kaynak grubu adını yazın** kutusuna kaynak grubunun adını girin ve seçin **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bing haber arama API'si hakkında daha fazla bilgi için bkz: [Bing haber arama nedir?](index.yml).
