---
title: "Azure Sanal Makineler’de SQL Server’a Genel Bakış | Microsoft Belgeleri"
description: "Azure Virtual Machines hizmetinde tam SQL Server sürümlerini çalıştırma hakkında bilgi edinin. Tüm SQL Server VM görüntülerinin ve ilgili içeriklerin doğrudan bağlantılarını alın."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: c505089e-6bbf-4d14-af0e-dd39a1872767
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: f0100423550046d18642180ce98e93ce3609749b
ms.contentlocale: tr-tr
ms.lasthandoff: 06/28/2017

---
# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Azure Virtual Machines’de SQL Server’a Genel Bakış
Bu konu başlığı, Azure sanal makinelerinde (VM’ler) SQL Server çalıştırmaya yönelik seçeneklerle birlikte [portal görüntülerinin bağlantılarını](#option-1-create-a-sql-vm-with-per-minute-licensing) ve [sık gerçekleştirilen görevlerin](#manage-your-sql-vm) genel açıklamasını içermektedir.

> [!NOTE]
> SQL Server’ı zaten biliyor ve yalnızca bir SQL Server sanal makinesinin nasıl dağıtılacağını görmek istiyorsanız bkz. [Azure portal’da bir SQL Server VM’si sağlama](virtual-machines-windows-portal-sql-server-provision.md).
> 
> 

## <a name="overview"></a>Genel Bakış
Veritabanı yöneticisi veya geliştiriciyseniz Azure VM’leri, şirket içi SQL Server iş yüklerinizi ve uygulamalarınızı Buluta taşımanız için bir yöntem sağlar. Aşağıdaki videoda, SQL Server Azure VM’leri için teknik bir genel bakış sağlanmıştır.

> [!VIDEO https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016/player]
> 
> 

Video, aşağıdaki alanları kapsar:

| Zaman | Alan |
| --- | --- |
| 00:21 |Azure VM’leri nedir? |
| 01:45 |Güvenlik |
| 02:50 |Bağlantı |
| 03:30 |Depolama güvenilirliği ve performansı |
| 05:20 |VM boyutları |
| 05:54 |Yüksek kullanılabilirlik ve SLA |
| 07:30 |Yapılandırma desteği |
| 08:00 |İzleme |
| 08:32 |Demo: SQL Server 2016 VM’si oluşturma |

> [!NOTE]
> Bu videoda SQL Server 2016'ya odaklanılmıştır; ancak Azure, SQL Server'ın 2012, 2014 ve 2016 dahil birçok sürümü için VM görüntüleri sağlamaktadır. 
> 
> 

## <a name="scenarios"></a>Senaryolar
Verilerinizi Azure’da barındırmayı seçmeniz için birçok neden vardır. Uygulamanız Azure’a taşınıyorsa verilerin taşınma performansı da artırır. Bununla birlikte, başka avantajları da vardır. Küresel bir erişim ağı ve olağanüstü durum kurtarma için birden fazla veri merkezine otomatik olarak erişebilirsiniz. Ayrıca veriler son derece güvenilir ve dayanıklı olur.

Azure VM’lerinde çalışan SQL Server, ilişkisel verilerinizi Azure’da depolamak için yararlanabileceğiniz bir seçenektir. Birkaç senaryo için iyi bir seçimdir. Örneğin, bir Azure VM’sini şirket içi SQL Server makinesinde mümkün olduğunca benzer şekilde yapılandırmak isteyebilirsiniz. Alternatif olarak, aynı veritabanı sunucusunda ek uygulamalar ve hizmetler çalıştırmak isteyebilirsiniz. Daha fazla senaryoyu ve dikkat edilmesi gereken noktayı göz önünde bulundurmanıza yardım edebilecek iki ana kaynak vardır:

* [Azure sanal makinelerinde SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/), Azure VM’lerinde SQL Server’ın kullanılmasına ilişkin en iyi senaryolara genel bakış sağlar. 
* [Bir bulut SQL Server seçeneği belirleyin: Azure SQL (PaaS) Veritabanı ya da Azure VM’lerde SQL Server (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md), SQL Veritabanı ve VM’de çalışan SQL Server arasında ayrıntılı bir karşılaştırma sağlar.

## <a name="create-a-new-sql-vm"></a>Yeni bir SQL sanal makinesi oluşturma
Aşağıdaki bölümlerde SQL Server sanal makine galeri görüntüleri için Azure portalının doğrudan bağlantıları verilmektedir. Seçtiğiniz görüntüye bağlı olarak, SQL Server lisans maliyetlerini dakika başına temelde ödeyebilir veya kendi lisansınızı getirebilirsiniz (KLG).

Yeni SQL VM oluşturmaya yönelik adım adım yönergeleri, [Azure Portal'da SQL Server sanal makine hazırlama](virtual-machines-windows-portal-sql-server-provision.md) öğreticisinde bulabilirsiniz. Ayrıca,uygun makine boyutunu seçmeyi ve sağlama işlemi sırasında kullanılabilir diğer seçenekleri açıklayan [ SQL Server VM’ler için performans en iyi uygulamaları](virtual-machines-windows-sql-performance.md)’nı gözden geçirin.

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a>Seçenek 1: Dakika başına lisanslama ile SQL sanal makinesi oluşturma
Aşağıdaki tabloda sanal makine galerisindeki en son SQL Server görüntülerinin bir matrisi verilmektedir. Belirtilen sürüm, yayın ve işletim sisteminizle yeni bir SQL VM oluşturmaya başlamak için bağlantılardan birine tıklayın. 

> [!TIP]
> Bu görüntülerin VM ve SQL fiyatlandırmasını anlamak için bkz. [SQL Server Azure VM’leri için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md).

| Sürüm | İşletim Sistemi | Sürüm |
| --- | --- | --- |
| **SQL Server 2016 SP1** |Windows Server 2016 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1ExpressWindowsServer2016), [Developer](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1DeveloperWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2ExpressWindowsServer2012R2) |
| **SQL Server 2012 SP3** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2) |

Bu listeye ek olarak kullanılabilen başka SQL Server sürümü ve işletim sistemi birleşimleri de vardır. Azure portalda market araması yaparak diğer görüntülere ulaşabilirsiniz. 

## <a id="BYOL"></a> Seçenek 2: Var olan bir lisans ile SQL sanal makinesi oluşturma
Ayrıca kendi lisansınızı getirebilirsiniz (KLG). Bu senaryoda, SQL Server Lisans için hiçbir ek bir ücret olmadan yalnızca VM için ödeme yaparsınız. Kendi lisansınızı kullanmak için, aşağıdaki SQL Server sürümleri, yayınları ve işletim sistemleri matrisini kullanın. Portalda, bu görüntü adlarına **{KLG}** ön eki getirilir.

> [!TIP]
> Kendi lisansınızı getirmek, sürekli üretim iş yüklerinde zaman içinde paradan tasarruf etmenizi sağlayabilir. Daha fazla bilgi için bkz. [SQL Server Azure VM’leri için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md).

| Sürüm | İşletim sistemi | Sürüm |
| --- | --- | --- |
| **SQL Server 2016 SP1** |Windows Server 2016 |[Enterprise KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016), [Standard KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2StandardWindowsServer2012R2) |
| **SQL Server 2012 SP2** |Windows Server 2012 R2 |[Enterprise KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard KLG](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2) |

Bu listeye ek olarak kullanılabilen başka SQL Server sürümü ve işletim sistemi birleşimleri de vardır. Azure portalında market araması yaparak diğer görüntüleri bulabilirsiniz ("{KLG} SQL Server" aratın).

> [!IMPORTANT]
> KLG VM görüntülerini kullanmak için [Azure’da Yazılım Güvencesi ile Lisans Taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility/) içeren bir Kuruluş Sözleşmeniz olmalıdır. Ayrıca, kullanmak istediğiniz SQL Server sürümü/yayını için geçerli bir lisans da gerekir. VM’nizi sağladıktan sonra **10** gün içinde [gerekli KLG bilgilerini Microsoft’a vermelisiniz.](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) 

> [!NOTE]
> Dakika başına ödemeli SQL Server VM'nin lisanslama modelini kendi lisansınızı kullanacak şekilde değiştirmezsiniz. Bu durumda yeni bir KLG VM oluşturmanız ve veritabanlarınızı yeni VM'ye geçirmeniz gerekir. 

## <a name="manage-your-sql-vm"></a>SQL VM’nizi yönetme
SQL Server sanal makinenizi sağladıktan sonra isteğe bağlı birkaç yönetim görevi vardır. Birçok yönden, SQL Server’ı tam olarak şirket içi SQL Server örneğindeki gibi yapılandırır ve yönetirsiniz. Ancak bazı görevler Azure’a özgüdür. Aşağıdaki bölümlerde daha fazla bilgi için bağlantılar ile birlikte bu alanlardan bazıları vurgulanmaktır.

### <a name="connect-to-the-vm"></a>VM’ye bağlanma
SQL Server VM’nize SQL Server Management Studio (SSMS) gibi araçlar üzerinden bağlanmak başlıca yönetim adımlarından biridir. Yeni SQL Server VM’ye bağlanma hakkındaki yönergeler için bkz. [Azure’da SQL Server Sanal Makinesine bağlanma](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Verilerinizi geçirme
Varolan bir veritabanınız varsa, bunu yeni sağlanan SQL VM'ye taşımak istersiniz. Geçiş seçenekleri ve kılavuzların listesi için bkz. [Azure VM’de bir Veritabanını SQL Server’a Geçirme](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Yüksek kullanılabilirliği yapılandırma
Size yüksek kullanılabilirlik gerekiyorsa, SQL Server Kullanılabilirlik gruplarını yapılandırmayı dikkate alın. Bu, bir sanal ağda birden fazla Azure VM’yi içerir. Azure portal bu yapılandırmayı sizin için ayarlayan bir şablona sahiptir. Daha fazla bilgi için bkz. [Azure Resource Manager sanal makinelerde AlwaysOn Kullanılabilirlik grubu yapılandırma](virtual-machines-windows-portal-sql-alwayson-availability-groups.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Kullanılabilirlik Grubunuzu ve ilgili dinleyiciyi el ile yapılandırmak istiyorsanız, bkz. [Azure VM’de AlwaysOn Kullanılabilirlik Grupları yapılandırma](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Diğer yüksek kullanılabilirlik dikkate alınacak noktaları için bkz. [Azure Virtual Machines’de SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Verilerinizi yedekleme
Azure sanal makineleri, blob depolama biriminde düzenli olarak veritabanınızın yedeklerini oluşturan [Otomatik Yedekleme](virtual-machines-windows-sql-automated-backup.md) özelliğinden yararlanabilir. Bu tekniği el ile de kullanabilirsiniz. Daha fazla bilgi için. bkz. [SQL Server Yedekleme ve Geri Yükleme için Azure Storage’ı Kullanma](virtual-machines-windows-use-storage-sql-server-backup-restore.md). Tüm yedekleme ve geri yükleme seçeneklerine genel bakış için bkz. [Azure Virtual Machines’de SQL Server için Yedekleme ve geri Yükleme](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Otomatik güncelleştirmeler
Azure sanal makineleri önemli Windows ve SQL Server güncelleştirmelerini otomatik olarak yüklemek için bir bakım penceresi zamanlamak üzere [Otomatik Düzeltme Eki Uygulama](virtual-machines-windows-sql-automated-patching.md) özelliğini kullanabilir.

### <a name="customer-experience-improvement-program-ceip"></a>Müşteri deneyimini geliştirme programı (CEIP)
Müşteri Deneyimini Geliştirme Programı (CEIP) varsayılan olarak etkindir. Bu, SQL Server’ın geliştirilmesine yardımcı olmak için Microsoft’a düzenli olarak raporlar gönderir. CEIP’i hazırladıktan sonra devre dışı bırakmak istemiyorsanız CEIP için herhangi bir yönetim görevi gerekmez. VM’ye uzak masaüstüyle bağlanarak CEIP özelleştirebilir ya da devre dışı bırakabilirsiniz. Ardından **SQL Server Hata ve Kullanım Raporlama** yardımcı programını çalıştırın. Raporlamayı devre dışı bırakmak için yönergeleri izleyin. 

Daha fazla bilgi için [Lisans Koşullarını kabul etme](https://msdn.microsoft.com/library/ms143343.aspx) konu başlığının CEIP bölümüne bakın. 

## <a name="next-steps"></a>Sonraki adımlar

Fiyatlandırmayla ilgili sorular için bkz. [SQL Server Azure VM’leri için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md) ve [Azure fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). Hedef SQL Server sürümünüzü **İşletim Sistemi/Yazılım** listesinden seçin. Ardından farklı boyutlardaki sanal makinelerin fiyatlarını görüntüleyebilirsiniz.

Başka sorunuz mu var? Önce, bkz. [Azure Virtual Machines’de SQL Server Kullanmaya Başlama SSS](virtual-machines-windows-sql-server-iaas-faq.md). Ayrıca sorularınızı ve yorumlarınızı, Microsoft ve toplulukla etkileşim kurmak amacıyla bir SQL VM konusunun alt kısmına da ekleyebilirsiniz.

