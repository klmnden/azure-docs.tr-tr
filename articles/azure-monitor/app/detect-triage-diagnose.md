---
title: DevOps için Azure Application Insights’a genel bakış | Microsoft Docs
description: Dev Ops ortamında Application Insights kullanmayı öğrenin.
author: mrbullwinkle
services: application-insights
documentationcenter: ''
manager: carmonm
ms.assetid: 6ccab5d4-34c4-4303-9d3b-a0f1b11e6651
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.custom: mvc
ms.topic: overview
ms.date: 09/06/2018
ms.author: mbullwin
ms.openlocfilehash: 45824ba93e86622b1bbd92aae01f18f89bee6adf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60795341"
---
# <a name="overview-of-application-insights-for-devops"></a>DevOps için Application Insights’a genel bakış

[Application Insights](../../azure-monitor/app/app-insights-overview.md) ile uygulamanızın canlıyken nasıl performans gösterdiğini ve kullanıldığını hızlıca bulabilirsiniz. Bir sorun olduğunda bunu bilmenizi sağlar, etkisini değerlendirmenize ve nedenini belirlemenize yardımcı olur.

Web uygulamaları geliştiren bir ekip şunları anlatıyor:

* *"Birkaç gün önce 'küçük' bir düzeltmeyi dağıttık. Kapsamlı bir başarı testi çalıştırmadık, ancak ne yazık ki beklenmeyen bir değişiklik yüke karışarak ön ve arka uçlar arasında uyumsuzluğa neden oldu. Anında sunucu özel durumları arttı, uyarımız tetiklendi ve durumdan haberdar edildik. Application Insights portalında birkaç tıklamada sorunu sınırlandırmak için özel durum çağrı yığınlarından yeterli bilgi bilgi aldık. Anında geri alıp hasarı sınırladık. Application Insights devops döngüsünün bu kısmını çok kolay ve işlem yapılabilir yapmış."*

Bu makalede, müşterilere hızla yanıt verir ve güncelleştirmeler yaparken Application Insights’ı nasıl kullandıklarını görmek için Fabrikam Bank’ta çevrimiçi bankacılık sistemi (OBS) geliştiren bir ekibi izliyoruz.  

Ekip, aşağıdaki çizimde gösterilen bir DevOps döngüsünde çalışıyor:

![DevOps döngüsü](./media/detect-triage-diagnose/00-devcycle.png)

Gereksinimler geliştirme kapsamlarına (görev listesi) girer. Çalışan yazılımların sık teslim edildiği, genellikle var olan uygulamanın geliştirmeleri ve uzantıları biçimindeki kısa sprintlerle çalışırlar. Dinamik uygulama sık sık yeni özelliklerle güncelleştirilir. Uygulama canlıyken ekip onun performansını ve kullanımını Application Insights’ın yardımıyla izler. Bu APM verileri kendi geliştirme kapsamlarına geri beslenir.

Ekip, Application Insights’ı kullanarak canlı Web uygulamasını şunun için yakından izler:

* Performans. Yanıt sürelerinin istek sayısıyla nasıl değiştiğini; ne kadar CPU, ağ, disk ve diğer kaynakların kullanıldığını; hangi uygulama kodunun performansı yavaşlattığını; ve performans sorunlarının nerede olduğunu bilmek isterler.
* Hatalar. Özel durumlar veya başarısız istekler varsa ya da bir performans sayacı sorunsuz bölgenin dışına çıkıyorsa önlem alabilmesi için ekibin bunu hızlı öğrenmesi gerekir.
* Kullanım. Yeni bir özellik her yayımlandığında ekip onun hangi ölçekte kullanıldığını ve kullanıcıların herhangi bir güçlük çekip çekmediğini bilmek ister.

Şimdi döngünün geri bildirim kısmına odaklanalım:

![Algıla-Önceliklendir-Tanıla](./media/detect-triage-diagnose/01-pipe1.png)

## <a name="detect-poor-availability"></a>Zayıf kullanılabilirliği algılama
Marcela Markova OBS ekibinde kıdemli bir geliştiricidir ve çevrimiçi performansı izlemeden sorumludur. Birkaç [kullanılabilirlik testi](../../azure-monitor/app/monitor-web-app-availability.md) kurar:

* Uygulamanın ana giriş sayfası için tek URL’li bir test, http://fabrikambank.com/onlinebanking/. HTTP kodu 200 ölçütlerini ve 'Hoş Geldiniz!' metnini ayarlar. Bu test başarısız olursa ağda veya sunucularda ciddi bir yanlışlık veya belki de bir dağıtım sorunu vardır. (Ya da birisi ona bilgi vermeden sayfadaki Hoş Geldiniz! mesajını değiştirmiştir.)
* Oturum açıp geçerli hesap listesini alan, her sayfada birkaç önemli ayrıntıyı denetleyen daha derin ve çok adımlı bir test. Bu test, hesapların olduğu veritabanı bağlantısının çalıştığını doğrular. Kurgusal bir müşteri kimliğini kullanır: test amacıyla böyle birkaç kimlik bulundurulmaktadır.

Bu testler ayarlandığında Marcela ekibin tüm kesintileri hızlı bir şekilde öğreneceğinden emindir.  

Web testi grafiğinde hatalar kırmızı nokta olarak görünür:

![Önceki dönem boyunca çalıştırılmış olan Web testlerinin görüntüsü](./media/detect-triage-diagnose/04-webtests.png)

Ancak daha da önemlisi, herhangi bir hata hakkındaki uyarı geliştirme ekibine e-posta ile gönderilir. Bu şekilde, neredeyse müşterilerin tümünden önce bunu öğrenirler.

## <a name="monitor-performance"></a>Performansı izleme
Application Insights’ta genel bakış sayfasında çeşitli [ana ölçümleri](../../azure-monitor/app/web-monitor-performance.md) gösteren bir grafik vardır.

![Performans KPI grafiklerine genel bakış ekran görüntüsü](./media/detect-triage-diagnose/overview-graphs.png)

Tarayıcı sayfa yüklenme süresi doğrudan Web sayfalarından gönderilen telemetriden türetilir. Sunucu yanıt süresi, sunucu istek sayısı ve başarısız istek sayılarının tümü Web sunucusunda ölçülür ve oradan Application Insights'a gönderilir.

Marcela, sunucu yanıt grafiğinden biraz endişelenmiştir. Bu grafik, sunucunun kullanıcının tarayıcısından HTTP isteği alması ile yanıt döndürmesi arasındaki ortalama zamanı gösterir. Sistemdeki yük değiştiği için bu grafikte de bir değişim görmek tuhaf değildir. Ancak buradaki durumda, isteklerin sayısındaki küçük artışlar ile yanıt süresindeki büyük artışlar arasında bir bağıntı var gibi duruyor. Bu, sistemin sınırlarda çalıştığının göstergesi olabilir.

Marcela Sunucu grafiklerini açıyor:

![Çeşitli ölçümler](./media/detect-triage-diagnose/002-servers.png)

Burada kaynak sınırlaması ile ilgili hiçbir belirti yok göründüğünden sunucu yanıt grafiklerindeki tepeler belki de yalnızca bir rastlantıdır.

## <a name="set-alerts-to-meet-goals"></a>Hedeflere ulaşmak için uyarılar ayarlama
Bununla birlikte, yanıt sürelerinden gözünü ayırmamak ister. Çok yükselirlerse bunu hemen bilmek istemektedir.

Bu nedenle, belirli bir eşikten yüksek olan yanıt süreleri için [uyarı](../../azure-monitor/app/metrics-explorer.md) ayarlar. Yanıt süreleri yavaşsa bunu öğreneceği için bu ona güven verir.

![Uyarı dikey penceresi ekleme](./media/detect-triage-diagnose/07-alerts.png)

Uyarılar, başka birçok ölçümün üzerinde ayarlanabilir. Örneğin, özel durum sayısı yükseldiğinde ya da kullanılabilir bellek düştüğünde veya istemci isteklerinde bir sıçrama olduğunda e-posta alabilirsiniz.

## <a name="stay-informed-with-smart-detection-alerts"></a>Akıllı Algılama Uyarıları ile haberdar olma
Ertesi gün, gerçekten Application Insights’tan bir uyarı e-postası ulaşır. Ancak açtığında ayarlamış olduğu yanıt süresi uyarısı olmadığını görür. Bunun yerine, başarısız isteklerde, diğer bir deyişle hata kodu 500 veya üzerini döndüren isteklerde ani bir artış olduğu söylenmektedir.

Başarısız istekler genellikle koddaki bir özel durumdan sonra oluşturulan ve kullanıcıların hatayı gördükleri yerlerdir. Belki, "Üzgünüz, bilgilerinizi şu an güncelleştiremedik" gibi bir ileti görebilirler. Ya da kesinlikle en utanç verici olanı, kullanıcının ekranında Web sunucusunun marifetiyle bir yığın dökümü görünmesidir.

Bu uyarı beklenmediktir, çünkü en son ona baktığında başarısız istek sayısı rahatlatıcı şekilde düşüktür. Meşgul bir sunucuda az sayıda hata beklenebilir.

Bu uyarıyı yapılandırmak zorunda olmadığı için bu durum onun için biraz da şaşkınlık vericidir. Application Insights, Akıllı Algılama’yı içerir. Uygulamanızın normal hata düzenine otomatik olarak uyum sağlar ve belirli bir sayfadaki, ağır yük altındaki ya da diğer ölçümlere bağlı olan hatalara "alışır". Uyarıyı yalnızca beklenegelenin üzerinde bir artış olduğunda başlatır.

![öngörülü tanılama e-postası](./media/detect-triage-diagnose/21.png)

Bu çok kullanışlı bir e-postadır. Yalnızca bir uyarı başlatmaz. Çok sayıda önceliklendirme ve tanılama işi de yapar.

Kaç müşterinin etkilendiğini ve bunların hangi Web sayfaları veya işlemleri olduğunu gösterir. Marcela ekibin tamamının yangın talimi gibi bunun üzerinde çalışması gerekip gerekmediğine veya bunun gelecek haftaya kadar göz ardı edilip edilemeyeceğine karar verebilir.

E-posta ayrıca, belirli bir özel durumun oluştuğunu ve daha da ilginci, hatanın belirli bir veritabanına yönelik başarısız çağrılarla ilişkili olduğunu gösterir. Bu durum, Marcela'nın ekibi yakın zamanda herhangi bir güncelleştirme dağıtmamış olsa da hatanın neden aniden göründüğünü açıklamaktadır.

Marcela, bu e-postaya dayanarak veritabanı ekip liderine ping atar. Son yarım saat içinde bir yama yayımladıklarını ve bakın şu işe, belki de küçük bir şema değişikliği yapılmış olabileceğini öğrenir...

Böylece, daha günlükleri bile araştırmadan ve ortaya çıktıktan sonra 15 dakika içinde sorun düzeltilme yoluna girmiştir. Ancak Marcela, Application Insights’ı açma bağlantısına tıklar. Uygulama doğrudan başarısız istekle açılır ve ilişkili bağımlılık çağrıları listesinde başarısız veritabanı çağrısını görür.

![başarısız istek](./media/detect-triage-diagnose/23.png)

## <a name="detect-exceptions"></a>Özel durumları algılama
Ufak bir kurulumla [özel durumlar](../../azure-monitor/app/asp-net-exceptions.md) otomatik olarak Application Insights'a bildirilir. Koda [TrackException()](../../azure-monitor/app/api-custom-events-metrics.md#trackexception) çağrıları ekleyerek de açık olarak yakalanabilirler:  

    var telemetry = new TelemetryClient();
    ...
    try
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string>
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send the exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }


Fabrikam Bank ekibi, kurtarma kaçınılmaz olmadığı sürece bir özel durum olduğunda her zaman telemetri gönderme uygulamasını geliştirmiştir.  

Aslında, kendi stratejisi daha geniştir: Müşteri, kod içinde bir özel duruma karşılık olmadığını ne yapmak, istedikleri engellenen olduğu her durumda telemetri gönderdikleri. Örneğin, bankalar arası dış transfer sistemi bir operasyonel nedenle (müşterinin kusuru olmadan) “bu işlem tamamlanamıyor” mesajı veriyorsa o olayı izlerler.

    var successCode = AttemptTransfer(transferAmount, ...);
    if (successCode < 0)
    {
       var properties = new Dictionary <string, string>
            {{ "Code", returnCode, ... }};
       var measurements = new Dictionary <string, double>
         {{"Value", transferAmount}};
       telemetry.TrackEvent("transfer failed", properties, measurements);
    }

Yığının bir kopyasını gönderdiğinden özel durumları bildirmek için TrackException kullanılır. Diğer olayları bildirmek için de TrackEvent kullanılır. Tanılama aşamasında yararlı olabilecek herhangi bir özelliği iliştirebilirsiniz.

Özel durumlar ve olaylar [Tanılama Araması](diagnostic-search.md) dikey penceresinde gösterilir. Ek özellikleri ve yığın izlemesini görmek için bunlarda ayrıntıya inebilirsiniz.

![Tanılama Araması'nda, belirli veri türlerini göstermek için filtreleri kullanın.](./media/detect-triage-diagnose/appinsights-333facets.png)


## <a name="monitor-proactively"></a>Proaktif izleme
Marcela, öyle oturup uyarıları beklemez. Her dağıtımdan hemen sonra [yanıt sürelerini](../../azure-monitor/app/web-monitor-performance.md) inceler; özel durum sayılarının yanı sıra hem genel rakamlara hem de en yavaş istekler tablosuna bakar.  

![Yanıt süresi grafiği ve sunucu yanıt süreleri kılavuzu.](./media/detect-triage-diagnose/response-time.png)

Genellikle her haftayı bir önceki ile karşılaştırarak her dağıtımın performans üzerindeki etkisini değerlendirebilir. Ani bir kötüleşme olduğunda durumu ilgili geliştiricilere iletir.

## <a name="triage-issues"></a>Önceliklendirme sorunları
Önceliklendirme, yani bir sorunun önem derecesini ve kapsamını değerlendirmek algılamadan sonraki ilk adımdır. Ekibe gece yarısı çağrı yapmalı mıyız? Yoksa kapsamdaki sonraki uygun boşluğa kadar bekletilebilir mi? Önceliklendirmede bazı temel sorular vardır.

Bu ne sıklıkta oluyor? Genel Bakış dikey penceresindeki grafikler soruna bazı bakış açıları verir. Örneğin, Fabrikam uygulaması bir gecede dört Web test uyarısı oluşturmuştur. Ekip sabah grafiğe baktığında testlerin çoğu yeşil olsa da hala bazı kırmızı noktalar olduğunu görebilir. Kullanılabilirlik grafiğinde ayrıntılara inildiğinde aralıklı olarak ortaya çıkan bu sorunların tümünün tek bir test konumundan geldiği açıktır. Bunun yalnızca bir yolu etkileyen bir ağ sorunu olduğu ve büyük olasılıkla kendi kendine temizleneceği açıktır.  

Aksine, özel durum sayıları ve yanıt sürelerinin grafiğindeki çarpıcı ve kararlı artış, kuşkusuz endişelenilmesi gereken bir şeydir.

Yararlı bir önceliklendirme taktiği Önce Kendinizin Denemesidir. Aynı sorunla karşılaştıysanız gerçek olduğunu bilirsiniz.

Kullanıcıların kaçta kaçı etkilendi? Kaba bir yanıt almak için hata oranını oturum sayısına bölün.

Yavaş yanıtlar olduğunda, en yavaş yanıtlanan istekler tablosunu her sayfanın kullanım sıklığı ile karşılaştırın.

Engellenen senaryo ne kadar önemli? Bu, belirli bir kullanıcı hikayesine engel olan işlevsel bir sorunsa çok önemli midir? Müşteriler, faturalarını ödeyemiyorsa bu ciddidir; ekran rengi tercihlerini değiştiremiyorsa belki de bekleyebilir. Olayın ya da özel durumun ayrıntısı veya yavaş sayfanın kimliği, müşterilerin nerede sorun yaşadığını size söyler.

## <a name="diagnose-issues"></a>Sorunları tanılama
Tanılama hata ayıklama ile pek aynı değildir. Kodu izlemeye başlamadan önce sorunun niçin, nerede ve ne zaman oluştuğu üzerine kaba bir fikriniz olmalıdır.

**Ne zaman oluyor?** Olay ve ölçüm grafikleri tarafından sağlanan geçmiş görünümü, etkileri olası nedenlerle ilişkilendirmeyi kolaylaştırır. Yanıt süresinde veya özel durum oranlarında aralıklı yükselmeler oluyorsa istek sayısına bakın: aynı zamanda tepe yapıyorsa bu bir kaynak sorunu olabilir. Daha fazla CPU mu yoksa bellek mi atamanız gerekiyor? Yoksa bu, yükü yönetemeyen bir bağımlılık mı?

**Sorun biz miyiz?**  Belirli bir istek türünün performansında ani düşme ile karşılaşıyorsanız (örneğin, müşteri hesap özeti istediğinde) sorun Web uygulamanızda değil bir dış alt sistemde olabilir. Ölçüm Gezgini’nde Bağımlılık Hatası ve Bağımlılık Süresi oranlarını seçin ve son birkaç saatlik ya da günlük geçmişlerini algıladığınız sorunla karşılaştırın. İlişkili değişiklikler varsa bir dış alt sistem suçlanabilir.  


![Bağımlılık hatası ve bağımlılıklara yapılan çağrıların süresi grafikleri](./media/detect-triage-diagnose/11-dependencies.png)

Bazı yavaş bağımlılık sorunları coğrafi konum sorunlarıdır. Fabrikam Bank, Azure sanal makinelerini kullanmaktadır ve Web sunucuları ile hesap sunucularının yanlışlıkla farklı ülkelerde konumlandırıldığı keşfedilir. Bunlardan biri geçirildiğinde çarpıcı bir iyileşme yaşanmıştır.

**Biz ne yaptık?** Sorun bağımlılıkta değil görünüyorsa ve hiçbir zaman da olmadıysa büyük olasılıkla buna son zamanlarda yapılan bir değişiklik neden olmuştur. Ölçüm ve olay grafikleri tarafından sağlanan geçmiş perspektif, tüm ani değişiklikleri dağıtımlarla ilişkilendirmeyi kolaylaştırır. Bu durum, sorunun aranacağı yeri daraltır. Uygulama kodundaki hangi satırların performansı yavaşlattığını belirlemek için Application Insights Profiler’ı etkinleştirin. Lütfen [Application Insights ile canlı Azure Web uygulamalarının profilini oluşturma](./../../azure-monitor/app/profiler.md) sayfasına bakın. Profiler etkinleştirildikten sonra, aşağıdakine benzer bir izleme görürsünüz. Bu örnekte, soruna *GetStorageTableData* yönteminin neden olduğu kolayca görülebilir.  

![App Insights Profiler İzlemesi](./media/detect-triage-diagnose/AppInsightsProfiler.png)

**Neler oluyor?** Bazı sorunlar nadiren oluşur ve çevrimdışı test ederken izlenmeleri güç olabilir. Tek yapabileceğimiz hatayı canlı oluştuğu sırada yakalamaya çalışmaktır. Özel durum raporlarındaki yığın dökümlerini inceleyebilirsiniz. Ayrıca, ister sık kullandığınız günlük çerçevesi ile ister TrackTrace() ya da TrackEvent() ile izleme çağrıları yazabilirsiniz.  

Fabrikam hesaplar arası aktarımlarda zaman zaman ortaya çıkan ancak yalnızca belirli hesap türlerinde olan bir sorunla karşılaştı. Neler olduğunu daha iyi anlamak için hesap türünü her çağrıya bir özellik olarak iliştirerek koddaki anahtar noktalara TrackTrace() çağrıları eklediler. Bunu yapmak, yalnızca Tanılama Aramasındaki izlemeleri filtrelemeyi kolaylaştırdı. Ayrıca parametre değerlerini izleme çağrılarına özellikler ve ölçümler olarak iliştirdiler.

## <a name="respond-to-discovered-issues"></a>Bulunan sorunlara çözüm bulma
Soruna tanı koyduktan sonra düzeltmek için bir plan yapabilirsiniz. Belki son zamanlarda yapılan bir değişikliği geri almanız gerekiyordur veya belki de yalnızca devam edip düzeltebilirsiniz. Düzeltme yapıldıktan sonra Application Insights başarılı olup olmadığınızı bildirir.  

Fabrikam Bank'ın geliştirme ekibi, performans ölçümü için Application Insights kullanmazdan öncesine göre daha fazla yapılandırılmış bir yaklaşımı benimser.

* Performans hedeflerini Application Insights genel bakış sayfasındaki belirli ölçümlere göre ayarlarlar.
* 'Huniler' aracılığıyla kullanıcının ilerlemesini ölçen ölçümler gibi performans ölçümlerini, uygulamaya ilk başından tasarımdan yerleştirirler.  

## <a name="monitor-user-activity"></a>Kullanıcı etkinliğini izleme
Yanıt süresi tutarlı bir şekilde iyiyken birkaç özel durum olduğunda geliştirme ekibi kullanılabilirlik aşamasına geçebilir. Kullanıcıların deneyimini nasıl iyileştirecekleri ve istenilen hedeflere ulaşmak için daha fazla kullanıcıyı nasıl teşvik edecekleri hakkında düşünebilirler.

Application Insights, kullanıcıların bir uygulama ile neler yaptıklarını öğrenmek için de kullanılabilir. Sorunsuz çalışmaya başladıktan sonra ise ekip hangi özelliklerin en popüler olduğunu, hangi kullanıcıların hoşuna gidip hangilerinin zorluk çektiğini ve ne sıklıkta geri döndüklerini öğrenmek ister. Bu onların sonraki işlerini önceliklendirmelerine yardımcı olur. Ayrıca, geliştirme döngüsü bir parçası olarak her özelliğin başarısını ölçmeyi planlayabilirler.

Örneğin, Web sitesinde tipik bir kullanıcı gezintisinde belirgin bir "huni" vardır. Birçok müşteri, farklı türlerdeki kredi oranlarına bakar. Daha küçük bir grup da devam ederek teklif formunu doldurur. Teklif alanlardan birkaçı devam eder ve krediyi alır.

![Sayfa görüntüleme sayıları](./media/detect-triage-diagnose/funnel.png)

En çok müşterinin nerede çıktığını değerlendiren işletme, daha fazla kullanıcıyı huninin altına kadar nasıl çekeceği üzerine çalışabilir. Bazı durumlarda, bir kullanıcı deneyimi (UX) hatası olabilir; örneğin, 'İleri' düğmesini bulmak zordur veya yönergeler açık değildir. Büyük olasılıkla, çıkma nedenleri belirgin bir şekilde işle ilgili nedenlerdir: belki de kredi oranları çok yüksektir.

Nedenler ne olursa olsun, veriler ekibin kullanıcıların neler yaptığını ortaya çıkarmasına yardımcı olur. Daha fazla ayrıntı ortaya çıkarmak için daha çok izleme çağrısı eklenebilir. Her bir düğmeye tıklanması gibi en ince ayrıntısından kredinin ödenmesi gibi önemli başarılara kadar tüm kullanıcı eylemlerini saymak için TrackEvent() kullanılabilir.

Ekip kullanıcı etkinliği ile ilgili bilgilere ulaşmaya alışıyor. Günümüzde, yeni bir özelliği her tasarladıklarında, kullanımı hakkında geri bildirimi nasıl alacakları üzerine çalışıyorlar. İzleme çağrılarını daha en başından özelliğin içinde tasarlıyorlar. Her geliştirme döngüsünde özelliği geliştirmek için bu geri bildirimleri kullanıyorlar.

[Kullanımı izleme hakkında daha fazla bilgi edinin](../../azure-monitor/app/usage-overview.md).

## <a name="apply-the-devops-cycle"></a>DevOps döngüsünü uygulama
Bir ekip Application Insights’ı sadece bağımsız sorunları düzeltmek için değil aynı zamanda geliştirme yaşam döngülerini de iyileştirmek için işte böyle kullanır. Umarım, bu Application Insights’ın kendi uygulamalarınızdaki uygulama performans yönetiminde size nasıl yardımcı olabileceği hakkında bazı fikirler vermiştir.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanızın özelliklerine bağlı olarak çeşitli şekillerde başlayabilirsiniz. Size en çok uyanı seçin:

* [ASP.NET Web uygulaması](../../azure-monitor/app/asp-net.md)
* [Java Web uygulaması](../../azure-monitor/app/java-get-started.md)
* [Node.js Web uygulaması](../../azure-monitor/app/nodejs.md)
* Zaten dağıtılmış uygulamalar üzerinde barındırılan [IIS](../../azure-monitor/app/monitor-web-app-availability.md)
* [Azure](../../azure-monitor/app/app-insights-overview.md).
* [Web sayfaları](../../azure-monitor/app/javascript.md) -Tek Sayfalık Uygulama veya normal Web sayfası - bunu kendi başına veya sunucu seçeneklerinden herhangi biriyle birlikte kullanın.
* Uygulamanızı genel İnternet'ten test etmek için [kullanılabilirlik testleri](../../azure-monitor/app/monitor-web-app-availability.md).
