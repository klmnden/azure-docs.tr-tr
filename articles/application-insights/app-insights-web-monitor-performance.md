---
title: Uygulamanızın sistem durumu ve Application Insights ile kullanım izleme
description: Application Insights ile çalışmaya başlayın. Kullanım, kullanılabilirlik ve şirket içi veya Microsoft Azure uygulamalarının performansını analiz edin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: sdash
ms.openlocfilehash: 02421492528e44ed6a913443a7793235170d4881
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="monitor-performance-in-web-applications"></a>Web uygulamalarının performansını izleme


Uygulamanızın düzgün çalışıp emin olun ve hatalar hakkında hızlı bir şekilde öğrenin. [Application Insights] [ start] herhangi bir performans sorunlarını ve özel durumlar hakkında söyleyin ve bulmak ve kök nedeni tanılamanıza yardımcı.

Application Insights, Java ve ASP.NET web uygulamaları ve Hizmetleri, WCF hizmetleri izleyebilirsiniz. Bunlar barındırılan şirket içi, sanal makinelerde ya da Microsoft Azure Web siteleri farklı olabilir. 

İstemci tarafında, Application Insights telemetri web sayfalarını ve çok çeşitli cihazlar iOS, Android ve Windows mağazası uygulamaları dahil olmak üzere alabilir.

## <a name="setup"></a>Performans izleme işlevini ayarlama
(Diğer bir deyişle, Applicationınsights.config yoksa) projenize Application Insights henüz eklemediniz varsa, bu şekilde başlamak için birini seçin:

* [ASP.NET web uygulamaları](app-insights-asp-net.md)
  * [Özel durum izleme Ekle](app-insights-asp-net-exceptions.md)
  * [Bağımlılık izleme Ekle](app-insights-monitor-performance-live-website-now.md)
* [J2EE web uygulamaları](app-insights-java-get-started.md)
  * [Bağımlılık izleme Ekle](app-insights-java-agent.md)

## <a name="view"></a>Performans ölçümleri keşfederken
İçinde [Azure portalı](https://portal.azure.com), uygulamanız için ayarladığınız Application Insights kaynağı konumuna göz atın. Genel Bakış dikey penceresinde temel performans verilerini gösterir:

Daha fazla ayrıntı görmek ve daha uzun bir süre için sonuçları görmek için herhangi bir grafiğe tıklayın. Örneğin, istekleri kutucuğa tıklayın ve ardından bir zaman aralığı seçin:

![Aracılığıyla daha fazla veri için tıklayın ve bir zaman aralığı seçin](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

Bu görüntüler, veya yeni bir grafik ekleyin ve onun ölçümleri seçin hangi ölçümler seçmek için bir grafik tıklatın:

![Bir grafik ölçümler seçmek için tıklatın](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> **Tüm ölçümleri işaretini** kullanılabilir tam seçimi görmek için. Ölçümleri gruba ayrılır; herhangi bir grubun üyesi seçildiğinde, bu yalnızca diğer üyeler o grubun görünür.

## <a name="metrics"></a>Tüm ortalama yaptığı? Performans kutucukları ve raporlar
Çeşitli performans ölçümleri alma vardır. Varsayılan uygulama dikey penceresinde görünen olanla başlayalım tıklatın.

### <a name="requests"></a>İstekler
Belirli bir süre içinde alınan HTTP isteklerinin sayısı. Bu diğer raporlarda yük uygulamanızı nasıl davranacağını görmek için sonuçları karşılaştırmak değişir.

HTTP istekleri sayfaları, veri ve görüntüleri için tüm GET veya POST istekleri içerir.

Belirli URL sayılarını elde etmek için kutucuğa tıklayın.

### <a name="average-response-time"></a>Ortalama yanıt süresi
Uygulamanız ve döndürülen yanıt girerek web isteği arasındaki süreyi ölçer.

Noktaları bir hareketli ortalama gösterir. Çok sayıda isteği yoksa olabilir olmadan belirgin bir en yüksek ortalama gelen sapma veya grafikte DIP bazı.

Olağan dışı yükselmeleri arayın. Genel olarak, bir artışa istekleri ile artmaya yanıt süresi bekler. Neden orantısız ise, uygulamanızı CPU veya kullandığı bir hizmet kapasitesi gibi bir kaynak sınırına ulaşması.

Belirli URL kez almak için kutucuğa tıklayın.

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a>En yavaş istekler
![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

Hangi isteklerinin performans ayarlaması gerekebilir gösterir.

### <a name="failed-requests"></a>Başarısız istekler
![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

Yakalanmayan bir özel durum oluşturdu isteği sayısı.

Belirli hataları ayrıntılarını görmek için kutucuğa tıklayın ve kendi ayrıntı görmek için ayrı bir istek seçin. 

Hatalar için temsili bir örnek için tek tek denetleme korunur.

### <a name="other-metrics"></a>Diğer ölçümleri
Görmek için görüntülemek, bir grafiğe tıklayın ve kullanılabilir tam görmek için tüm ölçümleri seçimini diğer ölçümleri ayarlayın. (İ) her ölçümü 's tanımı görmek için tıklatın.

![Tüm kümenin görmek için tüm ölçümleri seçimini kaldırın](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)

Herhangi bir ölçümü seçerek aynı grafikte bulunamaz başkalarının devre dışı bırakır.

## <a name="set-alerts"></a>Uyarı ayarlama
Alışılmadık değerlerin herhangi bir ölçümü, e-postayla bildirilmesini bir uyarı ekleyin. Hesap Yöneticileri veya belirli e-posta adresleri e-posta göndermek için ya da seçebilirsiniz.

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

Diğer özellikler önce kaynağı ayarlayın. Performans ya da kullanım ölçümleri uyarıları ayarlamak istiyorsanız, Web testini kaynakları seçmeyin.

Eşik değerini girmeniz istenir birimleri Not dikkat edin.

*Uyarı Ekle düğmesi bakın yok.* -Bu bir gruptur olan salt okunur erişim hesabı? Hesap yöneticinize danışın.

## <a name="diagnosis"></a>Sorunlarını tanılama
Aşağıda, bulma ve performans sorunlarını tanılamak için birkaç ipucu verilmiştir:

* Ayarlanan [web testleri] [ availability] web sitenizi arıza ya da yanlış veya yavaş yanıt uyarılmak üzere. 
* Yüklemek için hataları veya yavaş yanıt ilişkiliyse görmek için diğer ölçümleri isteği sayısıyla karşılaştırın.
* [Ekle ve arama izleme deyimleri] [ diagnostic] sorunları belirlemenize yardımcı olmak için kodunuzda.
* Web uygulamanızda işlemiyle izlemek [ölçümleri bir canlı akışı][livestream].
* .Net uygulamanızla durumunu yakalama [anlık görüntü hata ayıklayıcı][snapshot].

## <a name="find-and-fix-performance-bottlenecks-with-performance-investigation-experience"></a>Bulun ve performans araştırma deneyimiyle performans sorunları giderin

Performans araştırma deneyimi yavaş gerçekleştirme işlemleri kullanarak Web uygulamanızda gözden geçirmek için kullanabilirsiniz. Hızlı bir şekilde belirli bir yavaş işlem seçin ve kullanmak [profil oluşturucu](app-insights-profiler.md) kök kodunu aşağıya doğru yavaş işlemleri neden olur. Seçili işlem için bir bakışta ne kadar deneyimi müşterileriniz için bozuk hızlı bir şekilde değerlendirmek gösterilen yeni süre dağıtım kullanıyor. Yavaş her işlem için kullanıcı etkileşimi kaç etkilendiğini görebilirsiniz. Aşağıdaki örnekte, biz deneyimi müşteriler/ayrıntıları alma işlemi için daha yakından bakmak karar verdiniz. Süre dağıtımlarında üç ani olduğunu görebilirsiniz. Soldaki depo yaklaşık 400 ms olduğu ve harika esnek deneyim temsil eder. Orta depo olduğundan 1.2 s ve temsil kendinizi kurtarın deneyimi. Son olarak, 3.6 s sahibiz memnun bırakmayı müşterilerimizin neden büyük olasılıkla 99 yüzdebirlik deneyimi temsil eden başka bir küçük depo. Bu deneyim on kez aynı işlem için harika deneyimi daha yavaştır. 

![GET müşteriler/Ayrıntılar üç süresi ani](./media/app-insights-web-monitor-performance/PerformanceTriageViewZoomedDistribution.png)

Bu işlem için kullanıcı deneyimi daha iyi bir fikir edinmek için daha büyük bir zaman aralığı seçebilirsiniz. Biz sonra da zamanında işlemi yavaş olduğu belirli bir zaman penceresinin üzerinde daraltabilirsiniz. Aşağıdaki örnekte, varsayılandan Sal 12 ve Çar 13 arasındaki 9:47 ila 12:47 zaman penceresine uzaklaştırılacağını ve aralığı zaman zaman aralığı için 7 gün 24 saat yaptık. Süre dağıtım ve örnek ve profil oluşturucu izlemelerini sayısı sağ tarafta güncelleştirildi.

![Müşteriler/7 günde üç süresi ani bir zaman penceresi ile aralığı Ayrıntıları Al](./media/app-insights-web-monitor-performance/PerformanceTriageView7DaysZoomedTrend.png)

İçinde yavaş deneyimleri üzerinde daraltmak için biz sonraki 95'inci ile 99 arasında kalan süreleri içine Yakınlaştır. Bunlar, %4 yavaş kullanıcı etkileşimlerin temsil eder.

![Müşteriler/7 günde üç süresi ani bir zaman penceresi ile aralığı Ayrıntıları Al](./media/app-insights-web-monitor-performance/PerformanceTriageView7DaysZoomedTrendZoomed95th99th.png)

Şimdi temsilcisi örnekler ya da bakma örnekleri düğmesine veya temsilcisi profil oluşturucu izleme, profil oluşturucu izlemeleri düğmeyi tıklatarak geçebiliriz. Bu örnekte, zaman penceresi ve aralık süresi ilgi içinde alma müşteriler/Ayrıntılar için toplanan dört izlemeleri vardır.

Bazen sorunu kodunuzda olmaz ancak kodunuzu yerine bağımlılık olarak çağırır. Bu tür yavaş bağımlılıkları araştırmak için performans değerlendirme görünümünde bağımlılıkları sekmesine geçiş yapabilirsiniz. Varsayılan olarak performans görünümü oluşturan eğilim ortalamalar, ancak gerçekten bakmak istediğinizi 95 (veya 99th olgun bir hizmeti izleme durumda). Aşağıdaki örnekte biz PUT fabrikamaccount burada diyoruz yavaş Azure BLOB bağımlılığını odaklanmıştır. Aynı bağımlılık yavaş çağrıları yaklaşık 120 ms kümeleme üç kat daha yavaştır, ancak iyi küme yaklaşık 40 ms karşılaşır. Birçok önemli ölçüde yavaşlamaya ilgili işlemi neden eklemek için bu çağrılardan almaz. Temsili örnekleri ve işlemleri sekmesiyle gibi profil oluşturucu izleme, ayrıntılarına geçebilir.

![Müşteriler/7 günde üç süresi ani bir zaman penceresi ile aralığı Ayrıntıları Al](./media/app-insights-web-monitor-performance/SlowDependencies95thTrend.png)

Performans araştırma deneyimi tarafı boyunca ilgili Öngörüler odaklanmak için karar örnek kümesi gösterir. Tüm kullanılabilir Öngörüler aramak için en iyi 30 gün zaman aralığı için geçiş yapın ve ardından tüm işlemler için geçen ay boyunca Öngörüler görmek için genel yoludur.

![Müşteriler/7 günde üç süresi ani bir zaman penceresi ile aralığı Ayrıntıları Al](./media/app-insights-web-monitor-performance/Performance30DayOveralllnsights.png)


## <a name="next"></a>Sonraki adımlar
[Web testleri] [ availability] -sahip web istekleri uygulamanız dünya düzenli aralıklarla gönderilir.

[Yakalama ve arama tanılama izlemeleri] [ diagnostic] - Ekle aramaları izlemek ve sorunları belirlemenize sonuçları SIFT.

[Kullanımı izleme] [ usage] -kişiler, uygulamanızın kullanımını bulun.

[Sorun giderme] [ qna] - ve soru- cevap



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[usage]: app-insights-web-track-usage.md
[livestream]: app-insights-live-stream.md
[snapshot]: app-insights-snapshot-debugger.md



