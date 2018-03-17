---
title: "ASP.NET Core için Azure Application Insights | Microsoft Docs"
description: "Kullanılabilirlik, performans ve kullanım için Web uygulamalarını izleyin."
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: mbullwin
ms.openlocfilehash: 77c48a22f974e027b4e8858d5e38018bbf5bb54f
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="application-insights-for-aspnet-core"></a>ASP.NET Core için Application Insights

Azure Application Insights web uygulamanıza kod düzeyine ayrıntılı izleme sağlar. Web uygulamanız için kullanılabilirliği, performansı ve kullanımı kolay izleyebilirsiniz. Ayrıca, bir kullanıcının bildirmesini beklemeden uygulamanızdaki hataları hızlıca tanımlayıp tespit edebilirsiniz.

Bu makalede, örnek ASP.NET Core oluşturmada size yol gösterilir [Razor sayfalarının](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) Visual Studio ve Azure Application Insights ile izlemeye başlamak nasıl uygulama.

## <a name="prerequisites"></a>Önkoşullar

- NET çekirdek 2.0.0 SDK veya sonraki bir sürümü.
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.3 veya ASP.NET ve web geliştirme iş yükü ile sonraki bir sürümü.

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Visual Studio'da bir ASP.NET Core projesi oluşturma

1. Sağ tıklatın ve Başlat **Visual Studio 2017** yönetici olarak.
2. Seçin **dosya** > **yeni** > **proje** (Ctrl-Shift-N).

   ![Visual Studio dosya yeni proje menüsünün ekran görüntüsü](./media/app-insights-asp-net-core/0001-file-new-project.png)

3. Genişletme **Visual C#** > seçin **.NET Core** > **ASP.NET Core Web uygulaması**. Girin bir **adı** > **çözüm adı** > denetleyin **oluştur yeni Git deposu**.

   ![Visual Studio dosya yeni proje Sihirbazı ekran görüntüsü](./media/app-insights-asp-net-core/0002-new-project-web-application.png)

4. Seçin **.Net Core** > **ASP.NET Core 2.0** **Web uygulaması** > **Tamam**.

    ![Visual Studio dosya yeni proje seçimi menüsünün ekran görüntüsü](./media/app-insights-asp-net-core/0003-dot-net-core.png)

## <a name="add-application-insights-telemetry"></a>Application Insights Telemetrisi ekleme

1. Seçin **proje** > **Application Insights Telemetrisi Ekle...** (Alternatif olarak sağ **bağlantılı Hizmetler** > bağlı hizmet Ekle.)

    ![Visual Studio dosya yeni proje seçimi menüsünün ekran görüntüsü](./media/app-insights-asp-net-core/0004-add-application-insights-telemetry.png)

2. **Ücretsiz Olarak Başla**’yı seçin.

    ![Visual Studio dosya yeni proje seçimi menüsünün ekran görüntüsü](./media/app-insights-asp-net-core/0005-start-free.png)

3. Uygun bir seçin **abonelik** > **kaynak** > ve koleksiyon 1 GB'den aylık izin verilip verilmeyeceğini > **kaydetmek**.

    ![Visual Studio dosya yeni proje seçimi menüsünün ekran görüntüsü](./media/app-insights-asp-net-core/0006-register.png)

## <a name="changes-made-to-your-project"></a>Projenize Made değiştirir

Application Insights çok düşük ek yükleri olur. Projenize Application Insights telemetri ekleyerek yapılan değişiklikleri gözden geçirmek için:

Seçin **Görünüm** > **Takım Gezgini** (Ctrl +\, Ctrl + M) > **proje** > **değişiklikleri**

- Toplam dört değişiklikler:

  ![Application Insights ekleyerek değiştirilen dosyalar ekran görüntüsü](./media/app-insights-asp-net-core/0007-changes.png)

- Yeni bir dosya oluşturulur:

   **ConnectedService.json**

  ![Application Insights ekleyerek değiştirilen dosyalar ekran görüntüsü](./media/app-insights-asp-net-core/0008-connectedservice-json.png)

- Değiştirilen üç dosyaları:

  **appsettings.json**

   ![Application Insights ekleyerek değiştirilen dosyalar ekran görüntüsü](./media/app-insights-asp-net-core/0009-appsettings-json.png)

  **ContosoDotNetCore.csproj**

   ![Application Insights ekleyerek değiştirilen dosyalar ekran görüntüsü](./media/app-insights-asp-net-core/0010-contoso-netcore-csproj.png)

   **Program.cs**

   ![Application Insights ekleyerek değiştirilen dosyalar ekran görüntüsü](./media/app-insights-asp-net-core/0011-program-cs.png)

## <a name="synthetic-transactions-with-powershell"></a>PowerShell ile yapay işlemler

Uygulamanızı başlatma tıklattıktan sonra geçici bağlantıları el ile test trafiği oluşturmak için kullanılabilir. Ancak, bu genellikle PowerShell'de basit bir yapay işlem oluşturmak yararlıdır.

1. IIS Express'i tıklatarak uygulamanızı çalıştırma ![Ekran görüntüsü, Visual Studio IIS Express simgesi](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Tarayıcınızın adres çubuğundan URL'yi kopyalayın. Biçim http://localhost olduğu: {rastgele bağlantı noktası numarası}

   ![Tarayıcı url adres çubuğunun ekran görüntüsü](./media/app-insights-asp-net-core/0013-copy-url.png)

3. Test uygulamanızı karşı 100 yapay işlemler oluşturmak için aşağıdaki PowerShell döngü çalıştırın. Sonra bağlantı noktası numarasını değiştirin **localhost:** önceki adımda kopyaladığınız url ile eşleşmek üzere.

   ```PS
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-application-insights-portal"></a>Açık uygulama Insights portalı

PowerShell önceki bölümden çalıştırdıktan sonra işlemleri görüntüleyebilir ve veri toplanmakta olan onaylamak için Application Insights başlatın. 

Visual Studio menüsünden seçin **proje** > **Application Insights** > **açık uygulama Insights portalı**

   ![Application Insights Overview ekran görüntüsü](./media/app-insights-asp-net-core/0014-portal-01.png)

> [!NOTE]
> Yukarıdaki örnek ekran görüntüsünde **canlı akış**, **sayfa görünümü yükleme süresi**, ve **başarısız istekler** şu anda toplanmadı. Sonraki bölümde, her eklerken size yol gösterir. Zaten topluyorsanız **canlı akış**, ve **sayfa görünümü yükleme süresi**, yalnızca adımlarını izleyin **başarısız istekler**.

## <a name="collect-failed-requests-live-stream--page-view-load-time"></a>Başarısız istekler, canlı akış ve sayfa görünümü yükleme süresi toplama

### <a name="failed-requests"></a>Başarısız Olan İstekler

Teknik olarak **başarısız istekler** toplanmakta olan, ancak hiçbiri henüz oluşmuş. Bir özel durum boyunca işlemini hızlandırmak için gerçek özel durum benzetimini yapmak için var olan proje eklenebilir. Uygulamanızı hala devam etmeden önce Visual Studio'da çalışıp çalışmadığını **durdurma hata ayıklama** (Shift + F5)

1. İçinde **Çözüm Gezgini** > genişletin **sayfaları** > **About.cshtml** > açmak **About.cshtml.cs**.

   ![Visual Studio çözüm Gezgini'nin ekran görüntüsü](./media/app-insights-asp-net-core/0015-solution-explorer-about.png)

2. Bir özel durum altında ekleme ``Message=`` > değişikliği dosyaya kaydedin.

   ```C#
   throw new Exception("Test Exception");
   ```

   ![Özel durum kodu ekran görüntüsü](./media/app-insights-asp-net-core/000016-exception.png)

### <a name="live-stream"></a>Canlı Akış

Application Insights canlı akış işlevlerini ASP.NET Core güncelleştirme ile birlikte erişmek için **Microsoft.ApplicationInsights.AspNetCore 2.2.0** NuGet paketleri.

Visual Studio'dan seçin **proje** > **NuGet paketlerini Yönet** > **Microsoft.ApplicationInsights.AspNetCore** > sürüm **2.2.0** > **güncelleştirme**.

  ![NuGet Paket Yöneticisi'nin ekran görüntüsü](./media/app-insights-asp-net-core/0017-update-nuget.png)

Birden çok onay istekleri ortaya okuyup değişikliklerle onaylıyorsanız kabul edin.

### <a name="page-view-load-time"></a>Sayfa görünümü yükleme süresi

1. Visual Studio'da gidin **Çözüm Gezgini** > **sayfaları** > iki dosya değiştirilmesi gerekir: **_Layout.cshtml**, ve **_ ViewImports.cshtml**

2. İçinde **_viewımports.cshtml**, ekleyin:

   ```C#
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```
     ![Kod değişikliği _viewımports.cshtml için ekran görüntüsü](./media/app-insights-asp-net-core/00018-view-imports.png)

3. İçinde **Layout.cshtml** önce aşağıdaki satırı ekleyin ``</head>`` etiketi, ancak herhangi bir betiği öncesinde.

    ```C#
    @Html.Raw(snippet.FullScript)
    ```
    ![Kod değişikliği layout.cshtml için ekran görüntüsü](./media/app-insights-asp-net-core/0018-layout-cshtml.png)

### <a name="test-failed-requests-page-view-load-time-live-stream"></a>Test başarısız istekleri, sayfa görünümü yükleme süresi, canlı akış

Önceki adımları tamamladığınıza göre çıkışı test ve her şeyin çalıştığını onaylayın.

1. IIS Express'i tıklatarak uygulamanızı çalıştırma ![Ekran görüntüsü, Visual Studio IIS Express simgesi](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Gidin **hakkında** test özel durum tetiklemek için sayfa. (Hata ayıklama modunda çalıştırıyorsanız,'e tıklamanız gerekir **devam** Visual Studio'da Application Insights tarafından çekilmesi özel önce.)

3. Benzetimli PowerShell işlem betikten daha önce yeniden çalıştırın (bağlantı noktası numarası betikteki ayarlamanız gerekebilir.)

4. Uygulamaları Öngörüler genel bakış Visual Studio menüsünden seçeneğini belirleyin, hala açık değilse **proje** > **Application Insights** > **uygulama açın Insights portalında**. 

   > [!TIP]
   > Yeni trafiğinizi henüz görmüyorsanız denetleyin **zaman aralığı** tıklatıp **yenileme**.

   ![Genel Bakış ekran penceresi](./media/app-insights-asp-net-core/0019-overview-updated.png)

5. Canlı akış seçin

   ![Canlı ölçümleri akışının ekran görüntüsü](./media/app-insights-asp-net-core/0020-live-metrics-stream.png)

   (PowerShell komut dosyası, dinamik ölçümleri görmelisiniz hala çalışıyor durduruldu ise betiği yeniden ile canlı akış açık çalıştırın.)

## <a name="app-insights-sdk-comparison"></a>App Insights SDK karşılaştırma

Application Insights ürün grubu sabit özellik eşliği olarak arasında mümkün olduğunca yakın elde etmek için çalışmakta olduğu [tam .NET Framework SDK](https://github.com/Microsoft/ApplicationInsights-dotnet) ve .net Core SDK. 2.2.0 sürümü, [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore) için Application Insights büyük ölçüde özelliği boşluk kapattı.

Farkları ve bileşim arasında daha iyi anlamak için [.NET ve .NET Core](https://docs.microsoft.com/en-us/dotnet/standard/choosing-core-framework-server).

   | SDK karşılaştırma | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Canlı ölçümleri**      | **+** |**-** | **+** |
   | **Server Telemetri kanalı** | **+** |**-** | **+**|
   |**Uyarlamalı örnekleme**| **+** | **-** | **+**|
   | **SQL bağımlılık çağrıları**     | **+** |**-** | **+**|
   | **Performans sayaçları*** | **+** | **-**| **-**|

_Performans sayaçları_ bu bağlamda başvurduğu [sunucu tarafı performans sayaçları](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-performance-counters) işlemci, bellek ve disk kullanımı gibi.

## <a name="open-source-sdk"></a>Açık kaynak SDK'sı
[Okuma ve koda katkıda bulunan](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Sonraki adımlar
* [Kullanıcıların akar keşfedin](app-insights-usage-flows.md) kullanıcılarınızın uygulamanız nasıl gezindiğini anlamak için.
* [Anlık görüntü koleksiyonunu yapılandırma](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications) bir özel durum şu anda kaynak kodu ve değişkenleri durumunu görmek için.
* [API kullanan](app-insights-api-custom-events-metrics.md) kendi olayları ve ölçümleri, uygulamanızın performansı ve kullanımı daha ayrıntılı bir görünüm için gönderilecek.
* [Kullanılabilirlik testleri](app-insights-monitor-web-app-availability.md) uygulamanızdan sürekli dünyanın denetleyin.