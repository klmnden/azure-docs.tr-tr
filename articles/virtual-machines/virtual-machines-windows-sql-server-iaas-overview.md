<properties
    pageTitle="Azure Sanal Makineler’de SQL Server’a Genel Bakış | Microsoft Azure"
    description="Azure Virtual Machines hizmetinde tam SQL Server sürümlerini çalıştırma hakkında bilgi edinin. Tüm SQL Server VM görüntülerinin ve ilgili içeriklerin doğrudan bağlantılarını alın."
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
    ms.date="10/11/2016"
    ms.author="jroth"/>


# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Azure Virtual Machines’de SQL Server’a Genel Bakış

Bu konu başlığı, Azure sanal makinelerinde (VM’ler) SQL Server çalıştırmaya yönelik seçeneklerle birlikte [portal görüntülerinin bağlantılarını](#option-1-create-a-sql-vm-with-per-minute-licensing) ve [sık gerçekleştirilen görevlerin](#manage-your-sql-vm) genel açıklamasını içermektedir.

>[AZURE.NOTE] SQL Server’ı zaten biliyor ve yalnızca bir SQL Server sanal makinesinin nasıl dağıtılacağını görmek istiyorsanız bkz. [Azure portal’da bir SQL Server VM’si sağlama](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="overview"></a>Genel Bakış
Veritabanı yöneticisi veya geliştiriciyseniz Azure VM’leri, şirket içi SQL Server iş yüklerinizi ve uygulamalarınızı Buluta taşımanız için bir yöntem sağlar. Aşağıdaki videoda, SQL Server Azure VM’leri için teknik bir genel bakış sağlanmıştır.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

Video, aşağıdaki alanları kapsar:

|Zaman|Alan|
|---|---|
| 00:21 | Azure VM’leri nedir? |
| 01:45 | Güvenlik |
| 02:50 | Bağlantı |
| 03:30 | Depolama güvenilirliği ve performansı |
| 05:20 | VM boyutları |
| 05:54 | Yüksek kullanılabilirlik ve SLA |
| 07:30 | Yapılandırma desteği |
| 08:00 | İzleme |
| 08:32 | Demo: SQL Server 2016 VM’si oluşturma |

>[AZURE.NOTE] Bu video, SQL Server 2016’ya odaklanmaktadır; ancak Azure, SQL Server’ın 2008, 2012, 2014 ve 2016 dahil birçok sürümü için VM görüntüleri sağlar. 

## <a name="understand-your-options"></a>Seçeneklerinizi anlayın
Verilerinizi Azure’da barındırmayı seçmeniz için birçok neden vardır. Uygulamanız Azure’a taşınıyorsa verilerin taşınma performansı da artırır. Bununla birlikte, başka avantajları da vardır. Küresel bir erişim ağı ve olağanüstü durum kurtarma için birden fazla veri merkezine otomatik olarak erişebilirsiniz. Ayrıca veriler son derece güvenilir ve dayanıklı olur.

Azure VM’lerinde çalışan SQL Server, ilişkisel verilerinizi Azure’da depolamak için yararlanabileceğiniz bir seçenektir. Aşağıdaki tabloda, Azure’daki SQL teklifleri hakkında kısa bir özet sunulmuştur.

|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| SQL teklifi | Açıklama |
|---:|---|---|
|![Azure Sanal Makinelerde SQL Server](./media/virtual-machines-windows-sql-server-iaas-overview/sql-server-virtual-machine.png)|[Azure Sanal Makinelerde SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)|SQL Server’ı Azure Sanal Makinelerinde (bu konu başlığının odak noktası) çalıştırın. SQL Server’ın perakende sürümlerinde doğrudan sanal makineyi yönetin ve veritabanınızı çalıştırın. |
|![SQL Veritabanı](./media/virtual-machines-windows-sql-server-iaas-overview/azure-sql-database.png)|[SQL Veritabanı](https://azure.microsoft.com/services/sql-database/)|Temel altyapıyı yönetmek zorunda kalmadan veritabanınıza erişmek ve veritabanınızı ölçeklendirmek için SQL Veritabanı hizmetini kullanın.|
|![SQL Veri Ambarı](./media/virtual-machines-windows-sql-server-iaas-overview/azure-sql-data-warehouse.png)|[SQL Veri Ambarı](https://azure.microsoft.com/en-us/services/sql-data-warehouse/)|Büyük miktarda ilişkisel ve ilişkisel olmayan veriyi işlemek için Azure SQL Veri Ambarı’nı kullanın. Bir hizmet olarak ölçeklenebilir veri ambarı özellikleri sağlar.|
|![SQL Server Esnetme Veritabanı](./media/virtual-machines-windows-sql-server-iaas-overview/sql-server-stretch-database.png)|[SQL Server Esnetme Veritabanı](https://azure.microsoft.com/en-us/services/sql-server-stretch-database/)|Şirket içi ilişkisel verileri Microsoft SQL Server 2016’dan Azure’a dinamik olarak esnetin.|

Bu farklı seçeneklerle, Azure VM’lerinde çalışan SQL Server birçok senaryo için doğru tercih olur. Örneğin, bir Azure VM’sini şirket içi SQL Server makinesinde mümkün olduğunca benzer şekilde yapılandırmak isteyebilirsiniz. Alternatif olarak, aynı veritabanı sunucusunda ek uygulamalar ve hizmetler çalıştırmak isteyebilirsiniz. Karar vermenize yardımcı olacak iki kaynak bulunur:

 - [Azure sanal makinelerinde SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/), Azure VM’lerinde SQL Server’ın kullanılmasına ilişkin en iyi senaryolara genel bakış sağlar. 
 - [Bir bulut SQL Server seçeneği belirleyin: Azure SQL (PaaS) Veritabanı ya da Azure VM’lerde SQL Server (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md), SQL Veritabanı ve VM’de çalışan SQL Server arasında ayrıntılı bir karşılaştırma sağlar.

## <a name="create-a-new-sql-vm"></a>Yeni bir SQL sanal makinesi oluşturma
Aşağıdaki bölümlerde SQL Server sanal makine galeri görüntüleri için Azure portalının doğrudan bağlantıları verilmektedir. Seçtiğiniz görüntüye bağlı olarak, SQL Server lisans maliyetlerini dakika başına temelde ödeyebilir veya kendi lisansınızı getirebilirsiniz (KLG).

Bu işleme ilişkin adım adım yönergeler, [Azure portal'da SQL Server sanal makine hazırlama](virtual-machines-windows-portal-sql-server-provision.md) adlı öğreticide mevcuttur. Ayrıca,uygun makine boyutunu seçmeyi ve sağlama işlemi sırasında kullanılabilir diğer seçenekleri açıklayan [ SQL Server VM’ler için performans en iyi uygulamaları](virtual-machines-windows-sql-performance.md)’nı gözden geçirin.

## <a name="option-1:-create-a-sql-vm-with-per-minute-licensing"></a>Seçenek 1: Dakika başına lisanslama ile SQL sanal makinesi oluşturma
Aşağıdaki tabloda sanal makine galerisindeki kullanılabilir SQL Server görüntülerinin bir matrisi verilmektedir. Belirtilen sürüm, yayın ve işletim sisteminizle yeni bir SQL VM oluşturmaya başlamak için bağlantılardan birine tıklayın.

|Sürüm|İşletim Sistemi|Sürüm|
|---|---|---|
|**SQL 2016**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [Dev](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL 2014 SP1**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL 2014**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 SP3**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL 2008 R2 SP3**|Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2 SP3**|Windows Server 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## <a name="option-2:-create-a-sql-vm-with-an-existing-license"></a>Seçenek 2: Var olan bir lisans ile SQL sanal makinesi oluşturma
Ayrıca kendi lisansınızı getirebilirsiniz (KLG). Bu senaryoda, SQL Server Lisans için hiçbir ek bir ücret olmadan yalnızca VM için ödeme yaparsınız. Kendi lisansınızı kullanmak için, aşağıdaki SQL Server sürümleri, yayınları ve işletim sistemleri matrisini kullanın. Portalda, bu görüntü adlarına **{KLG}** ön eki getirilir.

|Sürüm|İşletim sistemi|Sürüm|
|---|---|---|
|**SQL Server 2016**|Windows Server 2012 R2|[Enterprise KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Standard KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Enterprise KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Enterprise KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

> [AZURE.IMPORTANT] KLG VM görüntülerini kullanmak için, [Azure’da Yazılım Güvencesi ile Lisans Taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility/) içeren bir Kuruluş Sözleşmeniz olmalıdır. Ayrıca, kullanmak istediğiniz SQL Server sürümü/yayını için geçerli bir lisans da gerekir. VM’nizi sağladıktan sonra **10** gün içinde [gerekli KLG bilgilerini Microsoft’a vermelisiniz.](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf)

## <a name="manage-your-sql-vm"></a>SQL VM’nizi yönetme
SQL Server sanal makinenizi sağladıktan sonra isteğe bağlı birkaç yönetim görevi vardır. Birçok yönden, SQL Server’ı tam olarak şirket içi SQL Server örneğindeki gibi yapılandırır ve yönetirsiniz. Ancak bazı görevler Azure’a özgüdür. Aşağıdaki bölümlerde daha fazla bilgi için bağlantılar ile birlikte bu alanlardan bazıları vurgulanmaktır.

### <a name="connect-to-the-vm"></a>VM’ye bağlanma
SQL Server VM’nize SQL Server Management Studio (SSMS) gibi araçlar üzerinden bağlanmak başlıca yönetim adımlarından biridir. Yeni SQL Server VM’ye bağlanma hakkındaki yönergeler için bkz. [Azure’da SQL Server Sanal Makinesine bağlanma](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Verilerinizi geçirme
Varolan bir veritabanınız varsa, bunu yeni sağlanan SQL VM'ye taşımak istersiniz. Geçiş seçenekleri ve kılavuzların listesi için bkz. [Azure VM’de bir Veritabanını SQL Server’a Geçirme](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Yüksek kullanılabilirliği yapılandırma
Size yüksek kullanılabilirlik gerekiyorsa, SQL Server Kullanılabilirlik gruplarını yapılandırmayı dikkate alın. Bu, bir sanal ağda birden fazla Azure VM’yi içerir. Azure portal bu yapılandırmayı sizin için ayarlayan bir şablona sahiptir. Daha fazla bilgi için bkz. [Azure Resource Manager sanal makinelerde AlwaysOn Kullanılabilirlik grubu yapılandırma](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). Kullanılabilirlik Grubunuzu ve ilgili dinleyiciyi el ile yapılandırmak istiyorsanız, bkz. [Azure VM’de AlwaysOn Kullanılabilirlik Grupları yapılandırma](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Diğer yüksek kullanılabilirlik dikkate alınacak noktaları için bkz. [Azure Virtual Machines’de SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Verilerinizi yedekleme
Azure sanal makineleri, blob depolama biriminde düzenli olarak veritabanınızın yedeklerini oluşturan [Otomatik Yedekleme](virtual-machines-windows-sql-automated-backup.md) özelliğinden yararlanabilir. Bu tekniği el ile de kullanabilirsiniz. Daha fazla bilgi için. bkz. [SQL Server Yedekleme ve Geri Yükleme için Azure Storage’ı Kullanma](virtual-machines-windows-use-storage-sql-server-backup-restore.md). Tüm yedekleme ve geri yükleme seçeneklerine genel bakış için bkz. [Azure Virtual Machines’de SQL Server için Yedekleme ve geri Yükleme](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Otomatik güncelleştirmeler
Azure sanal makineleri önemli Windows ve SQL Server güncelleştirmelerini otomatik olarak yüklemek için bir bakım penceresi zamanlamak üzere [Otomatik Düzeltme Eki Uygulama](virtual-machines-windows-sql-automated-patching.md) özelliğini kullanabilir.

### <a name="customer-experience-improvement-program-(ceip)"></a>Müşteri deneyimini geliştirme programı (CEIP)
Müşteri Deneyimini Geliştirme Programı (CEIP) varsayılan olarak etkindir. Bu, SQL Server’ın geliştirilmesine yardımcı olmak için Microsoft’a düzenli olarak raporlar gönderir. CEIP’i hazırladıktan sonra devre dışı bırakmak istemiyorsanız CEIP için herhangi bir yönetim görevi gerekmez. VM’ye uzak masaüstüyle bağlanarak CEIP özelleştirebilir ya da devre dışı bırakabilirsiniz. Ardından **SQL Server Hata ve Kullanım Raporlama** yardımcı programını çalıştırın. Raporlamayı devre dışı bırakmak için yönergeleri izleyin. 

Daha fazla bilgi için [Lisans Koşullarını kabul etme](https://msdn.microsoft.com/library/ms143343.aspx) konu başlığının CEIP bölümüne bakın. 

## <a name="next-steps"></a>Sonraki adımlar
Azure Virtual Machines’de SQL Server için.[Öğrenme Yolunu keşfedin](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/).

Başka sorunuz mu var? Önce, bkz. [Azure Virtual Machines’de SQL Server Kullanmaya Başlama SSS](virtual-machines-windows-sql-server-iaas-faq.md). Ayrıca sorularınızı ve yorumlarınızı, Microsoft ve toplulukla etkileşim kurmak amacıyla bir SQL VM konusunun alt kısmına da ekleyebilirsiniz.



<!--HONumber=Oct16_HO3-->


