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
ms.date: 10/11/2016
ms.author: mbullwin
ms.openlocfilehash: bcfb3a52793ba0daca980564d5d2248629b5caf4
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53323021"
---
# <a name="system-performance-counters-in-application-insights"></a>Application ınsights'ta sistem performans sayaçları
Windows sağlayan çok çeşitli [performans sayaçları](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) CPU doluluğu, bellek, disk ve ağ kullanımı gibi. Ayrıca kendi tanımlayabilirsiniz. [Application Insights](app-insights-overview.md) bu performans sayaçlarını bir şirket içi konak veya sanal makinede yönetimsel erişim sahibi uygulamanızı IIS altında çalışıp çalışmadığını gösterebilir. Grafik Canlı uygulamanızın kullanabileceği kaynakları belirtmek ve sunucu örnekleri arasında dengesiz yük belirlemenize yardımcı olabilir.

Performans sayaçları, bir tablo, sunucu örneği tarafından bu parçaları içerir sunucuları dikey penceresinde görünür.

![Uygulama anlayışları'nda bildirilen performans sayaçları](./media/app-insights-performance-counters/counters-by-server-instance.png)

(Performans sayaçları Azure Web Apps için kullanılamaz. Ancak [Application Insights'a Azure Tanılama verileri gönderme](../azure-monitor/platform/diagnostics-extension-to-application-insights.md).)

## <a name="view-counters"></a>Sayaçları görüntüleyin
Sunucu dikey penceresinde varsayılan birtakım performans sayaçlarını gösterir. 

Diğer sayaçları görmek için sunucu dikey penceresinde grafikleri düzenlemek veya yeni bir [ölçüm Gezgini](app-insights-metrics-explorer.md) dikey penceresinde ve yeni bir grafik ekleyin. 

Bir grafik düzenlerken kullanılabilir sayaçları ölçümler olarak listelenir.

![Uygulama anlayışları'nda bildirilen performans sayaçları](./media/app-insights-performance-counters/choose-performance-counters.png)

Tek bir yerde tüm faydalı grafikleri görmek için oluşturun bir [Pano](app-insights-dashboards.md) ve bunları sabitleyin.

## <a name="add-counters"></a>Sayaçları ekleme
Performans sayacı, istediğiniz ölçümleri listesinde gösterilmiyor, Application Insights SDK'sı web sunucunuza toplama değildir çünkü olmasıdır. Bunu yapmak için yapılandırabilirsiniz.

1. Hangi sayaç sunucuda bu PowerShell komutunu kullanarak sunucunuzun kullanılabilir olduğunu bulun:
   
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

Standart sayaçları hem bunları kendiniz uyguladıysanız yakalayabilirsiniz. `\Objects\Processes` Standart bir sayaç örneği olan tüm Windows sistemlerinde kullanılabilir. `\Sales(photo)\# Items Sold` bir web hizmetinde uygulanan özel bir sayaç örneğidir. 

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
Arama ve performans sayacı raporlarda görüntüleme [Analytics](app-insights-analytics.md).

**PerformanceCounters** şema sunan `category`, `counter` adı ve `instance` her performans sayacının adı.  Her uygulama için telemetriyi yalnızca bu uygulama için sayaçları görürsünüz. Örneğin, görmek için hangi sayaçları kullanılabilir: 

![Application Insights analytics performans sayaçları](./media/app-insights-performance-counters/analytics-performance-counters.png)

('İnstance' burada performans sayacı örneğine başvurur değil rol veya sunucu makine örneği. Performans sayacı örneği adı genellikle sayaçları işlemci zamanı gibi işlem veya uygulama adıyla ayırır.)

Son dönem boyunca kullanılabilir belleğin bir grafik almak için: 

![Application Insights Analytics bellek zaman grafiğini](./media/app-insights-performance-counters/analytics-available-memory.png)

Gibi diğer telemetri **performanceCounters** bir sütunda da `cloud_RoleInstance` uygulamanızın üzerinde çalıştığı ana bilgisayar sunucu örneğinin kimliğini belirtir. Örneğin, farklı makinelerde uygulamanızın performansını karşılaştırmak için şunu yazın: 

![Application Insights Analytics rol örneği ile bölümlenmiş performans](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a>ASP.NET ve Application Insights sayıları
*Özel durum oranı ve özel durumlar ölçümleri arasındaki fark nedir?*

* *Özel durum oranı* bir sistem performans sayacı. CLR atılır ve toplam aralığı uzunluğu tarafından bir örnekleme aralığındaki böler tüm işlenen ve yakalanamayan özel durumları sayar. Application Insights SDK'sı, bu sonuç toplar ve portala gönderir.
* *Özel durumlar* grafiğin örnekleme aralığı portalda tarafından alınan TrackException raporlarının sayısı. Burada yazdığınız TrackException kodunuzda çağırır ve tüm içermez yalnızca işlenen özel durumları içerir [işlenmeyen özel durumları](app-insights-asp-net-exceptions.md). 

## <a name="performance-counters-in-aspnet-core-applications"></a>Asp.Net Core uygulamalarında performans sayaçları
Performans sayaçları, uygulamanın tam .NET Framework yalnızca hedeflediği ise desteklenir. .Net Core için performans sayaçlarını toplama yeteneği yoktur uygulamalar.

## <a name="alerts"></a>Uyarılar
Diğer ölçümler gibi yapabilecekleriniz [uyarı ayarlama](app-insights-alerts.md) bir performans sayacı belirttiğiniz sınırı dışında aşması durumunda sizi uyarır. Uyarılar dikey penceresini açın ve eklemek uyarı tıklayın.

## <a name="next"></a>Sonraki adımlar
* [Bağımlılık izleme](app-insights-asp-net-dependencies.md)
* [Özel durum izleme](app-insights-asp-net-exceptions.md)

