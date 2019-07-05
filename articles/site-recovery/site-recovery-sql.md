---
title: SQL Server ve Azure Site Recovery ile SQL Server için olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Bu makalede, SQL Server ve Azure Site Recovery kullanarak SQL Server için olağanüstü durum kurtarma ayarlama açıklanır.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/30/2019
ms.author: sutalasi
ms.openlocfilehash: 1c44b10b54a5f58dff1aecf36c3633cc8ffbd8f0
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491785"
---
# <a name="set-up-disaster-recovery-for-sql-server"></a>SQL Server için olağanüstü durum kurtarmayı ayarlayın

Bu makalede, SQL Server iş sürekliliği ve olağanüstü durum kurtarma (BCDR) teknolojilerini bir birleşimini kullanarak bir uygulama SQL Server arka ucunu korumak açıklanır ve [Azure Site Recovery](site-recovery-overview.md).

Başlamadan önce nakliye ve etkin coğrafi çoğaltma otomatik yük devretme grupları Yük Devretme Kümelemesi, Always On kullanılabilirlik grupları, veritabanı yansıtma, dahil olmak üzere, SQL Server olağanüstü durum kurtarma olanaklarını oturum anladığınızdan emin olun.

## <a name="dr-recommendation-for-integration-of-sql-server-bcdr-technologies-with-site-recovery"></a>SQL Server BCDR teknolojileriyle Site Recovery ile tümleştirilmesi için DR öneri

Kurtarma SQL sunucuları için bir BCDR teknoloji seçimini dayalı olmalıdır RTO ve RPO ihtiyaçlarınıza göre tablo aşağıda. Bu seçimi yaptıktan sonra Site Recovery ile yük devretme işlemi tüm uygulamanızın kurtarılmasını düzenlemek için bu teknolojinin tümleştirilebilir.

**Dağıtım türü** | **BCDR teknolojisi** | **SQL için beklenen RTO** | **SQL için beklenen RPO** |
--- | --- | --- | ---
Azure Iaas VM üzerindeki veya şirket içi SQL Server| **[Her zaman üzerinde kullanılabilirlik grubu](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-2017)** | İkincil çoğaltma birincil olarak yapmak için geçen süre ile eşdeğerdir | İkincil bir çoğaltmaya zaman uyumsuz çoğaltma, bu nedenle bazı verilerin kaybolması yoktur.
Azure Iaas VM üzerindeki veya şirket içi SQL Server| **[Yük Devretme Kümelemesi (her zaman şirket FCI)](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server?view=sql-server-2017)** | Düğümler arasında yük devretme için geçen süre ile eşdeğerdir | Paylaşılan depolama alanı kullanır, bu nedenle yük devretme sırasında depolama örneğinin aynı görünümde kullanılabilir.
Azure Iaas VM üzerindeki veya şirket içi SQL Server| **[Veritabanı yansıtma (yüksek performanslı mod)](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server?view=sql-server-2017)** | Yansıtma sunucusu normal bir bekleme sunucusunun kullandığı hizmet zorlamak için geçen süre ile eşdeğerdir. | Çoğaltma zaman uyumsuzdur. Yansıtma veritabanının asıl veritabanı arkasında biraz lag. Boşluk genelde küçüktür howvever, asıl veya yansıtma sunucunun sistem ağır bir yük altında ise önemli hale gelebilir.<br></br>Günlük aktarma veritabanı yansıtma için ek olabilir ve zaman uyumsuz veritabanı yansıtma için iyi bir alternatiftir
Azure PaaS olarak SQL<br></br>(Elastik havuzlar, SQL veritabanı sunucuları) | **Etkin coğrafi çoğaltma** | 30 saniyelik bir kez tetiklendi<br></br>Yük devretme ikincil veritabanlarından biri etkinleştirildiğinde, diğer tüm ikincil veritabanı otomatik olarak yeni birincil siteye bağlanır. | 5 saniye RPO<br></br>Etkin coğrafi çoğaltma, zaman uyumsuz olarak kaydedilen işlem sayısı birincil veritabanında anlık görüntü yalıtımı kullanarak ikincil bir veritabanı çoğaltma için SQL Server Always On teknoloji yararlanır. <br></br>İkincil veri kısmi hareket hiçbir zaman olması garanti edilir.
Azure üzerinde etkin coğrafi çoğaltma ile yapılandırılmış PaaS olarak SQL<br></br>(SQL veritabanı yönetilen örneği, elastik havuzlar, SQL veritabanı sunucuları) | **Otomatik yük devretme grupları** | RTO 1 saat | 5 saniye RPO<br></br>Otomatik Yük devretme grupları üzerinde etkin coğrafi çoğaltma grubu semantiği sağlar ancak aynı zaman uyumsuz çoğaltma mekanizması kullanılır.
Azure Iaas VM üzerindeki veya şirket içi SQL Server| **Azure Site Recovery ile çoğaltmayı** | Genellikle 15 dakikadan kısa sürer. [Daha fazla bilgi edinin](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/) RTO Azure Site Recovery tarafından sağlanan SLA'sı hakkında bilgi edinmek için. | 1 saat uygulama tutarlılığı ve kilitlenme tutarlılığı için 5 dakika. 

> [!NOTE]
> Azure Site Recovery ile SQL iş yüklerini koruyorsanız bazı önemli noktalar:
> * Azure Site Recovery uygulamadan bağımsız olduğu ve bu nedenle, Azure Site Recovery tarafından desteklenen bir işletim sisteminde dağıtılan SQL server'ın herhangi bir sürümünü korunabilir. [Daha fazla bilgi edinin](vmware-physical-azure-support-matrix.md#replicated-machines).
> * Azure, Hyper-V, VMware veya fiziksel altyapı bir dağıtım için Site Recovery kullanmayı da tercih edebilirsiniz. Lütfen izleyin [Kılavuzu](site-recovery-sql.md#how-to-protect-a-sql-server-cluster-standard-editionsql-server-2008-r2) sonunda, SQL Server kümesi Azure Site Recovery ile koruma hakkında belge.
> * Olun göstermediği verileri makinede gözlemlenen oranı (Yazma Bayt / sn) değiştirmek [Site Recovery limitlerine](vmware-physical-azure-support-matrix.md#churn-limits). Windows makineleri için bu Görev Yöneticisi Performans sekmesindeki görüntüleyebilirsiniz. Yazma gözlemleyin her disk için hız.
> * Azure Site Recovery çoğaltma yük devretme kümesi örnekleri, depolama alanları doğrudan'ı destekler. [Daha fazla bilgi edinin](azure-to-azure-how-to-enable-replication-s2d-vms.md).
 

## <a name="disaster-recovery-of-application"></a>Olağanüstü durum kurtarma uygulama

**Azure Site Recovery kurtarma planları Yardım tüm uygulamanızın yük devretme ve yük devretme testi yönetir.** 

Kurtarma planı tam gereksinimlerinize göre özelleştirilmiş emin olmak için bazı Önkoşullar vardır. Herhangi bir SQL Server dağıtımı, genellikle bir Active Directory gerekir. Ayrıca uygulama katmanınızı bağlantısı gerekir.

### <a name="step-1-set-up-active-directory"></a>1\. adım: Active Directory'yi ayarlama

Kurtarma ikincil sitedeki SQL Server'ın düzgün çalışması için Active Directory'yi otomasyon'dan ayarlayın.

* **Küçük kuruluş**— tüm site için başarısız istiyorsanız uygulamalara ve şirket içi site için tek bir etki alanı denetleyicisine küçük bir sayı ile ikincil etki alanı denetleyicisi çoğaltmak için Site Recovery çoğaltma kullanmanızı öneririz veri merkezi veya azure'a.
* **Orta veya büyük kuruluş**— çok sayıda uygulama, bir Active Directory ormanı varsa ve uygulama veya iş yükü tarafından devralınırsa çalışmak isterseniz, Azure veya ikincil veri merkezinde bir ek etki alanı denetleyicisini ayarlama ayarlamanızı öneririz. İçin uzak bir siteyi kurtarmak için Always On kullanılabilirlik grupları kullanıyorsanız, kurtarılan bir SQL Server örneği için kullanılacak başka bir ek etki alanı denetleyicisi, azure'da veya ikincil sitede'kurmak ayarlamanızı öneririz.

Bu makaledeki yönergeler, bir etki alanı denetleyicisi ikincil konumdaki kullanılabilir olduğunu varsayın. [Daha fazla bilgi edinin](site-recovery-active-directory.md) Active Directory Site Recovery ile koruma hakkında.

### <a name="step-2-ensure-connectivity-with-other-application-tiers-and-web-tier"></a>2\. adım: Diğer uygulama katmanları ve web katmanı ile bağlantı emin olun

Veritabanı katmanı hedef Azure bölgesi içinde çalışır duruma geldikten sonra uygulama ve web katmanı ile bağlantı olduğundan emin olun. Yük devretme testi ile bağlantıyı doğrulamak için gerekli adımları önceden dikkat edilmelidir.

Birkaç örnek ile bağlantı konular için uygulamaları nasıl tasarlayacağınızı anlayın:
* [Bulutta olağanüstü durum kurtarma için uygulama tasarlama](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md)
* [Elastik havuz olağanüstü durum kurtarma stratejileri](../sql-database/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)

### <a name="step-3-integrate-with-always-on-active-geo-replication-or-auto-failover-groups-for-application-failover"></a>3\. adım: Always On, etkin coğrafi çoğaltma veya uygulama yük devretme için otomatik yük devretme grupları ile tümleştirme

BCDR teknolojileriyle Always On etkin coğrafi çoğaltma ve otomatik yük devretme grupları, hedef Azure bölgeniz çalışan SQL server'ın ikincil çoğaltmaları sahip. Bu nedenle, ilk adım, uygulamanın yük devretme için bu çoğaltma (ikincil bir etki alanı denetleyicisi zaten sahip olduğu varsayılarak) birincil olarak olmasını sağlamaktır. Bu adım, otomatik yük devretme bir yapmayı tercih ederseniz gerekli olmayabilir. Yalnızca veritabanı yük devretme tamamlandıktan sonra web veya uygulama katmanları yük devretme gerekir.

> [!NOTE] 
> Azure Site Recovery ile SQL makineleri koruduysanız, bu makineleri bir kurtarma grubu oluşturun ve bunların yük devretme kurtarma planında eklemek yeterlidir.

[Bir kurtarma planı oluşturma](site-recovery-create-recovery-plans.md) uygulama ve web katmanı sanal makineleri ile. İzleyin veritabanı katmanı yük devretmesi eklemek için aşağıdaki adımları:

1. Betikleri Azure Otomasyon hesabınızda içeri aktarın. Bu betikleri SQL kullanılabilirlik grubu yük devretme içeren içinde bir [Resource Manager sanal makinesi](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAG.ps1) ve [Klasik sanal makine](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAGClassic.ps1).

    [![Azure’a dağıtma](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)


1. ASR SQL FailoverAG kurtarma planının ilk grup bir ön eylem ekleyin.

1. Kullanılabilirlik grupları adını sağlamak için bir Otomasyon değişkeni oluşturma komut dosyasında mevcut yönergeleri izleyin.

### <a name="step-4-conduct-a-test-failover"></a>4\. Adım: Bir sınama yük devretmesi

SQL Always On gibi bazı BCDR teknolojileri, yük devretme testini yerel olarak desteklemez. Bu nedenle, aşağıdaki yaklaşımı önerilir **gibi teknolojiler ile tümleştirdiğinizde**:

1. Ayarlanan [Azure Backup](../backup/backup-azure-arm-vms.md) azure'da kullanılabilirlik grubu çoğaltması barındıran sanal makine üzerinde.

1. Kurtarma planı test yük devretmesi tetiklemeden önce sanal makine önceki adımda alınan yedekleme kurtarma.

    ![Azure yedeklemeden geri yükleme](./media/site-recovery-sql/restore-from-backup.png)

1. [Bir çekirdeği zorlama](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum#PowerShellProcedure) yedeklemeden geri sanal makinede.

1. Test yük devretme ağı bir IP dinleyici IP güncelleştirin.

    ![Dinleyici güncelleştirme IP](./media/site-recovery-sql/update-listener-ip.png)

1. Dinleyici çevrimiçi duruma getirin.

    ![Dinleyici çevrimiçi duruma getirme](./media/site-recovery-sql/bring-listener-online.png)

1. Bir yük dengeleyici IP her kullanılabilirlik grubu dinleyicisi için karşılık gelen ön uç IP havuzu altında oluşturulan ve arka uç havuzuna eklenmiş SQL sanal makinesi oluşturun.

     ![Yük Dengeleyici - ön uç IP havuzu oluşturma](./media/site-recovery-sql/create-load-balancer1.png)

    ![Yük Dengeleyici - arka uç havuzu oluşturma](./media/site-recovery-sql/create-load-balancer2.png)

1. Bu kurtarma planında sonraki kurtarma gruplardaki web katmanı tarafından izlenen uygulama katmanınızı yük devretmesi ekleyin. 
1. Uygulamanın uçtan uca yük devretme testi için kurtarma planının bir yük devretme testi yaparsınız.

## <a name="steps-to-do-a-failover"></a>Bir yük devretme yapma adımları

Adım 3'te Kurtarma planındaki betik eklediniz ve bunu adım 4'te özel bir yaklaşım ile bir yük devretme testi yaparak doğruladıktan sonra 3. adımda oluşturulan kurtarma planı yük devretme yapabilirsiniz.

Geçiş adımlarını uygulama ve web katmanları için yük devretme testi ve yük devretme kurtarma planları aynı olması gerektiğini unutmayın.

## <a name="how-to-protect-a-sql-server-cluster-standard-editionsql-server-2008-r2"></a>Bir SQL Server kümesi (standard edition/SQL Server 2008 R2) koruma

SQL Server Standard edition veya SQL Server 2008 R2 çalıştıran bir küme için SQL Server'ı korumak için Site Recovery çoğaltma kullanmanızı öneririz.

### <a name="azure-to-azure-and-on-premises-to-azure"></a>Azure ve şirket içi Azure için azure'a

Site Recovery, Konuk sunmaz bir Azure bölgesine çoğaltmak, küme desteği. SQL Server, Standard edition için düşük maliyetli olağanüstü durum kurtarma çözümü de sağlamaz. Bu senaryoda, bir tek başına birincil konumda SQL Server için SQL Server kümesini koruma ve ikincil kurtarma öneririz.

1. Birincil Azure bölgesi veya şirket içi sitede ek tek başına SQL Server örneğini yapılandırın.
1. Korumak istediğiniz veritabanları için bir yansıtma olarak hizmet verecek bir örneğini yapılandırın. Yüksek güvenilirlik modunda yansıtmasını yapılandırın.
1. Birincil sitede Site Recovery yapılandırma ([Azure](azure-to-azure-tutorial-enable-replication.md), [Hyper-V](site-recovery-hyper-v-site-to-azure.md) veya [VMware Vm'lerini/fiziksel sunucuları)](site-recovery-vmware-to-azure-classic.md).
1. Yeni SQL Server örneğinin ikincil bir siteye çoğaltmak için Site Recovery çoğaltma kullanın. Yüksek güvenilirlik yansıtma kopyası olduğundan, birincil küme ile senkronize edilir, ancak Site Recovery çoğaltması kullanılarak çoğaltılır.


![Standart küme](./media/site-recovery-sql/standalone-cluster-local.png)

### <a name="failback-considerations"></a>Yeniden çalışma hakkında önemli noktalar

SQL Server Standard kümeler için SQL server yedekleme ve geri yükleme, özgün kümeyle reestablishment yansıtma yansıtma örneğine geri dönme planlanmamış bir yük devretme sonrasında gerektirir.

## <a name="frequently-asked-questions"></a>Sıkça Sorulan Sorular

### <a name="how-does-sql-get-licensed-when-protected-with-azure-site-recovery"></a>Nasıl SQL Azure Site Recovery ile korunan, lisanslı?
Tüm Azure Site Recovery senaryoları (şirket içinden Azure’a olağanüstü durum kurtarma veya bölgeler arası Azure IaaS olağanüstü durum kurtarma) için SQL Server’a yönelik Azure Site Recovery çoğaltma işlemi Yazılım Güvencesi – Olağanüstü Durum Kurtarma avantajı kapsamındadır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/)

### <a name="will-azure-site-recovery-support-my-sql-version"></a>Azure Site Recovery, SQL sürümüm desteklenecek?
Azure Site Recovery, uygulamadan bağımsız olur. Bu nedenle, desteklenen bir işletim sisteminde dağıtılan SQL server'ın herhangi bir sürümünü Azure Site Recovery tarafından korunabilir. [Daha fazla bilgi edinin](vmware-physical-azure-support-matrix.md#replicated-machines)

## <a name="next-steps"></a>Sonraki adımlar
* [Daha fazla bilgi edinin](site-recovery-components.md) Site Recovery mimarisi hakkında.
* Azure SQL sunucuları için daha fazla bilgi edinin [yüksek kullanılabilirlik çözümleri](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions) için ikincil Azure bölgesine kurtarma.
* Azure SQL veritabanı için daha fazla bilgi edinin [iş sürekliliği](../sql-database/sql-database-business-continuity.md) ve [yüksek kullanılabilirlik](../sql-database/sql-database-high-availability.md) ikincil bir Azure bölgesinde kurtarma seçenekleri.
* Şirket içi SQL server makine için [daha fazla bilgi edinin](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md#hybrid-it-disaster-recovery-solutions) kurtarma Azure sanal Makineler'de yüksek kullanılabilirlik seçenekleri hakkında.
