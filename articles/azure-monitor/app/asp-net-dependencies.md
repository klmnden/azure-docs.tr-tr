---
title: Bağımlılık Azure Application Insights izleme | Microsoft Docs
description: Şirket içi veya Microsoft Azure Application Insights ile web uygulaması, bağımlılık çağrılarını izleme.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: mbullwin
ms.openlocfilehash: d8ba5b19ad5d8f03203e9a028fbc5aec84e5ec06
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565379"
---
# <a name="dependency-tracking-in-azure-application-insights"></a>Azure Application Insights izleme bağımlılığı 

A *bağımlılık* uygulamanız tarafından çağrılan bir dış bileşen. Bu genellikle adlı HTTP veya bir veritabanı veya dosya sistemi kullanılarak bir hizmettir. [Application Insights](../../azure-monitor/app/app-insights-overview.md) olup bağımlılık çağrıları süresini ölçer, veya değil, birlikte ek bilgileri bağımlılığının adı gibi vb. başarısız. Özel bağımlılık çağrıları incelemek ve istekler ve özel durumlar için ilişkilendirin.

## <a name="automatically-tracked-dependencies"></a>Otomatik olarak izlenen bağımlılıkları

İle birlikte gönderilmektedir .NET ve .NET Core için Application Insights SDK'ları `DependencyTrackingTelemetryModule` bağımlılıkları otomatik olarak toplayan Telemetri modül olduğu. Bu bağımlılık toplama için otomatik olarak etkinleştirilir [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net) ve [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) bağlı resmi belgeleri göre yapılandırıldığında uygulamalar. `DependencyTrackingTelemetryModule` olarak sevk edilir [bu](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) NuGet paketi ve NuGet paketlerini birini kullanırken otomatik olarak getirildikten `Microsoft.ApplicationInsights.Web` veya `Microsoft.ApplicationInsights.AspNetCore`.

 `DependencyTrackingTelemetryModule` şu anda aşağıdaki bağımlılıkları otomatik olarak izler:

|Bağımlılıkları |Ayrıntılar|
|---------------|-------|
|HTTP/Https | Yerel veya uzak http/https çağrıları |
|WCF çağrıları| HTTP tabanlı bağlamalar kullanıldığında yalnızca otomatik olarak izlenir.|
|SQL | İle yapılan çağrılar `SqlClient`. Bkz: [bu](##advanced-sql-tracking-to-get-full-sql-query) SQL yakalamak için sorgu.  |
|[Azure depolama (Blob, tablo, kuyruk)](https://www.nuget.org/packages/WindowsAzure.Storage/) | Azure depolama istemci ile yapılan çağrılar. |
|[EventHub istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | 1\.1.0 sürümü ve üzeri. |
|[Service Bus istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus)| Sürüm 3.0.0 ve üstü. |
|Azure Cosmos DB | HTTP/HTTPS kullanılıyorsa yalnızca otomatik olarak izlenir. TCP modu, Application Insights tarafından yakalanan olmaz. |


## <a name="setup-automatic-dependency-tracking-in-console-apps"></a>Otomatik bağımlılık izleme konsol uygulamalarında Kurulumu

Otomatik olarak bağımlılıkları.NET/.NET Core konsol uygulamaları izlemek için Nuget paketini yüklemek `Microsoft.ApplicationInsights.DependencyCollector`ve başlatma `DependencyTrackingTelemetryModule` gibi:

```csharp
    DependencyTrackingTelemetryModule depModule = new DependencyTrackingTelemetryModule();
    depModule.Initialize(TelemetryConfiguration.Active);
```

### <a name="how-automatic-dependency-monitoring-works"></a>Otomatik bağımlılık izleme çalışır?

Bağımlılıklar, aşağıdaki tekniklerden birini kullanarak otomatik olarak toplanır:

* Select yöntemleri etrafında bayt kod araçları kullanarak. (InstrumentationEngine StatusMonitor veya Azure Web uygulaması uzantısı)
* EventSource geri çağırmaları
* (En son.NET/.NET Core SDK'ları içinde) DiagnosticSource geri çağırmaları

## <a name="manually-tracking-dependencies"></a>Bağımlılıkları el ile izleme

Otomatik olarak toplanan değildir ve bu nedenle el ile izleme gerektiren bağımlılıklara bazı örnekler aşağıda verilmiştir.

* Azure Cosmos DB yalnızca otomatik olarak izlenir [HTTP/HTTPS](../../cosmos-db/performance-tips.md#networking) kullanılır. TCP modu, Application Insights tarafından yakalanan olmaz.
* Redis

SDK'sı tarafından otomatik olarak toplanan için bu bağımlılıkları el ile kullanarak bunları izleyebilirsiniz [TrackDependency API](api-custom-events-metrics.md#trackdependency) standart otomatik toplama modülleri tarafından kullanılır.

Örneğin, kendi yazmadığınız sahip bir derleme, kodunuzu derlemek, kolaylaştırır, yanıt süreleri için hangi katkı öğrenmek için tüm çağrıları, süresi. Application ınsights bağımlılık grafiklerinde görüntülenir. Bu veriler için kullanarak göndermek `TrackDependency`.

```csharp

    var startTime = DateTime.UtcNow;
    var timer = System.Diagnostics.Stopwatch.StartNew();
    try
    {
        // making dependency call
        success = dependency.Call();
    }
    finally
    {
        timer.Stop();
        telemetryClient.TrackDependency("myDependencyType", "myDependencyCall", "myDependencyData",  startTime, timer.Elapsed, success);
    }
```

Alternatif olarak, `TelemetryClient` genişletme yöntemleri sağlar `StartOperation` ve `StopOperation` kullanılabilen bağımlılıkları el ile izlemek için gösterildiği gibi [burada](custom-operations-tracking.md#outgoing-dependencies-tracking)

Standart bağımlılık izleme modülünü devre dışı geçiş yapmak istiyorsanız, içinde DependencyTrackingTelemetryModule başvurusunu kaldırın [Applicationınsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) ASP.NET uygulamaları için. ASP.NET Core uygulamaları için yönergeleri izleyin [burada](asp-net-core.md#configuring-or-removing-default-telemetrymodules).

## <a name="tracking-ajax-calls-from-web-pages"></a>AJAX çağrılarından Web sayfalarını izleme

Web sayfaları için Application Insights JavaScript SDK'sı AJAX çağrıları bağımlılık olarak açıklanan otomatik olarak toplar [burada](javascript.md#ajax-performance). Bu belge, sunucu bileşenleri bağımlılıklardan odaklanır.

## <a name="advanced-sql-tracking-to-get-full-sql-query"></a>Tam SQL sorgusunu almak için izleme SQL Gelişmiş

SQL çağrıları için sunucu ve veritabanı adını her zaman toplanan ve toplanan adı olarak depolanan `DependencyTelemetry`. Tam SQL sorgusunu metin içerebilir ' veri' adlı ek bir alan yoktur.

ASP.NET Core uygulamaları için tam SQL sorgusunu almak için gereken ek bir adım yoktur.

ASP.NET uygulamaları için tam SQL sorgusunu izleme altyapısı gerektirir bayt kod araçları yardımıyla toplanır. Aşağıda açıklandığı gibi platforma özgü ek adımlar gereklidir.

| Platform | Tam SQL sorgusunu erişmek için gerekli adımları |
| --- | --- |
| Azure Web Uygulaması |Web uygulaması Denetim Masası'ndaki [Application Insights dikey penceresini açın](../../azure-monitor/app/azure-web-apps.md) ve .NET altındaki SQL komutları etkinleştirin |
| IIS sunucusu (Azure VM, şirket içi ve benzeri.) | [Uygulamanın çalıştığı sunucunuza Durum İzleyicisi yükleme](../../azure-monitor/app/monitor-performance-live-website-now.md) ve IIS'yi yeniden başlatın.
| Azure bulut hizmeti | Ekleme [StatusMonitor yüklemek için başlangıç görevi](../../azure-monitor/app/cloudservices.md#set-up-status-monitor-to-collect-full-sql-queries-optional) <br> Uygulamanızı eklenen Applicationınsights SDK derleme zamanında için NuGet paketlerini yükleyerek olmalıdır [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net) veya [ASP.NET Core uygulamaları](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) |
| IIS Express | Desteklenmiyor

Yukarıdaki durumlarda, bu araçları motor doğrulama doğru şekilde yüklendiğinden SDK'sı sürümü toplanan olduğunu doğrulayarak ise `DependencyTelemetry` 'rddp' olduğu. 'rdddsd' veya 'rddf' bağımlılıkları DiagnosticSource veya EventSource geri çağırmalar toplanır ve bu nedenle tam SQL sorgusunu yakalanan olmaz gösterir.

## <a name="where-to-find-dependency-data"></a>Bağımlılık verileri nerede bulunur

* [Uygulama Haritası](app-map.md) komşu bileşenlerini ve uygulama arasındaki bağımlılıkları görselleştirir.
* [İşlem tanılamaları](transaction-diagnostics.md) gösterir birleşik, sunucu verileri bağıntılı.
* [Tarayıcı sekmesini](javascript.md#ajax-performance) kullanıcılarınızın tarayıcılarından AJAX çağrılarını gösterir.
* Bağımlılık çağrılarını denetlemek için yavaş veya başarısız istekleri tıklayın.
* [Analytics](#logs-analytics) sorgu bağımlılık verileri için kullanılabilir.

## <a name="diagnosis"></a> Yavaş istekleri tanılayın

Her istek olayı, bağımlılık çağrıları, özel durumlar ve uygulamanızı isteği işlerken izlenen gelen diğer olayları ile ilişkilidir. Bu nedenle bazı istekler hatalı yapıyorsanız, yavaş bağımlılık yanıtlarının nedeniyle olup olmadığını bulabilirsiniz.

### <a name="tracing-from-requests-to-dependencies"></a>Bağımlılıklar için gelen istekleri izleme

Açık **performans** gidin ve sekme **bağımlılıkları** üst yanındaki işlemleri sekmesini.

Tıklayarak bir **bağımlılık adı** genel altında. Bu bağımlılık'ın dağıtım süreleri, bir grafik bir bağımlılık seçtikten sonra sağ tarafta görünür.

![Performans sekmesine tıklayın bağımlılık sekmesinde üst sonra grafikteki bir bağımlılık adı](./media/asp-net-dependencies/2-perf-dependencies.png)

Mavi tıklayın **örnekleri** düğmesine sağ alt kısmındaki ve uçtan uca işlem ayrıntıları görmek için bir örnek.

![Bir örneğe uçtan uca işlem ayrıntıları görmek için tıklayın](./media/asp-net-dependencies/3-end-to-end.png)

### <a name="profile-your-live-site"></a>Sitenizin Canlı profil

Hiçbir fikriniz olduğu zaman gider? [Application Insights Profiler'ı](../../azure-monitor/app/profiler.md) izlemeleri HTTP canlı sitenize çağırır ve kodunuzda en uzun sürdü işlevleri gösterir.

## <a name="failed-requests"></a>Başarısız istekler

Başarısız istekler başarısız bağımlılık çağrıları ile ilişkili olabilir.

Biz gidebilirsiniz **hataları** sekmesinde solda ve ardından **bağımlılıkları** en üstteki sekmedeki.

![Başarısız istekler grafiğe tıklayın](./media/asp-net-dependencies/4-fail.png)

Burada başarısız bağımlılık sayısı görmeniz mümkün olacaktır. Bir bağımlılık adı altındaki tabloda tıklayarak çalışırken başarısız bir olay hakkında daha fazla bilgi almak için. Mavi tıklayabilirsiniz **bağımlılıkları** uçtan uca işlem ayrıntıları almak için sağ alt köşede düğmesi.

## <a name="logs-analytics"></a>Günlükleri (analiz)

Bağımlılıkları izleyebilirsiniz [Kusto sorgu dili](/azure/kusto/query/). Bazı örnekler aşağıda verilmiştir.

* Tüm başarısız bağımlılık çağrılarını bulun:

``` Kusto

    dependencies | where success != "True" | take 10
```

* AJAX çağrılarını bulun:

``` Kusto

    dependencies | where client_Type == "Browser" | take 10
```

* İsteklerle ilişkili bağımlılık çağrıları bulun:

``` Kusto

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* Sayfa görünümleri ile ilişkilendirilmiş bulma AJAX çağrıları:

``` Kusto 

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-does-automatic-dependency-collector-report-failed-calls-to-dependencies"></a>*Nasıl yoksa otomatik bağımlılık Toplayıcı rapor bağımlılıklara yapılan çağrılar başarısız oldu?*

* Başarısız bağımlılık çağrıları False olarak ayarlanmış 'başarılı' alanı olacaktır. `DependencyTrackingTelemetryModule` bildirmez `ExceptionTelemetry`. Tam veri modeli için bağımlılık açıklanan [burada](data-model-dependency-telemetry.md).

## <a name="open-source-sdk"></a>Açık kaynak SDK'sı
Her Application Insights SDK gibi bağımlılık toplama modülü de açık kaynaklıdır. Okuma ve koda katkıda bulunan veya rapor sorunu [resmi GitHub deposunu](https://github.com/Microsoft/ApplicationInsights-dotnet-server).

## <a name="next-steps"></a>Sonraki adımlar

* [Özel durumlar](../../azure-monitor/app/asp-net-exceptions.md)
* [Kullanıcı ve sayfa verileri](../../azure-monitor/app/javascript.md)
* [Kullanılabilirlik](../../azure-monitor/app/monitor-web-app-availability.md)
