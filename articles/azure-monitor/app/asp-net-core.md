---
title: ASP.NET Core için Azure Application Insights | Microsoft Docs
description: Kullanılabilirlik, performans ve kullanım için ASP.NET Core web uygulamalarını izleyin.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/03/2018
ms.author: mbullwin
ms.openlocfilehash: 934d0a73bf6345edd79ae00526a1db0361b3524d
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58482792"
---
# <a name="application-insights-for-aspnet-core"></a>ASP.NET Core için Application Insights

Azure Application Insights, web uygulamanıza kod düzeyine kapsamlı izleme sağlar. Web uygulamanızın kullanılabilirliğini, performansını ve kullanımını kolayca izleyebilir. Ayrıca, bir kullanıcının bildirmesini beklemeden uygulamanızdaki hataları hızlıca tanımlayıp tespit edebilirsiniz.

Bu makalede örnek bir ASP.NET Core oluşturma adımlarında size kılavuzluk eder [Razor sayfaları](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) Visual Studio'da uygulama. Ayrıca, Application Insights'ı kullanarak izlemeye nasıl başlayabileceğinizi gösterir.

## <a name="prerequisites"></a>Önkoşullar

- .NET core 2.0.0 SDK veya üzeri
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7.3 sürümü veya üzeri, ASP.NET ve web geliştirme iş yüküyle birlikte sağlanır

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Visual Studio'da bir ASP.NET Core projesi oluşturma

1. Sağ **Visual Studio 2017**ve ardından **yönetici olarak çalıştır**.
2. Seçin **dosya** > **yeni** > **proje** (Ctrl + Shift + N).

   ![Visual Studio'nun ekran yeni proje menüsü](./media/asp-net-core/001-new-project.png)

3. **Visual C#** seçeneğini genişletin. Seçin **.NET Core** > **ASP.NET Core Web uygulaması**. Bir proje adı ve bir çözüm adı girin ve ardından **yeni Git deposu Oluştur**.

   ![Visual Studio'nun ekran Yeni Proje Sihirbazı](./media/asp-net-core/002-asp-net-core-web-application.png)

4. Seçin **.NET Core** > **ASP.NET Core 2.0** **Web uygulaması** > **Tamam**.

    ![Visual Studio'nun ekran yeni proje şablonu seçimi](./media/asp-net-core/003-web-application.png)

## <a name="application-insights-search"></a>Application Insights Search

Visual Studio 2015 güncelleştirme 2 veya sonraki bir ASP.NET Core 2 + tabanlı proje ile avantajlarından yararlanabilirsiniz [Application Insights arama](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio), hatta açıkça Application ınsights'ı projenize eklemeden önce.

Bu işlevi test etmek için:

1. Uygulamanızı çalıştırın. Uygulamanızı çalıştırmak için seçin **IIS Express** simgesi (![ekran görüntüsü, Visual Studio IIS Express simgesi](./media/asp-net-core/004-iis-express.png)).

2. Seçin **görünümü** > **diğer Windows** > **Application Insights arama**.

   ![Visual Studio'nun ekran tanılama araçları seçimi](./media/asp-net-core/005-view-other-windows-search.png)

3. Şu anda hata ayıklama oturumu telemetrisi yalnızca yerel analiz için kullanılabilir. Application ınsights'ı tam olarak etkinleştirmek için işaretleyin **Telemetri hazırlık** sonraki bölümde listelenen adımları üst sağ alt köşesinde veya tamamlandı.

   ![Arama Visual Studio Application Insights'ın ekran görüntüsü](./media/asp-net-core/006-search.png)

> [!NOTE]
> Visual Studio özellikleri javascript'iniz nasıl gibi hakkında daha fazla bilgi edinmek için [Application Insights arama](../../azure-monitor/app/visual-studio.md) ve [CodeLens](../../azure-monitor/app/visual-studio-codelens.md) yerel ASP.NET Core projenize Application Insights eklemeden önce bkz [ Application Insights arama devam](#application-insights-search-continued).

## <a name="add-application-insights-telemetry"></a>Application Insights Telemetrisi ekleme

1. Seçin **proje** > **Application Insights Telemetrisi Ekle**. (Veya sağ tıklayabilirsiniz **bağlı hizmetler**ve ardından **bağlı hizmet Ekle**.)

    ![Visual Studio'nun ekran yeni proje seçimi menüsü](./media/asp-net-core/007-project-add-telemetry.png)

2. Seçin **başlama**. (Visual Studio sürümüne bağlı olarak, metin biraz farklı olabilir. Bazı daha önceki sürümlerde bir **ücretsiz Başlat** yerine düğme.)

    ![Ekran görüntüsü, Application Insights başlama düğmesi](./media/asp-net-core/008-get-started.png)

3. Aboneliğinizi seçin ve ardından **kaynak** > **kaydetme**.

## <a name="changes-made-to-your-project"></a>Projeniz için yapılan değişiklikler

Application Insights, düşük ek yükleri. Projenize Application Insights telemetri ekleme tarafından yapılan değişiklikleri gözden geçirmek için:

Seçin **görünümü** > **Takım Gezgini** (Ctrl +\, Ctrl + M) > **proje** > **değişiklikleri**

- Dört toplam değişiklikler görünür:

  ![Application Insights'ı ekleyerek değiştirilen dosya görüntüsü](./media/asp-net-core/009-changes.png)

- Yeni bir dosya oluşturulur:

  - _ConnectedService.json_

    ```json
    {
     "ProviderId": "Microsoft.ApplicationInsights.ConnectedService.ConnectedServiceProvider",
     "Version": "8.12.10405.1",
     "GettingStartedDocument": {
       "Uri": "https://go.microsoft.com/fwlink/?LinkID=798432"
     }
    }
    ```

- Üç dosyayı (değişiklikleri vurgulamak için eklenen ek açıklamalar) değiştirilir:

  - _appsettings.json_:

    ```json
    {
      "Logging": {
        "IncludeScopes": false,
        "LogLevel": {
          "Default": "Warning"
        }
      },
    // Changes to file post adding Application Insights Telemetry:
      "ApplicationInsights": {
        "InstrumentationKey": "10101010-1010-1010-1010-101010101010"
      }
    }
    //
    ```

  - _ContosoDotNetCore.csproj_:

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
    <!--Changes to file post adding Application Insights Telemetry:-->
        <ApplicationInsightsResourceId>/subscriptions/2546c5a9-fa20-4de1-9f4a-62818b14b8aa/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/DotNetCore</ApplicationInsightsResourceId>
        <ApplicationInsightsAnnotationResourceId>/subscriptions/2546c5a9-fa20-4de1-9f4a-62818b14b8aa/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/DotNetCore</ApplicationInsightsAnnotationResourceId>
    <!---->
      </PropertyGroup>
      <ItemGroup>
    <!--Changes to file post adding Application Insights Telemetry:-->
        <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.1.1" />
    <!---->
        <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.8" />
      </ItemGroup>
      <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.4" />
      </ItemGroup>
    <!--Changes to file post adding Application Insights Telemetry:-->
      <ItemGroup>
        <WCFMetadata Include="Connected Services" />
      </ItemGroup>
    <!---->
    </Project>
    ```

  -  _Program.cs_:

      ```csharp
      using System;
      using System.Collections.Generic;
      using System.IO;
      using System.Linq;
      using System.Threading.Tasks;
      using Microsoft.AspNetCore;
      using Microsoft.AspNetCore.Hosting;
      using Microsoft.Extensions.Configuration;
      using Microsoft.Extensions.Logging;

      namespace DotNetCore
      {
          public class Program
          {
              public static void Main(string[] args)
              {
                  BuildWebHost(args).Run();
              }

              public static IWebHost BuildWebHost(string[] args) =>
                  WebHost.CreateDefaultBuilder(args)
      // Change to file post adding Application Insights Telemetry:
                      .UseApplicationInsights()
      //
                      .UseStartup<Startup>()
                      .Build();
          }
      }
      ```

## <a name="send-ilogger-logs-to-application-insights"></a>Application Insights'a ILogger günlükleri gönderme

Application Insights ILogger gönderilen yakalama günlükleri destekler. Kod örnekleri günlük kullanıma alma kurmaya [burada](https://docs.microsoft.com/azure/azure-monitor/app/ilogger).

## <a name="synthetic-transactions-with-powershell"></a>PowerShell ile yapay işlemler

Yapay işlemleri kullanarak uygulamanızı isteklerine otomatik hale getirmek için:

1. Uygulamanızı çalıştırmak için seçin ![Ekran görüntüsü, Visual Studio IIS Express simgesi](./media/asp-net-core/004-iis-express.png) simge.

2. Tarayıcınızın adres çubuğundan URL'yi kopyalayın. URL'si şu biçimdedir `http://localhost:<port number>`.

   ![Tarayıcı adres çubuğuna URL'yi ekran görüntüsü](./media/asp-net-core/0013-copy-url.png)

3. Test uygulamanızı kullanarak 100 yapay işlemler oluşturmak için aşağıdaki PowerShell döngü çalıştırın. Sonra bağlantı noktası numarasını değiştirin `localhost:` önceki adımda kopyaladığınız URl ile eşleşmek üzere. Örneğin:

   ```powershell
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-the-application-insights-portal"></a>Application Insights portalında açın

Önceki bölümde PowerShell komutları çalıştırdıktan sonra işlemleri görüntülemek ve veri toplanmakta olan onaylamak için Application Insights'ı açın. 

Visual Studio menüde **proje** > **Application Insights** > **Application Insights portalını Aç**.

   ![Application Insights'a genel bakış görüntüsü](./media/asp-net-core/010-portal.png)

> [!NOTE]
> Yukarıdaki örnek ekran görüntüsünde **Canlı Stream**, **sayfa görüntüleme yükleme süresi**, ve **başarısız istekler** toplanan değildir. Sonraki bölümde her birini ekleme adımlarında size kılavuzluk eder. Zaten topluyorsanız **Canlı Stream** ve **sayfa görüntüleme yükleme süresi**, yalnızca için adımları tamamlayın **başarısız istekler**.

## <a name="collect-failed-requests-live-stream-and-page-view-load-time"></a>Başarısız istekler, Canlı Stream ve sayfa görüntüleme yükleme süresi toplama

### <a name="failed-requests"></a>Başarısız İstekler

Teknik olarak, başarısız olan istekleri toplanır, ancak hiç başarısız istek henüz oluşmuş. İşlemi hızlandırmak için bir özel durum gerçek özel durum benzetimini yapmak için varolan bir projeye ekleyebilirsiniz. Devam etmeden önce Visual Studio'da, uygulama çalışırken, seçin **hata ayıklamayı Durdur** (Shift + F5).

1. İçinde **Çözüm Gezgini**, genişletme **sayfaları** > **About.cshtml**ve ardından açın *About.cshtml.cs*.

   ![Visual Studio çözüm Gezgini'nin ekran görüntüsü](./media/asp-net-core/011-about.png)

2. Bir özel durum altında eklemek ``Message=``ve ardından değişiklikleri dosyaya kaydedin.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc.RazorPages;

    namespace DotNetCore.Pages
    {
        public class AboutModel : PageModel
        {
            public string Message { get; set; }

            public void OnGet()
            {
                Message = "Your application description page.";
                throw new Exception("Test Exception");
            }
        }
    }
    ```

### <a name="live-stream"></a>Canlı Akış

Application Insights ile ASP.NET Core Canlı Stream işlevlerini erişmeye Microsoft.ApplicationInsights.AspNetCore 2.2.0 NuGet paketlerini güncelleştirin.

Visual Studio'da **proje** > **NuGet paketlerini Yönet** > **Microsoft.ApplicationInsights.AspNetCore** > (sürüm) **2.2.0** > **güncelleştirme**.

  ![NuGet Paket Yöneticisi'nin ekran görüntüsü](./media/asp-net-core/012-nuget-update.png)

Birden çok onay istemlerini görünür. Okuyun ve değişiklikleri kabul etmiyorsanız kabul edin.

### <a name="page-view-load-time"></a>Sayfa görünümü yükleme süresi

1. Visual Studio'da Git **Çözüm Gezgini** > **sayfaları**. İki dosyayı değiştirmeniz gerekir: *Layout.cshtml* ve *ViewImports.cshtml*.

2. İçinde *ViewImports.cshtml*, bu kodu ekleyin:

   ```csharp
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```

3. İçinde *Layout.cshtml*, önce aşağıdaki kodu ekleyin ``</head>`` etiketi ve tüm diğer komut dosyalarını önce:

    ```csharp
    @Html.Raw(snippet.FullScript)
    ```

### <a name="test-failed-requests-page-view-load-time-and-live-stream"></a>Test başarısız istekler, sayfa görünümü yükleme süresi ve Stream Canlı

Test ve her şeyin çalıştığını doğrulamak için:

1. Uygulamanızı çalıştırın. Uygulamanızı çalıştırmak için seçin ![Ekran görüntüsü, Visual Studio IIS Express simgesi](./media/asp-net-core/004-iis-express.png) simge.

2. Git **hakkında** test özel durum tetiklemek için sayfa. (Visual Studio'da hata ayıklama modunda iseniz seçin **devam** için Application Insights'da gösterilmesi özel durum.)

3. Daha önce kullandığınız sanal PowerShell işlem betiğini yeniden çalıştırın. (Komut bağlantı noktası numarasını ayarlamak gerekebilir.)

4. Varsa **genel bakış** uygulamaları İçgörüler sayfasında hala değilse Visual Studio menüde açık **proje** > **Application Insights**  >  **Application Insights portalını Aç**. 

   > [!TIP]
   > Yeni trafiğiniz görmüyorsanız, değerini denetleyin **zaman aralığı**ve ardından **Yenile**.

   ![Genel Bakış penceresinin ekran görüntüsü](./media/asp-net-core/0019-overview-updated.png)

5. Seçin **Canlı Stream**.

   ![Canlı ölçümleri Stream ekran görüntüsü](./media/asp-net-core/0020-live-metrics-stream.png)

   (PowerShell betiğinizi hala çalışıyorsa, Canlı ölçümleri görmeniz gerekir. PowerShell komut dosyanızı çalışmayı durdurdu, betiği yeniden Canlı Stream açıkken çalıştırın.)

## <a name="application-insights-sdk-comparison"></a>Application Insights SDK'sı karşılaştırma

Application Insights ürün grubu arasında özellik denkliğini sağlamamız sabit çalışmaktadır [tam .NET Framework SDK](https://github.com/Microsoft/ApplicationInsights-dotnet) ve .NET Core SDK. 2.2.0 sürüm [ASP.NET Core SDK'sı](https://github.com/Microsoft/ApplicationInsights-aspnetcore) için Application Insights özellik farkı büyük ölçüde kapatır.

Aşağıdaki tabloda farklar daha fazla açıklar ve arasında denge [.NET ve .NET Core](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server):

   | SDK karşılaştırması | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Canlı ölçümleri**      | **+** |**-** | **+** |
   | **Sunucu telemetri kanalı** | **+** |**-** | **+**|
   |**Uyarlamalı örnekleme**| **+** | **-** | **+**|
   | **SQL bağımlılık çağrıları**     | **+** |**-** | **+**|
   | **Performans sayaçları*** | **+** | **-**| **-**|

Performans sayaçlarını bu bağlamda başvurduğu [sunucu tarafı performans sayaçları](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) işlemci, bellek ve disk kullanımı gibi.

## <a name="open-source-sdk"></a>Açık kaynak SDK'sı
[Okuma ve koda katkıda](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates).

## <a name="application-insights-search-continued"></a>Application Insights arama devamı

Bu bölümde, Application Insights arama Visual Studio'daki bir ASP.NET Core 2 projesi için nasıl çalıştığını daha iyi anlamanıza yardımcı olabilir. Hatta, açıkça Application Insights NuGet paketleri henüz yüklemediyseniz, bu şekilde çalışır. Ayrıca hata ayıklama çıkışını incelemek yararlı olabilir.

Çıkış sözcük için arama yaparsanız _Insight_, aşağıdakine benzer sonuçlar vurgulanır:

```DebugOutput
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.aspnetcore.applicationinsights.hostingstartup\2.0.3\lib\netcoreapp2.0\Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll'.
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.applicationinsights.aspnetcore\2.1.1\lib\netstandard1.6\Microsoft.ApplicationInsights.AspNetCore.dll'.

Application Insights Telemetry (unconfigured): {"name":"Microsoft.ApplicationInsights.Dev.Message","time":"2018-06-03T17:32:38.2796801Z","tags":{"ai.location.ip":"127.0.0.1","ai.operation.name":"DEBUG /","ai.internal.sdkVersion":"aspnet5c:2.1.1","ai.application.ver":"1.0.0.0","ai.cloud.roleInstance":"CONTOSO-SERVER","ai.operation.id":"de85878e-4618b05bad11b5a6","ai.internal.nodeName":"CONTOSO-SERVER","ai.operation.parentId":"|de85878e-4618b05bad11b5a6."},"data":{"baseType":"MessageData","baseData":{"ver":2,"message":"Request starting HTTP/1.1 DEBUG http://localhost:53022/  0","severityLevel":"Information","properties":{"AspNetCoreEnvironment":"Development","Protocol":"HTTP/1.1","CategoryName":"Microsoft.AspNetCore.Hosting.Internal.WebHost","Host":"localhost:53022","Path":"/","Scheme":"http","ContentLength":"0","DeveloperMode":"true","Method":"DEBUG"}}}}
```

Çıkışta, CoreCLR iki derlemeleri yükler: 

- _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_
- _Microsoft.ApplicationInsights.AspNetCore.dll_.

_Yapılandırılmamış_ her bir Application Insights telemetri örneğini başvurusu gösteren bu uygulama bir ikey ile ilişkili değil. Uygulamanız çalıştığı sırada oluşturulan verileri Azure'a gönderilmez. Veriler, yalnızca yerel arama ve analiz için kullanılabilir.

İşlevleri kısmen mümkün olduğundan NuGet paketini _Microsoft.AspNetCore.All_ alır [ _Microsoft.ASPNetCoreApplicationInsights.HostingStartup_ ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.applicationinsights.hostingstartup.applicationinsightshostingstartup?view=aspnetcore-2.1) bir bağımlılık olarak.

![NuGet ekran Microsoft.AspNETCore.all için bağımlılık grafiği](./media/asp-net-core/013-dependency.png)

Siz bir ASP.NET Core projesi VSCode veya başka bir düzenleyicide, düzenleme, projenize Application Insights açıkça eklemediyseniz, Visual Studio'nun dışında bu derlemeleri otomatik olarak hata ayıklama sırasında yük mıydı.

Ancak, Visual Studio'da ayarlamak bu aydınlatma dış derlemelerden yerel Application Insights özellikleri kullanılarak elde edilir [Ihostingstartup arabirimi](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1). Arabirim, hata ayıklama sırasında Application Insights dinamik olarak ekler.

Uygulama geliştirme hakkında daha fazla bilgi bir [Ihostingstartup ile ASP.NET Core dış derlemede](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/platform-specific-configuration?view=aspnetcore-2.1). 

### <a name="disable-application-insights-in-visual-studio-net-core-projects"></a>Application Insights, Visual Studio .NET Core projelerinde devre dışı bırak

Otomatik ışık yukarı Application ınsights arama rağmen kafa karıştırıcı beklenirken uygulamasında hata ayıklama telemetri oluşturulan görmekten işlevselliği yararlı olabilir.

Yalnızca telemetri oluşturma devre dışı bırakma yeterli ise, bu kod bloğu için ekleyebilirsiniz **yapılandırma** yöntemi, _Startup.cs_ dosyası:

```csharp
  var configuration = app.ApplicationServices.GetService<Microsoft.ApplicationInsights.Extensibility.TelemetryConfiguration>();
            configuration.DisableTelemetry = true;
            if (env.IsDevelopment())
```

CoreCLR hala yükler _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_ ve _Microsoft.ApplicationInsights.AspNetCore.dll_, ancak dosyalar hiçbir şey yapmaz.

Application Insights, Visual Studio .NET Core projenizde tamamen devre dışı bırakmak isterseniz, tercih edilen yöntem seçmektir **Araçları** > **seçenekleri**  >   **Projeler ve çözümler** > **Web projeleri**. Seçin **ASP.NET Core web projeleri için yerel Application Insights'ı devre dışı** onay kutusu. Bu işlev, Visual Studio 15.6 sürümünde eklendi.

![Ekran görüntüsü, Visual Studio Seçenekleri penceresini Web projelerini ekranı](./media/asp-net-core/014-disable.png)

Visual Studio'nun önceki bir sürümü çalıştırıyorsanız ve tamamen aracılığıyla yüklenmiş tüm derlemeleri kaldırmak istediğiniz *Ihostingstartup*, iki seçeneğiniz vardır:

* Ekleme `.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")` için _Program.cs_:

  ```csharp
  using System;
  using System.Collections.Generic;
  using System.IO;
  using System.Linq;
  using System.Threading.Tasks;
  using Microsoft.AspNetCore;
  using Microsoft.AspNetCore.Hosting;
  using Microsoft.Extensions.Configuration;
  using Microsoft.Extensions.Logging;

  namespace DotNetCore
  {
      public class Program
      {
          public static void Main(string[] args)
          {
              BuildWebHost(args).Run();
          }

          public static IWebHost BuildWebHost(string[] args) =>
              WebHost.CreateDefaultBuilder(args)
                  .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
                  .UseStartup<Startup>()
                  .Build();
      }
  }
  ```

* Ekleme ``"ASPNETCORE_preventHostingStartup": "True"`` için _launchSettings.json_ ortam değişkenleri.

Bu yöntemlerden birini kullanarak sorunu bunlar yalnızca Application ınsights'ı devre dışı bırakmadığımızdan emin olan. Yöntemleri kullanan Visual Studio'da herhangi bir şey devre dışı *Ihostingstartup* ışık yukarı işlevselliği.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Sonraki adımlar
* [Kullanıcı akışları keşfedin](../../azure-monitor/app/usage-flows.md) için kullanıcıların uygulamanızda nasıl gezindiğini anlayın.
* [Anlık görüntü koleksiyonunu yapılandırma](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications) bir özel durum şu anda kaynak kodu ve değişkenleri durumunu görmek için.
* [API'yi kullanmak](../../azure-monitor/app/api-custom-events-metrics.md) kendi olayları ve ölçümleri daha ayrıntılı bir görünüm, uygulamanızın performans ve kullanım için gönderilecek.
* Kullanım [kullanılabilirlik testleri](../../azure-monitor/app/monitor-web-app-availability.md) uygulamanızdan sürekli olarak dünyanın dört bir yanındaki denetlemek için.
