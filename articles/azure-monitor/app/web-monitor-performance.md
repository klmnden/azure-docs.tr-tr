---
title: Uygulamanızın durumunu ve kullanımını Application Insights ile izleme
description: Application Insights ile çalışmaya başlayın. Kullanımını, kullanılabilirliğini ve şirket içi veya Microsoft Azure uygulama performansını analiz edin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/10/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: d7b8037f50fc4877fe233925f3e922648169f73b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60373156"
---
# <a name="monitor-performance-in-web-applications"></a>Web uygulamalarının performansını izleme


Uygulamanızın iyi durumda olduğundan emin olun ve hızlı bir şekilde tüm hataları hakkında bilgi edinin. [Application Insights] [ start] herhangi bir performans sorunlarını ve özel durumlar hakkında bilgi ve Yardım bulmak ve nedenlerini tanılayın.

Application Insights, hem Java hem de ASP.NET web uygulamaları ve Hizmetleri, WCF hizmetleri izleyebilirsiniz. Şirket içinde barındırılabilir, Microsoft Azure Web siteleri veya sanal makinelerde olabilirler. 

İstemci tarafında, Application Insights web sayfaları ve çok çeşitli cihazlar iOS, Android ve Windows Store uygulamaları dahil olmak üzere alınan telemetri alabilir.

## <a name="setup"></a>Performans izlemeyi ayarlama
(Diğer bir deyişle, Applicationınsights.config yoksa), projenize Application ınsights'ı henüz eklemediniz varsa, başlamak için aşağıdaki yöntemlerden biriyle seçin:

* [ASP.NET web uygulamaları](../../azure-monitor/app/asp-net.md)
  * [Özel durum izleme Ekle](../../azure-monitor/app/asp-net-exceptions.md)
  * [Bağımlılık izleme ekleme](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [Java EE web uygulamaları](../../azure-monitor/app/java-get-started.md)
  * [Bağımlılık izleme ekleme](../../azure-monitor/app/java-agent.md)

## <a name="view"></a>Performans ölçümleri keşfetme
İçinde [Azure portalında](https://portal.azure.com), uygulamanız için ayarladığınız Application Insights kaynağına göz atın. Genel Bakış dikey temel performans verilerini gösterir:

Daha fazla ayrıntı ve daha uzun bir süre için sonuçları görmek için herhangi bir grafiğe tıklayın. Örneğin, istekleri kutucuğa tıklayın ve ardından bir zaman aralığı seçin:

![Daha fazla veri için tıklayın ve bir zaman aralığı seçin](./media/web-monitor-performance/appinsights-48metrics.png)

Bir grafik hangi ölçümleri, görüntüler, veya yeni bir grafik ekleyin ve kendi ölçümleri'ni seçin'i seçmek için tıklayın:

![Ölçümler seçmek için bir grafiğe tıklayın](./media/web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> **Tüm ölçümleri'seçeneğinin işaretini kaldırın** kullanılabilir olan tam seçimi görmek için. Ölçümler gruplara ayrılır; herhangi bir grubun üyesi seçildiğinde, yalnızca diğer grubun üyesi görünür.

## <a name="metrics"></a>Tüm ortalama yaptığı? Performans kutucukları ve raporlar
Çeşitli performans ölçümlerini alabileceğiniz vardır. Varsayılan uygulama dikey penceresinde görünen bu başlayalım.

### <a name="requests"></a>İstekler
Belirli bir süre içinde alınan HTTP isteklerinin sayısı. Bu yük uygulamanızın nasıl davrandığını görmek için diğer raporları sonuçları karşılaştırmak değişir.

HTTP isteklerini sayfaları, veriler ve resimler için tüm GET veya POST istekleri içerir.

Sayıları belirli URL'lerini almak için kutucuğa tıklayın.

### <a name="average-response-time"></a>Ortalama yanıt süresi
Uygulamanız ve döndürülen yanıt girerek web isteği arasındaki süreyi ölçer.

Hareketli Ortalama noktalarını gösterir. Çok sayıda istek varsa, olabilir bazı belirgin bir tepe olmadan ortalamasından sapma veya grafikte DIP.

Olağan dışı en yüksek sayılar arayın. Genel olarak, yanıt süresi ile isteklerinde bir artış olduğunda artmaya bekler. Roslyn'in orantısız ise, uygulamanızın CPU veya kullandığı hizmet kapasitesi gibi bir kaynak limitini aştıktan.

Belirli URL kez almak için kutucuğa tıklayın.

![](./media/web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a>En yavaş istekler
![](./media/web-monitor-performance/appinsights-44slowest.png)

İsteklerin performans ayarlaması gerekebilir gösterir.

### <a name="failed-requests"></a>Başarısız istekler
![](./media/web-monitor-performance/appinsights-46failed.png)

Yakalanmayan Özel durum oluşturdu isteklerinin sayısı.

Belirli hataları ayrıntılarını görmek için kutucuğa tıklayın ve ayrıntılarını görmek için ayrı bir istek'i seçin. 

Hatalar için temsili bir örnek için bireysel İnceleme korunur.

### <a name="other-metrics"></a>Diğer ölçümleri
Ne kadar diğer ölçümleri görüntülemek, bir grafiğe tıklayın ve ardından kullanılabilir tam görmek için tüm ölçümleri seçimini ayarlayın. (İ), her bir ölçüm 's tanımını görmek için tıklayın.

![Tüm kümesini görmek için tüm ölçümlerin seçimini kaldırın](./media/web-monitor-performance/appinsights-62allchoices.png)

Herhangi bir ölçüm seçmeyi aynı grafikte yer alamaz diğerleri devre dışı bırakır.

## <a name="set-alerts"></a>Uyarı ayarlama
Ölçümlerden herhangi birinin alışılmadık değerlerin e-postayla gönderilecek bir uyarı ekleyin. Hesap Yöneticileri veya belirli bir e-posta adreslerine e-posta göndermek ya da seçebilirsiniz.

![](./media/web-monitor-performance/appinsights-413setMetricAlert.png)

Kaynak önce diğer özelliklerini ayarlayın. Web testi kaynakları üzerinde performans veya kullanım ölçüm uyarıları ayarlamak istiyorsanız seçmeyin.

Eşik değerini girmeniz istenir birimleri Not dikkat edin.

*Uyarı Ekle düğmesinin göremiyorum.* -Bu olan bir grubu salt okunur erişim için kullandığınız hesap? Hesap yöneticinize başvurun.

## <a name="diagnosis"></a>Sorunları tanılama
Bulma ve performans sorunlarını tanılamak için bazı ipuçları şunlardır:

* Ayarlanan [web testleri] [ availability] , web site aşağıya inerse veya yanlış veya yavaş yanıt verme durumunda uyarı almak istiyorsanız. 
* Diğer ölçümlerle yüklemek için ilgili hataları veya yavaş yanıt olmadığını görmek için istek sayısı ile karşılaştırın.
* [Ekle ve arama izleme deyimleri] [ diagnostic] kodunuzdaki sorunları belirlemenize yardımcı olmak için.
* Web uygulamanızda işlemle izlemek [Canlı ölçümleri Stream][livestream].
* .NET uygulamanızı durumu yakalama [Snapshot Debugger][snapshot].

## <a name="find-and-fix-performance-bottlenecks-with-performance-investigation-experience"></a>Bulun ve performans araştırma deneyimi ile performans sorunlarını giderin

Performans araştırma deneyimi, Web uygulamanız yavaş gerçekleştirme işlemleri gözden geçirmek için kullanabilirsiniz. Hızlı bir şekilde yavaş bir işlem seçin ve kullanmak [Profiler](../../azure-monitor/app/profiler.md) kök kod aşağı yavaş işlemleri neden olur. Seçilen işlem için ne deneyimi müşterileriniz için bozuk bir bakışta hızla değerlendirebilirsiniz gösterilen yeni süre dağılımı kullanma. Yavaş her işlem için kullanıcı etkileşimi kaç etkilendiğini görebilirsiniz. Aşağıdaki örnekte, GET Customers/Details işlemi deneyimini birine daha yakından bakalım biz karar verdiniz. Süre dağılımı içinde üç ani artışlar olduğunu görebiliriz. En soldaki depo yaklaşık 400 ms olan ve hızlı yanıt veren deneyiminin mükemmel temsil eder. Orta depo, 1.2 s ve temsil kendinizi kurtarın deneyimi. Son olarak, 3.6 s sahibiz müşterilerimizin memnun bırakın yol açabilen 99. yüzdebirlik deneyimi temsil eden başka bir küçük ani artış gerçekleşti. Bu deneyimi on kez harika bir deneyim için aynı işlemi yavaştır. 

![GET Customers/Details üç süresi ani](./media/web-monitor-performance/PerformanceTriageViewZoomedDistribution.png)

Bu işlem için kullanıcı deneyimi daha iyi bir fikir edinmek için daha büyük bir zaman aralığı seçebilirsiniz. Biz sonra da sürede işlemi yavaş olduğu belirli bir zaman penceresinde daraltabilirsiniz. Aşağıdaki örnekte, biz olan varsayılan zaman aralığı 7 gün için zaman aralığı ve sonra 9:47 için 12:47 zaman penceresine Sal 12 ve Çar 13 arasındaki uzaklaştırılacağını 24 saat değiştirdikten. Süre dağılımı hem örnek ve profil oluşturucu izlemeleri sayısı sağ tarafta güncelleştirildi.

![GET Customers/Details 7 günde üç süresi ani bir zaman penceresi ile aralığı](./media/web-monitor-performance/PerformanceTriageView7DaysZoomedTrend.png)

İçinde yavaş deneyimler daraltmak için biz sonraki 99. yüzdebirlik dilimde 95 arasında kalan süreleri yakınlaştırmak. Bunlar yavaş kullanıcı etkileşimi sonucu oluşan %4 temsil eder.

![GET Customers/Details 7 günde üç süresi ani bir zaman penceresi ile aralığı](./media/web-monitor-performance/PerformanceTriageView7DaysZoomedTrendZoomed95th99th.png)

Artık temsili örnekler ya da göz örnekleri düğmesine veya temsili profiler izlemeleri Profiler izlemeleri düğmesine tıklayarak tıklayarak gerçekleştirebiliriz. Bu örnekte, GET Customers/Details için zaman penceresi ve aralığı süresi ilgi toplanmış dört izlemeleri vardır.

Kodunuzda bazen sorun olmaz, ancak bir bağımlılık yerine kodunuzu çağırır. Bu tür yavaş bağımlılıkları incelemek için performans değerlendirmesi görünümünün bağımlılıkları sekmesine geçiş yapabilirsiniz. Varsayılan olarak performans görünümünü popüler ortalamalar, ancak gerçekten bakmak istediklerinizi 95. yüzdebirlik (veya 99th olgun bir hizmet izlemekte olduğunuz durumunda). Aşağıdaki örnekte biz burada PUT fabrikamaccount diyoruz yavaş Azure BLOB bağımlı duruldu. Yavaş çağrıları aynı bağımlılık yaklaşık 120 ms kümeleme üç kat daha yavaştır, çalışırken iyi küme yaklaşık 40 ms karşılaşır. Birçok ilgili işlemin fark edilir derecede yavaşlamasına neden eklemek için bu çağrılardan almaz. Temsili bir örnek ve işlemleri sekmesini gibi profiler izlemeleri ayrıntılarına ulaşabilirsiniz.

![GET Customers/Details 7 günde üç süresi ani bir zaman penceresi ile aralığı](./media/web-monitor-performance/SlowDependencies95thTrend.png)

Performans araştırma deneyimi yanı sıra ilgili içgörüler odaklanmak için karar örnek kümesini gösterir. Kullanılabilir içgörüler edinebileceğiniz aramak için en iyi geçiş için 30 günlük bir zaman aralığı seçin ve sonra tüm işlemler için geçen ay boyunca öngörüleri görmek için genel yoludur.

![GET Customers/Details 7 günde üç süresi ani bir zaman penceresi ile aralığı](./media/web-monitor-performance/Performance30DayOveralllnsights.png)


## <a name="next"></a>Sonraki adımlar
[Web testleri] [ availability] -sahip uygulamanıza düzenli aralıklarla dünyanın dört bir yanındaki gönderilen web istekleri.

[Yakalama ve arama tanılama izlemeleri] [ diagnostic] - izleme çağrıları ekleyin ve sorunları belirlemenize sonuçlarını taranması.

[Kullanımı izleme] [ usage] -insanların uygulamanızı nasıl kullandığını keşfedin.

[Sorun giderme] [ qna] - ve soru- cevap



<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[greenbrown]: ../../azure-monitor/app/asp-net.md
[qna]: ../../azure-monitor/app/troubleshoot-faq.md
[redfield]: ../../azure-monitor/app/monitor-performance-live-website-now.md
[start]: ../../azure-monitor/app/app-insights-overview.md
[usage]: usage-overview.md
[livestream]: ../../azure-monitor/app/live-stream.md
[snapshot]: ../../azure-monitor/app/snapshot-debugger.md



