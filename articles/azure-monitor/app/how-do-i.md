---
title: Nasıl... yapabilirim Azure Application Insights | Microsoft Docs
description: Application ınsights'ta SSS.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 48b2b644-92e4-44c3-bc14-068f1bbedd22
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/04/2017
ms.author: mbullwin
ms.openlocfilehash: 5e22a3f3b362811fd87460ec41b61a990f4d83fb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60902115"
---
# <a name="how-do-i--in-application-insights"></a>Application Insights’ta nasıl ... yapabilirim?
## <a name="get-an-email-when-"></a>E-posta...
### <a name="email-if-my-site-goes-down"></a>Sitem kalırsa e-posta
Ayarlanmış bir [kullanılabilirlik web testi](../../azure-monitor/app/monitor-web-app-availability.md).

### <a name="email-if-my-site-is-overloaded"></a>Sitem aşırı yüklüyse e-posta
Ayarlanmış bir [uyarı](../../azure-monitor/app/alerts.md) üzerinde **sunucu yanıt süresi**. Bir eşik değeri 1 ve 2 saniye arasında çalışmalıdır.

![](./media/how-do-i/030-server.png)

Uygulamanızı hata kodları döndürme tarafından yükü işaretlerini de gösterebilir. Bir uyarı ayarlayın **başarısız istekler**.

Bir uyarı ayarlamak istiyorsanız **sunucu özel durumları**, yapmanız gerekebilir [bazı ek kurulum](../../azure-monitor/app/asp-net-exceptions.md) verileri görmek için.

### <a name="email-on-exceptions"></a>Özel durumları e-posta
1. [Özel durum izleme işlevini ayarlama](../../azure-monitor/app/asp-net-exceptions.md)
2. [Uyarı ayarlama](../../azure-monitor/app/alerts.md) ölçüm özel durum sayısı

### <a name="email-on-an-event-in-my-app"></a>Uygulamamı bir koşulda e-posta
Belirli bir olay meydana geldiğinde bir e-posta almak istediğiniz varsayalım. Application Insights sunmaz bu özelliği doğrudan, ancak bunu [bir ölçüm eşiği aştığında bir uyarı gönder](../../azure-monitor/app/alerts.md).

Uyarılar, üzerinde ayarlanabilir [özel ölçümler](../../azure-monitor/app/api-custom-events-metrics.md#trackmetric), ancak değil özel olaylar. Olay gerçekleştiğinde bir ölçüm artırmak için bazı kodlar yazma:

    telemetry.TrackMetric("Alarm", 10);

veya:

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

İki durumlu uyarılar olduğundan, düşük bir değere sona erdirilmesi uyarıyı göz önüne aldığınızda göndermek zorunda:

    telemetry.TrackMetric("Alarm", 0.5);

Grafik oluşturma [ölçüm Gezgini](../../azure-monitor/app/metrics-explorer.md) , uyarı görmek için:

![](./media/how-do-i/010-alarm.png)

Artık ölçüm kısa bir süre için bir orta değer olduğunda harekete geçirmek için bir uyarı ayarlayın:

![](./media/how-do-i/020-threshold.png)

Ortalama süresi için en düşük ayarlayın.

Ölçüm yukarıda gittiğinde hem eşiğin altında e-posta alırsınız.

Dikkat edilmesi gereken bazı noktalar:

* Bir uyarı ("uyarı" ve "iyi durumda") iki durum vardır. Yalnızca bir ölçüm alındığında durumu değerlendirilir.
* Durum değiştiğinde bir e-posta gönderilir. Her ikisi de yüksek göndermek zorunda neden ve düşük değer ölçümleri budur.
* Uyarı değerlendirmek için ortalama önceki dönem boyunca alınan değerleri alınır. Bir ölçüm alınan her seferinde, ayarladığınız süresinden daha sık e-posta gönderilmesi için gerçekleşir.
* "Uyarı" ve "iyi" e-postalar gönderilir olduğundan, iki durumlu koşulu olarak kesin olay yeniden yapmayı mı düşünüyorsunuz göz önünde bulundurun isteyebilirsiniz. Örneğin, bir "iş tamamlandı" olay yerine, başlangıç ve bitiş bir işin e-postaları nereden bir "devam eden işi" koşula sahipsiniz.

### <a name="set-up-alerts-automatically"></a>Uyarıları otomatik olarak ayarlama
[Yeni uyarılar oluşturmak için PowerShell kullanma](../../azure-monitor/app/alerts.md#automation)

## <a name="use-powershell-to-manage-application-insights"></a>Application Insights'ı yönetmek için PowerShell'i kullanma
* [Yeni kaynaklar oluşturma](../../azure-monitor/app/powershell-script-create-resource.md)
* [Yeni uyarılar oluşturun](../../azure-monitor/app/alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a>Farklı sürümlerine ait ayrı telemetri

* Bir uygulamada birden çok rol: Tek bir Application Insights kaynağı kullanın ve filtre [cloud_Rolename](../../azure-monitor/app/app-map.md).
* Geliştirme, test ve yayın sürümleri ayırma: Farklı Application Insgihts kaynakları kullanın. İzleme anahtarları web.config yerden devam edebiliyorduk. [Daha fazla bilgi edinin](../../azure-monitor/app/separate-resources.md)
* Raporlama derleme sürümleri: Bir telemetri Başlatıcısı aracılığıyla bir özellik ekleyin. [Daha fazla bilgi edinin](../../azure-monitor/app/separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a>Arka uç sunucularının ve Masaüstü uygulamaları izleme
[Windows Server SDK modülü kullanmak](../../azure-monitor/app/windows-desktop.md).

## <a name="visualize-data"></a>Verileri görselleştirme
#### <a name="dashboard-with-metrics-from-multiple-apps"></a>Birden fazla uygulama ölçümleri içeren Pano
* İçinde [ölçüm Gezgini'nde](../../azure-monitor/app/metrics-explorer.md)grafiğinizi özelleştirin ve bir sık kullanılan olarak kaydedebilirsiniz. Azure panosuna sabitleyin.

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a>Diğer kaynakları ve Application Insights verilerini içeren Pano
* [Telemetri Power BI'a aktarma](../../azure-monitor/app/export-power-bi.md ).

Or

* SharePoint, SharePoint web bölümleri verileri görüntüleme panonuz olarak kullanın. [SQL'e aktarma için sürekli dışarı aktarma ve Stream Analytics kullanın](../../azure-monitor/app/code-sample-export-sql-stream-analytics.md).  PowerView için bir SharePoint web bölümü oluşturma ve veritabanı incelemek için PowerView kullanın.

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a>Anonim veya kimliği doğrulanmış kullanıcıları Filtrele
Kullanıcılarınız oturum açarsanız, ayarlayabileceğiniz [kimliği doğrulanmış kullanıcı kimliği](../../azure-monitor/app/api-custom-events-metrics.md#authenticated-users). (Bunu otomatik olarak gerçekleşmez.)

Ardından şunları yapabilirsiniz:

* Belirli bir kullanıcı kimliklerine göre ara

![](./media/how-do-i/110-search.png)

* Anonim veya kimliği doğrulanmış kullanıcılara ölçütleri Filtrele

![](./media/how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a>Özellik adlarını ve değerleri değiştirme
Oluşturma bir [filtre](../../azure-monitor/app/api-filtering-sampling.md#filtering). Bu, değiştirme veya uygulamanızdan Application Insights'a gönderilmeden önce telemetri filtreleme sağlar.

## <a name="list-specific-users-and-their-usage"></a>Belirli kullanıcılar ve kullanımları listeler
Yalnızca isterseniz [belirli kullanıcılar için arama](#search-specific-users), ayarlayabileceğiniz [kimliği doğrulanmış kullanıcı kimliği](../../azure-monitor/app/api-custom-events-metrics.md#authenticated-users).

Hangi sayfaların gibi verileri içeren bir liste istiyorsanız, bunlar bakın veya ne sıklıkta oturum açtıklarında, iki seçeneğiniz vardır:

* [Küme kimliği doğrulanmış kullanıcı kimliği](../../azure-monitor/app/api-custom-events-metrics.md#authenticated-users), [dışarı aktarmak için bir veritabanı](../../azure-monitor/app/code-sample-export-sql-stream-analytics.md) var. kullanıcı verilerinizi analiz etmek için uygun araçları kullanın.
* Az sayıda kullanıcı varsa, özel olayları veya ölçüm değeri veya olay adı istediğiniz verileri kullanarak ve kullanıcı kimliği özelliği olarak ayarlayarak ölçümleri gönderin. Sayfa görünümlerini çözümlemek için standart JavaScript trackPageView çağrısı değiştirin. Sunucu tarafı telemetri analiz etmek için kullanıcı kimliği için tüm sunucu telemetri eklemek için bir telemetri Başlatıcısı kullanın. Bundan sonra filtreleyin ve bölümlendirin ölçümleri ve kullanıcı kimliği üzerinde aramalar yapabilirsiniz.

## <a name="reduce-traffic-from-my-app-to-application-insights"></a>Application ınsights'ı uygulamamı trafiği azaltmak
* İçinde [Applicationınsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md), tüm modülleri gerek yoktur, bu tür bir performans sayacı Toplayıcı devre dışı bırakın.
* Kullanım [örnekleme ve filtreleme](../../azure-monitor/app/api-filtering-sampling.md) SDK konumunda.
* Web sayfalarınızda Ajax çağrılarının sayısı sınırı her sayfa görüntülemesindeki bildirdi. Kod parçacığında sonra `instrumentationKey:...` , Ekle: `,maxAjaxCallsPerView:3` (veya uygun bir sayı).
* Kullanıyorsanız [TrackMetric](../../azure-monitor/app/api-custom-events-metrics.md#trackmetric), ölçüm değerleri toplu işlerin toplam sonuç göndermeden önce işlem. Bir aşırı yüklemesini sağlayan için TrackMetric() yoktur.

Daha fazla bilgi edinin [fiyatlandırma ve kotaları](../../azure-monitor/app/pricing.md).

## <a name="disable-telemetry"></a>Telemetri devre dışı bırak
İçin **dinamik olarak durdurmak ve başlatmak** sunucudan telemetri iletimini ve koleksiyon:

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



İçin **seçili standart Toplayıcı devre dışı** - örnek, performans sayaçları, HTTP istekleri ve bağımlılıkları - silin veya ilgili satırları açıklama [Applicationınsights.config](../../azure-monitor/app/api-custom-events-metrics.md). Örneğin, kendi TrackRequest veri göndermek istiyorsanız bunu.

## <a name="view-system-performance-counters"></a>Görünümü sistem performans sayaçları
Ölçüm Gezgini'nde gösterebilirsiniz ölçümler arasında sistem performans sayaçları kümesidir. Adlı önceden tanımlanmış bir dikey pencere yoktur **sunucuları** birkaç tanesinin görüntüler.

![Application Insights kaynağınızı açın ve sunucular](./media/how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a>Performans sayacı verisi görürseniz
* **IIS sunucusu** kendi makinenizde veya bir VM'de. [Durum İzleyicisi yükleme](../../azure-monitor/app/monitor-performance-live-website-now.md).
* **Azure web sitesi** -performans sayaçları henüz desteklemiyoruz. Standart bir parçası olarak Azure web sitesi Denetim Masası'nı alabilirsiniz birkaç ölçüm vardır.
* **UNIX sunucusu** - [toplanan yükleyin](../../azure-monitor/app/java-collectd.md)

### <a name="to-display-more-performance-counters"></a>Daha fazla performans sayaçlarını görüntülemek için
* İlk olarak, [yeni bir grafik ekleyin](../../azure-monitor/app/metrics-explorer.md) ve sayaç sunuyoruz temel kümesinde olup olmadığına bakın.
* Aksi halde [performansı sayaç modülü tarafından toplanan kümesi sayaç eklemek](../../azure-monitor/app/performance-counters.md).
