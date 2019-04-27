---
title: Applicationınsights.config başvuru - Azure | Microsoft Docs
description: Etkinleştirmek veya devre dışı veri toplama modülleri ve performans sayaçları ve diğer parametreleri ekleyin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 6e397752-c086-46e9-8648-a1196e8078c2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/19/2018
ms.reviewer: olegan
ms.author: mbullwin
ms.openlocfilehash: 1a5b6d435dcc82b59c30302f9cd711975864594c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60901919"
---
# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>ApplicationInsights.config veya .xml ile Application Insights SDK yapılandırma
Application Insights .NET SDK'sı NuGet paketlerini birtakım oluşur. [Çekirdek paket](https://www.nuget.org/packages/Microsoft.ApplicationInsights) Application Insights'a telemetri göndermek için API sağlar. [Ek paketleri](https://www.nuget.org/packages?q=Microsoft.ApplicationInsights) telemetri sağlamak *modülleri* ve *başlatıcılar* telemetri uygulamanız ve onun içeriği otomatik olarak izlemek için. Yapılandırma dosyası ayarlayarak, etkinleştirmek veya telemetri modülleri ve başlatıcılar devre dışı bırakın ve bunlardan bazıları için parametreleri ayarlayın.

Yapılandırma dosyası adlı `ApplicationInsights.config` veya `ApplicationInsights.xml`, uygulamanın türüne bağlı olarak. Otomatik olarak projenize eklenir, siz [SDK'ın çoğu sürümünü birden yüklemek][start]. Ayrıca bir web uygulaması tarafından eklenir [Durum İzleyicisi bir IIS sunucusunda][redfield], veya Application ınsights'ı seçtiğinizde, [için bir Azure Web sitesinde veya sanal makine uzantısı](azure-web-apps.md).

Denetimi eşdeğer bir dosyaya hiç [SDK'sı bir web sayfasında][client].

Bu belgede, dosya, bunlar SDK'sı bileşenlerinin nasıl kontrol ve bu bileşenlerin hangi NuGet paketlerini yükleme yapılandırma konusuna bakın bölümleri açıklanmaktadır.

> [!NOTE]
> Applicationınsights.config veya .xml yönergelerini .NET Core SDK'sı için geçerli değildir. Değişiklikler bir .NET Core uygulaması için genellikle appsettings.json dosyasını kullanırız. Bunun bir örneği bulunabilir [anlık görüntü hata ayıklayıcısı belgeleri.](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger)

## <a name="telemetry-modules-aspnet"></a>Telemetri modülleri (ASP.NET)
Her telemetri modülü, belirli türde bir veri toplar ve veri göndermek için çekirdek API kullanır. Modüller, ayrıca gerekli satır .config dosyasına ekleyin farklı NuGet paketleri tarafından yüklenir.

Yapılandırma dosyasında her bir modül için bir düğüm yok. Bir modül devre dışı bırakmak için düğüm silin veya açıklama teslim.

### <a name="dependency-tracking"></a>Bağımlılık izleme
[Bağımlılık izleme](../../azure-monitor/app/asp-net-dependencies.md) uygulamanızı yapar veritabanları ve dış hizmetler ve veritabanları için çağrıları ile ilgili telemetri toplar. Bir IIS sunucusunda çalışması bu modülü izin vermek için şunları yapmanız [Durum İzleyicisi yükleme][redfield]. Azure web apps veya Vm'leri, kullanılacak [Application Insights uzantısını seçin](azure-web-apps.md).

İzleme kodu kullanarak kendi bağımlılık da yazabilirsiniz [TrackDependency API](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency).

* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet paketi.

### <a name="performance-collector"></a>Performans Toplayıcı
[Sistem performans sayaçlarını toplar](../../azure-monitor/app/performance-counters.md) gibi CPU, bellek ve ağ IIS yüklemelerinden yükleyin. Performans sayaçları, tuttuğumuz dahil olmak üzere, hangi sayaçları toplamak için belirtebilirsiniz.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet paketi.

### <a name="application-insights-diagnostics-telemetry"></a>Application Insights tanılama Telemetrisi
`DiagnosticsTelemetryModule` Application Insights izleme kodu kendisini hataları bildirir. Örneğin, kodun performans sayaçları erişemiyorsanız veya bir `ITelemetryInitializer` bir özel durum oluşturur. Bu modülü tarafından izlenen izleme telemetrisi görünür [tanılama araması][diagnostic]. Tanılama verileri için dc.services.vsallin.net gönderir.

* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.applicationınsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet paketi. Yalnızca bu paketi yüklerseniz, Applicationınsights.config dosyası otomatik olarak oluşturulmaz.

### <a name="developer-mode"></a>Geliştirici modu
`DeveloperModeWithDebuggerAttachedTelemetryModule` Application Insights zorlar `TelemetryChannel` veri hemen bir hata ayıklayıcı uygulama işlemi için bağlı olduğunda, bir kerede bir telemetri öğesinin göndermek. Uygulamanızın telemetri izler ve Application Insights portalında görüntülendiğinde bu şu arasındaki süre miktarını azaltır. CPU ve ağ bant genişliğini önemli ölçüde neden olur.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Application Insights Windows Server](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet paketi

### <a name="web-request-tracking"></a>Web istek izleme
Raporları [yanıt süresi ve sonuç kodu](../../azure-monitor/app/asp-net.md) HTTP isteği.

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.applicationınsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet paketi

### <a name="exception-tracking"></a>Özel durum izleme
`ExceptionTrackingTelemetryModule` parçaları işlenmeyen özel durumlar, web uygulamanızın. Bkz: [hataları ve özel durumlar][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.applicationınsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet paketi
* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule` -parçaları [görev özel durumlarını gözlemlenmeyen](https://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule` -çalışan rolleri, windows Hizmetleri ve konsol uygulamaları için işlenmeyen özel durumları izler.
* [Application Insights Windows Server](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet paketi.

### <a name="eventsource-tracking"></a>EventSource izleme
`EventSourceTelemetryModule` EventSource olaylarını Application Insights izlemelerini olarak gönderilmesini yapılandırmanızı sağlar. EventSource olaylarını izleme hakkında daha fazla bilgi için bkz: [EventSource olaylarını kullanarak](../../azure-monitor/app/asp-net-trace-logs.md#using-eventsource-events).

* `Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule`
* [Microsoft.ApplicationInsights.EventSourceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener) 

### <a name="etw-event-tracking"></a>ETW olay izleme
`EtwCollectorTelemetryModule` olayları ETW sağlayıcısından izlemeleri olarak Application ınsights'a gönderilmek üzere yapılandırmanıza olanak sağlar. ETW olayları izleme hakkında daha fazla bilgi için bkz: [kullanarak ETW olaylarını](../../azure-monitor/app/asp-net-trace-logs.md#using-etw-events).

* `Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule`
* [Microsoft.ApplicationInsights.EtwCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector) 

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights
Microsoft.applicationınsights paket sağlar [API çekirdek](https://msdn.microsoft.com/library/mt420197.aspx) SDK. Bunu diğer telemetri modüller kullanın ve ayrıca [kendi telemetrinizi tanımlamak için kullanmak](../../azure-monitor/app/api-custom-events-metrics.md).

* Applicationınsights.config girdisi yok.
* [Microsoft.applicationınsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet paketi. Bu NuGet yüklerseniz, hiçbir .config dosyası oluşturulur.

## <a name="telemetry-channel"></a>Telemetri kanal
Telemetri kanal arabelleğe alma ve Application Insights hizmetine telemetri iletimini yönetir.

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel` Hizmetler için varsayılan kanalıdır. Veri bellekte arabelleğe alır.
* `Microsoft.ApplicationInsights.PersistenceChannel` konsol uygulamaları için bir alternatiftir. Uygulamanızı listeyi kapatır ve bu uygulama yeniden başlatıldığında gönderecek unflushed veri kalıcı depolama için kaydedebilirsiniz.

## <a name="telemetry-initializers-aspnet"></a>Telemetri başlatıcıları (ASP.NET)
Telemetri başlatıcılar, birlikte telemetrinin her öğesine gönderilen bağlam özelliklerini ayarlayın.

Yapabilecekleriniz [kendi başlatıcılar yazma](../../azure-monitor/app/api-filtering-sampling.md#add-properties) bağlam özelliklerini ayarlamak için.

Standart başlatıcılar tüm Web veya Windows Server NuGet paketi tarafından ayarlanır:

* `AccountIdTelemetryInitializer` AccountID özelliği ayarlar.
* `AuthenticatedUserIdTelemetryInitializer` JavaScript SDK'sı tarafından belirlenen AuthenticatedUserId özelliği ayarlar.
* `AzureRoleEnvironmentTelemetryInitializer` güncelleştirmeleri `RoleName` ve `RoleInstance` özelliklerini `Device` Azure çalışma zamanı ortamdan ayıklanan bilgilerle tüm telemetri öğeleri için bağlam.
* `BuildInfoConfigComponentVersionTelemetryInitializer` güncelleştirmeleri `Version` özelliği `Component` Ayıklanan değerin tüm telemetri öğeleri için bağlam `BuildInfo.config` MS Build tarafından oluşturulan dosyası.
* `ClientIpHeaderTelemetryInitializer` güncelleştirmeleri `Ip` özelliği `Location` bağlam tüm telemetri öğelerinin temel alarak `X-Forwarded-For` HTTP isteği üstbilgisi.
* `DeviceTelemetryInitializer` Aşağıdaki özelliklerini güncelleştirir `Device` tüm telemetri öğeleri için bağlam.
  * `Type` "Bilgisayar" için ayarlayın
  * `Id` web uygulamasının çalıştığı bilgisayarın etki alanı adına ayarlanır.
  * `OemName` Ayıklanan değere ayarlanır `Win32_ComputerSystem.Manufacturer` WMI kullanılarak alan.
  * `Model` Ayıklanan değere ayarlanır `Win32_ComputerSystem.Model` WMI kullanılarak alan.
  * `NetworkType` Ayıklanan değere ayarlanır `NetworkInterface`.
  * `Language` adına ayarlanır `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer` güncelleştirmeleri `RoleInstance` özelliği `Device` web uygulamasının çalıştığı bilgisayarın etki alanı adına sahip tüm telemetri öğeleri için bağlam.
* `OperationNameTelemetryInitializer` güncelleştirmeleri `Name` özelliği `RequestTelemetry` ve `Name` özelliği `Operation` bağlam tüm telemetri öğelerinin temel HTTP yöntemi, aynı zamanda adlarını ASP.NET MVC denetleyici ve eylem isteği işlemek üzere çağrılır.
* `OperationIdTelemetryInitializer` veya `OperationCorrelationTelemetryInitializer` güncelleştirmeleri `Operation.Id` bağlam özelliği tüm telemetri öğelerinin izlenen otomatik olarak oluşturulan bir isteği işlerken `RequestTelemetry.Id`.
* `SessionTelemetryInitializer` güncelleştirmeleri `Id` özelliği `Session` ayıklanan değerle tüm telemetri öğeleri için bağlam `ai_session` tanımlama bilgisi kullanıcının tarayıcısında çalışan Applicationınsights JavaScript izleme kodu tarafından oluşturulur.
* `SyntheticTelemetryInitializer` veya `SyntheticUserAgentTelemetryInitializer` güncelleştirmeleri `User`, `Session`, ve `Operation` tüm telemetri öğelerinin bağlamları özelliklerini, bir kullanılabilirlik testi veya arama motoru robot gibi yapay bir kaynaktan bir isteği işlerken izlenir. Varsayılan olarak, [ölçüm Gezgini](../../azure-monitor/app/metrics-explorer.md) yapay telemetri görüntülemez.

    `<Filters>` Tanımlama isteklerinin özelliklerini ayarlayın.
* `UserTelemetryInitializer` güncelleştirmeleri `Id` ve `AcquisitionDate` özelliklerini `User` ayıklanan değerlere sahip tüm telemetri öğeleri için bağlam `ai_user` tanımlama bilgisi kullanıcının çalışan Application Insights JavaScript izleme kodu tarafından oluşturulan Tarayıcı.
* `WebTestTelemetryInitializer` Bu geliyor HTTP istekleri için kullanıcı kimliği, oturum kimliği ve yapay kaynak özelliklerini ayarlar [kullanılabilirlik testleri](../../azure-monitor/app/monitor-web-app-availability.md).
  `<Filters>` Tanımlama isteklerinin özelliklerini ayarlayın.

Service Fabric'te çalışan .NET uygulamaları için dahil edebileceğiniz `Microsoft.ApplicationInsights.ServiceFabric` NuGet paketi. Bu paketi içeren bir `FabricTelemetryInitializer`, telemetri öğeleri için Service Fabric özellikleri ekler. Daha fazla bilgi için [GitHub sayfasına](https://github.com/Microsoft/ApplicationInsights-ServiceFabric/blob/master/README.md) bu NuGet paketi tarafından eklenen özellikler hakkında.

## <a name="telemetry-processors-aspnet"></a>Telemetri işlemci (ASP.NET)
Telemetri işlemcileri filtreleyebilir ve yalnızca SDK'sından portala göndermeden önce her telemetri öğesinin değiştirin.

Yapabilecekleriniz [kendi telemetri işlemci yazma](../../azure-monitor/app/api-filtering-sampling.md#filtering).

#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Uyarlamalı örnekleme telemetri işlemciden (2.0.0-beta3)
Bu, varsayılan olarak etkindir. Uygulamanız çok miktarda telemetri gönderir, bu işlemci bazıları da kaldırır.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

Parametre elde etmek için algoritma çalışır bir hedef sağlar. Sunucunuz birden çok makine kümesi ise gerçek telemetri hacmi uygun şekilde çarpılmasına şekilde SDK'ın her örnek bağımsız olarak çalışır.

[Örnekleme hakkında daha fazla bilgi](../../azure-monitor/app/sampling.md).

#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Sabit fiyat örnekleme telemetri işlemciden (2.0.0-beta1)
Ayrıca bir standart olan [telemetri işlemci örnekleme](../../azure-monitor/app/api-filtering-sampling.md) (Başlangıç 2.0.1):

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
Bu parametreler, Java SDK'sı depolamak ve bunları topladığı telemetri verilerini temizleme nasıl etkiler.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity
SDK'ın bellek içi depolama alanında depolanan telemetri öğelerinin sayısı. Bu sayıya ulaşıldığında telemetri arabellek aktarılmadan - diğer bir deyişle, telemetri öğelerinin Application Insights sunucuya gönderilir.

* En küçük: 1
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
Ne sıklıkta bellek içi depolama alanında depolanan veriler (Application ınsights'a gönderilen) kopyalanması belirler.

* En küçük: 1
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
Yerel diskte kalıcı depolama için ayrılan MB en büyük boyutunu belirler. Bu depolama, Application Insights uç noktasına aktarılması başarısız oldu. kalıcı telemetri öğeleri için kullanılır. Depolama boyutu sağlandığında yeni telemetri öğelerinin atılacak.

* En küçük: 1
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

#### <a name="local-forwarder"></a>Yerel iletici

[Yerel ileticisi](opencensus-local-forwarder.md) Application Insights tarafından toplanan bir aracı veya [OpenCensus](https://opencensus.io/) çeşitli SDK'lar ve çerçeveleri alınan telemetri ve uygulama anlayışları'na yönlendirir. Bu, Windows ve Linux altında çalıştırma yeteneğine sahiptir. Application Insights Java SDK ile birlikte olduğunda yerel ileticisi için tam destek sağlar. [Canlı ölçümleri](../../azure-monitor/app/live-stream.md) ve Uyarlamalı örnekleme.

```xml
<Channel type="com.microsoft.applicationinsights.channel.concrete.localforwarder.LocalForwarderTelemetryChannel">
<EndpointAddress><!-- put the hostname:port of your LocalForwarder instance here --></EndpointAddress>

<!-- The properties below are optional. The values shown are the defaults for each property -->

<FlushIntervalInSeconds>5</FlushIntervalInSeconds><!-- must be between [1, 500]. values outside the bound will be rounded to nearest bound -->
<MaxTelemetryBufferCapacity>500</MaxTelemetryBufferCapacity><!-- units=number of telemetry items; must be between [1, 1000] -->
</Channel>
```

SpringBoot başlangıç kullanıyorsanız, aşağıdaki yapılandırma dosyanız (application.properties) ekleyin:

```yml
azure.application-insights.channel.local-forwarder.endpoint-address=<!--put the hostname:port of your LocalForwarder instance here-->
azure.application-insights.channel.local-forwarder.flush-interval-in-seconds=<!--optional-->
azure.application-insights.channel.local-forwarder.max-telemetry-buffer-capacity=<!--optional-->
```

Varsayılan değerleri SpringBoot application.properties ve applicationınsights.XML yapılandırması için aynıdır.

## <a name="instrumentationkey"></a>Instrumentationkey
Bu, verilerinizi göründüğü Application Insights kaynağını belirler. Genellikle, ayrı bir kaynak ayrı bir anahtar ile her uygulamalarınızı oluşturun.

Farklı kaynaklar - uygulamanızdan sonuçları göndermek istiyorsanız, dinamik olarak - örneğin anahtar ayarlamak istiyorsanız, yapılandırma dosyasından anahtarı atlayın ve bunun yerine kod içinde ayarlayabilirsiniz.

TelemetryClient öğesinin tüm örneklerini anahtarını ayarlamak için standart telemetri modülleriyle birlikte ayarlayın anahtar TelemetryConfiguration.Active içinde. Bu, bir ASP.NET hizmetinde global.aspx.cs gibi başlatma yöntemini yapın:

```csharp

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      //...
```

Yalnızca belirli bir etkinlik kümesi farklı bir kaynağa göndermek istiyorsanız, bu anahtar için belirli bir TelemetryClient ayarlayabilirsiniz:

```csharp

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

Yeni bir anahtar almak için [Application Insights portalında yeni bir kaynak oluşturmak][new].



## <a name="applicationid-provider"></a>ApplicationId sağlayıcısı

_İçinde v2.6.0 itibaren kullanılabilir_

Bu sağlayıcı amacı, bir izleme anahtarını temel alan bir uygulama kimliği arama sağlamaktır. Uygulama Kimliği RequestTelemetry ve DependencyTelemetry dahil ve Portalı'nda bağıntı belirlemek için kullanılır.

Bu ayarlayarak kullanılabilir `TelemetryConfiguration.ApplicationIdProvider` kod veya yapılandırma.

### <a name="interface-iapplicationidprovider"></a>Arabirim: IApplicationIdProvider

```csharp
public interface IApplicationIdProvider
{
    bool TryGetApplicationId(string instrumentationKey, out string applicationId);
}
```


İki uygulamalarında sağladığımız [Microsoft.applicationınsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) sdk: `ApplicationInsightsApplicationIdProvider` ve `DictionaryApplicationIdProvider`.

### <a name="applicationinsightsapplicationidprovider"></a>ApplicationInsightsApplicationIdProvider

Profili API'mizi çevresinde bir sarmalayıcı budur. İstekler ve sonuçlarını önbelleğe alma kısıtlama.

Ya da yüklediğinizde bu sağlayıcı, yapılandırma dosyasına eklenir [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) veya [Microsoft.applicationınsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/)

Bu sınıf, isteğe bağlı bir özellik olan `ProfileQueryEndpoint`.
Varsayılan olarak bu ayar `https://dc.services.visualstudio.com/api/profiles/{0}/appId`.
Bu yapılandırma için bir proxy yapılandırmanız gerekiyorsa, proxy temel öneririz adresi ve dahil olmak üzere "/api/profilleri/{0}/appId". Unutmayın '{0}', istek başına çalışma zamanında izleme anahtarı ile değiştirilir.

#### <a name="example-configuration-via-applicationinsightsconfig"></a>Applicationınsights.config aracılığıyla örnek yapılandırma:
```xml
<ApplicationInsights>
    ...
    <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
        <ProfileQueryEndpoint>https://dc.services.visualstudio.com/api/profiles/{0}/appId</ProfileQueryEndpoint>
    </ApplicationIdProvider>
    ...
</ApplicationInsights>
```

#### <a name="example-configuration-via-code"></a>Kod aracılığıyla örnek yapılandırma:
```csharp
TelemetryConfiguration.Active.ApplicationIdProvider = new ApplicationInsightsApplicationIdProvider();
```

### <a name="dictionaryapplicationidprovider"></a>DictionaryApplicationIdProvider

Bu yapılandırılmış izleme anahtarınızı güvenirsiniz statik bir sağlayıcı, / uygulama kimliği çiftleri.

Bu sınıf bir özelliğe sahip `Defined`, uygulama kimliği çiftleri için izleme anahtarı, sözlük < string, string > olduğu.

Bu sınıf, isteğe bağlı bir özellik olan `Next` var olmayan bir izleme anahtarı istendiğinde başka bir sağlayıcı yapılandırmanızda yapılandırmak için kullanılabilir.

#### <a name="example-configuration-via-applicationinsightsconfig"></a>Applicationınsights.config aracılığıyla örnek yapılandırma:
```xml
<ApplicationInsights>
    ...
    <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.DictionaryApplicationIdProvider, Microsoft.ApplicationInsights">
        <Defined>
            <Type key="InstrumentationKey_1" value="ApplicationId_1"/>
            <Type key="InstrumentationKey_2" value="ApplicationId_2"/>
        </Defined>
        <Next Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights" />
    </ApplicationIdProvider>
    ...
</ApplicationInsights>
```

#### <a name="example-configuration-via-code"></a>Kod aracılığıyla örnek yapılandırma:
```csharp
TelemetryConfiguration.Active.ApplicationIdProvider = new DictionaryApplicationIdProvider{
 Defined = new Dictionary<string, string>
    {
        {"InstrumentationKey_1", "ApplicationId_1"},
        {"InstrumentationKey_2", "ApplicationId_2"}
    }
};
```




## <a name="next-steps"></a>Sonraki adımlar
[API hakkında daha fazla bilgi][api].

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[client]: ../../azure-monitor/app/javascript.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[exceptions]: ../../azure-monitor/app/asp-net-exceptions.md
[netlogs]: ../../azure-monitor/app/asp-net-trace-logs.md
[new]: ../../azure-monitor/app/create-new-resource.md 
[redfield]: ../../azure-monitor/app/monitor-performance-live-website-now.md
[start]: ../../azure-monitor/app/app-insights-overview.md
