---
title: "Azure Application Insights izleme bağımlılık | Microsoft Docs"
description: "Application Insights ile şirket içi veya Microsoft Azure web uygulamanızın kullanımını, kullanılabilirliğini ve performansını analiz edin."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: bwren
ms.openlocfilehash: 6e0b67ba98af27017901608dde4401600eb9957f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="set-up-application-insights-dependency-tracking"></a>Application Insights'ı ayarlayın: bağımlılık izleme
A *bağımlılık* uygulamanız tarafından çağrılan bir dış bileşendir. Bu genellikle HTTP veya bir veritabanı veya bir dosya sistemi kullanılarak adlı bir hizmettir. [Application Insights](app-insights-overview.md) uygulamanız için bağımlılıkları ne kadar bekleyeceğini ve ne sıklıkta bir bağımlılık araması başarısız ölçer. Belirli çağrıları araştırmak ve bunları istekler ve özel durumlar için ilişkilendirebilirsiniz.

![örnek grafikler](./media/app-insights-asp-net-dependencies/10-intro.png)

Giden kutusu bağımlılık İzleyicisi şu anda bu tür bağımlılıkların çağrıları raporları:

* Sunucu
  * SQL veritabanları
  * ASP.NET web ve HTTP tabanlı bağlamalar kullanan WCF hizmetleri
  * Yerel veya uzak HTTP çağrıları
  * Azure Cosmos DB, tablo, blob depolama ve sırası
* Web sayfaları
  * AJAX çağrıları

Kullanarak Works izleme [bayt kodu Araçları](https://msdn.microsoft.com/library/z9z62c29.aspx) seçili yöntemleri geçici. Performansa düzeydedir.

Aynı zamanda, diğer bağımlılıkları, hem de istemci ve sunucu kodu izlemek için kendi SDK çağrıları yazabilirsiniz kullanarak [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).

## <a name="set-up-dependency-monitoring"></a>Bağımlılık izleme işlevini ayarlama
Kısmi bağımlılık bilgi tarafından otomatik olarak toplanan [Application Insights SDK'sı](app-insights-asp-net.md). Tam veri almak için ana bilgisayar sunucusu için uygun aracısını yükleyin.

| Platform | Yükleme |
| --- | --- |
| IIS sunucusu |Her iki [sunucunuza Durum İzleyicisi yükleme](app-insights-monitor-performance-live-website-now.md) veya [uygulamanız .NET Framework 4.6 veya sonrası yükseltme](http://go.microsoft.com/fwlink/?LinkId=528259) yükleyip [Application Insights SDK'sı](app-insights-asp-net.md) uygulamanızda. |
| Azure Web Uygulaması |Web uygulama Denetim Masası'ndaki [, web uygulama Denetim Masası'nda Application Insights dikey penceresini açmak](app-insights-azure-web-apps.md) ve yükleme istenirse seçin. |
| Azure bulut hizmeti |[Kullanım başlangıç görevi](app-insights-cloudservices.md) veya [yükleme .NET framework 4.6 +](../cloud-services/cloud-services-dotnet-install-dotnet.md) |

## <a name="where-to-find-dependency-data"></a>Bağımlılık verileri nerede bulacağını
* [Uygulama eşlemesi](#application-map) uygulama ve neighbouring bileşenleri arasındaki bağımlılıkları visualizes.
* [Performans, tarayıcı ve hata dikey pencereleri](#performance-and-blades) sunucu bağımlılık verileri göster.
* [Tarayıcılar dikey penceresinde](#ajax-calls) kullanıcılarınızın tarayıcılarından AJAX çağrıları gösterilir.
* [Yavaş ya da başarısız isteklerinden tıklatın aracılığıyla](#diagnose-slow-requests) kendi bağımlılık denetlemek için çağırır.
* [Analytics](#analytics) sorgu bağımlılık verileri için kullanılabilir.

## <a name="application-map"></a>Uygulama Eşlemesi
Uygulama bileşenleri arasındaki bağımlılıkları keşfetmek için bir görsel Yardım uygulama eşlemesi görür. Ayrıca, uygulamanızdan alınan telemetri gelen otomatik olarak oluşturulur. Bu örnek AJAX çağrılarını tarayıcı komut dosyaları ve sunucu uygulamasından REST çağrılarını iki dış hizmetler için gösterir.

![Uygulama Eşlemesi](./media/app-insights-asp-net-dependencies/08.png)

* **Kutularından gidin** ilgili bağımlılık ve diğer grafik.
* **Harita PIN** için [Pano](app-insights-dashboards.md), burada bu tamamen işlevsel olacaktır.

[Daha fazla bilgi edinin](app-insights-app-map.md).

## <a name="performance-and-failure-blades"></a>Performans ve hata dikey pencereleri
Performans dikey penceresini sunucu uygulama tarafından oluşturulan bağımlılık çağrıları süresini gösterir. Bir Özet Grafik ve çağrısı tarafından bölümlenmiş bir tablo yok.

![Performans dikey bağımlılık grafikleri](./media/app-insights-asp-net-dependencies/dependencies-in-performance-blade.png)

Özet grafikleri veya bu çağrıları ham oluşumları aramak için Tablo öğelerini tıklatın.

![Bağımlılık çağrısı örnekleri](./media/app-insights-asp-net-dependencies/dependency-call-instance.png)

**Hata sayısı** üzerinde gösterilen **hataları** dikey. Aralık 200-399, veya bilinmeyen olmayan dönüş kodları hatasıdır.

> [!NOTE]
> **% 100 hataları?** -Bu, büyük olasılıkla kısmi bağımlılık verileri yalnızca aldıklarından gösterir. Yapmanız [bağımlılık izleme işlevini platformunuz için uygun ayarlama](#set-up-dependency-monitoring).
>
>

## <a name="ajax-calls"></a>AJAX çağrıları
Tarayıcılar dikey penceresinde AJAX çağrılarından süresi ve hata oranını gösterir [web sayfalarında JavaScript](app-insights-javascript.md). Bunlar bağımlılıklar olarak gösterilir.

## <a name="diagnosis"></a>Yavaş istekler tanılama
Her istek olayı bağımlılık çağrıları, özel durumlar ve uygulamanızı isteği işlerken izlenen diğer olayları ile ilişkilidir. Bu nedenle bazı istekleri hatalı gerçekleştiriyorsanız, bir bağımlılık yavaş yanıt nedeniyle olup olmadığını bulabilirsiniz.

Şimdi örneği, yol gösterir.

### <a name="tracing-from-requests-to-dependencies"></a>Gelen istekleri için bağımlılıkları izleme
Performans dikey penceresini açın ve istekleri kılavuz bakın:

![Ortalamalar ve sayıları istekleriyle listesi](./media/app-insights-asp-net-dependencies/02-reqs.png)

Üst biri çok uzun sürüyor. Biz zamanın nerede harcandığına çıkışı bulmak, görelim.

Satır ayrı istek olayları görmek için tıklatın:

![İstek örnekleri listesi](./media/app-insights-asp-net-dependencies/03-instances.png)

Daha fazla incelemek ve bu istekle ilişkili uzak bağımlılık çağrıları aşağıya doğru kaydırmak için uzun süre çalışan örnek tıklatın:

![Uzak bağımlılıkları çağrıları bulmak, olağan dışı süre tanımla](./media/app-insights-asp-net-dependencies/04-dependencies.png)

Bu istek bir yerel hizmete çağrıda harcanan zaman bakım çoğu gibi görünüyor.

Daha fazla bilgi için bu satırı seçin:

![Sorunlu tanımlamak için bu uzak bağımlılık ' a tıklayın](./media/app-insights-asp-net-dependencies/05-detail.png)

Bu sorunu olduğu görülüyor. Biz sorun pinpointed, biz yalnızca artık bu nedenle neden bu çağrı çok uzun sürüyor bulmanız.

### <a name="request-timeline"></a>İstek zaman çizelgesi
Farklı bir durumda, özellikle uzun hiçbir bağımlılık çağrısı yoktur. Ancak Zaman Çizelgesi görünümüne geçerek, gecikme bizim iç işlem oluştuğu görebiliriz:

![Uzak bağımlılıkları çağrıları bulmak, olağan dışı süre tanımla](./media/app-insights-asp-net-dependencies/04-1.png)

Olduğu anlaşılıyor ilk bağımlılık çağırdıktan sonra neden olan görmek için bizim kod gibi görünmelidir şekilde büyük bir boşluk olmalıdır.

### <a name="profile-your-live-site"></a>Canlı sitenizi profil

Fikir olduğu zaman gider mi? [Application Insights profil oluşturucu](app-insights-profiler.md) HTTP canlı sitenize çağırır ve hangi işlevleri kodunuzda gösterir izlemeleri en uzun zaman aldı.

## <a name="failed-requests"></a>Başarısız istekler
Başarısız olan istekler başarısız çağrılar bağımlılıkları ile ilişkili olabilir. Yeniden, biz aracılığıyla problemi izlemek için tıklatabilirsiniz.

![Başarısız istekler grafiği tıklatın](./media/app-insights-asp-net-dependencies/06-fail.png)

Aracılığıyla başarısız bir istek bir örneğini tıklatın ve onun ilişkili olay bakın.

![Bir istek türünü tıklatın, örnek aynı örneği farklı bir görünümünü almak için özel durum ayrıntıları almak için tıklatın.](./media/app-insights-asp-net-dependencies/07-faildetail.png)

## <a name="analytics"></a>Analiz
Bağımlılıkları izleyebilirsiniz [günlük analizi sorgu dili](https://docs.loganalytics.io/). Bazı örnekler aşağıda verilmiştir.

* Tüm başarısız bağımlılık çağrıları bulun:

```

    dependencies | where success != "True" | take 10
```

* AJAX çağrıları bulun:

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


* Sayfa görünümleri ile ilişkili Bul AJAX çağrıları:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```



## <a name="custom-dependency-tracking"></a>Özel bağımlılık izleme
Standart bağımlılık izleme modülü veritabanları ve REST API'leri gibi dış bağımlılıklar otomatik olarak bulur. Ancak, aynı şekilde kabul edilmesi için bazı ek bileşenleri isteyebilirsiniz.

Aynı kullanarak bağımlılık bilgi gönderir kod yazabilirsiniz [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency) standart modülleri tarafından kullanılır.

Örneğin, kendiniz yazmak kaydetmedi ile bir derlemeyi kodunuzu yapılandırdıysanız, yanıt sürelerini kolaylaştırır hangi katkı öğrenmek için tüm çağrıları, zaman. Application Insights bağımlılık grafikte görüntülenen bu verileri olmasını kullanarak göndermek `TrackDependency`.

```C#

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
            }
```

Standart bağımlılık izleme modülünü devre dışı geçiş yapmak istiyorsanız, DependencyTrackingTelemetryModule içinde başvurusunu kaldırın [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md).

## <a name="troubleshooting"></a>Sorun giderme
*Bağımlılık başarı bayrağı her zaman true veya false gösterir.*

*SQL sorgu tam olarak gösterilmez.*

* SDK'ın en son sürüme yükseltin. .NET sürüm 4.6'den az ise:
  * IIS ana: yükleme [Application Insights Aracısı](app-insights-monitor-performance-live-website-now.md) ana bilgisayar sunucuları üzerinde.
  * Azure web uygulaması: açık Application Insights sekmesinde web uygulama Denetim Masası'nda ve Application Insights yükleyin.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Sonraki adımlar
* [Özel durumlar](app-insights-asp-net-exceptions.md)
* [Kullanıcı ve sayfa verileri](app-insights-javascript.md)
* [Kullanılabilirlik](app-insights-monitor-web-app-availability.md)
