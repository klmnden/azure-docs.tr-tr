---
title: Panolar ve gezinti Azure Application ınsights | Microsoft Docs
description: Anahtar APM grafikleri ve sorguları görünümleri oluşturun.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 39b0701b-2fec-4683-842a-8a19424f67bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: afbf2bc32aa737eb5f6dde41035b206d6e260252
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60767614"
---
# <a name="navigation-and-dashboards-in-the-application-insights-portal"></a>Gezinti ve panolar Application Insights portalında
Sonra [projenize Application ınsights'ı ayarlama](../../azure-monitor/app/app-insights-overview.md), uygulamanızın performansı ve kullanımıyla ilgili telemetri verilerini projenizin Application Insights kaynağını görünür [Azure portalında](https://portal.azure.com).

## <a name="find-your-telemetry"></a>Telemetrinizi bulun
Oturum [Azure portalında](https://portal.azure.com) ve uygulamanız için oluşturduğunuz Application Insights kaynağına gidin.

![Gözat'a tıklayın, Application ınsights'ı ve ardından uygulamanızı seçin.](./media/app-insights-dashboards/00-start.png)

Uygulamanız için genel bakış dikey (sayfa) uygulamanızın ana tanılama ölçümleri bir özetini gösterir ve Portalı'nın diğer özellikleri için bir ağ geçididir.

![Telemetrinizi görüntüleme için ana yolları](./media/app-insights-dashboards/010-oview.png)

Grafikler ve Kılavuzlar herhangi birini özelleştirin ve bunları panoya sabitleyin. Bu şekilde, merkezi bir Panoda farklı uygulamalardan birlikte anahtar telemetri getirebilirsiniz.

## <a name="dashboards"></a>Panolar
İçin oturum açtıktan sonra gördüğünüz ilk şey [Microsoft Azure Portal'da](https://portal.azure.com) bir panodur. Burada, alınan telemetri dahil olmak üzere tüm Azure kaynaklarınız genelinde, sizin için en önemli olan grafikleri bir araya getirebilirsiniz [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md).

![Özelleştirilmiş Pano.](./media/app-insights-dashboards/31.png)

1. **Belirli kaynaklara gidin** uygulamanıza Application Insights gibi: Sol taraftaki çubuğun kullanın.
2. **Geçerli panoya dönmek**, veya son diğer görünümlerle geçin: Sol üst köşede açılır menüyü kullanın.
3. **Geçiş panolar**: Pano başlığı aşağı açılan menüden kullanın
4. **Oluşturun, düzenleyin ve panoları paylaşma** Pano araç.
5. **Panoyu Düzenle**: Bir kutucuğun üzerine gelin ve kendi üst çubuktaki taşımak, özelleştirme veya kaldırmak için kullanın.

## <a name="add-to-a-dashboard"></a>Bir panoya ekleme
Bir dikey pencere veya özellikle çekici grafikler kümesini bakarken, bir kopyasını panoya sabitleyebilirsiniz. Sonraki açışınızda var. dönüş görürsünüz.

![Bir grafiği sabitlemek için üzerine gelin ve ardından üst bilgisindeki "...".](./media/app-insights-dashboards/33.png)

1. Grafiği panoya Sabitle. Grafik bir kopyasını panosunda görünür.
2. Tüm dikey pencereyi panoya sabitleme - Panoda aracılığıyla tıklayabilirsiniz bir kutucuk olarak görünür.
3. Geçerli panoya dönmek için sol üst köşedeki tıklayın. Ardından, geçerli görünüme geri dönmek için açılan menüsünü kullanabilirsiniz.

Grafikler kutucuklar gruplandırılır dikkat edin: birden fazla grafik bir kutucuk içerebilir. Tüm kutucuğu panoya sabitleyin.

Grafik, grafiğin zaman aralığı üzerinde olduğu bir sıklık ile otomatik olarak yenilenir:

* Yukarı 1 saate zaman aralığı: 5 dakikada bir yenilenecek
* Zaman aralığı 1-24 saat: 15 dakikada bir yenilenecek
* Zaman aralığı 24 saat yukarıda: (Zaman aralığı) / 60.

### <a name="pin-any-query-in-analytics"></a>Herhangi bir sorgu Analytics'te sabitleme
Ayrıca [Analytics sabitleme](../../azure-monitor/log-query/get-started-portal.md) paylaşılan panoya grafik. Bu standart ölçümlerin yanı sıra herhangi bir rastgele sorgunun grafikler eklemenize olanak sağlar. 

Sonuçlar, her saat otomatik olarak yeniden hesaplanır. Hemen yeniden hesaplamak için grafikte Yenile simgesine tıklayın. (Tarayıcı Yenile hesaplamaz.)

## <a name="adjust-a-tile-on-the-dashboard"></a>Panodaki bir kutucuğa Ayarla
Panoda bir kutucuğu hale geldikten sonra ayarlayabilirsiniz.

![Düzenlemek için bir grafik üzerine gelin.](./media/app-insights-dashboards/36.png)

1. Bir grafiği kutucuğu ekleyin.
2. Ölçüm ve Gruplama boyutu (tablo, grafik) grafiğinin stilini ayarlayın.
3. Diyagram yakınlaştırmak için sürükleyin; timespan sıfırlamak için geri düğmesine tıklayın; Grafik kutucuğuna filtre özelliklerini ayarlayın.
4. Kutucuk başlığı ayarlayın.

Ölçüm Gezgini dikey penceresinden sabitlenmiş kutucuklar bir genel bakış dikey penceresinden sabitlenmiş olan kutucuklarda daha fazla düzenleme seçeneği vardır.

Özgün kutucuk sabitlediğiniz düzenlemeleriniz tarafından etkilenmez.

## <a name="switch-between-dashboards"></a>Panolar arasında geçiş yapın
Birden fazla Pano kaydedin ve bunlar arasında geçiş yapabilirsiniz. Bir grafikte veya dikey sabitlediğinizde bunlar geçerli panoya eklenir.

![Panolar arasında geçiş yapmak için Pano tıklayın ve kaydedilmiş bir Pano seçin. Yeni bir Pano kaydedip oluşturmak için Yeni'yi tıklatın. Yeniden düzenlemek için Düzenle'ye tıklayın.](./media/app-insights-dashboards/32.png)

Örneğin, takım odası ve diğer genel geliştirme için tam ekran görüntülemek için bir Pano olabilir.

Panoda bir dikey pencere bir kutucuk olarak görünür: kamasına gitmek için tıklayın. Grafikte özgün konumuna grafikte çoğaltır.

![Temsil ettiği dikey penceresini açmak için kutucuğa tıklayın](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a>Panoları paylaşma
Bir panoyu oluşturduktan sonra diğer kullanıcılarla paylaşabilirsiniz.

![Pano üstbilgisinde Paylaş'ı tıklatın.](./media/app-insights-dashboards/41.png)

Hakkında bilgi edinin [rolleri ve erişim denetimi](../../azure-monitor/app/resources-roles-access-control.md).

## <a name="create-dashboards-programmatically"></a>Panolar program aracılığıyla oluşturma
Pano oluşturma'yı kullanarak otomatikleştirebilirsiniz [Azure Resource Manager](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically) ve basit bir JSON Düzenleyicisi.

## <a name="app-navigation"></a>Uygulama gezinti
Genel Bakış dikey penceresinde uygulamanız hakkında daha fazla bilgi için ağ geçididir.

* **Herhangi bir grafik veya kutucuk** - herhangi bir kutucuğa tıklayın veya bu görüntüler hakkında daha fazla ayrıntı için grafik.

### <a name="overview-blade-buttons"></a>Genel Bakış dikey penceresi düğmeleri
![Genel Bakış dikey penceresinin üst gezinti çubuğu](./media/app-insights-dashboards/app-overview-top-nav.png)

* [**Ölçüm Gezgini** ](../../azure-monitor/app/metrics-explorer.md) -performans ve kullanım kendi grafikler oluşturun.
* [**Arama** ](../../azure-monitor/app/diagnostic-search.md) - olayları istekleri, özel durumlar gibi belirli örneklerini inceleyin veya günlük izlemelerini.
* [**Analytics** ](../../azure-monitor/app/analytics.md) -telemetriniz üzerinde güçlü sorgular.
* **Zaman aralığı** -dikey penceresinde tüm grafikler tarafından görüntülenen aralığını ayarlayın.
* **Silme** -bu uygulama için Application Insights kaynağını silin. Ayrıca uygulama kodunuzdan Application Insights paketlerini kaldırın, veya düzenleme [izleme anahtarını](../../azure-monitor/app/create-new-resource.md #copy-the-instrumentation-key) farklı bir Application Insights kaynağına telemetri yönlendirmek için uygulamanızda.

### <a name="essentials-tab"></a>Temel bilgiler sekmesinde
* [İzleme anahtarı](../../azure-monitor/app/create-new-resource.md #copy-the-instrumentation-key) -bu uygulama kaynağı tanımlar.

### <a name="app-navigation-bar"></a>Uygulama gezinti çubuğu
![Sol gezinti çubuğu](./media/app-insights-dashboards/app-left-nav-bar.png)

* **Genel Bakış** -uygulama genel bakış dikey penceresine dönün.
* **Etkinlik günlüğü** -uyarılar ve Azure yönetim etkinlikleri.
* [**Erişim denetimi** ](../../azure-monitor/app/resources-roles-access-control.md) -takım üyeleri ve diğerleri erişim sağlama.
* [**Etiketleri** ](../../azure-resource-manager/resource-group-using-tags.md) -uygulamanızı diğerleriyle gruplandırmak için etiketleri kullanın.

ARAŞTIRMA

* [**Uygulama Haritası** ](app-map.md) -uygulamanızı bileşenlerini gösteren etkin harita türetilmiş bağımlılık bilgileri.
* [**Akıllı algılama** ](../../azure-monitor/app/proactive-diagnostics.md) -son performans uyarıları gözden geçirin.
* [**Stream canlı** ](../../azure-monitor/app/live-stream.md) - neredeyse anında ölçümler, yeni bir yapı dağıtma yararlı bir dizi sabit veya hata ayıklama A.
* [**Kullanılabilirlik / Web testleri** ](../../azure-monitor/app/monitor-web-app-availability.md) -normal istekleri web uygulamanızdan world.* geçici olarak gönder
* [**Hatalar, performans** ](../../azure-monitor/app/web-monitor-performance.md) -özel durumlar ve hata oranları istekleri için uygulamanızı ve uygulamanıza gelen istekleri için yanıt sürelerini [bağımlılıkları](../../azure-monitor/app/asp-net-dependencies.md).
* [**Performans** ](../../azure-monitor/app/web-monitor-performance.md) -yanıt süresi, bağımlılık yanıt süreleri.
* [Sunucuları](../../azure-monitor/app/web-monitor-performance.md) -performans sayaçları. Kullanılabilir, [Durum İzleyicisi yükleme](../../azure-monitor/app/monitor-performance-live-website-now.md).
* **Tarayıcı** -sayfa görünümü ve AJAX performansı. Kullanılabilir, [web sayfalarınızı araçlama](../../azure-monitor/app/javascript.md).
* **Kullanım** -sayfa görünümü, kullanıcı ve oturum sayıları. Kullanılabilir, [web sayfalarınızı araçlama](../../azure-monitor/app/javascript.md).

YAPILANDIR

* **Başlarken** -satır içi öğretici.
* **Özellikleri** -izleme anahtarı, abonelik ve kaynak kimliği.
* [Uyarılar](../../azure-monitor/app/alerts.md) -ölçüm uyarı yapılandırması.
* [Sürekli dışarı aktarma](../../azure-monitor/app/export-telemetry.md) -Azure depolama alanına telemetri dışarı aktarma yapılandırın.
* [Performans testi](../../azure-monitor/app/monitor-web-app-availability.md#performance-tests) -yapay bir yük kullanarak Web sitenizde ayarlayın.
* [Kota ve fiyatlandırma](../../azure-monitor/app/pricing.md) ve [alım örneklemesi](../../azure-monitor/app/sampling.md).
* **API erişimi** -oluşturma [sürüm ek açıklamaları](annotations.md) ve veri erişimi API'si için.
* [**İş öğeleri** ](../../azure-monitor/app/diagnostic-search.md#create-work-item) -bir iş izleme sistemini telemetri denetlenirken hatalar oluşturabilmeniz bağlanın.

AYARLAR

* [**Kilitler** ](../../azure-resource-manager/resource-group-lock-resources.md) -Azure kaynakları kilitleme
* [**Otomasyon betiği** ](../../azure-monitor/app/powershell.md) -yeni kaynaklar oluşturmak için şablon olarak kullanabilirsiniz, böylece bir Azure kaynak tanımını dışarı aktarın.


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Sonraki adımlar

|  |  |
| --- | --- |
| [Ölçüm Gezgini](../../azure-monitor/app/metrics-explorer.md)<br/>Filtreleyin ve bölümlendirin ölçümleri |![Arama örneği](./media/app-insights-dashboards/64.png) |
| [Tanılama araması](../../azure-monitor/app/diagnostic-search.md)<br/>Bul ve olayları, ilgili olayları inceleyin ve hatalar oluşturun |![Arama örneği](./media/app-insights-dashboards/61.png) |
| [Analizler](../../azure-monitor/app/analytics.md)<br/>Güçlü sorgu dili |![Arama örneği](./media/app-insights-dashboards/63.png) |
