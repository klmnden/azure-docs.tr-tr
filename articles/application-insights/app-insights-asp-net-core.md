---
title: ASP.NET Core için Azure Application Insights | Microsoft Docs
description: Kullanılabilirlik, performans ve kullanım için Web uygulamalarını izleyin.
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
ms.openlocfilehash: 261bc78bfe427173ba81eef731e33eddd2ec379b
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294284"
---
# <a name="application-insights-for-aspnet-core"></a>ASP.NET Core için Application Insights

Azure Application Insights web uygulamanıza kod düzeyine ayrıntılı izleme sağlar. Web uygulamanız için kullanılabilirliği, performansı ve kullanımı kolay izleyebilirsiniz. Ayrıca, bir kullanıcının bildirmesini beklemeden uygulamanızdaki hataları hızlıca tanımlayıp tespit edebilirsiniz.

Bu makalede, örnek ASP.NET Core oluşturmada size yol gösterilir [Razor sayfalarının](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) Visual Studio ve Azure Application Insights ile izlemeye başlamak nasıl uygulama.

## <a name="prerequisites"></a>Önkoşullar

- NET çekirdek 2.0.0 SDK veya sonraki bir sürümü.
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) sürüm 15.7.3 veya ASP.NET ve web geliştirme iş yükü üstü. 

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Visual Studio'da bir ASP.NET Core projesi oluşturma

1. Sağ tıklatın ve Başlat **Visual Studio 2017** yönetici olarak.
2. Seçin **dosya** > **yeni** > **proje** (Ctrl-Shift-N).

   ![Visual Studio dosya yeni proje menüsünün ekran görüntüsü](./media/app-insights-asp-net-core/001-new-project.png)

3. Genişletme **Visual C#** > seçin **.NET Core** > **ASP.NET Core Web uygulaması**. Girin bir **adı** > **çözüm adı** > denetleyin **oluştur yeni Git deposu**.

   ![Visual Studio dosya yeni proje Sihirbazı ekran görüntüsü](./media/app-insights-asp-net-core/002-asp-net-core-web-application.png)

4. Seçin **.Net Core** > **ASP.NET Core 2.0** **Web uygulaması** > **Tamam**.

    ![Visual Studio dosya yeni proje seçimi menüsünün ekran görüntüsü](./media/app-insights-asp-net-core/003-web-application.png)

## <a name="application-insights-search"></a>Application Insights araması

Visual Studio'da varsayılan dayalı 2015 güncelleştirme 2 veya daha büyük bir ASP.NET Core 2 + ile proje sürüm yararlanabilir [Application Insights arama](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-visual-studio) bile önce açıkça eklemeniz Application Insights projenize.

Bu işlevselliği test etmek için:

1. IIS Express'i tıklatarak uygulamanızı çalıştırma ![Ekran görüntüsü, Visual Studio IIS Express simgesi](./media/app-insights-asp-net-core/004-iis-express.png)

2. Seçin **Görünüm** > **diğer pencereler** > **Application Insights arama**.

   ![Visual Studio Tanılama Araçları'nın ekran görüntüsü](./media/app-insights-asp-net-core/005-view-other-windows-search.png)

3. Hata ayıklama oturumu telemetri yalnızca yerel çözümleme için şu anda kullanılabilir değil. Application Insights tam olarak etkinleştirmek için seçin **Telemetri hazırlık** üst sağ veya sonraki bölümde yer alan adımları izleyin.

   ![Ekran görüntüsü Visual Studio Application Insights arama](./media/app-insights-asp-net-core/006-search.png)

> [!NOTE]
> Visual Studio ışık özelliklerini nasıl gibi hakkında daha fazla bilgi edinmek için [uygulama Insights arama](app-insights-visual-studio.md) ve [CodeLens](app-insights-visual-studio-codelens.md) eklediğiniz önce yerel olarak ASP.NET Core projenize Application Insights kullanıma Açıklama en [bu makalenin sonundaki.](#Application-Insights-search-continued)

## <a name="add-application-insights-telemetry"></a>Application Insights Telemetrisi ekleme

1. Seçin **proje** > **Application Insights Telemetrisi Ekle...** . (Veya sağ tıklayarak **bağlantılı Hizmetler** ve bağlı hizmet Ekle seçin.)

    ![Visual Studio dosya yeni proje seçimi menüsünün ekran görüntüsü](./media/app-insights-asp-net-core/007-project-add-telemetry.png)

2. Seçin **başlama**. (Visual Studio sürümüne bağlı olarak metnin biraz değişebilir. Bunun yerine, bazı eski sürümleri sahip bir **Başlat ücretsiz** düğmesini.)

    ![Visual Studio dosya yeni proje seçimi menüsünün ekran görüntüsü](./media/app-insights-asp-net-core/008-get-started.png)

3. Uygun bir seçin **abonelik** > **kaynak** > **kaydetmek**.

## <a name="changes-made-to-your-project"></a>Projenize Made değiştirir

Application Insights düşük ek yükleri olur. Projenize Application Insights telemetri ekleyerek yapılan değişiklikleri gözden geçirmek için:

Seçin **Görünüm** > **Takım Gezgini** (Ctrl +\, Ctrl + M) > **proje** > **değişiklikleri**

- Toplam dört değişiklikler:

  ![Application Insights ekleyerek değiştirilen dosyalar ekran görüntüsü](./media/app-insights-asp-net-core/009-changes.png)

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

1. IIS Express'i tıklatarak uygulamanızı çalıştırma ![Ekran görüntüsü, Visual Studio IIS Express simgesi](./media/app-insights-asp-net-core/004-iis-express.png)

2. Tarayıcınızın adres çubuğundan URL'yi kopyalayın. Şu biçimdedir http://localhost:{random bağlantı noktası numarası}

   ![Tarayıcı url adres çubuğunun ekran görüntüsü](./media/app-insights-asp-net-core/0013-copy-url.png)

3. Test uygulamanızı karşı 100 yapay işlemler oluşturmak için aşağıdaki PowerShell döngü çalıştırın. Sonra bağlantı noktası numarasını değiştirin **localhost:** önceki adımda kopyaladığınız url ile eşleşmek üzere.

   ```PowerShell
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-application-insights-portal"></a>Açık uygulama Insights portalı

PowerShell önceki bölümden çalıştırdıktan sonra işlemleri görüntüleyebilir ve veri toplanmakta olan onaylamak için Application Insights başlatın. 

Visual Studio menüsünden seçin **proje** > **Application Insights** > **açık uygulama Insights portalı**

   ![Application Insights Overview ekran görüntüsü](./media/app-insights-asp-net-core/010-portal.png)

> [!NOTE]
> Yukarıdaki örnek ekran görüntüsünde **canlı akış**, **sayfa görünümü yükleme süresi**, ve **başarısız istekler** şu anda toplanmadı. Sonraki bölümde, her eklerken size yol gösterir. Zaten topluyorsanız **canlı akış**, ve **sayfa görünümü yükleme süresi**, yalnızca adımlarını izleyin **başarısız istekler**.

## <a name="collect-failed-requests-live-stream--page-view-load-time"></a>Başarısız istekler, canlı akış ve sayfa görünümü yükleme süresi toplama

### <a name="failed-requests"></a>Başarısız İstekler

Teknik olarak **başarısız istekler** toplanmakta olan, ancak hiçbiri henüz oluşmuş. Bir özel durum boyunca işlemini hızlandırmak için gerçek özel durum benzetimini yapmak için var olan proje eklenebilir. Uygulamanızı hala devam etmeden önce Visual Studio'da çalışıp çalışmadığını **durdurma hata ayıklama** (Shift + F5)

1. İçinde **Çözüm Gezgini** > genişletin **sayfaları** > **About.cshtml** > açmak **About.cshtml.cs**.

   ![Visual Studio çözüm Gezgini'nin ekran görüntüsü](./media/app-insights-asp-net-core/011-about.png)

2. Bir özel durum altında eklemek ``Message=`` ve değişiklik dosyaya kaydedin.

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

Application Insights canlı akış işlevlerini ASP.NET Core güncelleştirme ile birlikte erişmek için **Microsoft.ApplicationInsights.AspNetCore 2.2.0** NuGet paketleri.

Visual Studio'dan seçin **proje** > **NuGet paketlerini Yönet** > **Microsoft.ApplicationInsights.AspNetCore** > sürüm **2.2.0** > **güncelleştirme**.

  ![NuGet Paket Yöneticisi'nin ekran görüntüsü](./media/app-insights-asp-net-core/012-nuget-update.png)

Birden çok onay istekleri meydana gelir. Okuyun ve değişikliklerle onaylıyorsanız kabul edin.

### <a name="page-view-load-time"></a>Sayfa görünümü yükleme süresi

1. Visual Studio'da gidin **Çözüm Gezgini** > **sayfaları** > iki dosya değiştirilmesi gerekir: _Layout.cshtml_, ve  _ViewImports.cshtml_

2. _ İçinde_ViewImports.cshtml_, ekleyin:

   ```csharp
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```

3. İçinde **_Layout.cshtml** önce aşağıdaki satırı ekleyin ``</head>`` aynı zamanda herhangi bir betiği önce etiketi.

    ```csharp
    @Html.Raw(snippet.FullScript)
    ```

### <a name="test-failed-requests-page-view-load-time-live-stream"></a>Test başarısız istekleri, sayfa görünümü yükleme süresi, canlı akış

Out sınamak ve her şeyin çalıştığını doğrulamak için:

1. IIS Express'i tıklatarak uygulamanızı çalıştırma ![Ekran görüntüsü, Visual Studio IIS Express simgesi](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Gidin **hakkında** test özel durum tetiklemek için sayfa. (Hata ayıklama modunda çalıştırıyorsanız,'e tıklamanız gerekir **devam** Visual Studio'da Application Insights'ta gösterilmesini özel durum için.)

3. Benzetimli PowerShell işlem betikten daha önce yeniden (bağlantı noktası numarası betikteki ayarlamanız gerekebilir.)

4. Uygulamaları Öngörüler genel bakış Visual Studio menüsünden seçeneğini belirleyin, hala açık değilse **proje** > **Application Insights** > **uygulama açın Insights portalında**. 

   > [!TIP]
   > Yeni trafiğinizi henüz görmüyorsanız denetleyin **zaman aralığı** tıklatıp **yenileme**.

   ![Genel Bakış ekran penceresi](./media/app-insights-asp-net-core/0019-overview-updated.png)

5. Canlı akış seçin

   ![Canlı ölçümleri akışının ekran görüntüsü](./media/app-insights-asp-net-core/0020-live-metrics-stream.png)

   (Çalıştır durdurulmuşsa komut dosyası, hala çalışıyor, PowerShell Canlı ölçümleri görmeniz gerekir, betik canlı akış ile yeniden açın.)

## <a name="app-insights-sdk-comparison"></a>App Insights SDK karşılaştırma

Application Insights ürün grubu sabit arasında özellik eşliği elde etmek için çalışmakta olduğu [tam .NET Framework SDK](https://github.com/Microsoft/ApplicationInsights-dotnet) ve .net Core SDK. 2.2.0 sürümü, [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore) için Application Insights büyük ölçüde özelliği boşluk kapattı.

Farkları ve bileşim arasında daha iyi anlamak için [.NET ve .NET Core](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

   | SDK karşılaştırma | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Canlı ölçümleri**      | **+** |**-** | **+** |
   | **Server Telemetri kanalı** | **+** |**-** | **+**|
   |**Uyarlamalı örnekleme**| **+** | **-** | **+**|
   | **SQL bağımlılık çağrıları**     | **+** |**-** | **+**|
   | **Performans sayaçları*** | **+** | **-**| **-**|

_Performans sayaçları_ bu bağlamda başvurduğu [sunucu tarafı performans sayaçları](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) işlemci, bellek ve disk kullanımı gibi.

## <a name="open-source-sdk"></a>Açık kaynak SDK'sı
[Okuma ve koda katkıda bulunan](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)

## <a name="application-insights-search-continued"></a>Uygulama Insights arama devam

Application Insights arama Visual Studio'da ASP.NET Core 2 projesinde için uygulama Öngörüler NuGet bile açık bir yüklemesi nasıl çalıştığını daha iyi anlamak için paketler henüz oluştu yapmadıysanız. Hata ayıklama çıktı incelemek yararlı olabilir.

Çıkış word için arama yaparsanız _Insight_ aşağıdakine benzer sonuçlar vurgular:

```DebugOuput
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.aspnetcore.applicationinsights.hostingstartup\2.0.3\lib\netcoreapp2.0\Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll'.
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.applicationinsights.aspnetcore\2.1.1\lib\netstandard1.6\Microsoft.ApplicationInsights.AspNetCore.dll'.

Application Insights Telemetry (unconfigured): {"name":"Microsoft.ApplicationInsights.Dev.Message","time":"2018-06-03T17:32:38.2796801Z","tags":{"ai.location.ip":"127.0.0.1","ai.operation.name":"DEBUG /","ai.internal.sdkVersion":"aspnet5c:2.1.1","ai.application.ver":"1.0.0.0","ai.cloud.roleInstance":"CONTOSO-SERVER","ai.operation.id":"de85878e-4618b05bad11b5a6","ai.internal.nodeName":"CONTOSO-SERVER","ai.operation.parentId":"|de85878e-4618b05bad11b5a6."},"data":{"baseType":"MessageData","baseData":{"ver":2,"message":"Request starting HTTP/1.1 DEBUG http://localhost:53022/  0","severityLevel":"Information","properties":{"AspNetCoreEnvironment":"Development","Protocol":"HTTP/1.1","CategoryName":"Microsoft.AspNetCore.Hosting.Internal.WebHost","Host":"localhost:53022","Path":"/","Scheme":"http","ContentLength":"0","DeveloperMode":"true","Method":"DEBUG"}}}}
```

CoreCLR yükleme iki derleme şöyledir: 

- _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_
- _Microsoft.ApplicationInsights.AspNetCore.dll_.

Ve _yapılandırabilirsiniz_ uygulamanızı çalışırken oluşturan verileri Azure'a gönderilen değildir ve yalnızca bu uygulama bir ikey ile ilişkili değil her örnek Application Insights telemetri gösterir yerel arama ve analiz için kullanılabilir.

Nasıl mümkündür parçası NuGet paketi olan _Microsoft.AspNetCore.All_ bağımlılık olarak alan [ _Microsoft.ASPNetCoreApplicationInsights.HostingStartup_](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.applicationinsights.hostingstartup.applicationinsightshostingstartup?view=aspnetcore-2.1)

![Ekran görüntüsü, NuGet bağımlılık Microsoft.AspNETCore.all için grafiği](./media/app-insights-asp-net-core/013-dependency.png)

Bir ASP.NET Core projesinde VSCode veya başka bir düzenleyici düzenleme projenize açıkça Application Insights eklemediyseniz Visual Studio dışında bu derlemeler otomatik olarak hata ayıklama sırasında yükleme olmayacaktır.

Ancak, Visual Studio'da yukarı bu aydınlatma dış derlemelerden yerel Application Insights özelliklerin kullanımı aracılığıyla gerçekleştirilir [IHostingStartup arabirimi](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1) da dinamik olarak ekler Application Insights hata ayıklama sırasında.

Uygulama geliştirme hakkında daha fazla bilgi edinmek için bir [ASP.NET Core IHostingStartup ile dış derlemede](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/platform-specific-configuration?view=aspnetcore-2.1). 

### <a name="how-to-disable-application-insights-in-visual-studio-net-core-projects"></a>Application Insights Visual Studio .NET Core projelerinde devre dışı bırakma

Application Insights arama işlevini ayarlama otomatik ışık bazı yararlı olabileceği bekleniyor doğru olduğunda oluşturulan görme hata ayıklama telemetri kafa karıştırıcı olabilir.

Yalnızca telemetri oluşturma devre dışı bırakma yeterli olması durumunda bu kod bloğu yapılandırma yöntemine ekleyebilirsiniz, _haline_ dosyası:

```csharp
  var configuration = app.ApplicationServices.GetService<Microsoft.ApplicationInsights.Extensibility.TelemetryConfiguration>();
            configuration.DisableTelemetry = true;
            if (env.IsDevelopment())
```

CoreCLR hala yük _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_ ve _Microsoft.ApplicationInsights.AspNetCore.dll_, ancak hiçbir şey olmaz.

Application Insights, Visual Studio .NET Core projenizde tamamen devre dışı bırakmak istiyorsanız, tercih edilen yöntem seçmektir **Araçları** > **seçenekleri**  >   **Projeler ve çözümler** > **Web projeleri** > ve ASP.NET Core web projeleri için yerel Application Insights devre dışı bırakmak için onay kutusunu işaretleyin. Bu işlevsellik Visual Studio 15,6 eklendi.

![Ekran görüntüsü, Visual Studio Seçenekleri penceresini Web projeleri ekranı](./media/app-insights-asp-net-core/014-disable.png)

Visual Studio'nun önceki bir sürümünü çalıştıran ve tamamen IHostingStartup yüklenen tüm derlemelerde kaldırmak istediğiniz ya da ekleyebilirsiniz:

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

Bu yöntemlerden birini kullanarak, yalnızca Application Insights işlevselliği yukarı IHostingStartup ışık yararlanarak Visual Studio'da herhangi bir şey devre dışı bırakılır devre dışı bırakılmaz sorunudur.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Sonraki adımlar
* [Kullanıcıların akar keşfedin](app-insights-usage-flows.md) kullanıcılarınızın uygulamanız nasıl gezindiğini anlamak için.
* [Anlık görüntü koleksiyonunu yapılandırma](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications) bir özel durum şu anda kaynak kodu ve değişkenleri durumunu görmek için.
* [API kullanan](app-insights-api-custom-events-metrics.md) kendi olayları ve ölçümleri, uygulamanızın performansı ve kullanımı daha ayrıntılı bir görünüm için gönderilecek.
* [Kullanılabilirlik testleri](app-insights-monitor-web-app-availability.md) uygulamanızdan sürekli dünyanın denetleyin.
