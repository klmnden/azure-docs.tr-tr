---
title: Web uygulaması performans izleme - Azure Application Insights | Microsoft Docs
description: Application Insights devOps döngüsü ile nasıl uyduğunu
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 479522a9-ff5c-471e-a405-b8fa221aedb3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/21/2018
ms.author: mbullwin
ms.openlocfilehash: 24b0bc01b5cb4f1d2696a7c9526d586c9b42d0fc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60899760"
---
# <a name="deep-diagnostics-for-web-apps-and-services-with-application-insights"></a>Application Insights ile ayrıntılı web uygulaması ve hizmet tanılama
## <a name="why-do-i-need-application-insights"></a>Application Insights neden gerekiyor?
Application Insights çalışan web uygulamanızı izler. Hataları ve performans sorunları hakkında size bildirir ve müşterilerin uygulamanızı kullanımını analiz etmenize yardımcı olur. Bu, birçok platformda (ASP.NET, Java EE, Node.js,...) üzerinde çalışan uygulamalar için çalışır ve Bulut veya şirket içinde barındırılan. 

![Web uygulamaları sunmaya karmaşıklığı yönleri](./media/devops/010.png)

Çalışırken, modern bir uygulama izlemek için gereklidir. En önemlisi, müşterilerin çoğu yapmadan önce hatalarını algılamak istediğiniz. Ayrıca bulmak ve yıkıcı değil ancak performans sorunları, düzeltmek istediğiniz, belki de trafiği yavaşlatmak veya bazı dileriz kullanıcılarınıza neden. Ve sistem için tatmininizi gerçekleştirirken, kullanıcıların ile yaptığı bilmek istiyorsunuz: Bunlar, en son özelliğini kullanıyor musunuz? Bunlar birlikte başarılı oluyor?

Modern web uygulamaları, bir sürekli teslim dönemi içinde geliştirilen: yeni bir özellik veya geliştirme; sürüm kullanıcılar için nasıl çalıştığını inceleyin; Bu bilgilere dayanan geliştirme bir sonraki adımını planlayın. Bu döngü önemli bir parçası gözlem aşamasıdır. Application Insights, bir web uygulaması performansını ve kullanımını izlemek için araçlar sağlar.

Bu işlemin en önemli bir yönüdür tanılama ve Tanılama ' dir. Uygulama başarısız olursa, ardından iş kaybolur. Birinci izleme framework'ün bu nedenle güvenilir bir şekilde hatalarını algılamak için hemen bildir ve sorunu tanılamak için gereken bilgilerle sunmak için rolüdür. Application Insights'ın yaptığı tam olarak budur.

### <a name="where-do-bugs-come-from"></a>Burada hataları gelir?
Web sistemlerinde hataları genellikle yapılandırma sorunlarını veya birçok bileşenleri arasında hatalı etkileşimleri durumlardan kaynaklanır. Canlı site olayı bağlayabileceğiniz ilk görevi bu nedenle sorunun locus belirlemektir: hangi bileşen veya ilişki nedeni?

Bazı gri artı olanlar bize, bilgisayar programı bir bilgisayarda çalıştırıldığı daha basit bir dönem hatırlayabileceğiniz. Geliştiriciler göndermeden önce kapsamlı olarak sınamak; ve, nadiren bakın veya bunu yeniden düşünün. Kullanıcıların yıllardır kalan hatalarla put etmesi gerekir. 

Artık böylece çok farklı noktalardır. Uygulamanızı çalıştırmak için farklı cihaz deseninizi oluşturmayı vardır ve her birinde aynı davranış tam garanti etmek zor olabilir. Uygulamalarını bulutta barındırma, hızlı hataların düzeltilmesi ancak sürekli bir yarışma ve yeni özelliklerin beklentisi sık aralıklarla anlamına da gelir anlamına gelir. 

Bu koşullarda, kesin bir denetim üzerinde hata sayısını tutmak tek otomatik birim testi yoludur. Her teslimat üzerinde her şeyi yeniden el ile test mümkün olacaktır. Birim testi şimdi bir sıradan yapı işleminin parçasıdır. Xamarin Test Cloud'a gibi araçlar, otomatik UI testi birden çok tarayıcı sürümlerinde sağlayarak yardımcı olur. Bu test regimes için en az bir uygulama içinde bulunan hatalar oranını tutulabilir umuyoruz olanak tanır.

Tipik web uygulamalarının çoğu Canlı bileşenleri vardır. İstemci (tarayıcı veya cihaz uygulaması) ve web sunucusuna ek olarak, var. büyük olasılıkla önemli bir arka uç işleme Belki de arka uç bileşenlerinin bir işlem hattı veya iş Birliği yapan parçaları bazı koleksiyonu ' dir. Ve kaç tanesinin denetiminizi olmayacaktır - kullandığınız dış hizmetler oldukları.

Bunlar gibi yapılandırmalarında zor ve test ya da, her olası hata modunun dışındaki Canlı sistem öngörüyor uneconomical olabilir. 

### <a name="questions-"></a>Sorular...
Biz web sistemi geliştirirken bazı sorular isteriz:

* Uygulamamı kilitlenmesi? 
* Tam olarak ne oldu? -Bir istek başarısız olursa nasıl var. alındı bilmek istiyorum. Bir izleme olaylarının ihtiyacımız...
* Uygulamamı yeterince hızlı mı? Ne kadar normal isteklerine yanıt vermek için sürer?
* Sunucu yükü işleyebilirsiniz? İstekleri oranını arttığında yanıt süresi sabit içeriyor mu?
* Yanıt verme düzeyi my - REST API'ler, veritabanları ve uygulamamı çağıran diğer bileşenleri bağımlılıklardır. Özellikle sistem yavaşsa, bu benim bileşendir veya yavaş yanıtlar birisinden alıyorum?
* Uygulamamı yukarı veya aşağı mi? Bu gelen tüm dünyada görülebilir? Vermemeye başlarsa haberim...
* Kök nedeni nedir? My bileşen ya da bağımlılık hatası oldu mu? Bir iletişim sorununu nedir?
* Kaç kullanıcının etkilendiğini? En önemli olan gidermek için birden çok sorun varsa?

## <a name="what-is-application-insights"></a>Application Insights nedir?
![Application Insights'ın temel iş akışı](./media/devops/020.png)

1. Application Insights, uygulamanızın Instruments ve uygulama çalışırken ilgili telemetri gönderir. Application Insights SDK'sını uygulamaya oluşturabilir ya da çalışma zamanında izleme uygulayabilirsiniz. İlk yöntem daha esnek aynıdır, kendi telemetrinizi normal modüllerine ekleyebilirsiniz.
2. Burada, depolanan işlenen ve Application Insights portalına telemetri gönderilir. (Application Insights, Microsoft Azure'da barındırılan olsa da, tüm web uygulamaları - yalnızca Azure uygulamalarını izleyebilirsiniz.)
3. Telemetri, formun grafikleri ve tabloları olayların size sunulur.

Telemetri iki ana türü vardır: toplanmış ve ham örnekleri. 

* Örnek verilerini, örneğin, web uygulamanız tarafından alınan isteği bir rapor içerir. Bulma ve Application Insights portalında arama aracını kullanarak bir isteği ayrıntılarını inceleyin. Örnek uygulamanızın ne kadar sürdüğünü isteği, yanı sıra istenen URL'ye yanıt, istemci ve diğer verileri konumunu yaklaşık gibi verileri dahildir.
* Yanıt süreleri ile isteklerinin hızı karşılaştırabilmeniz için toplanan veri birimi saati başına olay sayısı içerir. Ayrıca, ortalama istek yanıt süreleri gibi ölçümleri içerir.

Ana veri kategorileri şunlardır:

* Uygulamanıza (genellikle HTTP istekleri), URL, yanıt süresi ve başarı veya başarısızlık verileriyle istek sayısı.
* Bağımlılıkları - URI, yanıt süreleri ve başarı ile de uygulamanız tarafından yapılan BEKLEYEN ve SQL çağrıları
* Yığın izlemeleri gibi durumlar.
* Kullanıcıların tarayıcılardan gelen sayfanın görünüm verileri.
* Ölçüm ölçümleri yanı sıra, performans sayaçları gibi kendiniz yazın. 
* İş olaylarını izlemek için kullanabileceğiniz özel olaylar
* Günlük izlemelerini, hata ayıklama için kullanılır.

## <a name="case-study-real-madrid-fc"></a>Örnek olay incelemesi: Real Madrid'in F.C.
Web hizmeti, [Real Madrid futbol kulübü](https://www.realmadrid.com/) 450 milyon dünyanın işlevi görür. Fanlar, hem web tarayıcıları ve mobil uygulamalar kulübü'nın aracılığıyla erişin. Fanlar yalnızca kitap biletleri olamaz, ancak ayrıca sonuçları, oyuncuların ve yaklaşan oyun bilgileri ve video klipleri erişim. Hedefleri sayıda puanlanmış gibi filtrelerle arama yapabilirsiniz. Sosyal medya bağlantıları vardır. Kullanıcı deneyimi, yüksek oranda kişiselleştirilmiş ve iki yönlü bir iletişim fanlar etkileşim kurmak amacıyla tasarlanmıştır.

Çözüm [hizmet ve uygulamaların Microsoft Azure üzerinde bir sistem](https://www.microsoft.com/inculture/sports/real-madrid/). Ölçeklenebilirlik temel bir gereksinimdir: trafik değişkendir ve çok yüksek miktarlarda sırasında ve eşleşme etrafında ulaşabilirsiniz.

İçin Real Madrid, sistemin performansını izlemek önemlidir. Azure Application Insights, bir güvenilir ve yüksek düzeyde hizmet sağlama, sistem kapsamlı bir görünüm sağlar. 

Kulübü Ayrıca kendi fanlar derinlemesine anlamak alır: nerede (yalnızca %3 olan İspanya'da) hangi ilgi sahip oldukları oyuncuların, geçmiş sonuçlarını ve yaklaşan oyunlar ve bunların sonuçlarını eşleştirilecek vereceği.

Bu telemetri verilerini çoğu çözüm Basitleştirilmiş ve işletimsel karmaşıklığın daha az hiçbir eklenen kodu ile otomatik olarak toplanır.  İçin Real Madrid, Application Insights, her ay 3.8 milyar telemetri noktaları ile ilgilidir.

Real Madrid, telemetri verilerini görüntülemek için Power BI modülü kullanır.

![Application Insights telemetri Power BI görüntüle](./media/devops/080.png)

## <a name="smart-detection"></a>Akıllı algılama
[Proaktif tanılama](../../azure-monitor/app/proactive-diagnostics.md) yeni bir özelliktir. Herhangi bir özel yapılandırma sizin tarafınızdan olmadan Application ınsights'ı otomatik olarak algılar ve hata oranları uygulamanızda olağan dışı artışlar hakkında uyarır. Bu hatalar yaşanacağı beklentisiyle ve ayrıca bir artış istekleri, yalnızca orantılı olan yükseldiğinde bir arka plan yok saymak akıllı bir işlemdir. Sonra e-postası Ara hemen sonra bunu anlarsınız Örneğin, bağımlı hizmetlerden biri başarısız olursa veya yeni dağıttığınız yeni derleme yaparsanız düzgün şekilde çalışmıyor. (Ve Web kancaları ve böylece diğer uygulamalar tetikleyebilirsiniz.)

Bu özellik, bir diğer unsuru günlük derinlemesine bir analize telemetrinizin bulmak zor olan olağan dışı performans için desenler arama gerçekleştirir. Örneğin, belirli bir coğrafi bölgeye veya belirli tarayıcı sürümü ile ilişkili yavaş performans bulabilirsiniz.

Her iki durumda da uyarı yalnızca, bulunduğunda ancak ilgili özel durum raporları gibi sorunun tanılanmasına yardımcı olmak için ihtiyacınız olan verileri de size belirtileri bildirir.

![Proaktif tanılama e-postası](./media/devops/030.png)

Müşteri Samtec olduğu söylenebilir: "Bir yeni özellik sırasında tam geçişi, kaynak sınırlarını ulaşma ve zaman aşımlarına neden olan bir altında ölçeklendirilmiş veritabanı bulduk. Proaktif algılama Uyarıları gelen tam anlamıyla biz bildirilen çok neredeyse gerçek zamanlı sorun önceliklendirme şekilde. Azure platformu uyarılarla birlikte bu uyarıyı neredeyse anında sorunu yardımcı olmuştur. Toplam kapalı kalma süresi < 10 dakika."

## <a name="live-metrics-stream"></a>Canlı ölçümleri Stream
En son derleme dağıtımı yüzünde Endişeli bir deneyim olabilir. Herhangi bir sorun varsa, gerekirse yedekleyebilirsiniz böylece bunlar hakkında hemen bilmek istiyorsunuz. Canlı ölçümler Stream yaklaşık bir saniye gecikmeyle ana ölçümleri sağlar.

![Canlı ölçümleri](./media/devops/0040.png)

Hemen tüm hataları ve özel durumların bir örneği incelemenize olanak tanır.

![Canlı hata olayları](./media/devops/002-live-stream-failures.png)

## <a name="application-map"></a>Uygulama Eşlemesi
Uygulama Haritası, kolayca performans sorunlarını ve sorunlu akışlar dağıtılmış ortamınız genelinde belirlemenize izin vermek için bunun üstünde performans bilgilerini düzenleme uygulamanızın topolojisini, otomatik olarak bulur. Azure hizmetlerine Uygulama bağımlılıklarını bulmasını sağlar. Kodla ilgili veya bağımlılık ilgili ve ilgili tanılama tek bir yerde ayrıntıya gelen deneyimi anlayarak sorunu önceliklendirmenize. Örneğin, uygulamanızın performansında SQL katmanında nedeniyle başarısız olabilir. Uygulama Haritası ile hemen görmek ve SQL dizin Danışmanı ayrıntıya veya sorgu öngörüleri karşılaşırsınız.

![Uygulama Eşlemesi](./media/devops/0050.png)

## <a name="application-insights-analytics"></a>Application Insights Analytics
İle [Analytics](../../azure-monitor/app/analytics.md), güçlü SQL benzeri bir dildir içinde rastgele sorgularınızı yazabilirsiniz.  Tüm uygulama yığınını arasında tanılama çeşitli yönlerden bağlanın ve hizmet performansı, iş ölçümleri ve müşteri deneyimini ilişkilendirmek için doğru soruları sormanız kolay bir hale gelir. 

Tüm telemetri örneği ve Portalı'nda depolanan ölçüm ham verileri sorgulayabilirsiniz. Dil filtresi, birleştirme, toplama ve diğer işlemleri içerir. Alanlarını hesaplamak ve istatistiksel analiz gerçekleştirin. Tablo ve grafik görselleştirmeleri vardır.

![Analiz sorgusu ve sonuçları grafiği](./media/devops/0025.png)

Örneğin, kolaydır:

* Uygulama isteği performans verilerini deneyimlerini anlamak için müşteri katmanlara göre segmentlere ayırın.
* Belirli hata kodlarıyla veya özel olay adlarının sırasında Canlı site araştırmalar'ı arayın.
* Özellikleri nasıl elde ve benimsenen anlamak için belirli müşterilere uygulama kullanımını detayına gidin.
* Oturumlarının ve anında müşteri desteği sağlamak desteği ve operasyon ekipleri etkinleştirmek belirli kullanıcılar için yanıt sürelerini izleyin.
* Özellik öncelik soruları yanıtlamak için sık kullanılan uygulama özelliklerini belirler.

Müşteri DNN olduğu söylenebilir: "Application Insights sağlanan mümkün bize teşekkür denklemi eksik bir parçası olan birleştirme, sıralama, sorgu ve filtre veri gerektiğinde. Ekibimiz, güçlü bir sorgu dili ile veri öngörüleri bulmak ve sorunları çözmek etmemizi de sağladı bulmak için kendi resimleri ve deneyimi kullanmasını sağlayan bile vardı biliyoruz kaydetmedi. Çok ilginç yanıtların başlayarak soruları geldiğini *' ı elmas if...'.* "

## <a name="development-tools-integration"></a>Geliştirme araçları tümleştirme
### <a name="configuring-application-insights"></a>Application Insights'ı yapılandırma
Visual Studio ve Eclipse geliştirdiğiniz proje için doğru SDK paketlerini yapılandırmak için araçları elde edersiniz. Application Insights eklemek için bir menü komutu yoktur.

Log4N, NLog veya System.Diagnostics.Trace gibi bir izleme günlüğe kaydetme çerçevesi kullanıyorsanız gerçekleşir, istekler, bağımlılık izlemeleri kolayca ilişkilendirebilmek ardından günlükleri diğer telemetri yanı sıra Application Insights'a gönderme seçeneği alırsınız çağrıları ve özel durumlar.

### <a name="search-telemetry-in-visual-studio"></a>Visual Studio'da telemetri arama
Geliştirme ve hata ayıklama bir özelliği sırasında görüntüleyebilir ve doğrudan web portalında olduğu gibi aynı arama özelliklerini kullanarak Visual Studio'nun içinde telemetri arayın.

Ve Application Insights bir özel durum oturum açtığında, Visual Studio'da veri noktası görüntüleyebilir ve doğrudan ilgili koda atlama.

![Visual Studio arama](./media/devops/060.png)

Hata ayıklama sırasında Visual Studio'da ancak portala göndermeden görüntüleme geliştirme makinenizde telemetri tutmak seçeneğiniz vardır. Bu yerel seçenek üretim telemetri ile hata ayıklama karıştırma önler.

### <a name="work-items"></a>İş öğeleri
Bir uyarı oluştuğunda, Application Insights izleme sistemi işinizi otomatik olarak bir iş öğesi oluşturabilirsiniz.

## <a name="but-what-about"></a>Ancak ne...?
* [Gizlilik ve depolama](../../azure-monitor/app/data-retention-privacy.md) -telemetrinizi Azure güvenli sunucularda tutulur.
* -Performans etkisi çok düşüktür. Telemetri toplu.
* [Fiyatlandırma](../../azure-monitor/app/pricing.md) -, ücretsiz kullanmaya başlayabilir ve düşük birimi iken sürdürür.


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Sonraki adımlar
Application Insights ile çalışmaya başlamak kolaydır. Ana Seçenekler şunlardır:

* [IIS sunucuları](../../azure-monitor/app/monitor-performance-live-website-now.md), için ve ayrıca [Azure App Service](../../azure-monitor/app/app-insights-overview.md).
* Projeniz, geliştirme sırasında izleyin. Bunu yapabilmeniz [ASP.NET](../../azure-monitor/app/asp-net.md) veya [Java](../../azure-monitor/app/java-get-started.md) uygulamaları yanı [Node.js](../../azure-monitor/app/nodejs.md) ve çok sayıda [diğer türleri](../../azure-monitor/app/platforms.md). 
* Gereç [herhangi bir web sayfasında](../../azure-monitor/app/javascript.md) kısa kod parçacığını ekleyerek.

