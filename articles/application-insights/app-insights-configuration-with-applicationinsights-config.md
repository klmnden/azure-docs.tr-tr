---
title: "Applicationınsights.config başvuru - Azure | Microsoft Docs"
description: "Etkinleştirmek veya veri toplama modülleri devre dışı bırakın ve performans sayaçları ve diğer parametreleri ekleyin."
services: application-insights
documentationcenter: 
author: OlegAnaniev-MSFT
editor: mrbullwinkle
manager: carmonm
ms.assetid: 6e397752-c086-46e9-8648-a1196e8078c2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: mbullwin
ms.openlocfilehash: a35da5c84e4e79d7bc6f2167ec7e172970992612
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>ApplicationInsights.config veya .xml ile Application Insights SDK yapılandırma
Application Insights .NET SDK'sı bir NuGet paketlerini oluşur. [Çekirdek paket](http://www.nuget.org/packages/Microsoft.ApplicationInsights) Application Insights telemetri göndermek için API sağlar. [Ek paket](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) telemetri sağlamak *modülleri* ve *başlatıcıları* telemetri uygulamanız ve onun içeriği otomatik olarak izlemek için. Yapılandırma dosyası ayarlayarak, etkinleştirmek veya telemetri modülleri ve başlatıcılar devre dışı bırakın ve bazıları için parametreleri ayarlayın.

Yapılandırma dosyası adlı `ApplicationInsights.config` veya `ApplicationInsights.xml`, uygulamanın türüne bağlı olarak. Projenize otomatik olarak eklenen olduğunda, [SDK çoğu sürümlerini yüklemek][start]. Ayrıca bir web uygulamasına tarafından eklenir [Durum İzleyicisi bir IIS sunucusundaki][redfield], veya Application Insights'ı seçtiğinizde [bir Azure Web sitesine veya VM uzantısı](app-insights-azure-web-apps.md).

Denetim eşdeğer bir dosyaya hiç [SDK, bir web sayfasındaki][client].

Bu belgede, dosya, bunlar bileşenleri SDK ' nın nasıl kontrol ve bu bileşenleri hangi NuGet paketlerini yükleme yapılandırmada bkz bölümlerde açıklanmaktadır.

> [!NOTE]
> Applicationınsights.config ve .xml yönergeler .NET Core SDK için geçerli değildir. .NET Core uygulamasında yapılacak değişiklikler için genellikle appsettings.json dosyası kullanırız. Bu örneği bulunabilir [anlık görüntü hata ayıklayıcı belgeleri.](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications)

## <a name="telemetry-modules-aspnet"></a>Telemetri modülleri (ASP.NET)
Her bir telemetri modülü, belirli bir veri türü toplar ve veri göndermek için çekirdek API kullanır. Modüller, ayrıca gerekli satırları .config dosyasına ekleyin farklı NuGet paketleri tarafından yüklenir.

Yapılandırma dosyasındaki her modül için bir düğüm yok. Bir modül devre dışı bırakmak için düğüm silin veya açıklamadan çıkarın.

### <a name="dependency-tracking"></a>Bağımlılık izleme
[Bağımlılık izleme](app-insights-asp-net-dependencies.md) uygulamanızı yapar veritabanları ve dış hizmetler ve veritabanları için çağrıları hakkında telemetri toplar. Bir IIS Server'da çalışmak için bu modülü izin vermek için gereken [Durum İzleyicisi yükleme][redfield]. Azure web uygulamaları veya sanal makineleri, kullanılacak [Application Insights uzantısını seçin](app-insights-azure-web-apps.md).

Kod kullanarak izleme kendi bağımlılık de yazabilirsiniz [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).

* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet package.

### <a name="performance-collector"></a>Performans Toplayıcı
[Sistem performans sayaçlarını toplar](app-insights-performance-counters.md) gibi CPU, bellek ve ağ IIS yüklemelerinden yükleyin. Performans sayaçları, kendiniz ayarladığınız dahil olmak üzere toplamak için hangi sayaçları belirtebilirsiniz.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet package.

### <a name="application-insights-diagnostics-telemetry"></a>Application Insights tanılama Telemetrisi
`DiagnosticsTelemetryModule` Application Insights araçları kod kendisini hataları bildirir. Örneğin, kod performans sayaçları erişemiyorsanız veya bir `ITelemetryInitializer` bir özel durum oluşturur. Bu modülü tarafından izlenen izleme telemetri görünür [tanılama arama][diagnostic]. Tanılama verileri için dc.services.vsallin.net gönderir.

* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet paketi. Yalnızca bu paketi yüklerseniz, Applicationınsights.config dosyası otomatik olarak oluşturulmaz.

### <a name="developer-mode"></a>Geliştirici modu
`DeveloperModeWithDebuggerAttachedTelemetryModule` Application Insights zorlar `TelemetryChannel` veri hemen bir hata ayıklayıcı uygulama işlemi için bağlı olduğunda, bir seferde bir telemetri öğesi göndermek için. Uygulamanızı telemetri izler ve Application Insights portalında görüntülendiğinde bu anda arasındaki süreyi azaltır. Önemli yükünü CPU ve ağ bant genişliği neden olur.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet paketi

### <a name="web-request-tracking"></a>Web isteği izleme
Raporları [yanıt süresi ve sonuç kodu](app-insights-asp-net.md) HTTP isteklerinin sayısıdır.

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.applicationınsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet paketi

### <a name="exception-tracking"></a>Özel durum izleme
`ExceptionTrackingTelemetryModule` parçaları işlenmeyen özel durumlar web uygulamanızda. Bkz: [hataları ve özel durumları][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.applicationınsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet paketi
* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule` -parçaları [görev özel durumlarını unobserved](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule` -çalışan rolleri, windows Hizmetleri ve konsol uygulamaları için işlenmeyen özel durumları izler.
* [Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet paketi.

### <a name="eventsource-tracking"></a>EventSource izleme
`EventSourceTelemetryModule` EventSource olaylarını Application Insights izlemeleri olarak gönderilmesini yapılandırmanızı sağlar. EventSource olaylarını izleme hakkında daha fazla bilgi için bkz: [kullanarak EventSource olaylarını](app-insights-asp-net-trace-logs.md#using-eventsource-events).

* `Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule`
* [Microsoft.ApplicationInsights.EventSourceListener](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener) 

### <a name="etw-event-tracking"></a>ETW olay izleme
`EtwCollectorTelemetryModule` Application Insights izlemeleri olarak gönderilmesini ETW sağlayıcılar olaylarından yapılandırmanıza olanak sağlar. ETW olayları izleme hakkında daha fazla bilgi için bkz: [kullanarak ETW olayları](app-insights-asp-net-trace-logs.md#using-etw-events).

* `Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule`
* [Microsoft.ApplicationInsights.EtwCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector) 

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights
Microsoft.ApplicationInsights paket sağlar [API çekirdek](https://msdn.microsoft.com/library/mt420197.aspx) SDK'sının. Bu diğer telemetri modüllerini kullanmanız ve ayrıca [kendi telemetrinizi tanımlamak için kullanmak](app-insights-api-custom-events-metrics.md).

* Applicationınsights.config giriş yok.
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet paketi. Yalnızca bu NuGet yüklerseniz, hiçbir .config dosyası oluşturulur.

## <a name="telemetry-channel"></a>Telemetri kanal
Telemetri kanal arabelleğe alma ve telemetri Application Insights hizmetine aktarımını yönetir.

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel` Hizmetler için varsayılan kanalıdır. Bu veri arabelleğe alır.
* `Microsoft.ApplicationInsights.PersistenceChannel` konsol uygulamaları için bir alternatiftir. Uygulamanızı kapatır ve uygulama yeniden başlatıldığında Gönder unflushed tüm verileri kalıcı depolama birimine kaydedebilirsiniz.

## <a name="telemetry-initializers-aspnet"></a>Telemetri başlatıcıları (ASP.NET)
Telemetri başlatıcıları telemetrinin her öğesiyle birlikte gönderilir bağlam özellikleri ayarlayın.

Yapabilecekleriniz [kendi başlatıcıları yazma](app-insights-api-filtering-sampling.md#add-properties) bağlamı özelliklerini ayarlamak için.

Standart başlatıcıları tüm Web veya Windows Server NuGet paketleri tarafından ayarlanır:

* `AccountIdTelemetryInitializer` Hesap Kimliği özelliğini ayarlar.
* `AuthenticatedUserIdTelemetryInitializer` JavaScript SDK'sı tarafından belirlenen AuthenticatedUserId özelliğini ayarlar.
* `AzureRoleEnvironmentTelemetryInitializer` güncelleştirmeleri `RoleName` ve `RoleInstance` özelliklerini `Device` Azure çalışma zamanı ortamından ayıklanan bilgilerle tüm telemetri öğeleri için bağlamı.
* `BuildInfoConfigComponentVersionTelemetryInitializer` güncelleştirmeleri `Version` özelliği `Component` tüm telemetri öğeleri ayıklanan değerle bağlamının `BuildInfo.config` dosya MS Build tarafından üretilen.
* `ClientIpHeaderTelemetryInitializer` güncelleştirmeleri `Ip` özelliği `Location` tüm telemetri öğeleri bağlamında temel alarak `X-Forwarded-For` isteğin HTTP üstbilgisi.
* `DeviceTelemetryInitializer` Aşağıdaki özellikleri güncelleştirmeleri `Device` tüm telemetri öğeleri için bağlamı.
  * `Type` "Bilgisayar" ayarlayın
  * `Id` web uygulamasının çalıştığı bilgisayarın etki alanı adına ayarlanır.
  * `OemName` Ayıklanan değerine ayarlanır `Win32_ComputerSystem.Manufacturer` WMI kullanarak alan.
  * `Model` Ayıklanan değerine ayarlanır `Win32_ComputerSystem.Model` WMI kullanarak alan.
  * `NetworkType` Ayıklanan değerine ayarlanır `NetworkInterface`.
  * `Language` adına ayarlayın `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer` güncelleştirmeleri `RoleInstance` özelliği `Device` web uygulamasının çalıştığı bilgisayarın etki alanı adı ile tüm telemetri öğelerini bağlamı.
* `OperationNameTelemetryInitializer` güncelleştirmeleri `Name` özelliği `RequestTelemetry` ve `Name` özelliği `Operation` tüm telemetri öğeleri bağlamında temel HTTP yöntemini yanı sıra üzerinde adlarını, ASP.NET MVC denetleyicisi ve eylem isteği işlemek üzere çağrılır.
* `OperationIdTelemetryInitializer` veya `OperationCorrelationTelemetryInitializer` güncelleştirmeleri `Operation.Id` içerik özelliği tüm telemetri öğelerin izlenen otomatik olarak oluşturulan bir isteği işlerken `RequestTelemetry.Id`.
* `SessionTelemetryInitializer` güncelleştirmeleri `Id` özelliği `Session` tüm telemetri öğeleri ayıklanan değerle bağlamının `ai_session` kullanıcının tarayıcısında çalışan Applicationınsights JavaScript araçları kodu tarafından oluşturulan tanımlama bilgisi.
* `SyntheticTelemetryInitializer` veya `SyntheticUserAgentTelemetryInitializer` güncelleştirmeleri `User`, `Session` ve `Operation` kullanılabilirlik test veya arama motoru bot gibi yapay bir kaynaktan bir isteği işlerken tüm telemetri öğelerinin bağlamları özellikleri izlenir. Varsayılan olarak, [ölçüm Gezgini](app-insights-metrics-explorer.md) yapay telemetri görüntülemez.

    `<Filters>` İsteklerinin özelliklerini tanımlayan ayarlayın.
* `UserTelemetryInitializer` güncelleştirmeleri `Id` ve `AcquisitionDate` özelliklerini `User` ayıklanan değerlere sahip tüm telemetri öğeleri bağlamının `ai_user` tanımlama bilgisi kullanıcının içinde çalışan uygulama Insights JavaScript araçları kodu tarafından oluşturulan Tarayıcı.
* `WebTestTelemetryInitializer` Bu geliyor HTTP istekleri için kullanıcı kimliği ve oturum kimliği yapay kaynağı özellikleri ayarlar [kullanılabilirlik testleri](app-insights-monitor-web-app-availability.md).
  `<Filters>` İsteklerinin özelliklerini tanımlayan ayarlayın.

Service Fabric çalışan .NET uygulamaları için eklediğiniz `Microsoft.ApplicationInsights.ServiceFabric` NuGet paketi. Bu paketi içeren bir `FabricTelemetryInitializer`, telemetri öğelerine Service Fabric özellikler ekler. Daha fazla bilgi için bkz: [GitHub sayfası](https://go.microsoft.com/fwlink/?linkid=848457) bu NuGet paketi tarafından eklenen özellikler hakkında.

## <a name="telemetry-processors-aspnet"></a>Telemetri işlemci (ASP.NET)
Telemetri işlemciler filtre ve yalnızca SDK portala göndermeden önce her bir telemetri öğeyi değiştirin.

Yapabilecekleriniz [kendi telemetri işlemciler yazma](app-insights-api-filtering-sampling.md#filtering).

#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Uyarlamalı örnekleme telemetri işlemcisine (2.0.0-beta3)
Bu, varsayılan olarak etkindir. Uygulamanız çok sayıda telemetri gönderirse, bu işlemci bazıları da kaldırır.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

Parametre elde etmek için algoritma çalışır hedef sağlar. Sunucunuz birden fazla makine bir küme ise, telemetri gerçek hacmi uygun şekilde çarpılır şekilde SDK her örneği bağımsız olarak çalışır.

[Örnekleme hakkında daha fazla bilgi](app-insights-sampling.md).

#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Sabit oran örnekleme telemetri işlemcisine (2.0.0-beta1)
Ayrıca bir standart olan [telemetri işlemci örnekleme](app-insights-api-filtering-sampling.md) (Başlangıç 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Kanal parametreleri (Java)
Bu parametreler Java SDK'sı nasıl ve depolamak topladığı telemetri verilerini temizleme etkiler.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity
SDK'ın bellek içi depolamada depolanan telemetri öğe sayısı. Bu sayıya ulaşıldığında, telemetri arabellek Temizlenen - diğer bir deyişle, telemetri öğeleri Application Insights sunucusuna gönderilir.

* Min: 1
* En fazla: 1000
* Varsayılan: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds
Ne sıklıkla bellekte depolamada depolanan veri (Application Insights gönderilen) kopyalanması belirler.

* Min: 1
* En fazla: 300
* Varsayılan: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB
Yerel diskteki kalıcı depolama birimine ayrılan MB cinsinden maksimum boyutu belirler. Bu depolama Application Insights uç noktasına aktarılacak başarısız kalıcı telemetri öğeleri için kullanılır. Depolama boyutu sağlandığında yeni telemetri öğeleri atılacak.

* Min: 1
* En fazla: 100
* Varsayılan: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey
Bu, verilerinizi göründüğü Application Insights kaynağı belirler. Genellikle, ayrı bir kaynak ayrı bir anahtarla her uygulamalarınız için oluşturun.

Farklı kaynaklara - uygulamanızdan sonuçları göndermek istiyorsanız, dinamik olarak - örneğin anahtar ayarlamak istiyorsanız, yapılandırma dosyasından anahtarı atlayın ve bunun yerine kodda ayarlayın.

Anahtar TelemetryClient tüm örnekler için ayarlamak için standart telemetri modüller de dahil olmak üzere ayarlayın anahtar TelemetryConfiguration.Active içinde. Bu, bir ASP.NET hizmetinde global.aspx.cs gibi başlatma yöntemini yapın:

```csharp

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Yalnızca olayları belirli bir dizi farklı bir kaynağa göndermek istiyorsanız, bu anahtar için belirli bir TelemetryClient ayarlayabilirsiniz:

```csharp

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

Yeni bir anahtar almak için [Application Insights portalında yeni bir kaynak oluşturmak][new].

## <a name="next-steps"></a>Sonraki adımlar
[API hakkında daha fazla bilgi][api].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
