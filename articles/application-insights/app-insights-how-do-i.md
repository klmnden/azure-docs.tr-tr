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
ms.devlang: na
ms.topic: conceptual
ms.date: 04/04/2017
ms.author: mbullwin
ms.openlocfilehash: 8cee346a45cd20e7dd677fd7f2efed5500175598
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47096403"
---
# <a name="how-do-i--in-application-insights"></a>Application Insights’ta nasıl ... yapabilirim?
## <a name="get-an-email-when-"></a>E-posta...
### <a name="email-if-my-site-goes-down"></a>Sitem kalırsa e-posta
Ayarlanmış bir [kullanılabilirlik web testi](app-insights-monitor-web-app-availability.md).

### <a name="email-if-my-site-is-overloaded"></a>Sitem aşırı yüklüyse e-posta
Ayarlanmış bir [uyarı](app-insights-alerts.md) üzerinde **sunucu yanıt süresi**. Bir eşik değeri 1 ve 2 saniye arasında çalışmalıdır.

![](./media/app-insights-how-do-i/030-server.png)

Uygulamanızı hata kodları döndürme tarafından yükü işaretlerini de gösterebilir. Bir uyarı ayarlayın **başarısız istekler**.

Bir uyarı ayarlamak istiyorsanız **sunucu özel durumları**, yapmanız gerekebilir [bazı ek kurulum](app-insights-asp-net-exceptions.md) verileri görmek için.

### <a name="email-on-exceptions"></a>Özel durumları e-posta
1. [Özel durum izleme işlevini ayarlama](app-insights-asp-net-exceptions.md)
2. [Uyarı ayarlama](app-insights-alerts.md) ölçüm özel durum sayısı

### <a name="email-on-an-event-in-my-app"></a>Uygulamamı bir koşulda e-posta
Belirli bir olay meydana geldiğinde bir e-posta almak istediğiniz varsayalım. Application Insights sunmaz bu özelliği doğrudan, ancak bunu [bir ölçüm eşiği aştığında bir uyarı gönder](app-insights-alerts.md).

Uyarılar, üzerinde ayarlanabilir [özel ölçümler](app-insights-api-custom-events-metrics.md#trackmetric), ancak değil özel olaylar. Olay gerçekleştiğinde bir ölçüm artırmak için bazı kodlar yazma:

    telemetry.TrackMetric("Alarm", 10);

veya:

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

İki durumlu uyarılar olduğundan, düşük bir değere sona erdirilmesi uyarıyı göz önüne aldığınızda göndermek zorunda:

    telemetry.TrackMetric("Alarm", 0.5);

Grafik oluşturma [ölçüm Gezgini](app-insights-metrics-explorer.md) , uyarı görmek için:

![](./media/app-insights-how-do-i/010-alarm.png)

Artık ölçüm kısa bir süre için bir orta değer olduğunda harekete geçirmek için bir uyarı ayarlayın:

![](./media/app-insights-how-do-i/020-threshold.png)

Ortalama süresi için en düşük ayarlayın.

Ölçüm yukarıda gittiğinde hem eşiğin altında e-posta alırsınız.

Dikkat edilmesi gereken bazı noktalar:

* Bir uyarı ("uyarı" ve "iyi durumda") iki durum vardır. Yalnızca bir ölçüm alındığında durumu değerlendirilir.
* Durum değiştiğinde bir e-posta gönderilir. Her ikisi de yüksek göndermek zorunda neden ve düşük değer ölçümleri budur.
* Uyarı değerlendirmek için ortalama önceki dönem boyunca alınan değerleri alınır. Bir ölçüm alınan her seferinde, ayarladığınız süresinden daha sık e-posta gönderilmesi için gerçekleşir.
* "Uyarı" ve "iyi" e-postalar gönderilir olduğundan, iki durumlu koşulu olarak kesin olay yeniden yapmayı mı düşünüyorsunuz göz önünde bulundurun isteyebilirsiniz. Örneğin, bir "iş tamamlandı" olay yerine, başlangıç ve bitiş bir işin e-postaları nereden bir "devam eden işi" koşula sahipsiniz.

### <a name="set-up-alerts-automatically"></a>Uyarıları otomatik olarak ayarlama
[Yeni uyarılar oluşturmak için PowerShell kullanma](app-insights-alerts.md#automation)

## <a name="use-powershell-to-manage-application-insights"></a>Application Insights'ı yönetmek için PowerShell'i kullanma
* [Yeni kaynaklar oluşturma](app-insights-powershell-script-create-resource.md)
* [Yeni uyarılar oluşturun](app-insights-alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a>Farklı sürümlerine ait ayrı telemetri

* Bir uygulamada birden çok rol: tek bir Application Insights kaynağı kullanın ve filtre cloud_Rolename üzerinde. [Daha fazla bilgi](app-insights-monitor-multi-role-apps.md)
* Geliştirme, test ve yayın sürümleri ayırma: farklı Application Insgihts kaynakları kullanın. İzleme anahtarları web.config yerden devam edebiliyorduk. [Daha fazla bilgi](app-insights-separate-resources.md)
* Raporlama derleme sürümleri: bir telemetri Başlatıcısı kullanarak özellik ekleyin. [Daha fazla bilgi](app-insights-separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a>Arka uç sunucularının ve Masaüstü uygulamaları izleme
[Windows Server SDK modülü kullanmak](app-insights-windows-desktop.md).

## <a name="visualize-data"></a>Verileri görselleştirme
#### <a name="dashboard-with-metrics-from-multiple-apps"></a>Birden fazla uygulama ölçümleri içeren Pano
* İçinde [ölçüm Gezgini'nde](app-insights-metrics-explorer.md)grafiğinizi özelleştirin ve bir sık kullanılan olarak kaydedebilirsiniz. Azure panosuna sabitleyin.

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a>Diğer kaynakları ve Application Insights verilerini içeren Pano
* [Telemetri Power BI'a aktarma](app-insights-export-power-bi.md).

Veya

* SharePoint, SharePoint web bölümleri verileri görüntüleme panonuz olarak kullanın. [SQL'e aktarma için sürekli dışarı aktarma ve Stream Analytics kullanın](app-insights-code-sample-export-sql-stream-analytics.md).  PowerView için bir SharePoint web bölümü oluşturma ve veritabanı incelemek için PowerView kullanın.

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a>Anonim veya kimliği doğrulanmış kullanıcıları Filtrele
Kullanıcılarınız oturum açarsanız, ayarlayabileceğiniz [kimliği doğrulanmış kullanıcı kimliği](app-insights-api-custom-events-metrics.md#authenticated-users). (Bunu otomatik olarak gerçekleşmez.)

Ardından şunları yapabilirsiniz:

* Belirli bir kullanıcı kimliklerine göre ara

![](./media/app-insights-how-do-i/110-search.png)

* Anonim veya kimliği doğrulanmış kullanıcılara ölçütleri Filtrele

![](./media/app-insights-how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a>Özellik adlarını ve değerleri değiştirme
Oluşturma bir [filtre](app-insights-api-filtering-sampling.md#filtering). Bu, değiştirme veya uygulamanızdan Application Insights'a gönderilmeden önce telemetri filtreleme sağlar.

## <a name="list-specific-users-and-their-usage"></a>Belirli kullanıcılar ve kullanımları listeler
Yalnızca isterseniz [belirli kullanıcılar için arama](#search-specific-users), ayarlayabileceğiniz [kimliği doğrulanmış kullanıcı kimliği](app-insights-api-custom-events-metrics.md#authenticated-users).

Hangi sayfaların gibi verileri içeren bir liste istiyorsanız, bunlar bakın veya ne sıklıkta oturum açtıklarında, iki seçeneğiniz vardır:

* [Küme kimliği doğrulanmış kullanıcı kimliği](app-insights-api-custom-events-metrics.md#authenticated-users), [dışarı aktarmak için bir veritabanı](app-insights-code-sample-export-sql-stream-analytics.md) var. kullanıcı verilerinizi analiz etmek için uygun araçları kullanın.
* Az sayıda kullanıcı varsa, özel olayları veya ölçüm değeri veya olay adı istediğiniz verileri kullanarak ve kullanıcı kimliği özelliği olarak ayarlayarak ölçümleri gönderin. Sayfa görünümlerini çözümlemek için standart JavaScript trackPageView çağrısı değiştirin. Sunucu tarafı telemetri analiz etmek için kullanıcı kimliği için tüm sunucu telemetri eklemek için bir telemetri Başlatıcısı kullanın. Bundan sonra filtreleyin ve bölümlendirin ölçümleri ve kullanıcı kimliği üzerinde aramalar yapabilirsiniz.

## <a name="reduce-traffic-from-my-app-to-application-insights"></a>Application ınsights'ı uygulamamı trafiği azaltmak
* İçinde [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md), tüm modülleri gerek yoktur, bu tür bir performans sayacı Toplayıcı devre dışı bırakın.
* Kullanım [örnekleme ve filtreleme](app-insights-api-filtering-sampling.md) SDK konumunda.
* Web sayfalarınızda Ajax çağrılarının sayısı sınırı her sayfa görüntülemesindeki bildirdi. Kod parçacığında sonra `instrumentationKey:...` , Ekle: `,maxAjaxCallsPerView:3` (veya uygun bir sayı).
* Kullanıyorsanız [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), ölçüm değerleri toplu işlerin toplam sonuç göndermeden önce işlem. Bir aşırı yüklemesini sağlayan için TrackMetric() yoktur.

Daha fazla bilgi edinin [fiyatlandırma ve kotaları](app-insights-pricing.md).

## <a name="disable-telemetry"></a>Telemetri devre dışı bırak
İçin **dinamik olarak durdurmak ve başlatmak** sunucudan telemetri iletimini ve koleksiyon:

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



İçin **seçili standart Toplayıcı devre dışı** - örnek, performans sayaçları, HTTP istekleri ve bağımlılıkları - silin veya ilgili satırları açıklama [Applicationınsights.config](app-insights-api-custom-events-metrics.md). Örneğin, kendi TrackRequest veri göndermek istiyorsanız bunu.

## <a name="view-system-performance-counters"></a>Görünümü sistem performans sayaçları
Ölçüm Gezgini'nde gösterebilirsiniz ölçümler arasında sistem performans sayaçları kümesidir. Adlı önceden tanımlanmış bir dikey pencere yoktur **sunucuları** birkaç tanesinin görüntüler.

![Application Insights kaynağınızı açın ve sunucular](./media/app-insights-how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a>Performans sayacı verisi görürseniz
* **IIS sunucusu** kendi makinenizde veya bir VM'de. [Durum İzleyicisi yükleme](app-insights-monitor-performance-live-website-now.md).
* **Azure web sitesi** -performans sayaçları henüz desteklemiyoruz. Standart bir parçası olarak Azure web sitesi Denetim Masası'nı alabilirsiniz birkaç ölçüm vardır.
* **UNIX sunucusu** - [toplanan yükleyin](app-insights-java-collectd.md)

### <a name="to-display-more-performance-counters"></a>Daha fazla performans sayaçlarını görüntülemek için
* İlk olarak, [yeni bir grafik ekleyin](app-insights-metrics-explorer.md) ve sayaç sunuyoruz temel kümesinde olup olmadığına bakın.
* Aksi halde [performansı sayaç modülü tarafından toplanan kümesi sayaç eklemek](app-insights-performance-counters.md).
