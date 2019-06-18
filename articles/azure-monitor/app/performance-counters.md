---
title: Performans sayaçlarını Application ınsights | Microsoft Docs
description: Sistem ve Application ınsights'ta özel .NET performans sayaçları izleyin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 5b816f4c-a77a-4674-ae36-802ee3a2f56d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: mbullwin
ms.openlocfilehash: 0ec64a5ae412fb4a1900021fefcb7d9112b1b019
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66255334"
---
# <a name="system-performance-counters-in-application-insights"></a>Application ınsights'ta sistem performans sayaçları

Windows tarafından CPU doluluğu, bellek, disk ve ağ kullanımı gibi şeylere yönelik birçok çeşit [performans sayacı](https://docs.microsoft.com/windows/desktop/PerfCtrs/about-performance-counters) sunulur. Ayrıca, kendi performans Sayaçlarınızı tanımlayabilirsiniz. Uygulamanız IIS altında çalışıyor, bir şirket içi konak veya sanal makine için yönetici erişimine sahip olduğu sürece, performans sayaçlarını toplama desteklenir. Ancak Azure Web Apps, performans sayaçları doğrudan erişime sahip olmadığından olarak çalışan uygulamalar, kullanılabilir sayaçları kümesini toplanan Application Insights tarafından.

## <a name="view-counters"></a>Sayaçları görüntüleyin

Ölçümleri bölmesi, performans sayaçları varsayılan kümesini gösterir.

![Uygulama anlayışları'nda bildirilen performans sayaçları](./media/performance-counters/performance-counters.png)

ASP.NET/ASP.NET Core web uygulamaları için toplanacak yapılandırılan geçerli varsayılan sayaçlar şunlardır:

         - % Process\\Processor Time
         - % Process\\Processor Time Normalized
         - Memory\\Available Bytes
         - ASP.NET Requests/Sec
         - .NET CLR Exceptions Thrown / sec
         - ASP.NET ApplicationsRequest Execution Time
         - Process\\Private Bytes
         - Process\\IO Data Bytes/sec
         - ASP.NET Applications\\Requests In Application Queue
         - Processor(_Total)\\% Processor Time

## <a name="add-counters"></a>Sayaçları ekleme

İstediğiniz performans sayacı ölçüm listesinde bulunmayan, ekleyebilirsiniz.

1. Hangi sayaç yerel sunucuda bu PowerShell komutunu kullanarak sunucunuzun kullanılabilir olduğunu bulun:

    `Get-Counter -ListSet *`

    (Bkz [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)
2. Applicationınsights.config dosyasını açın.

   * Geliştirme sırasında uygulamanıza Application Insights eklediyseniz, projenizde applicationınsights.config dosyasını düzenleyin ve sunucularınıza yeniden dağıtın.
3. Performans Toplayıcı yönergesi düzenleyin:

```XML

    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

> [!NOTE]
> ASP.NET Core uygulamaları yok `ApplicationInsights.config`, ve bu nedenle yukarıdaki yöntemi ASP.NET Core uygulamaları için geçerli değil.

Standart sayaçları hem bunları kendiniz uyguladık yakalayabilirsiniz. `\Objects\Processes` tüm Windows sistemlerinde kullanılabilir olan standart bir sayaç örneğidir. `\Sales(photo)\# Items Sold` bir web hizmetinde uygulanan özel bir sayaç örneğidir.

Biçim `\Category(instance)\Counter"`, veya örnekleri için kategorileri, sadece `\Category\Counter`.

`ReportAs` Eşleşmeyen sayaç adları için gerekli olan `[a-zA-Z()/-_ \.]+` -diğer bir deyişle, bunlar aşağıdaki kümelerinde olmayan karakterler içeren: harf, yuvarlak köşeli ayraçlar, eğik çizgi, tire, alt çizgi, boşluk, nokta.

Bir örnek belirtirseniz, boyut bildirilen ölçümün "CounterInstanceName" toplanmayacak.

### <a name="collecting-performance-counters-in-code-for-aspnet-web-applications-or-netnet-core-console-applications"></a>ASP.NET Web uygulamaları veya.NET/.NET Core konsol uygulamaları için kod performans sayaçlarını toplama
Sistem performans sayaçları toplamak ve bunları Application Insights'a gönderme hakkında bilgi için aşağıdaki kod parçacığında uyum sağlayabilir:


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Process([replace-with-application-process-name])\Page Faults/sec", "PageFaultsPerfSec")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

Veya, oluşturduğunuz özel ölçümler ile aynı şeyi yapmak için:

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

### <a name="collecting-performance-counters-in-code-for-aspnet-core-web-applications"></a>ASP.NET Core Web uygulamaları için kod performans sayaçlarını toplama

Değiştirme `ConfigureServices` yönteminde, `Startup.cs` sınıfı aşağıdaki gibi.

```csharp
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following configures PerformanceCollectorModule.
  services.ConfigureTelemetryModule<PerformanceCollectorModule>((module, o) =>
            {
                // the application process name could be "dotnet" for ASP.NET Core self-hosted applications.
                module.Counters.Add(new PerformanceCounterCollectionRequest(
    @"\Process([replace-with-application-process-name])\Page Faults/sec", "DotnetPageFaultsPerfSec"));
            });
    }
```

## <a name="performance-counters-in-analytics"></a>Analytics'te performans sayaçları
Arama ve performans sayacı raporlarda görüntüleme [Analytics](../../azure-monitor/app/analytics.md).

**PerformanceCounters** şema sunan `category`, `counter` adı ve `instance` her performans sayacının adı.  Her uygulama için telemetriyi yalnızca bu uygulama için sayaçları görürsünüz. Örneğin, görmek için hangi sayaçları kullanılabilir: 

![Application Insights analytics performans sayaçları](./media/performance-counters/analytics-performance-counters.png)

(Performans sayacı örneği, olmayan rol veya sunucu makine örneği için 'Instance' burada ifade eder. Performans sayacı örneği adı genellikle sayaçları işlemci zamanı gibi işlem veya uygulama adıyla ayırır.)

Son dönem boyunca kullanılabilir belleğin bir grafik almak için: 

![Application Insights Analytics bellek zaman grafiğini](./media/performance-counters/analytics-available-memory.png)

Gibi diğer telemetri **performanceCounters** bir sütunda da `cloud_RoleInstance` uygulamanızın üzerinde çalıştığı ana bilgisayar sunucu örneğinin kimliğini belirtir. Örneğin, farklı makinelerde uygulamanızın performansını karşılaştırmak için şunu yazın: 

![Application Insights Analytics rol örneği ile bölümlenmiş performans](./media/performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a>ASP.NET ve Application Insights sayıları

*Özel durum oranı ve özel durumlar ölçümleri arasındaki fark nedir?*

* *Özel durum oranı* bir sistem performans sayacı. CLR atılır ve toplam aralığı uzunluğu tarafından bir örnekleme aralığındaki böler tüm işlenen ve yakalanamayan özel durumları sayar. Application Insights SDK'sı, bu sonuç toplar ve portala gönderir.

* *Özel durumlar* grafiğin örnekleme aralığı portalda tarafından alınan TrackException raporlarının sayısı. Burada yazdığınız TrackException kodunuzda çağırır ve tüm içermez yalnızca işlenen özel durumları içerir [işlenmeyen özel durumları](../../azure-monitor/app/asp-net-exceptions.md). 

## <a name="performance-counters-for-applications-running-in-azure-web-apps"></a>Azure Web Apps'te çalışan uygulamalar için performans sayaçları

ASP.NET ve ASP.NET Core uygulamaları Azure Web Apps'e dağıtılan özel bir korumalı alan ortamında çalıştırın. Bu ortam sistem performans sayaçları doğrudan erişim izin vermez. Ancak, sınırlı bir alt kümesinde sayaçları sunulur ortam değişkenleri olarak açıklandığı [burada](https://github.com/projectkudu/kudu/wiki/Perf-Counters-exposed-as-environment-variables). ASP.NET ve ASP.NET Core için Application Insights SDK'sı, Azure Web uygulamaları bu özel ortam değişkenlerinden performans sayaçlarını toplar. Yalnızca bir alt kümesini sayaçları bu ortamda kullanılabilir ve tam listesi bulunabilir [burada.](https://github.com/microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/PerformanceCollector/Perf.Shared/Implementation/WebAppPerformanceCollector/CounterFactory.cs)

## <a name="performance-counters-in-aspnet-core-applications"></a>ASP.NET Core uygulamalarında performans sayaçları

* [ASP.NET Core SDK'sı](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) sürüm 2.4.1'i ve yukarıda uygulamayı Azure Web uygulaması (Windows) olarak çalışıyorsa performans sayaçlarını toplar

* SDK sürümü 2.7.0-beta3 ve yukarıdaki uygulama içinde Windows çalıştıran ve hedefleme performans sayaçlarını toplar `NETSTANDARD2.0` veya üzeri.
* .NET Framework'ü hedefleyen uygulamalar için performans sayaçları, SDK'ın tüm sürümlerinde desteklenir.
* Performans sayacı desteği olmayan Windows eklendiğinde bu makale güncelleştirilecektir.

## <a name="alerts"></a>Uyarılar
Diğer ölçümler gibi yapabilecekleriniz [uyarı ayarlama](../../azure-monitor/app/alerts.md) bir performans sayacı belirttiğiniz sınırı dışında aşması durumunda sizi uyarır. Uyarılar bölmesini açın ve eklemek uyarı tıklayın.

## <a name="next"></a>Sonraki adımlar

* [Bağımlılık izleme](../../azure-monitor/app/asp-net-dependencies.md)
* [Özel durum izleme](../../azure-monitor/app/asp-net-exceptions.md)

