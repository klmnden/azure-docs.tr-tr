---
title: Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için SQL Server | Microsoft Docs
description: Azure sanal Makineler'de çalışan SQL Server HADR stratejileri, çeşitli türlerde tartışma.
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
ms.openlocfilehash: 379d3076559643b1445412074ed689e2e94a5ab2
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65408791"
---
# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Azure Sanal Makineler’de SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma

Microsoft Azure sanal makinelerinde (VM'ler) SQL Server ile bir yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR) veritabanı çözümü maliyetini düşürmeye yardımcı olabilir. SQL Server HADR çözümlerinin çoğu hem de yalnızca Azure hibrit çözüm olarak Azure sanal makinelerde desteklenir. Bir Azure çözümünüzde, Azure'da tüm HADR sistemin çalışır. Karma bir yapılandırmada, Azure ve diğer bölümü çalıştırmaları şirket içi kuruluşunuzdaki çözümün bir parçası çalıştırır. Azure ortamı esnekliğini kısmen veya tamamen bütçe ve SQL Server veritabanı sistemlerinizin HADR gereksinimlerini karşılamak için Azure'a taşımanıza olanak sağlar.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="understanding-the-need-for-an-hadr-solution"></a>HADR çözümünü gereksinimini anlama
Bu veritabanı sisteminizi hizmet düzeyi sözleşmesi (SLA) gerektiren HADR özelliklerine sahip olmak için size bağlıdır. Azure cloud services ve sanal makineler için hata kurtarma algılama hizmeti iyileştirme gibi yüksek kullanılabilirlik mekanizmaları sağlar olgu kendisi istenen SLA karşılayabilecek garanti etmez. Bu mekanizmalar sanal makinelerin yüksek kullanılabilirlik ancak olmayan sanal makinelerin içinde çalışan SQL Server yüksek kullanılabilirliğini koruyun. Sanal makine çevrimiçi ve iyi durumda olduğu sürece başarısız SQL Server örneği için mümkündür. Ayrıca, Azure tarafından sağlanan mekanizmalarını yazılım veya donanım arızalardan ve işletim sistemi yükseltmeleri gibi olaylar nedeniyle sanal makinelerin kapalı kalma süresi için izin yüksek kullanılabilirlik bile.

Ayrıca, coğrafi olarak yedekli depolama (GRS) coğrafi çoğaltma adlı bir özellik ile uygulanan, Azure, veritabanlarınızı bir yeterli olağanüstü durum kurtarma çözümü olmayabilir. Coğrafi çoğaltma veri zaman uyumsuz olarak gönderdiğinden, olağanüstü durumda en son güncelleştirmeleri kaybolabilir. Coğrafi çoğaltma sınırlamaları hakkında daha fazla bilgi kapsamdaki [coğrafi çoğaltma veri ve günlük dosyalarını ayrı diskler için desteklenmeyen](#geo-replication-support) bölümü.

## <a name="hadr-deployment-architectures"></a>HADR dağıtım mimarisi
Azure'da desteklenen SQL Server HADR teknolojiler şunları içerir:

* [Always On kullanılabilirlik grupları](https://technet.microsoft.com/library/hh510230.aspx)
* [Her zaman yük devretme kümesi örnekleri](https://technet.microsoft.com/library/ms189134.aspx)
* [Günlük aktarma](https://technet.microsoft.com/library/ms187103.aspx)
* [SQL Server Yedekleme ve geri yükleme ile Azure Blob Depolama hizmeti](https://msdn.microsoft.com/library/jj919148.aspx)
* [Veritabanı yansıtma](https://technet.microsoft.com/library/ms189852.aspx) - SQL Server 2016'da kullanım dışı

Birlikte yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri olan bir SQL Server çözümü uygulamak için teknolojileri birleştirmek mümkündür. Kullandığınız teknolojisine bağlı olarak, Azure sanal ağı ile VPN tüneli karma dağıtımı gerektirebilir. Aşağıdaki bölümler, örnek dağıtım mimarisi bazılarını gösterir.

## <a name="azure-only-high-availability-solutions"></a>Yalnızca Azure: Yüksek kullanılabilirlik çözümleri

Always On kullanılabilirlik kullanılabilirlik grupları adlı grupları ile-bir veritabanı düzeyinde SQL Server için yüksek oranda kullanılabilirlik çözümü olabilir. Yüksek oranda kullanılabilirlik çözümü bir örnek düzeyinde her zaman üzerinde yük devretme kümesi örnekleri ile - yük devretme kümesi örnekleri oluşturabilirsiniz. Ek artıklık için kullanılabilirlik grupları üzerinde yük devretme kümesi örnekleri oluşturarak iki düzeyde artıklık oluşturabilirsiniz. 

| Teknoloji | Örnek mimariler |
| --- | --- |
| **Kullanılabilirlik grupları** |Kullanılabilirlik çoğaltmaları aynı bölgedeki Azure vm'lerinde çalışan yüksek kullanılabilirlik sağlar. Windows Yük Devretme Kümelemesi bir Active Directory etki alanı gerektirdiğinden, VM'yi bir etki alanı denetleyicisi yapılandırmanız gerekir.<br/> ![Kullanılabilirlik grupları](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>Daha fazla bilgi için [kullanılabilirlik grupları yapılandırma (GUI) Azure](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Yük devretme kümesi örnekleri** |Paylaşılan depolama gerektirir, yük devretme kümesi örneği'ne (FCI) 3 farklı yollarla oluşturulabilir.<br/><br/>1. Bağlı depolama kullanarak Azure Vm'lerinde çalışan bir iki düğümlü yük devretme kümesi [Windows Server 2016 depolama alanları doğrudan \(S2D\) ](virtual-machines-windows-portal-sql-create-failover-cluster.md) yazılım tabanlı bir sanal SAN sağlamak için.<br/><br/>2. Bir üçüncü taraf kümeleme çözümü tarafından desteklenen depolama ile Azure Vm'lerinde çalışan bir iki düğümlü yük devretme kümesi. SIOS DataKeeper kullanan belirli bir örnek için bkz. [yüksek kullanılabilirlik için Yük Devretme Kümelemesi ve 3 taraf yazılım SIOS DataKeeper kullanarak bir dosya paylaşımı](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>3. ExpressRoute üzerinden uzak bir iSCSI hedef paylaşılan blok depolama ile Azure Vm'lerinde çalışan bir iki düğümlü yük devretme kümesi. Örneğin, NetApp özel depolama (NPS), iSCSI hedefi ExpressRoute ile Equinix Azure vm'lerine aracılığıyla kullanıma sunar.<br/><br/>Üçüncü taraf paylaşılan depolama ve veri çoğaltma çözümleri için yük devretme sırasında veri erişimle ilgili sorunları satıcısının başvurmanız gerekir.<br/><br/>Bu Not FCI üst kısmındaki [Azure dosya depolama](https://azure.microsoft.com/services/storage/files/) Bu çözüm, Premium depolama kullanmaz olduğundan henüz desteklenmiyor. Bunu yakında desteklemek için çalışıyoruz. |

## <a name="azure-only-disaster-recovery-solutions"></a>Yalnızca Azure: Olağanüstü durum kurtarma çözümleri
Kullanılabilirlik grupları, veritabanı yansıtmaya veya Yedekleme kullanarak azure'da SQL Server veritabanlarınız için bir olağanüstü durum kurtarma çözümüne sahip ve depolama blobları ile geri yükleyebilirsiniz.

| Teknoloji | Örnek mimariler |
| --- | --- |
| **Kullanılabilirlik grupları** |Kullanılabilirlik çoğaltması, olağanüstü durum kurtarma için birden çok veri merkezinde Azure sanal makinelerinde çalışan. Bu bölgeler arası çözüm tam site arızasına karşı korur. <br/> ![Kullanılabilirlik grupları](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>Bir bölge içinde tüm çoğaltmalar aynı bulut hizmeti ve aynı sanal ağ içinde olmalıdır. Her bölgeye ayrı bir sanal ağ olduğundan, bu çözümleri VNet sanal ağ bağlantısı gerektirir. Daha fazla bilgi için [Azure portalını kullanarak VNet-VNet bağlantı yapılandırma](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md). Ayrıntılı yönergeler için bkz. [farklı bölgelerdeki Azure sanal Makineler'de SQL Server kullanılabilirlik grubu yapılandırma](virtual-machines-windows-portal-sql-availability-group-dr.md).|
| **Veritabanı yansıtma** |Asıl ve yansıtma ve olağanüstü durum kurtarma için farklı veri merkezlerinde çalıştıran sunucular. Sunucu sertifikaları kullanarak dağıtmanız gerekir. SQL Server 2008 veya Azure VM'deki SQL Server 2008 R2 için SQL Server veritabanı yansıtma desteklenmiyor. <br/>![Veritabanı yansıtma](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Yedekleme ve geri yükleme ile Azure Blob Depolama hizmeti** |Olağanüstü durum kurtarma için farklı bir veri merkezinde blob depolama için doğrudan üretim veritabanları yedeklendi.<br/>![Yedekleme ve Geri Yükleme](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>Daha fazla bilgi için [yedekleme ve Azure sanal Makineler'de SQL Server için geri yükleme](virtual-machines-windows-sql-backup-recovery.md). |
| **Çoğaltma ve Azure Site Recovery ile azure'a yük devretme SQL Server** |Üretim SQL Server'ın bir Azure veri merkezi doğrudan Azure depolama, olağanüstü durum kurtarma için farklı Azure veri merkezine çoğaltılır.<br/>![Azure Site Recovery kullanarak çoğaltma](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_standalone_sqlserver-asr.png)<br/>Daha fazla bilgi için [korumak SQL Server olağanüstü durum kurtarma ve Azure Site Recovery kullanarak SQL Server](../../../site-recovery/site-recovery-sql.md). |


## <a name="hybrid-it-disaster-recovery-solutions"></a>Karma BT'de: Olağanüstü durum kurtarma çözümleri
Kullanılabilirlik grupları, veritabanı yansıtması, günlük aktarma ve Yedekleme kullanarak karma BT ortamındaki SQL Server veritabanlarınız için bir olağanüstü durum kurtarma çözümüne sahip ve Azure Web günlüğü depolama ile geri yükleyebilirsiniz.

| Teknoloji | Örnek mimariler |
| --- | --- |
| **Kullanılabilirlik grupları** |Azure Vm'leri ve siteler arası olağanüstü durum kurtarma için şirket içinde çalışan diğer çoğaltmaları çalışan bazı kullanılabilirlik çoğaltmalarının. Üretim sitesini ya da şirket içi olabilir veya bir Azure veri merkezinde.<br/>![Kullanılabilirlik grupları](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Tüm kullanılabilirlik çoğaltmalarının aynı yük devretme kümesinde olması gerektiğinden, küme her iki ağ (birden çok alt ağ yük devretme kümesi) yayılmış gerekir. Bu yapılandırma, Azure ve şirket içi ağ arasında bir VPN bağlantısı gerektirir.<br/><br/>Veritabanlarınızın başarılı olağanüstü durum kurtarma için olağanüstü durum kurtarma sitesinde bir kopya etki alanı denetleyicisi yüklemeniz gerekir.<br/><br/>Bir mevcut Always On kullanılabilirlik grubu için bir Azure çoğaltması ekleme SSMS'de çoğaltması Ekleme Sihirbazı'nı kullanmak mümkündür. Daha fazla bilgi için öğreticiye bakın: Her zaman açık kullanılabilirlik grubunuzun Azure'a genişletin. |
| **Veritabanı yansıtma** |Bir Azure VM ve diğer çalışan şirket içi sunucu sertifikaları kullanarak siteler arası olağanüstü durum kurtarma için çalışan bir iş ortağı. İş ortakları aynı Active Directory etki alanında olması gerekmez ve hiçbir VPN bağlantısı gereklidir.<br/>![Veritabanı yansıtma](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Başka bir veritabanı senaryo yansıtma, bir Azure VM ve diğer çalışan şirket içi siteler arası olağanüstü durum kurtarma için aynı Active Directory etki alanında çalışan bir iş ortağı içerir. A [Azure sanal ağı ile şirket içi ağ arasında VPN bağlantısı](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) gereklidir.<br/><br/>Veritabanlarınızın başarılı olağanüstü durum kurtarma için olağanüstü durum kurtarma sitesinde bir kopya etki alanı denetleyicisi yüklemeniz gerekir. SQL Server 2008 veya Azure VM'deki SQL Server 2008 R2 için SQL Server veritabanı yansıtma desteklenmiyor. |
| **Günlük aktarma** |Bir Azure VM ve diğer çalışan şirket içi siteler arası olağanüstü durum kurtarma için çalıştıran bir sunucu. Azure sanal ağı ve şirket içi ağ arasında bir VPN bağlantısı gerekiyor, bu nedenle Windows dosya paylaşımını, günlük aktarma bağlıdır.<br/>![Günlük aktarma](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Veritabanlarınızın başarılı olağanüstü durum kurtarma için olağanüstü durum kurtarma sitesinde bir kopya etki alanı denetleyicisi yüklemeniz gerekir. |
| **Yedekleme ve geri yükleme ile Azure Blob Depolama hizmeti** |Böylece, şirket içi olağanüstü durum kurtarma için Azure blob depolama için doğrudan yedeklenen üretim veritabanlarını.<br/>![Yedekleme ve Geri Yükleme](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>Daha fazla bilgi için [yedekleme ve Azure sanal Makineler'de SQL Server için geri yükleme](virtual-machines-windows-sql-backup-recovery.md). |
| **Çoğaltma ve Azure Site Recovery ile azure'a yük devretme SQL Server** |Böylece, şirket içi üretim SQL Server olağanüstü durum kurtarma için Azure depolama doğrudan çoğaltılır.<br/>![Azure Site Recovery kullanarak çoğaltma](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_standalone_sqlserver-asr.png)<br/>Daha fazla bilgi için [korumak SQL Server olağanüstü durum kurtarma ve Azure Site Recovery kullanarak SQL Server](../../../site-recovery/site-recovery-sql.md). |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Azure'da SQL Server HADR için önemli noktalar
Azure sanal makineleri, depolama ve ağ, bir şirket içi BT altyapısı sanallaştırılmamış değerinden farklı işletimsel özelliklerine sahip. Azure'da SQL Server HADR çözümünün başarılı bir uygulama bu farklılıkları anlamanıza ve bunlara uyum sağlaması çözümünüzü tasarlama gerektirir.

### <a name="high-availability-nodes-in-an-availability-set"></a>Bir kullanılabilirlik kümesinde yüksek kullanılabilirliği düğümleri
Azure kullanılabilirlik kümeleri, yüksek kullanılabilirlik düğümleri ayrı hata etki alanları (FD) ve güncelleştirme etki alanları (UD) yerleştirmek etkinleştirin. Azure Vm'leri'nın aynı kullanılabilirlik kümesine yerleştirilmesi bunları aynı bulut hizmetinde dağıtmanız gerekir. Yalnızca aynı bulut hizmetindeki düğümleri aynı kullanılabilirlik kümesine katılabilirsiniz. Daha fazla bilgi için bkz. [Sanal Makinelerin Kullanılabilirliğini Yönetme](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="failover-cluster-behavior-in-azure-networking"></a>Azure ağı yük devretme kümesi davranışı
RFC uyumlu olmayan DHCP hizmeti azure'da oluşturulmasını belirli yük devretme küme yapılandırmalarını küme düğümlerinden birinin aynı IP adresi gibi yinelenen bir IP adresi atanmış küme ağ adı nedeniyle başarısız olmasına neden olabilir. Windows Yük devretme kümesi özelliği bağlıdır kullanılabilirlik grupları uygularken bir sorun budur.

İki düğümlü bir küme oluşturduğunuzda ve çevrimiçi duruma getirilmeleri senaryoyu göz önünde bulundurun:

1. Kümeyi çevrimiçi duruma ve ardından küme ağ adı için dinamik olarak atanan bir IP adresi Düğüm1 ister.
2. DHCP hizmeti, istek Düğüm1'den gelen tanıdığı NODE1'ın kendi IP adresi dışındaki IP adresi DHCP hizmeti tarafından verilen kendisi.
3. Windows Düğüm1 ve yük devretme küme ağ adı yinelenen bir adresi atanır ve varsayılan küme grubu çevrimiçi duruma başaramıyor algılar.
4. Varsayılan Küme grubu NODE1'ın IP adresi küme IP adresi olarak değerlendirir ve varsayılan küme grubu çevrimiçi duruma getirir Düğüm2 taşır.
5. Düğüm2 Düğüm1 ile bağlantı kurmaya çalıştığında NODE1'ın IP adresi kendisine çözümlendiğinden Düğüm1 yönlendirilmiş paketleri Düğüm2 hiçbir zaman ayrılmaz. Düğüm2 Düğüm1 ile bağlantı kurulamıyor, ardından çekirdek kaybeder ve kümeyi kapatır.
6. Bu arada, Düğüm1, Düğüm2 için paket gönderebilirsiniz, ancak Düğüm2 yanıt. Düğüm1, çekirdek kaybeder ve kümeyi kapatır.

Bu senaryo, küme ağ adı çevrimiçi hale getirmek için küme ağ adı için kullanılmayan bir statik IP adresi, bağlantı-yerel IP adresi gibi 169.254.1.1, gibi atayarak önlenebilir. Bu işlemi kolaylaştırmak için bkz: [kullanılabilirlik grupları için yük devretme kümesi yapılandırma Windows azure'da](https://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx).

Daha fazla bilgi için [kullanılabilirlik grupları yapılandırma (GUI) Azure](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Kullanılabilirlik grubu dinleyicisi desteği
Kullanılabilirlik grubu dinleyicisi, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016 çalıştıran Azure Vm'leri üzerinde desteklenir. Bu destek yük dengeli uç noktaları etkinleştirilmiş olan kullanılabilirlik grubu düğümlerini Azure Vm'leri üzerinde kullanılarak gerçekleştirilir. Azure yanı sıra bunları şirket içinde çalışan çalıştıran her iki istemci uygulamalarının çalışması dinleyici için özel yapılandırma adımları gerekir.

Dinleyicinize ayarlamak için iki ana seçeneğiniz vardır: dış (Genel) veya iç. Dış (ortak) dinleyici, internet'e yönelik Yük Dengeleyici kullanır ve ilişkili olan bir genel sanal IP (internet üzerinden erişilebilir olan VIP). İç dinleyici iç yük dengeleyici kullanır ve yalnızca aynı sanal ağ içindeki istemcileri destekler. Yük Dengeleyici türü için doğrudan sunucu dönüşü etkinleştirmeniz gerekir. 

Kullanılabilirlik grubu birden çok Azure alt ağlar (örneğin, Azure bölgeleri aştığında dağıtımı) yayılırsa, istemci bağlantı dizesi içermelidir "**MultisubnetFailover = True**". Bu, paralel bağlantı denemelerinde de çoğaltmaları, farklı alt ağlarda sonuçlanır. Dinleyici ayarlama hakkında yönergeler için bkz:

* [Azure'da kullanılabilirlik grupları için ILB dinleyicisi yapılandırma](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
* [Azure'da kullanılabilirlik grupları için dış dinleyici yapılandırma](../sqlclassic/virtual-machines-windows-classic-ps-sql-ext-listener.md).

Her kullanılabilirlik çoğaltması için hizmet örneği için doğrudan bağlanarak, ayrı olarak yine de bağlanabilir. Ayrıca, kullanılabilirlik grupları veritabanı yansıtma istemcileri ile geriye dönük uyumlu olduğundan, veritabanı çoğaltmaları benzer veritabanı yansıtma için yapılandırılmış olduğu sürece, iş ortakları yansıtma gibi kullanılabilirlik çoğaltmalarının bağlanabilirsiniz:

* Bir birincil çoğaltma ve bir ikincil çoğaltma
* İkincil çoğaltma olarak okunabilir olmayan yapılandırılır (**okunabilir ikincil** seçeneğini ayarlamak **Hayır**)

ADO.NET veya SQL Server Native Client'ı kullanarak bu veritabanı yansıtma benzeri yapılandırması için karşılık gelen bir örnekte istemci bağlantı dizesi aşağıda verilmiştir:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

İstemci bağlantısı hakkında daha fazla bilgi için bkz:

* [Bağlantı dizesi anahtar sözcükleri ile SQL Server Yerel İstemcisi kullanma](https://msdn.microsoft.com/library/ms130822.aspx)
* [İstemciler yansıtma oturumu (SQL Server) bir veritabanına bağlanma](https://technet.microsoft.com/library/ms175484.aspx)
* [Karma BT'de kullanılabilirlik grubu dinleyicisi bağlanma](https://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
* [Kullanılabilirlik grubu dinleyicisi, istemci bağlantısı ve uygulama yük devretme (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
* [Kullanılabilirlik grupları ile veritabanı yansıtma bağlantı dizelerini kullanma](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Karma BT'de ağ gecikmesi
Şirket içi ağınız ve Azure arasında yüksek ağ gecikme süresi ile süreler olabilir varsayımına HADR çözümünüzle dağıtmanız gerekir. Çoğaltmaları Azure'a dağıtırken, eşitleme modunu zaman uyumlu işleme yerine zaman uyumsuz tamamlama kullanmanız gerekir. Ne zaman veritabanı yansıtma sunucuları dağıtma hem şirket içinde ve Azure'da yüksek performanslı modu yerine yüksek güvenlik modu kullanın.

### <a name="geo-replication-support"></a>Coğrafi çoğaltma desteği
Azure diskleri coğrafi çoğaltma veri dosyası ve günlük dosyası aynı veritabanının farklı disklerde depolanacak desteklemez. GRS her disk üzerindeki değişiklikleri bağımsız olarak ve zaman uyumsuz olarak çoğaltır. Bu mekanizma, tek bir disk olarak coğrafi olarak çoğaltılmış kopyalama, ancak değil coğrafi olarak yedekli kopyalar birden çok disk içinde yazma sırasını garanti eder. Kendi veri dosyası ve günlük dosyası ayrı disklere depolamak için bir veritabanı yapılandırırsanız, kurtarılan diskleri bir olağanüstü durum sonrasında veri dosyası daha güncel bir kopyasını SQL Server ve işlem ACID özelliklerini yazma önceden yazılan günlük keser günlük dosyasından içerebilir ctions. Coğrafi çoğaltma depolama hesabındaki devre dışı bırakma seçeneği yoksa, tüm veri ve günlük dosyalarını belirli bir veritabanı için aynı disk üzerinde tutmanız gerekir. Veritabanı boyutu nedeniyle birden fazla disk kullanmanız gerekiyorsa veri yedekliği emin olmak için yukarıda listelenen olağanüstü durum kurtarma çözümleri birini dağıtmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
SQL Server ile bir Azure sanal makine oluşturmanız gerekiyorsa, bkz. [azure'da bir SQL Server sanal makinesi sağlama](virtual-machines-windows-portal-sql-server-provision.md).

Bir Azure sanal makinesinde çalışan SQL Server en iyi performansı elde için kılavuzunda bkz [Azure sanal Makineler'de SQL Server için en iyi performans](virtual-machines-windows-sql-performance.md).

Azure Vm'lerinde SQL Server çalıştırma ile ilgili diğer konular için bkz [Azure Virtual Machines'de SQL Server](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Diğer kaynaklar
* [Azure'da yeni bir Active Directory ormanı yükleme](../../../active-directory/active-directory-new-forest-virtual-machine.md)
* [Azure VM'de kullanılabilirlik grupları için yük devretme kümesi oluşturma](https://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)

