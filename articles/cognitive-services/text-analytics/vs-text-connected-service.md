---
title: Metin analizi C# Öğreticisi | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Bir ASP.NET Core web uygulamasından metin Analizine bağlanın.
services: cognitive-services
author: ghogen
manager: douge
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 06/01/2018
ms.author: ghogen
ms.openlocfilehash: c97f75e0a41a4bf314963dc26c6424a0b773822b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38665237"
---
# <a name="connect-to-the-text-analytics-service-by-using-connected-services-in-visual-studio"></a>Visual Studio bağlı Hizmetler'i kullanarak metin analizi hizmete bağlanın

Metin analiz hizmetini kullanarak, görsel verilerin işlenmesi ve kategorilere ayırma için zengin bilgiler ayıklayın ve görüntüleri hizmetlerinizi oluşturmanıza yardımcı olması için makine Yardımlı resim denetimi gerçekleştirin.

Bu makalede ve yardımcı makaleleri için metin analiz hizmeti Visual Studio bağlı hizmeti özelliği kullanmak için ayrıntıları sağlayın. Özellik, her iki Visual Studio 2017 15.7 veya sonraki sürümlerinde, Bilişsel hizmetler uzantısı yüklü yöneliktir.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
- Visual Studio 2017 sürüm 15.7, yüklü Web geliştirme iş yüküyle birlikte sağlanır. [Hemen indirin](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="add-support-to-your-project-for-the-text-analytics-service"></a>Destek için metin analizi hizmetini projenize ekleyin.

1. TextAnalyticsDemo adlı yeni bir ASP.NET Core web projesi oluşturun. Web uygulaması (Model-View-Controller) proje şablonu tüm varsayılan ayarlarla birlikte kullanın. Kod projesine kopyaladığınızda, ad alanı eşleşecek şekilde MyWebApplication, proje adı önemlidir.  Bu makaleler örnekte MVC kullanır, ancak herhangi bir ASP.NET proje türünü metin analizi bağlı hizmetini kullanabilirsiniz.

1. İçinde **Çözüm Gezgini**, çift tıklayarak **bağlı hizmet** öğesi.
   Projenize eklediğiniz Hizmetleri ile bağlı hizmet sayfasında görünür.

   ![Bağlı hizmet, Çözüm Gezgini ekran görüntüsü](../media/vs-common/Connected-Services-Solution-Explorer.PNG)

1. Kullanılabilir hizmetler menüsünde **ile metin analizi yaklaşım değerlendirmek**.

   ![Bağlı hizmetler ekran ekranı](./media/vs-text-connected-service/Cog-Text-Connected-Service-0.PNG)

   Visual Studio'da oturum açıldıktan ve hesabınızla ilişkili bir Azure aboneliğiniz varsa, aboneliklerinizin ile bir açılan listedeki bir sayfa görünür.

   ![Ekran, metin analizi bağlı hizmet ekranı](media/vs-text-connected-service/Cog-Text-Connected-Service-1.PNG)

1. Kullanın ve metin analizi hizmeti için bir ad seçin veya seçmek istediğiniz aboneliği seçin **Düzenle** bağlantı otomatik olarak oluşturulan adı değiştirmek, kaynak grubunu ve fiyatlandırma katmanı seçin.

   ![Kaynak grubu ve fiyatlandırma katmanı alanları ekran görüntüsü](media/vs-text-connected-service/Cog-Text-Connected-Service-2.PNG)

   Fiyatlandırma katmanları hakkında ayrıntıları bağlantısını izleyin.

1. Seçin **Ekle** bağlı hizmeti için destek eklemek için.
   Visual Studio projenizin NuGet paketlerini, yapılandırma dosyası girdisi ve metin analizi hizmetine bağlantıyı desteklemek üzere diğer değişiklikler eklemek için değiştirir. **Çıkış penceresine** projenize olup bitenleri günlük gösterir. Aşağıdaki gibi görmeniz gerekir:

   ```output
    [6/1/2018 3:04:02.347 PM] Adding Text Analytics to the project.
    [6/1/2018 3:04:02.906 PM] Creating new Text Analytics...
    [6/1/2018 3:04:06.314 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Language' version 1.0.0-preview...
    [6/1/2018 3:04:56.759 PM] Retrieving keys...
    [6/1/2018 3:04:57.822 PM] Updating appsettings.json setting: 'ServiceKey' = '<service key>'
    [6/1/2018 3:04:57.827 PM] Updating appsettings.json setting: 'ServiceEndPoint' = 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0'
    [6/1/2018 3:04:57.832 PM] Updating appsettings.json setting: 'Name' = 'TextAnalyticsDemo'
    [6/1/2018 3:05:01.840 PM] Successfully added Text Analytics to the project.
    ```
 
## <a name="use-the-text-analytics-service-to-detect-the-language-for-a-text-sample"></a>Metin örneği dili algılamak için metin analizi hizmetini kullanın.

1. Aşağıdaki using deyimlerini Startup.cs içinde.
 
   ```csharp
   using System.IO;
   using System.Net.Http;
   using System.Net.Http.Headers;
   using System.Text;
   using Microsoft.Extensions.Configuration;
   ```
 
1. Yapılandırma alanı ekleyin ve başlangıç sınıfı programınızda yapılandırmasını etkinleştirmek için yapılandırma alanı başlatan bir oluşturucu ekleyin.

   ```csharp
      private IConfiguration configuration;

      public Startup(IConfiguration configuration)
      {
          this.configuration = configuration;
      }
   ```

1. Denetleyicileri klasörüne DemoTextAnalyzeController adlı bir sınıf dosyası ekleyin ve içeriğini aşağıdaki kodla değiştirin:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    using Microsoft.Extensions.Configuration;
    using TextAnalyticsDemo.Models;
    
    namespace TextAnalyticsDemo.Controllers
    {
        public class DemoTextAnalyzeController : Controller
        {
            private IConfiguration configuration;
    
            public DemoTextAnalyzeController(IConfiguration configuration)
            {
                this.configuration = configuration;
            }
    
            public IActionResult TextAnalyzeResult(TextAnalyzeModel model)
            {
    
                if (!string.IsNullOrWhiteSpace(model.TextStr))
                {
                    ITextAnalyticsAPI client = this.GetTextAnalyzeClient(new MyHandler());
                    model.AnalyzeResult = client.DetectLanguage(
                        new BatchInput(
                            new List<Input>()
                            {
                                new Input("id",model.TextStr)
                            }));
                }
                return View(model);
            }
    
            [HttpPost("Analyze")]
            public IActionResult Analyze(TextAnalyzeModel model)
            {
                return RedirectToAction("TextAnalyzeResult", model);
            }
    
            // Using the ServiceKey from the configuration file,
            // get an instance of the Text Analytics client.
            private ITextAnalyticsAPI GetTextAnalyzeClient(DelegatingHandler handler)
            {
                string key = configuration.GetSection("CognitiveServices")["TextAnalytics:ServiceKey"];
    
                ITextAnalyticsAPI client = new TextAnalyticsAPI( handlers: handler);
                client.SubscriptionKey = key;
                client.AzureRegion = AzureRegions.Westus;
    
                return client;
            }
        }
    }
    ```
    
    İstemci, metin analizi API'sini çağırmak için kullanabileceğiniz nesne ve belirli bir metni DetectLanguage çağıran bir istek işleyicisi almak için GetTextAnalyzeClient kodu içerir.

1. Yukarıdaki kod tarafından kullanılan MyHandler yardımcı sınıfı ekleyin.

    ```csharp
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

1. Modelleri klasöründe modeli için bir sınıf ekleyin.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    
    namespace Demo.Models
    {
        public class TextAnalyzeModel
        {
            public string TextStr { get; set; }
    
            public LanguageBatchResult AnalyzeResult { get; set; }
    
            public SentimentBatchResult AnalyzeResult2 { get; set; }
        }
    }
    ```

1. Çözümlenmiş metin ve belirlenen dil analizi güvenirlik düzeyini temsil eden puanı gösteren görünüm ekleyin.
    
    ```cshtml
    @using System
    @model Demo.Models.TextAnalyzeModel
    
    @{
        ViewData["Title"] = "TextAnalyzeResult";
    }
    
    <h2>Text Language</h2>
    
    <div class="row">
        <section>
            <form asp-controller="DemoTextAnalyze" asp-action="Analyze" method="POST"
                  class="form-horizontal" enctype="multipart/form-data">
                <table width="90%">
                    <tr>
                        <td>
                            <input type="text" name="TextStr" class="form-control" />
                        </td>
                        <td>
                            <button type="submit" class="btn btn-default">Analyze</button>
                        </td>
                    </tr>
                </table>
            </form>
        </section>
    </div>
    
    <h2>Result</h2>
    <div>
        <dl class="dl-horizontal">
            <dt>
                Text :
            </dt>
            <dd>
                @Html.DisplayFor(model => model.TextStr)
            </dd>
            <dt>
                Language Name :
            </dt>
            <dd>
                @Html.DisplayFor(model => model.AnalyzeResult.Documents[0].DetectedLanguages[0].Name)
            </dd>
            <dt>
                Score :
            </dt>
            <dd>
                @Html.DisplayFor(model => model.AnalyzeResult.Documents[0].DetectedLanguages[0].Score)
            </dd>
        </dl>
    </div>
    <div>
        <hr />
        <p>
            <a asp-controller="Home" asp-action="Index">Return to Index</a>
        </p>
    </div>
    
    ```
 
1. Derleme ve örneği yerel olarak çalıştırın. Bir metin girin ve metin analizi algılar hangi dili bakın.
   
## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu silin. Bu bilişsel hizmet ve ilişkili kaynakları siler. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki Arama kutusuna kaynak grubunuzun adını girin. Bu öğreticide arama sonuçlarında kullanılan kaynak grubunu gördüğünüzde bunu seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** kutusuna kaynak grubunun adını yazın ve **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Metin analizi hizmeti hakkında daha fazla bilgi edinmek [metin analizi hizmeti belgeleri](index.yml).
