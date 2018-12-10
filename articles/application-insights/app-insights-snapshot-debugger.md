---
title: Azure Application Insights anlık görüntü hata ayıklayıcısı için .NET uygulamaları | Microsoft Docs
description: Hata ayıklama anlık görüntülerini üretim .NET uygulamalarında özel durumlar oluşturulduğunda otomatik olarak toplanır
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/08/2018
ms.reviewer: pharring
ms.author: mbullwin
ms.openlocfilehash: a92b54a80de645dda8ea0cc0259bd07f72330204
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53136724"
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>.NET uygulamalarında özel durumlarda anlık görüntü hata ayıklama

Bir özel durum oluştuğunda, hata ayıklama anlık görüntüsünü canlı web uygulamanızı otomatik olarak toplayabilirsiniz. Anlık görüntü, özel durumun oluştuğu şu anda kaynak kodu ve değişkenleri durumunu gösterir. Snapshot Debugger (Önizleme) içinde [Azure Application Insights](app-insights-overview.md) web uygulamanızdan özel telemetri izler. Böylece, üretim sorunlarını tanılamak ihtiyacınız olan bilgileri sahip anlık görüntüleri, üst özel durum atma özel durumları toplar. Dahil [Snapshot collector NuGet paketini](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanızda ve isteğe bağlı olarak koleksiyon parametrelerinde yapılandırma [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md). Anlık görüntüler görüntülenerek [özel durumları](app-insights-asp-net-exceptions.md) Application Insights portalında.

Hata ayıklama anlık görüntülerini portalda görüntüleyerek çağrı yığınını görebilir ve her bir çağrı yığını çerçevesinde değişkenleri inceleyebilirsiniz. Kaynak koduyla birlikte daha güçlü bir hata ayıklama deneyimi elde etmek için Visual Studio 2017 Enterprise ile anlık görüntüleri açmak. Visual Studio'da ayrıca [etkileşimli anlık görüntülerini almak için anlık görüntü noktaları ayarlamak](https://aka.ms/snappoint) olmadan için bir özel durum bekleniyor.

Hata ayıklama anlık görüntüleri yedi gün boyunca saklanır. Bu bekletme ilkesi, bir uygulama başına temelinde ayarlanır. Bu değeri arttırmak gerekiyorsa, Azure portalında bir destek talebi açarak artışı isteyebilirsiniz.

Anlık görüntü koleksiyonu için kullanılabilir:
* .NET Framework 4.5 veya üzeri çalışan .NET framework ve ASP.NET uygulamaları.
* Windows üzerinde çalışan .NET core 2.0 ve ASP.NET Core 2.0 uygulamaları.

Şu ortamlarda desteklenir:
* Azure uygulama hizmeti.
* İşletim sistemi ailesi 4 veya sonraki sürümlerini çalıştıran Azure bulut hizmeti.
* Windows Server 2012 R2 veya sonraki sürümlerde çalışan azure Service Fabric Hizmetleri.
* Windows Server 2012 R2 çalıştıran Azure sanal makineleri veya üzeri.
* Şirket içi Windows Server 2012 R2 çalıştıran sanal veya fiziksel makinelere veya üzeri.

> [!NOTE]
> İstemci uygulamaları (örneğin, WPF, Windows Forms veya UWP) desteklenmez.

### <a name="configure-snapshot-collection-for-aspnet-applications"></a>ASP.NET uygulamaları için anlık görüntü koleksiyonunu yapılandırma

1. [Web uygulamanızda Application Insights'ı etkinleştirmek](app-insights-asp-net.md), henüz yapmadınız.

2. Dahil [Microsoft.applicationınsights.snapshotcollectya da](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanıza NuGet paketi.

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
        <ThresholdForSnapshotting>1</ThresholdForSnapshotting>
        <!-- The maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- The maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often we reconnect to the stamp. The default value is 15 minutes.-->
        <ReconnectInterval>00:15:00</ReconnectInterval>
        <!-- How often to reset problem counters. -->
        <ProblemCounterResetInterval>1.00:00:00</ProblemCounterResetInterval>
        <!-- The maximum number of snapshots allowed in ten minutes.The default value is 1. -->
        <SnapshotsPerTenMinutesLimit>3</SnapshotsPerTenMinutesLimit>
        <!-- The maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>30</SnapshotsPerDayLimit>
        <!-- Whether or not to collect snapshot in low IO priority thread. The default value is true. -->
        <SnapshotInLowPriorityThread>true</SnapshotInLowPriorityThread>
        <!-- Agree to send anonymous data to Microsoft to make this product better. -->
        <ProvideAnonymousTelemetry>true</ProvideAnonymousTelemetry>
        <!-- The limit on the number of failed requests to request snapshots before the telemetry processor is disabled. -->
        <FailedRequestLimit>3</FailedRequestLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. Application ınsights'ı raporlanan özel durumların anlık görüntülerini toplanır. Bazı durumlarda (örneğin, eski sürümleri .NET platformu) için gereksinim duyabileceğiniz [özel durum koleksiyonunu yapılandırma](app-insights-asp-net-exceptions.md#exceptions) portalında anlık görüntülerle özel durumları görmek için.


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a>ASP.NET Core 2.0 uygulamaları için anlık görüntü koleksiyonunu yapılandırma

1. [ASP.NET Core web uygulamanızı Application Insights'ı etkinleştirme](app-insights-asp-net-core.md), henüz yapmadınız.

    > [!NOTE]
    > Uygulamanızı sürüm 2.1.1 başvuran emin veya Microsoft.ApplicationInsights.AspNetCore paketin daha yeni olması.

2. Dahil [Microsoft.applicationınsights.snapshotcollectya da](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanıza NuGet paketi.

3. Uygulamanızın değiştirme `Startup` eklemek ve anlık görüntü toplayıcının telemetri işlemci yapılandırmak için sınıf.

    Aşağıdaki using deyimlerini `Startup.cs`

   ```csharp
   using Microsoft.ApplicationInsights.SnapshotCollector;
   using Microsoft.Extensions.Options;
   using Microsoft.ApplicationInsights.AspNetCore;
   using Microsoft.ApplicationInsights.Extensibility;
   ```

   Aşağıdaki `SnapshotCollectorTelemetryProcessorFactory` sınıfının `Startup` sınıfı.

   ```csharp
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
       ...
    ```
    Ekleme `SnapshotCollectorConfiguration` ve `SnapshotCollectorTelemetryProcessorFactory` başlangıç ardışık düzenine Hizmetleri:

    ```csharp
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

4. Anlık görüntü toplayıcının için appsettings.json SnapshotCollectorConfiguration bölüm ekleyerek yapılandırın. Örneğin:

   ```json
   {
     "ApplicationInsights": {
       "InstrumentationKey": "<your instrumentation key>"
     },
     "SnapshotCollectorConfiguration": {
       "IsEnabledInDeveloperMode": false,
       "ThresholdForSnapshotting": 1,
       "MaximumSnapshotsRequired": 3,
       "MaximumCollectionPlanSize": 50,
       "ReconnectInterval": "00:15:00",
       "ProblemCounterResetInterval":"1.00:00:00",
       "SnapshotsPerTenMinutesLimit": 1,
       "SnapshotsPerDayLimit": 30,
       "SnapshotInLowPriorityThread": true,
       "ProvideAnonymousTelemetry": true,
       "FailedRequestLimit": 3
     }
   }
   ```

### <a name="configure-snapshot-collection-for-other-net-applications"></a>Diğer .NET uygulamaları için anlık görüntü koleksiyonunu yapılandırma

1. Uygulamanızı Application Insights ile izlenen olması değil, başlayın [Application Insights'ı etkinleştirme ve izleme anahtarı ayarını](app-insights-windows-desktop.md).

2. Ekleme [Microsoft.applicationınsights.snapshotcollectya da](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanıza NuGet paketi.

3. Application ınsights'ı raporlanan özel durumların anlık görüntülerini toplanır. Bunları rapor için kodunuzu değiştirmeniz gerekebilir. Özel durum işleme kodunu uygulamanızın yapısına bağlıdır, ancak bir örnek aşağıda verilmiştir:
    ```csharp
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

## <a name="grant-permissions"></a>İzinleri verme

Anlık görüntüleri erişim, rol tabanlı erişim denetimi (RBAC) tarafından korunur. Bir anlık görüntü incelemek için önce gerekli rol için bir abonelik sahibi tarafından eklenmelidir.

> [!NOTE]
> Otomatik olarak sahipleri ve katkıda bulunanların bu role sahip değilsiniz. Bunlar anlık görüntüleri görüntülemek istiyorsanız, kendilerini rolüne eklemelisiniz.

Abonelik sahipleri Ata `Application Insights Snapshot Debugger` rol kullanıcılara anlık görüntülerini inceler. Bu rol, bireysel kullanıcılar veya gruplar için Application Insights kaynağı hedef abonelik sahipleri tarafından veya kendi kaynak grubuna veya aboneliğe atanabilir.

1. Azure portalında Application Insights kaynağına gidin.
1. Tıklayın **erişim denetimi (IAM)**.
1. Tıklayın **+ rol ataması Ekle** düğmesi.
1. Seçin **Application Insights Snapshot Debugger** gelen **rolleri** aşağı açılan listesi.
1. Arayın ve kullanıcı eklemek için bir ad girin.
1. Tıklayın **Kaydet** rolüne kullanıcı eklemek için Ekle düğmesine.


> [!IMPORTANT]
> Anlık görüntüler, potansiyel olarak değişkeni ve parametre değerlerini kişisel ve hassas bilgileri içerebilir.

## <a name="debug-snapshots-in-the-application-insights-portal"></a>Application Insights portalında anlık görüntü hata ayıklama

Belirli bir özel durum veya bir sorun kimliği için bir anlık görüntü kullanılabiliyorsa bir **Aç hata ayıklama anlık görüntüsü** düğmesinin üzerindeki [özel durum](app-insights-asp-net-exceptions.md) Application Insights portalında.

![Özel durum hata ayıklama anlık görüntüsü düğmesinde Aç](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

Anlık görüntü hata ayıklama Görünümü'nde, çağrı yığını ve değişkenleri bölmesini görürsünüz. Çağrı yığını Bölmesi'nde çerçeve çağrı yığınının seçtiğinizde, yerel değişkenleri görüntüleyebilir ve bu işlevin parametreleri değişkenleri Bölmesi'nde çağırın.

![Portalı'nda hata ayıklama anlık görüntüsünü görüntüle](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

Anlık görüntüler, hassas bilgiler içerebilir ve görüntülenebilir olmayan varsayılan olarak. Anlık görüntüleri görüntülemek için olmalıdır `Application Insights Snapshot Debugger` size atanan rol.

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a>Visual Studio 2017 Enterprise ile anlık görüntü hata ayıklama
1. Tıklayın **anlık görüntüyü indir** indirmek için düğmeye bir `.diagsession` dosyasını Visual Studio 2017 Enterprise tarafından açılabilir.

2. Açmak için `.diagsession` dosya anlık görüntü hata ayıklayıcısı VS bileşeninin yüklü olması gerekir. Anlık görüntü hata ayıklayıcı bileşeni ASP.net iş yükünü VS gerekli bir bileşenidir ve VS yükleyici tek tek bileşenler listesinden seçilebilir. Visual Studio sürümü 15.5 önce kullanıyorsanız uzantısını yüklemeniz gerekir [VS Market'te](https://aka.ms/snapshotdebugger).

3. Anlık görüntü dosyası açtıktan sonra Visual Studio'da mini döküm hata ayıklama sayfası görüntülenir. Tıklayın **hata ayıklama yönetilen kodu** anlık görüntü hata ayıklama başlatılamıyor. Anlık görüntü geçerli işlemin durumunu ayıklayabilirsiniz, burada özel durumun oluştuğu kod satırına açılır.

    ![Visual Studio'da hata ayıklama anlık görüntüsünü görüntüle](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

İndirilen anlık görüntü, web uygulama sunucunuzda bulunan herhangi bir sembol dosyalarını içerir. Bu sembol dosyaları, kaynak kod ile anlık görüntü verileri ilişkilendirmek için gereklidir. App Service uygulamaları için web uygulamalarınızı yayımladığınızda, sembol dağıtımını etkinleştirmek emin olun.

## <a name="how-snapshots-work"></a>Anlık görüntüleri nasıl çalışır?

Anlık görüntü toplayıcının olarak uygulanan bir [Application Insights Telemetri İşlemci](app-insights-configuration-with-applicationinsights-config.md#telemetry-processors-aspnet). Uygulamanız çalışırken, anlık görüntü toplayıcısı Telemetri işlemci, uygulamanızın telemetri ardışık düzenine eklenen.
Her zaman uygulama çağrılarınızı [TrackException](app-insights-asp-net-exceptions.md#exceptions), oluşturulan özel durum ve oluşturma yöntemini türünden bir sorun kimliği anlık görüntü toplayıcının hesaplar.
Uygulamanızı çağırır TrackException, her zaman uygun sorun kimliği için bir sayaç artırılır Sayaç ulaştığında `ThresholdForSnapshotting` değeri, sorun kimliği, bir koleksiyon planı'na eklenir.

Anlık görüntü toplayıcının bunlar abone tarafından oluşturulan gibi ayrıca özel durumları izleyen [AppDomain.CurrentDomain.FirstChanceException](https://docs.microsoft.com/dotnet/api/system.appdomain.firstchanceexception) olay. Bu olayı tetikler, özel durum sorun kimliği hesaplanan ve koleksiyon planı içinde sorun kimlikleri karşı karşılaştırılır.
Ardından bir eşleşme varsa, çalışan işlem anlık görüntüsünü oluşturulur. Anlık görüntü benzersiz bir tanımlayıcı atanır ve özel durum tanımlayıcı ile damgalandı. FirstChanceException işleyici döndürdükten sonra oluşan özel durum normal şekilde işlenir. Sonuç olarak, özel durum yeniden TrackException yöntemi, Application Insights anlık görüntü tanımlayıcısı ile birlikte, burada bildirilir ulaşır.

Ana işlem, çalıştırmak ve trafiğin az kesinti ile kullanıcılara hizmet devam eder. Bu arada, anlık görüntü kapatmak için anlık görüntü yükleyici işlemi devredildiği. Anlık görüntü karşıya yükleyen bir mini döküm dosyası oluşturur ve tüm ilgili sembol (.pdb) dosyalarını yanı sıra Application ınsights'ı yükler.

> [!TIP]
> - Çalışan işlemi askıya alınmış bir kopyasını işlem anlık görüntüsüdür.
> - Anlık görüntü oluşturmak yaklaşık 10-20 milisaniye cinsinden alır.
> - İçin varsayılan değer `ThresholdForSnapshotting` 1'dir. Bu ayrıca en küçük değerdir. Bu nedenle, aynı özel durum tetiklemek uygulamanızı sahip **iki kez** anlık görüntüsünü oluşturmadan önce.
> - Ayarlama `IsEnabledInDeveloperMode` Visual Studio'da hata ayıklama sırasında anlık görüntü oluşturmak istiyorsanız True.
> - Anlık görüntü oluşturma oranı sınırlıdır `SnapshotsPerTenMinutesLimit` ayarı. Varsayılan olarak, bir anlık görüntü her on dakikada bir sınırdır.
> - Günde en fazla 50 anlık görüntüleri karşıya yüklenmeyebilir.

## <a name="current-limitations"></a>Geçerli sınırlamalar

### <a name="publish-symbols"></a>Yayım simgeleri
Snapshot Debugger, değişkenleri çözülecek ve Visual Studio'da hata ayıklama bir deneyim sağlamak üzere üretim sunucusundaki sembol dosyalarını gerektiri.
Sürüm 15.2 (veya üstü) App Service'te yayımlarken, varsayılan olarak sürüm yapıları için Visual Studio 2017 için semboller yayımlar. Önceki sürümlerde, aşağıdaki satırı yayımlama profilinizi eklemeniz gereken `.pubxml` sembolleri yayın modunda yayımlanır böylece dosya:

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

Azure işlem ve diğer türleri için Sembol dosyaları ana uygulama .dll aynı klasörde olduğundan emin olun (genellikle `wwwroot/bin`) ya da geçerli yolda kullanılabilir.

### <a name="optimized-builds"></a>En iyi duruma getirilmiş derlemeleri
Bazı durumlarda, yerel değişkenler, JIT derleyicisi tarafından uygulanan en iyi duruma getirme nedeniyle sürüm yapılarında görüntülenemiyor.
Ancak, Azure uygulama hizmetleri, anlık görüntü toplayıcının koleksiyon planı parçası olan oluşturma yöntemleri deoptimize.

> [!TIP]
> Application Insights Site uzantısının deoptimization destek almak için App Service içinde yükleyin.

## <a name="troubleshooting"></a>Sorun giderme

Bu ipuçlarını anlık görüntü hata ayıklayıcısı ile sorunları gidermenize yardımcı olur.

### <a name="use-the-snapshot-health-check"></a>Anlık görüntü sistem durumu denetimini kullanma
Bazı yaygın sorunlar Aç hata ayıklama gösterilmiyor anlık sonuçlanır. Bir tarihi geçmiş anlık görüntü toplayıcının, örneğin kullanma; Günlük karşıya yükleme sınırına ulaşması; veya belki de anlık görüntü yalnızca karşıya yüklemek için bir uzun sürüyor. Sık karşılaşılan sorunları gidermek için anlık görüntü sistem durumu denetimi kullanın.

Uçtan uca izleme görünümünün anlık görüntü sistem durumu denetimi alan özel durum bölmesinde bir bağlantı yoktur.

![Anlık görüntü durum denetimi girin](./media/app-insights-snapshot-debugger/enter-snapshot-health-check.png)

Etkileşimli, sohbet benzeri arabirimi, sık karşılaşılan sorun için arar ve bunları düzeltmek için size yol gösterir.

![Sistem durumu denetimi](./media/app-insights-snapshot-debugger/healthcheck.png)

Ardından, sorunu çözmezse, sorun giderme adımları aşağıdaki kılavuzuna başvurun.

### <a name="verify-the-instrumentation-key"></a>İzleme anahtarını doğrulayın

Yayımlanan uygulamanızı doğru izleme anahtarını kullandığınızdan emin olun. Genellikle, izleme anahtarı Applicationınsights.config dosyasından okunur. Değer portalında gördüğünüz Application Insights kaynağı için izleme anahtarı ile aynı olduğunu doğrulayın.

### <a name="upgrade-to-the-latest-version-of-the-nuget-package"></a>NuGet paketinin en son sürüme yükseltme

Microsoft.applicationınsights.snapshotcollectya da en son sürümünü kullandığınızdan emin olmak için Visual Studio'nun NuGet Paket Yöneticisi'ni kullanın. Sürüm Notları şu yolda bulunabilir: https://github.com/Microsoft/ApplicationInsights-Home/issues/167

### <a name="check-the-uploader-logs"></a>Yükleyici günlükleri denetleyin

Bir mini döküm dosyası (.dmp), bir anlık görüntü oluşturulduktan sonra disk üzerinde oluşturulur. Ayrı yükleyici işlemi bu mini döküm dosyası oluşturur ve bunu, Application Insights Snapshot Debugger depolama ilişkili tüm pdb birlikte yükler. Mini döküm dosyası başarıyla karşıya yüklendikten sonra bu diskinden silinir. Yükleyici işlem için günlük dosyaları diskte tutulur. Bir App Service ortamında, bu günlüklerde bulabilirsiniz `D:\Home\LogFiles`. Bu günlük dosyaları bulmak için App Service Kudu yönetim sitesi kullanın.

1. App Service uygulamanızı Azure portalında açın.
2. Tıklayın **Gelişmiş Araçlar**, veya arama **Kudu**.
3. Tıklayın **Git**.
4. İçinde **hata ayıklama konsoluna** aşağı açılan liste kutusunda **CMD**.
5. Tıklayın **LogFiles**.

İle başlayan bir ada sahip en az bir dosya görmeniz gerekir `Uploader_` veya `SnapshotUploader_` ve `.log` uzantısı. Tüm günlük dosyalarını indirin veya bunları bir tarayıcıda açmak için uygun simgeye tıklayın.
App Service örneğine tanımlayan benzersiz bir son eke dosya adını içerir. App Service örneğinizin birden fazla makine üzerinde barındırılıyorsa, her makine için ayrı günlük dosyası vardır. Karşıya yükleyen yeni bir mini döküm dosyası algıladığında, bu günlük dosyasına kaydedilir. Başarılı bir anlık görüntü ve karşıya yükleme örneği aşağıda verilmiştir:

```
SnapshotUploader.exe Information: 0 : Received Fork request ID 139e411a23934dc0b9ea08a626db16c5 from process 6368 (Low pri)
    DateTime=2018-03-09T01:42:41.8571711Z
SnapshotUploader.exe Information: 0 : Creating minidump from Fork request ID 139e411a23934dc0b9ea08a626db16c5 from process 6368 (Low pri)
    DateTime=2018-03-09T01:42:41.8571711Z
SnapshotUploader.exe Information: 0 : Dump placeholder file created: 139e411a23934dc0b9ea08a626db16c5.dm_
    DateTime=2018-03-09T01:42:41.8728496Z
SnapshotUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:45.7525022Z
SnapshotUploader.exe Information: 0 : Successfully wrote minidump to D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:45.7681360Z
SnapshotUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 214.42 MB (uncompressed)
    DateTime=2018-03-09T01:42:45.7681360Z
SnapshotUploader.exe Information: 0 : Upload successful. Compressed size 86.56 MB
    DateTime=2018-03-09T01:42:59.6184651Z
SnapshotUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2018-03-09T01:42:59.6184651Z
SnapshotUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2018-03-09T01:42:59.6809606Z
SnapshotUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2018-03-09T01:42:59.8059929Z
SnapshotUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:59.8530649Z
```

> [!NOTE]
> Yukarıdaki örnekte Microsoft.applicationınsights.snapshotcollectya da NuGet Paketi 1.2.0 sürümüne bulunur. Önceki sürümlerde, yükleyici işlemi olarak adlandırılır `MinidumpUploader.exe` ve günlük daha az ayrıntılı olarak verilmiştir.

Önceki örnekte, izleme anahtarını olan `c12a605e73c44346a984e00000000000`. Bu değer, uygulamanızın izleme anahtarını eşleşmesi gerekir.
Mini döküm dosyasında bir anlık görüntü kimliği ile ilişkili olduğu `139e411a23934dc0b9ea08a626db16c5`. Application Insights Analytics ilişkili özel durum telemetrisi bulmak için bu kimliği daha sonra kullanabilirsiniz.

Karşıya yükleyen her 15 dakikada hakkında yeni pdb tarar. Bir örneği aşağıda verilmiştir:

```
SnapshotUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2018-03-09T01:47:19.4457768Z
SnapshotUploader.exe Information: 0 : Scanning D:\home\site\wwwroot for local PDBs.
    DateTime=2018-03-09T01:47:19.4457768Z
SnapshotUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2018-03-09T01:47:19.4614027Z
SnapshotUploader.exe Information: 0 : Deleted PDB scan marker : D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\6368.pdbscan
    DateTime=2018-03-09T01:47:19.4614027Z
```

Uygulamalar için _olmayan_ App Service'te barındırılan, mini döküm dosyalarında aynı klasörde yükleyici günlükleri şunlardır: `%TEMP%\Dumps\<ikey>` (burada `<ikey>` izleme anahtarınız).

### <a name="troubleshooting-cloud-services"></a>Bulut Hizmetleri sorunlarını giderme
Bulut hizmetlerindeki roller için varsayılan geçici klasör kayıp anlık görüntüleri için önde gelen mini döküm dosyaları tutmak için çok küçük olabilir.
İhtiyaç duyulan alanı uygulamanızı eşzamanlı anlık görüntü sayısı ve toplam çalışma kümesine bağlıdır.
Çalışma kümesi 32-bit ASP.NET web rolü, genellikle 200 MB ile en fazla 500 MB arasındadır.
En az iki eş zamanlı anlık görüntüler için izin verin.
Örneğin, uygulamanız çalışma kümesi toplam 1 GB kullanıyorsa, en az 2 anlık görüntülerini depolamak için GB disk alanı olduğundan emin olmalısınız.
Anlık görüntüler için ayrılmış bir yerel kaynak ile bulut hizmeti rolünü yapılandırmak için aşağıdaki adımları izleyin.

1. Yeni bir yerel kaynak, bulut hizmet tanımı (.csdef) dosyasını düzenleyerek bulut hizmetinize ekleyin. Aşağıdaki örnek adlı bir kaynak tanımlar `SnapshotStore` boyutu 5 GB.
   ```xml
   <LocalResources>
     <LocalStorage name="SnapshotStore" cleanOnRoleRecycle="false" sizeInMB="5120" />
   </LocalResources>
   ```

2. İşaret eden bir ortam değişkeni eklemek için rolün başlatma kodunu değiştirmek `SnapshotStore` yerel kaynak. Çalışan rolleri için kodu, rolün eklenmelidir `OnStart` yöntemi:
   ```csharp
   public override bool OnStart()
   {
       Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
       return base.OnStart();
   }
   ```
   Web rolleri (ASP.NET), web uygulamanızın için kod eklenmelidir `Application_Start` yöntemi:
   ```csharp
   using Microsoft.WindowsAzure.ServiceRuntime;
   using System;

   namespace MyWebRoleApp
   {
       public class MyMvcApplication : System.Web.HttpApplication
       {
          protected void Application_Start()
          {
             Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
             // TODO: The rest of your application startup code
          }
       }
   }
   ```

3. Tarafından kullanılan geçici klasör konumu geçersiz kılmak için rolün Applicationınsights.config dosyasını güncelleştirme `SnapshotCollector`
   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Use the SnapshotStore local resource for snapshots -->
      <TempFolder>%SNAPSHOTSTORE%</TempFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

### <a name="overriding-the-shadow-copy-folder"></a>Gölge kopya klasörü geçersiz kılma

Anlık görüntü toplayıcının başlatıldığında anlık görüntü yükleyici işlemi çalıştırmak için uygun diskte bir klasörü bulmayı dener. Seçtiğiniz klasör gölge kopya klasör olarak bilinir.

Anlık görüntü toplayıcının, bazı iyi bilinen konumları, anlık görüntü Uploader ikili dosyalarını kopyalamak için izinlere sahip sağlamaktan denetler. Aşağıdaki ortam değişkenlerini kullanılır:
- Fabric_Folder_App_Temp
- LOCALAPPDATA
- APPDATA
- TEMP

Uygun bir klasör bulunamazsa, anlık görüntü toplayıcının hata bildiren raporları _"uygun gölge kopya klasörü bulunamadı."_

Kopyalama başarısız olursa, anlık görüntü toplayıcısı raporları bir `ShadowCopyFailed` hata.

Yükleyici başlatılamıyor ise, anlık görüntü toplayıcısı raporları bir `UploaderCannotStartFromShadowCopy` hata. Genellikle ileti gövdesini içeren `System.UnauthorizedAccessException`. Bu hata genellikle uygulama sınırlı izinlere sahip bir hesap altında çalıştığı için oluşur. Hesabın gölge kopya klasörüne yazmak için yeterli izne sahip, ancak kod yürütmek için izinlere sahip değil.

Bu hatalar genellikle başlatma sırasında meydana olduğundan, bunlar genellikle tarafından izlenmesi bir `ExceptionDuringConnect` hata bildiren _"Yükleyicisi başlatılamadı."_

Bu hataların geçici olarak çözmek için gölge kopya klasörü kullanarak el ile belirtebilirsiniz `ShadowCopyFolder` yapılandırma seçeneği. Örneğin, Applicationınsights.config kullanma:

   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Override the default shadow copy folder. -->
      <ShadowCopyFolder>D:\SnapshotUploader</ShadowCopyFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

Veya, bir .NET Core uygulaması ile appsettings.json kullanıyorsanız:

   ```json
   {
     "ApplicationInsights": {
       "InstrumentationKey": "<your instrumentation key>"
     },
     "SnapshotCollectorConfiguration": {
       "ShadowCopyFolder": "D:\\SnapshotUploader"
     }
   }
   ```

### <a name="use-application-insights-search-to-find-exceptions-with-snapshots"></a>Application ınsights'ı özel durumların anlık görüntülerle bulmak için arama yapın

Anlık görüntü oluşturulduğunda, özel durum oluşturmaya bir anlık görüntü kimliği ile etiketlenir. Özel durum telemetrisi Application Insights'a bildirildiğinde bu anlık görüntü kimliği bir özel özellik olarak dahil edilir. Kullanarak **arama** Application Insights ile tüm telemetri bulabilirsiniz `ai.snapshot.id` özel özellik.

1. Azure portalında Application Insights kaynağınıza göz atın.
2. **Ara**'ya tıklayın.
3. Tür `ai.snapshot.id` arama metin kutusu ve Enter tuşuna basın.

![Portalda bir anlık görüntü kimliği ile telemetri arayın](./media/app-insights-snapshot-debugger/search-snapshot-portal.png)

Bu arama sonuç vermedi, anlık görüntü yok seçili zaman aralığındaki uygulamanız için Application ınsights'ı bildirildi.

Yükleyici günlükleri belirli bir anlık görüntüye Kimliğinden aramak için bu kimliği arama kutusuna yazın. Karşıya yüklenen bildiğiniz bir anlık görüntü için telemetri bulamazsanız, aşağıdaki adımları izleyin:

1. İzleme anahtarını doğrulayarak doğru Application Insights kaynağı arıyorsanız denetleyin.

2. Yükleyici günlük itibaren zaman damgası kullanarak, o zaman aralığınızı kapsayacak şekilde arama zaman aralığı filtresini ayarlayın.

Bu anlık görüntü Kimliğe sahip bir özel durum yine de görmüyorsanız, özel durum telemetrisi Application ınsights'ı bildirilen değildi. Bu durum, anlık görüntü sürdüğünü sonra uygulamanızı kilitlendi, ancak özel durum telemetrisi bildirilen önce gerçekleşebilir. Bu durumda, App Service günlükleri altında denetleyin `Diagnose and solve problems` beklenmeyen yeniden başlatmaları olup olmadığını görmek için veya işlenmemiş özel durumlar.

### <a name="edit-network-proxy-or-firewall-rules"></a>Ağ proxy veya güvenlik duvarı kurallarını Düzenle

Uygulamanız İnternet'e bir ara sunucu veya bir güvenlik duvarı üzerinden bağlanıyorsa, Snapshot Debugger hizmetiyle iletişim kurmak için uygulamanıza izin vermek için kuralları düzenlemek gerekebilir. İşte [IP adresleri ve anlık görüntü hata ayıklayıcısı tarafından kullanılan bağlantı noktaları listesini](app-insights-ip-addresses.md#snapshot-debugger).

## <a name="next-steps"></a>Sonraki adımlar

* [Anlık görüntü noktaları kodunuzda ayarlayın](https://docs.microsoft.com/visualstudio/debugger/debug-live-azure-applications) için bir özel durum beklemeden anlık görüntüler alınacak.
* [Web uygulamalarınızda özel durumları tanılama](app-insights-asp-net-exceptions.md) daha fazla özel durum Application Insights tarafından görülebilmesi açıklanmaktadır.
* [Akıllı algılama](app-insights-proactive-diagnostics.md) performans anormalliklerini otomatik olarak bulur.
