---
title: Azure'da SQL seçenekleri arasından seçim | Microsoft Docs
description: Azure sanal makinelerinde dağıtım içindeki Azure SQL veritabanı ve Azure SQL veritabanı ve SQL Server arasındaki seçenekleri arasından seçim yapmayı öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
keywords: SQL Server bulut, bulutta SQL Server, PaaS veritabanı, bulut SQL Server, DBaaS
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/11/2019
ms.openlocfilehash: d9cd5ba0b697cbf67f943eb49d66010745d8561e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60584863"
---
# <a name="choose-the-right-sql-server-option-in-azure"></a>Azure'da SQL Server seçeneği sağ seçin

Azure'da barındırılan bir altyapı (Iaas) içinde çalışan veya barındırılan bir hizmet olarak çalışan SQL Server iş yüklerinizi olabilir ([PaaS](https://azure.microsoft.com/overview/what-is-paas/)). PaaS içinde birden çok dağıtım seçenekleri ve her dağıtım seçeneğinin içinde hizmet katmanlarına sahip. Veritabanınızı yönetme, düzeltme ekleri uygulama, yedeklemeler almak istiyorsunuz veya bu işlemleri Azure için temsilci atamak istediğiniz önemli sorulardan biri PaaS veya Iaas arasında karar verirken sormanız gerekir mi?
Yanıt bağlı olarak aşağıdaki seçenekleriniz vardır:

- [Azure SQL veritabanı](sql-database-technical-overview.md): En son kararlı Kurumsal SQL Server sürümü üzerinde temel bir tam olarak yönetilen SQL veritabanı altyapısı. Bu, bir ilişkisel veritabanı olarak-endüstri kategorisine girer Azure bulutunda barındırılan hizmet (DBaaS) *olarak bir-hizmet Platform (PaaS)*. SQL veritabanı, her biri, sahip olduğu, barındırdığı ve Microsoft tarafından yönetilen, standartlaştırılmış donanım ve yazılım üzerine inşa edilmiştir, birden çok dağıtım seçenekleri vardır. SQL veritabanı ile yerleşik özellikleri ve SQL Server kullanıldığında kapsamlı yapılandırma gerektiren işlevleri kullanabilirsiniz (şirket içinde veya bir Azure sanal makinesinde). SQL Database'i kullandığınızda, kesinti olmadan daha fazla güç için ölçeğinizi artırmaya veya genişletmeye yönelik seçeneklerle kullandıkça ödersiniz. SQL veritabanı, SQL Server'da yerleşik yüksek kullanılabilirlik, zeka ve yönetimi gibi mevcut olmayan ek özellikler vardır. Azure SQL veritabanı, aşağıdaki dağıtım seçeneklerinden sunar:
  
  - Olarak bir [tek veritabanı](sql-database-single-database.md) kendi kaynak kümesi ile yönetilen bir SQL veritabanı sunucusu. Tek bir veritabanı benzer bir [içerdiği veritabanları](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) SQL Server'da. Bu seçenek, yeni uygulamaları buluttan kaynaklanan modern uygulama geliştirme için optimize edilmiştir.
  - Bir [elastik havuz](sql-database-elastic-pool.md), kaynak SQL veritabanı sunucusu yönetilen paylaşılan bir grup veritabanı koleksiyonu. Tek veritabanları, içine ve elastik havuz dışına taşınabilir. Bu seçenek, çok kiracılı SaaS uygulaması kullanılarak yeni bulut kaynaklı uygulamaların modern uygulama geliştirme için optimize edilmiştir.
  - [Yönetilen örnek](sql-database-managed-instance.md), paylaşılan bir kaynak kümesi ile sistem ve kullanıcı veritabanlarını koleksiyonu. Yönetilen örnek veritabanları ve ek örnek kapsamlı özellikler için [Microsoft SQL Server veritabanı altyapısı] teklifi paylaşılan kaynakları örneğine benzer. Yönetilen örnek veritabanı ile şirket içi geçişini destekler veritabanı değişiklik için en az. Bu seçenek, tüm Azure SQL veritabanı'nın PaaS avantajları sağlar, ancak daha önce SQL VM'ler yalnızca kullanılabilir özellikleri ekler. Bu, yerel sanal ağ (VNet) içerir ve şirket içi SQL Server ile % 100 uyumluluk yakın.
- [Azure Virtual Machines'de SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/) endüstri kategorisine girer *-olarak-hizmet altyapı (Iaas)* ve SQL Server Azure bulutunda tam olarak yönetilen bir sanal makine içinde çalıştırmanıza olanak tanır. [SQL Server sanal makineleri](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) de, sahip olduğu, barındırdığı ve Microsoft tarafından yönetilen standartlaştırılmış donanım üzerinde çalıştırın. Bir VM'de SQL Server kullanırken ödeyebilir miyim-gibi bir SQL Server görüntüsüne zaten dahil bir SQL Server lisansı için Git ya da mevcut bir lisansı kolayca kullanabilirsiniz. Ayrıca, durdurabilir veya VM gerektiği gibi devam. SQL Server yüklü ve Windows Server veya Linux sanal makineleri (VM'ler) olarak da bilinen bir altyapı (ıaas) olarak Azure üzerinde çalışan bulutta barındırılan. Azure sanal makinelerinde SQL Server'a geçirmek için iyi bir seçenek SQL Server veritabanları ve uygulamalar herhangi bir veritabanı değişiklik olmadan şirket ' dir. Tüm yeni sürümleri ve SQL Server sürümleri bir Iaas sanal makinesine yüklenmesi için kullanılabilir. SQL veritabanı'ndan en önemli fark, SQL Server Vm'leri veritabanı altyapısı üzerinde tam denetime izin ver ' dir. Bakım ve düzeltme, basit kurtarma modelini veya toplu günlük, duraklatmak veya gerektiğinde, engine başlatmak için daha az daha hızlı yük etkinleştirmek için oturum açmış değiştirme başlar ve SQL Server veritabanı altyapısı tam olarak özelleştirebilirsiniz seçebilirsiniz. Bu ek denetim ile sanal makineleri yönetmek için eklenen sorumluluğu ile birlikte gelir.

Bu seçenekler arasındaki başlıca farklar aşağıdaki tabloda listelenmiştir:

| VM’de SQL Server | Yönetilen örnek SQL veritabanı'nda | Tek veritabanı / SQL veritabanı elastik havuz |
| --- | --- | --- |
|SQL Server altyapısı üzerinde tam denetime sahiptir.<br/>% 99,95 düzeyinde kullanılabilirlik kadar.<br/>Şirket içi SQL Server'ın eşleşen sürümünün ile tam eşlik.<br/>Sabit ve iyi bilinen bir veritabanı altyapısı sürümü.<br/>SQL Server şirket içi kolayca geçiş.<br/>Azure sanal ağ içindeki özel IP adresi.<br/>Uygulama veya SQL Server'ın yerleştirildiği ana bilgisayardaki Hizmetleri dağıtmak için sahipsiniz.| Şirket içi SQL Server ile yüksek uyumluluk sağlar.<br/>% 99,99 oranında kullanılabilirlik garantisi.<br/>Düzeltme eki uygulama, Kurtarma yerleşik yedeklemeler.<br/>En son kararlı veritabanı altyapısı sürümü.<br/>SQL Server kolay geçiş.<br/>Azure sanal ağ içindeki özel IP adresi.<br/>Yerleşik Gelişmiş zeka ve güvenlik.<br/>Çevrimiçi kaynakları (CPU/depolama) değiştirin.|En yaygın olarak kullanılan SQL Server özellikleri kullanılabilir.<br/>% 99,99 oranında kullanılabilirlik garantisi.<br/>Düzeltme eki uygulama, Kurtarma yerleşik yedeklemeler.<br/>En son kararlı veritabanı altyapısı sürümü.<br/>Tek veritabanları için gerekli kaynakları (CPU/depolama) atama özelliği.<br/>Yerleşik Gelişmiş zeka ve güvenlik.<br/>Çevrimiçi kaynakları (CPU/depolama) değiştirin.|
|Yedeklemeler ve yamaları yönetmenize olanak gerekir.<br>Kendi yüksek kullanılabilirlik çözümü uygulamak gerekir.<br/>Bir kapalı kalma süresi yoktur resources(CPU/storage) değiştirilirken|Yine de bazı en az sayıda kullanılabilir değil SQL Server özellik yoktur.<br/>Tam bakım süresi (ancak saydam) garanti yok.<br/>SQL Server sürümüyle uyumluluğu, yalnızca veritabanı uyumluluk düzeylerinde kullanılarak gerçekleştirilebilir.|SQL Server'dan Geçiş zor olabilir.<br/>Bazı SQL Server özellikleri kullanılabilir değil.<br/>Tam bakım süresi (ancak saydam) garanti yok.<br/>SQL Server sürümüyle uyumluluğu, yalnızca veritabanı uyumluluk düzeylerinde kullanılarak gerçekleştirilebilir.<br/>Özel IP adresi atanamıyor (güvenlik duvarı kurallarını kullanarak erişimi sınırlayabilirsiniz).|

Her dağıtım seçeneğinin Microsoft Veri platformuna nasıl uyduğunu öğrenin ve iş gereksinimleriniz için doğru seçeneği bulma konusunda yardım alın. Sizin için maliyet tasarrufu ve minimum yönetim tüm diğer unsurlardan önce geliyorsa bu makale, hangi yaklaşımın en fazla önem verdiğiniz iş gereksinimleri açısından en iyi sonucu verdiği konusunda karar vermenize yardımcı olabilir.

## <a name="microsofts-sql-data-platform"></a>Microsoft SQL veri platformu

Azure ile şirket içi SQL Server veritabanlarının karşılaştırmasına yönelik herhangi bir tartışmada anlaşılması gereken ilk unsur, bunların tümünü kullanabileceğinizdir. Microsoft'un veri platformu, SQL Server teknolojisini kullanır ve fiziksel şirket içi makineler, özel bulut ortamları, üçüncü taraflarca barındırılan özel bulut ortamları ve genel bulutta kullanılabilir kılar. Azure sanal makineleri üzerinde SQL Server, söz konusu ortamlar genelinde aynı sunucu ürünleri, geliştirme araçları ve uzmanlık kümesin kullanırken, şirket içi ve bulut üzerinde barındırılan dağıtımların bir birleşimi yoluyla benzersiz ve çeşitli iş gereksinimlerini karşılamanıza olanak sağlar.

   ![SQL Server seçenekleri bulut: SQL server Iaas veya SaaS SQL veritabanı bulutta.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Diyagramda görüldüğü gibi, her bir teklif, altyapı üzerinde sahip olduğunuz yönetim düzeyine (X ekseni üzerinde) ve veritabanı düzeyi birleştirme ve otomasyon işlemleri ile elde edilen maliyet verimliliği (Y ekseni üzerinde) düzeyine göre belirlenebilir.

Bir uygulama tasarlarken, uygulamanın SQL Server kısmını barındırmak için dört temel seçenek mevcuttur:

- Sanallaştırılmamış fiziksel makinelerde SQL Server
- Şirket içi sanallaştırılmış makinelerde SQL Server (özel bulut)
- Azure Sanal Makine'de SQL Server (Microsoft genel bulut)
- Azure SQL Veritabanı (Microsoft genel bulut)

Aşağıdaki bölümlerde, Microsoft Genel bulutta SQL Server hakkında bilgi edinin: Azure SQL veritabanı ve Azure vm'lerinde SQL Server. Ayrıca, hangi seçeneğin uygulamanız için en uygun olduğunu belirlemek üzere genel iş teşviklerini inceleyeceksiniz.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Azure SQL Veritabanı ve Azure VM'lerinde SQL Server'a daha ayrıntılı bir bakış

Genellikle, bu iki SQL seçeneği farklı amaçlar için en iyi hale getirilmiştir:

- **Azure SQL Veritabanı**

Çok sayıda veritabanının hazırlanması ve yönetilmesi için minimum genel yönetim maliyetlerini azaltmak için en iyi duruma getirilmiş. Herhangi bir sanal makineyi, işletim sistemini veya veritabanı yazılımını yönetmeniz gerekmediğinden, bu, devam eden yönetim maliyetlerini azaltır. Yükseltme, yüksek kullanılabilirlik veya [yedeklemeleri](sql-database-automated-backups.md) yönetmeniz gerekli değildir. Genellikle, Azure SQL Veritabanı tek bir BT veya geliştirme kaynağı tarafından yönetilen veritabanlarının sayısını önemli ölçüde artırabilir. [Elastik havuzlar](sql-database-elastic-pool.md) de Kiracı yalıtımı ve veritabanları arasında kaynakların paylaşılması, maliyetleri azaltmak için ölçeği olanağı gibi özellikler ile SaaS çok kiracılı uygulama mimarileri destekler. [Yönetilen örnek](sql-database-managed-instance.md) veritabanları arasında kaynakların paylaşılması yanı sıra mevcut uygulamaların kolayca taşınmasına etkinleştirme örneği kapsamlı özellikler için destek sağlar.

- **Azure Vm'lerinde çalışan SQL Server**

Mevcut uygulamaları azure'a geçirme veya var olan şirket içi uygulamaları karma dağıtımlarda buluta genişletmek için en iyi duruma getirilmiş. Ayrıca, SQL Server’ı sanal bir makinede kullanarak geleneksel SQL Server uygulamaları geliştirip test edebilirsiniz. Azure VM'lerinde SQL Server ile, adanmış bir SQL Server örneği ve bulut tabanlı bir VM üzerinde tam yönetici haklarına sahip olursunuz. Bu, bir kuruluşun halihazırda sanal makinelerin bakımını yapmak için kullanılabilir BT kaynaklarına sahip olması durumunda ideal bir seçenektir. Bu özellikler uygulamanızın belirli performans ve kullanılabilirlik gereksinimlerini karşılamak üzere yüksek düzeyde özelleştirilmiş bir sistem oluşturmanıza olanak sağlar.

Aşağıdaki tabloda, SQL Database ve Azure VM'lerinde SQL Server'ın temel özellikleri özetlenmektedir:

| | SQL veritabanı tek veritabanları ve elastik havuzlar | SQL veritabanı yönetilen örneği |SQL Server ile Azure sanal makineleri |
| --- | --- | --- |---|
| **En iyi kullanım alanı:** |En son kararlı SQL Server özelliklerini kullanma ve geliştirme ve pazarlama alanında zaman kısıtlamaları sahip olmak istiyorsanız yeni bulut Tasarımlı uygulamalar. | Yeni uygulamalar veya en son kararlı SQL Server özelliklerini ve, kullanmak istediğiniz var olan şirket içi uygulamalar, minimum değişiklikle buluta geçirilir.  | Küçük değişiklikler veya hiç değişiklik ile buluta hızlı geçiş gerektiren var olan uygulamalar. Şirket içi üretim dışı SQL Server donanımı satın almak istemediğinizde hızlı geliştirme ve test senaryoları. |
|  | Veritabanı için yerleşik yüksek kullanılabilirlik, olağanüstü durum kurtarma ve yükseltme mekanizmalarına gereksinim duyan ekipler. | SQL veritabanı tek ve havuza alınmış veritabanları ile aynıdır. | Yapılandırabilirsiniz, takımlar düzgün ayarlamak, özelleştirme ve yüksek kullanılabilirlik, olağanüstü durum kurtarma ve SQL Server için düzeltme eki uygulama yönetin. Sağlanan bazı otomatik özellikler bunu önemli ölçüde basitleştirir. |
|  | Altta yatan işletim sistemi ve yapılandırma ayarlarını yönetmek istemeyen ekipler. | SQL veritabanı tek ve havuza alınmış veritabanları ile aynıdır. | Tam yönetici haklarına sahip özelleştirilmiş bir ortama ihtiyacınız vardır. |
|  | En fazla 100 TB veritabanları. | 8 TB'a kadar artırın. | 64 TB’ye varan depolama alanına sahip SQL Server örnekleri. Örnek gereken sayıda veritabanını destekleyebilir. |
| **Uyumluluk** | Çoğu şirket içi veritabanı düzeyinde özelliklerini destekler. | Neredeyse tüm destekleyen şirket içi örnek düzeyinde ve veritabanı düzeyinde özellikleri. | Tüm şirket içi özelliklerini destekler. |
| **Kaynaklar:** | Yapılandırma ve altyapı yönetimi için BT kaynaklarını kullanmak istemiyorsunuz istemediğiniz ancak uygulama katmanına odaklanmak istiyorsunuz. | SQL veritabanı tek ve havuza alınmış veritabanları ile aynıdır. | Yapılandırma ve yönetim için bazı BT kaynaklarına sahipsiniz. Sağlanan bazı otomatik özellikler bunu önemli ölçüde basitleştirir. |
| **Toplam sahip olma maliyeti:** | Donanım maliyetlerini ortadan kaldırır ve yönetim maliyetlerini azaltır. | SQL veritabanı tek ve havuza alınmış veritabanları ile aynıdır. | Donanım maliyetlerini ortadan kaldırır. |
| **İş sürekliliği:** |Ek olarak [yerleşik hata toleransı altyapı özelliklerine](sql-database-high-availability.md), Azure SQL veritabanı özellikleri gibi sağlar [otomatik yedeklemeler](sql-database-automated-backups.md), [-belirli bir noktaya geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore), [coğrafi geri yükleme](sql-database-recovery-using-backups.md#geo-restore), [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md), ve [otomatik yük devretme grupları](sql-database-auto-failover-group.md) iş sürekliliğini artırmak üzere. Daha fazla bilgi için bkz. [SQL Database iş sürekliliğine genel bakış](sql-database-business-continuity.md). | SQL veritabanı tek ve havuza alınmış veritabanlarını yanı sıra, kullanıcı tarafından başlatılan, yalnızca kopya yedekleri aynı kullanılabilir. | Azure VM’lerde SQL Server, veritabanınızın belirli gereksinimleri için bir yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü ayarlamanıza olanak sağlar. Böylece, uygulamanız için en iyi hale getirilmiş bir sisteme sahip olabilirsiniz. Yük devretme işlemlerini ihtiyaç duyulduğunda kendi kendinize test edebilir ve çalıştırabilirsiniz. Daha fazla bilgi için bkz. [Azure Virtual Machines'de SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md). |
| **Karma bulut:** |Şirket içi uygulamanız, Azure SQL Veritabanı'ndaki verilere erişebilir. | [Yerel sanal ağ uygulaması](sql-database-managed-instance-vnet-configuration.md) ve Azure Express Route veya VPN ağ geçidi kullanarak şirket içi ortamınıza bir bağlantı. | Azure VM'lerinde SQL Server ile kısmen bulutta ve kısmen şirket içinde çalıştırılan uygulamalara sahip olabilirsiniz. Örneğin, şirket içi ağınızı ve Active Directory Etki Alanı'nı [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) üzerinden buluta genişletebilirsiniz. Hibrit bulut çözümleri hakkında daha fazla bilgi için bkz. [şirket içi veri çözümlerini buluta genişletme](https://docs.microsoft.com/azure/architecture/data-guide/scenarios/hybrid-on-premises-and-cloud). |
|  | Verileri çoğaltmak için abone olarak [SQL Server işlem çoğaltmayı](https://msdn.microsoft.com/library/mt589530.aspx) destekler. | Çoğaltma için yönetilen örnek, bir önizleme özelliği olarak desteklenir. | Tam olarak destekler [SQL Server işlem çoğaltmayı](https://msdn.microsoft.com/library/mt589530.aspx), [Always On kullanılabilirlik grupları](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), Integration Services ve günlük aktarma veri çoğaltmak için. Ayrıca, geleneksel SQL Server yedeklemeleri tam olarak desteklenir |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Azure VM'lerinde Azure SQL Veritabanı'in veya SQL Server'ın seçilmesine yönelik iş faydaları

PaaS veya Iaas SQL veritabanlarınızın barındırılmasına yönelik seçme kararınızı etkileyebilir birkaç faktör vardır:

- [Maliyet](#cost) -hem PaaS ve Iaas seçeneği içeren temel fiyatı kapsayan temel altyapıyı ve lisanslama. Ancak, Iaas seçeneğiyle ek zaman ve kaynak Paas'ta fiyatına dahil bu yönetim özelliklerini alıyorsanız sırada veritabanınızı yönetmek için almaları gerekir. Iaas seçeneği kaynaklarınızı, PaaS sürümü her zaman sürece devam ederken maliyeti düşürmek için kullanmadığınız sırada kapalı olanak tanır, bırak ve gerektiğinde, kaynaklarınızı yeniden oluşturun.
- [Yönetim](#administration) -PaaS seçenekleri için veritabanını yönetmek için almaları için gereken süreyi azaltın. Ancak, özel yönetim görevleri ve betikleriniz gerçekleştirmek veya çalıştırmak istediğiniz aralığını da sınırlar. Örneğin, CLR tek veya havuza alınmış veritabanlarıyla desteklenmez, ancak bir yönetilen örnek için desteklenir. Ayrıca, PaaS herhangi bir dağıtım seçeneği, izleme bayrakları kullanımını destekler.
- [Hizmet düzeyi sözleşmesi](#service-level-agreement-sla) -hem Iaas ve PaaS sağlayan yüksek, sektörde standart SLA. Veritabanlarınızı kullanılabilirliğini sağlamak için ek bir mekanizma uygulamak gereken anlamı Iaas altyapısı için % 99,95 oranında SLA'sı garanti etse PaaS seçeneği % 99,99 SLA'sı garanti eder. PaaS, eşleşen bir yüksek kullanılabilirlik çözümü uygulamak istiyorsanız, olağanüstü durumda, ek SQL Server VM oluşturun ve çift veritabanınızı maliyeti olabilir, AlwaysOn Kullanılabilirlik Grupları Yapılandırma gerekebilir.
- [Azure'a taşıma zamanı](#market) -Azure VM'de SQL Server ortamınızın, tam bir eşleşme olan Azure SQL sanal makinesi şirket içinden geçiş veritabanlarını diğerine taşıma başka bir sunucuya şirket içinde çok farklı değildir. Yönetilen örnek ayrıca, son derece kolay geçiş sağlar; Ancak, bir yönetilen örneğe geçiş yapmadan önce uygulamanız gereken bazı değişiklikler olabilir.

Bu etkenler aşağıdaki bölümlerde daha ayrıntılı açıklanmıştır.

### <a name="cost"></a>Maliyet

İster nakit ihtiyacında olan yeni bir girişim ister sıkı bütçe kısıtlamaları altında işletilen yerleşik bir şirket bünyesindeki bir ekip olun, sınırlı fon genellikle veritabanlarınızın barındırılmasına yönelik kararların verilmesinde en önemli etmendir. Bu bölümde, faturalama ve Azure temel bilgileri şu iki ilişkisel veritabanı seçenekleri bakımından lisanslama hakkında bilgi edinin: SQL veritabanı ve Azure vm'lerinde SQL Server. Ayrıca toplam uygulama maliyetini hesaplama hakkında bilgi edineceksiniz.

#### <a name="billing-and-licensing-basics"></a>Faturalama ve lisanslama ile ilgili temel bilgiler

Şu anda **SQL veritabanı** hizmet olarak satılır ve çeşitli dağıtım seçenekleriyle ve kaynakların her biri faturalandırılır saatlik hizmet katmanına bağlı olarak sabit fiyat, farklı fiyatlarla birkaç hizmet katmanlarında kullanılabilir ve Seçtiğiniz işlem boyutu. En yeni bilgileri desteklenen geçerli hizmet katmanları, boyutları ve depolama alanı miktarları için bkz: işlem [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).

- SQL veritabanı tek veritabanı ile çok çeşitli temel katman için 5$ aylık başlangıç fiyatları gelen ihtiyaçlarınıza uygun bir hizmet katmanı seçebilirsiniz.
- Oluşturabileceğiniz [elastik havuzlar](sql-database-elastic-pool.md) kaynakları maliyetleri azaltmak ve kullanım uyum sağlamak için veritabanı örnekleri arasında paylaşmak için ani.
- SQL veritabanı yönetilen örnek sayesinde, ayrıca kendi lisansınızı getirebilirsiniz. Getir kendi lisanslama ile ilgili daha fazla bilgi için bkz: [azure'de Yazılım Güvencesiyle lisans taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility/) veya [Azure hibrit avantajı hesaplayıcı](https://azure.microsoft.com/pricing/hybrid-benefit/#sql-database) görmek için nasıl **en fazla %40Kaydet**.

Buna ek olarak, giden Internet trafiği için normal [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) faturalandırılırsınız. Dinamik olarak ayarlamak hizmet katmanları ve boyutları, uygulamanızın değişken verimlilik ihtiyaçlarını karşılamak için işlem kullanabilirsiniz.

**SQL Database** ile, veritabanı yazılımına Microsoft tarafından otomatik olarak yapılandırma, düzeltme eki ve yükseltme uygulanır; bu da yönetim maliyetlerinizi azaltır. Ayrıca, [yerleşik yedekleme](sql-database-automated-backups.md) özellikleri, özellikle çok sayıda veritabanınız olduğunda önemli maliyet tasarrufları sağlamanıza yardımcı olur.

**Azure VM’lerde SQL Server** ile platform tarafından sağlanan SQL Server görüntülerinin herhangi birini (lisans içerir) kullanabilir veya kendi SQL Server lisansınızı getirebilirsiniz. Desteklenen tüm SQL Server sürümleri (2008R2, 2012, 2014, 2016) (Developer, Express, Web, Standard, Enterprise) kullanılabilir. Ayrıca, görüntülerin Kendi Lisansını Getir sürümleri (KLG) mevcuttur. Azure tarafından sağlanan görüntüler kullanıldığında, işletim maliyeti, VM boyutunun yanı sıra seçtiğiniz SQL Server sürümüne bağlıdır. VM boyutundan veya SQL Server sürümünden bağımsız olarak, dakika başına VM diskleri için Azure Storage maliyetinin yanı sıra, SQL Server ve Windows veya Linux Sunucu Lisanslama ücreti ödersiniz. Dakika başına faturalandırma seçeneği, SQL Server'ı ek SQL Server lisansları satın almadan istediğiniz süreyle kullanmanıza olanak sağlar. Azure'a kendi SQL Server lisansınızı getirirseniz, sunucu ve depolama maliyetleri için ücretlendirilir. Kendi lisansını getir seçeneği hakkında daha fazla bilgi için bkz. [Azure'da Yazılım Güvencesi ile Lisans Mobilitesi](https://azure.microsoft.com/pricing/license-mobility/). Buna ek olarak, giden Internet trafiği için normal [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) faturalandırılırsınız.

#### <a name="calculating-the-total-application-cost"></a>Toplam uygulama maliyetini hesaplama

Bir bulut platformunu kullanmaya başladığınızda, uygulamanızı çalıştırmanın maliyeti yeni yazılım geliştirme maliyetini ve devam eden yönetim maliyetlerini yanı sıra genel bulut platformu hizmet maliyetlerini içerir.

**Azure SQL Veritabanı kullanıldığında:**

- Yüksek oranda azaltılmış yönetim maliyetleri
- Geçirilen uygulamaları (yönetilen örnekler) için sınırlı geliştirme maliyetleri
- SQL veritabanı hizmet maliyetleri
- Donanım satın alma maliyeti olmadan

**Azure VM'lerinde SQL Server kullanıldığında:**

- Daha yüksek yönetim maliyetlerine
- Geçirilen uygulamaları için geliştirme ücreti alınmaz sınırlıdır
- Sanal makine hizmet maliyetleri
- Donanım satın alma maliyeti olmadan

Fiyatlandırma konusunda daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/)
- [Sanal makine fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/) için [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
- [Azure Fiyatlandırma Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Yönetim

Birçok işletme için, bir bulut hizmetine geçiş yapma kararı, maliyetle ilgili olduğu kadar, yönetim karmaşıklığı yükünü gidermekle de ilgilidir. Iaas ve PaaS sayesinde Microsoft temel altyapıyı yönetir ve otomatik olarak olağanüstü durum kurtarma sağlamak için tüm verileri çoğaltan, yapılandırır ve veritabanı yazılımına yükseltir, yük dengelemeyi yönetir ve olması durumunda saydam yük devretme gerçekleştirir bir bir veri merkezi içinde sunucu hatası.

- İle **Azure SQL veritabanı**veritabanınızı yönetmeye devam edebilirsiniz ancak artık veritabanı altyapısı, işletim sistemini veya donanımı yönetmeniz gerekmez. Yönetmeye devam edebileceğiniz öğelere örnek olarak veritabanları ve oturum açma bilgileri, dizin ve sorgu ayarları ile denetim ve güvenlik verilebilir. Ayrıca, başka bir veri merkezine yüksek kullanılabilirliği yapılandırma minimal yapılandırma ve yönetim gerektirir.
- **Azure VM’lerde SQL Server** ile işletim sistemi ve SQL Server örneği yapılandırması üzerinde tam denetime sahip olursunuz. Bir VM ile, işletim sisteminin ve veritabanı yazılımının ne zaman güncelleştirileceğine/yükseltileceğine ve virüsten koruma araçları gibi herhangi bir ek yazılımın ne zaman yükleneceğine ilişkin karar size aittir. Düzeltme eki uygulama, yedekleme ve yüksek kullanılabilirliği önemli ölçüde kolaylaştırmak amacıyla bazı otomatik özellikler sağlanır. Ayrıca, VM'nin boyutunu, disk sayısını ve disklerin depolama yapılandırmalarını denetleyebilirsiniz. Azure gerektiğinde bir VM’nin boyutunu değiştirmenize izin verir. Bilgi için bkz. [Azure için Sanal Makine ve Bulut Hizmeti Boyutları](../virtual-machines/windows/sizes.md).

### <a name="service-level-agreement-sla"></a>Hizmet Düzeyi Sözleşmesi (SLA)

Birçok BT departmanı için, bir Hizmet Düzeyi Sözleşmesi'nin çalışma süresi yükümlülüklerinin yerine getirilmesi birincil önceliğe sahiptir. Bu bölümde, her bir veritabanı barındırma seçeneği için geçerli olan SLA'yı ele alacağız.

İçin **SQL veritabanı**, Microsoft SLA'sı % 99,99 kullanılabilirlik sağlar. En son bilgiler için bkz. [Hizmet Düzeyi Sözleşmesi](https://azure.microsoft.com/support/legal/sla/sql-database/).

**Azure VM'lerinde çalıştırılan SQL Server** için Microsoft, yalnızca Sanal Makine'yi kapsayan %99,95 oranında bir kullanılabilirlik SLA'sı sağlar. Bu SLA, VM'de çalıştırılan işlemleri (SQL Server gibi) kapsamaz ve bir kullanılabilirlik kümesinde en az iki VM örneğini barındırmanızı gerektirir. En son bilgiler için bkz. [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Veritabanı yüksek kullanılabilirlik (HA) vm'lerde, desteklenen yüksek kullanılabilirlik seçeneklerinden birini SQL Server'da aşağıdaki gibi yapılandırmanız gerekiyor [Always On kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server). Desteklenen bir yüksek kullanılabilirlik seçeneğinin kullanılması ek bir SLA sağlamaz, ancak %99,99’un üzerinde veritabanı kullanılabilirliği elde etmenize imkan tanır.

### <a name="market"></a>Azure'a taşıma zamanı

**SQL veritabanı tek veritabanlarını isterseniz elastik havuzları** Geliştirici üretkenliği ve hızlı zaman pazara açılma yeni çözümleri için kritik önem taşıdığında bulut Tasarımlı uygulamalarınız için doğru çözüm olan. Programlama DBA benzeri işlevsellik ile, altta yatan işletim sistemini ve veritabanını yönetmeye yönelik ihtiyacı azalttığından, bulut mimarları ve geliştiricileri için idealdir.

**SQL veritabanı yönetilen örneği** geçirilen veritabanı uygulamalarının Azure'da hızla pazara Getir olanak sağlayan, Azure SQL veritabanı mevcut uygulamaların geçişi büyük ölçüde basitleştirir.

**Azure Vm'lerinde çalışan SQL Server** mevcut veya yeni uygulamalarınızın büyük veritabanları gerektirir veya SQL Server veya Windows/Linux, tüm özelliklerine erişmek ve zaman kaçınmak istiyorsanız ve harcama yeni alınırken, şirket içi donanım mükemmel bir seçimdir. Aynı zamanda uygun olduğunda var olan geçirmek istediğiniz şirket içi uygulamaları ve veritabanları olarak azure'a olan-- Azure SQL veritabanı yönetilen örneği olmadığı yerde uygun durumlarda değildir. Sunu, uygulama ve veri katmanlarını değiştirmeniz gerekmediğinden, zaman ve bütçe var olan çözümünüzü yeniden tasarlama hakkında kaydedin. Bunun yerine, tüm çözümlerinizin Azure'a geçişini sağlamaya ve Azure platformu tarafından gerekli kılınabilen bazı performans iyileştirmelerini gerçekleştirmeye odaklanabilirsiniz. Daha fazla bilgi için bkz. [Azure Virtual Machines'de SQL Server için En İyi Performans Uygulamaları](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md).

## <a name="next-steps"></a>Sonraki adımlar

- SQL Veritabanı’nı kullanmaya başlamak için bkz. [İlk Azure SQL Veritabanınız](sql-database-single-database-get-started.md).
- Bkz. [SQL Veritabanı Fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).
- Azure VM'lerinde SQL Server'ı kullanmaya başlamak için bkz. [Azure'da SQL Server sanal makinesi sağlama](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).
