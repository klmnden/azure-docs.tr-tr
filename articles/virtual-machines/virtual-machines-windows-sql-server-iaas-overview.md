<properties
    pageTitle="Azure Virtual Machines’de SQL Server Kullanmaya Başlama | Microsoft Azure"
    description="Azure Virtual Machines ile şirket için SQL Server veritabanı iş yüklerinizi Buluta taşıyın. Önceden yapılandırılmış SQL VM görüntülerini hızla kullanmaya başlayın."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="06/03/2016"
    ms.author="jroth"/>

# Azure Virtual Machines’de SQL Server Kullanmaya Başlama

Bu konuda Azure Virtual Machines ‘de SQL Server çalıştırma seçenekleriniz açıklanmakta ve kullanmaya başlamanız için kılavuz ve kaynaklar sağlanmaktadır.

Şirket için SQL Server iş yüklerinizi Buluta taşımak isteyen bir veritabanı yöneticisi olabilirsiniz. Veya Azure uygulamanız için SQL Server’ın ilişkisel veritabanı özelliklerini dikkate alan bir geliştirici olabilirsiniz. Azure Virtual Machines ‘de SQL Server iş yüklerini çalıştırmanın avantajı nedir? Aşağıdaki genel bakış videosunda avantajlar açıklanır ve teknik genel bakış sağlanır.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

## Avantajları değerlendirme

Başlamadan önce, ilk olarak Azure VM’lerde SQL Server kullanarak ne kazanacağınızı değerlendirin.

Kuruluş uygulaması gibi diğer iş yüklerini Azure’a taşıyorsanız, gelişmiş performans için bağlı SQL Server veritabanlarını da Azure’a taşımak mantıklıdır. Ancak SQL Server'ı Azure VM’lerde barındırmanın başka avantajları da vardır. Örneğin, genel varlık ve olağanüstü durum kurtarma için birden fazla veri merkezine otomatik olarak erişim sahibi olursunuz. Senaryoların ve avantajların tam listesi için bkz. [Azure VM’lerde SQL Server ürün sayfası](https://azure.microsoft.com/services/virtual-machines/sql-server/).

> [AZURE.NOTE] Azure VM’lerde SQL Server’ı değerlendirirken, [SQL Database](../sql-database/sql-database-technical-overview.md), [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md), ve [SQL Server Stretch Database](../sql     -server-stretch-database/sql-server-stretch-database-overview.md) gibi Azure’daki diğer depolama ve .SQL seçeneklerini de gözden geçirin. Ayrıntılı bir karşılaştırma için bkz. [Bir bulut SQL Server seçeneği seçin: Azure SQL (PaaS) Veritabanı ya da Azure VM’lerde SQL Server (IaaS)](../sql-database/data-management-azure-sql-database-and-sql-server-iaas.md).

Azure VM’lerde SQL Server çalıştırmaya karar verdiğinizde, ilk kararlarınızdan biri SQL Server lisans maliyetlerini içeren bir VM görüntüsü kullanıp kullanmamaktır. Diğer seçeneğiniz kendi lisansını getirdir (KLG), böylece yalnızca VM için ödeme yaparsınız. Sonraki iki bölümde bu seçenekler açıklanmaktadır.

## Seçenek 1: Bir SQL VM dağıtma (dakika başına lisans)
Aşağıdaki tabloda sanal makine galerisindeki kullanılabilir SQL Server görüntülerinin bir matrisi verilmektedir. Belirtilen sürüm, yayın ve işletim sisteminizle yeni bir SQL VM oluşturmaya başlamak için bağlantılardan birine tıklayın. Tüm görüntülere [SQL Server lisans maliyetleri](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql) dahildir.

Adım adım rehberlik [Azure Portal'da SQL Server sanal makine sağlama](virtual-machines-windows-portal-sql-server-provision.md) adlı öğreticide mevcuttur. Ayrıca,uygun makine boyutunu seçmeyi ve sağlama işlemi sırasında kullanılabilir diğer seçenekleri açıklayan [ SQL Server VM’ler için performans en iyi uygulamaları](virtual-machines-windows-sql-performance.md)’nı gözden geçirin.

|SQL Server sürümü|İşletim sistemi|SQL Server yayını|
|---|---|---|
|**SQL Server 2016**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL Server 2014**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL Server 2012 SP3**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL Server 2008 R2 SP3**|Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL Server 2008 R2 SP3**|Windows Server 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## Seçenek 2: SQL VM dağıtma (KLG)
Diğer seçenek kendi lisansını getirdir (KLG). Bu senaryoda, SQL Server Lisans için hiçbir ek bir ücret olmadan yalnızca VM için ödeme yaparsınız. Kendi lisansınızı kullanmak için, aşağıdaki SQL Server sürümleri, yayınları ve işletim sistemleri matrisini kullanın. Portalda, görüntü adlarına Portal’da **{KLG}** ön eki eklenir.

> [AZURE.IMPORTANT] KLG VM görüntüleri kullanmak için, olması gerekir ve Kuruluş Sözleşmesi ile [Azure’da Yazılım Güvencesi ile Lisans Taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility/) içeren bir Kuruluş Sözleşmeniz olmalıdır. Ayrıca, kullanmak istediğiniz SQL Server sürümü/yayını için geçerli bir lisans da gerekir. VM’nizi sağladıktan sonra **10** gün içinde [gerekli KLG bilgilerini Microsoft’a vermelisiniz.](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf)

[Sağlama işlemi öğretici](virtual-machines-windows-portal-sql-server-provision.md)sindeki rehberlik geçerlidir, ancak aşağıdaki **KLG** görüntü seçeneklerinden birini kullanmalısınız. Ayrıca,uygun makine boyutunu seçmeyi ve sağlama işlemi sırasında kullanılabilir diğer seçenekleri açıklayan [ SQL Server VM’ler için performans en iyi uygulamaları](virtual-machines-windows-sql-performance.md)’nı gözden geçirin.

|SQL Server sürümü|İşletim sistemi|SQL Server yayını|
|---|---|---|
|**SQL Server 2016**|Windows Server 2012 R2|[Enterprise KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Standard KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Enterprise KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Enterprise KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

## SQL VM’nizi yönetme
SQL Server sanal makinenizi sağladıktan sonra isteğe bağlı birkaç yönetim görevi vardır. Bazı durumlarda, SQL Server’ı tam olarak şirket içindeki şekilde yapılandırır ve yönetirsiniz. Ancak bazı görevler Azure’a özgüdür. Aşağıdaki bölümlerde daha fazla bilgi için bağlantılar ile birlikte bu alanlardan bazıları vurgulanmaktır.

### Verilerinizi geçirme

Varolan bir veritabanınız varsa, bunu yeni sağlanan SQL VM'ye taşımak istersiniz. Geçiş seçenekleri ve kılavuzların listesi için bkz. [Azure VM’de bir Veritabanını SQL Server’a Geçirme](virtual-machines-windows-migrate-sql.md).

### Yüksek kullanılabilirliği yapılandırma

Size yüksek kullanılabilirlik gerekiyorsa, SQL Server Kullanılabilirlik gruplarını yapılandırmayı dikkate alın. Bu, bir sanal ağda birden fazla Azure VM’yi içerir. Azure portal bu yapılandırmayı sizin için ayarlayan bir şablona sahiptir. Daha fazla bilgi için bkz. [Azure Resource Manager sanal makinelerde AlwaysOn Kullanılabilirlik grubu yapılandırma](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). Kullanılabilirlik Grubunuzu ve ilgili dinleyiciyi el ile yapılandırmak istiyorsanız, bkz. [Azure VM’de AlwaysOn Kullanılabilirlik Grupları yapılandırma](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Diğer yüksek kullanılabilirlik dikkate alınacak noktaları için bkz. [Azure Virtual Machines’de SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma](virtual-machines-windows-sql-high-availability-dr.md).

### Verilerinizi yedekleme
Blob Storage kullanarak veritabanı yedeklerinizi doğrudan Azure’de depolayın. Daha fazla bilgi için. bkz. [SQL Server Yedekleme ve Geri Yükleme için Azure Storage’ı Kullanma](../sql-database/storage-use-storage-sql-server-backup-restore.md). Bu SQL VM’ler için başarılı olsa da, şirket içi SQL Server veritabanlarında da çalışmaktadır. Yedekleme ve geri yükleme seçeneklerine genel bakış için bkz. [Azure Virtual Machines’de SQL Server için Yedekleme ve geri Yükleme](virtual-machines-windows-sql-backup-recovery.md).

Azure VM’ler, SQL Server için [Otomatik Yedekleme](virtual-machines-windows-sql-automated-backup.md) ve [Otomatik Düzeltme Eki Uygulama](virtual-machines-windows-sql-automated-patching.md)’dan da faydalanabilir.

### Müşteri deneyimini geliştirme programı (CEIP)
Müşteri Deneyimini Geliştirme Programı (CEIP) varsayılan olarak etkindir. Sağlama işlemi sonrası devre dışı bırakmak istemediğiniz sürece, bu bir yönetim görevi değildir. VM’ye uzak masaüstüyle bağlanarak CEIP özelleştirebilir ya da devre dışı bırakabilirsiniz. Ardından **SQL Server Hata ve Kullanım Raporlama** yardımcı programını çalıştırın. Raporlamayı devre dışı bırakmak için yönergeleri izleyin.

## Sonraki adımlar
Azure Virtual Machines’de SQL Server için.[Öğrenme Yolunu keşfedin](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/).

Başka sorunuz mu var? Önce, bkz. [Azure Virtual Machines’de SQL Server Kullanmaya Başlama SSS](virtual-machines-windows-sql-server-iaas-faq.md). Ayrıca sorularınızı ve yorumlarınızı, Microsoft ve toplulukla etkileşim kurmak amacıyla bir SQL VM konusunun alt kısmına da ekleyebilirsiniz.



<!--HONumber=Jun16_HO2-->


