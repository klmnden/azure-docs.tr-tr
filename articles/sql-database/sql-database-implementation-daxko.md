---
title: "Azure SQL veritabanı Azure örnek olay incelemesi - Daxko/CSI | Microsoft Docs"
description: "Daxko/CSI SQL veritabanı geliştirme döngüsü hızlandırmak için ve müşteri hizmetleri ve performansını artırmak için kullanma hakkında bilgi edinin"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 00c8a713-f20c-4d6b-b8b7-0c1b9ba5f05b
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: Inactive
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 7a05836be4a0879fa7103d070c683f45c06cd741
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="daxkocsi-used-azure-to-accelerate-its-development-cycle-and-to-enhance-its-customer-services-and-performance"></a>Daxko/CSI Azure, kendi geliştirme döngüsü hızlandırmak için ve müşteri hizmetleri ve performansını artırmak için kullanılır.
![Daxko/CSI logosu](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Daxko/CSI yazılım karşılaştığı bir sınama: uygunluk ve dinlenme merkezlerinin müşteri tabanı, hızlı bir şekilde, kendi kapsamlı Kurumsal yazılım çözümü başarı sayesinde büyüyen ancak şirketin büyüyen o müşteri için BT altyapısı gereksinimleri olan temel tutmaktan test edildi BT personeli. Şirket gittikçe büyüyen veritabanlarını yönetmek için özellikle yükselen işlemlerinin yükünü kısıtlanmış. Da kötüsü, bu işlem yükünü şirketin yazılım yeni mobility özellikleri gibi yeni girişimleri için geliştirme kaynakları içine kesme.

David Molina, Daxko/CSI adresindeki ürün geliştirme Director göre ops yerine yazılım odaklanmak için kaynakları serbest veritabanı yönetimi basitleştirmek ve ölçeklenebilirliği artırmak için gereken hizmet olarak platform (PaaS) modeli CSI yazılım Azure sağlanan. "Azure SQL Database bize için harika bir seçenek oluştu. SQL Server, yük devretme kümesi ve diğer altyapıya gereksinim duyar bakımı hakkında endişelenmenize gerek olmayan bize için ideal."

Azure'a geçirme bu yana bir işlem personeli 600'den fazla müşteri veritabanlarını yönetmek için yalnızca iki CSI yazılım gerekir. Şirket boyutuna bağlı müşteri veritabanlarını taşımak için Azure SQL Database esnek havuzlar kullanır ve gerekir.

Molina devam eder, "müşterilerimizin değişikliği hemen Keçeli. Esnek havuzları önce bazen zaman aşımları ve diğer sorunlar veri bloğu dönemlerde sahip oldukları. Azure esnek havuzları ile bunlar gerektiği gibi veri bloğu ve sorun olmadan yazılım kullanın."

Müşteriler, yeni hizmetler ve işlemler ve yönetim postalarla yerine özellikler geliştirmeye odaklanmaya CSI yazılım kaynakları serbest Azure esnek havuzlar için performansı iyileştirme yanı sıra. BT kaynaklarla CSI, Spor salonu üyeleri Göster, personel verimliliğini artırmanıza yardımcı olmak için SpectrumNG sunumu, kurumsal yazılımlarının geliştirmek ve personeli ve üyeleri etkileşimli görevler ve gerçek zamanlı bildirimler için mobil erişim vermek yazılım olunmasına yardımcı oldu.

Azure ayrıca CSI hızlandırmak ve Otomasyon seçenekleri sağlayarak geliştirme ve kalite güvence (QA) döngüsü geliştirmemize yazılım Yardım. Şirketin Azure uygulamasıyla düğmesinin tıklatmayla bileşenleri yapı yöneticileri paketleyebilirsiniz. Molina açıklandığı gibi "yayın döngüsü kapsamında, QA yakından bizim üretim yığını taklit eden Azure bir test ortamında dağıtmak için sunulmuştur. Biz derlemeleri hemen değişiklikleri Kıdemli için bizim geliştirme ortamına dağıtabilirsiniz. Biz önce test etmek için eşlik olmadığına büyük kazanım için bize, olmasıdır."

## <a name="offloading-to-the-cloud"></a>Bulut için yük boşaltma
Buluta taşımadan önce CSI yazılımı başarıyla Houston yerel bir veri merkezinde çok müşterili kendi altyapısında yukarı yerleşik. Şirket genişletilmiş gibi satın alma, sağlama ve tüm müşterilerine desteklemek için gereken yazılım ve donanım Bakımı artan büyüyen sorunlar karşılaştığı. BT işlemleri işlemek için personel yeni kaynaklar sağlama ve müşterilerine yeni hizmetlerini çalışırken yavaşlama etmeleri başka bir performans sorunu hale geldi.

Böylece işlemlerini yerine kendi kod odaklanmak, ek yükü ortadan kaldırmak için bulut seçenekleri içine CSI yazılım arama. Şirket, üst bulut sağlayıcıları çoğunu yalnızca hala Iaas yığın yönetmek için büyük bir BT ekibiniz gerektiren hizmet olarak altyapı (Iaas) çözümleri sunar buldu. Sonunda CSI yazılım Azure PaaS çözüm için kendi gereksinimlerine en uygun olduğuna karar verdik. Molina açıklar, "biz yazılım tekliflerimiz bizim BT ek yükünü azaltırken odaklanabilirsiniz Azure göz önünden, donanım ve sistem yazılımı alır."

## <a name="making-the-transition-to-azure"></a>Azure için geçiş yapma
Azure PaaS çözümünün seçtikten sonra veritabanları ve arka uç altyapısı buluta geçiş CSI yazılım başlamıştır. Azure önce SpectrumNG müşteriler arka uçtaki Windows Communication Foundation (WCF) hizmetiyle iletişim bir istemci uygulamasını yüklemek gerekli. "Bazı müşteriler, kendi veri merkezlerini üstbilgideki barındırılan rağmen Molina göre çok müşterili ürünün oluşturduğumuz. Biz her şeyi bir veri merkezinde Houston, SQL Server veri deposu olarak kullanarak barındırılan.

"Ayrıca sunumu Ürünümüz Müşteri'nin web varlığı ve çevrimiçi sayfalar ve tüm üçüncü taraf tümleştirme desteklemek için bir SOAP API eşleştirmek için beyaz etiketli olacak şekilde tasarlanmıştır (ASP.net ile yazılmış) bir üyesi kullanıma yönelik web portalını dahil."

Buluta geçiş uzun sürdü değil mimarisi için. Molina göre "Biz config dosya bilgileri, bir merkezi bağlantı dizesi değiştirilmesi ve paketleme otomatikleştirme, karşıya yükleme ve dağıtım bizim sürümlerin okuma açığını ile ele çaba çoğunluğu."

CSI yazılım mühendisleri yapı Otomasyon geliştirmek için paketleri oluşturmak ve bunları sürüm her gece için hazırlama ortamına yüklemek için Azure PowerShell ve REST API'lerinin kullanılır.
Azure bulut tabanlı dağıtım için genel geçiş CSI yazılım BT Ekibi için hızlı ve sorunsuz geçti. Molina açıklar, "tüm, biz beta ortamı bulutta projede almaya üç ila dört hafta içinde vardı. Şaşırtıcı win bize için oluştu."

Geçiş CSI yazılım yapılandırma ve sınama ortamında sonra başlayan müşteriler. Zaten CSI yazılım barındırma kullanan müşteriler için geçişi neredeyse sorunsuz. Müşteriler, bir şirket içi dağıtımından geçirmek için geçiş buluta bazı ek zaman aldı, ancak hala öncelikle olan müşteriler ve CSI yazılım için sorun teşkil edecek boş.

Yeni müşteri için CSI yazılım ait yerleşik için aşağıdaki süreci bunları Azure'a kullanan BT personeli:

1. Azure PowerShell betikleri müşteri için yeni bir veritabanı dönmesi için kullanılır; tüm müşteriler için geçiş ilk yeterli verimliliği sağlamak için bir premium katmanındaki başlar.
2. Mümkün olduğunda, CSI yazılım Azure SQL Geçiş Sihirbazı varolan verileri Azure SQL veritabanı örneğine taşımak için kullanır.
3. Son olarak, Microsoft SQL Server Integration Services (SSIS) gerektiği gibi herhangi bir veri temizleme gerçekleştirmek için veya uyumsuzlukları veri mutabık kılmak için kullanılır.

Bugün, yaklaşık yüzde 99 CSI yazılım müşteriler dört bölgesel veri merkezleri arasında (Kuzey Orta Güney Merkezi, Doğu ve Batı) Azure içinde barındırılır. Her müşterinin coğrafi bölgede veri merkezleri sağlayarak, gecikme süresi en az olarak tutulur.

## <a name="azure-elastic-pools-free-up-it-resources"></a>BT kaynaklarını ücretsiz Azure esnek havuzlar
Azure çeşitli özellikler CSI yazılım yardımcı shift altyapı ve işlemleri özellik ve geliştirme odaklanmış olmaya odaklanmış engeller. Belki de en büyük avantajı esnek havuzlarını olmuştur.

CSI yazılım hakkında 550 veritabanları müşteriler için şu anda sağlar. Esnek havuzlar önce diğer birçok veritabanı katmanı yapısı içinde yönetmek zordu. OPS yöneticileri önemli BT-kaynak yükü gerekli müşteriler, veri bloğu ihtiyaçlarına göre performans katmanı atamak gerekiyordu. Esnek havuzları ile yöneticileri kiracılar premium ya da uygun şekilde standart havuzu atamak ve ardından boyutuna göre müşteriler taşıyabilir ve gerekir. Müşteriler, esnek havuzlar etkilerini hemen Keçeli; Esnek havuzları önce müşteriler zaman aşımları ve diğer sorunlar aşırı kullanım dönemlerinde gerekliydi ancak esnek havuzları ile gerektiğinde etkinlik WINS'e müşteri deneyimi ve sorunsuz SpectrumNG kullanmaya devam.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Azure active coğrafi çoğaltma raporlama hızlandırır.
Birkaç CSI yazılım müşterileri de Azure active coğrafi çoğaltma avantajlarından sürüyor. Etkin coğrafi çoğaltma ile en fazla dört okunabilir ikincil veritabanları aynı veya farklı bir veri merkezi bölgelerde yapılandırılabilir. CSI yazılım iki yolla etkin coğrafi çoğaltma kullanır: ilk olarak, ikincil veritabanlarıyla datacenter kesintisi veya bağlanamama birincil veritabanına; söz konusu olduğunda kullanılabilir ve ikinci olarak, ikincil veritabanlarıyla okunabilir ve raporlama işleri gibi salt okunur iş yükleri boşaltması için kullanılabilir. Bazı CSI yazılım müşteriler bu avantajı raporlama iş akışları hızlandırmak için kullanın.

## <a name="csi-software-application-logic-and-architecture"></a>CSI yazılım uygulama mantığını ve mimarisi
SpectrumNG web rollerini kullanır. Çok kiracılı uygulama olduğu için bir WCF Hizmeti müşterilerden ilk bağlantı isteğini işlemek için kullanılır. Molina durumları, "istek bize ne olursa olsun yapmamız gereken yapmak için kendi veritabanlarını giden bir bağlantı dizesi yapı sağlayan her müşteri tanımlar."

Kendi hizmet web katmanı için Azure otomatik ölçeklendirme, avantajı CSI yazılım gün ve saate göre alır. Kullanılabilir kaynakları otomatik olarak daha yüksek kullanım iş saatlerinde uyum sağlayacak şekilde her bölgesel veri merkezi saat dilimini göre artırılır. Kaynaklar ayrıca müşteri gereksinimlerine daha düşük olduğunda hafta sonları üzerinde ölçeklendirmeyi azaltın ayarlanır.

![Daxko/CSI mimarisi](./media/sql-database-implementation-daxko/figure1.png)

Şekil 1 '. Bulut Hizmetleri çalışan rolü yapılandırılmış verileri Azure SQL veritabanı ve tablo depolama yarı yapılandırılmış verilerden çizer. Bir bulut aracılığıyla veri web rolü Hizmetleri ile SpectrumNG kullanıcıların etkileşim.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Web uygulamaları ve web planı katmanı için mobil uygulamaları kullanma
Eksiksiz bir mobil platform dahil olmak üzere yeni girişimleri etkinleştirmek için Azure SQL veritabanı CSI yazılım kaynakları serbest kullanarak Azure web uygulamalarında barındırılan özel bir API temel. Platform Spor salonu üyeleri ve personel zamanlamalarını denetlemek, sınıfları kitap ve iletileri almak için mobil cihazlarını kullanmasına olanak verir.

Platform, tek bir bileşen almak için hizmet odaklı mimari (SOA) kullanır. — bir satış noktası (POS) sistemi veya bir satış sistemi gibi — başka bir web plana anında taşıyın ve sonra bu bileşen özgün web plan üzerinde şey bırakarak desteklemek için bir hizmet dönmesi. Bu özelliği CSI yazılım inanılmaz esneklik ve maliyetleri düşük tutun yardımcı olur.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Azure CSI yazılım geliştiriciler odaklanmak uygulama ve hizmetlere izin verir
Azure SQL veritabanı yalnızca bir katkıda bulunuyor hızlı ve güvenilir hizmet keyfini SpectrumNG müşterileri değil, ayrıca CSI Software'in için büyük kazanım BT personeli ve geliştiricilerin. Bulutta Azure ops aktararak CSI yazılım kaynakları ve altyapı için kendi ek yük azaltılmış, büyük ölçüde, kiracılar için performansı iyileştirmek için micromanage veritabanları için kendi geliştirme döngüleri ve artık gereksinimleri hızlandırılmış.

## <a name="more-information"></a>Daha fazla bilgi
* Azure esnek havuzları hakkında daha fazla bilgi için bkz: [esnek havuzlar](sql-database-elastic-pool.md).
* Veritabanı Araçları ve esnek ölçeklendirme hakkında daha fazla bilgi için bkz: [esnek veritabanı araçlarını ve esnek ölçeklendirme](sql-database-elastic-scale-get-started.md).
* SQL Server veritabanını geçirme hakkında daha fazla bilgi edinmek için bkz: [bir SQL Server veritabanını Azure'a geçirmek](sql-database-cloud-migrate.md).
* Aktif coğrafi çoğaltma hakkında daha fazla bilgi için bkz: [aktif coğrafi çoğaltma](sql-database-geo-replication-overview.md).
* Web rolleri ve çalışan rolleri hakkında daha fazla bilgi için bkz: [çalışan rolleri](../fundamentals-introduction-to-azure.md#compute).    
* Azure Service Bus hakkında daha fazla bilgi için bkz: [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).
* Otomatik ölçek hakkında daha fazla bilgi için bkz: [bulut Hizmetleri ölçeklendirme](../cloud-services/cloud-services-how-to-scale-portal.md).

