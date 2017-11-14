---
title: "Performans sayaçları Application Insights | Microsoft Docs"
description: "Sistem ve Application Insights özel .NET performans sayaçları izleyin."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 5b816f4c-a77a-4674-ae36-802ee3a2f56d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/11/2016
ms.author: mbullwin
ms.openlocfilehash: 40821d32c5bdfe51bb3cb205660d6f25b2c3fadc
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="system-performance-counters-in-application-insights"></a>Application Insights sistem performans sayaçları
Windows, çok çeşitli sağlar [performans sayaçları](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) CPU doluluğu, bellek, disk ve ağ kullanımı gibi. Ayrıca kendi tanımlayabilirsiniz. [Application Insights](app-insights-overview.md) bu performans sayaçlarını bir şirket içi ana bilgisayar veya sanal makinede yönetici erişimi için elinizde uygulamanız IIS altında çalışıp çalışmadığını gösterebilir. Grafikler, Canlı uygulamanızın kullanılabilir kaynakları belirtmek ve sunucu örnekleri arasında dengesiz yük belirlemenize yardımcı olabilir.

Performans sayaçları tablo sunucu örneği tarafından bu parçaları içeren sunucuları dikey penceresinde görünür.

![Application Insights'ta bildirilen performans sayaçları](./media/app-insights-performance-counters/counters-by-server-instance.png)

(Performans sayaçlarını Azure Web uygulamaları için kullanılabilir değil. Ancak [Application Insights'a Azure Tanılama verileri gönderme](app-insights-azure-diagnostics.md).)

## <a name="view-counters"></a>Sayaçları görüntüleyin
Sunucuları dikey penceresine varsayılan bir performans sayaçlarını gösterir. 

Diğer sayaçları görmek için sunucuları dikey penceresine grafiklerde düzenleyin veya yeni bir [ölçüm Gezgini](app-insights-metrics-explorer.md) dikey ve yeni grafikler ekleyin. 

Bir grafiği düzenlediğinizde kullanılabilir sayaçlar ölçümleri listelenir.

![Application Insights'ta bildirilen performans sayaçları](./media/app-insights-performance-counters/choose-performance-counters.png)

Tek bir yerde tüm en yararlı grafikleri görmek için oluşturma bir [Pano](app-insights-dashboards.md) ve bunları sabitleyebilirsiniz.

## <a name="add-counters"></a>Sayaç ekleme
İstediğiniz performans sayacı ölçümleri listesinde gösterilen değil, Application Insights SDK'sı web sunucunuz toplama değil çünkü olmasıdır. Bunu yapmak için yapılandırabilirsiniz.

1. Hangi sayaçları sunucuda bu PowerShell komutunu kullanarak sunucunuzun kullanılabilir olduğunu bulabilirsiniz:
   
    `Get-Counter -ListSet *`
   
    (Bkz [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)
2. Applicationınsights.config açın.
   
   * Geliştirme sırasında uygulamanıza Application Insights eklediyseniz, projenizde Applicationınsights.config düzenlemek ve sunucularınıza yeniden dağıtın.
   * Çalışma zamanında bir web uygulaması izleme için Durum İzleyicisi kullandıysanız, IIS'de uygulamanızın kök dizininde Applicationınsights.config bulun. Var. her sunucu örneğinde güncelleştirin.
3. Performans Toplayıcı yönergesi düzenleyin:
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

Standart sayaçları hem olanlar kendiniz uyguladık yakalayabilirsiniz. `\Objects\Processes`Standart bir sayaç örneği tüm Windows sistemlerinde kullanılabilir. `\Sales(photo)\# Items Sold`bir web hizmeti uygulanan özel bir sayaç örneğidir. 

Biçim `\Category(instance)\Counter"`, veya örnekleri yok kategoriler, sadece `\Category\Counter`.

`ReportAs`Eşleşmeyen sayaç adları için gerekli olan `[a-zA-Z()/-_ \.]+` -diğer bir deyişle, aşağıdaki kümelerinde olmayan karakter içerir: harfler, köşeli ayraçlar, eğik çizgi, tire, alt çizgi, boşluk, yuvarlak nokta.

Bir örnek belirtirseniz, boyut olarak bildirilen ölçüsünün "CounterInstanceName" toplanacaktır.

### <a name="collecting-performance-counters-in-code"></a>Kodda performans sayaçlarını toplama
Sistem performans sayaçları toplamak ve Application Insights'a gönderme için aşağıdaki kod parçacığında uyum sağlayabilir:


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```
Veya, oluşturduğunuz özel ölçümleri ile aynı şey yapabilirsiniz:

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

## <a name="performance-counters-in-analytics"></a>Analytics performans sayaçları
Arama ve performans sayacı raporlarda görüntüleme [Analytics](app-insights-analytics.md).

**PerformanceCounters** şeması sunan `category`, `counter` adı ve `instance` her performans sayacı adı.  Her uygulama için telemetri yalnızca bu uygulama için sayaçları görürsünüz. Örneğin, görmek için hangi sayaçları kullanılabilir: 

![Application Insights analytics performans sayaçları](./media/app-insights-performance-counters/analytics-performance-counters.png)

('Instance' burada başvuruyor performans sayacı örneği için değil rol veya sunucu makine örneği. Performans sayacı örneği adı genellikle sayaçları işlemci zamanı gibi işlem ya da uygulama adıyla kesim.)

Bir grafik, kullanılabilir belleğin son süresi içinde almak için: 

![Application Insights analytics'te bellek timechart](./media/app-insights-performance-counters/analytics-available-memory.png)

Gibi diğer telemetri **performanceCounters** de bir sütuna sahip `cloud_RoleInstance` uygulamanızı çalıştıran ana bilgisayar sunucu örneğinin kimliğini gösterir. Örneğin, uygulamanızın performansı ile farklı makinelerde karşılaştırmak için şunu yazın: 

![Performans rol örneğinde Application Insights tarafından analiz bölümlenmiş.](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a>ASP.NET ve Application Insights sayılar
*Özel durum oranı ve özel durumları ölçümleri arasındaki fark nedir?*

* *Özel durum oranı* bir sistem performans sayacı. CLR oluşturulan ve toplam bir örnekleme aralığı içinde aralık uzunluğu ile böler tüm işlenen ve işlenmeyen özel durumları sayar. Application Insights SDK'sı bu sonucu toplar ve portala gönderir.
* *Özel durumlar* grafiğin örnekleme aralığı portalında tarafından alınan TrackException raporları sayısıdır. Burada yazdığınız TrackException kodunuzda çağırır ve tüm içermeyen yalnızca işlenmiş özel durumları içerir [işlenmeyen özel durumlar](app-insights-asp-net-exceptions.md). 

## <a name="performance-counters-in-aspnet-core-applications"></a>Asp.Net Core uygulamaları performans sayaçları
Performans sayaçları, yalnızca uygulamanın tam .NET Framework hedefliyorsa desteklenir. .Net için performans sayaçlarını toplama yeteneği yoktur çekirdek uygulamalar.

## <a name="alerts"></a>Uyarılar
Diğer ölçümleri gibi aşağıdakileri yapabilirsiniz [bir uyarı ayarlamak](app-insights-alerts.md) bir performans sayacı belirttiğiniz bir sınır dışında giderilmediğine sizi uyarır. Uyarıları dikey penceresini açın ve eklemek uyarı'ı tıklatın.

## <a name="next"></a>Sonraki adımlar
* [Bağımlılık izleme](app-insights-asp-net-dependencies.md)
* [Özel durum izleme](app-insights-asp-net-exceptions.md)

