---
title: "Uygulamanızın sistem durumu ve Application Insights ile kullanım izleme"
description: "Application Insights ile çalışmaya başlayın. Kullanım, kullanılabilirlik ve şirket içi veya Microsoft Azure uygulamalarının performansını analiz edin."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: mbullwin
ms.openlocfilehash: 32000f5a85c84913aa820df00f1bb7f877bf037f
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="monitor-performance-in-web-applications"></a>Web uygulamalarının performansını izleme


Uygulamanızın düzgün çalışıp emin olun ve hatalar hakkında hızlı bir şekilde öğrenin. [Application Insights] [ start] herhangi bir performans sorunlarını ve özel durumlar hakkında söyleyin ve bulmak ve kök nedeni tanılamanıza yardımcı.

Application Insights, Java ve ASP.NET web uygulamaları ve Hizmetleri, WCF hizmetleri izleyebilirsiniz. Bunlar barındırılan şirket içi, sanal makinelerde ya da Microsoft Azure Web siteleri farklı olabilir. 

İstemci tarafında, Application Insights telemetri web sayfalarını ve çok çeşitli cihazlar iOS, Android ve Windows mağazası uygulamaları dahil olmak üzere alabilir.

>[!Note]
> Size yeni bir deneyim bulma yavaş sayfaları web uygulamanızda gerçekleştirmek için kullanılabilir yaptınız. Erişimi yoksa, Önizleme seçenekleri ile yapılandırarak etkinleştirmek [Önizleme dikey](app-insights-previews.md). Bu yeni deneyim okuyun [bulun ve etkileşimli performans araştırma ile performans sorunları giderin](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).

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

Noktaları bir hareketli ortalama gösterir. Çok sayıda isteği varsa, olabilir olmadan belirgin bir en yüksek ortalama gelen sapma veya grafikte DIP bazı.

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

>[!Note]
> Etkileşimli bir tam ekran deneyimi için Application Insights performans araştırma geçiş sürecinde duyuyoruz. Aşağıdaki belgeler yeni deneyimi ilk kapsayan ve hala geçiş kullanılabilir olmaya devam ederken, erişiminiz olması durumunda önceki deneyimi gözden geçirir.

## <a name="find-and-fix-performance-bottlenecks-with-an-interactive-full-screen-performance-investigation"></a>Bulun ve etkileşimli tam ekran performans araştırma ile performans sorunları giderin

Yeni Application Insights etkileşimli performans araştırma yavaş gerçekleştirme işlemleri kullanarak Web uygulamanızda gözden geçirmek için kullanabilirsiniz. Hızlı bir şekilde belirli bir yavaş işlem seçin ve kullanmak [profil oluşturucu](app-insights-profiler.md) kök kodunu aşağıya doğru yavaş işlemleri neden olur. Seçili işlem için bir bakışta ne kadar deneyimi müşterileriniz için bozuk hızlı bir şekilde değerlendirmek gösterilen yeni süre dağıtım kullanıyor. Aslında, yavaş her işlem için kullanıcı etkileşimi kaç etkilendiğini görebilirsiniz. Aşağıdaki örnekte, biz deneyimi müşteriler/ayrıntıları alma işlemi için daha yakından bakmak karar verdiniz. Süre dağıtımlarında üç ani olduğunu görebilirsiniz. Soldaki depo yaklaşık 400ms olduğu ve harika esnek deneyim temsil eder. Orta depo yaklaşık 1.2s olduğu ve kendinizi kurtarın deneyimi temsil eder. Son olarak 3.6s memnun bırakmayı müşterilerimizin neden büyük olasılıkla 99 yüzdebirlik deneyimi temsil eden başka bir küçük depo sahibiz. Bu deneyim on kez aynı işlem için harika deneyimi daha yavaştır. 

![GET müşteriler/Ayrıntılar üç süresi ani](./media/app-insights-web-monitor-performance/PerformanceTriageViewZoomedDistribution.png)

Bu işlem için kullanıcı deneyimi daha iyi bir fikir edinmek için daha büyük bir zaman aralığı seçebilirsiniz. Biz sonra da zamanında işlemi özellikle yavaş olduğu belirli bir zaman penceresinin üzerinde daraltabilirsiniz. Aşağıdaki örnekte varsayılandan Sal 12 ve Çar 13 arasındaki 9:47 ila 12:47 zaman penceresine uzaklaştırılacağını ve aralığı zaman zaman aralığı için 7 gün 24 saat yaptık. Sağ tarafta süresi dağıtım ve örnek ve profil oluşturucu izlemelerini sayısı güncelleştirildi unutmayın.

![Müşteriler/7 günde üç süresi ani bir zaman penceresi ile aralığı Ayrıntıları Al](./media/app-insights-web-monitor-performance/PerformanceTriageView7DaysZoomedTrend.png)

İçinde yavaş deneyimleri üzerinde daraltmak için biz sonraki 95'inci ile 99 arasında kalan süreleri içine Yakınlaştır. Bu, özellikle yavaş kullanıcı etkileşimlerin %4 temsil eder.

![Müşteriler/7 günde üç süresi ani bir zaman penceresi ile aralığı Ayrıntıları Al](./media/app-insights-web-monitor-performance/PerformanceTriageView7DaysZoomedTrendZoomed95th99th.png)

Şimdi temsilcisi örnekler ya da bakma örnekleri düğmesine veya temsilcisi profil oluşturucu izleme, profil oluşturucu izlemeleri düğmeyi tıklatarak geçebiliriz. Bu örnekte, zaman penceresi ve aralık süresi ilgi içinde alma müşteriler/Ayrıntılar için toplanan dört izlemeleri vardır.

Bazen sorunu kodunuzda olmaz ancak bunun yerine bir bağımlılık olarak çağrıları kodu. Bu tür yavaş bağımlılıkları araştırmak için performans değerlendirme görünümünde bağımlılıkları sekmesine geçiş yapabilirsiniz. Varsayılan olarak performans görünümü oluşturan eğilim ortalamalar, ancak gerçekten bakmak istediğinizi 95 olduğunu unutmayın (veya 99th çok olgun bir hizmeti izleme durumda). Aşağıdaki örnekte biz PUT fabrikamaccount burada diyoruz yavaş Azure BLOB bağımlılığını odaklanmıştır. Aynı bağımlılık yavaş çağrıları yaklaşık 120ms kümeleme üç kat daha yavaştır, ancak 40ms geçici iyi deneyimler küme. Birçok önemli ölçüde yavaşlamaya ilgili işlemi neden eklemek için bu çağrılardan almaz. Temsili örnekleri ve işlemleri sekmesiyle gibi profil oluşturucu izleme, ayrıntılarına geçebilir.

![Müşteriler/7 günde üç süresi ani bir zaman penceresi ile aralığı Ayrıntıları Al](./media/app-insights-web-monitor-performance/SlowDependencies95thTrend.png)

Etkileşimli tam ekran performans araştırmaya yeni olan başka bir gerçekten güçlü Öngörüler ile tümleştirme özelliğidir. Application Insights algılayabilir ve Yardım yanı sıra Öngörüler yanıtlama gerileme olarak yüzey, odaklanmak için karar örnek kümesinde ortak özellikleri tanımlayın. Tüm kullanılabilir Öngörüler aramak için en iyi 30 gün zaman aralığı için geçiş yapın ve ardından tüm işlemler için geçen ay boyunca Öngörüler görmek için genel yoludur.

![Müşteriler/7 günde üç süresi ani bir zaman penceresi ile aralığı Ayrıntıları Al](./media/app-insights-web-monitor-performance/Performance30DayOveralllnsights.png)

Yeni performans değerlendirme görünümünde Application Insights tam anlamıyla Web uygulama kullanıcılarınızın zayıf deneyimleri neden haystack içinde iğne bulmanıza yardımcı olabilir.

## <a name="deprecated-find-and-fix-performance-bottlenecks-with-a-narrow-bladed-legacy-performance-investigation"></a>Kullanım dışı: Bulun ve dar bladed eski performans araştırma ile performans sorunları giderin

Web uygulamanızı genel performansını yavaşlamasının alanlarını bulmak için eski Application Insights bladed performans araştırma kullanabilirsiniz. Yavaşlamadan olan ve kullanan belirli sayfaları bulabilirsiniz [profil oluşturucu](app-insights-profiler.md) bu sorunları kodunu aşağıya doğru kök nedenini izlemek için. 

### <a name="create-a-list-of-slow-performing-pages"></a>Yavaş gerçekleştiren sayfalar listesi oluşturma 

Performans sorunları bulmak için ilk yavaş yanıt veren sayfalar listesini almak için adımdır. Daha fazla araştırmak için olası sayfalarının listesini almak için performans dikey penceresini kullanarak aşağıdaki ekran gösterilir. Bu sayfadan yaklaşık 6:00 PM ve yeniden yaklaşık 10 PM, olduğunu bir konumlanır uygulama yanıt süresini hızlı şekilde görebilirsiniz. GET müşteri/Ayrıntılar işlemi 507.05 milisaniye ortalama yanıt süresi olan bazı uzun süre çalışan işlemleri olduğunu da görebilirsiniz. 

![Uygulama Öngörüler etkileşimli performansı](./media/app-insights-web-monitor-performance/performance1.png)

### <a name="drill-down-on-specific-pages"></a>Belirli sayfalara detaya gitme

Uygulamanızın performansını görüntüsünü oluşturduktan sonra belirli yavaş performanslı işlemleri daha fazla bilgi alabilirsiniz. Aşağıda gösterildiği gibi ayrıntıları görmek için listedeki herhangi bir işlem tıklayın. Grafikten performans bir bağımlılığı dayanarak if görebilirsiniz. Kaç kullanıcının çeşitli yanıt sürelerini yaşadı de görebilirsiniz. 

![Uygulama Öngörüler işlemleri dikey penceresi](./media/app-insights-web-monitor-performance/performance5.png)

### <a name="drill-down-on-a-specific-time-period"></a>Belirli bir döneme detaya gitme

Araştırmak için zamanında bir noktası belirledikten sonra bile daha fazla sınırlandıramazsınız performans konumlanır neden olabilecek belirli işlemler bakma detaya. Belirli bir noktasında zamanında tıkladığınızda aşağıda gösterildiği gibi sayfa ayrıntılarını alın. Aşağıdaki örnekte belirli bir süre sunucu yanıt kodları ve işlem süresi ile birlikte için listelenen işlemleri görebilirsiniz. Ayrıca bu bilgileri Geliştirme ekibiniz göndermek ihtiyacınız varsa, TFS iş öğesi açmak için URL'yi de vardır.

![Uygulama Öngörüler saat dilimi](./media/app-insights-web-monitor-performance/performance2.png)

### <a name="drill-down-on-a-specific-operation"></a>Belirli bir işlemi detaya gitme

Araştırmak için zamanında bir noktası belirledikten sonra bile daha fazla sınırlandıramazsınız performans konumlanır neden olabilecek belirli işlemler bakma detaya. Aşağıda gösterildiği gibi işlem ayrıntılarını görmek için listeden bir işlem tıklayın. Bu örnekte, işlem başarısız oldu ve Application Insights uygulama belirtti. özel durum ayrıntıları sağlamıştır görebilirsiniz. Yeniden, bu dikey pencereden bir TFS iş öğesi kolayca oluşturabilirsiniz.

![Uygulama Öngörüler işlemi dikey penceresi](./media/app-insights-web-monitor-performance/performance3.png)


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



