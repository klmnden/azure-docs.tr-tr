---
title: "Panolar ve Gezinti bölmesinde Azure Application Insights | Microsoft Docs"
description: "Anahtar APM grafikler ve sorguları görünümleri oluşturun."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 39b0701b-2fec-4683-842a-8a19424f67bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 9987f04e7e71df5fe10c8bc209a390cb940ec4f2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="navigation-and-dashboards-in-the-application-insights-portal"></a>Gezinti ve Application Insights portalında panoları
Sonra [projenizde Application Insights'ı ayarlamak](app-insights-overview.md), uygulamanızın performansı ve kullanımı hakkında telemetri verileri projenizin Application Insights kaynağını görünür [Azure portal](https://portal.azure.com).

## <a name="find-your-telemetry"></a>Telemetrinizi Bul
Oturum [Azure portal](https://portal.azure.com) ve uygulamanız için oluşturduğunuz Application Insights kaynağı gidin.

![Gözat'ı tıklatın, Application Insights ve ardından uygulamanızı seçin.](./media/app-insights-dashboards/00-start.png)

Uygulamanız için genel bakış dikey (sayfa) anahtar tanılama ölçümleri, uygulamanızın bir özetini gösterir ve Portalı'nın diğer özellikleri için bir ağ geçididir.

![Önde gelen yollar telemetrinizi görüntüleme](./media/app-insights-dashboards/010-oview.png)

Tüm grafikler ve kılavuzları özelleştirebilir ve bunları bir panoya Sabitle. Bu şekilde, merkezi bir Panoda farklı uygulamalardan birlikte anahtar telemetri getirebilirsiniz.

## <a name="dashboards"></a>Panolar
İçin oturum açtıktan sonra gördüğünüz ilk şey [Microsoft Azure portal](https://portal.azure.com) bir Pano. Burada birlikte telemetrisinden dahil olmak üzere tüm Azure kaynaklarınızı, üzerinden sizin için en önemli olan grafikleri getirebileceğinizden [Azure Application Insights](app-insights-overview.md).

![Özelleştirilmiş bir Pano.](./media/app-insights-dashboards/31.png)

1. **Belirli kaynaklara gidin** uygulamanıza Application Insights gibi: sol Çubuğu'nu kullanın.
2. **Geçerli panoya dönmek**, veya geçiş son diğer görünümlere: sol üst kısmında açılan menüsünü kullanın.
3. **Geçiş panolar**: Pano başlığında açılan menüsünü kullanın
4. **Oluşturma, düzenleme ve panoları paylaşmak** Pano araç.
5. **Pano düzenleme**: kutucuğunun üzerine gelerek ve taşımak, özelleştirme veya kaldırmak için üst çubuğunu kullanabilirsiniz.

## <a name="add-to-a-dashboard"></a>Bir Pano ekleyin
Bir dikey veya özellikle ilginç grafikler kümesi bakarken bir kopyasını panoya sabitleyebilirsiniz. Sonraki sefer var. dönmek görürsünüz.

![Bir grafik sabitlemek için üzerine gelin ve ardından üstbilgisindeki "...".](./media/app-insights-dashboards/33.png)

1. Pano grafiği PIN. Grafik bir kopyasını panosunda görüntülenir.
2. Pano tüm dikey pencere sabitlemek - aracılığıyla tıklayabilirsiniz bir kutucuk olarak panosunda görüntülenir.
3. Geçerli panoya dönmek için sol üst köşede'ı tıklatın. Ardından geçerli görünümüne dönmek için açılan menüyü kullanın.

Grafikleri döşeme gruplandırılır dikkat edin: bir kutucuk birden fazla grafik içerebilir. Panoya tüm kutucuğu sabitleyin.

Grafik, grafiğin zaman aralığı bağımlı sıklık otomatik olarak yenilenir:

* Zaman aralığı 1 saat: her 5 dakikada bir Yenile
* Aralık 1-24 saat saat: 15 dakikada bir Yenile
* Zaman aralığı 24 saat yukarıda: (zaman aralığı) / 60.

### <a name="pin-any-query-in-analytics"></a>Herhangi bir sorgu analizleri sabitleyin
Ayrıca [Analytics PIN](app-insights-analytics-using.md#pin-to-dashboard) için grafikleri bir [paylaşılan](#share-dashboards-with-your-team) Pano. Bu, standart ölçümleri yanında herhangi bir rastgele sorgu grafiklerini eklemenize olanak sağlar. 

Sonuçları her saat otomatik olarak hesaplanır. Hemen yeniden hesaplamak için grafiği Yenile simgesine tıklayın. (Tarayıcı Yenile hesaplamaz.)

## <a name="adjust-a-tile-on-the-dashboard"></a>Panoda bir kutucuğu Ayarla
Panoda bir kutucuğu olduktan sonra ayarlayabilirsiniz.

![Düzenlemek için bir grafik üzerine gelin.](./media/app-insights-dashboards/36.png)

1. Bir grafik döşeme ekleyin.
2. Ölçüm, grup tarafından boyut ve grafiğinin stilini (tablo, grafik) ayarlayın.
3. Diyagramı yakınlaştırır sürükleyin; timespan sıfırlamak için geri düğmesini tıklatın; Döşeme grafiklerde filtre özelliklerini ayarlayın.
4. Kutucuk başlığı ayarlayın.

Ölçüm Gezgini dikey penceresinden Sabitlenen kutucuklar bir genel bakış dikey penceresinden Sabitlenen kutucuklar'den daha fazla düzenleme seçeneğiniz vardır.

Sabitlenmiş özgün döşeme düzenlemeleriniz tarafından etkilenmez.

## <a name="switch-between-dashboards"></a>Panolar arasında geçiş
Birden fazla panoyu kaydedin ve bunlar arasında geçiş yapabilirsiniz. Bir grafik veya dikey pencere sabitlemek bunlar geçerli panoya eklenmesi.

![Panolar arasında geçiş yapmak için Pano tıklatın ve kaydedilmiş bir Pano seçin. Yeni bir Pano kaydedip oluşturmak için Yeni'yi tıklatın. Yeniden düzenlemek için Düzenle'yi tıklatın.](./media/app-insights-dashboards/32.png)

Örneğin, takım odası ve genel geliştirme için başka bir tam ekran görüntüleme için tek bir Panoda olabilir.

Panoda bir kutucuk bir dikey pencere görünür: dikey penceresine gitmek için tıklatın. Bir grafik özgün konumuna grafikte çoğaltır.

![Temsil ettiği dikey penceresini açmak için kutucuğa tıklayın](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a>Panoları paylaşmak
Bir Pano oluşturduğunuz zaman, diğer kullanıcılarla paylaşabilirsiniz.

![Pano üstbilgisinde Paylaş'ı tıklatın.](./media/app-insights-dashboards/41.png)

Hakkında bilgi edinin [rolleri ve erişim denetimi](app-insights-resources-roles-access-control.md).

## <a name="app-navigation"></a>Uygulama gezinme
Genel Bakış dikey penceresinde uygulamanızı hakkında daha fazla bilgi için ağ geçididir.

* **Herhangi bir grafik veya döşeme** - herhangi bir kutucuğu tıklayın veya görüntülenenlerin hakkında daha fazla ayrıntı için grafik.

### <a name="overview-blade-buttons"></a>Genel Bakış dikey düğmeleri
![Genel Bakış dikey üst gezinti çubuğu](./media/app-insights-dashboards/app-overview-top-nav.png)

* [**Ölçüm Gezgini** ](app-insights-metrics-explorer.md) -performans ve kullanım kendi grafikleri oluşturun.
* [**Arama** ](app-insights-diagnostic-search.md) - olayları istekleri, özel durumlar gibi belirli örneklerini araştırmak veya oturum izlemeleri.
* [**Analytics** ](app-insights-analytics.md) -telemetrinizi üzerinden güçlü sorgular.
* **Zaman aralığı** -dikey penceresinde tüm grafikler tarafından görüntülenen aralığını ayarlayın.
* **Silme** -bu uygulama için Application Insights kaynağını silin. Ayrıca uygulama kodunuzdan Application Insights paketlerini kaldırın, veya Düzenle [izleme anahtarını](app-insights-create-new-resource.md#copy-the-instrumentation-key) farklı bir Application Insights kaynağı telemetri yönlendirmek için uygulamanızda.

### <a name="essentials-tab"></a>Essentials sekmesi
* [İzleme anahtarını](app-insights-create-new-resource.md#copy-the-instrumentation-key) -bu uygulama kaynak tanımlar.
* Fiyatlandırma - özellikler kullanılabilir ve ayarlanmış birim caps olun.

### <a name="app-navigation-bar"></a>Uygulama gezinti çubuğu
![Sol gezinti çubuğu](./media/app-insights-dashboards/app-left-nav-bar.png)

* **Genel Bakış** -uygulama genel bakış dikey penceresine dönün.
* **Etkinlik günlüğü** -uyarı ve Azure yönetim olayları.
* [**Erişim denetimi** ](app-insights-resources-roles-access-control.md) -takım üyeleri ve diğerleri erişim sağlama.
* [**Etiketler** ](../azure-resource-manager/resource-group-using-tags.md) -uygulamanızı başkalarıyla gruplandırmak için etiketler kullanın.

ARAŞTIR

* [**Uygulama eşlemesi** ](app-insights-app-map.md) -, uygulamanızın bileşenleri gösteren etkin eşleme türetilmiş bağımlılık bilgileri.
* [**Akıllı algılama** ](app-insights-proactive-diagnostics.md) -son performans uyarıları gözden geçirin.
* [**Canlı akış** ](app-insights-live-stream.md) - sabit neredeyse anında ölçümler, yeni bir yapı dağıtırken yararlı kümesi veya hata ayıklama A.
* [**Kullanılabilirlik / Web testleri** ](app-insights-monitor-web-app-availability.md) -world.* geçici web uygulamanıza normal İsteği Gönder
* [**Hataları, performans** ](app-insights-web-monitor-performance.md) -özel durumları, hata oranları ve istekleri uygulamanız için ve uygulamanıza gelen isteklere yanıt süreleri [bağımlılıkları](app-insights-asp-net-dependencies.md).
* [**Performans** ](app-insights-web-monitor-performance.md) -yanıt süresi, bağımlılık yanıt sürelerini.
* [Sunucuları](app-insights-web-monitor-performance.md) -performans sayaçları. Kullanılabilir ise, [Durum İzleyicisi yükleme](app-insights-monitor-performance-live-website-now.md).
* **Tarayıcı** -sayfa görünümü ve AJAX performans. Kullanılabilir ise, [web sayfalarınıza izleme](app-insights-javascript.md).
* **Kullanım** -sayfa görünümü, kullanıcı ve oturum sayısını. Kullanılabilir ise, [web sayfalarınıza izleme](app-insights-javascript.md).

YAPILANDIRMA

* **Başlarken** -satır içi öğretici.
* **Özellikler** -izleme anahtarı, abonelik ve kaynak kimliği.
* [Uyarıları](app-insights-alerts.md) -ölçüm uyarı yapılandırma.
* [Sürekli verme](app-insights-export-telemetry.md) -Azure depolama alanına telemetri verilmesini yapılandırın.
* [Performans testi](app-insights-monitor-web-app-availability.md#performance-tests) -Web sitenizde yapay yük ayarlayın.
* [Kota ve fiyatlarını](app-insights-pricing.md) ve [alım örnekleme](app-insights-sampling.md).
* **API erişimini** -oluşturmak [yayın ek açıklamaları](app-insights-annotations.md) ve veri erişim API'si.
* [**İş öğeleri** ](app-insights-diagnostic-search.md#create-work-item) -telemetri incelenirken hatalar oluşturabilmesi için izleme sistemi iş bağlanın.

AYARLAR

* [**Kilitler** ](../azure-resource-manager/resource-group-lock-resources.md) -Azure kaynaklarını Kilitle
* [**Otomasyon betiğini** ](app-insights-powershell.md) -Azure kaynak tanımını dışa yeni kaynak oluşturmak için bir şablon olarak kullanabilirsiniz.


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Sonraki adımlar

|  |  |
| --- | --- |
| [Ölçüm Gezgini](app-insights-metrics-explorer.md)<br/>Filtre ve segment ölçümleri |![Arama örneği](./media/app-insights-dashboards/64.png) |
| [Tanılama arama](app-insights-diagnostic-search.md)<br/>Bul ve olayları, ilgili olayları inceleyin ve hatalar oluşturma |![Arama örneği](./media/app-insights-dashboards/61.png) |
| [Analizler](app-insights-analytics.md)<br/>Güçlü sorgu dili |![Arama örneği](./media/app-insights-dashboards/63.png) |
