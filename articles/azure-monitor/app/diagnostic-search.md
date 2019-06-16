---
title: Azure Application Insights arama'yı kullanarak | Microsoft Docs
description: Web uygulamanız tarafından gönderilen arama ve filtre ham telemetri.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 2a437555-8043-45ec-937a-225c9bf0066b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: mbullwin
ms.openlocfilehash: dfbaabd3d27804909334a7a370bcc89115e625c4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60900265"
---
# <a name="using-search-in-application-insights"></a>Uygulama anlayışları'nda arama kullanma
Arama özelliği, [Application Insights](../../azure-monitor/app/app-insights-overview.md) bulmak ve sayfa görüntülemeleri, özel durumlar gibi tek bir telemetri öğelerini keşfedin veya web istekleri için kullanın. Ve günlük izlemeleri ve kodlanmış olayları görüntüleyebilir.

(Verileriniz üzerinde daha karmaşık sorgular için [Analytics](../../azure-monitor/log-query/get-started-portal.md).)

## <a name="where-do-you-see-search"></a>Burada arama görüyor musunuz?

### <a name="in-the-azure-portal"></a>Azure portalında

Tanılama araması uygulamanızı Application Insights'a genel bakış dikey penceresinden açıkça açabilirsiniz:

![Tanılama Aramayı Aç](./media/diagnostic-search/001.png)

![Tanılama araması grafiklerin ekran görüntüsü](./media/diagnostic-search/002.png)

Tanılama araması ana gövdesi bir listedir telemetri öğelerinin - sunucu istekleri, görünümler, kodlanmış özel olaylar ve benzeri sayfa. Listenin en üstünde zaman içinde olay sayısını gösteren Özet bir grafiktir.

Yeni olaylar almak için Yenile'ye tıklayın.

### <a name="in-visual-studio"></a>Visual Studio’da

Visual Studio'da da Application Insights arama penceresi yok. Ayıkladığınız uygulama tarafından üretilen telemetri olayları görüntülemek için en kullanışlıdır. Ancak, Azure portalında yayımlanmış uygulamanızdan toplanan olayları da gösterebilirsiniz.

Arama penceresi Visual Studio'da açın:

![Visual Studio Application Insights arama açın](./media/diagnostic-search/32.png)

Arama penceresi, web Portalı'na benzer özelliklere sahiptir:

![Visual Studio Application Insights arama penceresi](./media/diagnostic-search/34.png)

İşlemi İzle sekmesi, bir istek veya sayfa görünümü açtığınızda kullanılabilir. Bir'işlemi ' olduğu için tek bir istek veya sayfa görüntüleme ile ilişkili bir olay dizisi. Örneğin, bağımlılık çağrıları, özel durumlar, izleme günlükleri ve özel olaylar tek bir işlemin parçası olabilir. İşlemi İzle sekmesi zamanlama ve istek veya sayfa görüntüleme ile ilgili olarak bu olayların süresi grafik şeklinde gösterir. 

## <a name="inspect-individual-items"></a>Bireysel öğeleri inceleyin

Anahtar alanları görmek için herhangi bir telemetri öğesinin ve ilgili öğeleri seçin.

![Bir bireysel bağımlılığı isteğinin ekran görüntüsü](./media/diagnostic-search/003.png)

Bu uçtan uca işlem Ayrıntıları görünümü başlatılır:

![Uçtan uca işlem ayrıntıları görünümünün ekran görüntüsü.](./media/diagnostic-search/004.png)

## <a name="filter-event-types"></a>Olay türlerini Filtrele
Filtre dikey penceresini açın ve görmek istediğiniz olay türlerini seçin. (Daha sonra dikey pencere açılır filtreler geri yüklemek istiyorsanız Sıfırla'ya tıklayın.)

![Filtreyi seçin ve telemetri türlerini seçin](./media/diagnostic-search/02-filter-req.png)

Olay türleri şunlardır:

* **İzleme** - [tanılama günlükleri](../../azure-monitor/app/asp-net-trace-logs.md) TrackTrace, log4Net, NLog ve System.Diagnostic.Trace çağrılar dahil olmak üzere.
* **İstek** -sayfaları, betikleri, görüntüler, Stil dosyaları ve verileri dahil olmak üzere sunucu uygulamanız tarafından alınan HTTP istekleri. Bu olaylar, istek ve yanıt genel bakış grafiklerinde oluşturmak için kullanılır.
* **Sayfa görünümü** - [Telemetri web istemcisi tarafından gönderilen](../../azure-monitor/app/javascript.md)sayfa görünümü raporları oluşturmak için kullanılır. 
* **Özel olay** - için TrackEvent() çağrıları eklediyseniz [kullanım izleme](../../azure-monitor/app/api-custom-events-metrics.md), burada bulabilirsiniz.
* **Özel durum** - yakalanmayan [sunucu özel durumları](../../azure-monitor/app/asp-net-exceptions.md)hem de TrackException() kullanarak oturum açın.
* **Bağımlılık** - [sunucu uygulamanızı çağrılarından](../../azure-monitor/app/asp-net-dependencies.md) REST API'leri veya veritabanları ve AJAX gibi diğer hizmetlere çağırır, [istemci kodu](../../azure-monitor/app/javascript.md).
* **Kullanılabilirlik** -sonuçlarını [kullanılabilirlik testleri](../../azure-monitor/app/monitor-web-app-availability.md).

## <a name="filter-on-property-values"></a>Özellik değerlerine göre filtreleme
Olayları değerlerine özelliklerine göre filtreleyebilirsiniz. Kullanılabilir özellikler, seçtiğiniz olay türlerine bağlıdır. 

Örneğin, istekleri belirli bir yanıtı koduyla çıkış seçin. 

![Bir özelliği'ni genişletin ve bir değer seçin](./media/diagnostic-search/03-response500.png)

Belirli bir özellik yok değerlerini seçerek tüm değerleri seçmeyi aynı etkiye sahiptir. Bu, söz konusu özellik üzerinde filtreleme devre dışı geçer.

### <a name="narrow-your-search"></a>Aramanızı daraltın
Filtre değerleri sağındaki sayıları kaç örnekleri var. geçerli filtrelenmiş kümededir Göster dikkat edin. 

Bu örnekte, 'Rpt/çalışanlar' Sonuçları '500' hataların çoğu istek açıktır:

![Bir özelliği'ni genişletin ve bir değer seçin](./media/diagnostic-search/04-failingReq.png)

## <a name="find-events-with-the-same-property"></a>Aynı özellik ile olayları bulun
Aynı özellik değerine sahip tüm öğeleri bulun:

![Bir özelliğe sağ tıklayın](./media/diagnostic-search/12-samevalue.png)

## <a name="search-the-data"></a>Veri arama

> [!NOTE]
> Daha karmaşık sorgular yazmak için açık [ **Analytics** ](../../azure-monitor/log-query/get-started-portal.md) arama dikey pencerenin üst.
> 

Herhangi bir özellik değeri terimlerini arayabilirsiniz. Bu yazdığınız, özellikle yararlı olacaktır [özel olaylar](../../azure-monitor/app/api-custom-events-metrics.md) özellik değerlerine sahip. 

Aralık, daha kısa bir aralık içinde aramalar olarak daha hızlı bir zaman ayarlamak isteyebilirsiniz. 

![Tanılama Aramayı Aç](./media/diagnostic-search/appinsights-311search.png)

Arama için tam sözcük, alt dizeler değil. Özel karakterleri için tırnak işaretleri kullanın.

| string | olan *değil* tarafından bulunamadı | ancak bunlar Bul |
| --- | --- | --- |
| HomeController.About |Giriş<br/>Denetleyici<br/>Çıkış | homecontroller<br/>hakkında<br/>"homecontroller.about"|
|Amerika Birleşik Devletleri|UNI<br/>ted|Birleşik<br/>durumları<br/>VE Birleşik Devletleri<br/>"ABD"

Kullanabileceğiniz arama ifadeleri şunlardır:

| Örnek sorgu | Etki |
| --- | --- |
| `apple` |Alanları "apple" sözcüğünü içeren bir zaman aralığında tüm etkinlikler bulun |
| `apple AND banana` <br/>`apple banana` |Her iki sözcükleri içeren etkinlikler bulun. Capital "ve" değil kullan "ve". <br/>Kısa biçim. |
| `apple OR banana` |Her iki word içeren etkinlikler bulun. "Veya", yok "veya". |
| `apple NOT banana` |Bir sözcük, ancak diğer içeren etkinlikler bulun. |

## <a name="sampling"></a>Örnekleme
Uygulamanız çok sayıda telemetri oluşturuyorsa (ve ASP.NET SDK sürüm 2.0.0-Beta3 veya daha sonra), Uyarlamalı örnekleme modülü olayların yalnızca bir temsilci fraksiyonunu göndererek portala gönderilen hacmi otomatik olarak azaltır. Ancak aynı istekle ilgili olayları seçtikten veya bir grup olarak seçili, böylece ilgili olaylar arasında gezinebilirsiniz. 

[Örnekleme hakkında bilgi edinin](../../azure-monitor/app/sampling.md).

## <a name="create-work-item"></a>İş öğesi oluşturma
Tüm telemetri öğesinin Ayrıntıları ile GitHub ya da Azure DevOps bir hata oluşturabilirsiniz. 

![Yeni iş öğesini, alanları düzenleyin ve sonra Tamam'a tıklayın.](./media/diagnostic-search/42.png)

Azure DevOps kuruluşunuz ve proje bağlantısını yapılandırmak için bunu ilk kez istenir.

![Azure DevOps hizmetlerinizi ve proje adı URL'sini girin ve Yetkilendir'e tıklayın](./media/diagnostic-search/41.png)

(Ayrıca bağlantı iş öğeleri dikey penceresinde yapılandırabilirsiniz.)

## <a name="send-more-telemetry-to-application-insights"></a>Daha fazla telemetri Application Insights'a gönderme
Application Insights SDK'sı tarafından gönderilen kullanıma hazır telemetriyi ek olarak, şunları yapabilirsiniz:

* İçinde sık kullandığınız günlük çerçeveden günlük izlemelerini yakalama [.NET](../../azure-monitor/app/asp-net-trace-logs.md) veya [Java](../../azure-monitor/app/java-trace-logs.md). Bunun anlamı, günlük izlemelerini aramak ve sayfa görüntülemeleri, özel durumlar ve diğer olaylarla ilişkilendirin. 
* [Kod yazma](../../azure-monitor/app/api-custom-events-metrics.md) özel olaylar, sayfa görüntülemeleri ve özel durumlar göndermek için. 

[Günlükleri ve özel telemetri, Application Insights'a gönderme öğrenin](../../azure-monitor/app/asp-net-trace-logs.md).

## <a name="questions"></a>SORU- CEVAP
### <a name="limits"></a>Ne kadar veri tutulur?

Bkz: [sınırları özeti](../../azure-monitor/app/pricing.md#limits-summary).

### <a name="how-can-i-see-post-data-in-my-server-requests"></a>Gönderme verisi sunucu isteklerim nasıl görebilirim?
Gönderme verisi otomatik olarak oturum yok, ancak kullanabileceğiniz [TrackTrace ya da günlük aramaları](../../azure-monitor/app/asp-net-trace-logs.md). Gönderme verisi ileti parametreyi yerleştirin. İletide, özelliklerde filtre aynı şekilde filtre uygulanamıyor, ancak boyut sınırını uzundur.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="add"></a>Sonraki adımlar
* [Karmaşık sorgular Analytics'te yazma](../../azure-monitor/log-query/get-started-portal.md)
* [Günlükleri ve özel telemetri, Application Insights'a gönderme](../../azure-monitor/app/asp-net-trace-logs.md)
* [Kullanılabilirlik ve yanıt hızını testleri ayarlama](../../azure-monitor/app/monitor-web-app-availability.md)
* [Sorun giderme](../../azure-monitor/app/troubleshoot-faq.md)
