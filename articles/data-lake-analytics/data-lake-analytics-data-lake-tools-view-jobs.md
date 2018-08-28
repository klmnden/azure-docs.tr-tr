---
title: Azure Data Lake Analytics işleri için iş tarayıcı ve iş görünümünü kullanma
description: Bu makalede, Azure Data Lake Analytics işleri için iş tarayıcı ve iş görünümünü kullanmayı açıklar.
services: data-lake-analytics
ms.service: data-lake-analytics
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonwhowell
ms.assetid: bdf27b4d-6f58-4093-ab83-4fa3a99b5650
ms.topic: conceptual
ms.date: 08/02/2017
ms.openlocfilehash: 474478c8049dd97558b49b1df4b00655268fc0b3
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43044107"
---
# <a name="use-job-browser-and-job-view-for-azure-data-lake-analytics"></a>İş tarayıcı ve iş görünümünü kullanma Azure Data Lake Analytics için
Azure Data Lake Analytics hizmetine gönderilen işler arşivleri bir [query store'u](#query-store). Bu makalede, iş tarayıcı ve iş görünümünü Visual Studio için Azure Data Lake araçları geçmiş iş bilgilerini bulmak için nasıl kullanılacağını öğrenin. 

Varsayılan olarak, Data Lake Analytics hizmetine 30 gün boyunca işleri arşivler. Sona erme süresi, özelleştirilmiş süresi dolma ilkesini yapılandırarak, Azure portalından yapılandırılabilir. Dolduktan sonra iş bilgilerine erişmek mümkün olmayacaktır. 

## <a name="prerequisites"></a>Önkoşullar
Bkz: [Önkoşullar Visual Studio için Data Lake Araçları](data-lake-analytics-data-lake-tools-get-started.md#prerequisites).

## <a name="open-the-job-browser"></a>İş tarayıcısı açın
İş tarayıcı üzerinden erişim **Sunucu Gezgini > Azure > Data Lake Analytics > işler** Visual Studio'da.  İş tarayıcı kullanarak bir Data Lake Analytics hesabının query store erişebilir. İş tarayıcı Query Store sol tarafta, temel iş bilgilerini gösterir ve iş görünümünde doğru gösteren ayrıntılı iş bilgileri.

## <a name="job-view"></a>İş Görünümü
İş, bir işin ayrıntılı bilgileri görüntüler. Bir işi'ni açmak için bir iş işi tarayıcıda çift tıklayın veya iş görünümü tıklayarak Data Lake Menüsü'nden açın. İş URL'si ile doldurulmuş bir iletişim kutusu görmeniz gerekir.

![Visual Studio iş tarayıcı Data Lake araçları](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view.png)

İş görünümü içerir:

* İş Özeti
  
    Çalışan iş sayısının daha yeni bilgileri görmek için iş görünümünü yenileyin.
  
  * İş durumu (graf):
    
      İş durumu iş aşamaları ana hatlarıyla gösterir:
    
      ![Azure Data Lake Analytics işi aşamaları durumu](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-phases.png)
    
    * Hazırlama: betiğinizi derleme ve derleme hizmeti kullanılarak betik en iyi duruma getirme buluta yükleyin.
    * Sıraya alınan: İşleri için yeterli kaynak bekleyen veya hesap sınırlaması başına en fazla eşzamanlı iş işleri aşan sıralanır. Sıraya alınan işler - bir dizi, düşük öncelik ayarı belirler sayısı, öncelik o kadar yüksektir.
    * Çalıştırılıyor: İş gerçekten Data Lake Analytics hesabınız çalışıyor.
    * Sonlandırılıyor: İş (örneğin dosyayı sonlandırılıyor) tamamlıyor.
      
      İşin her aşamasında başarısız olabilir. Örneğin, derleme hataları hazırlama aşamasında, kuyruğa alınmış aşamasında zaman aşımı hatalarını ve çalışan aşama, vb. yürütme hataları.
  * Temel Bilgiler
    
      İş özeti paneli alt bölümünde temel iş bilgilerini gösterir.
    
      ![Azure Data Lake Analytics işi aşamaları durumu](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-info.png)
    
    * İş sonucu: Başarılı veya başarısız oldu. İşin her aşamasında başarısız olabilir.
    * Toplam süre: gönderme zamanı ve bitiş saati arasında saatinden (süre) Wall.
    * Toplam işlem süresi: Her köşe yürütme zamanı toplamı, bunu yalnızca bir köşe yürütülen iş saati olarak düşünebilirsiniz. Köşe hakkında daha fazla bilgi için toplam köşe bakın.
    * Gönder/başlangıç/bitiş zamanı: Data Lake Analytics hizmetine iş gönderme/iş/uçları başarılı ya da başarısız işi çalıştırmak için başlatır aldığında süre.
    * Derleme/sıraya alınan/çalıştırılıyor: Duvar saati süresi kuyruğa alınmış/hazırlama/çalıştırma aşamasında ayırıyor.
    * Hesap: Data Lake Analytics hesabı işin çalıştırılması için kullanılır.
    * Yazar: işi gönderen kullanıcı gerçek bir kişinin hesabını veya bir sistem hesabı olabilir.
    * Önceliği: İşin önceliği. Alt sayısı, öncelik o kadar yüksektir. Yalnızca bir dizi kuyruğundaki iş etkiler. Daha yüksek bir öncelik ayarı çalışan işleri etkisiz hale değil.
    * Paralellik: İstenen en fazla sayıda eş zamanlı Azure veri Lake Analytics birimi (ADLAUs) olarak da bilinir köşeler. Şu anda altı GB RAM ve iki sanal çekirdekli bir VM ile bir köşe eşittir, Data Lake Analytics bu yükseltilip ancak gelecekte güncelleştirir.
    * Kalan bayt: Bayt, iş tamamlanana kadar işlenmesi gerekir.
    * Okunan/yazılan bayt: çalışmaya başladığı işinden bu yana, okunan/yazılan bayt.
    * Toplam köşeler: iş çok parçalı bir işin ayrılır, her parça işin bir köşe çağrılır. Bu değer, kaç parçalı iş oluşan bir işin açıklar. Temel işlem birimi, başka bir deyişle Azure Data Lake Analytics birimi (ADLAU), bir köşe düşünebilirsiniz ve köşeler paralellik içinde çalıştırılabilir. 
    * Tamamlanan/çalışan/başarısız oldu: Tamamlandı/çalışan/başarısız köşe sayısı. Köşe her iki kullanıcı kodunu ve sistem hataları nedeniyle başarısız olabilir, ancak sistem yeniden deneme köşeler otomatik olarak birkaç kez başarısız oldu. Köşe denedikten sonra hala başarısız olursa tüm işi başarısız olur.
* İş Grafiği
  
    U-SQL betiği, veri çıkışı giriş veri dönüştürme mantığını temsil eder. Betik derlenmiş ve hazırlama aşamasında, bir fiziksel yürütme planı için en iyi duruma getirilmiş. İş grafiği fiziksel yürütme planını göstermektir.  Aşağıdaki diyagram işlem gösterilmektedir:
  
    ![Azure Data Lake Analytics işi aşamaları durumu](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-logical-to-physical-plan.png)
  
    Bir iş, pek çok parçalı bir işin ayrılır. Her parça işin bir köşe adı verilir. Köşe Süper köşe (aşama olarak da bilinir) gruplandırılır ve iş grafiği görselleştirilir. İş grafiği, yeşil aşama placards aşamalarını gösterir.
  
    Her köşe bir aşamadaki aynı türde bir iş ile aynı verileri farklı parçalarını yapıyor. Örneğin, sahip olduğunuz bir TB veri dosyasıyla ve bundan okuma köşeler yüzlerce, bunların her biri bir öbek okunuyor. Bu köşeler aynı Aşama ve bunun yapılması farklı parçalarını aynı giriş dosyası üzerinde çalışma gruplandırılır.
  
  * <a name="state-information"></a>Aşama bilgileri
    
      Belirli bir aşamasında bazı sayılar afişinde gösterilmektedir.
    
      ![Azure Data Lake Analytics işi graf aşaması](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage.png)
    
    * SV1 Ayıklayın: bir sayı ve işlem yöntemi ile adlı bir aşama, adı.
    * 84 köşeler: Bu aşamada köşe toplam sayısı. Şekil, kaç tane işin parçaları, bu aşamada ayrılmıştır gösterir.
    * 12.90 s/köşe: Bu aşamada ortalama köşe yürütme süresi. Bu şekil toplama (her köşe yürütme süresi) göre hesaplanır / (toplam köşe sayısı). Yani paralellik içinde yürütülen tüm köşeleri atayabilir, tüm aşama tamamlandı 12.90 s. Ayrıca bu aşamadaki tüm işler seri olarak yapıldıysa maliyeti olur #vertices geldiğini * ortalama süresi.
    * yazılan 850,895 satır: Bu aşamada yazılan satır sayısı toplam sayısı.
    * Bu aşamada bayt okuma/yazılan veri miktarı R/w.
    * Renkler: Renkleri aşamasında farklı köşe durumu belirtmek için kullanılır.
      
      * Yeşil, köşe başarılı gösterir.
      * Turuncu, köşe denenen gösterir. Yeniden denenen köşe başarısız oldu, ancak sistem tarafından otomatik olarak ve başarıyla yeniden denendi ve genel aşaması başarıyla tamamlandı. Köşe denenen ancak yine de başarısız oldu, rengi kırmızı ve tüm işi başarısız oldu kapatır.
      * Kırmızı başarısız, gösterir belirli bir köşe anlamına gelir sistem tarafından birkaç kez yeniden denendi ancak yine de başarısız oldu. Bu senaryo, tüm işinin başarısız olmasına neden olur.
      * Belirli bir köşe çalışıyor mavi anlamına gelir.
      * Beyaz gösterir köşe bekliyor. Köşe bir ADLAU kullanılabilir hale gelir ve giriş verilerini hazır olmayabilir olduğundan, giriş bekleniyor sonra zamanlanmış bekliyor olabilir.
      
      Fare imleci bir duruma göre gelerek aşama için daha fazla ayrıntı bulabilirsiniz:
      
      ![Azure Data Lake Analytics işi graf aşama ayrıntıları](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage-details.png)
  * Köşeler: Örneğin, kaç köşeler tamamladınız, toplamda kaç köşe başarısız oldu veya hala çalışan/bekliyor, vb. olan köşe ayrıntılarını açıklar.
  * Veri pod arası/içi okuma: dosyaları ve verileri depolanmış olan birden çok pod'ların içinde dağıtılmış dosya sistemi. Buradaki değer ne kadar veri aynı pod'u ya da pod çapraz Okunmuş açıklar.
  * Toplam işlem süresi: zaman ayırdığınız tüm aşamasında çalışıyorsanız yalnızca bir köşe yürütülür gibi her bir aşamada köşe yürütme süresi toplamı, bunu göz önünde bulundurun.
  * Veri ve satırları yazılan/okuma: ne kadar veri veya satırları okunan/yazılan veya okunması gerektiğini gösterir.
  * Köşe okuma hataları: kaç köşeler veri okuma sırasında başarısız olan açıklar.
  * Köşe kopyası atma işlemleri: bir köşe çok yavaş çalışıyorsa, sistem aynı parça işin çalıştırmak için birden fazla köşe zamanlama. Köşe birini tamamladıktan sonra başarıyla reductant köşeler atılacak. Yinelen köşe atma yinelenen aşamasında atılır köşeler sayısını kaydeder.
  * Köşe geri alma işlemleri: köşe başarıyla gerçekleştirildi, ancak bazı nedenlerden dolayı daha sonra yeniden. Örneğin, aşağı akış köşe Ara girdi verilerini kaybederse, tekrar çalıştırmak için Yukarı Akış köşe sorar.
  * Köşe zamanlama yürütmeleri: zamanlanan köşeler toplam süre.
  * Okunan en az/ortalama/en fazla okunan köşe verileri: en az/ortalama/en fazla her köşe verilerini okuyun.
  * Bir aşama süresi: Duvar saati süresi alır, bu değeri görmek için profili yüklemek gerekir.
  * İş Kayıttan Yürütme
    
      Data Lake Analytics işleri çalıştırır ve işlerin bilgileri gibi çalışan köşeler arşivleri köşeler, durduruldu, başlatıldığında başarısız oldu ve nasıl bunlar yeniden denenir. vs. Tüm bilgileri otomatik olarak sorgu Mağazası'nda oturum ve kendi iş profilinde depolanır. İş profili "Profilini Yükle" iş görünümünde aracılığıyla indirebilir ve iş profili indirdikten sonra iş kayıttan yürütme görüntüleyebilirsiniz.
    
      İş kayıttan yürütme, kümede ne bir epitome görselleştirme olur. İş yürütme ilerleme durumunu izleyin ve performans anormalliklerini ve sorunlarını çok kısa bir süre içinde (küçüktür 30 saniye genellikle) kullanıma görsel olarak algılayan yardımcı olur.
  * İş ısı Haritası görüntüleme 
    
      İş ısı Haritası görünen açılır menüde iş grafiği aracılığıyla seçilebilir. 
    
      ![Azure Data Lake Analytics işi graf yığın harita görünümü](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-display.png)
    
      Bu, g/ç, süresi ve aktarım hızı ısı Haritası üzerinden burada işin çoğu zaman harcadığını veya işinizin bir g/ç sınırı işi olup bulabilir ve benzeri bir işin gösterir.
    
      ![Azure Data Lake Analytics işi graf yığın harita örneği](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-example.png)
    
    * İlerleme: İş yürütme ilerleme, bilgilerini [aşama bilgileri](#stage-information).
    * Okunan/yazılan veri: toplam veri her aşamada okunan/yazılan ısı haritası.
    * İşlem süresi: tüm iş aşamasında ile yalnızca 1 köşe yürütülürse uzun sürecektir nasıl ısı Haritası toplamın (her köşe yürütme süresi), bu göz önünde bulundurun.
    * Düğüm başına ortalama yürütme süresi: ısı Haritası toplamın (her köşe yürütme süresi) / (köşe sayı). Paralellik içinde yürütülen tüm köşeleri atayabilir, bu zaman çerçevesi tüm aşama yapılmayacak anlamına gelir.
    * Giriş/Çıkış aktarım hızı: giriş/çıkış aktarım hızı, her bir aşamaya ısı haritasını, işinizin bir g/ç ilişkili işi bu olup olmadığını doğrulayabilirsiniz.
* Meta Veri İşlemleri
  
    U-SQL komut dosyanıza bazı meta veri işlemleri, aşağıdaki gibi bir veritabanı oluşturmak, drop table, vb. Bu işlemler sonra derleme meta veri işlemi gösterilmektedir. Onaylamalar Bul, varlık oluşturabilir, varlıkları buraya bırakın.
  
    ![Azure Data Lake Analytics işi görünümü meta veri işlemleri](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-metadata-operations.png)
* Durum Geçmişi
  
    Durum geçmişi de iş özetinde görselleştirilir, ancak burada daha fazla ayrıntıya ulaşabilirsiniz. İş, kuyruğa alınmış hazırlandığında başlatılan çalışması sona erdi gibi ayrıntılı bilgileri bulabilirsiniz. Ayrıca iş derlenmiş kaç kez bulabilirsiniz (CcsAttempts: 1), ne zaman iş gönderilen kümeye gerçekte (ayrıntı: kümeye işi gönderdikten), vb.
  
    ![Azure Data Lake Analytics işi görünümü durumu geçmişi](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-state-history.png)
* Tanılama
  
    Araç, iş yürütmeyi otomatik olarak tanılar. Bazı hatalar veya işlerinizdeki performans sorunları olduğunda uyarı alırsınız. Tam bilgi almak için profil indirme gerektiğini unutmayın. 
  
    ![Azure Data Lake Analytics işi görünümü tanılama](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-diagnostics.png)
  
  * Uyarı: Bir uyarı burada derleyici uyarı ile gösterilir. Uyarı göründükten sonra daha fazla ayrıntı sağlamak için "x sorunu/sorunları" bağlantısına tıklayabilirsiniz.
  * Köşe çok uzun çalıştırın: herhangi bir köşe dışında (örneğin 5 saat) çalıştığında, sorunları burada bulunur.
  * Kaynak Kullanımı: gereksinim duyduğunuzdan daha fazla veya yeterli paralellik ayırdığınızda, sorunları burada bulunur. Daha fazla ayrıntı görmek ve daha iyi bir kaynak ayırma bulmak için senaryolarını gerçekleştirmek için kaynak kullanımı da tıklayabilirsiniz (daha fazla ayrıntı için bu Kılavuzu'na bakın).
  * Bellek denetimi: tüm köşe 5 GB'den fazla bellek kullanıyorsa, sorunları burada bulunur. Sistemi kısıtlaması daha fazla bellek kullanıyorsa, iş yürütme sistemi tarafından sonlandırıldı.

## <a name="job-detail"></a>İş Ayrıntısı
İş ayrıntısı betik, kaynaklar ve köşe yürütme görünümü de dahil olmak üzere, işin ayrıntılı bilgileri gösterir.

![Azure Data Lake Analytics iş ayrıntısı](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-details.png)

* Betik
  
    U-SQL betiği iş sorgu Mağazası'nda depolanır. Özgün U-SQL betiği görüntüleyin ve gerekirse yeniden gönderin.
* Kaynaklar
  
    Kaynakları aracılığıyla query store depolanan iş derleme çıktıları bulabilirsiniz. Örneğin, "iş grafiği göstermek için kullanılan algebra.xml", kayıtlı derlemeleri, vb. burada bulabilirsiniz.
* Köşe yürütme görünümü
  
    Bu, köşe yürütme ayrıntıları gösterir. İş profili toplam veri okunan/yazılan, çalışma zamanı, durum, vb. gibi her köşe yürütme günlüğü arşivler. Bu görünüm nasıl bir işin çalışma hakkında daha fazla bilgi alabilirsiniz. Daha fazla bilgi için [Visual Studio için Data Lake araçlarına köşe yürütme görünümünü kullanma](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).

## <a name="next-steps"></a>Sonraki Adımlar
* Tanılama bilgilerini günlüğe kaydetmek için bkz. [Azure Data Lake Analytics için tanılama günlüklerine erişme](data-lake-analytics-diagnostic-logs.md)
* Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.
* Köşe yürütme görünümünü kullanma hakkında bilgi için bkz: [Visual Studio için Data Lake araçlarına köşe yürütme görünümünü kullanma](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)

