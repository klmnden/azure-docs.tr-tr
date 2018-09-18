---
title: Azure SQL veritabanı Azure örnek olay incelemesi - Daxko/CSI | Microsoft Docs
description: Daxko/CSI SQL veritabanı, geliştirme döngüsünü hızlandırmalarına ve müşteri hizmet ve performansı geliştirmek için kullanma hakkında bilgi edinin
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: reference
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: carlrab
ms.openlocfilehash: 10720f42f7a9b10b42ccaaaad81acca369592f6a
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45731221"
---
# <a name="daxkocsi-used-azure-to-accelerate-its-development-cycle-and-to-enhance-its-customer-services-and-performance"></a>Daxko/CSI Azure, kendi geliştirme döngüsünü hızlandırmalarına ve müşteri hizmet ve performansı geliştirmek için kullanılır.
![Daxko/CSI logosu](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Daxko/CSI yazılımı karşılaşılan bir zorluk: Müşteri tabanı uygunluk ve dinlence merkezleri kapsamlı Kurumsal yazılım çözümünün başarısı sayesinde hızla büyüyen ancak BT altyapısı gereksinimleri büyüyen o müşteri için tutma temel test şirketin BT personeli. Şirket giderek büyüyen veritabanlarını yönetmek için özellikle yükselen işlemlerinin yükünü kısıtlanmış. Daha da kötüsü, bu işlem ek yükü, şirketin yazılım için yeni mobility özellikleri gibi yeni girişimler için geliştirme kaynakları içine kesme.

Daxko/CSI, ürün geliştirme yöneticisi olan David Molina göre yazılımı odaklanmaya kaynakları boşaltmaya veritabanı yönetimi basitleştirin ve ölçeklenebilirliği artırmak için gereken hizmet olarak platform (PaaS) modelinde ile CSI yazılımı Azure sağlanan OPS yerine. "Azure SQL veritabanı, bizim için mükemmel bir seçenektir oluştu. SQL Server Yük devretme kümesi ve tüm diğer altyapı için gerekirse bakımı hakkında endişelenmeye gerek kalmadan bizim için ideal."

Azure'a geçiş sonrasında CSI yazılımı yalnızca iki 600'den fazla müşteri veritabanını yönetmek üzere bir operasyon personeli gerekir. Şirket boyutuna göre müşteri veritabanlarını taşımak için Azure SQL veritabanı elastik havuzları kullanır ve gerekir.

Molina devam eder, "müşterilerimiz değişikliği hemen düşünmüştür. Elastik havuzlar önce bazen zaman aşımları ve diğer sorunları veri bloğu dönemleri sırasında sahip oldukları. Azure elastik havuzlar sayesinde bunlar gerektiği gibi veri bloğu ve yazılım sorunları olmadan kullanın."

Müşteriler, yeni hizmetler ve özellikler, operasyon ve Yönetimi uğraşmanızı yerine geliştirmeye odaklanabilirsiniz CSI yazılımı kaynakları serbest Azure elastik havuzlar için performansı iyileştirme yanı sıra. Bu BT kaynakları, uygulamaları üyeleri etkileşim kurun, personel verimliliğini artırmak amacıyla SpectrumNG, sunan, Kurumsal yazılımı geliştirmek ve personel ve üyeleri etkileşimli görevler ve gerçek zamanlı bildirimler için mobil erişim verin CSI yazılımı yardımcı olmuştur.

Azure ayrıca hızlandırmak ve Otomasyon seçenekleri sağlayarak geliştirme ve kalite güvencesi (kapsayan QA) döngüsü geliştirmek CSI yazılımı yardımcı olmuştur. Şirketin Azure uygulamasıyla yapı yöneticileri, bir düğmeye tıklayarak bileşenleri paketleyebilirsiniz. Molina açıklandığı gibi "sürüm döngüsünün bir parçası olarak, QA artık bizim üretim yığın yakından taklit eden Azure, bir test ortamında dağıtmak kullanabilirsiniz. Biz derlemeleri hemen değişiklikleri Kıdemli için geliştirme ortamımız dağıtabilirsiniz. Biz önce test etmek için eşlik gerekmedi bizim için büyük bir kazanç olmasıdır."

## <a name="offloading-to-the-cloud"></a>Buluta boşaltma
Buluta geçmeden önce CSI yazılımı başarılı bir şekilde kendi çok kiracılı altyapısını tarihlerinde Houston'da yerel bir veri merkezinde yerleşik. Şirket ayrıntılı olarak artan büyüyen satın alma, hazırlama ve tüm müşterilerine desteklemek için gereken yazılım ve donanım Bakımı sorunlar karşılaştığı. BT işlemleri işlemek için personel yeni kaynakları sağlama ve müşterilerine yeni hizmetlerini çalışırken yavaşlama götüren başka bir performans sorununa dönüştü.

İşlemlerini yerine kendi kod odaklanmak böylece bu ek yükü ortadan kaldırmak için bulut seçeneklerine içine CSI yazılımı arıyordu. Şirket, birçok en iyi bulut sağlayıcıları yalnızca Iaas yığın yönetmek için büyük bir BT ekibiniz gerektiren hizmet olarak altyapı (Iaas) çözümleri sunma bulundu. Sonunda CSI yazılımı, Azure PaaS çözümü için kendi gereksinimlerine en uygun olduğuna karar verdik. Molina açıklar, "biz tekliflerimizi yazılım bizim BT ek yükünü azaltırken odaklanmanızı sağlayacak Azure ortada, donanım ve sistem yazılımı alır."

## <a name="making-the-transition-to-azure"></a>Azure'a geçiş yapma
Azure PaaS çözümü seçtikten sonra veritabanları ve arka uç altyapısını buluta geçirme CSI yazılımı başladı. Azure önce SpectrumNG müşteriler, arka uçta bir Windows Communication Foundation (WCF) hizmetiyle iletişim kurduğu bir istemci uygulaması yüklemek gerekli. "Bazı müşteriler kendi veri merkezlerini her şeyi barındırılan rağmen Molina göre çok kiracılı olmasını ürün incelemelerini oluşturduk. Biz her şeyi bir veri merkezinde Houston, SQL Server veri deposu olarak kullanarak barındırılan.

"Ayrıca sunan ürünümüzün müşterinin web varlığı ve çevrimiçi sayfaları ve tüm üçüncü taraf tümleştirmeyi desteklemek için bir SOAP API eşleştirmek için beyaz etiketli olacak şekilde tasarlanmıştır (ASP.net ile yazılmış) bir üyesi'e yönelik web portalı dahil."

Buluta geçiş uzun almaz mimarisi. Molina göre "Biz config dosya bilgileri, bir merkezi bağlantı dizesini değiştirme ve paketleme otomatikleştirme, karşıya yükleme ve yayınlarımızı dağıtımını okuma biçimini değiştirme ile ele çalışmasının çoğu."

CSI yazılım mühendisleri yapı Otomasyonu geliştirmek için paketler oluşturmak ve bunları her gece yayını için bir hazırlık ortamına yüklemek için Azure PowerShell ve REST API'leri kullanılır.
Azure bulut tabanlı dağıtım için genel geçiş CSI yazılımı BT Ekibi için hızlı ve sorunsuz bir şekilde geçti. Molina açıklar, "tüm, beta ortam bulutta projede alınmasına üç ila dört hafta içinde vardı. Bu bizim için şaşırtıcı bir kazanç sağladı."

Yapılandırma ve ortam test sonra geçiş CSI yazılımı başlangıcından müşteriler. CSI yazılımı barındırma zaten kullanan müşteriler için geçişi neredeyse sorunsuz. Bir şirket içi dağıtımından geçişini gerçekleştiren müşteriler, buluta geçiş için ek süre aldı, ancak hala öncelikle olan hem müşterilerin hem de CSI yazılımı sorunlu olarak ücretsiz.

Yeni müşteriler için CSI yazılımı ait yerleşik için aşağıdaki işlemi bunları Azure'a kullanan BT personeli:

1. Azure PowerShell betikleri, müşteri için yeni bir veritabanı dönmesi için kullanılır; geçiş ilk yeterli aktarım hızının emin olmak için premium katmandaki tüm müşteriler başlar.
2. CSI yazılımı, Azure SQL Geçiş Sihirbazı'nı mümkün olduğunda, mevcut verileri Azure SQL veritabanı örneğine taşımak için kullanır.
3. Son olarak, Microsoft SQL Server Integration Services (SSIS) uyumsuzlukları verilerinde mutabakat sağlamak veya gerektiği gibi herhangi bir veri temizleme işlemini gerçekleştirmek için kullanılır.

Bugün, yaklaşık yüzde 99'csı yazılımı müşterilere dört bölgesel veri merkezleri arasında (Kuzey Orta ABD, Orta Güney, Doğu ve Batı), Azure'da barındırılır. Her bir müşterinin coğrafi bölgede veri merkezlerinde sağlayarak, gecikme süresi en az olarak tutulur.

## <a name="azure-elastic-pools-free-up-it-resources"></a>BT kaynaklarını ücretsiz Azure elastik havuzlar
Çeşitli Azure özelliklerinin CSI yazılımı yardımcı shift altyapı ve işlemleri özellik ve geliştirme odaklı olmak için odaklı engeller. Belki de en büyük avantaj, elastik havuzlarını olmuştur.

CSI yazılımı, müşteriler için şu anda yaklaşık 550 veritabanları sağlar. Elastik havuzlar önce bir katman yapısı içinde bu kadar fazla sayıda veritabanını yönetmek zordu. OPS yöneticileri, hizmet katmanları atayın ve önemli BT-kaynak yükü gereken müşteriler, veri bloğu ihtiyaçlarını temel alarak boyutları işlem gerekiyordu. Elastik havuzlar sayesinde yöneticileri kiracılar, premium veya standart bir havuz, uygun şekilde atamak ve ardından boyutuna göre müşteriler taşıyabilir ve gerekir. Müşteriler, elastik havuzlar etkilerini hemen düşünmüştür; Elastik havuzlar önce müşterilerin veri bloğu kullanım dönemleri sırasında zaman aşımları ve diğer sorunlar vardı ancak elastik havuzlar sayesinde müşteriler gerektiğinde etkinlik artışları oluşabilir ve SpectrumNG sorun olmadan kullanmaya devam edebilirsiniz.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Azure etkin coğrafi çoğaltma raporlama hızlandırır.
Birkaç CSI yazılımı Müşteriler ayrıca Azure etkin coğrafi çoğaltma avantajlarından sürüyor. Etkin coğrafi çoğaltma ile en fazla dört okunabilir ikincil veritabanı, aynı veya farklı bir veri merkezi bölgelerinde yapılandırılabilir. CSI yazılımı iki yolla etkin coğrafi çoğaltma kullanır: önce ikincil veritabanlarını bir veri merkezi kesintisi veya şubelerde birincil veritabanına bağlanmak için söz konusu olduğunda kullanılabilir ve ikinci olarak, ikincil veritabanlarıyla okunabilir ve raporlama işleri gibi salt okunur iş yüklerini boşaltma için kullanılabilir. CSI yazılımı bazı müşteriler bu Avantajdan raporlama iş akışları hızlandırmak için kullanın.

## <a name="csi-software-application-logic-and-architecture"></a>CSI yazılımı uygulama mantığı ve mimarisi
SpectrumNG web rollerini kullanır. Çok kiracılı bir uygulama olduğundan, bir WCF Hizmeti müşterilerden ilk bağlantı isteği işlemek için kullanılır. Molina durumları gibi "istek bize bir bağlantı dizesine, veritabanlarına yapmanız gereken her şeyi yapmak için yapı sağlayan her müşteri tanımlar."

Hizmet web katmanı için CSI yazılımı Azure otomatik ölçeklendirme, gün ve saatte tabanlı yararlanır. Kullanılabilir kaynaklar otomatik olarak iş saatlerinde daha yüksek kullanım uyum sağlamak için her bir bölgesel veri merkezi saat dilimine göre artırılır. Kaynaklar ayrıca müşteri gereksinimleri daha düşük olduğunda sonralı ölçeğini ayarlanır.

![Daxko/CSI mimarisi](./media/sql-database-implementation-daxko/figure1.png)

Şekil 1. Cloud services çalışan rolü yapılandırılmış verileri Azure SQL veritabanı ve tablo depolama yarı yapısal verilerle çizer. Bir bulut aracılığıyla veri web rolü Hizmetleri ile SpectrumNG kullanıcıların etkileşim.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Web uygulamaları ve web-planı katmanı için mobil uygulamaları kullanma
Yeni girişim, eksiksiz bir mobil platform da dahil olmak üzere etkinleştirmek için Azure SQL veritabanı'nın CSI yazılımı kaynakları serbest kullanarak Azure web apps'te barındırılan özel bir API temel. Platform uygulamaları üyeleri ve personel zamanlamalarını denetlemek, sınıfları kitap ve iletileri almak için mobil cihazlarını kullanmasına izin sağlar.

Platform, tek bir bileşen almak için hizmet odaklı mimari (SOA) kullanır. — ister bir satış noktası (POS) veya satış sistemlerinin — için başka bir web planı anında taşıyın ve ardından söz konusu bileşen üzerinde diğer her şey bırakarak desteklemek için hizmet başlatma özgün web planı. Bu CSI yazılımı Bilim insanları için inanılmaz esneklik sağlar ve maliyetleri düşük tutun yardımcı olur.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Azure uygulamaları ve Hizmetleri CSI yazılımı geliştiricileri odaklanan olanak tanır.
Azure SQL veritabanı yalnızca boon hızlı, güvenilir hizmet keyfini SpectrumNG müşteriler için değil, aynı zamanda CSI yazılımın için büyük bir kazanç olan BT personeli ve geliştiriciler. Ops bulutta azure'a yük boşaltma tarafından CSI yazılımı kaynakları ve altyapı için kendi ek yükü azaltıldı, büyük ölçüde micromanage veritabanlarına, kiracılar için performansı iyileştirmek için kendi geliştirme döngülerinizi ve artık gereksinimleri hızlandırılmış.

## <a name="more-information"></a>Daha fazla bilgi
* Azure elastik havuzlar hakkında daha fazla bilgi için bkz: [elastik havuzlar](sql-database-elastic-pool.md).
* Veritabanı Araçları ve esnek ölçeklendirme hakkında daha fazla bilgi için bkz: [esnek veritabanı araçlarını ve esnek ölçeklendirme](sql-database-elastic-scale-get-started.md).
* Bir SQL Server veritabanını geçirme hakkında daha fazla bilgi için bkz: [bir SQL veritabanını Azure'a geçirme](sql-database-cloud-migrate.md).
* Etkin coğrafi çoğaltma hakkında daha fazla bilgi için bkz: [etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md).
* Azure Service Bus hakkında daha fazla bilgi için bkz: [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).
* Otomatik ölçeklendirme hakkında daha fazla bilgi için bkz: [bulut Hizmetleri ölçeklendirme](../cloud-services/cloud-services-how-to-scale-portal.md).

