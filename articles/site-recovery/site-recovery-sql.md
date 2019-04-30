---
title: SQL Server ve Azure Site Recovery ile SQL Server için olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Bu makalede, SQL Server ve Azure Site Recovery kullanarak SQL Server için olağanüstü durum kurtarma ayarlama açıklanır.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: sutalasi
ms.openlocfilehash: 67526eddd19c5869aa54432f963d9b80396f878d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61471732"
---
# <a name="set-up-disaster-recovery-for-sql-server"></a>SQL Server için olağanüstü durum kurtarmayı ayarlayın

Bu makalede, SQL Server iş sürekliliği ve olağanüstü durum kurtarma (BCDR) teknolojilerini bir birleşimini kullanarak bir uygulama SQL Server arka ucunu korumak açıklanır ve [Azure Site Recovery](site-recovery-overview.md).

Başlamadan önce yük devretme kümelemesi, Always On kullanılabilirlik grupları, veritabanı yansıtması ve günlük aktarma da dahil olmak üzere, SQL Server olağanüstü durum kurtarma olanaklarını anladığınızdan emin olun.


## <a name="sql-server-deployments"></a>SQL Server dağıtımları

SharePoint, Dynamics ve SAP, veri hizmetlerini uygulamak amacıyla gibi uygulamaları ile tümleştirilebilir ve birçok iş yükleri, SQL Server bir temel kullanın.  SQL Server, çeşitli yollarla dağıtılabilir:

* **Tek başına SQL Server**: SQL Server ve tüm veritabanlarını tek bir makinede (fiziksel veya sanal) barındırılır. Sanallaştırılmış konak küme yerel yüksek kullanılabilirlik için kullanılır. Konuk düzeyinde yüksek kullanılabilirlik uygulanmadı.
* **Örneği (FCI üzerinde her zaman) SQL Server Yük devretme**: Windows Yük devretme kümesinde paylaşılan diskleri ile örnekli SQL Server çalıştıran iki veya daha fazla düğüm yapılandırılır. Bir düğüm kapalı ise, küme SQL Server başka bir örneğine devredebilir. Bu kurulum, genellikle bir birincil sitede yüksek kullanılabilirlik uygulamak için kullanılır. Bu dağıtım hatası ya da kesinti paylaşılan depolama katmanında karşı korumaz. İSCSI, fiber kanal veya paylaşılan vhdx'i kullanarak paylaşılan bir disk uygulanabilir.
* **SQL Always On kullanılabilirlik grupları**: İki veya daha fazla düğüm hiçbir şey küme, zaman uyumlu çoğaltma ve otomatik yük devretme ile bir kullanılabilirlik grubunda yapılandırılmış SQL Server veritabanları ile paylaşılan olarak ayarlanır.

  Bu makalede, uzak bir site veritabanını kurtarmak için aşağıdaki yerel SQL olağanüstü durum kurtarma teknolojilerden yararlanır:

* SQL Always On kullanılabilirlik SQL Server 2012 veya 2014 Enterprise sürümleri için olağanüstü durum kurtarma sağlamak için grupları.
* SQL veritabanı yüksek güvenilirlik modunda, SQL Server 2008 R2 veya SQL Server Standard sürümü (tüm sürümler) için yansıtma.

## <a name="site-recovery-support"></a>Site Recovery desteği

### <a name="supported-scenarios"></a>Desteklenen senaryolar
Site Recovery, SQL Server tabloda özetlendiği gibi koruyabilir.

**Senaryo** | **İkincil bir siteye** | **Azure’a**
--- | --- | ---
**Hyper-V** | Evet | Evet
**VMware** | Evet | Evet
**Fiziksel sunucu** | Evet | Evet
**Azure** |NA| Evet

### <a name="supported-sql-server-versions"></a>Desteklenen SQL Server sürümleri
Bu SQL Server sürümleri için desteklenen senaryolar desteklenir:

* SQL Server 2016 Enterprise ve Standard
* SQL Server 2014 Enterprise ve Standard
* SQL Server 2012 Enterprise ve Standard
* SQL Server 2008 R2 Enterprise ve Standard

### <a name="supported-sql-server-integration"></a>Desteklenen SQL Server Tümleştirme

Site kurtarma, olağanüstü durum kurtarma çözümü sağlamak için tabloda özetlenen yerel SQL Server BCDR teknolojileriyle tümleştirilebilir.

**Özellik** | **Ayrıntılar** | **SQL Server** |
--- | --- | ---
**AlwaysOn kullanılabilirlik grubu** | Tek başına birden fazla SQL Server'ın birden fazla düğüme sahip bir yük devretme kümesinde çalıştırın.<br/><br/>Veritabanları, SQL Server örneklerinde (yansıtılmış) kopyalanabilir ve böylece hiçbir paylaşılan depolama gerekli yük devretme grupları gruplandırılabilir.<br/><br/>Bir birincil site ve bir veya daha fazla ikincil site arasında olağanüstü durum kurtarma sağlar. İki düğüm bir paylaşılan SQL Server veritabanlarını kümeyle zaman uyumlu çoğaltma ve otomatik yük devretme ile bir kullanılabilirlik grubunda yapılandırılmış hiçbir şey ayarlanabilir. | SQL Server 2016, SQL Server 2014 ve SQL Server 2012 Enterprise edition
**Yük Devretme Kümelemesi (her zaman şirket FCI)** | SQL Server şirket içi SQL Server iş yükleri için yüksek kullanılabilirlik Windows Yük devretme yararlanır.<br/><br/>Paylaşılan disk ile SQL Server örneklerini çalıştıran düğümlerin bir yük devretme kümesinde yapılandırılabilir. Kapalı örneğini ise küme için farklı bir devreder.<br/><br/>Küme hatası veya paylaşılan depolama kesintilerine karşı koruma sağlamaz. Paylaşılan disk iSCSI, fiber kanal uygulanabilir veya paylaşılan Vhdx'ler. | SQL Server Enterprise sürümleri<br/><br/>SQL Server Standard sürümü (yalnızca iki düğüm sınırlı)
**Veritabanı yansıtma (yüksek güvenlik modu)** | Tek bir ikincil kopya için tek bir veritabanını korur. Her iki yüksek güvenilirlik (zaman uyumlu) kullanılabilir ve yüksek performans (uyumsuz) çoğaltma modu. Bir yük devretme kümesi gerektirmez. | SQL Server 2008 R2<br/><br/>SQL Server Enterprise tüm sürümler
**Tek başına SQL Server** | Tek bir sunucu üzerinde (fiziksel veya sanal), veritabanı ve SQL Server barındırılır. Konak küme sanal sunucusudur yüksek kullanılabilirlik için kullanılır. Konuk düzeyinde yüksek kullanılabilirlik. | Enterprise veya Standard edition

## <a name="deployment-recommendations"></a>Dağıtım önerileri

Bu tablo, Site Recovery ile SQL Server BCDR teknolojileriyle tümleştirme için önerilerimizle özetler.

| **Sürüm** | **Sürüm** | **Dağıtım** | **Şirket içinden şirket içi** | **Şirket içinden Azure** |
| --- | --- | --- | --- | --- |
| SQL Server 2016, 2014 veya 2012 |Enterprise |Yük devretme kümesi örneği |Always On kullanılabilirlik grupları |Always On kullanılabilirlik grupları |
|| Enterprise |Always On kullanılabilirlik grupları için yüksek kullanılabilirlik |Always On kullanılabilirlik grupları |Always On kullanılabilirlik grupları |
|| Standart |Yük devretme kümesi örneği (FCI) |Site Recovery çoğaltma ile yerel yansıtma |Site Recovery çoğaltma ile yerel yansıtma |
|| Kurumsal veya standart |Tek Başına |Site Recovery çoğaltması |Site Recovery çoğaltması |
| SQL Server 2008 R2 veya 2008 |Kurumsal veya standart |Yük devretme kümesi örneği (FCI) |Site Recovery çoğaltma ile yerel yansıtma |Site Recovery çoğaltma ile yerel yansıtma |
|| Kurumsal veya standart |Tek Başına |Site Recovery çoğaltması |Site Recovery çoğaltması |
| SQL Server (tüm sürümler) |Kurumsal veya standart |Yük devretme kümesi örneği - DTC uygulama |Site Recovery çoğaltması |Desteklenmiyor |

## <a name="deployment-prerequisites"></a>Dağıtım önkoşulları

* Desteklenen bir SQL Server sürümünü çalıştıran bir şirket içi SQL Server dağıtımı. Genellikle, ayrıca Active Directory için SQL server'ınızı gerekir.
* Senaryo için dağıtmak istediğiniz gereksinimleri. Destek gereksinimleri hakkında daha fazla bilgi [azure'a çoğaltma](site-recovery-support-matrix-to-azure.md) ve [şirket içi](site-recovery-support-matrix.md), ve [dağıtım önkoşulları](site-recovery-prereq.md).

## <a name="set-up-active-directory"></a>Active Directory'yi ayarlama

Kurtarma ikincil sitedeki SQL Server'ın düzgün çalışması için Active Directory'yi otomasyon'dan ayarlayın.

* **Küçük kuruluş**— tüm site için başarısız istiyorsanız uygulamalara ve şirket içi site için tek bir etki alanı denetleyicisine küçük bir sayı ile ikincil etki alanı denetleyicisi çoğaltmak için Site Recovery çoğaltma kullanmanızı öneririz veri merkezi veya azure'a.
* **Orta veya büyük kuruluş**— çok sayıda uygulama, bir Active Directory ormanı varsa ve uygulama veya iş yükü tarafından devralınırsa çalışmak isterseniz, Azure veya ikincil veri merkezinde bir ek etki alanı denetleyicisini ayarlama ayarlamanızı öneririz. İçin uzak bir siteyi kurtarmak için Always On kullanılabilirlik grupları kullanıyorsanız, kurtarılan bir SQL Server örneği için kullanılacak başka bir ek etki alanı denetleyicisi, azure'da veya ikincil sitede'kurmak ayarlamanızı öneririz.

Bu makaledeki yönergeler, bir etki alanı denetleyicisi ikincil konumdaki kullanılabilir olduğunu varsayın. [Daha fazla bilgi edinin](site-recovery-active-directory.md) Active Directory Site Recovery ile koruma hakkında.


## <a name="integrate-with-sql-server-always-on-for-replication-to-azure"></a>Azure'a çoğaltma için SQL Server Always On ile tümleştirin

Yapmanız gerekenler şu şekildedir:

1. Betikleri Azure Otomasyon hesabınızda içeri aktarın. Bu betikleri SQL kullanılabilirlik grubu yük devretme içeren içinde bir [Resource Manager sanal makinesi](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAG.ps1) ve [Klasik sanal makine](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAGClassic.ps1).

    [![Azure’a dağıtma](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)


1. ASR SQL FailoverAG kurtarma planının ilk grup bir ön eylem ekleyin.

1. Kullanılabilirlik grupları adını sağlamak için bir Otomasyon değişkeni oluşturma komut dosyasında mevcut yönergeleri izleyin.

### <a name="steps-to-do-a-test-failover"></a>Yük devretme testi yapma adımları

SQL Always On yerel olarak test yük devretmesi desteklemiyor. Bu nedenle, şunları öneririz:

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

1. Kurtarma planının bir yük devretme testi yaparsınız.

### <a name="steps-to-do-a-failover"></a>Bir yük devretme yapma adımları

Kurtarma planında betik eklediniz ve kurtarma planı yük devretme testi yaparak doğrulanmış sonra kurtarma planı yük devretme yapabilirsiniz.


## <a name="integrate-with-sql-server-always-on-for-replication-to-a-secondary-on-premises-site"></a>Bir ikincil şirket içi siteye çoğaltma için SQL Server Always On ile tümleştirin

SQL Server kullanılabilirlik gruplarını yüksek kullanılabilirlik (veya bir FCI) kullanıyorsa kurtarma sitesinde de kullanılabilirlik grupları kullanmanızı öneririz. Dağıtılmış işlemler kullanmayan uygulamalar için geçerli olduğunu unutmayın.

1. [Veritabanlarını yapılandırma](https://msdn.microsoft.com/library/hh213078.aspx) kullanılabilirlik gruplarına.
1. İkincil sitede bir sanal ağ oluşturun.
1. Sanal ağ ve birincil site arasındaki siteden siteye VPN bağlantısı ayarlayın.
1. Kurtarma sitesinde bir sanal makine oluşturma ve SQL Server yükleyin.
1. Mevcut Always On kullanılabilirlik grupları için yeni bir SQL Server sanal genişletin. Bu SQL Server örneği, bir zaman uyumsuz çoğaltma kopya olarak yapılandırın.
1. Bir kullanılabilirlik grubu dinleyicisi oluşturun veya var olan dinleyiciyi zaman uyumsuz çoğaltma sanal makinesi dahil olacak şekilde güncelleştirin.
1. Uygulama grubu dinleyicisi kullanarak ayarlandığından emin olun. Kurulum, veritabanı sunucusu adı kullanılarak ise, bu nedenle, yük devretme sonrasında yeniden yapılandırmanız gerekmez dinleyici kullanacak şekilde güncelleştirmek.

Dağıtılmış işlemler kullanan uygulamalar için Site Recovery ile dağıttığınız olan öneririz [VMware/fiziksel sunucu siteden siteye çoğaltmanın](site-recovery-vmware-to-vmware.md).

### <a name="recovery-plan-considerations"></a>Kurtarma planı değerlendirmeleri
1. Bu örnek betik, birincil ve ikincil sitelerde VMM kitaplığına ekleyin.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

1. Uygulama için bir kurtarma planı oluşturduğunuzda, kullanılabilirlik grupları başarısız yönelik bir betiği çağırır Script Grup 1 adım, bir ön eylem ekleyin.

## <a name="protect-a-standalone-sql-server"></a>Tek başına SQL Server'ı koruma

Bu senaryoda, SQL Server makinesinde korumak için Site Recovery çoğaltma kullanmanızı öneririz. Uygulanacak adımlar, SQL Server VM veya fiziksel sunucu ve olup Azure'a çoğaltmak istediğiniz veya ikincil bir site şirket olduğunu bağlı olacaktır. Hakkında bilgi edinin [Site Recovery senaryoları](site-recovery-overview.md).

## <a name="protect-a-sql-server-cluster-standard-editionwindows-server-2008-r2"></a>SQL Server kümesini (standard edition/Windows Server 2008 R2) koruma

SQL Server Standard edition veya SQL Server 2008 R2 çalıştıran bir küme için SQL Server'ı korumak için Site Recovery çoğaltma kullanmanızı öneririz.

### <a name="on-premises-to-on-premises"></a>Şirket içinden şirket içine

* Dağıtılmış işlemler uygulamayı kullanıyorsa, dağıttığınız öneririz [Site Recovery SAN çoğaltması ile](site-recovery-vmm-san.md) için bir Hyper-V ortamına veya [VMware VMware/fiziksel sunucuya](site-recovery-vmware-to-vmware.md) VMware ortamı için.
* DTC olmayan uygulamalar için yerel yüksek güvenilirlik DB yansıtma yararlanarak bir tek başına sunucu olarak kümeyi kurtarmak için yukarıdaki yaklaşımı kullanın.

### <a name="on-premises-to-azure"></a>Şirket içinden Azure'a

Site Recovery, Konuk sunmaz Azure'a çoğaltırken küme desteği. SQL Server, Standard edition için düşük maliyetli olağanüstü durum kurtarma çözümü de sağlamaz. Bu senaryoda, bir tek başına SQL Server şirket içi SQL Server kümesini koruma ve Azure'da kurtarma öneririz.

1. Ek tek başına SQL Server örneği, şirket içi sitede yapılandırırsınız.
1. Korumak istediğiniz veritabanları için bir yansıtma olarak hizmet verecek bir örneğini yapılandırın. Yüksek güvenilirlik modunda yansıtmasını yapılandırın.
1. Şirket içi sitede, Site Recovery yapılandırma ([Hyper-V](site-recovery-hyper-v-site-to-azure.md) veya [VMware Vm'lerini/fiziksel sunucuları)](site-recovery-vmware-to-azure-classic.md).
1. Yeni SQL Server örneği Azure'a çoğaltmak için Site Recovery çoğaltma kullanın. Yüksek güvenilirlik yansıtma kopyası olduğundan, birincil küme ile eşitlenecektir, ancak Site Recovery çoğaltma kullanarak Azure'a çoğaltılır.


![Standart küme](./media/site-recovery-sql/standalone-cluster-local.png)

### <a name="failback-considerations"></a>Yeniden çalışma hakkında önemli noktalar

SQL Server Standard kümeler için SQL server yedekleme ve geri yükleme, özgün kümeyle reestablishment yansıtma yansıtma örneğine geri dönme planlanmamış bir yük devretme sonrasında gerektirir.

## <a name="next-steps"></a>Sonraki adımlar
[Daha fazla bilgi edinin](site-recovery-components.md) Site Recovery mimarisi hakkında.
