---
title: Bağımlılık Azure Application Insights izleme | Microsoft Docs
description: Kullanım, kullanılabilirlik ve şirket içi veya Microsoft Azure Application Insights ile web uygulaması performansını analiz edin.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: mbullwin
ms.openlocfilehash: c77b5810164aef7508f717a0f75d90cf6cba2089
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60691378"
---
# <a name="set-up-application-insights-dependency-tracking"></a>Application ınsights'ı ayarlayın: Bağımlılık izleme
A *bağımlılık* uygulamanız tarafından çağrılan bir dış bileşen. Bu genellikle adlı HTTP veya bir veritabanı veya dosya sistemi kullanılarak bir hizmettir. [Application Insights](../../azure-monitor/app/app-insights-overview.md) ne sıklıkta bağımlılık çağrı başarısız olur ve uygulama bağımlılıkları için bekleyeceği süreyi ölçer. Belirli çağrıları incelemek ve bunları istekler ve özel durumlar için ilişkilendirebilirsiniz.

Kullanıma hazır bağımlılık İzleyicisi, şu anda bu tür bir bağımlılık çağrıları raporları:

* Sunucu
  * SQL veritabanları
  * ASP.NET web uygulamaları ve HTTP tabanlı bağlamalar kullanan WCF hizmetleri
  * Yerel veya uzak HTTP çağrıları
  * Azure Cosmos DB, tablo, blob depolama ve kuyruk 
* Web sayfaları
  * AJAX çağrıları

Works kullanarak izlemeyi [bayt kod Araçları](https://msdn.microsoft.com/library/z9z62c29.aspx) yaklaşık .NET Framework (içindeki en son .NET SDK'ları) geri çağırmalar DiagnosticSource bağlı veya yöntemlerini seçin. En düşük performansa getirdiği yüktür.

Ayrıca, diğer bağımlılıklar, istemci ve sunucu kodunda her ikisi de izlemek için kendi SDK çağrıları yazabilirsiniz kullanarak [TrackDependency API](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency).

> [!NOTE]
> Azure Cosmos DB yalnızca otomatik olarak izlenir [HTTP/HTTPS](../../cosmos-db/performance-tips.md#networking) kullanılır. TCP modu, Application Insights tarafından yakalanan olmaz.

## <a name="set-up-dependency-monitoring"></a>Bağımlılık izleme işlevini ayarlama
Kısmi bağımlılık bilgileri tarafından otomatik olarak toplanan [Application Insights SDK'sı](asp-net.md). Tam veri almak için ana bilgisayar sunucusu için uygun aracıyı yükleyin.

| Platform | Yükleme |
| --- | --- |
| IIS sunucusu |Ya da [sunucunuza Durum İzleyicisi yükleme](../../azure-monitor/app/monitor-performance-live-website-now.md) veya [uygulamanızı .NET framework 4.6 veya üzeri için yükseltme](https://go.microsoft.com/fwlink/?LinkId=528259) yükleyip [Application Insights SDK'sı](asp-net.md) uygulamanızda. |
| Azure Web Uygulaması |Web uygulaması Denetim Masası'ndaki [Application Insights dikey penceresini açın, web uygulaması Denetim Masası'ndaki](../../azure-monitor/app/azure-web-apps.md) yükleme istenirse seçin. |
| Azure bulut hizmeti |[Kullanım başlangıç görevi](../../azure-monitor/app/cloudservices.md) veya [yükleme .NET framework 4.6 +](../../cloud-services/cloud-services-dotnet-install-dotnet.md) |

## <a name="where-to-find-dependency-data"></a>Bağımlılık verileri nerede bulunur
* [Uygulama Haritası](#application-map) komşu bileşenlerini ve uygulama arasındaki bağımlılıkları görselleştirir.
* [Performans ve tarayıcı hatası dikey pencereleri](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-performance) sunucu bağımlılık verileri gösterir.
* [Tarayıcılar dikey](#ajax-calls) kullanıcılarınızın tarayıcılarından AJAX çağrılarını gösterir.
* Bağımlılık çağrılarını denetlemek için yavaş veya başarısız istekleri tıklayın.
* [Analytics](#analytics) sorgu bağımlılık verileri için kullanılabilir.

## <a name="application-map"></a>Uygulama Eşlemesi
Uygulama Haritası, uygulama bileşenleri arasındaki bağımlılıkları bulmak için bir görsel Yardım olarak görev yapar. Ayrıca, uygulamanızdan alınan telemetri gelen otomatik olarak oluşturulur. Bu örnek AJAX çağrılarını tarayıcı komut dosyaları ve iki dış hizmetlerle sunucu uygulamasından REST çağrılarını gösterir.

![Uygulama Eşlemesi](./media/asp-net-dependencies/cloud-rolename.png)

* **Kutularından gidin** ilgili bağımlılık ve diğer grafikleri.
* **Harita sabitleme** için [Pano](../../azure-monitor/app/app-insights-dashboards.md), burada da tam işlevsel olmayacaktır.

[Daha fazla bilgi edinin](../../azure-monitor/app/app-map.md).

## <a name="performance-and-failure-blades"></a>Performans ve başarısızlık dikey pencereleri
Sunucu uygulama tarafından yapılan bağımlılık çağrılarına süresi performans dikey penceresini gösterir.

**Hata sayısı** gösterilir **hataları** dikey penceresi. Aralık 200-399, ya da bilinmeyen değil tüm dönüş kodlarını hatasıdır.

> [!NOTE]
> **% 100 hata mı var?** -Bu, büyük olasılıkla yalnızca kısmi bağımlılık verileri alıyorsanız gösterir. Şunları yapmanız [bağımlılık izleme platformunuz için uygun ayarlamak](#set-up-dependency-monitoring).
>
>

## <a name="ajax-calls"></a>AJAX çağrıları
Tarayıcılar dikey AJAX çağrılarından süresi ve hata oranını gösterir [JavaScript web sayfalarınızda](../../azure-monitor/app/javascript.md). Bunlar bağımlılıklar olarak gösterilir.

## <a name="diagnosis"></a> Yavaş istekleri tanılayın
Her istek olayı, bağımlılık çağrıları, özel durumlar ve uygulamanızı isteği işlerken izlenen gelen diğer olayları ile ilişkilidir. Bu nedenle bazı istekler hatalı gerçekleştiriyorsanız, yavaş bağımlılık yanıtlarının nedeniyle olup olmadığını bulabilirsiniz.

### <a name="profile-your-live-site"></a>Sitenizin Canlı profil

Hiçbir fikriniz olduğu zaman gider? [Application Insights Profiler'ı](../../azure-monitor/app/profiler.md) izlemeleri HTTP canlı sitenize çağırır ve kodunuzda hangi işlevlerin en uzun sürdü gösterir.

## <a name="analytics"></a>Analiz
Bağımlılıkları izleyebilirsiniz [Kusto sorgu dili](/azure/kusto/query/). Bazı örnekler aşağıda verilmiştir.

* Tüm başarısız bağımlılık çağrılarını bulun:

```

    dependencies | where success != "True" | take 10
```

* AJAX çağrılarını bulun:

```

    dependencies | where client_Type == "Browser" | take 10
```

* İsteklerle ilişkili bağımlılık çağrıları bulun:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* Sayfa görünümleri ile ilişkilendirilmiş bulma AJAX çağrıları:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```



## <a name="custom-dependency-tracking"></a>Özel bağımlılık izleme
Standart bağımlılık izleme modülü, veritabanları ve REST API'leri gibi dış bağımlılıkları otomatik olarak bulur. Ancak, aynı şekilde değerlendirilmesi için bazı ek bileşenler isteyebilirsiniz.

Bağımlılık bilgileri gönderir kodu aynı kullanarak yazabileceğiniz [TrackDependency API](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency) standart modülleri tarafından kullanılır.

Örneğin, kendi yazmadığınız sahip bir derleme, kodunuzu derlemek, kolaylaştırır, yanıt süreleri için hangi katkı öğrenmek için tüm çağrıları, süresi. Application ınsights bağımlılık grafiklerinde görüntülenir. Bu veriler için kullanarak göndermek `TrackDependency`.

```csharp

            var startTime = DateTime.UtcNow;
            var timer = System.Diagnostics.Stopwatch.StartNew();
            try
            {
                success = dependency.Call();
            }
            finally
            {
                timer.Stop();
                telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
                // The call above has been made obsolete in the latest SDK. The updated call follows this format:
                // TrackDependency (string dependencyTypeName, string dependencyName, string data, DateTimeOffset startTime, TimeSpan duration, bool success);
            }
```

Standart bağımlılık izleme modülünü devre dışı geçiş yapmak istiyorsanız, içinde DependencyTrackingTelemetryModule başvurusunu kaldırın [Applicationınsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md).

## <a name="troubleshooting"></a>Sorun giderme
*Bağımlılık başarı bayrağı her zaman true veya false gösterir.*

*SQL sorgu tam olarak gösterilmez.*

Aşağıdaki tabloya başvurun ve kayıtlarınızın bağımlılık uygulamanız için izlemeyi etkinleştirmek için doğru yapılandırmayı seçtiniz.

| Platform | Yükleme |
| --- | --- |
| IIS sunucusu |Her iki [sunucunuza Durum İzleyicisi yükleme](../../azure-monitor/app/monitor-performance-live-website-now.md). Veya [uygulamanızı .NET framework 4.6 veya üzeri için yükseltme](https://go.microsoft.com/fwlink/?LinkId=528259) yükleyip [Application Insights SDK'sı](asp-net.md) uygulamanızda. |
| IIS Express |IIS sunucusu bunun yerine kullanın. |
| Azure Web Uygulaması |Web uygulaması Denetim Masası'ndaki [Application Insights dikey penceresini açın, web uygulaması Denetim Masası'ndaki](../../azure-monitor/app/azure-web-apps.md) yükleme istenirse seçin. |
| Azure Cloud Service |[Kullanım başlangıç görevi](../../azure-monitor/app/cloudservices.md) veya [yükleme .NET framework 4.6 +](../../cloud-services/cloud-services-dotnet-install-dotnet.md). |

## <a name="next-steps"></a>Sonraki adımlar
* [Özel durumlar](../../azure-monitor/app/asp-net-exceptions.md)
* [Kullanıcı ve sayfa verileri](../../azure-monitor/app/javascript.md)
* [Kullanılabilirlik](../../azure-monitor/app/monitor-web-app-availability.md)
