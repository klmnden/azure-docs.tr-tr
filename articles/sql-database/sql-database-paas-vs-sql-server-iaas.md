---
title: SQL (PaaS) Veritabanı ile VM'lerdeki bulutta bulunan SQL Server (IaaS) karşılaştırması | Microsoft Docs
description: "Hangi bulut SQL Server seçeneğinin uygulamanıza uygun olduğunu öğrenin: Azure SQL (PaaS) Veritabanı veya Azure Virtual Machines'deki bulutta bulunan SQL Server."
services: sql-database, virtual-machines
keywords: SQL Server bulut, bulutta SQL Server, PaaS veritabanı, bulut SQL Server, DBaaS
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 2fb5a7cbca4df0faa06864f580814f31cc2b609c
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114410"
---
# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Bir bulut SQL Server seçeneği seçin: Azure SQL (PaaS) Veritabanı ya da Azure VM'lerde SQL Server (IaaS)

Azure üzerinde barındırılan bir altyapısı (Iaas) çalışan veya barındırılan bir hizmet olarak çalışması, SQL Server iş yüklerini olabilir ([PaaS](https://azure.microsoft.com/overview/what-is-paas/)):

* [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/): Kurumsal SQL modern uygulama geliştirme için en iyi duruma getirilmiş Server sürümü, temel bir SQL veritabanı altyapısı. Azure SQL veritabanı SQL barındırılan hizmet olarak iki sürümü sunar: mantıksal sunucuları ve [Azure SQL veritabanı yönetilen örnekleri (Önizleme)](sql-database-managed-instance.md). Her iki sürümleri ile Azure SQL veritabanı, SQL Server'da yerleşik zekaya ve yönetim gibi kullanılabilir olmayan ek özellikler ekler. İlk sürümüyle içeren bir mantıksal sunucu olabilir [tek veritabanlarını](sql-database-servers-databases.md) ve sunucuları halinde gruplandırabilirsiniz bir [esnek havuz](sql-database-elastic-pool.md) kaynakları paylaşır ve maliyetlerini azaltmak için. Tek ve havuza veritabanlarını içeren bir Azure SQL Database mantıksal sunucusu, SQL Server veritabanı kapsamlı özelliklerinin çoğunu sağlar. Azure SQL veritabanı yönetilen örnek ile Azure SQL veritabanı paylaşılan kaynakları veritabanları ve ek örneği kapsamlı özellikler sunar. Yönetilen Azure SQL veritabanı örneği ile veritabanı geçiş destekleyen herhangi bir veritabanı değişiklik için en az.
* [Azure Virtual Machines'de SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server yüklü ve bulutta Azure, olarak da bilinen bir altyapı (ıaas) olarak çalışan Windows Server veya Linux sanal makineleri (VM'ler) üzerinde barındırılan. SQL Azure sanal makinelerde geçirmek için iyi bir seçenek SQL Server veritabanları ve uygulamaları herhangi bir veritabanı değişiklik olmadan şirket içi sunucusudur. Tüm son sürümleri ve SQL Server sürümleri bir Iaas sanal makinede yükleme için kullanılabilir. SQL veritabanı en önemli fark, SQL Server Vm'lerinin veritabanı altyapısı üzerinde tam denetime izin ver ' dir. Bakım ve düzeltme, basit kurtarma modeline veya toplu günlük, duraklatmak veya gerekli olduğunda, altyapı başlatmak için daha az daha hızlı yük etkinleştirmek için oturum açmış değiştirmek için başlar ve SQL Server veritabanı motorunun tam olarak özelleştirebilirsiniz seçebilirsiniz. Bu ek denetimi ile sanal makineleri yönetmek için eklenen sorumluluk gelir.

Her bir seçeneğin Microsoft veri platformuna nasıl uyduğunu öğrenin ve iş gereksinimleriniz için doğru seçeneği bulma konusunda yardım alın. Sizin için maliyet tasarrufu ve minimum yönetim tüm diğer unsurlardan önce geliyorsa bu makale, hangi yaklaşımın en fazla önem verdiğiniz iş gereksinimleri açısından en iyi sonucu verdiği konusunda karar vermenize yardımcı olabilir.

## <a name="microsofts-sql-data-platform"></a>Microsoft SQL veri platformu

Azure ile şirket içi SQL Server veritabanlarının karşılaştırmasına yönelik herhangi bir tartışmada anlaşılması gereken ilk unsur, bunların tümünü kullanabileceğinizdir. Microsoft'un veri platformu, SQL Server teknolojisini kullanır ve fiziksel şirket içi makineler, özel bulut ortamları, üçüncü taraflarca barındırılan özel bulut ortamları ve genel bulutta kullanılabilir kılar. Azure sanal makineleri üzerinde SQL Server, söz konusu ortamlar genelinde aynı sunucu ürünleri, geliştirme araçları ve uzmanlık kümesin kullanırken, şirket içi ve bulut üzerinde barındırılan dağıtımların bir birleşimi yoluyla benzersiz ve çeşitli iş gereksinimlerini karşılamanıza olanak sağlar.

   ![Bulut SQL Server seçenekleri: IaaS üzerinde SQL sunucusu veya bulutta SaaS veritabanı.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Diyagramda görüldüğü gibi, her bir teklif, altyapı üzerinde sahip olduğunuz yönetim düzeyine (X ekseni üzerinde) ve veritabanı düzeyi birleştirme ve otomasyon işlemleri ile elde edilen maliyet verimliliği (Y ekseni üzerinde) düzeyine göre belirlenebilir.

Bir uygulama tasarlarken, uygulamanın SQL Server kısmını barındırmak için dört temel seçenek mevcuttur:

* Sanallaştırılmamış fiziksel makinelerde SQL Server
* Şirket içi sanallaştırılmış makinelerde SQL Server (özel bulut)
* Azure Sanal Makine'de SQL Server (Microsoft genel bulut)
* Azure SQL Database (Microsoft genel bulut)

Aşağıdaki bölümlerde, Microsoft genel bulutta SQL Server ile ilgili bilgi edineceksiniz: Azure SQL Veritabanı ve Azure VM’lerinde SQL Server. Ayrıca, hangi seçeneğin uygulamanız için en uygun olduğunu belirlemek üzere genel iş teşviklerini inceleyeceksiniz.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Azure SQL Database ve Azure VM'lerinde SQL Server'a daha ayrıntılı bir bakış

**Azure SQL veritabanı** bir ilişkisel veritabanı-a-endüstri kategorisine girer Azure bulutta barındırılan hizmet olarak (DBaaS) *Platform olarak-hizmet (PaaS)*. [SQL veritabanı](sql-database-technical-overview.md), Microsoft'un sahibi olduğu, barındırdığı ve bakımını yaptığı standartlaştırılmış donanım ve yazılım üzerinde geliştirilmiştir. SQL Database ile yerleşik özellikleri ve SQL Server'daki kapsamlı yapılandırma gerektiren işlevlerini kullanabilirsiniz. SQL Database'i kullandığınızda, kesinti olmadan daha fazla güç için ölçeğinizi artırmaya veya genişletmeye yönelik seçeneklerle kullandıkça ödersiniz. Her ikisi için de destek ile Azure SQL veritabanı [tek veritabanlarını](sql-database-servers-databases.md) ve [esnek havuzlar](sql-database-elastic-pool.md) kaynaklar için bulutta yeni uygulamalar geliştirmek için ideal bir ortam paylaşımıdır. Ve ile [yönetilen Azure SQL veritabanı örneği](sql-database-managed-instance.md), kendi lisansınızı getirebilirsiniz. Ayrıca, bu seçenek, tüm Azure SQL veritabanı PaaS avantajları sağlar ancak daha önce SQL Vm'lerinin yalnızca kullanılabilir özellikleri ekler. Bu yerel bir sanal ağ (VNet) içerir ve şirket içi SQL Server ile % 100 uyumluluk yakınında. [Örnek yönetilen](sql-database-managed-instance.md) şirket içi geçişler Azure için gereken en küçük değişikliklerle veritabanı için idealdir. 

**Azure Virtual Machines'de (VM'ler) SQL Server**, *Hizmet olarak Altyapı (IaaS)* endüstri kategorisine girer ve buluttaki bir sanal makine içinde SQL Server'ı çalıştırmanıza olanak sağlar. [SQL Server sanal makineleri](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) da ait, barındırılan ve bakımını yaptığı standartlaştırılmış donanım üzerinde çalıştırın. SQL Server VM üzerinde kullanırken ya da ödeme olabilecek-gibi bir SQL Server görüntüsü zaten bulunan bir SQL Server Lisans Git ya da varolan bir lisans kolayca kullanın. Ayrıca, durdurmak veya VM gerektiği gibi devam ettirebilirsiniz.

Genellikle, bu iki SQL seçeneği farklı amaçlar için en iyi hale getirilmiştir:

* **Azure SQL veritabanı** sağlama ve birçok veritabanı yönetmek için en düşük için genel yönetim maliyetlerini azaltmak için optimize edilmiştir. Herhangi bir sanal makineyi, işletim sistemini veya veritabanı yazılımını yönetmeniz gerekmediğinden, bu, devam eden yönetim maliyetlerini azaltır. Yükseltme, yüksek kullanılabilirlik veya [yedeklemeleri](sql-database-automated-backups.md) yönetmeniz gerekli değildir. Genellikle, Azure SQL Database tek bir BT veya geliştirme kaynağı tarafından yönetilen veritabanlarının sayısını önemli ölçüde artırabilir. [Esnek havuzlar](sql-database-elastic-pool.md) da Kiracı yalıtımı ve veritabanları arasında kaynak paylaşımı tarafından maliyetlerini azaltmak için ölçeklendirmenizi gibi özelliklerle SaaS çok kiracılı uygulama mimarisi destekler. [Yönetilen Azure SQL veritabanı örneği](sql-database-managed-instance.md) veritabanları arasında kaynak paylaşımı yanı sıra var olan uygulamaların kolay geçiş etkinleştirme örneği kapsamlı özellikler için destek sağlar.
* **Azure VM’ler üzerinde çalışan SQL Server**, var olan uygulamaları Azure’a geçirmek veya var olan şirket içi uygulamaları karma dağıtımlarda buluta genişletmek için en iyi duruma getirilmiştir. Ayrıca, SQL Server’ı sanal bir makinede kullanarak geleneksel SQL Server uygulamaları geliştirip test edebilirsiniz. Azure VM'lerinde SQL Server ile, adanmış bir SQL Server örneği ve bulut tabanlı bir VM üzerinde tam yönetici haklarına sahip olursunuz. Bu, bir kuruluşun halihazırda sanal makinelerin bakımını yapmak için kullanılabilir BT kaynaklarına sahip olması durumunda ideal bir seçenektir. Bu özellikler uygulamanızın belirli performans ve kullanılabilirlik gereksinimlerini karşılamak üzere yüksek düzeyde özelleştirilmiş bir sistem oluşturmanıza olanak sağlar.

Aşağıdaki tabloda, SQL Database ve Azure VM'lerinde SQL Server'ın temel özellikleri özetlenmektedir:

| | Azure SQL Database<br>Mantıksal sunucu, esnek havuzlar ve tek veritabanları | Azure SQL Database<br>Yönetilen Örnek |Azure Sanal Makinesi<br>SQL Server |
| --- | --- | --- |---|
| **En iyi kullanım alanı:** |İstediğiniz yeni bulut Tasarımlı uygulamalar en son kararlı SQL Server özellikleri andhave alanında zaman kısıtlamaları geliştirme ve pazarlama kullanın. | Yeni uygulamalar veya en son kararlı SQL Server özellikleri ve, kullanmak istediğiniz mevcut şirket içi uygulamalar minimum değişiklikle buluta geçirilir.  | Küçük değişiklikler veya herhangi bir değişiklik olan buluta hızlı geçiş gerektiren var olan uygulamalar. Şirket içi üretim dışı SQL Server donanımı satın almak istemediğinizde hızlı geliştirme ve test senaryoları. |
|  | Veritabanı için yerleşik yüksek kullanılabilirlik, olağanüstü durum kurtarma ve yükseltme mekanizmalarına gereksinim duyan ekipler. | SQL veritabanı ile aynıdır. | Yapılandırabilirsiniz, takımlar ince ayarlamak, özelleştirme ve yüksek kullanılabilirlik, olağanüstü durum kurtarma ve SQL Server için düzeltme eki uygulama yönetme. Sağlanan bazı otomatik özellikler bunu önemli ölçüde basitleştirir. | |
|  | Altta yatan işletim sistemi ve yapılandırma ayarlarını yönetmek istemeyen ekipler. | SQL veritabanı ile aynıdır. | Tam yönetici haklarına sahip bir özelleştirilmiş ortamı gerekir. | |
|  | En fazla 4 TB'lık veritabanları veya olabilir büyük veritabanları [yatay veya dikey olarak bölümlenmiş](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) genişleme desenini kullanarak. | SQL veritabanı ile aynıdır. | 64 TB’ye varan depolama alanına sahip SQL Server örnekleri. Örnek gereken sayıda veritabanını destekleyebilir. |
| **Uyumluluk** | Çoğu şirket içi veritabanı düzeyi özelliklerini destekler. | Desteklenen neredeyse tüm şirket içi örneği ve veritabanı düzeyi özellikleri. | Tüm şirket içi özelliklerini destekler. |
| **Kaynaklar:** | Temel alınan altyapının yapılandırma ve yönetimi için BT kaynakları kullanmak istemiyorsunuz, ancak uygulama katmanına odaklanmak istiyorsunuz. | SQL veritabanı ile aynıdır. | Yapılandırma ve yönetim için bazı BT kaynaklarına sahipsiniz. Sağlanan bazı otomatik özellikler bunu önemli ölçüde basitleştirir. |
| **Toplam sahip olma maliyeti:** | Donanım maliyetlerini ortadan kaldırır ve yönetim maliyetlerini azaltır. | SQL veritabanı ile aynıdır. | Donanım maliyetlerini ortadan kaldırır. |
| **İş sürekliliği:** |Ek olarak [yerleşik hata toleransı altyapı özelliklerine](sql-database-high-availability.md), Azure SQL veritabanı sağlar özellikleri gibi [yedeklemeleri otomatik](sql-database-automated-backups.md), [noktası zaman geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore), [coğrafi geri yükleme](sql-database-recovery-using-backups.md#geo-restore), ve [yük devretme grupları ve etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md) iş sürekliliğini artırmak üzere. Daha fazla bilgi için bkz. [SQL Database iş sürekliliğine genel bakış](sql-database-business-continuity.md). | SQL veritabanı yanı sıra, kullanıcı tarafından başlatılan, yalnızca kopya yedekleri aynı kullanılabilir. | Azure VM’lerde SQL Server, veritabanınızın belirli gereksinimleri için bir yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü ayarlamanıza olanak sağlar. Böylece, uygulamanız için en iyi hale getirilmiş bir sisteme sahip olabilirsiniz. Yük devretme işlemlerini ihtiyaç duyulduğunda kendi kendinize test edebilir ve çalıştırabilirsiniz. Daha fazla bilgi için bkz. [Azure Virtual Machines'de SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md). |
| **Karma bulut:** |Şirket içi uygulamanız, Azure SQL Database'deki verilere erişebilir. | [Yerel sanal ağ uygulaması](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-managed-instance-vnet-configuration) ve Azure Express rota ya da VPN ağ geçidi kullanarak şirket içi ortamınıza bağlantı. | Azure VM'lerinde SQL Server ile kısmen bulutta ve kısmen şirket içinde çalıştırılan uygulamalara sahip olabilirsiniz. Örneğin, şirket içi ağınızı ve Active Directory Etki Alanı'nı [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) üzerinden buluta genişletebilirsiniz. Ek olarak, [Azure'da SQL Server Veri Dosyaları](http://msdn.microsoft.com/library/dn385720.aspx)'nı kullanarak şirket içi veri dosyalarını Azure Storage'da depolayabilirsiniz. Daha fazla bilgi için bkz. [SQL Server 2014 Karma Bulutu'na giriş](http://msdn.microsoft.com/library/dn606154.aspx). |
|  | Verileri çoğaltmak için abone olarak [SQL Server işlem çoğaltmayı](https://msdn.microsoft.com/library/mt589530.aspx) destekler. | Çoğaltma, Azure SQL veritabanı yönetilen örneği için desteklenmiyor. | Tam olarak destekler [SQL Server işlem çoğaltma](https://msdn.microsoft.com/library/mt589530.aspx), [Always On kullanılabilirlik grupları](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), Integration Services ve günlük aktarma veri çoğaltmak için. Ayrıca, geleneksel SQL Server yedeklemeleri tam olarak desteklenir | |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Azure VM'lerinde Azure SQL Database'in veya SQL Server'ın seçilmesine yönelik iş faydaları
### <a name="cost"></a>Maliyet
İster nakit ihtiyacında olan yeni bir girişim ister sıkı bütçe kısıtlamaları altında işletilen yerleşik bir şirket bünyesindeki bir ekip olun, sınırlı fon genellikle veritabanlarınızın barındırılmasına yönelik kararların verilmesinde en önemli etmendir. Bu bölümde, Azure'da faturalama ve lisanslama ile ilgili temel bilgileri şu iki ilişkisel veritabanı seçeneğine istinaden öğreneceksiniz: SQL Veritabanı ve Azure VM’lerde SQL Server. Ayrıca toplam uygulama maliyetini hesaplama hakkında bilgi edineceksiniz.

#### <a name="billing-and-licensing-basics"></a>Faturalama ve lisanslama ile ilgili temel bilgiler

Şu anda **SQL veritabanı** bir hizmet olarak satılır ve bunların tümü faturalandırılır saatlik seçtiğiniz Hizmet katmanını ve performans düzeyi temelinde sabit bir oranda, kaynaklar için farklı fiyatlarla birkaç hizmet katmanları mevcuttur. SQL veritabanı örneğiyle yönetilen kendi lisansınızı getirebilirsiniz. Kendi lisansını getir seçeneği hakkında daha fazla bilgi için bkz. [Azure'da Yazılım Güvencesi ile Lisans Mobilitesi](https://azure.microsoft.com/pricing/license-mobility/). Buna ek olarak, giden Internet trafiği için normal [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) faturalandırılırsınız. Hizmet katmanları ve performans düzeyleri uygulamanızın değişken verimlilik ihtiyaçlarını karşılamak için dinamik olarak ayarlayabilirsiniz. En son bilgileri desteklenen geçerli hizmet katmanları, bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [vCore tabanlı satın alma modeli (Önizleme)](sql-database-service-tiers-vcore.md). Ayrıca oluşturabilirsiniz [esnek havuzlar](sql-database-elastic-pool.md) kaynakları maliyetlerini azaltmak ve kullanım uyum sağlamak için veritabanı örnekleri arasında paylaşmak için ani.

**SQL Database** ile, veritabanı yazılımına Microsoft tarafından otomatik olarak yapılandırma, düzeltme eki ve yükseltme uygulanır; bu da yönetim maliyetlerinizi azaltır. Ayrıca, [yerleşik yedekleme](sql-database-automated-backups.md) özellikleri, özellikle çok sayıda veritabanınız olduğunda önemli maliyet tasarrufları sağlamanıza yardımcı olur. 

**Azure VM’lerde SQL Server** ile platform tarafından sağlanan SQL Server görüntülerinin herhangi birini (lisans içerir) kullanabilir veya kendi SQL Server lisansınızı getirebilirsiniz. Desteklenen tüm SQL Server sürümleri (2008R2, 2012, 2014, 2016) (Developer, Express, Web, Standard, Enterprise) kullanılabilir. Ayrıca, görüntülerin Kendi Lisansını Getir sürümleri (KLG) mevcuttur. Azure tarafından sağlanan görüntüler kullanıldığında, işletim maliyeti, VM boyutunun yanı sıra seçtiğiniz SQL Server sürümüne bağlıdır. VM boyutu veya SQL Server sürümünden bağımsız olarak, dakika başına lisanslama ücreti SQL Server ve Windows veya Linux, VM diskleri için Azure Storage maliyetinin yanı sıra ücret ödersiniz. Dakika başına faturalandırma seçeneği, SQL Server'ı ek SQL Server lisansları satın almadan istediğiniz süreyle kullanmanıza olanak sağlar. Azure için kendi SQL Server lisansınızı getirirseniz, sunucu ve yalnızca depolama maliyetleri için sizden ücret kesilir. Kendi lisansını getir seçeneği hakkında daha fazla bilgi için bkz. [Azure'da Yazılım Güvencesi ile Lisans Mobilitesi](https://azure.microsoft.com/pricing/license-mobility/). Buna ek olarak, giden Internet trafiği için normal [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) faturalandırılırsınız.

#### <a name="calculating-the-total-application-cost"></a>Toplam uygulama maliyetini hesaplama
Bir bulut platformunu kullanmaya başladığınızda, uygulamanızı çalıştırmanın maliyeti yeni geliştirme maliyetini ve devam eden yönetim maliyetlerini ve genel bulut platformu hizmet maliyetlerini içerir.

**Azure SQL Veritabanı kullanıldığında:**

- Yüksek oranda küçültülmüş yönetim maliyetleri
- Geçirilen uygulamaları için sınırlı geliştirme maliyetleri
- SQL Database hizmet maliyetleri
- Donanım satın alma maliyetleri olmadan

**Azure VM'lerinde SQL Server kullanıldığında:**

- Daha yüksek yönetim maliyetleri
- Geçirilen uygulamalar için geliştirme maliyetleri olmadan sınırlıdır
- Sanal makine hizmet maliyetleri
- SQL Server Lisans maliyetleri
- Donanım satın alma maliyetleri olmadan

Fiyatlandırma konusunda daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [SQL Veritabanı Fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/)
* [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows) için [Virtual Machine Fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Azure Fiyatlandırma Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Yönetim
Birçok işletme için, bir bulut hizmetine geçiş yapma kararı, maliyetle ilgili olduğu kadar, yönetim karmaşıklığı yükünü gidermekle de ilgilidir. Iaas ve PaaS, Microsoft altyapının yönetir ve otomatik olarak olağanüstü durum kurtarma sağlamak üzere tüm verilerini çoğaltır, yapılandırır ve veritabanı yazılımını yükselten, yük dengelemeyi yönetir ve varsa saydam yük devretme mu bir bir veri merkezi içinde sunucu hatası. 

- İle **Azure SQL veritabanı**veritabanınızı yönetmeye devam edebilirsiniz ancak artık veritabanı altyapısı, sunucu işletim sistemini veya donanımı yönetmeniz gerekmez.  Yönetmeye devam edebileceğiniz öğelere örnek olarak veritabanları ve oturum açma bilgileri, dizin ve sorgu ayarları ile denetim ve güvenlik verilebilir. Ayrıca, yüksek kullanılabilirlik için başka bir veri merkezi yapılandırma minimal yapılandırma ve yönetim gerektirir.
- **Azure VM’lerde SQL Server** ile işletim sistemi ve SQL Server örneği yapılandırması üzerinde tam denetime sahip olursunuz. Bir VM ile, işletim sisteminin ve veritabanı yazılımının ne zaman güncelleştirileceğine/yükseltileceğine ve virüsten koruma araçları gibi herhangi bir ek yazılımın ne zaman yükleneceğine ilişkin karar size aittir. Düzeltme eki uygulama, yedekleme ve yüksek kullanılabilirliği önemli ölçüde kolaylaştırmak amacıyla bazı otomatik özellikler sağlanır. Ayrıca, VM'nin boyutunu, disk sayısını ve disklerin depolama yapılandırmalarını denetleyebilirsiniz. Azure gerektiğinde bir VM’nin boyutunu değiştirmenize izin verir. Bilgi için bkz. [Azure için Sanal Makine ve Bulut Hizmeti Boyutları](../virtual-machines/windows/sizes.md). 

### <a name="service-level-agreement-sla"></a>Hizmet Düzeyi Sözleşmesi (SLA)
Birçok BT departmanı için, bir Hizmet Düzeyi Sözleşmesi'nin çalışma süresi yükümlülüklerinin yerine getirilmesi birincil önceliğe sahiptir. Bu bölümde, her bir veritabanı barındırma seçeneği için geçerli olan SLA'yı ele alacağız.

İçin **SQL veritabanı**, Microsoft, SLA % 99,99 kullanılabilirlik sağlar. En son bilgiler için bkz. [Hizmet Düzeyi Sözleşmesi](https://azure.microsoft.com/support/legal/sla/sql-database/). 

**Azure VM'lerinde çalıştırılan SQL Server** için Microsoft, yalnızca Sanal Makine'yi kapsayan %99,95 oranında bir kullanılabilirlik SLA'sı sağlar. Bu SLA, VM'de çalıştırılan işlemleri (SQL Server gibi) kapsamaz ve bir kullanılabilirlik kümesinde en az iki VM örneğini barındırmanızı gerektirir. En son bilgiler için bkz. [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Veritabanı için yüksek kullanılabilirlik (HA) VM'ler içinde desteklenen yüksek kullanılabilirlik seçeneklerinden birini SQL Server'da aşağıdaki gibi yapılandırmanız gerekiyor [Always On kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server). Desteklenen bir yüksek kullanılabilirlik seçeneğinin kullanılması ek bir SLA sağlamaz, ancak %99,99’un üzerinde veritabanı kullanılabilirliği elde etmenize imkan tanır.

### <a name="market"></a>Azure'a taşımak için zaman
**SQL Database mantıksal sunucu, esnek havuzlar ve tek veritabanları** Geliştirici üretkenliği ve hızlı zaman pazarlama noew çözümleri için önemli olduğundan, doğru çözümdür bulut Tasarımlı uygulamalar için. Programlama DBA benzeri işlevsellik ile, altta yatan işletim sistemini ve veritabanını yönetmeye yönelik ihtiyacı azalttığından, bulut mimarları ve geliştiricileri için idealdir. 

**SQL veritabanı yönetilen örneği** Azure SQL Database, Azure içinde hızlı bir şekilde pazara uygulamaları geçirilen bir veritabanını getirmek etkinleştirme mevcut uygulamalarına geçişini büyük ölçüde basitleştirir.

**Azure Vm'lerinde çalışan SQL Server** mevcut veya yeni uygulamalarınızın büyük veritabanları gerektiren veya SQL Server veya Windows/Linux'ındaki tüm özelliklere erişmek ve süresi önlemek istiyorsanız ve yeni alınırken, harcama şirket içi donanım mükemmeldir. Ayrıca varolan geçirmek istediğiniz zaman iyi bir tercihtir şirket içi uygulamaların ve veritabanlarının oldukları gibi Azure'a olan-- yönetilen Azure SQL veritabanı örneğinde olduğu iyi durumda değil. Sunumu, uygulamayı ve veri katmanlarını değiştirmeniz gerekmediği için, var olan çözümünüzü yeniden yapılandırma konusunda zamandan ve bütçeden tasarruf sağlarsınız. Bunun yerine, tüm çözümlerinizin Azure'a geçişini sağlamaya ve Azure platformu tarafından gerekli kılınabilen bazı performans iyileştirmelerini gerçekleştirmeye odaklanabilirsiniz. Daha fazla bilgi için bkz. [Azure Virtual Machines'de SQL Server için En İyi Performans Uygulamaları](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Özet
Bu makalede, SQL Database ve Azure Virtual Machines'de (VM'ler) SQL Server işlenmiş ve kararınızı etkileyebilecek genel iş teşvikleri ele alınmıştır. Aşağıda, dikkate almanız için önerilerin bir özeti sağlanmıştır:

**Azure SQL Database**'i aşağıdaki koşullar geçerli olduğunda tercih edin:

* Bulut hizmetlerinin sağladığı maliyet tasarruflarından ve performans iyileştirmesinden yararlanmak için yeni bulut tabanlı uygulamalar oluşturuyorsunuz. Bu yaklaşım, tam olarak yönetilen bir bulut hizmetinin avantajlarını sağlar, daha kısa bir ilk pazarlama süresi elde edilmesine yardımcı olur ve uzun vadeli maliyet iyileştirmesi sağlayabilir.
* Microsoft'un veritabanlarınız üzerinde genel yönetim işlemlerini gerçekleştirmesini ve veritabanları için daha güçlü kullanılabilirlik SLA'larını gerekli kılmasını istiyorsanız.
* Var olan bir uygulama olarak geçirmek istediğiniz-Azure SQL veritabanı yönetilen örneği için ve SQL Server ve/veya Gelişmiş Güvenlik ve ağ ek eşlikli yararlanın. Yönetilen örneği, yeni ve mevcut uygulamalar için iyi bir seçimdir.

**Azure VM'lerinde SQL Server**'ı aşağıdaki koşullar geçerli olduğunda tercih edin:

* Geçiş veya buluta genişletmek istediğiniz şirket içi uygulamalara sahip veya 4 TB'den büyük kurumsal uygulamalar oluşturmak istiyorsanız. Bu yaklaşım, SQL Server sürümüne ve seçim, büyük veritabanı kapasite, SQL Server ve Windows/Linux üzerinde tam denetim kullanmanın avantajı sağlar ve şirket içi tünel güvenli. Bu yaklaşım var olan uygulamaların geliştirme ve değişiklik maliyetlerini azaltır.
* BT kaynaklarınız var ve sonuçta kendi düzeltme eki uygulama, yedekleme ve veritabanı yüksek kullanılabilirlik özelliklerinize sahip olabilirsiniz. Otomatik özelliklerin bazıları bu işlemleri önemli ölçüde basitleştirebilir. 

## <a name="next-steps"></a>Sonraki adımlar

* SQL Veritabanı’nı kullanmaya başlamak için bkz. [İlk Azure SQL Veritabanınız](sql-database-get-started-portal.md).
* Bkz. [SQL Veritabanı Fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).
* Azure VM'lerinde SQL Server'ı kullanmaya başlamak için bkz. [Azure'da SQL Server sanal makinesi sağlama](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).