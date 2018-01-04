---
title: "Akıllı algılama - performans anormalliklerini | Microsoft Docs"
description: "Application Insights, uygulama telemetri akıllı çözümleme yapar ve olası sorunları sizi uyarır. Bu özellik Kurulum gerekir."
services: application-insights
documentationcenter: windows
author: antonfrMSFT
manager: carmonm
ms.assetid: 6acd41b9-fbf0-45b8-b83b-117e19062dd2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: mbullwin
ms.openlocfilehash: 3310239b5569ca5b63bd39acb4d192a4e54780e4
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="smart-detection---performance-anomalies"></a>Akıllı algılama - performans Anormalliklerini

[Application Insights](app-insights-overview.md) otomatik olarak, web uygulamanızın performansını analiz eder ve olası sorunlar hakkında sizi uyarabilir. Bizim akıllı algılama bildirimlerini aldığından, bu okuma.

Bu özellik için Application Insights, uygulamanızın yapılandırma dışında hiçbir özel kurulum gerektirir (üzerinde [ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), veya [Node.js](app-insights-nodejs.md)hem de [web sayfası kod](app-insights-javascript.md)). Uygulamanızı yeterli telemetri oluşturduğunda etkin olur.

## <a name="when-would-i-get-a-smart-detection-notification"></a>Bir akıllı algılama bildirim ulaştıklarında?

Application Insights, uygulamanızın performansını şu yollardan biriyle düştü algıladı:

* **Yanıt süresi düşüşü** -uygulamanızı daha yavaş için kullanıldığını isteklerine yanıt başlatıldı. Son dağıtımınız'teki bir gerileme olduğundan değişiklik Örneğin hızlı silinmiş olabilir. Veya, belki de bellek sızıntısı tarafından neden aşamalı, silinmiş olabilir. 
* **Bağımlılık süresi düşüşü** -uygulamanızı bir REST API'si, veritabanı ya da diğer bağımlılık çağrılar. Bağımlılık için kullanılan daha daha yavaş yanıt vermiyor.
* **Yavaş performans düzeni** -uygulamanızı yalnızca bazı istekleri etkileyen bir performans sorunu görünüyor. Örneğin, sayfalar daha yavaş diğerlerinden tarayıcının bir türündeki yüklüyorsunuz; veya isteklerini belirli bir sunucudan daha yavaş hizmet vermeleri sağlanır. Şu anda bizim algoritmalar, sayfa yükleme sürelerinin, istek yanıt sürelerini ve bağımlılık yanıt sürelerini bakın.  

Akıllı algılama en az 8 gün çalışılabilir birim telemetrisini normal performansının taban çizgisini oluşturmak için gereklidir. Bu nedenle, uygulamanızı belirli bir döneme ait çalıştıran sonra herhangi bir önemli sorun bir bildirim neden olur.


## <a name="does-my-app-definitely-have-a-problem"></a>Uygulamam kesinlikle bir sorun var mı?

Hayır, bir bildirim uygulamanızı kesinlikle bir sorun olduğu anlamına gelmez. Bu öneri hakkında daha fazla yakından bakmak isteyebilirsiniz yeterlidir şeyle ilgili olur.

## <a name="how-do-i-fix-it"></a>Bunu nasıl düzeltirim?

Bildirimleri tanılama bilgileri içerir. Örnek aşağıda verilmiştir:


![Sunucu yanıt süresi düşüşü algılama bir örneği burada verilmiştir](./media/app-insights-proactive-diagnostics/server_response_time_degradation.png)

1. **Önceliklendirme**. Bildirim kaç kullanıcı veya kaç işlemleri etkilenir gösterir. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsam**. Sorun, tüm trafik veya yalnızca bazı sayfaları söz konusu? Belirli tarayıcılar veya konumları sınırlı? Bu bilgiler bildirimden alınabilir.
3. **Tanılama**. Genellikle, bildirim tanılama bilgileri sorunun doğasına önerir. Örneğin, istek oranı yüksek olduğunda yanıt süresi düşerse sunucunuz veya bağımlılıkları aşırı öneren. 

    Aksi takdirde, Application Insights'ta performans dikey penceresini açın. Burada, bulacaksınız [profil oluşturucu](app-insights-profiler.md) veri. Özel durumlar varsa, siz de deneyebilirsiniz [anlık görüntü hata ayıklayıcı](app-insights-snapshot-debugger.md).



## <a name="configure-email-notifications"></a>E-posta bildirimleri yapılandırma

Akıllı algılama bildirimleri varsayılan olarak etkindir ve sahip olanlar gönderilen [sahipleri, Katkıda Bulunanlar ve okuyucular erişmek için Application Insights kaynağı](app-insights-resources-roles-access-control.md). Bunu değiştirmek için ya da tıklatın **yapılandırma** e-posta bildirimi ya da Application ınsights'ta akıllı algılama ayarlarını Aç. 
  
  ![Akıllı algılama ayarları](./media/app-insights-proactive-diagnostics/smart_detection_configuration.png)
  
  * Kullanabileceğiniz **aboneliği** e-posta bildirimleri almayı durdurmak için akıllı algılama e-postadaki bağlantıya.

E-postaları akıllı algılamaların performans anormalliklerini hakkında bir e-posta Application Insights kaynağı başına günde sınırlıdır. Yalnızca o gün algılandı en az bir yeni sorun olduğunda e-posta gönderilir. Herhangi bir iletisi yineler alamazsınız. 

## <a name="faq"></a>SSS

* *Bu nedenle, Microsoft personeli my verilerini görmesine?*
  * Hayır. Hizmet tamamen otomatik olarak yapılır. Yalnızca bildirimleri alır. Verileriniz [özel](app-insights-data-retention-privacy.md).
* *Application Insights tarafından toplanan tüm verileri çözümlemek?*
  * Değil şu anda. Şu anda isteği yanıt süresi, bağımlılık yanıt süresi ve sayfa yükleme süresi çözümleyin. Ek ölçümler analizini ileriye doğru arama bizim biriktirme listesi üzerinde ' dir.

* Ne tür bir uygulama bu için çalışıyor mu?
  * Bu degradations uygun telemetri oluşturması herhangi bir uygulamada algılanır. Application Insights web uygulamanızda yüklediyseniz, ardından istekleri ve bağımlılıkları otomatik olarak izlenir. Ancak, arka uç Hizmetleri veya diğer uygulamalardan çağrıları eklediyseniz [TrackRequest()](app-insights-api-custom-events-metrics.md#trackrequest) veya [TrackDependency](app-insights-api-custom-events-metrics.md#trackdependency), sonra da akıllı algılama aynı şekilde çalışır.

* *I kendi anomali algılama kuralları oluşturabilir veya mevcut kurallar özelleştirme?*

  * Henüz, ancak şunları yapabilirsiniz:
    * [Uyarıları ayarlamak](app-insights-alerts.md) , söyleyin, ne zaman bir ölçüm bir eşik kestiği.
    * [Telemetri verme](app-insights-export-telemetry.md) için bir [veritabanı](app-insights-code-sample-export-sql-stream-analytics.md) veya [Powerbı için](app-insights-export-power-bi.md), burada, bunu kendiniz çözümleyebilirsiniz.
* *Sıklıkla çözümleme yapılır?*

  * Biz analiz günlük önceki gün (tam gün içinde UTC saat dilimi) telemetrisinden çalıştırın.
* *Bu nedenle, bu Değiştir mu [ölçüm uyarıları](app-insights-alerts.md)?*
  * Hayır.  Olağan dışı düşünebilirsiniz her davranışı algılama için yürütme yok.


* *Bir anımsatıcı, herhangi bir bildirim yanıtı yapmayın, alacak mıyım?*
  * Hayır, her sorun hakkında bir ileti yalnızca bir kez alın. Sorun devam ederse akıllı dikey akış algılama, güncelleştirilir.
* *I e-posta kesildi. Portalda bildirimleri nereden bulabilirim?*
  * Uygulamanıza Application Insights bakış tıklatın **akıllı algılama** döşeme. Var. tüm bildirimleri 90 gün geri Bul mümkün olur.

## <a name="how-can-i-improve-performance"></a>Performansı artırmak ne?
Yavaş ve başarısız yanıtları kendi deneyimlerden bildiğiniz gibi web sitesi kullanıcı için en büyük frustrations biridir. Bu nedenle, sorunları gidermek önemlidir.

### <a name="triage"></a>Değerlendirme
İlk olarak, önemli mu? Bir sayfa her zaman yüklenmesi yavaş, ancak yalnızca %1 sitenizin kullanıcılarının herhangi bir zamanda bakmak sahip belki de dikkat etmeniz gereken daha önemli noktalar varsa. Diğer taraftan, %1 kullanıcı açın, ancak her zaman özel durumları oluşturur yalnızca, araştırma değer olabilir.

Etkisi deyimi (etkilenen kullanıcılar veya trafiği %) genel bir kılavuz olarak kullanın, ancak Yazının tamamını olmadığını unutmayın. Onaylamak için diğer kanıt toplayın.

Sorunu parametrelerinin göz önünde bulundurun. Coğrafya bağımlı olması durumunda ayarlanan [kullanılabilirlik testleri](app-insights-monitor-web-app-availability.md) bu bölge dahil: yalnızca ağ ilgili sorunlar olabilir, bu alanına.

### <a name="diagnose-slow-page-loads"></a>Yavaş sayfa yüklemeleri tanılama
Sorun nerede? Sunucu yavaş yanıt, sayfa çok uzun veya çok fazla görüntülemek için iş yapmak tarayıcı sahip mi?

Tarayıcılar ölçüm dikey penceresi açın. Tarayıcı sayfa yükleme süresi gösterir zaman nerede bulunacağını bölümlenmiş görüntüler. 

* Varsa **isteği gönderdiğinizde** olan yüksek ya da sunucu yavaş yanıt ya da çok miktarda veri ile bir post isteği olur. Bakmak [performans ölçümleri](app-insights-web-monitor-performance.md#metrics) yanıt sürelerini araştırmak için.
* Ayarlanan [bağımlılık izleme](app-insights-asp-net-dependencies.md) yavaşlığı dış hizmetler veya veritabanınızı nedeniyle olup olmadığına bakın.
* Varsa **alma yanıt** yaygındır, sayfanızın ve bağımlı bölümleri - JavaScript, CSS, vb. (ancak zaman uyumsuz olarak yüklenen veriler) görüntüleri uzun. Ayarlanmış bir [kullanılabilirlik test](app-insights-monitor-web-app-availability.md)ve bağımlı bölümleri yükleme seçeneğine ayarladığınızdan emin olun. Bazı sonuçları aldığınızda, bir sonuç ayrıntılarını açın ve farklı dosyaların yükleme süreleri görmek için genişletin.
* Yüksek **istemci işleme süresi** betikleri yavaş çalışıyor önerir. Nedeni açık değilse, bazı zamanlama kod eklemeyi göz önünde bulundurun ve saatleri trackMetric çağrılarında gönderin.

### <a name="improve-slow-pages"></a>Yavaş sayfa geliştirmek
Tüm burada yinelenecek denemez şekilde sunucu yanıtlarını ve sayfa yükleme sürelerinin artırma önerileri, tam web yoktur. Aşağıda, büyük olasılıkla zaten, yalnızca düşünüyorum sağlamak için bilmeniz birkaç ipucu verilmiştir:

* Yavaş büyük dosyalar nedeniyle yükleniyor: komut dosyaları ve diğer bölümleri zaman uyumsuz olarak yükler. Betik paketleme kullanın. Ana sayfa verilerine ayrı olarak yük pencere öğeleri sonu. Uzun tablolar için eski düz HTML gönderme: verileri JSON veya diğer compact biçimi olarak istemek için bir komut dosyası kullanın ve sonra tablo yerinde doldurun. Tüm bu konuda yardımcı olmak için harika çerçeveleri vardır. (Bunlar da büyük komut dosyaları, Elbette oluşturulmasını gerektirir.)
* Yavaş sunucu bağımlılıkları: bileşenlerinizi coğrafi konumlara göz önünde bulundurun. Örneğin, Azure kullanıyorsanız, web sunucusu ve veritabanı aynı bölgede yer aldığından emin olun. Sorguları ihtiyaç duydukları daha fazla bilgi almak? Önbelleğe alma veya Yardım toplu işleme musunuz?
* Kapasite sorunları: ara sunucu ölçümlere yanıt sürelerinin ve istek sayısı. İstek sayısı genişliğindeki yükselmeleri ile yanıt sürelerini orantısız en yüksek, sunucularınızın uzatılır olasıdır.


## <a name="server-response-time-degradation"></a>Sunucu yanıt süresi düşüşü

Yanıt süresi düşüşü bildirimi size bildirir:

* Bu işlem için normal yanıt süresine göre yanıt süresi.
* Kaç tane kullanıcılar etkilenir.
* Ortalama yanıt süresi ve algılama ve 7 gün önce gün bu işlem için 90 yüzdebirlik yanıt süresi. 
* Algılama gününü ve 7 gün önce bu işlemi isteği sayısı.
* Bu işlemde bozulması ve ilgili bağımlılıklar degradations arasında bağıntı. 
* Sorunu tanılamak yardımcı olması için bağlar.
  * Profil Oluşturucu izlemeleri işlemi zamanın nerede harcandığına görüntülemenize yardımcı olur (bağlantıyı profil oluşturucu izleme örnekler algılama süre boyunca bu işlem için toplanan kullanılabilir). 
  * Performans raporları Gezgini'nde ölçüm burada dilim ve bu işlem için zaman aralığı/filtreleri inin.
  * Belirli çağrıları özelliklerini görüntülemek bu çağrıları için arama yapın.
  * Hata raporları - durumunda > 1 Bu anlamına olduğunu hatalar oldu performansın düşmesine katkıda bulunan bu işlemde sayısı.

## <a name="dependency-duration-degradation"></a>Bağımlılık süresi düşüşü

Modern uygulama giderek daha ağır güvenilirlik dış hizmetler hakkında müşteri adayları, çoğu durumda, mikro hizmetler tasarım yaklaşımı benimsemeye. Bazı veri platformu veya olsa bile, uygulamanızın kullanır, örneğin, kendi bot büyük olasılıkla daha İnsan şekillerde etkileşim kurmak, aracılarını etkinleştirmek için bazı bilişsel hizmetler sağlayıcıdaki 8.8.8.8'den hizmeti ve yanıtları sekmelerin yerine çekmesini bot için bazı veri deposu hizmetini oluşturma m.  

Örnek bağımlılık düşüşü bildirimi:

![Bağımlılık süresi düşüşü algılama bir örneği burada verilmiştir](./media/app-insights-proactive-diagnostics/dependency_duration_degradation.png)

Size bildirir dikkat edin:

* Bu işlem için normal yanıt süresine göre süresi
* Kaç kullanıcının etkilenen
* Ortalama süreye ve 90 yüzdebirlik süresince bu bağımlılığı algılama gününü ve 7 gün önce
* Algılama gününü ve 7 gün önce çağrı bağımlılık sayısı
* Sorunu tanılamak yardımcı olması için bağlantılar
  * Bu bağımlılığı ölçüm Explorer'da performans raporları
  * Bu bağımlılık çağrılarını çağrıları özelliklerini görüntülemek için arama
  * Hata raporları - durumunda > 1 vardı bu ortalama süre düşmesine katkıda bulunan algılama dönemde bağımlılık çağrıları başarısız sayısı. 
  * Bu bağımlılık süresi ve sayısını hesaplamak sorgularıyla Analytics açın  

## <a name="smart-detection-of-slow-performing-patterns"></a>Yavaş gerçekleştirme desenleri akıllı algılama 

Application Insights, kullanıcılarınızın kısmı etkiler veya yalnızca bazı durumlarda kullanıcıları etkileyen performans sorunlarını bulur. Örneğin, sayfaları yük hakkında bildirim bir tür diğer türler tarayıcıların, tarayıcı yavaştır veya istekleri daha yavaş belirli bir sunucudan hizmet alır. Belirli bir işletim sistemi kullanan istemciler için bir coğrafi alanda yavaş sayfa yükler gibi özellikleri bileşimleri ile ilgili sorunları da bulabilir.  

Bu gibi bozukluklar veri inceleyerek algılamak çok zor ancak düşündüğünüzden daha yaygın. Çoğunlukla bunlar müşterilerinizin şikayetçi olduğunda yalnızca yüzey. O zamana dek çok geç: Etkilenen kullanıcılar zaten rakiplerinizin için geçiş!

Şu anda bizim algoritmalar, sayfa yükleme sürelerinin, sunucuda istek yanıt sürelerini ve bağımlılık yanıt sürelerini bakın.  

Eşikleri ayarlayın veya kurallarını yapılandırmak gerekmez. Machine learning ve veri araştırma algoritmalarını anormal desenleri algılamak için kullanılır.

![E-posta uyarıdan Azure tanılama raporu açmak için bağlantıya tıklayın](./media/app-insights-proactive-performance-diagnostics/03.png)

* **Zaman** sorunu algılandı zamanı gösterir.
* **Ne** açıklar:

  * Algılanan Sorun;
  * Bulduk olay kümesini özelliklerini sorunlu davranış görüntülenir.
* Tablonun tüm diğer olaylar ortalama davranışını gerçekleştirme hatalı kümesiyle karşılaştırır.

Ölçüm Gezgini ve arama yavaş gerçekleştirme kümesinin özelliklerini ve saat üzerinde filtrelenmiş ilgili raporları açmak için bağlantıları tıklatın.

Zaman aralığı ve telemetri keşfetmek için filtreleri değiştirin.

## <a name="next-steps"></a>Sonraki adımlar
Bu tanılama araçları, uygulamanızdan alınan telemetri incelemek yardımcı olur:

* [Profil Oluşturucu](app-insights-profiler.md) 
* [Anlık görüntü hata ayıklayıcı](app-insights-snapshot-debugger.md)
* [Analizler](app-insights-analytics-tour.md)
* [Analytics akıllı tanılama](app-insights-analytics-diagnostics.md)

Akıllı algılamaların tamamen otomatik olarak yapılır. Ancak, belki de daha fazla bazı uyarıları ayarlamak ister misiniz?

* [El ile yapılandırılmış ölçüm uyarıları](app-insights-alerts.md)
* [Kullanılabilirlik web testleri](app-insights-monitor-web-app-availability.md)
