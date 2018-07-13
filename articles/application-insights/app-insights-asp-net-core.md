---
title: ASP.NET Core için Azure Application Insights | Microsoft Docs
description: Web uygulamalarının kullanılabilirliğini, performansını ve kullanımını izleyin.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/03/2018
ms.author: mbullwin
ms.openlocfilehash: 6635906b6aae418fa3754f1152fae3e0d8903ffc
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989777"
---
# <a name="application-insights-for-aspnet-core"></a>ASP.NET Core için Application Insights

Azure Application Insights, web uygulamanıza kod düzeyine kapsamlı izleme sağlar. Web uygulamanızın kullanılabilirliğini, performansını ve kullanımını kolayca izleyebilir. Ayrıca, bir kullanıcının bildirmesini beklemeden uygulamanızdaki hataları hızlıca tanımlayıp tespit edebilirsiniz.

Bu makalede, örnek bir ASP.NET Core oluşturma işlemini gösterir. [Razor sayfaları](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) uygulama Visual Studio ve Azure Application Insights ile izleme Başlat öğrenin.

## <a name="prerequisites"></a>Önkoşullar

- NET Core 2.0.0 SDK'sı veya üzeri.
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7.3 sürümü veya üzeri ASP.NET ve web geliştirme iş yüküyle birlikte sağlanır. 

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Visual Studio'da bir ASP.NET Core projesi oluşturma

1. Sağ tıklatın ve Başlat **Visual Studio 2017** yönetici olarak.
2. Seçin **dosya** > **yeni** > **proje** (Ctrl-Shift-N).

   ![Visual Studio dosya yeni proje menüsünün ekran görüntüsü](./media/app-insights-asp-net-core/001-new-project.png)

3. Genişletin **Visual C#** > seçin **.NET Core** > **ASP.NET Core Web uygulaması**. Girin bir **adı** > **çözüm adı** > denetleyin **yeni Git deposu Oluştur**.

   ![Visual Studio dosya yeni proje sihirbazının ekran görüntüsü](./media/app-insights-asp-net-core/002-asp-net-core-web-application.png)

4. Seçin **.Net Core** > **ASP.NET Core 2.0** **Web uygulaması** > **Tamam**.

    ![Visual Studio dosya yeni proje seçimi menüsünün ekran görüntüsü](./media/app-insights-asp-net-core/003-web-application.png)

## <a name="application-insights-search"></a>Application Insights araması

Visual Studio'da varsayılan proje tabanlı 2015 güncelleştirme 2 veya daha büyük bir ASP.NET Core 2 + sürümü avantajlarından yararlanabilirsiniz [Application Insights arama](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio) bile önce açıkça eklemeniz Application ınsights'ı projenize.

Bu işlevleri test etmek için:

1. IIS Express tıklayarak uygulamanızı çalıştırma ![Ekran görüntüsü, Visual Studio IIS Express simgesi](./media/app-insights-asp-net-core/004-iis-express.png)

2. Seçin **görünümü** > **diğer Windows** > **Application Insights arama**.

   ![Visual Studio Tanılama Araçları'nın ekran görüntüsü](./media/app-insights-asp-net-core/005-view-other-windows-search.png)

3. Hata ayıklama oturumu telemetrisi şu anda yalnızca yerel analiz için kullanılabilir. Application ınsights'ı tam olarak etkinleştirmek için işaretleyin **Telemetri hazırlık** üst sağ veya sonraki bölümde yer alan adımları izleyin.

   ![Arama Visual Studio Application Insights'ın ekran görüntüsü](./media/app-insights-asp-net-core/006-search.png)

> [!NOTE]
> Visual Studio özellikleri javascript'iniz nasıl gibi hakkında daha fazla bilgi edinmek için [Application Insights arama](app-insights-visual-studio.md) ve [CodeLens](app-insights-visual-studio-codelens.md) eklediğiniz önce yerel olarak ASP.NET Core projenize Application ınsights'ı gözden geçirin Açıklama en [bu makalenin sonundaki.](#Application-Insights-search-continued)

## <a name="add-application-insights-telemetry"></a>Application Insights Telemetrisi ekleme

1. Seçin **proje** > **Application Insights Telemetrisi Ekle...** . (Veya sağ tıklayabilirsiniz **bağlı hizmetler** ve bağlı hizmet Ekle'yi seçin.)

    ![Visual Studio dosya yeni proje seçimi menüsünün ekran görüntüsü](./media/app-insights-asp-net-core/007-project-add-telemetry.png)

2. Seçin **başlama**. (Visual Studio sürümünüze bağlı olarak metin biraz farklılık gösterebilir. Bunun yerine, bazı eski sürümleri sahip bir **ücretsiz Başlat** düğmesi.)

    ![Visual Studio dosya yeni proje seçimi menüsünün ekran görüntüsü](./media/app-insights-asp-net-core/008-get-started.png)

3. Uygun bir seçin **abonelik** > **kaynak** > **kaydetme**.

## <a name="changes-made-to-your-project"></a>Projenize Made değiştirir

Application Insights, düşük ek yükleri. Projenize Application Insights telemetri ekleme tarafından yapılan değişiklikleri gözden geçirmek için:

Seçin **görünümü** > **Takım Gezgini** (Ctrl +\, Ctrl + M) > **proje** > **değişiklikleri**

- Toplam dört değişiklikler:

  ![Application Insights'ı ekleyerek değiştirilen dosya görüntüsü](./media/app-insights-asp-net-core/009-changes.png)

- Yeni bir dosya oluşturulur:

   _ConnectedService.json_

```json
{
  "ProviderId": "Microsoft.ApplicationInsights.ConnectedService.ConnectedServiceProvider",
  "Version": "8.12.10405.1",
  "GettingStartedDocument": {
    "Uri": "https://go.microsoft.com/fwlink/?LinkID=798432"
  }
}
```

- Üç dosya değiştirildiğinde: (değişiklikleri vurgulamak için eklenen ek açıklamalar)

  _appsettings.json_

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

  _ContosoDotNetCore.csproj_

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

   _Program.cs_

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

## <a name="synthetic-transactions-with-powershell"></a>PowerShell ile yapay işlemler

Yapay işlemler uygulamanızla istekler otomatik hale getirmek için.

1. IIS Express tıklayarak uygulamanızı çalıştırma ![Ekran görüntüsü, Visual Studio IIS Express simgesi](./media/app-insights-asp-net-core/004-iis-express.png)

2. Tarayıcınızın adres çubuğundan URL'yi kopyalayın. Biçimindedir `http://localhost:{random port number}`

   ![Tarayıcı url adres çubuğunun ekran görüntüsü](./media/app-insights-asp-net-core/0013-copy-url.png)

3. 100 yapay işlemler karşı test uygulamanızı oluşturmak için aşağıdaki PowerShell döngü çalıştırın. Sonra bağlantı noktası numarasını değiştirin **localhost:** önceki adımda kopyaladığınız url ile eşleşmek üzere.

   ```PowerShell
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-application-insights-portal"></a>Açık Application Insights portalını

Çalıştırdıktan sonra önceki bölümde PowerShell işlemleri görüntülemek ve veri toplanmakta olan onaylamak için Application ınsights'ı başlatın. 

Visual Studio menüden **proje** > **Application Insights** > **Application Insights portalını Aç**

   ![Application Insights'a genel bakış görüntüsü](./media/app-insights-asp-net-core/010-portal.png)

> [!NOTE]
> Yukarıdaki örnek ekran görüntüsünde **Canlı Stream**, **sayfa görüntüleme yükleme süresi**, ve **başarısız istekler** şu anda toplanmadı. Sonraki bölümde, her eklerken size kılavuzluk edecektir. Zaten topluyorsanız **Canlı Stream**, ve **sayfa görüntüleme yükleme süresi**, yalnızca adımlarını izleyin **başarısız istekler**.

## <a name="collect-failed-requests-live-stream--page-view-load-time"></a>Başarısız istekler, Canlı Stream ve sayfa görüntüleme yükleme süresi toplama

### <a name="failed-requests"></a>Başarısız İstekler

Teknik olarak **başarısız istekler** toplanmakta olan, ancak hiçbiri henüz oluşmuş. Bir özel durum boyunca sürecini hızlandırmak için gerçek bir özel durum benzetimini yapmak için var olan proje eklenebilir. Uygulamanızı yine de devam etmeden önce Visual Studio'da çalışıp çalışmadığını **hata ayıklamayı Durdur** (Shift + F5)

1. İçinde **Çözüm Gezgini** > genişletin **sayfaları** > **About.cshtml** > açık **About.cshtml.cs**.

   ![Visual Studio çözüm Gezgini'nin ekran görüntüsü](./media/app-insights-asp-net-core/011-about.png)

2. Bir özel durum altında eklemek ``Message=`` ve değişikliği bir dosyaya kaydedin.

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

Application Insights ile ASP.NET Core güncelleştirmeye Canlı Stream işleve **Microsoft.ApplicationInsights.AspNetCore 2.2.0** NuGet paketleri.

Visual Studio'dan seçin **proje** > **NuGet paketlerini Yönet** > **Microsoft.ApplicationInsights.AspNetCore** > sürümü **2.2.0** > **güncelleştirme**.

  ![NuGet Paket Yöneticisi'nin ekran görüntüsü](./media/app-insights-asp-net-core/012-nuget-update.png)

Birden çok onay istemlerini meydana gelir. Okuyun ve değişiklikleri kabul etmiyorsanız kabul edin.

### <a name="page-view-load-time"></a>Sayfa görünümü yükleme süresi

1. Visual Studio'da gidin **Çözüm Gezgini** > **sayfaları** > iki dosya değiştirilmesi gerekiyor: _Layout.cshtml_, ve  _ViewImports.cshtml_

2. _ İçinde_ViewImports.cshtml_, ekleyin:

   ```csharp
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```

3. İçinde **_Layout.cshtml** önce altına satır Ekle ``</head>`` etiketi, aynı zamanda başka bir betikleri önce.

    ```csharp
    @Html.Raw(snippet.FullScript)
    ```

### <a name="test-failed-requests-page-view-load-time-live-stream"></a>Test başarısız istekler, sayfa görünümü yükleme süresi, Canlı Stream

Çıkış test ve her şeyin çalıştığını doğrulamak için:

1. IIS Express tıklayarak uygulamanızı çalıştırma ![Ekran görüntüsü, Visual Studio IIS Express simgesi](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Gidin **hakkında** test özel durum tetiklemek için sayfa. (Hata ayıklama modunda çalıştırıyorsanız, e tıklamanız gerekir **devam** için Visual Studio'daki Application Insights'da gösterilmesi için özel durum.)

3. Benzetimli PowerShell işlem komut dosyasından daha önce yeniden çalıştırın (komut bağlantı noktası numarası ayarlamanız gerekebilir.)

4. Uygulama Bilgileri'ne genel bakış Visual Studio menü seçeneğini belirleyin, hala açık değilse **proje** > **Application Insights** > **uygulama açma Insights portalında**. 

   > [!TIP]
   > Yeni trafiğinizi henüz görmüyorsanız denetleyin **zaman aralığı** tıklatıp **Yenile**.

   ![Ekran genel bakış penceresi](./media/app-insights-asp-net-core/0019-overview-updated.png)

5. Canlı Stream seçin

   ![Canlı ölçümleri Stream ekran görüntüsü](./media/app-insights-asp-net-core/0020-live-metrics-stream.png)

   (Çalışma durdurulmuşsa, PowerShell Betiği, hala çalışıyor Canlı ölçümleri görmeniz gerekir, betik Canlı Stream ile yeniden açın.)

## <a name="app-insights-sdk-comparison"></a>App Insights SDK'sı karşılaştırma

Application Insights ürün grubu arasında özellik denkliğini sağlamamız sabit çalışmaktadır [tam .NET Framework SDK](https://github.com/Microsoft/ApplicationInsights-dotnet) ve.Net Core SDK'sı. 2.2.0 sürüm [ASP.NET Core SDK'sı](https://github.com/Microsoft/ApplicationInsights-aspnetcore) için Application Insights özellik farkı büyük ölçüde kapattı.

Farkları ve bir denge hakkında daha fazla anlamak için [.NET ve .NET Core](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

   | SDK karşılaştırması | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Canlı ölçümleri**      | **+** |**-** | **+** |
   | **Sunucu Telemetri kanalı** | **+** |**-** | **+**|
   |**Uyarlamalı örnekleme**| **+** | **-** | **+**|
   | **SQL bağımlılık çağrıları**     | **+** |**-** | **+**|
   | **Performans sayaçları*** | **+** | **-**| **-**|

_Performans sayaçları_ bu bağlamda başvurduğu [sunucu tarafı performans sayaçları](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) işlemci, bellek ve disk kullanımı gibi.

## <a name="open-source-sdk"></a>Açık kaynak SDK'sı
[Okuma ve koda katkıda bulunun](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)

## <a name="application-insights-search-continued"></a>Application Insights arama devamı

Daha iyi bile açık bir Application Insights NuGet paketlerinin yüklemesini henüz oluştuğunda erişilmiş henüz Application Insights arama için bir ASP.NET Core 2 proje Visual Studio'da nasıl çalıştığını anlayın. Hata ayıklama çıktı incelemek yararlı olabilir.

Çıkış sözcük için arama yaparsanız _Insight_ aşağıdakine benzer sonuçlar vurgular:

```DebugOuput
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.aspnetcore.applicationinsights.hostingstartup\2.0.3\lib\netcoreapp2.0\Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll'.
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.applicationinsights.aspnetcore\2.1.1\lib\netstandard1.6\Microsoft.ApplicationInsights.AspNetCore.dll'.

Application Insights Telemetry (unconfigured): {"name":"Microsoft.ApplicationInsights.Dev.Message","time":"2018-06-03T17:32:38.2796801Z","tags":{"ai.location.ip":"127.0.0.1","ai.operation.name":"DEBUG /","ai.internal.sdkVersion":"aspnet5c:2.1.1","ai.application.ver":"1.0.0.0","ai.cloud.roleInstance":"CONTOSO-SERVER","ai.operation.id":"de85878e-4618b05bad11b5a6","ai.internal.nodeName":"CONTOSO-SERVER","ai.operation.parentId":"|de85878e-4618b05bad11b5a6."},"data":{"baseType":"MessageData","baseData":{"ver":2,"message":"Request starting HTTP/1.1 DEBUG http://localhost:53022/  0","severityLevel":"Information","properties":{"AspNetCoreEnvironment":"Development","Protocol":"HTTP/1.1","CategoryName":"Microsoft.AspNetCore.Hosting.Internal.WebHost","Host":"localhost:53022","Path":"/","Scheme":"http","ContentLength":"0","DeveloperMode":"true","Method":"DEBUG"}}}}
```

CoreCLR yükleme iki derleme şöyledir: 

- _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_
- _Microsoft.ApplicationInsights.AspNetCore.dll_.

Ve _yapılandırılmamış_ Application Insights örneği uygulamanız çalışırken oluşturulan verileri Azure'a gönderilmez ve yalnızca bu uygulama bir ikey ile ilişkili olmayan telemetri gösterir. yerel arama ve analiz için kullanılabilir.

Nasıl bu mümkündür bir parçası olan NuGet paketini _Microsoft.AspNetCore.All_ bağımlılık olarak alan [ _Microsoft.ASPNetCoreApplicationInsights.HostingStartup_](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.applicationinsights.hostingstartup.applicationinsightshostingstartup?view=aspnetcore-2.1)

![NuGet ekran bağımlılık Microsoft.AspNETCore.all için grafiği](./media/app-insights-asp-net-core/013-dependency.png)

Bir ASP.NET Core projesi VSCode veya başka bir düzenleyicide düzenlemekte olduğunuz, projenize Application Insights açıkça eklemediyseniz, Visual Studio'nun dışında bu derlemeleri otomatik olarak hata ayıklama sırasında yük mıydı.

Ancak, Visual Studio'da dış derlemelerden yerel Application ınsights'ı özelliklerini ayarlama bu aydınlatma kullanımı aracılığıyla gerçekleştirilir [Ihostingstartup arabirimi](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1) , dinamik olarak ekleyen Application Insights hata ayıklama sırasında.

Uygulama geliştirme hakkında daha fazla bilgi edinmek için bir [Ihostingstartup ile ASP.NET Core dış derlemede](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/platform-specific-configuration?view=aspnetcore-2.1). 

### <a name="how-to-disable-application-insights-in-visual-studio-net-core-projects"></a>Application Insights, Visual Studio .NET Core projelerinde devre dışı bırakma

Application Insights arama işlevini ayarlama otomatik ışık bazı yararlı olabilir, ancak beklenirken uygulamasında oluşturulan görme hata ayıklama telemetri kafa karıştırıcı olabilir.

Yalnızca telemetri oluşturma devre dışı bırakma yeterli değilse, bu kod bloğu yapılandırma yöntemine ekleyebilirsiniz, _Startup.cs_ dosyası:

```csharp
  var configuration = app.ApplicationServices.GetService<Microsoft.ApplicationInsights.Extensibility.TelemetryConfiguration>();
            configuration.DisableTelemetry = true;
            if (env.IsDevelopment())
```

CoreCLR yine de yüklenecektir _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_ ve _Microsoft.ApplicationInsights.AspNetCore.dll_, ancak hiçbir şey olmaz.

Application Insights, Visual Studio .NET Core projenizde tamamen devre dışı bırakmak isterseniz, tercih edilen yöntem seçmektir **Araçları** > **seçenekleri**  >   **Projeler ve çözümler** > **Web projeleri** > ve ASP.NET Core web projeleri için yerel Application Insights'ı devre dışı bırakmak için onay kutusunu işaretleyin. Bu işlev, Visual Studio 15.6 sürümünde eklendi.

![Ekran görüntüsü, Visual Studio Seçenekleri penceresini Web projelerini ekranı](./media/app-insights-asp-net-core/014-disable.png)

Visual Studio'nun önceki bir sürümünü çalıştıran ve tamamen Ihostingstartup yüklenen derlemelerin kaldırmak istediğiniz ya da ekleyebilirsiniz:

`.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")`

için _Program.cs_:

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

Veya alternatif olarak ekleyebilirsiniz ``"ASPNETCORE_preventHostingStartup": "True"`` için _launchSettings.json_ ortam değişkenleri.

Bu yöntemlerden birini kullanarak bunu yalnızca Application Insights'ın işlevsellik'kurmak Ihostingstartup ışık yararlanarak Visual Studio'da herhangi bir şey devre dışı bırakılır devre dışı bırakılmaz sorunudur.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Sonraki adımlar
* [Kullanıcı akışları keşfedin](app-insights-usage-flows.md) için kullanıcıların uygulamanızda nasıl gezindiğini anlayın.
* [Anlık görüntü koleksiyonunu yapılandırma](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications) bir özel durum şu anda kaynak kodu ve değişkenleri durumunu görmek için.
* [API'yi kullanmak](app-insights-api-custom-events-metrics.md) kendi olayları ve ölçümleri daha ayrıntılı bir görünüm, uygulamanızın performans ve kullanım için gönderilecek.
* [Kullanılabilirlik testleri](app-insights-monitor-web-app-availability.md) uygulamanızdan sürekli olarak dünyanın dört bir yanındaki denetleyin.
