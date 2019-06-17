---
title: Akıllı algılama - performans anomalileri | Microsoft Docs
description: Application Insights, uygulama telemetrinizde akıllı analiz gerçekleştirir ve olası sorunları sizi uyarır. Bu özellik, herhangi bir kurulum gerekir.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 6acd41b9-fbf0-45b8-b83b-117e19062dd2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/04/2017
ms.reviewer: antonfr
ms.author: mbullwin
ms.openlocfilehash: b1a3b04427839736359c88f8ad6a8db5eedf8488
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61294092"
---
# <a name="smart-detection---performance-anomalies"></a>Akıllı algılama - performans Anomalileri

[Application Insights](../../azure-monitor/app/app-insights-overview.md) otomatik olarak web uygulamanızın performansını analiz eder ve olası sorunlar hakkında sizi uyarabilir. Bizim akıllı algılama bildirim aldığından, bu okuma.

Bu özellik, uygulamanız için Application Insights yapılandırma dışında hiçbir özel kurulum gerektirir (üzerinde [ASP.NET](../../azure-monitor/app/asp-net.md), [Java](../../azure-monitor/app/java-get-started.md), veya [Node.js](../../azure-monitor/app/nodejs.md)hem de [web sayfası kod](../../azure-monitor/app/javascript.md)). Uygulamanızı yeterli telemetri oluşturduğunda etkin değil.

## <a name="when-would-i-get-a-smart-detection-notification"></a>Bir akıllı algılama bildirim ne zaman elde edersiniz?

Application Insights, uygulamanızın performansını şu yollardan biriyle düştü algıladı:

* **Yanıt süresinde performans düşüşü** -uygulamanız için harcadığımız süreden daha yavaş isteklere yanıt başlatıldı. Son dağıtımınız'teki bir gerileme olduğundan değişiklik hızlı örnek için silinmiş olabilir. Veya, belki de bir bellek sızıntısı tarafından neden aşamalı, silinmiş olabilir. 
* **Bağımlılık süresi düşüşü** -REST API, veritabanı veya diğer bağımlılık çağrıları uygulamanızı sağlar. Bağımlılık için harcadığımız süreden daha yavaş yanıt veriyor.
* **Yavaş performans deseni** -yalnızca bazı istekleri etkileyen bir performans sorunu, uygulamanızı görünür. Örneğin, sayfaları bir tür tarayıcı diğerlerinden daha yavaş yükleniyor; veya istekleri belirli bir sunucudan daha yavaş hizmet vermeleri sağlanır. Şu anda, sayfa yükleme süreleri, istek yanıt süreleri ve bağımlılık yanıt süreleri algoritmalarınızı arayın.  

Akıllı algılama, normal performansının taban çizgisini oluşturmak için en az 8 gün çalışılabilir hacimde telemetri gerektirir. Bu nedenle, uygulamanızın belirli bir döneme ait çalıştırıldıktan sonra herhangi bir önemli sorun bildirim neden olur.


## <a name="does-my-app-definitely-have-a-problem"></a>Uygulamamı kesinlikle bir sorun var mı?

Hayır, bir bildirim uygulamanızı kesinlikle bir sorun olduğu anlamına gelmez. Bu öneri hakkında daha fazla bilgi yakından atmak isteyebilirsiniz yalnızca bir şey hakkında olur.

## <a name="how-do-i-fix-it"></a>Bunu nasıl düzeltirim?

Bildirimler, tanılama bilgilerini içerir. Bir örneği aşağıda verilmiştir:


![İşte bir örnek sunucu yanıt süresinde performans düşüşü algılama](media/proactive-performance-diagnostics/server_response_time_degradation.png)

1. **Önceliklendirme**. Bildirim kaç kullanıcının veya kaç operations etkilenen gösterir. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsam**. Sorun, tüm trafiği veya yalnızca bazı sayfalar etkileniyor? Belirli tarayıcılar veya konumlara sınırlıdır? Bu bilgiler gelen bildirim elde edilebilir.
3. **Tanılama**. Genellikle, bildirim tanılama bilgileri sorunun doğasına önerir. Örneğin, yanıt süresi uzuyor istek hızı yüksek olduğunda, sunucu veya bağımlılıklar aşırı önerir. 

    Aksi takdirde, Application Insights'da performans dikey penceresini açın. Burada, bulacaksınız [Profiler](profiler.md) veri. Özel durumlar varsa da deneyebilirsiniz [anlık görüntü hata ayıklayıcısı](../../azure-monitor/app/snapshot-debugger.md).



## <a name="configure-email-notifications"></a>E-posta bildirimlerini yapılandırma

Akıllı algılama bildirimleri varsayılan olarak etkindir ve sahip olanlar için gönderilen [sahipleri, Katkıda Bulunanlar ve okuyucular erişmek için Application Insights kaynağı](../../azure-monitor/app/resources-roles-access-control.md). Bunu değiştirmek için ' a tıklayın ya da **yapılandırma** e-posta bildirimi veya Application Insights, akıllı algılama ayarlarını Aç. 
  
  ![Akıllı algılama ayarları](media/proactive-performance-diagnostics/smart_detection_configuration.png)
  
  * Kullanabileceğiniz **aboneliği** e-posta bildirimleri almayı durdurmak için akıllı algılama e-postadaki bağlantıya.

Akıllı algılama performans anomalileri hakkında e-posta, Application Insights kaynağı başına bir günde bir e-posta sınırlıdır. Yalnızca o gün algılandı ve yeni en az bir sorun olduğunda e-posta gönderilir. Tüm iletinin yineler elde etmezsiniz. 

## <a name="faq"></a>SSS

* *Bu nedenle, Microsoft personeli verilerimi geldi mi?*
  * Hayır. Hizmeti tamamen otomatik olarak yapılır. Yalnızca bildirimleri alın. Verileriniz [özel](../../azure-monitor/app/data-retention-privacy.md).
* *Application Insights tarafından toplanan tüm verileri analiz etmeye?*
  * Değil şu anda. Şu anda isteği yanıt süresi, bağımlılık yanıt süresi ve sayfa yükleme süresi analiz ediyoruz. Ek ölçümler analizini ileriye doğru arama çalışıyoruz ' dir.

* Ne tür bir uygulama için bu çalışır?
  * Bu performans düşüşü yaşanması uygun telemetri oluşturması herhangi bir uygulamada algılanır. Web uygulamanızda Application Insights'ı yüklediyseniz, ardından istekleri ve bağımlılıkları otomatik olarak izlenir. Ancak, arka uç Hizmetleri ya da diğer uygulamalarda çağrıları eklediyseniz [TrackRequest()](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest) veya [TrackDependency](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency), akıllı algılama aynı şekilde çalışmayacaktır.

* *Kendi anomali algılama kuralları oluşturun veya miyim mevcut kurallarını özelleştirme?*

  * Henüz, ancak şunları yapabilirsiniz:
    * [Uyarıları Ayarlama](../../azure-monitor/app/alerts.md) , bilgi, ne zaman bir ölçüm eşiği aştığında.
    * [Telemetriyi dışarı aktarma](../../azure-monitor/app/export-telemetry.md) için bir [veritabanı](../../azure-monitor/app/code-sample-export-sql-stream-analytics.md) veya [Power BI için](../../azure-monitor/app/export-power-bi.md ), burada, bunu kendiniz analiz edebilirsiniz.
* *Ne sıklıkta çözümleme yapılır?*

  * Analiz günlük önceki günün (UTC saat dilimine tam gün) telemetrisi üzerinde çalıştırıyoruz.
* *Bu nedenle, bu Değiştir mu [ölçüm uyarıları](../../azure-monitor/app/alerts.md)?*
  * Hayır.  Olağan dışı düşünebilirsiniz her davranış algılama için işleme yok.


* *Yanıt olarak bir bildirim herhangi bir şey yapmaz, anımsatıcı alırım?*
  * Hayır, her sorun hakkında bir ileti yalnızca bir kez alırsınız. Sorun devam ederse dikey akışı akıllı algılama, güncelleştirilecektir.
* *Ben, e-posta kaybolur. Portalda bildirimleri nerede bulabilirim?*
  * Uygulamanızı Application Insights bakış **akıllı algılama** Döşe. Burada tüm bildirimleri ayarlama 90 gün geriye bulmak mümkün olacaktır.

## <a name="how-can-i-improve-performance"></a>Performansı nasıl geliştirebilirim?
Yavaş ve başarısız yanıtları kendi deneyiminden bildiğiniz gibi web sitesi kullanıcı için en büyük frustrations biridir. Bu nedenle, sorunları gidermek önemlidir.

### <a name="triage"></a>Önceliklendirme
İlk olarak, önemli mi? Belki de her zaman bir sayfa yavaş, ancak yalnızca %1 sitenizin kullanıcı hiç olmadığı kadar bakmak zorunda, dikkat etmeniz gereken önemli noktalar vardır. Öte yandan, %1 kullanıcı açabilirsiniz, ancak her seferinde istisnalar fırlatıyorsa yalnızca, incelemeye değer olabilir.

Etkisi ifadesi (etkilenen kullanıcılar veya trafik yüzdesi) genel bir kılavuz olarak kullanın, ancak hikayenin tamamını olmadığını unutmayın. Onaylamak için başka bir kanıt toplayın.

Sorunun parametreleri göz önünde bulundurun. Coğrafya bağlı ise, ayarlanan [kullanılabilirlik testleri](../../azure-monitor/app/monitor-web-app-availability.md) bu bölge dahil olmak üzere: yalnızca ağ ilgili sorunlar olabilir, bu alanına.

### <a name="diagnose-slow-page-loads"></a>Yavaş sayfa yüklemelerinin tanılayın
Sorun nerede? Sunucu yavaş yanıt, sayfa çok uzun veya çok görüntülemek için iş yapmak tarayıcı sahip?

Tarayıcılar ölçüm dikey penceresini açın. Tarayıcı sayfa yükleme süresi gösterir zaman nerede bulunacağını segmentli görüntüsü. 

* Varsa **isteği gönderdiğinizde** olduğundan yüksek ya da sunucu yavaş yanıt veya istek çok fazla veri içeren bir postadır. Bakmak [performans ölçümlerini](../../azure-monitor/app/web-monitor-performance.md#metrics) yanıt süreleri araştırmak için.
* Ayarlanan [bağımlılık izleme](../../azure-monitor/app/asp-net-dependencies.md) yavaşlık dış hizmetlere veya veritabanınızı nedeniyle olup olmadığına bakın.
* Varsa **yanıt alma** yaygındır, sayfanız ve bağımlı bölümleri - JavaScript, CSS, görüntü ve benzeri (ancak zaman uyumsuz olarak yüklenen veriler) uzun. Ayarlanmış bir [kullanılabilirlik testi](../../azure-monitor/app/monitor-web-app-availability.md)ve bağımlı bölümleri Yükle seçeneği ayarladığınızdan emin olun. Bazı sonuçlar elde ettiğinizde, bir sonuç ayrıntılarını açın ve farklı dosyaların yükleme sürelerini görmek için genişletin.
* Yüksek **istemci işleme süresi** betikleri yavaş çalışıyor önerir. Nedeni açık değilse, bazı zamanlama kod eklemeyi göz önünde bulundurun ve trackMetric çağrılarında gönderim zamanları.

### <a name="improve-slow-pages"></a>Yavaş sayfa geliştirin
Burada tüm yinelenecek denemez şekilde sunucu yanıtları ve sayfa yükleme sürelerinin iyileştirme önerileri, tam bir web yoktur. Burada, büyük olasılıkla hakkında düşünmek hemen başlamanızı sağlayacak bildiğiniz birkaç ipucu verilmiştir:

* Büyük dosyalar nedeniyle yavaş yükleniyor: Betikler ve diğer bölümleri zaman uyumsuz olarak yükleyin. Betik paketini kullanın. Ana sayfaya verilerini ayrı olarak yüklenen pencere öğeleri bölün. Uzun tablolar için düz eski HTML gönderme: veri, JSON ya da diğer kompakt bir biçime istemek için bir betik kullanın, sonra yerinde tablosunu doldurmak. Tüm bu konuda yardım sağlayacak harika çerçeveleri vardır. (Bunlar da büyük komut dosyaları, Elbette dahildir.)
* Yavaş sunucu bağımlılıkları: Coğrafi konumları bileşenleriniz göz önünde bulundurun. Örneğin, Azure kullanıyorsanız, web sunucusu ve veritabanı aynı bölgede olduğundan emin olun. Sorgular, ihtiyaç duydukları daha fazla bilgi almak? Önbelleğe alma veya Yardım toplu işleme musunuz?
* Kapasite sorunları: Yanıt sürelerinin server ölçümlerine bakıyor ve istek sayısı. Yanıt sürelerinin orantısız başa istek sayısını en üst seviyeye çıkarsa, sunucularınızı uzatılır olasıdır.


## <a name="server-response-time-degradation"></a>Sunucu yanıt süresinde performans düşüşü

Yanıt süresi performans düşüşü bildirim size bildirir:

* Bu işlem için normal yanıt süresi ile karşılaştırıldığında yanıt süresi.
* Kaç kullanıcının etkilendiğini.
* Ortalama yanıt süresi ve bu işlem günün algılama ve 7 gün önce 90. yüzdebirlik yanıt süresi. 
* Gün algılama ve 7 gün önce bu işlemi isteklerinin sayısı.
* Bu işlem bir düşüş ve ilgili bağımlılıklar performansındaki Düşüşler arasında bağıntı. 
* Sorunu tanılamanıza yardımcı olmak için bağlar.
  * İşlem zaman nerede harcandığını görüntülemek için Profiler izlemeleri (bağlantı Profiler izlemesi örnekleri algılama dönemi boyunca bu işlem için toplanan kullanılabilir). 
  * Ölçüm Gezgini'nde, burada dilim ve bu işlem için zaman aralığını/filtreleri zar performans raporları.
  * Bu çağrı belirli arama özelliklerini görüntülemek arama yapın.
  * Hata raporları - varsa sayısı > 1 Bu anlamına yapıldığını hataları bu işlemde bir performans düşüşüne olmuş.

## <a name="dependency-duration-degradation"></a>Bağımlılık süresi düşüşü

Modern uygulamanın daha ağır güvenilirlik için dış hizmetler hakkında müşteri adayları, çoğu durumda, mikro hizmetler tasarım yaklaşımı benimseyin. Uygulamanız bazı verileri platform veya bile dayanıyorsa gibi kendi bot hizmeti büyük olasılıkla daha insansı bir biçimde etkileşim kurmak robotlarınızın etkinleştirmek için bazı bilişsel Hizmetler Sağlayıcısı geçirir ve yanıtları Excel'den çekmek robot için bazı veri depolama hizmeti oluşturun m.  

Örnek bağımlılık performans düşüşü bildirimi:

![İşte bir örnek bağımlılık süresi düşüşü algılama](media/proactive-performance-diagnostics/dependency_duration_degradation.png)

Size bildirir dikkat edin:

* Bu işlem için normal yanıt süresi ile karşılaştırıldığında süresi
* Kaç kullanıcının etkilendiğini
* Ortalama süresi ve bu bağımlılık 90. yüzdebirlik süresinin gün algılama ve 7 gün önce
* Bağımlılık sayısı gün algılama ve 7 gün önce çağırır
* Sorunu tanılamanıza yardımcı olacak bağlantılar
  * Bu bağımlılık için ölçüm Gezgini'nde performans raporları
  * Çağrıları özelliklerini görüntülemek için bu bağımlılık çağrıları için arama yapın
  * Hata raporları - sayısı > başka bir deyişle, başarısız bağımlılık vardı 1 için süresi düşüşü katkısı olan algılama dönemi boyunca çağırırsa. 
  * Bu bağımlılık süresi ve sayısını hesaplayan sorgularla Analytics'i açın  

## <a name="smart-detection-of-slow-performing-patterns"></a>Akıllı algılama yavaş gerçekleştirme desenleri 

Application Insights, kullanıcılarınızın kısmı yalnızca etkileyen, veya bazı durumlarda kullanıcılar yalnızca etkileyen performans sorunlarını bulur. Örneğin, sayfa yükleme hakkında bildirim daha yavaş tarayıcı başka tür tarayıcıları, bir tür veya istekleri belirli bir sunucudan daha yavaş sunulur. Yavaş sayfa, belirli bir işletim sistemi kullanan istemciler için bir coğrafi alanda yükler gibi özellikleri bileşimleri ile ilişkili sorunları da bulabilir.  

Anomalileri bunlar gibi verileri inceleyerek algılamak çok zor olan, ancak, düşündüğünüzden daha yaygındır. Müşterilerinizin şikayet olduğunda genellikle bunlar yalnızca yüzey. O zamana dek çok geç: Etkilenen kullanıcılar zaten rakiplerinizin için geçiş!

Şu anda, sayfa yükleme süreleri, sunucuda istek yanıt süreleri ve bağımlılık yanıt süreleri algoritmalarınızı arayın.  

Herhangi bir eşik ayarlayın veya kuralları yapılandırmak gerekmez. Makine öğrenimi ve veri araştırma algoritmalarını anormal desenleri algılamak için kullanılır.

![E-posta uyarıdan Azure'da tanılama raporu açmak için bağlantıya tıklayın](./media/proactive-performance-diagnostics/03.png)

* **Zaman** sorunun algılandığı zaman gösterir.
* **Hangi** açıklar:

  * Algılanan Sorun;
  * Sorun davranışı bulduk olay kümesini özellikleri görüntülenir.
* Tablo, diğer tüm olayları ortalama davranışını düşük performanslı kümesiyle karşılaştırır.

Ölçüm Gezgini ve arama süresi ve yavaş gerçekleştirme kümesinin özelliklerini göre filtrelenen, ilgili raporları açmak için bağlantıları tıklatın.

Zaman aralığını ve telemetri keşfetmek için filtreleri değiştirin.

## <a name="next-steps"></a>Sonraki adımlar
Bu tanılama araçları, uygulamanızdan alınan telemetri incelemenize yardımcı:

* [Profil Oluşturucu](profiler.md) 
* [Anlık görüntü hata ayıklayıcısı](../../azure-monitor/app/snapshot-debugger.md)
* [Analizler](../../azure-monitor/log-query/get-started-portal.md)
* [Analytics akıllı tanılama](../../azure-monitor/app/analytics.md)

Akıllı algılama tamamen otomatik olarak yapılır. Ancak belki de daha fazla bazı uyarıları ayarlamak ister misiniz?

* [El ile yapılandırılan ölçüm uyarıları](../../azure-monitor/app/alerts.md)
* [Kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md)
