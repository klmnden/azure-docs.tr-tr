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
ms.openlocfilehash: d38a575af54f044d64efc67b5483a67ffcd2fcd6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60256962"
---
# <a name="system-performance-counters-in-application-insights"></a>Application ınsights'ta sistem performans sayaçları

Windows tarafından CPU doluluğu, bellek, disk ve ağ kullanımı gibi şeylere yönelik birçok çeşit [performans sayacı](https://docs.microsoft.com/windows/desktop/PerfCtrs/about-performance-counters) sunulur. Ayrıca, kendi performans Sayaçlarınızı tanımlayabilirsiniz. Uygulamanız IIS altında çalışıyor, bir şirket içi konak veya sanal makine için yönetici erişimine sahip olduğu sürece.

## <a name="view-counters"></a>Sayaçları görüntüleyin

Ölçümleri bölmesi, performans sayaçları varsayılan kümesini gösterir.

![Uygulama anlayışları'nda bildirilen performans sayaçları](./media/performance-counters/performance-counters.png)

.NET web uygulamaları için toplanan geçerli varsayılan sayaçlar şunlardır:

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

Tek bir yerde tüm faydalı grafikleri görmek için oluşturun bir [Pano](../../azure-monitor/app/app-insights-dashboards.md) ve bunları sabitleyin.

## <a name="add-counters"></a>Sayaçları ekleme

İstediğiniz performans sayacı ölçüm listesinde bulunmayan, ekleyebilirsiniz.

1. Hangi sayaç yerel sunucuda bu PowerShell komutunu kullanarak sunucunuzun kullanılabilir olduğunu bulun:
   
    `Get-Counter -ListSet *`
   
    (Bkz [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)
2. Applicationınsights.config dosyasını açın.
   
   * Geliştirme sırasında uygulamanıza Application Insights eklediyseniz, projenizde applicationınsights.config dosyasını düzenleyin ve sunucularınıza yeniden dağıtın.
   * Çalışma zamanında bir web uygulaması izleme için Durum İzleyicisi'ni kullandıysanız, IIS uygulama kök dizinindeki applicationınsights.config dosyasını bulun. Öğeyi bir var her sunucu örneğinde güncelleştirin.
3. Performans Toplayıcı yönergesi düzenleyin:
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

Standart sayaçları hem bunları kendiniz uyguladık yakalayabilirsiniz. `\Objects\Processes` tüm Windows sistemlerinde kullanılabilir olan standart bir sayaç örneğidir. `\Sales(photo)\# Items Sold` bir web hizmetinde uygulanan özel bir sayaç örneğidir. 

Biçim `\Category(instance)\Counter"`, veya örnekleri için kategorileri, sadece `\Category\Counter`.

`ReportAs` Eşleşmeyen sayaç adları için gerekli olan `[a-zA-Z()/-_ \.]+` -diğer bir deyişle, bunlar aşağıdaki kümelerinde olmayan karakterler içeren: harf, yuvarlak köşeli ayraçlar, eğik çizgi, tire, alt çizgi, boşluk, nokta.

Bir örnek belirtirseniz, boyut bildirilen ölçümün "CounterInstanceName" toplanmayacak.

### <a name="collecting-performance-counters-in-code"></a>Koddaki performans sayaçlarını toplama
Sistem performans sayaçları toplamak ve bunları Application Insights'a gönderme hakkında bilgi için aşağıdaki kod parçacığında uyum sağlayabilir:


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```
Veya, oluşturduğunuz özel ölçümler ile aynı şeyi yapmak için:

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
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

## <a name="performance-counters-in-aspnet-core-applications"></a>ASP.NET Core uygulamalarında performans sayaçları
Performans sayaçları, uygulamanın tam .NET Framework yalnızca hedeflediği ise desteklenir. .NET Core uygulamaları için performans sayaçlarını toplama yeteneği yoktur.

## <a name="alerts"></a>Uyarılar
Diğer ölçümler gibi yapabilecekleriniz [uyarı ayarlama](../../azure-monitor/app/alerts.md) bir performans sayacı belirttiğiniz sınırı dışında aşması durumunda sizi uyarır. Uyarılar bölmesini açın ve eklemek uyarı tıklayın.

## <a name="next"></a>Sonraki adımlar
* [Bağımlılık izleme](../../azure-monitor/app/asp-net-dependencies.md)
* [Özel durum izleme](../../azure-monitor/app/asp-net-exceptions.md)

