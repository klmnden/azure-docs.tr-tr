---
title: "Azure Data Lake Analytics işleri için iş tarayıcı ve iş görünümünde kullanın | Microsoft Docs"
description: "Azure Data Lake Analytics işleri için iş tarayıcı ve iş görünümünde kullanmayı öğrenin. "
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: bdf27b4d-6f58-4093-ab83-4fa3a99b5650
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/02/2017
ms.author: jgao
ms.openlocfilehash: 8f1729f84a4fde2a56427a41b356d6263818519e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-job-browser-and-job-view-for-azure-data-lake-analytics-jobs"></a>Azure Data lake Analytics işleri için iş tarayıcı ve iş görünümünde kullanın
Azure Data Lake Analytics hizmeti gönderilen işler arşivler bir [sorgu deposu](#query-store). Bu makalede, iş tarayıcı ve iş görünümünde Visual Studio için Azure Data Lake araçları içinde geçmiş işi bilgileri bulmak için nasıl kullanılacağını öğrenin. 

Varsayılan olarak, Data Lake Analytics hizmeti 30 gün boyunca işleri arşivler. Sona erme süresi özelleştirilmiş süre sonu ilkesi yapılandırarak Azure portalından yapılandırılabilir. İş bilgileri dolduktan sonra erişebilir olmaz. 

## <a name="prerequisites"></a>Ön koşullar
Bkz: [Visual Studio Önkoşullar için Data Lake Araçları](data-lake-analytics-data-lake-tools-get-started.md#prerequisites).

## <a name="open-the-job-browser"></a>İş tarayıcıda aç
İş tarayıcı yoluyla erişim **Sunucu Gezgini > Azure > Data Lake Analytics > işleri** Visual Studio.  İş tarayıcı kullanarak, bir Data Lake Analytics hesabı sorgu deposu erişebilir. İş tarayıcı Query Store sol tarafta, temel iş bilgilerini gösterir ve iş görünümünde sağ gösteren ayrıntılı iş bilgi.

## <a name="job-view"></a>İşi görüntüle
İş, bir işin ayrıntılı bilgileri görüntüler. Bir işi'ni açmak için bir iş iş tarayıcıda çift tıklatın veya iş görünümünde tıklayarak Data Lake menüsünü açın. Proje URL'si ile doldurulan bir iletişim kutusu görmeniz gerekir.

![Visual Studio Proje tarayıcı Data Lake araçları](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view.png)

İş görünümünde içerir:

* İş Özeti
  
    İş çalışan işi daha yeni bilgileri görmek için görünümü yenileyin.
  
  * İş durumu (grafiği):
    
      İş durumu proje aşamalarını özetlenmektedir:
    
      ![Azure Data Lake Analytics işi aşamaları durumu](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-phases.png)
    
    * Hazırlama: derleme ve derleme hizmetini kullanarak betik en iyi duruma getirme buluta komut dosyanızı karşıya yükleyin.
    * Sıraya: Yeterli kaynağı için bekleyen sıraya alınan whey olan işleri ya da hesap sınırlaması başına en fazla eşzamanlı iş işleri aşan. Düşük öncelik ayarı sıraya alınan işleri - sırasını belirler sayısı, öncelik o kadar yüksektir.
    * Çalışan: İşi aslında Data Lake Analytics hesabınızı çalışıyor.
    * Sonlandırılıyor: İşi (örneğin dosya sonlandırılıyor) üzeredir.
      
      İş her aşamasında başarısız olabilir. Örneğin, derleme hataları hazırlama aşamasında, zaman aşımı hataları sıraya alınan aşamasında ve çalışan aşaması, vb. yürütme hataları.
  * Temel bilgiler
    
      İş özeti panelinin alt bölümünde temel iş bilgilerini gösterir.
    
      ![Azure Data Lake Analytics işi aşamaları durumu](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-info.png)
    
    * İş sonuç: Başarılı veya başarısız oldu. İş her aşamasında başarısız olabilir.
    * Toplam süre: saat gönderiliyor ve bitiş saati arasındaki saatin (süresi) duvar.
    * Toplam işlem süresi: Her köşe yürütme süresi toplamı, bu iş yalnızca bir köşe yürütülür süresi olarak düşünebilirsiniz. Köşe hakkında daha fazla bilgi bulmak için toplam köşeleri bakın.
    * Gönder/başlangıç/bitiş saati: zaman zaman iş gönderme/başlatır, iş/uçları işi başarılı bir şekilde çalıştırmak için Data Lake Analytics hizmeti alır.
    * Derleme/sıraya alınan/çalışıyor: Duvar saati süresi sıraya alınan/hazırlama/çalışan aşamasında harcanan.
    * Hesap: Data Lake Analytics hesabı işi çalıştırmak için kullanılır.
    * Yazar: işin gönderildiği kullanıcı gerçek bir kişinin hesap veya bir sistem hesabı olabilir.
    * Öncelik: İşin önceliği. Alt sayısı, öncelik o kadar yüksektir. Yalnızca bir dizi sırasındaki işleri de etkiler. Daha yüksek öncelik ayarı çalışan işlerin önüne geçer değil.
    * Paralellik: İstenen en fazla sayıda eşzamanlı Azure Data Lake Analytics birimleri (ADLAUs), diğer adıyla köşeleri. Şu anda bir köşesinin altı GB RAM ve iki sanal çekirdek ile bir VM ile eşit olan, bu yükseltme ancak Data Lake Analytics gelecekte güncelleştirir.
    * Bayt sol: iş tamamlanana kadar işlenmesi için gereken bayt sayısı.
    * Okuma ve yazılan bayt: çalışan iş, bu yana okunan ve yazılan silinmiş bayt sayısı.
    * Toplam köşeleri: iş çok parçalı bir işin ayrılır, her parça işin bir köşe denir. Bu değer kaç parçalı iş oluşan bir işin açıklar. Temel işlem birim olarak, diğer adıyla Azure Data Lake Analytics birim (ADLAU), bir köşe düşünebilirsiniz ve köşeleri paralellik çalıştırabilirsiniz. 
    * Tamamlanan/çalışan/başarısız oldu: Tamamlandı/çalışan/başarısız tepe sayısı. Köşeleri her iki kullanıcı kodu ve sistem hataları nedeniyle başarısız olabilir, ancak sistem yeniden deneme köşeleri otomatik olarak birkaç kez başarısız oldu. Köşe denedikten sonra yine başarısız olursa, tüm işi başarısız olur.
* İş grafiği
  
    U-SQL betiği çıktı verileri için giriş verileri dönüştürme mantığı temsil eder. Betik derlenmiş ve bir fiziksel yürütme plana hazırlama aşamasında, en iyi duruma getirilmiş. İş grafiğinin fiziksel yürütme planı göstermektir.  Aşağıdaki diyagramda işlem gösterilmektedir:
  
    ![Azure Data Lake Analytics işi aşamaları durumu](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-logical-to-physical-plan.png)
  
    Bir iş, çok parçalı bir işin ayrılır. Her parça işin bir köşe adı verilir. Köşeleri Süper köşe (diğer adıyla aşama) gruplandırılmış ve iş grafiği görünür. İş grafikte yeşil aşama placards aşamalarını gösterir.
  
    Bir aşamasında her köşe aynı verileri farklı parçalarının çalışmak aynı türde yapıyor. Örneğin, bir TB veri sahip bir dosya varsa ve buradan okuma köşeleri yüzlerce vardır, bunların her biri bir öbek okuyor. Bu köşeleri aynı Aşama ve bunun aynı aynı giriş dosyası farklı parçalarının iş gruplandırılmıştır.
  
  * <a name="state-information"></a>Aşama bilgileri
    
      Belirli bir aşamada, bazı numaralarını afişinde gösterilmektedir.
    
      ![Azure Data Lake Analytics işi grafik aşaması](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage.png)
    
    * SV1 Ayıkla: bir sayı ve işlem yöntemi tarafından adlı bir aşamasının adı.
    * 84 köşeleri: Bu aşama tepe toplam sayısı. Şekil kaç parçalı bir işin bu aşamada bölünmüş gösterir.
    * 12.90 s/köşe: Bu aşama için ortalama köşe yürütme süresi. Bu şekil SUM (her köşe yürütme süresi) tarafından hesaplanır / (toplam köşe sayısı). Yani paralellik içinde yürütülen tüm köşeleri atayabilir, tüm aşama tamamlandı 12.90 s. Bu aşamada tüm iş seri olarak yapıldığında, maliyet olacaktır #vertices de anlamına * ortalama zaman.
    * yazılan 850,895 satırlar: toplam satır sayısı bu aşamada yazılır.
    * R/w miktarda veri okunur/yazılan bayt bu aşamasında.
    * Renkler: Renkler aşamasında farklı köşe durumunu belirtmek için kullanılır.
      
      * Yeşil köşe başarılı gösterir.
      * Turuncu köşe denenen gösterir. Denenen köşe başarısız oldu, ancak sistem tarafından otomatik olarak ve başarıyla yeniden denendi ve genel aşaması başarıyla tamamlandı. Köşe denenen ancak hala başarısız oldu, rengi kırmızı ve başarısız tüm iş bırakır.
      * Kırmızı başarısız, gösterir belirli bir köşe başka bir deyişle, sistem tarafından birkaç kez yeniden denendi ancak hala başarısız oldu. Bu senaryoda tüm iş başarısız olmasına neden olur.
      * Belirli bir köşe çalıştıran mavi anlamına gelir.
      * Beyaz gösterir köşe bekliyor. Köşe bir ADLAU kullanılabilir duruma gelir veya kendi giriş verilerini hazır olmayabilir beri bu girişi bekleniyor sonra zamanlanacak bekliyor olabilir.
      
      Fare imlecinizi bir duruma göre gelerek aşaması için daha fazla ayrıntı bulabilirsiniz:
      
      ![Azure Data Lake Analytics iş grafiği aşama ayrıntıları](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage-details.png)
  * Köşeleri: Örneğin, kaç tane köşeleri tamamladınız, toplamda kaç köşeleri başarısız oldu veya hala çalışıyor/bekleme, vb. olan köşeleri Ayrıntılar açıklanmaktadır.
  * Veri okuma arası/içi pod: dosyaları ve verileri depolanmış olan birden çok pod'ları içinde dağıtılmış dosya sistemi. Buradaki değer ne kadar veri aynı pod veya pod çapraz Okunmuş açıklar.
  * Toplam işlem süresi: zaman ayırdığınız tüm aşamasında çalışıyorsanız yalnızca bir köşe yürütülür gibi her bir aşamada köşe yürütme süresi toplamı, onu düşünebilirsiniz.
  * Veri ve satır yazılmış/okuma: ne kadar veri satırları okunan ve yazılan veya okumak gereken gösterir.
  * Köşe okuma hataları: kaç köşeleri veri okuma sırasında başarısız olan açıklar.
  * Köşe yinelenen atar: köşe çok yavaş çalışıyorsa, sistem aynı parça işin çalıştırmak için birden çok köşeleri zamanlama. Köşeleri birini tamamladıktan sonra başarıyla reductant köşeleri atılacak. Köşe yinelenen iptali yinelenen aşamasında atılır köşeleri sayısını kaydeder.
  * Köşe iptalleri: köşe başarıyla gerçekleştirildi, ancak bazı nedenlerle daha sonra yeniden. Örneğin, aşağı akış köşe Ara giriş verisi kaybederse, tekrar çalıştırmak için Yukarı Akış köşe sorar.
  * Köşe zamanlama yürütmeleri: köşeleri zamanlanmış toplam süre.
  * Ortalama/min/Max köşe veri okuma: en düşük/ortalama/en fazla her köşe veri okuyun.
  * Aşama: Duvar saati süresi alır, bu değeri görmek için profil yüklemeniz gerekir.
  * İş Kayıttan Yürütme
    
      Data Lake Analytics işleri çalışır ve işleri bilgilerinin gibi çalışan köşeleri arşivler köşeleri, durduruldu, başlatıldığında başarısız oldu ve bunların nasıl denenen, vs. Tüm bilgileri otomatik olarak sorgu deposunda oturum açmış ve kendi iş profil içinde saklanır. İş profili iş görünümünde "Yük profili" aracılığıyla yükleyebilirsiniz ve iş profili indirdikten sonra iş kayıttan yürütme görüntüleyebilirsiniz.
    
      İş kayıttan yürütme, kümede ne epitome görselleştirme ' dir. İş yürütme ilerleme durumunu izleyin ve performans anormalliklerini ve sorunlarını çok kısa bir süre içinde (değerinden genellikle 30s) kullanıma görsel olarak algıla yardımcı olur.
  * İş ısı Haritası görüntüleme 
    
      İş ısı Haritası görünen açılır listede iş grafiği aracılığıyla seçilebilir. 
    
      ![Azure Data Lake Analytics işi grafik yığın harita görünümü](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-display.png)
    
      G/ç, saati ve verimlilik ısı Haritası üzerinden burada iş çoğu zaman harcayan veya işinizi bir g/ç sınır işi olup bulabilir ve benzeri bir işin gösterir.
    
      ![Azure Data Lake Analytics işi grafik yığın eşleme örneği](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-example.png)
    
    * İlerleme durumu: İş yürütme ilerleme, bilgilerine bakın [aşama bilgi](#stage-information).
    * Okuma ve yazılan veriler: ısı Haritası toplam veri her aşamada okunur/yazılır.
    * Saat işlem: aşamasında tüm iş ile yalnızca 1 köşe yürütülürse uzun sürecektir nasıl ısı Haritası SUM (her köşe yürütme süresi), bu düşünebilirsiniz.
    * Düğüm başına ortalama yürütme süresi: ısı Haritası SUM (her köşe yürütme süresi) / (köşe numarası). Hangi paralellik içinde yürütülen tüm köşeleri atayabilir, tüm aşama bu zaman dilimi içinde gerçekleştirilmesi anlamına gelir.
    * Giriş/Çıkış işleme: giriş/çıkış işleme, her bir aşamaya ısı haritası, işinizi bir g/ç ilişkili işi bu aracılığıyla olup olmadığını doğrulayabilirsiniz.
* Meta veri işlemleri
  
    U-SQL komut dosyanıza bazı meta veri işlemleri, aşağıdaki gibi bir veritabanı oluşturmak, drop table, vb. Bu işlemlerin sonra derleme meta veri işlemi gösterilmektedir. Onaylar bulmak, varlıkları oluşturma, varlıkları buraya bırak.
  
    ![Azure Data Lake Analytics iş görünümünde meta veri işlemleri](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-metadata-operations.png)
* Durum geçmişi
  
    Durum geçmişi ayrıca iş özeti görselleştirilen ancak burada daha fazla bilgi alabilirsiniz. İş, sıraya alındı, hazırlandığında başlatılan çalışması sona erdi gibi ayrıntılı bilgi bulabilirsiniz. Ayrıca iş derlenmiş kaç kez bulabilirsiniz (CcsAttempts: 1), ne zaman iş gönderilen kümeye gerçekte (ayrıntı: kümeye işi gönderdikten), vb.
  
    ![Azure Data Lake Analytics iş görünümünde durumu geçmişi](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-state-history.png)
* Tanılama
  
    Aracı iş yürütme otomatik olarak tanılar. Bazı hatalar veya işleriniz performans sorunları olduğunda uyarı alırsınız. Lütfen buraya tam bilgi almak için profili indirme gerektiğini unutmayın. 
  
    ![Azure Data Lake Analytics iş görünümünde tanılama](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-diagnostics.png)
  
  * Uyarı: Bir uyarı burada ile derleyici uyarısı görüntülenir. Uyarı göründükten sonra daha fazla ayrıntı için "x sorunu/sorunları" bağlantıyı tıklatabilirsiniz.
  * Çok uzun çalıştırma köşe: herhangi bir izdüşümünü saati (deyin 5 saat) dışında çalıştırıyorsa, sorunları burada bulunacaktır.
  * Kaynak Kullanımı: gerekenden daha fazla veya yeterli paralellik ayırdığınızda, sorunları burada bulunacaktır. Daha fazla ayrıntı görmek ve daha iyi kaynak ayırma bulmak için durum senaryoları gerçekleştirmek için kaynak kullanımı tıklatabilirsiniz (daha fazla ayrıntı için bu Kılavuzu'na bakın).
  * Bellek onay: herhangi bir izdüşümünü 5 GB'den fazla bellek kullanıyorsa, sorunları burada bulunacaktır. Sistem sınırlaması daha fazla bellek kullanıyorsa, iş yürütme sistem tarafından sonlandırıldı.

## <a name="job-detail"></a>İş ayrıntısı
İş ayrıntısı komut dosyası, kaynakları ve köşe yürütme görünümü de dahil olmak üzere işin ayrıntılı bilgilerini gösterir.

![Azure Data Lake Analytics iş ayrıntısı](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-details.png)

* Betik
  
    U-SQL betiği iş sorgu deposunda saklanır. Özgün U-SQL betiği görüntülemek ve gerekirse yeniden gönderin.
* Kaynaklar
  
    Kaynakları aracılığıyla sorgu deposu içinde depolanan iş derleme çıkışları bulabilirsiniz. Örneğin, "iş grafiği göstermek için kullanılan algebra.xml", kaydettiğiniz derlemeler, vb. burada bulabilirsiniz.
* Köşe yürütme görünümü
  
    Bu, köşe yürütme ayrıntıları gösterir. İş profili toplam veri okunur/yazılır, çalışma zamanı, durum, vb. gibi her köşe yürütme günlüğü arşivler. Bu görünüm nasıl bir işin çalışma hakkında daha fazla ayrıntı elde edebilirsiniz. Daha fazla bilgi için bkz: [köşe yürütme görünümü Visual Studio için Data Lake araçları kullanmak](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).

## <a name="next-steps"></a>Sonraki Adımlar
* Tanılama bilgilerini günlüğe kaydetmek için bkz. [Azure Data Lake Analytics için tanılama günlüklerine erişme](data-lake-analytics-diagnostic-logs.md)
* Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.
* Köşe yürütme görünümü kullanmak için bkz: [köşe yürütme görünümü Data Lake araçları Visual Studio için kullanın.](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)

