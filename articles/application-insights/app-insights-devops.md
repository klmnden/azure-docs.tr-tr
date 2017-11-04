---
title: "Web uygulaması performans izleme - Azure Application Insights | Microsoft Docs"
description: "Application Insights devOps döngüsü nasıl uyduğunu"
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 479522a9-ff5c-471e-a405-b8fa221aedb3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: 24e249bb515c509f2fba1f943ac5e23a1ea9965e
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="deep-diagnostics-for-web-apps-and-services-with-application-insights"></a>Application Insights ile ayrıntılı web uygulaması ve hizmet tanılama
## <a name="why-do-i-need-application-insights"></a>Application Insights neden gerekiyor mu?
Application Insights çalışan web uygulamanızı izler. Hataları ve performans sorunları hakkında bildirir ve müşteriler, uygulamanızın kullanımını analiz etmenize yardımcı olur. Birçok platformda (ASP.NET, J2EE, Node.js,...) üzerinde çalışan uygulamalar için çalışır ve Bulut veya şirket içi barındırılır. 

![Web uygulamaları teslim karmaşıklığını yönleri](./media/app-insights-devops/010.png)

Modern uygulama çalışırken izlemek için gereklidir. En önemlisi, müşterilerinizin çoğunu önce hatalarını algılayacak istiyor. Ayrıca bulmak ve değil yıkıcı sırada performans sorunları, düzeltmek istediğiniz, belki de sistemi yavaşlatır veya bazı rahatsızlıktan kullanıcılarınıza neden. Ve hangi kullanıcıların ile yaptıklarını öğrenmek istiyorsanız sistem virüsten koruma yazılımını gerçekleştirirken: en son özelliğini kullanıyor? Bunlar ile başarılı oluyor?

Modern web uygulamaları kesintisiz teslim döngüsünde geliştirilen: yeni bir özellik ya da geliştirme; sürüm kullanıcılar için ne kadar iyi çalıştığı inceleyin; Bu bilgilere dayanan geliştirme, bir sonraki adımını planlayın. Bu döngü önemli bir parçası gözlem aşamasıdır. Application Insights performans ve kullanım için bir web uygulaması izlemek için araçlar sağlar.

En önemli bu işlem tanılama ve tanılama yönüdür. Uygulama başarısız olursa, sonra iş kaybolmasına. Birinci izleme framework'ün bu nedenle güvenilir bir şekilde hatalarını algılayacak, sizi hemen uyarır ve sorunu tanılamak için gereken bilgilerle sunmak için rolüdür. Bu, tam olarak Application Insights ne olur.

### <a name="where-do-bugs-come-from"></a>Hatalar alınacağı yeri?
Web sistemlerinde hataları, genellikle yapılandırma sorunları veya birçok bileşenleri arasındaki hatalı etkileşimler ortaya çıkar. Canlı site olay tackling ilk görevi bu nedenle sorun locus belirlemektir: hangi bileşen veya ilişki nedeni?

Bazı gri artı olanlar bize, bilgisayar programı bir bilgisayarda çalıştırıldığı daha basit bir dönem anımsamasını sağlayabilirsiniz. Geliştiriciler göndermeden önce baştan sona test; ve, gönderilen nadiren bakın veya bu konuda yeniden düşünün. Kullanıcılar ile fazlalık hataları yıllardır put etmesi gerekir. 

Şimdi bu nedenle çok farklı noktalardır. Uygulamanızı çalıştırmak için farklı cihaz sayısız varsa ve her biri üzerinde tam aynı davranışı sağlamak zor olabilir. Uygulamalarını bulutta barındırma hızlı hataların düzeltilmesi, ancak sürekli rekabet ve yeni özellikleri beklentisi sık aralıklarla de anlamına gelir. 

Bu koşullar, hata sayısı kesin bir denetim tutmak için yalnızca otomatik birim testi yoludur. Her teslim üzerindeki her şeyi yeniden el ile test etmek mümkün olacaktır. Birim testi şimdi bir sıradan yapı işleminin parçasıdır. Xamarin Test Cloud gibi araçları birden çok tarayıcı sürümlerinde test otomatikleştirilmiş UI sağlayarak yardımcı olur. Bu test regimes bize en az bir uygulama içinde bulunan hataların oranını tutulabilir umuyoruz izin verir.

Normal web uygulamaları birçok Canlı bileşeni vardır. (Uygulamasında bir tarayıcı veya aygıt) istemci ve web sunucusuna ek olarak, yoktur önemli arka uç işleme olması muhtemeldir. Belki de arka uç bileşenlerinin bir ardışık düzen veya iş parça ok koleksiyonu ' dir. Ve bunların kaç, denetiminde olmayacaktır - dış hizmetler, bağımlı oldukları.

Bu gibi yapılandırmalarında zor ve için test veya, her olası hata modu Canlı sisteminde dışında öngörüyor uneconomical olabilir. 

### <a name="questions-"></a>Soru...
Biz web sistemi geliştirirken size bazı sorular sorun:

* Uygulamam kilitlenen? 
* Tam olarak ne oldu? -Bir istek başarısız olursa nasıl var. taşınmadığını bilmek istiyorum. Bir izleme olaylarının ihtiyacımız...
* Uygulamam yeterince hızlı mı? Ne kadar tipik isteklerini yanıtlamak için sürer?
* Sunucunun yükünü işleyebilir? İstekleri oranını miktarı artar, yanıt süresi sürekli içeriyor mu?
* Nasıl yanıt vereceğini my - REST API'leri, veritabanları ve Uygulamam çağırır diğer bileşenleri bağımlılıklardır. Özellikle, sistem yavaşsa, my bileşenidir veya yavaş yanıt birisinden alıyorum?
* Uygulamam yukarı veya aşağı mi? Bu gelen tüm dünyada görülebilir mi? Durdurur, bilmeniz izin ver...
* Kök nedeni nedir? My bileşeni ya da bir bağımlılık hatası oldu mu? Bir iletişim sorunu mu?
* Kaç kullanıcının etkilenen? Üstesinden gelmek için birden fazla sorun varsa, bu en önemli olduğu?

## <a name="what-is-application-insights"></a>Application Insights nedir?
![Application Insights temel iş akışı](./media/app-insights-devops/020.png)

1. Application Insights uygulamanızı Instruments ve uygulama çalışırken ilgili telemetri gönderir. Application Insights SDK'sı uygulamaya oluşturabilir ya da çalışma zamanında izleme uygulayabilirsiniz. Kendi telemetrinizi normal modüller ekleyebilirsiniz gibi eski yöntemi daha esnektir.
2. Burada, depolanan işlenen ve Application Insights portalındaki telemetriyi gönderilir. (Application Insights Microsoft Azure üzerinde barındırılan olsa da, tüm web uygulamaları - yalnızca Azure uygulamalarını izleyebilirsiniz.)
3. Telemetri grafikleri biçimidir ve olayların tabloları size sunulur.

Telemetri iki ana türü vardır: toplanan ve raw örnekleri. 

* Örnek veri Örneğin, web uygulamanız tarafından alınan bir isteğin bir rapor içerir. İçin bulun ve Application Insights portalında arama aracını kullanarak bir istek ayrıntılarını inceleyin. Örnek uygulamanızı nasıl uzun sürdüğü için istek, yanı sıra istenen URL yanıt, istemci ve diğer verileri konumunu yaklaşık gibi verileri dahildir.
* Böylece yanıt süreleri ile istekleri oranını karşılaştırabilirsiniz birim saat başına olay sayısı toplanan verileri içerir. Ayrıca, ortalama istek yanıt süreleri gibi ölçümleri içerir.

Veri ana kategorileri şunlardır:

* Uygulamanıza (genellikle HTTP istekleri), URL, yanıt süresi ve başarı veya başarısızlık verileriyle istek sayısı.
* Bağımlılıklar - URI, yanıt sürelerini ve başarı ile de uygulamanız tarafından oluşturulan REST ve SQL çağrıları
* Yığın izlemeleri dahil olmak üzere özel durumlar.
* Kullanıcıların tarayıcılardan gelen sayfası görünüm verileri.
* Ölçüm ölçümleri yanı sıra, performans sayaçları gibi kendiniz yazın. 
* İş olaylarını izlemek için kullanabileceğiniz özel olaylar
* Günlük izlemelerini hata ayıklama için kullanılır.

## <a name="case-study-real-madrid-fc"></a>Örnek olay incelemesi: Gerçek Madrid F.C.
Web hizmetinin [gerçek Madrid futbol kulübü](http://www.realmadrid.com/) hakkında 450 milyon fanlar dünyanın işlevi görür. Fan hem web tarayıcıları ve Kulübe'nın mobil uygulamaları aracılığıyla erişim. Fan yalnızca biletleri kitap, ancak sonuçları, oynatıcıları ve yaklaşan oyunlar bilgileri ve video klip de erişim. Hedefleri sayıda belirtmek gibi filtrelerle arama yapabilirsiniz. Sosyal medya bağlantılar da vardır. Kullanıcı deneyimini yüksek oranda kişiselleştirilmiş ve iki yönlü iletişim fanlar bulunmaya tasarlanmıştır.

Çözüm [hizmetler ve uygulamalar Microsoft azure'da oluşan bir sistemdir](https://www.microsoft.com/en-us/enterprise/microsoftcloud/realmadrid.aspx). Ölçeklenebilirlik anahtar bir gereksinimdir: trafiği değişkendir ve çok yüksek birimleri sırasında ve eşleşmeleri geçici ulaşabilirsiniz.

İçin gerçek Madrid, sistem performansını izlemek için önemlidir. Azure Application Insights, bir güvenilir ve yüksek düzeyde hizmet sağlama, sistem üzerindeki kapsamlı bir görünüm sağlar. 

Club Ayrıca kendi fanların ayrıntılı anlama alır: oldukları (% 3'yalnızca olan İspanya '), hangi ilgi sahip oldukları oynatıcılar, geçmiş sonuçları ve yaklaşan oyunları ve nasıl bunlar sonuçlar eşleşecek şekilde yanıt.

Bu telemetri verileri çoğu çözüm Basitleştirilmiş ve azaltılmış işletim karmaşıklığını hiçbir eklenen kodu ile otomatik olarak toplanır.  İçin gerçek Madrid, Application Insights, her ay 3.8 milyar telemetri noktaları ile ilgilidir.

Gerçek Madrid Power BI modülü kendi telemetri görüntülemek için kullanır.

![Application Insights telemetri Power BI görünümü](./media/app-insights-devops/080.png)

## <a name="smart-detection"></a>Akıllı algılama
[Öngörülü tanılama](app-insights-proactive-diagnostics.md) yeni bir özelliktir. Herhangi bir özel yapılandırma sizin tarafınızdan olmadan Application Insights otomatik olarak algılar ve olağan dışı miktarı artar, uygulamanızda başarısızlık oranları içinde hakkında sizi uyarır. Arka plan zaman hataları ve ayrıca bir artışa istekleri yalnızca ile orantılı olan miktarı artar yoksaymayı akıllıca olur. Sonra e-postası Ara hemen hakkında anlarsınız dolayısıyla Örneğin, bağımlı hizmetleri birinde bir hata olduğunda ya da yeni, henüz dağıtılan yapı kadar iyi çalışmıyor. (Ve Web kancalarını diğer uygulamalar tetiklersiniz.)

Bu özellik bir diğer unsuru bulmak sabit olağan dışı desenleri performans için arayan telemetrinize günlük ayrıntılı bir analizini gerçekleştirir. Örneğin, belirli bir coğrafi bölge ile veya belirli bir tarayıcı sürümü ile ilişkili yavaş performans bulabilirsiniz.

Her iki durumda da uyarı yalnızca, bulunduğundan, ancak ilgili özel durum raporları gibi sorunu tanılamalarına yardımcı olmak için gereksinim duyduğunuz veri sunar Belirtiler bildirir.

![Öngörülü tanılama e-posta](./media/app-insights-devops/030.png)

Müşteri Samtec belirtilmektedir: "son özelliğini sırasında cutover altında ölçeklendirilmiş kaynak sınırlarına basarsa ve zaman aşımlarına neden olan bir veritabanı bulduk. Öngörülü algılama Uyarıları gelen tam anlamıyla biz tanıtılan gibi çok yakın gerçek zamanlı sorun önceliklendirmek gibi. Azure platformu uyarılarla birlikte bu uyarı bize neredeyse anında sorunu düzeltin olunmasına yardımcı oldu. Toplam kapalı kalma süresi < 10 dakika."

## <a name="live-metrics-stream"></a>Canlı ölçümleri akış
En son sürüme dağıtma harcad bir deneyim olabilir. Herhangi bir sorun varsa, böylece, gerekirse yedekleyebilirsiniz, bunlarla ilgili hemen bilmek ister. Ölçümler bir canlı akışı hakkında bir saniye gecikmeyle anahtar ölçümleri sağlar.

![Canlı ölçümleri](./media/app-insights-devops/040.png)

Ve hemen bir örnek herhangi bir hata veya özel durumları inceleyin olanak tanır.

![Canlı hata olayları](./media/app-insights-devops/live-stream-failures.png)

## <a name="application-map"></a>Uygulama Eşlemesi
Uygulama eşlemesi, kolayca performans sorunlarını ve sorunlu akışları dağıtılmış ortamınızda çoğaltmanın tanımlamanıza olanak üstünde performans bilgileri yerleştirmede uygulama topolojinizi otomatik olarak bulur. Azure Hizmetleri Uygulama bağımlılıklarını bulmasını sağlar. Kod ilgili ise veya bağımlılık ilgili ve ilgili tanılama tek konumdan ayrıntıya gelen deneyimi anlayarak sorun önceliklendirme. Örneğin, uygulamanızın SQL katmanındaki performans düşüşünü nedeniyle başarısız. Uygulama eşlemesi ile hemen görmek ve SQL dizin Danışmanı'nı detaya veya sorgu öngörüleri karşılaşabilirsiniz.

![Uygulama Eşlemesi](./media/app-insights-devops/050.png)

## <a name="application-insights-analytics"></a>Uygulama Öngörüler analizi
İle [Analytics](app-insights-analytics.md), rasgele sorgular güçlü SQL benzeri bir dil yazabilirsiniz.  Tüm uygulama yığınını arasında tanılama çeşitli yönlerden bağlanın ve hizmet performansı iş ölçümleri ve müşteri deneyimi ile ilişkilendirmek için doğru soruları sorabilirsiniz kolay olur. 

Tüm telemetri örneği ve Portalı'nda depolanan ölçüm ham verileri sorgulayabilirsiniz. Dil filtresi, birleştirme, toplama ve diğer işlemleri içerir. Alanlarını hesaplamak ve istatistiksel çözümleme gerçekleştirin. Tablo ve grafik görselleştirmeleri vardır.

![Analytics sorgu ve sonuçları grafiği](./media/app-insights-devops/025.png)

Örneğin, kolay olduğundan:

* Uygulamanızın istek performans verileri deneyimlerini anlamak için müşterinin katmanları göre segmentlere ayırmak.
* Belirli hata kodları veya özel olay adları sırasında dinamik site araştırmalar arayın.
* Özellikleri nasıl edinilen ve benimsenen anlamak için belirli müşterilere'nın uygulama kullanımını detaya.
* Oturumlar ve anında müşteri desteği sağlamak destek ve işlemleri ekipleri etkinleştirmek belirli kullanıcılar için yanıt sürelerini izleyen.
* Özellik öncelik soruları yanıtlamak için sık kullanılan uygulama özelliklerini belirler.

Müşteri DNN belirtilmektedir: "Application Insights sağlanan bizimle olan denklemi eksik parçası mümkün birleştirme, sıralama, sorgu ve filtre veri gerektiğinde. Ekibimiz güçlü sorgu dili verileri bize Öngörüler bulmak ve sorunları çözmek izin bulmak için kendi resimleri ve deneyimi kullanmasını sağlayan bile biz vardı biliyoruz alamadık. Başlayarak soruları gelen çok ilginç yanıtların *' ı şimdi çok daha güzel If...'.*"

## <a name="development-tools-integration"></a>Geliştirme araçları tümleştirme
### <a name="configuring-application-insights"></a>Application Insights yapılandırma
Visual Studio ve Eclipse geliştirdiğiniz projenin doğru SDK paketleri yapılandırmak için araçlar vardır. Application Insights eklemek için menü komutu yoktur.

Ardından framework Log4N, NLog veya System.Diagnostics.Trace gibi oturum izleme kullanılmasını görülüyorsa, böylece istekleri, bağımlılık çağrıları ve özel durumları ile kolayca izlemeleri ilişkilendirebilirsiniz günlükleri diğer telemetri ile birlikte Application Insights'a gönderme seçeneği alın.

### <a name="search-telemetry-in-visual-studio"></a>Visual Studio'da arama telemetri
Geliştirme ve bir özellik hatalarını ayıklarken görüntülemek ve doğrudan Visual Studio'da web portalı olduğu gibi aynı arama özellikleri kullanarak, telemetri arayın.

Ve Application Insights bir özel durum kapattığında, Visual Studio'da veri noktası görüntülemek ve doğrudan ilgili koda atlama.

![Visual Studio arama](./media/app-insights-devops/060.png)

Hata ayıklama sırasında Visual Studio ancak portala göndermeden görüntüleme telemetri geliştirme Makinenizde tutmak için seçeneğiniz vardır. Bu yerel seçeneği üretim telemetri ile hata ayıklama karıştırma önler.

### <a name="build-annotations"></a>Ek açıklamaları oluşturma
Yapı ve uygulamanızı dağıtmak için Visual Studio Team Services kullanıyorsanız dağıtım ek açıklamaları grafiklerde Portalı'nda görünür. En son sürüm ölçümleri herhangi bir etkisi olsaydı, belirgin olur.

![Ek açıklamaları oluşturma](./media/app-insights-devops/070.png)

### <a name="work-items"></a>İş öğeleri
Bir uyarı oluştuğunda, Application Insights, otomatik olarak bir iş öğesi izleme sistemi çalışmanızı oluşturabilirsiniz.

## <a name="but-what-about"></a>Ancak ne...?
* [Gizlilik ve depolama](app-insights-data-retention-privacy.md) -telemetrinizi Azure güvenli sunucularda tutulur.
* Performans - etkisi çok düşüktür. Telemetri toplu hale.
* [Fiyatlandırma](app-insights-pricing.md) -, ücretsiz kullanmaya başlayabilir ve düşük biriminde iken sürdürür.


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Sonraki adımlar
Application Insights ile Başlarken kolaydır. Ana Seçenekler şunlardır:

* Zaten çalışan web uygulaması izleme. Bu, tüm yerleşik performans telemetrisini sağlar. İçin kullanılabilir [Java](app-insights-java-live.md) ve [IIS sunucuları](app-insights-monitor-performance-live-website-now.md), için ve ayrıca [Azure web uygulamaları](app-insights-azure.md).
* Projenizi geliştirme sırasında izleme. Bunu yapabilmeniz [ASP.NET](app-insights-asp-net.md) veya [Java](app-insights-java-get-started.md) uygulamaları yanı [Node.js](app-insights-nodejs.md) ve ana bilgisayar [diğer türleri](app-insights-platforms.md). 
* Aracı [herhangi bir web sayfasında](app-insights-javascript.md) kısa kod parçacığını ekleyerek düzenleyin.

