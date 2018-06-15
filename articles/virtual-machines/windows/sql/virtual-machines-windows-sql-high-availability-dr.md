---
title: Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için SQL Server | Microsoft Docs
description: Azure sanal makinelerinde çalışan SQL Server için HADR stratejileri çeşitli türleri bir tartışma.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 53981f7e-8370-4979-b26a-93a5988d905f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/27/2017
ms.author: mikeray
ms.openlocfilehash: e9b4ca959b93e097bb52a841cec02cc476ef5f48
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
ms.locfileid: "29401268"
---
# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Azure Sanal Makineler’de SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma

Microsoft Azure sanal makinelerle (VM'ler) SQL Server yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR) veritabanı çözümü maliyetlerini düşürmeye yardımcı olabilir. SQL Server HADR çözümlerinin çoğu hem de yalnızca Azure karma çözüm olarak Azure sanal makinelerde desteklenir. Yalnızca Azure bir çözümde tüm HADR sistem Azure üzerinde çalışır. Bir karma yapılandırmada, çözümün parçası Azure ve diğer bölümü çalıştırır şirket içi kuruluşunuzdaki çalıştırır. Azure ortamı esnekliğini bütçe ve SQL Server veritabanı sistemlerinizi HADR gereksinimlerini karşılamak için Azure'a kısmen veya tamamen taşımanızı sağlar.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="understanding-the-need-for-an-hadr-solution"></a>HADR çözümünü gereksinimini anlama
Bu, veritabanı sistem hizmet düzeyi sözleşmesi (SLA) gerektiren HADR özelliklerine sahip olduğunu emin olmak için size bağlıdır. Azure bulut Hizmetleri ve sanal makineler için hata kurtarma algılama için iyileştirme hizmeti gibi yüksek kullanılabilirlik mekanizmaları sağlar olgu kendisi istenen SLA karşılayabilecek garanti etmez. Bu mekanizmaların sanal makinelerin yüksek kullanılabilirliğini ancak VM içinde çalışan SQL Server olmayan yüksek kullanılabilirliğini koruyun. SQL Server örneği VM çevrimiçi ve iyi durumda olsa da başarısız mümkündür. Ayrıca, Azure tarafından sağlanan mekanizmaları VM'lerin yazılım veya donanım hataları ve işletim sistemi yükseltmeleri kurtarma gibi olaylar nedeniyle kapalı kalma süresi için izin yüksek kullanılabilirlik bile.

Ayrıca, coğrafi olarak yedekli depolama (GRS) coğrafi çoğaltma adlı bir özelliği ile uygulanır, Azure bir veritabanınız için yeterli olağanüstü durum kurtarma çözümü olmayabilir. Coğrafi çoğaltma veri zaman uyumsuz olarak gönderdiğinden, en son güncelleştirmeleri olağanüstü durumda kaybolmuş olabilir. Coğrafi çoğaltma sınırlamalar hakkında daha fazla bilgi ele alınmıştır [coğrafi çoğaltma veri ve günlük dosyalarını ayrı diskler üzerinde desteklenmiyor](#geo-replication-support) bölümü.

## <a name="hadr-deployment-architectures"></a>HADR dağıtım mimarisi
Azure'da desteklenen SQL Server HADR teknolojileri şunları içerir:

* [Always On kullanılabilirlik grupları](https://technet.microsoft.com/library/hh510230.aspx)
* [Her zaman yük devretme kümesi örnekleri](https://technet.microsoft.com/library/ms189134.aspx)
* [Günlük aktarma](https://technet.microsoft.com/library/ms187103.aspx)
* [SQL Server Yedekleme ve geri yükleme ile Azure Blob Depolama Birimi hizmeti](https://msdn.microsoft.com/library/jj919148.aspx)
* [Veritabanı yansıtma](https://technet.microsoft.com/library/ms189852.aspx) - SQL Server 2016'de kullanım dışı

Birlikte yüksek kullanılabilirlik ve olağanüstü durum kurtarma özelliklerine sahip bir SQL Server çözümü uygulamak için teknolojileri birleştirmek mümkündür. Kullandığınız teknolojisine bağlı olarak, karma bir dağıtım Azure sanal ağı ile VPN tüneli gerektirebilir. Aşağıdaki bölümler, bazı örnek dağıtım mimarilerinin gösterir.

## <a name="azure-only-high-availability-solutions"></a>Yalnızca Azure: yüksek oranda kullanılabilirlik çözümleri

Always On kullanılabilirlik kullanılabilirlik grupları adı verilen grupları ile-veritabanı düzeyinde SQL Server için yüksek oranda kullanılabilirlik çözümü olabilir. Yüksek oranda kullanılabilirlik çözümü bir örnek düzeyinde her zaman üzerinde yük devretme kümesi örnekleri ile - yük devretme kümesi örnekleri oluşturabilirsiniz. Ek artıklık için kullanılabilirlik grupları yük devretme kümesi örnekleri oluşturarak iki düzeyde artıklık oluşturabilirsiniz. 

| Teknoloji | Örnek mimarisi |
| --- | --- |
| **Kullanılabilirlik grupları:** |Aynı bölgede Azure vm'lerinde çalışan kullanılabilirlik çoğaltmalarının yüksek kullanılabilirlik sağlar. Windows Yük Devretme Kümelemesi bir Active Directory etki alanı gerektirdiğinden bir etki alanı denetleyicisi VM yapılandırmanız gerekir.<br/> ![Kullanılabilirlik grupları:](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>Daha fazla bilgi için bkz: [kullanılabilirlik gruplarını yapılandırma Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Yük devretme kümesi örnekleri** |Paylaşılan depolama gerektirir, yük devretme kümesi örneği'ne (FCI) 3 farklı yollarla oluşturulabilir.<br/><br/>1. Bağlı depolama kullanarak Azure Vm'lerde çalıştıran iki düğümlü bir yük devretme [Windows Server 2016 depolama alanları doğrudan \(S2D\) ](virtual-machines-windows-portal-sql-create-failover-cluster.md) için yazılım tabanlı bir sanal SAN sağlayın.<br/><br/>2. Bir üçüncü taraf kümeleme çözümü tarafından desteklenen depolama ile Azure Vm'lerde çalıştıran iki düğümlü yük devretme kümesi. SIOS DataKeeper kullanan bir örnek için bkz: [yüksek kullanılabilirlik için Yük Devretme Kümelemesi ve 3 taraf yazılımları SIOS DataKeeper kullanarak bir dosya paylaşımı](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>3. Azure Vm'lerinde uzak iSCSI hedef paylaşılan blok depolama ile ExpressRoute aracılığıyla çalıştıran iki düğümlü yük devretme kümesi. Örneğin, NetApp özel depolama (NPS), bir iSCSI hedefi Azure vm'lerine Equinix ile ExpressRoute aracılığıyla kullanıma sunar.<br/><br/>Üçüncü taraf paylaşılan depolama alanı ve veri çoğaltma çözümleri için yük devretme verileri erişimle ilgili sorunları satıcısına başvurmanız gerekir.<br/><br/>Bu kullanarak not üstünde FCI [Azure File storage](https://azure.microsoft.com/services/storage/files/) Bu çözüm, Premium depolama alanını değil çünkü henüz desteklenmiyor. Bunu en kısa sürede desteklemek için çalışıyoruz. |

## <a name="azure-only-disaster-recovery-solutions"></a>Yalnızca Azure: olağanüstü durum kurtarma çözümleri
Kullanılabilirlik grupları, veritabanı yansıtma veya Yedekleme kullanarak Azure, SQL Server veritabanları için olağanüstü durum kurtarma çözümüne sahip ve depolama BLOB'lar ile geri yükleyin.

| Teknoloji | Örnek mimarisi |
| --- | --- |
| **Kullanılabilirlik grupları:** |Olağanüstü durum kurtarma için Azure VM'de birden çok veri merkezi arasında çalışan kullanılabilirlik çoğaltmalarının. Bu bölgeler arası çözüm eksiksiz site kesilmesine karşı korur. <br/> ![Kullanılabilirlik grupları:](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>Bir bölge içinde tüm çoğaltmaları aynı bulut hizmeti ve aynı sanal ağ içinde olmalıdır. Her bölge ayrı VNet olmanız gerekir, çünkü bu çözümleri VNet VNet bağlantısı gerektirir. Daha fazla bilgi için bkz: [Azure portalını kullanarak VNet-VNet bağlantı yapılandırma](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md). Ayrıntılı yönergeler için bkz: [de farklı bölgelerdeki Azure Virtual Machines'de SQL Server kullanılabilirlik grubu yapılandırma](virtual-machines-windows-portal-sql-availability-group-dr.md).|
| **Veritabanı yansıtma** |Asıl ve yansıtma ve olağanüstü durum kurtarma için farklı veri merkezlerinde çalıştıran sunucular. Bir active directory etki alanı birden çok veri merkezi yayılamaz sunucu sertifikaları kullanarak dağıtmanız gerekir.<br/>![Veritabanı yansıtma](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Yedekleme ve geri yükleme ile Azure Blob Depolama Birimi hizmeti** |Olağanüstü durum kurtarma için farklı bir veri merkezinde blob depolama alanına doğrudan üretim veritabanları yedeklendi.<br/>![Yedekleme ve Geri Yükleme](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>Daha fazla bilgi için bkz: [yedekleme ve Azure Virtual Machines'de SQL Server için geri yükleme](virtual-machines-windows-sql-backup-recovery.md). |
| **Çoğaltılır ve Azure Site Recovery ile azure'a yük devretme SQL Server** |Üretim SQL Server'ın doğrudan olağanüstü durum kurtarma için farklı Azure veri merkezi, Azure depolama alanına çoğaltılan bir Azure veri merkezi.<br/>![Azure Site Recovery kullanarak Yinele](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_standalone_sqlserver-asr.png)<br/>Daha fazla bilgi için bkz: [korumak SQL Server olağanüstü durum kurtarma ve Azure Site Recovery kullanarak SQL Server](../../../site-recovery/site-recovery-sql.md). |


## <a name="hybrid-it-disaster-recovery-solutions"></a>Karma BT: Olağanüstü durum kurtarma çözümleri
Kullanılabilirlik grupları, veritabanı yansıtma, günlük aktarma ve Yedekleme kullanarak karma BT ortamında, SQL Server veritabanları için olağanüstü durum kurtarma çözümüne sahip ve Azure blogu depolama ile geri yükleyin.

| Teknoloji | Örnek mimarisi |
| --- | --- |
| **Kullanılabilirlik grupları:** |Azure Vm'leri ve şirket içi siteler arası olağanüstü durum kurtarma için çalışan diğer çoğaltmaları çalışan bazı kullanılabilirlik çoğaltmalarının. Üretim sitesini ya da şirket içi olabilir veya bir Azure veri merkezi.<br/>![Kullanılabilirlik grupları:](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Tüm kullanılabilirlik çoğaltmaları aynı yük devretme kümesinde olması gerektiğinden, kümenin her iki ağ (birden çok alt ağ yük devretme kümesi) span gerekir. Bu yapılandırma, Azure ile şirket içi ağınız arasında bir VPN bağlantısı gerektirir.<br/><br/>Veritabanlarınızın başarılı olağanüstü durum kurtarma için olağanüstü durum kurtarma sitesinde bir kopya etki alanı denetleyicisi de yüklemeniz gerekir.<br/><br/>Azure bir kopya var olan her zaman üzerinde kullanılabilirlik grubuna eklemek üzere SSMS çoğaltma Ekleme Sihirbazı'nı kullanmak da mümkündür. Daha fazla bilgi için bkz: Azure için her zaman üzerindeki kullanılabilirlik grubu genişletir. |
| **Veritabanı yansıtma** |Bir Azure VM ve diğer çalışan şirket içi sunucu sertifikaları kullanarak siteler arası olağanüstü durum kurtarma için çalışan bir iş ortağı. İş ortakları aynı Active Directory etki alanında olması gerekmez ve hiçbir VPN bağlantısı gereklidir.<br/>![Veritabanı yansıtma](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Başka bir veritabanı yansıtma senaryo, bir Azure VM ve diğer çalışan şirket içi siteler arası olağanüstü durum kurtarma için aynı Active Directory etki alanında çalışan bir iş ortağı içerir. A [Azure sanal ağı ve şirket içi ağ arasında VPN bağlantısı](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) gereklidir.<br/><br/>Veritabanlarınızın başarılı olağanüstü durum kurtarma için olağanüstü durum kurtarma sitesinde bir kopya etki alanı denetleyicisi de yüklemeniz gerekir. |
| **Günlük aktarma** |Bir Azure VM ve diğer çalışan şirket içi siteler arası olağanüstü durum kurtarma çalıştıran bir sunucu. Azure sanal ağı ve şirket içi ağ arasında bir VPN bağlantısı gerekli sağlayacak şekilde günlük aktarma Windows dosya paylaşımı üzerinde bağlıdır.<br/>![Günlük aktarma](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Veritabanlarınızın başarılı olağanüstü durum kurtarma için olağanüstü durum kurtarma sitesinde bir kopya etki alanı denetleyicisi de yüklemeniz gerekir. |
| **Yedekleme ve geri yükleme ile Azure Blob Depolama Birimi hizmeti** |Böylece, şirket içi olağanüstü durum kurtarma için Azure blob depolama alanına doğrudan yedeklenen üretim veritabanlarını.<br/>![Yedekleme ve Geri Yükleme](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>Daha fazla bilgi için bkz: [yedekleme ve Azure Virtual Machines'de SQL Server için geri yükleme](virtual-machines-windows-sql-backup-recovery.md). |
| **Çoğaltılır ve Azure Site Recovery ile azure'a yük devretme SQL Server** |Böylece, şirket içi üretim SQL Server olağanüstü durum kurtarma için Azure Storage doğrudan çoğaltılan.<br/>![Azure Site Recovery kullanarak Yinele](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_standalone_sqlserver-asr.png)<br/>Daha fazla bilgi için bkz: [korumak SQL Server olağanüstü durum kurtarma ve Azure Site Recovery kullanarak SQL Server](../../../site-recovery/site-recovery-sql.md). |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Azure SQL Server HADR için önemli noktalar
Azure VM'ler, depolama ve ağ bir şirket içi BT altyapısı sanallaştırılmamış daha farklı işletimsel özelliklere sahiptir. Bir başarılı, Azure HADR SQL Server çözümde bu farkları anlamak ve bunlara uyum sağlaması çözümünüzü tasarlama gerektirir.

### <a name="high-availability-nodes-in-an-availability-set"></a>Bir kullanılabilirlik kümesinde yüksek kullanılabilirlik düğümler
Azure kullanılabilirlik kümeleri, yüksek kullanılabilirlik düğümleri ayrı hata etki alanlarını (FDs) ve güncelleme etki alanına (UDs) yerleştirmek etkinleştirin. Azure VM'ler'ın aynı kullanılabilirlik kümesinde yerleştirilmesi bunları aynı bulut hizmetinde dağıtmanız gerekir. Yalnızca aynı bulut hizmetindeki düğümleri aynı kullanılabilirlik kümesinde yer alabilir. Daha fazla bilgi için bkz. [Sanal Makinelerin Kullanılabilirliğini Yönetme](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="failover-cluster-behavior-in-azure-networking"></a>Azure ağı yük devretme kümesi davranışı
RFC uyumlu olmayan DHCP hizmeti Azure oluşturma, belirli bir yük devretme küme yapılandırmaları, küme düğümlerinden birinin aynı IP adresi gibi yinelenen bir IP adresi atanmasını küme ağ adı nedeniyle başarısız olmasına neden olabilir. Windows Yük devretme kümesi özelliğini bağlıdır kullanılabilirlik grupları uygularken bu bir sorundur.

İki düğümlü bir küme oluşturduğunuzda ve çevrimiçi duruma senaryoyu göz önünde bulundurun:

1. Kümeyi çevrimiçi duruma sonra küme ağ adı için dinamik olarak atanmış bir IP adresi Düğüm1 ister.
2. DHCP hizmeti isteği Düğüm1'den gelen tanıdığı DHCP hizmeti tarafından verilen Düğüm1'ın kendi IP adresini dışında hiçbir IP adresi kendisi.
3. Windows algılar yinelenen bir adresi hem Düğüm1 ve yük devretme küme ağ adı için atanan ve çevrimiçi olması varsayılan küme grubu başarısız olur.
4. Varsayılan Küme grubu Düğüm1'ın IP adresi küme IP adresi olarak değerlendirir ve varsayılan küme grubu çevrimiçi duruma getirir Düğüm2 taşır.
5. Düğüm2 Düğüm1 ile bağlantı kurma girişiminde bulunduğunda Düğüm1'ın IP adresi kendisine çözümlediği çünkü Düğüm1 yönlendirilmiş paketleri hiçbir zaman Düğüm2 bırakın. Düğüm2 Düğüm1 ile bağlantı kurulamıyor, daha sonra çekirdek kaybeder ve kümeyi kapatır.
6. Bu arada, Düğüm1, Düğüm2 için paketleri gönderebilir, ancak Düğüm2 yanıt. Düğüm1 çekirdek kaybeder ve kümeyi kapatır.

Bu senaryoda, küme ağ adı çevrimiçi duruma getirmek için küme ağ adı için kullanılmayan bir statik IP adresi, bağlantı-yerel IP adresi 169.254.1.1 gibi gibi atayarak önlenebilir. Bu işlemi basitleştirmek için bkz: [yapılandırma Windows Yük devretme kümesinde Azure kullanılabilirlik grupları için](http://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx).

Daha fazla bilgi için bkz: [(GUI) Azure kullanılabilirlik gruplarını yapılandırma](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Kullanılabilirlik grubu dinleyicisi desteği
Kullanılabilirlik grubu dinleyicileri Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016 çalışan Azure Vm'lerinde desteklenir. Yük dengeli uç noktalarının kullanılabilirlik grubu düğümler Azure Vm'lerinde etkin kullanılarak bu desteği mümkün hale getirilir. Azure yanı sıra bu şirket içi çalışan çalışan iki istemci uygulamaları için çalışmaya dinleyiciler için özel yapılandırma adımları gerekir.

Dinleyiciyi ayarlamak için iki ana seçeneğiniz vardır: dış (Genel) veya dahili. Dış (Genel) dinleyici internet'e yönelik Yük Dengeleyici kullanır ve ile bir genel sanal IP (Internet üzerinden erişilebilen VIP) ilişkilendirilir. Bir iç dinleyicisi bir iç yük dengeleyici kullanır ve yalnızca aynı sanal ağ içindeki istemcileri destekler. İçin yük dengeleyici türü, doğrudan sunucu dönüşü etkinleştirmeniz gerekir. 

Kullanılabilirlik Grubu (örneğin, Azure bölgeleri kestiği dağıtımı) birden fazla Azure alt yayılırsa, istemci bağlantı dizesi içermelidir "**MultisubnetFailover = True**". Bu, farklı alt ağlar yinelemelere paralel bağlantı denemelerinde sonuçlanır. Dinleyici ayarlama hakkında yönergeler için bkz:

* [Kullanılabilirlik grupları için bir ILB dinleyicisi Azure'da yapılandırın](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
* [Kullanılabilirlik grupları için dış dinleyici Azure'da yapılandırın](../sqlclassic/virtual-machines-windows-classic-ps-sql-ext-listener.md).

Hizmet örneği için doğrudan bağlanarak her kullanılabilirlik çoğaltması için ayrı ayrı hala bağlanabilir. Ayrıca, kullanılabilirlik grupları istemcileri yansıtma veritabanı ile geriye dönük olarak uyumlu olduğundan, veritabanı çoğaltmaları benzer veritabanı yansıtması için yapılandırılmış olduğu sürece ortakları yansıtma gibi kullanılabilirlik çoğaltmalarının bağlanabilir:

* Bir birincil çoğaltma ve bir ikincil çoğaltma
* İkincil çoğaltma olarak okunabilir olmayan yapılandırılır (**okunabilir ikincil** seçeneği ayarlanmış **Hayır**)

ADO.NET veya SQL Server Native Client'ı kullanarak bu veritabanı yansıtma benzeri yapılandırması için karşılık gelen bir örnekte istemci bağlantı dizesi aşağıdadır:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

İstemci bağlantısı hakkında daha fazla bilgi için bkz:

* [SQL Server Native Client ile bağlantı dizesi anahtar sözcükleri kullanma](https://msdn.microsoft.com/library/ms130822.aspx)
* [İstemciler bir veritabanı yansıtma oturumuna (SQL Server) bağlanır](https://technet.microsoft.com/library/ms175484.aspx)
* [Karma BT'de kullanılabilirlik grubu dinleyicisi bağlanma](http://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
* [Kullanılabilirlik grubu dinleyicileri, istemci bağlantısı ve uygulama yük devretme (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
* [Veritabanı yansıtma bağlantı dizeleri kullanılabilirlik grupları ile kullanma](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Karma BT'de ağ gecikmesi
Şirket içi ağınız ve Azure arasında yüksek ağ gecikme süresi ile süreler olabilir varsayım HADR çözümünüzle dağıtmanız gerekir. Çoğaltmaları Azure'a dağıtırken, zaman uyumsuz tamamlama için eşitleme modu yerine zaman uyumlu yürütme kullanmanız gerekir. Ne zaman veritabanı sunucuları yansıtma dağıtma hem şirket içi ve Azure'da, yüksek performanslı modu yerine yüksek güvenlik modunu kullanın.

### <a name="geo-replication-support"></a>Coğrafi çoğaltma desteği
Coğrafi çoğaltma Azure disklerde veri dosyası ve günlük dosyası ayrı disklerde depolanması için aynı veritabanının desteklemez. GRS değişiklikleri her diskte bağımsız olarak ve zaman uyumsuz olarak çoğaltır. Bu mekanizma tek bir disk olarak coğrafi olarak çoğaltılmış kopyalama, ancak olmayan birden çok disk coğrafi olarak çoğaltılmış kopyalarını içinde yazma sırayı garanti altına alır. Veri dosyası ve günlük dosyası ayrı disklerde depolamak için bir veritabanı yapılandırırsanız, bir olağanüstü durum sonra kurtarılmış diskleri veri dosyasının daha güncel bir kopyasını SQL Server ve işlemleri ACID özelliklerini yazma tamamlanan günlüğüne keser günlük dosyası daha içerebilir. Coğrafi çoğaltma depolama hesabı üzerinde devre dışı bırakma seçeneği yoksa, tüm veri ve günlük dosyalarını verilen bir veritabanı için aynı disk üzerinde tutmanız gerekir. Veritabanının boyutu nedeniyle birden fazla disk kullanmanız gerekiyorsa, veri artıklık sağlamak için yukarıda listelenen olağanüstü durum kurtarma çözümlerden birini dağıtmak için gerekir.

## <a name="next-steps"></a>Sonraki adımlar
SQL Server ile bir Azure sanal makine oluşturmak ihtiyacınız varsa bkz [Azure üzerinde bir SQL Server sanal makine sağlama](virtual-machines-windows-portal-sql-server-provision.md).

Bir Azure VM'de çalışan SQL Server'dan en iyi performansı elde etmek kılavuzunda bkz [Azure Virtual Machines'de SQL Server için performans en iyi uygulamaları](virtual-machines-windows-sql-performance.md).

Azure Vm'lerde SQL Server çalıştırma ile ilgili diğer konular için bkz: [Azure Virtual Machines'de SQL Server](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Diğer kaynaklar
* [Azure'da yeni bir Active Directory ormanı yüklemek](../../../active-directory/active-directory-new-forest-virtual-machine.md)
* [Azure VM'de kullanılabilirlik grupları için yük devretme kümesi oluşturma](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)

