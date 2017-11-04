---
title: "Nasıl yedeklerim... Azure Application Insights'ta | Microsoft Docs"
description: "Application ınsights'ta SSS."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 48b2b644-92e4-44c3-bc14-068f1bbedd22
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: mbullwin
ms.openlocfilehash: a32127f14c93012b5ace11ff982824f9ecba7d94
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="how-do-i--in-application-insights"></a>Application Insights’ta nasıl ... yapabilirim?
## <a name="get-an-email-when-"></a>Bir e-posta almak zaman...
### <a name="email-if-my-site-goes-down"></a>Sitem devre dışı kalırsa e-posta
Ayarlanmış bir [kullanılabilirlik web testi](app-insights-monitor-web-app-availability.md).

### <a name="email-if-my-site-is-overloaded"></a>Sitem aşırı yüklüyse e-posta
Ayarlanmış bir [uyarı](app-insights-alerts.md) üzerinde **sunucu yanıt süresi**. 1 ve 2 saniye arasında bir eşik çalışması gerekir.

![](./media/app-insights-how-do-i/030-server.png)

Uygulamanızı hata kodları döndürme yükü belirtileri da gösterebilir. Bir uyarı ayarlamak **başarısız istekler**.

Bir uyarı ayarlamak istiyorsanız **sunucu özel durumları**, yapmak zorunda kalabilirsiniz [bazı ek kurulum](app-insights-asp-net-exceptions.md) verileri görmek için.

### <a name="email-on-exceptions"></a>Özel durumları e-posta
1. [Özel durum izleme işlevini ayarlama](app-insights-asp-net-exceptions.md)
2. [Bir uyarı ayarlamak](app-insights-alerts.md) ölçüm özel durum sayısı

### <a name="email-on-an-event-in-my-app"></a>Uygulamam olayda e-posta
Şimdi belirli bir olay gerçekleştiğinde bir e-posta almak istersiniz varsayalım. Application Insights sunmaz bu tesis doğrudan, ancak bunu [bir eşik ölçüm kestiği olduğunda bir uyarı gönder](app-insights-alerts.md).

Uyarılar, üzerinde ayarlanabilir [özel ölçümleri](app-insights-api-custom-events-metrics.md#trackmetric)olmayan özel olaylar ancak. Olay gerçekleştiğinde bir ölçüm artırmak için bazı kod yazın:

    telemetry.TrackMetric("Alarm", 10);

Ya da:

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

İki durumlu uyarıları sahip olduğundan, düşük bir değere sona erdirilmesi için uyarıyı göz önüne aldığınızda göndermek zorunda:

    telemetry.TrackMetric("Alarm", 0.5);

Grafik oluşturma [ölçüm Gezgini](app-insights-metrics-explorer.md) , uyarı görmek için:

![](./media/app-insights-how-do-i/010-alarm.png)

Artık bir uyarı ölçüm kısa bir süre için bir orta değer gittiğinde yangın ayarlayın:

![](./media/app-insights-how-do-i/020-threshold.png)

Ortalama süresi en az olarak ayarlayın.

E-postaları ölçüm yukarıda gittiğinde hem eşiğin altına elde edersiniz.

Dikkate alınması gereken bazı noktalar:

* Bir uyarı iki durumlu ("uyarı" ve "sağlıklı") sahiptir. Yalnızca bir ölçü alındığında durumu değerlendirilir.
* Durumu değiştiğinde bir e-posta gönderilir. Her ikisi de yüksek göndermek zorunda neden ve düşük değer ölçümleri budur.
* Uyarı değerlendirmek için ortalama önündeki nokta alınan değerlerini alınır. Ölçüm alınan her zaman, e-postaları ayarladığınız süresinden daha sık gönderilmesini şekilde gerçekleşir.
* Hem "uyarı" ve "sağlıklı" e-postaları gönderilen olduğundan, tek adımda olayınızın iki durumlu koşul olarak yeniden düşünüyorum düşünmek isteyebilirsiniz. Örneğin, "işlem tamamlandı" bir olay yerine, bir işin başlangıç ve sonunda e-postaları nereden bir "devam eden işi" koşula sahipsiniz.

### <a name="set-up-alerts-automatically"></a>Uyarıları otomatik olarak ayarla
[Yeni uyarılar oluşturmak için PowerShell kullanın](app-insights-alerts.md#automation)

## <a name="use-powershell-to-manage-application-insights"></a>Application Insights yönetmek için PowerShell kullanma
* [Yeni kaynaklar oluşturma](app-insights-powershell-script-create-resource.md)
* [Yeni uyarılar oluştur](app-insights-alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a>Farklı sürümlerdeki ayrı telemetri

* Bir uygulama içinde birden çok rol: tek bir Application Insights kaynağı kullanın ve filtre cloud_Rolename üzerinde. [Daha fazla bilgi](app-insights-monitor-multi-role-apps.md)
* Geliştirme, test ve yayın sürümleri ayırma: farklı Application Insights kaynakları kullanın. Web.config araçları anahtarları seçin. [Daha fazla bilgi](app-insights-separate-resources.md)
* Sürümleri derleme raporlama: telemetri Başlatıcı kullanarak özellik ekleme. [Daha fazla bilgi](app-insights-separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a>Arka uç sunucularının ve Masaüstü uygulamaları izleme
[Windows Server SDK modülü kullanmak](app-insights-windows-desktop.md).

## <a name="visualize-data"></a>Verileri görselleştirin
#### <a name="dashboard-with-metrics-from-multiple-apps"></a>Birden çok uygulamalardan ölçümlerle Panosu
* İçinde [ölçüm Gezgini](app-insights-metrics-explorer.md), grafiğinizi özelleştirmek ve sık kullanılan olarak Kaydet. Azure panosuna sabitleyin.

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a>Diğer kaynaklardan ve Application Insights verilerle Panosu
* [Power BI telemetri verme](app-insights-export-power-bi.md).

Veya

* SharePoint web bölümleri verileri görüntüleme panonuz olarak SharePoint kullanın. [SQL için dışarı aktarmak için sürekli dışarı aktarma ve akış analizi kullanın](app-insights-code-sample-export-sql-stream-analytics.md).  Bir SharePoint web bölümü için PowerView oluşturma ve veritabanı incelemek için PowerView kullanın.

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a>Anonim veya kimliği doğrulanmış kullanıcıları Filtrele
Kullanıcılarınız oturum açarsa, ayarlayabileceğiniz [kullanıcı kimliği kimlik doğrulaması](app-insights-api-custom-events-metrics.md#authenticated-users). (Bunu otomatik olarak gerçekleşmez.)

Sonra şunları yapabilirsiniz:

* Belirli bir kullanıcı kimliklerine göre ara

![](./media/app-insights-how-do-i/110-search.png)

* Anonim veya kimliği doğrulanmış kullanıcılar için filtre ölçümleri

![](./media/app-insights-how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a>Özellik adlarını ve değerleri değiştirme
Oluşturma bir [filtre](app-insights-api-filtering-sampling.md#filtering). Bu, değiştirmek veya telemetriyi uygulamanızdan Application Insights'a gönderilmeden önce Filtre sağlar.

## <a name="list-specific-users-and-their-usage"></a>Liste belirli kullanıcılar ve kullanımı
Yalnızca istiyorsanız, [belirli kullanıcılar için arama](#search-specific-users), ayarlayabileceğiniz [kullanıcı kimliği kimlik doğrulaması](app-insights-api-custom-events-metrics.md#authenticated-users).

Hangi sayfaların gibi verileri sahip kullanıcıların listesini istiyorsanız, bunlar bakmak veya ne sıklıkta oturum, iki seçeneğiniz vardır:

* [Kümesi kimliği doğrulanmış kullanıcı kimliği](app-insights-api-custom-events-metrics.md#authenticated-users), [dışarı aktarmak için bir veritabanı](app-insights-code-sample-export-sql-stream-analytics.md) ve orada kullanıcı verilerinizi çözümlemek için uygun araçları kullanın.
* Az sayıda kullanıcılar varsa, özel olaylar veya ilgi veri ölçüm değeri veya olay adı kullanarak ve kullanıcı kimliği bir özellik olarak ayarlayarak ölçümleri gönderin. Sayfa görünümleri analiz etmek için standart JavaScript trackPageView çağrısı değiştirin. Sunucu tarafı telemetri çözümlemek için kullanıcı kimliği için tüm sunucu telemetri eklemek için bir telemetri Başlatıcı kullanın. Bundan sonra filtre ve segment ölçümleri ve kullanıcı kimliği arama yapabilirsiniz.

## <a name="reduce-traffic-from-my-app-to-application-insights"></a>Application Insights'a Uygulamam trafiği azaltmak
* İçinde [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md), gerek yoktur, herhangi bir modül gibi performans sayacı Toplayıcı devre dışı bırakın.
* Kullanım [örnekleme ve filtre](app-insights-api-filtering-sampling.md) SDK adresindeki.
* Web sayfalarınızda Ajax çağrıları her sayfa görünümü için bildirilen sayısını sınırlayın. Sonra kod parçacığında bulunan `instrumentationKey:...` , Ekle: `,maxAjaxCallsPerView:3` (veya uygun rakam).
* Kullanıyorsanız, [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), sonuç göndermeden önce toplu ölçüm değerlerin toplamını işlem. Bir aşırı yüklemesini için sağlayan TrackMetric() yoktur.

Daha fazla bilgi edinmek [fiyatlandırma ve kotaları](app-insights-pricing.md).

## <a name="disable-telemetry"></a>Telemetri devre dışı bırak
İçin **dinamik olarak durdurmak ve başlatmak** sunucudan telemetri iletimini ve koleksiyon:

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



İçin **seçili standart toplayıcıları devre dışı** - örnek, performans sayaçları, HTTP isteklerini veya bağımlılıkları - silin veya açıklama ilgili satırları çıkışı [Applicationınsights.config](app-insights-api-custom-events-metrics.md). Örneğin, kendi TrackRequest veri göndermek istiyorsanız, bunu.

## <a name="view-system-performance-counters"></a>Görünüm sistem performans sayaçları
Ölçüm Gezgininde Göster ölçümler arasında bir sistem performans sayaçları kümesidir. Başlıklı önceden tanımlanmış bir dikey pencere yok **sunucuları** birkaç görüntüler.

![Application Insights kaynağınıza açın ve sunucuları'nı tıklatın](./media/app-insights-how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a>Hiçbir performans sayacı verilerini görürseniz
* **IIS server** kendi makine veya bir VM üzerinde. [Durum İzleyicisi yükleme](app-insights-monitor-performance-live-website-now.md).
* **Azure web sitesi** -performans sayaçları henüz desteklemiyoruz. Azure web sitesi Denetim Masası'nı standart bir parçası olarak alabilirsiniz çeşitli ölçümleri vardır.
* **UNIX sunucusu** - [collectd yükleyin](app-insights-java-collectd.md)

### <a name="to-display-more-performance-counters"></a>Daha fazla performans sayaçlarını görüntülemek için
* İlk olarak, [yeni bir grafik ekleyin](app-insights-metrics-explorer.md) ve sayaç sunuyoruz temel kümesinde olup olmadığını görebilirsiniz.
* Aksi durumda, [performans sayacı modülü tarafından toplanan kümesine sayaç eklemek](app-insights-performance-counters.md).
