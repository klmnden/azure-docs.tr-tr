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
ms.date: 09/14/2018
ms.author: carlrab
ms.openlocfilehash: 57e83376747b9a3e2d30dec37d4a378a167580e5
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733119"
---
# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Bir bulut SQL Server seçeneği seçin: Azure SQL (PaaS) Veritabanı ya da Azure VM'lerde SQL Server (IaaS)

Azure'da barındırılan bir altyapı (Iaas) içinde çalışan veya barındırılan bir hizmet olarak çalışan SQL Server iş yüklerinizi olabilir ([PaaS](https://azure.microsoft.com/overview/what-is-paas/)):

- [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/): Enterprise Edition'ın SQL Server'da modern uygulama geliştirme için en iyi duruma getirilmiş, temel bir SQL veritabanı altyapısı. Azure SQL veritabanı dağıtım için çeşitli seçenekler sunar:
  - Tek bir veritabanı için dağıtabileceğiniz bir [mantıksal sunucu](sql-database-logical-servers.md).
  - İçine dağıtabileceğiniz bir [elastik havuz](sql-database-elastic-pool.md) üzerinde bir [mantıksal sunucu](sql-database-logical-servers.md) kaynakları paylaşır ve maliyetleri azaltmak için. 

      > [!NOTE]
      > Tek ve havuza alınmış veritabanlarını içeren bir Azure SQL veritabanı, SQL Server veritabanı kapsamlı özelliklerinin çoğunu sağlar.

      Aşağıdaki şekilde bu dağıtım seçenekleri gösterilmektedir:

      ![dağıtım seçenekleri](./media/sql-database-technical-overview/deployment-options.png) 
  - Dağıtabileceğiniz bir [Azure SQL veritabanı yönetilen örneği (Önizleme)](sql-database-managed-instance.md). 

      > [!NOTE]
      > İki sürümü ile Azure SQL veritabanı, SQL Server'da yerleşik zeka ve yönetimi gibi mevcut olmayan ek özellikler ekler. İlk sürüm ile birlikte Azure SQL veritabanı yönetilen örneği, Azure SQL veritabanı veritabanları ve ek örnek kapsamlı özellikler için paylaşılan kaynaklar sunar. Azure SQL veritabanı yönetilen örneği ile veritabanı geçişi destekleyen herhangi bir veritabanı değişiklik için en az.
- [Azure Virtual Machines'de SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server'ın yüklü ve olarak da bilinen bir altyapı (ıaas) olarak Azure üzerinde çalışan Windows Server veya Linux sanal makineleri (VM'ler) bulutta barındırılan. Azure sanal makinelerinde SQL Server'a geçirmek için iyi bir seçenek SQL Server veritabanları ve uygulamalar herhangi bir veritabanı değişiklik olmadan şirket ' dir. Tüm yeni sürümleri ve SQL Server sürümleri bir Iaas sanal makinesine yüklenmesi için kullanılabilir. SQL veritabanı'ndan en önemli fark, SQL Server Vm'leri veritabanı altyapısı üzerinde tam denetime izin ver ' dir. Bakım ve düzeltme, basit kurtarma modelini veya toplu günlük, duraklatmak veya gerektiğinde, engine başlatmak için daha az daha hızlı yük etkinleştirmek için oturum açmış değiştirme başlar ve SQL Server veritabanı altyapısı tam olarak özelleştirebilirsiniz seçebilirsiniz. Bu ek denetim ile sanal makineleri yönetmek için eklenen sorumluluğu ile birlikte gelir.

Her dağıtım seçeneğinin Microsoft Veri platformuna nasıl uyduğunu öğrenin ve iş gereksinimleriniz için doğru seçeneği bulma konusunda yardım alın. Sizin için maliyet tasarrufu ve minimum yönetim tüm diğer unsurlardan önce geliyorsa bu makale, hangi yaklaşımın en fazla önem verdiğiniz iş gereksinimleri açısından en iyi sonucu verdiği konusunda karar vermenize yardımcı olabilir.

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

**Azure SQL veritabanı** bir ilişkisel olarak-a-endüstri kategorisine girer Azure bulutunda barındırılan hizmet veritabanıdır (DBaaS) *olarak bir-hizmet Platform (PaaS)*. [SQL veritabanı](sql-database-technical-overview.md), Microsoft'un sahibi olduğu, barındırdığı ve bakımını yaptığı standartlaştırılmış donanım ve yazılım üzerinde geliştirilmiştir. SQL veritabanı ile yerleşik özellikleri ve geniş kapsamlı SQL Server yapılandırmasında gerekli işlevleri kullanabilirsiniz. SQL Database'i kullandığınızda, kesinti olmadan daha fazla güç için ölçeğinizi artırmaya veya genişletmeye yönelik seçeneklerle kullandıkça ödersiniz. Azure SQL veritabanı, bulutta yeni uygulamalar geliştirmek için ideal bir ortam olan. Ve ile [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md), kendi lisansınızı getirebilirsiniz. Ayrıca, bu seçenek, tüm Azure SQL veritabanı'nın PaaS avantajları sağlar ancak daha önce SQL VM'ler yalnızca kullanılabilir özellikleri ekler. Bu, yerel sanal ağ (VNet) içerir ve şirket içi SQL Server ile % 100 uyumluluk yakın. [Yönetilen örnek](sql-database-managed-instance.md) şirket içi azure'a geçişini gerekli minimum değişikliklerle veritabanı için idealdir. 

**Azure Virtual Machines'de (VM'ler) SQL Server**, *Hizmet olarak Altyapı (IaaS)* endüstri kategorisine girer ve buluttaki bir sanal makine içinde SQL Server'ı çalıştırmanıza olanak sağlar. [SQL Server sanal makineleri](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) de, sahip olduğu, barındırdığı ve Microsoft tarafından yönetilen standartlaştırılmış donanım üzerinde çalıştırın. Bir VM'de SQL Server kullanırken ödeyebilir miyim-gibi bir SQL Server görüntüsüne zaten dahil bir SQL Server lisansı için Git ya da mevcut bir lisansı kolayca kullanabilirsiniz. Ayrıca, durdurabilir veya VM gerektiği gibi devam.

Genellikle, bu iki SQL seçeneği farklı amaçlar için en iyi hale getirilmiştir:

* **Azure SQL veritabanı** yönetim maliyetleri, çok sayıda veritabanının hazırlanması ve yönetilmesi için en düşük düzeye indirmek için optimize edilmiştir. Herhangi bir sanal makineyi, işletim sistemini veya veritabanı yazılımını yönetmeniz gerekmediğinden, bu, devam eden yönetim maliyetlerini azaltır. Yükseltme, yüksek kullanılabilirlik veya [yedeklemeleri](sql-database-automated-backups.md) yönetmeniz gerekli değildir. Genellikle, Azure SQL Database tek bir BT veya geliştirme kaynağı tarafından yönetilen veritabanlarının sayısını önemli ölçüde artırabilir. [Elastik havuzlar](sql-database-elastic-pool.md) de Kiracı yalıtımı ve veritabanları arasında kaynakların paylaşılması, maliyetleri azaltmak için ölçeği olanağı gibi özellikler ile SaaS çok kiracılı uygulama mimarileri destekler. [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md) veritabanları arasında kaynakların paylaşılması yanı sıra mevcut uygulamaların kolayca taşınmasına etkinleştirme örneği kapsamlı özellikler için destek sağlar.
* **Azure VM’ler üzerinde çalışan SQL Server**, var olan uygulamaları Azure’a geçirmek veya var olan şirket içi uygulamaları karma dağıtımlarda buluta genişletmek için en iyi duruma getirilmiştir. Ayrıca, SQL Server’ı sanal bir makinede kullanarak geleneksel SQL Server uygulamaları geliştirip test edebilirsiniz. Azure VM'lerinde SQL Server ile, adanmış bir SQL Server örneği ve bulut tabanlı bir VM üzerinde tam yönetici haklarına sahip olursunuz. Bu, bir kuruluşun halihazırda sanal makinelerin bakımını yapmak için kullanılabilir BT kaynaklarına sahip olması durumunda ideal bir seçenektir. Bu özellikler uygulamanızın belirli performans ve kullanılabilirlik gereksinimlerini karşılamak üzere yüksek düzeyde özelleştirilmiş bir sistem oluşturmanıza olanak sağlar.

Aşağıdaki tabloda, SQL Database ve Azure VM'lerinde SQL Server'ın temel özellikleri özetlenmektedir:

| | Azure SQL Database<br>Mantıksal sunucuları, elastik havuzlar ve tek veritabanları | Azure SQL Database<br>Yönetilen Örnek |Azure Sanal Makinesi<br>SQL Server |
| --- | --- | --- |---|
| **En iyi kullanım alanı:** |İstediğiniz yeni bulut Tasarımlı uygulamalar en son kararlı SQL Server özellikleri andhave alanında zaman kısıtlamaları geliştirme ve pazarlama kullanın. | Yeni uygulamalar veya en son kararlı SQL Server özelliklerini ve, kullanmak istediğiniz var olan şirket içi uygulamalar, minimum değişiklikle buluta geçirilir.  | Küçük değişiklikler veya hiç değişiklik ile buluta hızlı geçiş gerektiren var olan uygulamalar. Şirket içi üretim dışı SQL Server donanımı satın almak istemediğinizde hızlı geliştirme ve test senaryoları. |
|  | Veritabanı için yerleşik yüksek kullanılabilirlik, olağanüstü durum kurtarma ve yükseltme mekanizmalarına gereksinim duyan ekipler. | SQL veritabanı ile aynıdır. | Yapılandırabilirsiniz, takımlar düzgün ayarlamak, özelleştirme ve yüksek kullanılabilirlik, olağanüstü durum kurtarma ve SQL Server için düzeltme eki uygulama yönetin. Sağlanan bazı otomatik özellikler bunu önemli ölçüde basitleştirir. | |
|  | Altta yatan işletim sistemi ve yapılandırma ayarlarını yönetmek istemeyen ekipler. | SQL veritabanı ile aynıdır. | Tam yönetici haklarına sahip özelleştirilmiş bir ortama ihtiyacınız vardır. | |
|  | 4 TB'a kadar olan veritabanları veya olabilir, daha büyük veritabanları [yatay veya dikey olarak bölümlenmiş](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) bir ölçek genişletme düzeni kullanılarak. | SQL veritabanı ile aynıdır. | 64 TB’ye varan depolama alanına sahip SQL Server örnekleri. Örnek gereken sayıda veritabanını destekleyebilir. |
| **Uyumluluk** | Çoğu şirket içi veritabanı düzeyinde özelliklerini destekler. | Neredeyse tüm destekleyen şirket içi örnek düzeyinde ve veritabanı düzeyinde özellikleri. | Tüm şirket içi özelliklerini destekler. |
| **Kaynaklar:** | Temel alınan altyapının yapılandırma ve yönetimi için BT kaynakları kullanmak istemiyorsunuz, ancak uygulama katmanına odaklanmak istiyorsunuz. | SQL veritabanı ile aynıdır. | Yapılandırma ve yönetim için bazı BT kaynaklarına sahipsiniz. Sağlanan bazı otomatik özellikler bunu önemli ölçüde basitleştirir. |
| **Toplam sahip olma maliyeti:** | Donanım maliyetlerini ortadan kaldırır ve yönetim maliyetlerini azaltır. | SQL veritabanı ile aynıdır. | Donanım maliyetlerini ortadan kaldırır. |
| **İş sürekliliği:** |Ek olarak [yerleşik hata toleransı altyapı özelliklerine](sql-database-high-availability.md), Azure SQL veritabanı özellikleri gibi sağlar [otomatik yedeklemeler](sql-database-automated-backups.md), [-belirli bir noktaya geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore), [coğrafi geri yükleme](sql-database-recovery-using-backups.md#geo-restore), ve [yük devretme grupları ve etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md) iş sürekliliğini artırmak üzere. Daha fazla bilgi için bkz. [SQL Database iş sürekliliğine genel bakış](sql-database-business-continuity.md). | Aynı kullanıcı tarafından başlatılan, yalnızca kopya yedekleri yanı sıra SQL veritabanı olarak kullanılabilir. | Azure VM’lerde SQL Server, veritabanınızın belirli gereksinimleri için bir yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü ayarlamanıza olanak sağlar. Böylece, uygulamanız için en iyi hale getirilmiş bir sisteme sahip olabilirsiniz. Yük devretme işlemlerini ihtiyaç duyulduğunda kendi kendinize test edebilir ve çalıştırabilirsiniz. Daha fazla bilgi için bkz. [Azure Virtual Machines'de SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md). |
| **Karma bulut:** |Şirket içi uygulamanız, Azure SQL Database'deki verilere erişebilir. | [Yerel sanal ağ uygulaması](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration) ve Azure Express Route veya VPN ağ geçidi kullanarak şirket içi ortamınıza bir bağlantı. | Azure VM'lerinde SQL Server ile kısmen bulutta ve kısmen şirket içinde çalıştırılan uygulamalara sahip olabilirsiniz. Örneğin, şirket içi ağınızı ve Active Directory Etki Alanı'nı [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) üzerinden buluta genişletebilirsiniz. Ek olarak, [Azure'da SQL Server Veri Dosyaları](http://msdn.microsoft.com/library/dn385720.aspx)'nı kullanarak şirket içi veri dosyalarını Azure Storage'da depolayabilirsiniz. Daha fazla bilgi için bkz. [SQL Server 2014 Karma Bulutu'na giriş](http://msdn.microsoft.com/library/dn606154.aspx). |
|  | Verileri çoğaltmak için abone olarak [SQL Server işlem çoğaltmayı](https://msdn.microsoft.com/library/mt589530.aspx) destekler. | Azure SQL veritabanı yönetilen örneği için çoğaltma desteklenmiyor. | Tam olarak destekler [SQL Server işlem çoğaltmayı](https://msdn.microsoft.com/library/mt589530.aspx), [Always On kullanılabilirlik grupları](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), Integration Services ve günlük aktarma veri çoğaltmak için. Ayrıca, geleneksel SQL Server yedeklemeleri tam olarak desteklenir | |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Azure VM'lerinde Azure SQL Database'in veya SQL Server'ın seçilmesine yönelik iş faydaları
### <a name="cost"></a>Maliyet
İster nakit ihtiyacında olan yeni bir girişim ister sıkı bütçe kısıtlamaları altında işletilen yerleşik bir şirket bünyesindeki bir ekip olun, sınırlı fon genellikle veritabanlarınızın barındırılmasına yönelik kararların verilmesinde en önemli etmendir. Bu bölümde, Azure'da faturalama ve lisanslama ile ilgili temel bilgileri şu iki ilişkisel veritabanı seçeneğine istinaden öğreneceksiniz: SQL Veritabanı ve Azure VM’lerde SQL Server. Ayrıca toplam uygulama maliyetini hesaplama hakkında bilgi edineceksiniz.

#### <a name="billing-and-licensing-basics"></a>Faturalama ve lisanslama ile ilgili temel bilgiler

Şu anda **SQL veritabanı** hizmet olarak satılır ve kaynakların her biri faturalandırılır saatlik hizmet katmanı ve seçtiğiniz işlem boyutuna göre sabit bir ücrete, farklı fiyatlarla birkaç hizmet katmanlarında kullanılabilir. SQL veritabanı yönetilen örnek sayesinde, ayrıca kendi lisansınızı getirebilirsiniz. Kendi lisansını getir seçeneği hakkında daha fazla bilgi için bkz. [Azure'da Yazılım Güvencesi ile Lisans Mobilitesi](https://azure.microsoft.com/pricing/license-mobility/). Buna ek olarak, giden Internet trafiği için normal [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) faturalandırılırsınız. Dinamik olarak ayarlamak hizmet katmanları ve boyutları, uygulamanızın değişken verimlilik ihtiyaçlarını karşılamak için işlem kullanabilirsiniz. En yeni bilgileri desteklenen geçerli hizmet katmanları, bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md). Ayrıca oluşturabilirsiniz [elastik havuzlar](sql-database-elastic-pool.md) kaynakları maliyetleri azaltmak ve kullanım uyum sağlamak için veritabanı örnekleri arasında paylaşmak için ani.

**SQL Database** ile, veritabanı yazılımına Microsoft tarafından otomatik olarak yapılandırma, düzeltme eki ve yükseltme uygulanır; bu da yönetim maliyetlerinizi azaltır. Ayrıca, [yerleşik yedekleme](sql-database-automated-backups.md) özellikleri, özellikle çok sayıda veritabanınız olduğunda önemli maliyet tasarrufları sağlamanıza yardımcı olur. 

**Azure VM’lerde SQL Server** ile platform tarafından sağlanan SQL Server görüntülerinin herhangi birini (lisans içerir) kullanabilir veya kendi SQL Server lisansınızı getirebilirsiniz. Desteklenen tüm SQL Server sürümleri (2008R2, 2012, 2014, 2016) (Developer, Express, Web, Standard, Enterprise) kullanılabilir. Ayrıca, görüntülerin Kendi Lisansını Getir sürümleri (KLG) mevcuttur. Azure tarafından sağlanan görüntüler kullanıldığında, işletim maliyeti, VM boyutunun yanı sıra seçtiğiniz SQL Server sürümüne bağlıdır. VM boyutundan veya SQL Server sürümünden bağımsız olarak, dakika başına VM diskleri için Azure Storage maliyetinin yanı sıra, SQL Server ve Windows veya Linux Sunucu Lisanslama ücreti ödersiniz. Dakika başına faturalandırma seçeneği, SQL Server'ı ek SQL Server lisansları satın almadan istediğiniz süreyle kullanmanıza olanak sağlar. Azure'a kendi SQL Server lisansınızı getirirseniz, sunucu ve depolama maliyetleri için ücretlendirilir. Kendi lisansını getir seçeneği hakkında daha fazla bilgi için bkz. [Azure'da Yazılım Güvencesi ile Lisans Mobilitesi](https://azure.microsoft.com/pricing/license-mobility/). Buna ek olarak, giden Internet trafiği için normal [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) faturalandırılırsınız.

#### <a name="calculating-the-total-application-cost"></a>Toplam uygulama maliyetini hesaplama
Bir bulut platformunu kullanmaya başladığınızda, uygulamanızı çalıştırmanın maliyeti yeni yazılım geliştirme maliyetini ve devam eden yönetim maliyetlerini yanı sıra genel bulut platformu hizmet maliyetlerini içerir.

**Azure SQL Veritabanı kullanıldığında:**

- Yüksek oranda azaltılmış yönetim maliyetleri
- Geçirilen uygulamalar için sınırlı geliştirme maliyetleri
- SQL veritabanı hizmet maliyetleri
- Donanım satın alma maliyeti olmadan

**Azure VM'lerinde SQL Server kullanıldığında:**

- Daha yüksek yönetim maliyetlerine
- Geçirilen uygulamaları için geliştirme ücreti alınmaz sınırlıdır
- Sanal makine hizmet maliyetleri
- SQL Server Lisans maliyetleri
- Donanım satın alma maliyeti olmadan

Fiyatlandırma konusunda daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [SQL Veritabanı Fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/)
* [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows) için [Virtual Machine Fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Azure Fiyatlandırma Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Yönetim
Birçok işletme için, bir bulut hizmetine geçiş yapma kararı, maliyetle ilgili olduğu kadar, yönetim karmaşıklığı yükünü gidermekle de ilgilidir. Iaas ve PaaS sayesinde Microsoft temel altyapıyı yönetir ve otomatik olarak olağanüstü durum kurtarma sağlamak için tüm verileri çoğaltan, yapılandırır ve veritabanı yazılımına yükseltir, yük dengelemeyi yönetir ve olması durumunda saydam yük devretme gerçekleştirir bir bir veri merkezi içinde sunucu hatası. 

- İle **Azure SQL veritabanı**veritabanınızı yönetmeye devam edebilirsiniz ancak artık veritabanı altyapısını, sunucu işletim sistemini veya donanımı yönetmeniz gerekmez.  Yönetmeye devam edebileceğiniz öğelere örnek olarak veritabanları ve oturum açma bilgileri, dizin ve sorgu ayarları ile denetim ve güvenlik verilebilir. Ayrıca, başka bir veri merkezine yüksek kullanılabilirliği yapılandırma minimal yapılandırma ve yönetim gerektirir.
- **Azure VM’lerde SQL Server** ile işletim sistemi ve SQL Server örneği yapılandırması üzerinde tam denetime sahip olursunuz. Bir VM ile, işletim sisteminin ve veritabanı yazılımının ne zaman güncelleştirileceğine/yükseltileceğine ve virüsten koruma araçları gibi herhangi bir ek yazılımın ne zaman yükleneceğine ilişkin karar size aittir. Düzeltme eki uygulama, yedekleme ve yüksek kullanılabilirliği önemli ölçüde kolaylaştırmak amacıyla bazı otomatik özellikler sağlanır. Ayrıca, VM'nin boyutunu, disk sayısını ve disklerin depolama yapılandırmalarını denetleyebilirsiniz. Azure gerektiğinde bir VM’nin boyutunu değiştirmenize izin verir. Bilgi için bkz. [Azure için Sanal Makine ve Bulut Hizmeti Boyutları](../virtual-machines/windows/sizes.md). 

### <a name="service-level-agreement-sla"></a>Hizmet Düzeyi Sözleşmesi (SLA)
Birçok BT departmanı için, bir Hizmet Düzeyi Sözleşmesi'nin çalışma süresi yükümlülüklerinin yerine getirilmesi birincil önceliğe sahiptir. Bu bölümde, her bir veritabanı barındırma seçeneği için geçerli olan SLA'yı ele alacağız.

İçin **SQL veritabanı**, Microsoft SLA'sı % 99,99 kullanılabilirlik sağlar. En son bilgiler için bkz. [Hizmet Düzeyi Sözleşmesi](https://azure.microsoft.com/support/legal/sla/sql-database/). 

**Azure VM'lerinde çalıştırılan SQL Server** için Microsoft, yalnızca Sanal Makine'yi kapsayan %99,95 oranında bir kullanılabilirlik SLA'sı sağlar. Bu SLA, VM'de çalıştırılan işlemleri (SQL Server gibi) kapsamaz ve bir kullanılabilirlik kümesinde en az iki VM örneğini barındırmanızı gerektirir. En son bilgiler için bkz. [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Veritabanı yüksek kullanılabilirlik (HA) vm'lerde, desteklenen yüksek kullanılabilirlik seçeneklerinden birini SQL Server'da aşağıdaki gibi yapılandırmanız gerekiyor [Always On kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server). Desteklenen bir yüksek kullanılabilirlik seçeneğinin kullanılması ek bir SLA sağlamaz, ancak %99,99’un üzerinde veritabanı kullanılabilirliği elde etmenize imkan tanır.

### <a name="market"></a>Azure'a taşıma zamanı
**SQL veritabanı mantıksal sunucuları, elastik havuzlar ve tek veritabanları** Geliştirici üretkenliği ve hızlı zaman pazara açılma yeni çözümleri için kritik önem taşıdığında bulut Tasarımlı uygulamalarınız için doğru çözüm olduğunu. Programlama DBA benzeri işlevsellik ile, altta yatan işletim sistemini ve veritabanını yönetmeye yönelik ihtiyacı azalttığından, bulut mimarları ve geliştiricileri için idealdir. 

**SQL veritabanı yönetilen örneği** geçirilen veritabanı Azure'da hızla pazara uygulamalarınızı getirin olanak sağlayan, Azure SQL veritabanı mevcut uygulamaların geçişi büyük ölçüde basitleştirir.

**Azure Vm'lerinde çalışan SQL Server** mevcut veya yeni uygulamalarınızın büyük veritabanları gerektirir veya SQL Server veya Windows/Linux, tüm özelliklerine erişmek ve süresini önlemek istiyorsanız ve harcama yeni alınırken, şirket içi donanım mükemmel bir seçimdir. Aynı zamanda uygun olduğunda var olan geçirmek istediğiniz şirket içi uygulamaları ve veritabanları olarak azure'a olan-- Azure SQL veritabanı yönetilen örneği olmadığı yerde uygun durumlarda değildir. Sunumu, uygulamayı ve veri katmanlarını değiştirmeniz gerekmediği için, var olan çözümünüzü yeniden yapılandırma konusunda zamandan ve bütçeden tasarruf sağlarsınız. Bunun yerine, tüm çözümlerinizin Azure'a geçişini sağlamaya ve Azure platformu tarafından gerekli kılınabilen bazı performans iyileştirmelerini gerçekleştirmeye odaklanabilirsiniz. Daha fazla bilgi için bkz. [Azure Virtual Machines'de SQL Server için En İyi Performans Uygulamaları](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Özet
Bu makalede, SQL Database ve Azure Virtual Machines'de (VM'ler) SQL Server işlenmiş ve kararınızı etkileyebilecek genel iş teşvikleri ele alınmıştır. Aşağıda, dikkate almanız için önerilerin bir özeti sağlanmıştır:

**Azure SQL Database**'i aşağıdaki koşullar geçerli olduğunda tercih edin:

* Bulut hizmetlerinin sağladığı maliyet tasarruflarından ve performans iyileştirmesinden yararlanmak için yeni bulut tabanlı uygulamalar oluşturuyorsunuz. Bu yaklaşım, tam olarak yönetilen bir bulut hizmetinin avantajlarını sağlar, daha kısa bir ilk pazarlama süresi elde edilmesine yardımcı olur ve uzun vadeli maliyet iyileştirmesi sağlayabilir.
* Microsoft'un veritabanlarınız üzerinde genel yönetim işlemlerini gerçekleştirmesini ve veritabanları için daha güçlü kullanılabilirlik SLA'larını gerekli kılmasını istiyorsanız.
* Var olan bir uygulama olarak geçirmek istediğiniz-Azure SQL veritabanı yönetilen örneği için ve SQL Server ve/veya Gelişmiş Güvenlik ve ağ hizmeti ile ek eşlik yararlanın. Yönetilen örnek, yeni ve mevcut uygulamalar için iyi bir seçimdir.

**Azure VM'lerinde SQL Server**'ı aşağıdaki koşullar geçerli olduğunda tercih edin:

* Geçirme veya buluta genişletmek istiyorsanız, mevcut şirket içi uygulamalara sahip olduğunuz veya 4 TB'den büyük kuruluş uygulamaları oluşturmak istiyorsanız. Bu yaklaşım, SQL Server sürümüne ve seçim, büyük veritabanı kapasitesi, SQL Server ve Windows/Linux üzerinde tam denetim kullanmanın avantajı sağlar ve güvenli şirket içi tünel. Bu yaklaşım var olan uygulamaların geliştirme ve değişiklik maliyetlerini azaltır.
* BT kaynaklarınız var ve sonuçta kendi düzeltme eki uygulama, yedekleme ve veritabanı yüksek kullanılabilirlik özelliklerinize sahip olabilirsiniz. Otomatik özelliklerin bazıları bu işlemleri önemli ölçüde basitleştirebilir. 

## <a name="next-steps"></a>Sonraki adımlar

* SQL Veritabanı’nı kullanmaya başlamak için bkz. [İlk Azure SQL Veritabanınız](sql-database-get-started-portal.md).
* Bkz. [SQL Veritabanı Fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).
* Azure VM'lerinde SQL Server'ı kullanmaya başlamak için bkz. [Azure'da SQL Server sanal makinesi sağlama](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).
