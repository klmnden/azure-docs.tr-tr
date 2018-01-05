---
title: "Azure Application Insights .NET uygulamaları için hata ayıklayıcı anlık görüntü | Microsoft Docs"
description: "Hata ayıklama anlık görüntüleri otomatik olarak üretim .NET uygulamaları özel durumlar, toplanan"
services: application-insights
documentationcenter: 
author: pharring
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: mbullwin
ms.openlocfilehash: f1efbfc1f85f4c2fa404742e2d71344b3426c94d
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>Anlık görüntü özel durumları .NET uygulamalarında hata ayıklama

Özel durum oluştuğunda, hata ayıklama anlık görüntü canlı web uygulamanızı otomatik olarak toplayabilirsiniz. Anlık görüntü özel durumu şu anda kaynak kodu ve değişkenleri durumunu gösterir. Anlık görüntü ayıklayıcıda (Önizleme) [Azure Application Insights](app-insights-overview.md) özel durum, web uygulamanızın telemetrisinden izler. Böylece üretim sorunları tanılamak için gereken bilgileri sahip anlık görüntüleri, üst atma özel durumlarını toplar. Dahil [anlık görüntü Toplayıcı NuGet paketi](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanızda ve isteğe bağlı olarak koleksiyon parametrelerinde yapılandırma [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md). Anlık görüntüler görünmez [özel durumları](app-insights-asp-net-exceptions.md) Application Insights portalında.

Yığın ve her çağrı yığını çerçevesi en değişkenlerle incelemek çağrı görmek için Portalı'nda hata ayıklama anlık görüntüleri görüntüleyebilirsiniz. Kaynak kodu ile daha güçlü bir hata ayıklama deneyimini almak için anlık görüntüleri olan açık Visual Studio 2017 kuruluş tarafından [Visual Studio için anlık görüntü hata ayıklayıcısı uzantısı yükleme](https://aka.ms/snapshotdebugger). Visual Studio'da şunları da yapabilirsiniz [etkileşimli olarak anlık görüntülerini almak için Snappoints ayarlamak](https://aka.ms/snappoint) olmadan için bir özel durum bekleniyor.

Anlık görüntü koleksiyonu için kullanılabilir:
* .NET framework ve ASP.NET uygulamaları .NET Framework 4.5 veya sonraki sürümlerini çalıştırıyor.
* Windows üzerinde çalışan .NET core 2.0 ve ASP.NET Core 2.0 uygulamaları.

Aşağıdaki ortamlarda desteklenir:
* Azure uygulama hizmeti.
* İşletim sistemi ailesi 4 veya üstünü çalıştıran Azure bulut hizmeti.
* Windows Server 2012 R2 veya sonraki sürümlerde çalışan azure Service Fabric hizmetler.
* Windows Server 2012 R2 çalıştıran Azure sanal makineler veya sonraki bir sürümü.
* Şirket içi Windows Server 2012 R2 çalıştıran sanal veya fiziksel makineler veya sonraki bir sürümü.

> [!NOTE]
> İstemci uygulamaları (örneğin, WPF, Windows Forms veya UWP) desteklenmez.

### <a name="configure-snapshot-collection-for-aspnet-applications"></a>ASP.NET uygulamaları için anlık görüntü koleksiyonunu yapılandırma

1. [Application Insights web uygulamanızda etkinleştirmek](app-insights-asp-net.md), henüz yapmadınız.

2. Dahil [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanıza NuGet paketi. 

3. Pakete eklenen varsayılan seçenekleri gözden [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md):

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- The default is true, but you can disable Snapshot Debugging by setting it to false -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this to true. -->
        <!-- DeveloperMode is a property on the active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need to see an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>5</ThresholdForSnapshotting>
        <!-- The maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- The maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often to reset problem counters. -->
        <ProblemCounterResetInterval>06:00:00</ProblemCounterResetInterval>
        <!-- The maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>50</SnapshotsPerDayLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. Anlık görüntüler için Application Insights bildirilen özel durum toplanır. Bazı durumlarda (örneğin, daha eski sürümlerini .NET platformu) için gereksinim duyabileceğiniz [özel durum koleksiyonunu yapılandırma](app-insights-asp-net-exceptions.md#exceptions) portalında anlık görüntüler içeren özel durumları görmek için.


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a>ASP.NET Core 2.0 uygulamaları için anlık görüntü koleksiyonunu yapılandırma

1. [Application Insights ASP.NET Core web uygulamanızda etkinleştirmek](app-insights-asp-net-core.md), henüz yapmadınız.

> [!NOTE]
> Uygulamanızı sürüm 2.1.1 başvuruyor emin veya Microsoft.ApplicationInsights.AspNetCore paketinin daha yeni.

2. Dahil [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanıza NuGet paketi.

3. Uygulamanızın değiştirme `Startup` eklemek ve anlık görüntü toplayıcının telemetri işlemci yapılandırmak için sınıf.

   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   using Microsoft.Extensions.Options;
   ...
   class Startup
   {
       private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
       {
           private readonly IServiceProvider _serviceProvider;

           public SnapshotCollectorTelemetryProcessorFactory(IServiceProvider serviceProvider) =>
               _serviceProvider = serviceProvider;

           public ITelemetryProcessor Create(ITelemetryProcessor next)
           {
               var snapshotConfigurationOptions = _serviceProvider.GetService<IOptions<SnapshotCollectorConfiguration>>();
               return new SnapshotCollectorTelemetryProcessor(next, configuration: snapshotConfigurationOptions.Value);
           }
       }

       public Startup(IConfiguration configuration) => Configuration = configuration;

       public IConfiguration Configuration { get; }

       // This method gets called by the runtime. Use this method to add services to the container.
       public void ConfigureServices(IServiceCollection services)
       {
           // Configure SnapshotCollector from application settings
           services.Configure<SnapshotCollectorConfiguration>(Configuration.GetSection(nameof(SnapshotCollectorConfiguration)));

           // Add SnapshotCollector telemetry processor.
           services.AddSingleton<ITelemetryProcessorFactory>(sp => new SnapshotCollectorTelemetryProcessorFactory(sp));

           // TODO: Add other services your application needs here.
       }
   }
   ```

4. Anlık görüntü Toplayıcı appsettings.json için SnapshotCollectorConfiguration bölüm ekleyerek yapılandırın. Örneğin:

   ```json
   {
     "ApplicationInsights": {
       "InstrumentationKey": "<your instrumentation key>"
     },
     "SnapshotCollectorConfiguration": {
       "IsEnabledInDeveloperMode": true
     }
   }
   ```

### <a name="configure-snapshot-collection-for-other-net-applications"></a>Diğer .NET uygulamaları için anlık görüntü koleksiyonunu yapılandırma

1. Uygulamanızı Application Insights ile zaten izlenmemektedir, başlayın [Application Insights etkinleştirme ve izleme anahtarı ayarını](app-insights-windows-desktop.md).

2. Ekleme [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanıza NuGet paketi.

3. Anlık görüntüler için Application Insights bildirilen özel durum toplanır. Bunları raporlamak için kodunuzu değiştirmeniz gerekebilir. Özel durum işleme kodunu yapısını uygulamanızın bağlıdır, ancak bir örnek aşağıda verilmiştir:
    ```C#
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle the request.
        }
        catch (Exception ex)
        {
            // Report the exception to Application Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow the exception if desired.
        }
   }
    ```
    
## <a name="grant-permissions"></a>İzin ver

Azure abonelik sahipleri anlık görüntüleri inceleyebilirsiniz. Diğer kullanıcıların bir sahibi tarafından izin verilmesi gerekir.

İzin vermek için Ata `Application Insights Snapshot Debugger` anlık görüntüleri araştırmasını kullanıcılara rol. Bu rolü, bireysel kullanıcılar veya gruplar için Application Insights kaynağı hedef abonelik sahipleri tarafından veya kendi kaynak grubuna veya aboneliğe atanabilir.

1. Erişim denetimi (IAM) dikey penceresini açın.
1. Tıklatın + Ekle düğmesi.
1. Uygulama Öngörüler anlık görüntü hata ayıklayıcı rolleri aşağı açılan listeden seçin.
1. Arayın ve eklemek kullanıcı için bir ad girin.
1. Kullanıcı rolüne eklemek için Kaydet düğmesine tıklayın.


> [!IMPORTANT]
> Anlık görüntüler olası değişken ve parametre değerlerini kişisel ve diğer hassas bilgiler içerebilir.

## <a name="debug-snapshots-in-the-application-insights-portal"></a>Application Insights portalında anlık görüntüleri hata ayıklama

Belirli bir özel durum ya da bir sorun kimliği için bir anlık görüntü kullanılabiliyorsa, bir **açık hata ayıklama anlık görüntü** düğmesi görünür [özel durum](app-insights-asp-net-exceptions.md) Application Insights portalında.

![Özel durum açık hata ayıklama anlık görüntü düğmesine](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

Anlık görüntü hata ayıklama Görünümü'nde, çağrı yığını ve değişkenleri bölmesini görürsünüz. Çağrı yığını çerçeveler çağrı yığını Bölmesi'nde seçtiğinizde, yerel değişkenleri görüntüleyebilir ve bu işlev parametrelerini değişkenleri bölmesinde çağırın.

![Hata ayıklama görünümü anlık portalında](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

Varsayılan olarak görüntülenebilir olmadıkları ve anlık görüntüleri hassas bilgiler içerebilir. Anlık görüntüler görüntülemek için bilmeniz gereken `Application Insights Snapshot Debugger` rolü size atanmış.

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a>Visual Studio 2017 Enterprise sahip anlık görüntüleri hata ayıklama
1. Tıklatın **karşıdan anlık görüntü** karşıdan yüklemek için düğmeyi bir `.diagsession` Visual Studio 2017 kuruluş tarafından açılabilir dosya. 

2. Açmak için `.diagsession` dosya, şunları yapmalısınız ilk [anlık görüntü hata ayıklayıcısı uzantısı için Visual Studio yükleyip](https://aka.ms/snapshotdebugger).

3. Anlık görüntü dosyayı açtıktan sonra Visual Studio'da mini döküm hata ayıklama sayfası görüntülenir. Tıklatın **yönetilen kod hata ayıklama** anlık görüntü hata ayıklama başlatılamıyor. Anlık görüntü işlemi geçerli durumunu ayıklayabilirsiniz böylece burada özel durum oluştu kod satırına açar.

    ![Visual Studio'da hata ayıklama anlık görünümü](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

İndirilen anlık görüntü web uygulaması sunucunuzda bulunan tüm simge dosyaları içerir. Bu simge dosyaları, anlık görüntü verileri kaynak kodu ile ilişkilendirmek için gereklidir. Uygulama hizmeti uygulamalarınız için web uygulamaları yayımlarken simgesi dağıtım etkinleştirdiğinizden emin olun.

## <a name="how-snapshots-work"></a>Anlık görüntüler nasıl çalışır

Uygulamanız başladığında, ayrı anlık görüntü yükleyici işlemi, uygulamanız için anlık görüntü istekleri izleyen oluşturulur. Bir anlık görüntü istendiğinde, bir çalışan işlemi gölge kopyasını yaklaşık 10 ila 20 milisaniye cinsinden yapılır. Gölge işleminin ardından analiz ve çalıştırmanız ve kullanıcılar için trafiği hizmet ana işlem devam ederken bir anlık görüntüsü oluşturulur. Anlık görüntü, ardından Application Insights'a anlık görüntüyü görüntülemek için gereken ilgili simge (.pdb) dosyalarla birlikte yüklenir.

## <a name="current-limitations"></a>Geçerli sınırlamalar

### <a name="publish-symbols"></a>Simgeler yayımlama
Anlık görüntü hata ayıklayıcı değişkenleri kod çözme ve Visual Studio'da hata ayıklama deneyimini sağlamak üzere üretim sunucusunda simge dosyaları gerektirir. Uygulama hizmeti yayımladığında Visual Studio 2017 15.2 sürümü yayın derlemeleri simgelerini varsayılan olarak yayımlar. Önceki sürümlerde, aşağıdaki satırı yayımlama profilinizi eklemenize gerek `.pubxml` simgeleri yayın modunda yayımlanan dosyasını:

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

Azure işlem ve diğer türleri için simge dosyaları ana uygulama .dll aynı klasörde olduğundan emin olun (genellikle `wwwroot/bin`) ya da geçerli yolda kullanılabilir.

### <a name="optimized-builds"></a>En iyi duruma getirilmiş derlemeleri
Bazı durumlarda, yerel değişkenleri oluşturma işlemi sırasında uygulanan en iyi duruma getirme nedeniyle yayın derlemelerde görüntülenemiyor.

## <a name="troubleshooting"></a>Sorun giderme

Bu ipuçları anlık görüntü hata ayıklayıcısı ile sorunları gidermenize yardımcı olur.

### <a name="verify-the-instrumentation-key"></a>İzleme anahtarını doğrulayın

Yayımlanan uygulamanızda doğru izleme anahtarını kullandığınızdan emin olun. Genellikle, Application Insights izleme anahtarı Applicationınsights.config dosyasını okur. Değer Portalı'nda bkz Application Insights kaynağı izleme anahtarı ile aynı olduğunu doğrulayın.

### <a name="check-the-uploader-logs"></a>Yükleyici günlüklerini kontrol edin

Bir anlık görüntü oluşturulduktan sonra bir mini döküm dosyası (.dmp) disk üzerinde oluşturulur. Ayrı yükleyici işlem, mini döküm dosyası alır ve bunu uygulama Öngörüler anlık görüntü hata ayıklayıcı depolama ilişkili tüm pdb birlikte yükler. Mini döküm başarıyla yükledi sonra diskten silinir. Mini döküm Yükleyici günlük dosyaları diskte korunur. Bir uygulama hizmeti ortamı'nda, bu günlükler bulabilirsiniz `D:\Home\LogFiles\Uploader_*.log`. Bu günlük dosyaları bulmak için uygulama hizmeti Kudu yönetim sitesi kullanın.

1. Uygulama hizmeti uygulamanızı Azure Portal'da açın.

2. Seçin **Gelişmiş Araçlar** dikey veya arama **Kudu**.
3. tıklatın **Git**.
4. İçinde **hata ayıklama konsoluna** aşağı açılan liste kutusunda **CMD**.
5. Tıklatın **LogFiles**.

İle başlayan bir ada sahip en az bir dosya görmelisiniz `Uploader_` ve `.log` uzantısı. Tüm günlük dosyalarını indirin veya bir tarayıcıda açmak için uygun simgeye tıklayın.
Dosya adı makine adını içerir. Uygulama hizmeti örneğinizi birden fazla makine üzerinde barındırılıyorsa, her makine için farklı günlük dosyaları vardır. Yeni bir mini döküm dosyası karşıya yükleyen algıladığında, günlük dosyasına kaydedilir. Başarılı bir karşıya yükleme bir örneği burada verilmiştir:

```
MinidumpUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:08.0349846Z
MinidumpUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 329.12 MB
    DateTime=2017-05-25T14:25:16.0145444Z
MinidumpUploader.exe Information: 0 : Upload successful.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2017-05-25T14:25:44.2310982Z
MinidumpUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2017-05-25T14:25:44.5435948Z
MinidumpUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:44.6095821Z
```

İzleme anahtarını önceki örnekte olduğu `c12a605e73c44346a984e00000000000`. Bu değer, uygulamanız için izleme anahtarını eşleşmelidir.
Mini döküm Kimliğine sahip bir anlık görüntüsü ile ilişkili `139e411a23934dc0b9ea08a626db16c5`. Daha sonra uygulama Öngörüler analizleri ilişkili özel durum telemetrisi bulmak için bu kodu kullanabilirsiniz.

Karşıya yükleyen her 15 dakikada hakkında yeni pdb tarar. Örnek aşağıda verilmiştir:

```
MinidumpUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\home\site\wwwroot\ for local PDBs.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\local\Temporary ASP.NET Files\root\a6554c94\e3ad6f22\assembly\dl3\81d5008b\00b93cc8_dec5d201 for local PDBs.
    DateTime=2017-05-25T15:11:38.8160276Z
MinidumpUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2017-05-25T15:11:38.8316450Z
MinidumpUploader.exe Information: 0 : Deleted PDB scan marker D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\.pdbscan.
    DateTime=2017-05-25T15:11:38.8316450Z
```

Uygulamalar için _değil_ App Service içinde barındırılan, yükleyici Mini dökümler ile aynı klasörde günlüklerin: `%TEMP%\Dumps\<ikey>` (burada `<ikey>` araçları anahtarınız).

Bulut Hizmetleri rolleri için varsayılan geçici klasör mini döküm dosyası tutamayacak kadar küçük olabilir. Bu durumda, alternatif bir klasör TempFolder özelliği aracılığıyla Applicationınsights.Config'de belirtebilirsiniz.

```xml
<TelemetryProcessors>
  <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
    <!-- Use an alternative folder for minidumps -->
    <TempFolder>C:\Snapshots\Go\Here</TempFolder>
    </Add>
</TelemetryProcessors>
```

### <a name="use-application-insights-search-to-find-exceptions-with-snapshots"></a>Anlık görüntüler istisnalar bulmak için Application Insights arama kullanın

Bir anlık görüntü oluşturulduğunda oluşturma özel durum ile bir anlık görüntü kimliği etiketli Ne zaman özel durum telemetrisi Application Insights için anlık görüntü kimliği bir özel özellik olarak dahil olduğunu bildirdi. Arama dikey Application Insights'ta kullanarak, tüm telemetri ile bulabilirsiniz `ai.snapshot.id` özel özellik.

1. Azure portalında Application Insights kaynağınıza göz atın.
2. Tıklatın **arama**.
3. Tür `ai.snapshot.id` arama metin kutusuna ve Enter tuşuna basın.

![Bir anlık görüntü kimliği portalındaki telemetriyle arayın](./media/app-insights-snapshot-debugger/search-snapshot-portal.png)

Bu arama sonuç döndürürse, hiç anlık görüntü seçili zaman aralığı içinde uygulamanız için Application Insights bildirildi.

Yükleyici günlüklerini belirli bir anlık görüntüye Kimliğinden aramak için arama kutusunu bu kimliği yazın. Karşıya yüklenen bildiğiniz bir anlık görüntü için telemetri bulamazsanız, aşağıdaki adımları izleyin:

1. İzleme anahtarını doğrulayarak sağ Application Insights kaynağı arıyorsanız denetleyin.

2. Zaman damgası yükleyici günlüğündeki kullanarak, bu zaman aralığı karşılamak üzere arama zaman aralığı filtre ayarlayın.

Bu anlık görüntü Kimliğine sahip bir özel durum hala göremiyorsanız, özel durum telemetrisi Application Insights'a bildirilen değildi. Bu durum, anlık görüntü sürdü sonra uygulamanızın kilitlendi, ancak özel durum telemetrisi bildirilen önce gerçekleşebilir. Bu durumda, altında uygulama hizmeti günlüklerini kontrol `Diagnose and solve problems` beklenmeyen yeniden başlatmalar olup olmadığını görmek için veya işlenmeyen özel durum.

## <a name="next-steps"></a>Sonraki adımlar

* [Kodunuzda snappoints ayarlamak](https://docs.microsoft.com/visualstudio/debugger/debug-live-azure-applications) için bir özel durum beklemeden anlık görüntüleri alınamıyor.
* [Özel durumlar, web uygulamalarında tanılama](app-insights-asp-net-exceptions.md) daha fazla özel durumlar Application Insights tarafından görülebilmesi için açıklanmaktadır. 
* [Akıllı algılama](app-insights-proactive-diagnostics.md) performans anormalliklerini otomatik olarak bulur.
