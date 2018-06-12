---
title: Azure Application Insights'ta arama'yı kullanarak | Microsoft Docs
description: Web uygulamanız tarafından gönderilen arama ve filtreleme ham telemetri.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 2a437555-8043-45ec-937a-225c9bf0066b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: c6a94fd1cebff4aa657ad5293715550161003d21
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294393"
---
# <a name="using-search-in-application-insights"></a>Application Insights'ta aramayı kullanma
Arama özelliğidir [Application Insights](app-insights-overview.md) bulmak ve sayfa görünümleri, özel durumlar gibi tek tek telemetri öğeleri keşfedin veya web istekleri için kullanın. Ve günlük izlemelerini ve kodlanmış olayları görüntüleyebilirsiniz.

(Verilerinizi üzerinden daha karmaşık sorgular için kullanın [Analytics](app-insights-analytics-tour.md).)

## <a name="where-do-you-see-search"></a>Burada arama görüyor musunuz?
### <a name="in-the-azure-portal"></a>Azure portalında
Tanılama arama, uygulamanızın uygulama Öngörüler genel bakış dikey penceresinden açıkça açabilirsiniz:

![Tanılama arama Aç](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)

Bazı grafikler ve kılavuz öğeleri tıklattığınızda da açılır. Bu durumda, kendi filtrelerini seçtiğiniz öğe türüne odaklanmak için önceden ayarlanır. 

Örneğin, genel bakış dikey çubuk grafiği, yanıt süresine göre sınıflandırılmış isteklerinin yoktur. Bu yanıt zaman aralığı içinde tek tek isteklerin listesini görmek için bir performans aralığı üzerinden tıklatın:

![İsteği Performans'ı tıklatın](./media/app-insights-diagnostic-search/07-open-from-filters.png)

Tanılama arama ana gövdesini listesidir telemetri öğelerinin - sunucu istekleri, sayfa görünümleri, kodlanmış özel olaylar ve benzeri. Listenin başında olayları sayıları zamanla gösteren Özet bir grafiktir.

Yeni olaylar almak için Yenile'yi tıklatın.

### <a name="in-visual-studio"></a>Visual Studio’da

Visual Studio'da, ayrıca bir uygulama Insights arama penceresi yok. Hatalarını ayıkladığınız uygulama tarafından oluşturulan telemetri olayları görüntülemek için en kullanışlıdır. Ancak Azure portalında yayımlanmış uygulamanızı toplanan olayları gösterebilir.

Arama penceresi Visual Studio'da açın:

![Visual Studio Application Insights arama açın](./media/app-insights-diagnostic-search/32.png)

Arama penceresi web Portalı'na benzer özelliklere sahiptir:

![Visual Studio Application Insights arama penceresi](./media/app-insights-diagnostic-search/34.png)

Bir isteğin ya da bir sayfa görünümü açtığınızda izleme işlemi sekmesi kullanılabilir. Bir'işlemi ' için tek bir istek veya sayfa görünümünü ile ilişkili olayları dizisidir. Örneğin, bağımlılık çağrıları, özel durumlar, izleme günlükleri ve özel olaylar tek bir işlemin parçası olabilir. İzleme işlemi sekmesi grafik zamanlama ve istek veya sayfa görünümü ile ilgili olarak bu olayların süresini gösterir. 

## <a name="inspect-individual-items"></a>Bireysel öğeleri inceleyin.
Anahtar alanları görmek için herhangi bir telemetri öğesini ve ilişkili öğeleri seçin. Alanlar, tamamını görmek istiyorsanız, tıklayın "...". 

![Yeni iş öğesini, alanları düzenleyin ve ardından Tamam'ı tıklatın.](./media/app-insights-diagnostic-search/10-detail.png)

## <a name="filter-event-types"></a>Filtre olay türleri
Filtre dikey penceresini açın ve görmek istediğiniz olay türlerini seçin. (Daha sonra dikey penceresi açılır Filtreleri geri yüklemek isterseniz Sıfırla'yı tıklatın.)

![Filtre seçin ve telemetri türlerini seçin](./media/app-insights-diagnostic-search/02-filter-req.png)

Olay türleri şunlardır:

* **İzleme** - [tanılama günlükleri](app-insights-asp-net-trace-logs.md) TrackTrace, log4Net, NLog ve System.Diagnostic.Trace çağrıları dahil.
* **İstek** -sayfaları, komut dosyaları, görüntüler, Stil dosyaları ve verileri de dahil olmak üzere sunucu uygulamanız tarafından alınan HTTP isteği. Bu olaylar, istek ve yanıt genel bakış grafiklerinde oluşturmak için kullanılır.
* **Sayfa görünümü** - [Telemetri web istemcisi tarafından gönderilen](app-insights-javascript.md), sayfa görünümü raporları oluşturmak için kullanılan. 
* **Özel olay** - TrackEvent() için çağrıları eklediyseniz [izlemek kullanım](app-insights-api-custom-events-metrics.md), burada bulabilirsiniz.
* **Özel durum** - yakalanmayan [sunucu özel durumları](app-insights-asp-net-exceptions.md)ve TrackException() kullanarak oturum açın.
* **Bağımlılık** - [sunucu uygulamanızı gelen çağrıları](app-insights-asp-net-dependencies.md) çağrılarını REST API'leri veya veritabanları ve AJAX gibi diğer hizmetler için [istemci kodu](app-insights-javascript.md).
* **Kullanılabilirlik** -sonuçlarını [kullanılabilirlik testleri](app-insights-monitor-web-app-availability.md).

## <a name="filter-on-property-values"></a>Özellik değerleri üzerinde filtreleme
Özelliklerinin değerlerini olaylarına filtreleyebilirsiniz. Kullanılabilir özellikler seçtiğiniz olay türlerine bağlıdır. 

Örneğin, belirli yanıt kodu istekleriyle çıkışı seçin. 

![Bir özelliği'ni genişletin ve bir değer seçin](./media/app-insights-diagnostic-search/03-response500.png)

Belirli bir özellik yok değerleri seçme tüm değerleri seçme aynı etkiye sahiptir. Bu, söz konusu özellik üzerinde filtreleme kapalı geçer.

### <a name="narrow-your-search"></a>Aramanızı daraltın
Filtre değerleri sağındaki sayıları kaç yineleme var. geçerli filtrelenmiş kümesinde yer alan Göster dikkat edin. 

Bu örnekte 'Rpt/çalışanlar' '500' hataların çoğu sonuçlarında istek açıktır:

![Bir özelliği'ni genişletin ve bir değer seçin](./media/app-insights-diagnostic-search/04-failingReq.png)




## <a name="find-events-with-the-same-property"></a>Aynı özelliğe sahip olayları Bul
Aynı özellik değerine sahip tüm öğeleri bulun:

![Bir özellik sağ tıklayın](./media/app-insights-diagnostic-search/12-samevalue.png)


## <a name="search-the-data"></a>Veri arama

> [!NOTE]
> Daha karmaşık sorgular yazmak için açık [ **Analytics** ](app-insights-analytics-tour.md) Ara dikey üstünden.
> 

Herhangi bir özellik değerlerini koşulları için arama yapabilirsiniz. Bu, yazdıysanız özellikle yararlıdır [özel olaylar](app-insights-api-custom-events-metrics.md) özellik değerlerine sahip. 

Aralık, daha kısa bir aralığı üzerinden aramaları olarak daha hızlı bir zaman ayarlamak isteyebilirsiniz. 

![Tanılama arama Aç](./media/app-insights-diagnostic-search/appinsights-311search.png)

Tam sözcük, değil alt dizeler arayın. Özel karakterler kapsamak için tırnak işaretleri kullanın.

| dize | olan *değil* tarafından bulunamadı | Ancak bu Bul |
| --- | --- | --- |
| HomeController.About |giriş sayfası<br/>Denetleyici<br/>Çıkışı | homecontroller<br/>hakkında<br/>"homecontroller.about"|
|Amerika Birleşik Devletleri|UNI<br/>düzenlenmiş|Birleşik<br/>durumları<br/>VE Birleşik Devletleri<br/>"ABD"

Kullanabileceğiniz arama ifadeleri şunlardır:

| Örnek sorgu | Etki |
| --- | --- |
| `apple` |Tüm olayları alanları "apple" sözcüğünü eklemediğinizden zaman aralığı içinde Bul |
| `apple AND banana` |Her iki sözcüklerini olayları bulun. Capital "ve", değil kullan "ve". |
| `apple OR banana`<br/>`apple banana` |Her iki word içeren olayları bulun. "", Kullanıp "veya".<br/>Kısa formu. |
| `apple NOT banana` |Bir sözcük, ancak diğer içeren olayları bulun. |



## <a name="sampling"></a>Örnekleme
Uygulamanız çok sayıda telemetri oluşturuyorsa (ve ASP.NET SDK sürüm 2.0.0-beta3 kullanıyorsanız veya sonrası), Uyarlamalı örnekleme modülü olayların yalnızca bir temsilci fraksiyonunu göndererek portala gönderilen birimi otomatik olarak azaltır. Ancak, aynı istekle ilişkili olaylar seçili veya böylece ilgili olaylar arasında gezinebilirsiniz grup olarak işaretli. 

[Örnekleme hakkında bilgi edinin](app-insights-sampling.md).



## <a name="create-work-item"></a>İş öğesi oluştur
Tüm telemetri öğesinden ayrıntılarla GitHub veya Visual Studio Team Services içinde bir hata oluşturabilirsiniz. 

![Yeni iş öğesini, alanları düzenleyin ve ardından Tamam'ı tıklatın.](./media/app-insights-diagnostic-search/42.png)

Bunu ilk kez Team Services hesabınızın ve proje bağlantısını yapılandırmak için istenir.

![Team Services sunucunuzun ve proje adı URL'sini doldurun ve Yetkilendir'i tıklatın](./media/app-insights-diagnostic-search/41.png)

(De bağlantı iş öğelerini dikey penceresinde yapılandırabilirsiniz.)

## <a name="save-your-search"></a>Aramayı kaydetme
İstediğiniz tüm filtreleri belirledikten sonra arama sık kullanılan olarak kaydedebilirsiniz. Bir kurumsal hesap çalışıyorsanız, diğer ekip üyeleri ile paylaşmak seçebilirsiniz.

![Sık Kullanılanlar'a tıklayın, kümesinin adı ve Kaydet'e tıklayın.](./media/app-insights-diagnostic-search/08-favorite-save.png)

Arama yeniden görmek için **genel bakış dikey penceresine gidin** ve Sık Kullanılanlar açın:

![Sık Kullanılanlar döşeme](./media/app-insights-diagnostic-search/09-favorite-get.png)

Göreli zaman aralığı ile kaydettiyseniz, en son verileri yeniden açılan dikey sahiptir. Mutlak zaman aralığıyla kaydettiyseniz, her zaman aynı veri bakın. (Sık kullanılan kaydetmek istediğinizde 'Göreli' kullanılabilir değilse, başlığında zaman aralığı'nı tıklatın ve özel bir aralık olmadığından bir zaman aralığı ayarlayın.)

## <a name="send-more-telemetry-to-application-insights"></a>Daha fazla telemetri Application Insights'a gönderme
Application Insights SDK'sı tarafından gönderilen giden kutusu telemetriyi ek olarak, şunları yapabilirsiniz:

* İçinde sık kullandığınız günlük çerçeveden günlük izlemelerini yakalama [.NET](app-insights-asp-net-trace-logs.md) veya [Java](app-insights-java-trace-logs.md). , Günlük izlemelerini arayın ve sayfa görünümleri, özel durumlar ve diğer olaylarla ilişkilendirmek anlamına gelir. 
* [Kod yazma](app-insights-api-custom-events-metrics.md) özel olaylar, sayfa görünümleri ve özel durumları göndermek için. 

[Günlükleri ve özel telemetri Application Insights'a gönderme öğrenin](app-insights-asp-net-trace-logs.md).

## <a name="questions"></a>SORU- CEVAP
### <a name="limits"></a>Ne kadar veri tutulur?

Bkz: [sınırları Özet](app-insights-pricing.md#limits-summary).

### <a name="how-can-i-see-post-data-in-my-server-requests"></a>My server istekleri gönderme verisi nasıl görebilirim?
Gönderme verisi otomatik olarak oturum yoktur, ancak kullanabileceğiniz [TrackTrace ya da günlük çağrıları](app-insights-asp-net-trace-logs.md). Gönderme verisi ileti parametresinde yerleştirin. Özellikleri filtreleyebilirsiniz aynı şekilde ileti üzerinde filtre uygulayamazsınız, ancak boyut sınırını uzun.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="add"></a>Sonraki adımlar
* [Karmaşık sorgular analizleri yazma](app-insights-analytics-tour.md)
* [Günlükleri ve özel telemetri Application Insights'a gönderme](app-insights-asp-net-trace-logs.md)
* [Kullanılabilirlik ve yanıt hızını testleri ayarlama](app-insights-monitor-web-app-availability.md)
* [Sorun giderme](app-insights-troubleshoot-faq.md)
