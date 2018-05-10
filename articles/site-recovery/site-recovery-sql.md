---
title: SQL Server ve Azure Site Recovery uygulamalarla çoğaltmak | Microsoft Docs
description: Bu makalede, Azure Site Recovery için SQL Server olağanüstü durum özellikleri kullanarak SQL Server çoğaltma açıklar.
services: site-recovery
documentationcenter: ''
author: prateek9us
manager: gauravd
editor: ''
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2018
ms.author: pratshar
ms.openlocfilehash: 7afa05b53186ceac13bef3294c7a139f77193110
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="protect-sql-server-using-sql-server-disaster-recovery-and-azure-site-recovery"></a>SQL Server olağanüstü durum kurtarma ve Azure Site Recovery kullanarak SQL Server koruma

Bu makalede, SQL Server iş devamlılığı ve olağanüstü durum kurtarma (BCDR) teknolojileri kullanan bir uygulama SQL Server arka ucunu korumak açıklar ve [Azure Site Recovery](site-recovery-overview.md).

Başlamadan önce yük devretme kümelemesi, Always On kullanılabilirlik grupları, veritabanı yansıtma ve günlük aktarma dahil olmak üzere, SQL Server olağanüstü durum kurtarma özellikleri anladığınızdan emin olun.


## <a name="sql-server-deployments"></a>SQL Server dağıtımları

Birçok iş yükü, bir temel SQL Server kullanmak ve SharePoint, Dynamics ve SAP, Veri Hizmetleri uygulamak gibi uygulamaları ile tümleştirilebilir.  SQL Server çeşitli şekillerde dağıtılabilir:

* **Tek başına SQL Server**: SQL Server ve tüm veritabanlarını tek bir makinede (fiziksel veya sanal) barındırılır. Sanallaştırılmış kümeleme konak yerel yüksek kullanılabilirlik için kullanılır. Konuk düzeyinde yüksek kullanılabilirlik uygulanmadı.
* **SQL Server Yük Devretme Kümelemesi örnekleri (her zaman üzerinde FCI)**: paylaşılan diskler ile instanced SQL Server çalıştıran iki veya daha fazla düğüm, Windows Yük devretme kümesinde yapılandırılır. Bir düğüm kapalı ise, küme SQL Server üzerinde başka bir örneğine başarısız olabilir. Bu kurulum, genellikle bir birincil sitede yüksek kullanılabilirlik uygulamak için kullanılır. Bu dağıtım hatası veya paylaşılan depolama katmanı kesinti karşı koruma sağlamaz. Paylaşılan bir diskin iSCSI, fiber kanal veya paylaşılan vhdx'i kullanılarak uygulanabilir.
* **SQL Always On kullanılabilirlik grupları**: iki veya daha fazla düğüm ayarlanır paylaşılan hiçbir şey küme, zaman uyumlu çoğaltma ve otomatik yük devretme ile bir kullanılabilirlik grubunda yapılandırılmış SQL Server veritabanları ile.

 Bu makalede, uzak bir site veritabanlarını kurtarmak için aşağıdaki yerel SQL olağanüstü durum kurtarma teknolojileri yararlanır:

* SQL Always On kullanılabilirlik olağanüstü durum kurtarma için SQL Server 2012 veya 2014 Enterprise sürümleri sağlamak için grupları.
* SQL veritabanı yüksek güvenilirlik modunda, SQL Server 2008 R2 veya SQL Server Standard edition (tüm sürümler) için yansıtma.

## <a name="site-recovery-support"></a>Site kurtarma desteği

### <a name="supported-scenarios"></a>Desteklenen senaryolar
Site Recovery, SQL Server tabloda özetlendiği gibi koruyabilirsiniz.

**Senaryo** | **İkincil bir siteye** | **Azure’a**
--- | --- | ---
**Hyper-V** | Evet | Evet
**VMware** | Evet | Evet
**Fiziksel sunucu** | Evet | Evet
**Azure**|NA| Evet

### <a name="supported-sql-server-versions"></a>Desteklenen SQL Server sürümleri
Bu SQL Server sürümleri için desteklenen senaryolar desteklenir:

* SQL Server 2016 Enterprise ve Standard
* SQL Server 2014 Enterprise ve Standard
* SQL Server 2012 Enterprise ve Standard
* SQL Server 2008 R2 Enterprise ve Standard

### <a name="supported-sql-server-integration"></a>Desteklenen SQL Server Integration

Site Recovery, olağanüstü durum kurtarma çözümü sağlamak için tabloda özetlenen yerel SQL Server BCDR teknolojileri ile tümleştirilebilir.

**Özellik** | **Ayrıntılar** | **SQL Server** |
--- | --- | ---
**AlwaysOn kullanılabilirlik grubu** | Tek başına birden çok SQL Server'ın birden fazla düğüme sahip bir yük devretme kümesinde çalıştırın.<br/><br/>Veritabanlarını SQL Server örneklerinde (yansıtılmış) kopyalanabilir ve böylece paylaşılan depolama gerekli yük devretme gruplar halinde gruplandırılabilir.<br/><br/>Bir birincil site ve bir veya daha fazla ikincil siteler arasında olağanüstü durum kurtarma sağlar. İki düğüm bir paylaşılan bir kullanılabilirlik grubuna zaman uyumlu çoğaltma ve otomatik yük devretme kümesi SQL Server veritabanları ile yapılandırılmış hiçbir şey ayarlanabilir. | SQL Server 2016, SQL Server 2014 ve SQL Server 2012 Enterprise edition
**Yük Devretme Kümelemesi (her zaman üzerinde FCI)** | SQL Server yüksek kullanılabilirlik, şirket içi SQL Server iş yükleri için Windows Yük Devretme Kümelemesi yararlanır.<br/><br/>Düğümleri paylaşılan diskler ile SQL Server örneklerini çalıştıran bir yük devretme kümesinde yapılandırılır. Kapalı bir örnek ise küme için farklı bir yöneltilir.<br/><br/>Küme hatası veya paylaşılan depolama alanında kesintilere karşı koruma sağlamaz. Paylaşılan disk iSCSI, fiber kanal uygulanabilir veya paylaşılan Vhdx'ler. | SQL Server Enterprise sürümleri<br/><br/>SQL Server Standard edition (yalnızca iki düğüm için sınırlı)
**Veritabanı yansıtma (yüksek güvenilirlik mod)** | Tek bir ikincil kopya tek bir veritabanına korur. Her iki yüksek güvenilirlik (zaman uyumlu) kullanılabilir ve yüksek performans (zaman uyumsuz) çoğaltma modları. Bir yük devretme kümesi gerektirmez. | SQL Server 2008 R2<br/><br/>SQL Server Enterprise tüm sürümler
**Tek başına SQL Server** | SQL Server ve veritabanı tek bir sunucuda (fiziksel veya sanal) barındırılır. Kümeleme konak sunucunun sanal ise yüksek kullanılabilirlik için kullanılır. Konuk düzeyinde yüksek kullanılabilirliği. | Enterprise veya Standard edition

## <a name="deployment-recommendations"></a>Dağıtım önerileri

Bu tablo, SQL Server BCDR teknolojileri Site Recovery ile tümleştirmek için bizim önerileri özetler.

| **Sürüm** | **Sürüm** | **Dağıtım** | **Şirket içi için** | **Şirket içi Azure** |
| --- | --- | --- | --- | --- |
| SQL Server 2012 veya 2014 |Enterprise |Yük devretme kümesi örneği |Always On kullanılabilirlik grupları |Always On kullanılabilirlik grupları |
|| Enterprise |Always On kullanılabilirlik grupları için yüksek kullanılabilirlik |Always On kullanılabilirlik grupları |Always On kullanılabilirlik grupları | |
|| Standart |Yük devretme kümesi örneği (FCI) |Site Recovery çoğaltma yerel yansıtma ile |Site Recovery çoğaltma yerel yansıtma ile | |
|| Enterprise veya Standard |Tek Başına |Site Recovery çoğaltma |Site Recovery çoğaltma | |
| SQL Server 2008 R2 veya 2008 |Enterprise veya Standard |Yük devretme kümesi örneği (FCI) |Site Recovery çoğaltma yerel yansıtma ile |Site Recovery çoğaltma yerel yansıtma ile |
|| Enterprise veya Standard |Tek Başına |Site Recovery çoğaltma |Site Recovery çoğaltma | |
| SQL Server (tüm sürümler) |Enterprise veya Standard |Yük devretme kümesi örneği - DTC uygulama |Site Recovery çoğaltma |Desteklenmiyor |

## <a name="deployment-prerequisites"></a>Dağıtım önkoşulları

* Desteklenen bir SQL Server sürümü çalıştıran bir şirket içi SQL Server dağıtımı. Genellikle, ayrıca Active Directory, SQL server için gerekir.
* Senaryo için dağıtmak istediğiniz gereksinimleri. Destek gereksinimleri hakkında daha fazla bilgi edinin [Azure'a çoğaltma için](site-recovery-support-matrix-to-azure.md) ve [şirket içi](site-recovery-support-matrix.md), ve [dağıtımının önkoşulları](site-recovery-prereq.md).
* Azure kurtarma ayarlamak için Çalıştır [Azure Sanal Makine Hazırlık Değerlendirmesi](http://www.microsoft.com/download/details.aspx?id=40898) , SQL Server sanal makinelerde Azure Site Recovery ile uyumlu oldukları emin olmak için aracı.

## <a name="set-up-active-directory"></a>Active Directory'yi ayarlama ayarlayın

İkincil kurtarma sitedeki SQL Server'ın düzgün çalışması için Active Directory'yi ayarlama ayarlayın.

* **Küçük işletme**— tüm site vermesine istiyorsanız, uygulamalar ve şirket içi site için tek bir etki alanı denetleyicisi küçük bir sayı ile etki alanı denetleyicisi için ikincil çoğaltmak için Site Recovery çoğaltma kullandığınız öneririz veri merkezi veya Azure'a.
* **Orta ve büyük kuruluş**— uygulamalar, bir Active Directory ormanı bulunan çok sayıda varsa ve uygulama ya da iş yükü tarafından devralınırsa devretmek istediğiniz, sizin ayarladığınız bir ek etki alanı denetleyicisi ikincil veri merkezinde veya Azure öneririz. Always On kullanılabilirlik grupları uzak bir siteyi kurtarmak için kullanıyorsanız, başka bir ek etki alanı denetleyicisi ikincil sitedeki veya Azure, kurtarılan SQL Server örneği için kullanılacak ayarlamanız önerilir.

Bu makaledeki yönergeleri ikincil konumda bir etki alanı denetleyicisinin kullanılabilir olduğunu varsayın. [Daha fazla bilgi](site-recovery-active-directory.md) Active Directory Site Recovery ile koruma hakkında.


## <a name="integrate-with-sql-server-always-on-for-replication-to-azure"></a>Azure'a çoğaltma için SQL Server Always On ile tümleştirme

İşte yapmanız gerekenler:

1. Komut dosyaları Azure Otomasyon hesabınızda içeri aktarın. Bu yük devretme SQL kullanılabilirlik grubu için komut dosyaları içerir, bir [Resource Manager sanal makine](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAG.ps1) ve [Klasik sanal makine](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAGClassic.ps1).

    [![Azure’a dağıtma](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)


1. ASR SQL FailoverAG ilk grup kurtarma planının bir ön eylem olarak ekleyin.

1. Kullanılabilirlik grupları adını sağlamak için bir Otomasyon değişken oluşturmak için komut dosyasındaki kullanılabilir yönergeleri izleyin.

### <a name="steps-to-do-a-test-failover"></a>Yük devretme testi yapma adımları

SQL Always On yerel yük devretme testi desteklemiyor. Bu nedenle, şunları öneririz:

1. Ayarlanan [Azure Backup](../backup/backup-azure-arm-vms.md) Azure kullanılabilirlik grubu çoğaltması barındıran sanal makinede.

1. Kurtarma planı yük devretme testi tetiklemeden önce sanal makine önceki adımda alınmış yedekten kurtarın.

    ![Azure yedekten geri yükleyin ](./media/site-recovery-sql/restore-from-backup.png)

1. [Bir çekirdeği zorlama](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum#PowerShellProcedure) yedekten geri sanal makinede.

1. Test yük devretme ağda kullanılabilir bir IP için dinleyici IP güncelleştirin.

    ![Dinleyici güncelleştirme IP](./media/site-recovery-sql/update-listener-ip.png)

1. Dinleyici çevrimiçi duruma getirin.

    ![Dinleyici çevrimiçi duruma getirme](./media/site-recovery-sql/bring-listener-online.png)

1. Bir yük dengeleyici ön uç IP havuzu her kullanılabilirlik grubu dinleyicisi için karşılık gelen altında IP oluşturulan ve arka uç havuzundaki eklenen SQL sanal makinesi ile oluşturun.

     ![Yük Dengeleyici - ön uç IP havuzu oluştur ](./media/site-recovery-sql/create-load-balancer1.png)

    ![Yük Dengeleyici - arka uç havuzu oluşturma ](./media/site-recovery-sql/create-load-balancer2.png)

1. Kurtarma planı bir sınama yük devretmesi yapın.

### <a name="steps-to-do-a-failover"></a>Bir yük devretme yapma adımları

Kurtarma planında komut dosyası eklenmiştir ve kurtarma planı yük devretme testi yaparak doğrulanması sonra kurtarma planı yük devretme yapabilirsiniz.


## <a name="integrate-with-sql-server-always-on-for-replication-to-a-secondary-on-premises-site"></a>Bir ikincil şirket içi siteye çoğaltma için SQL Server Always On ile tümleştirme

SQL Server kullanılabilirlik gruplarını yüksek kullanılabilirlik (veya bir FCI) kullanıyorsa, kullanılabilirlik grupları kurtarma sitesinde de kullanmanızı öneririz. Bu dağıtılmış işlemler kullanan olmayan uygulamalar için geçerli olduğunu unutmayın.

1. [Veritabanlarını yapılandırma](https://msdn.microsoft.com/library/hh213078.aspx) kullanılabilirlik gruplarına.
1. İkincil sitede bir sanal ağ oluşturun.
1. Sanal ağ ve birincil site arasında bir siteden siteye VPN bağlantısı ayarlayın.
1. Kurtarma sitesinde bir sanal makine oluşturun ve SQL Server yükleyin.
1. Varolan Always On kullanılabilirlik grupları ve yeni SQL Server VM genişletir. Bu SQL Server örneği bir zaman uyumsuz çoğaltma kopya olarak yapılandırın.
1. Bir kullanılabilirlik grubu dinleyicisi oluşturun veya var olan dinleyiciyi zaman uyumsuz çoğaltma sanal makinesi içerecek şekilde güncelleştirin.
1. Uygulama grubu dinleyicisi kullanılarak ayarlandığından emin olun. Kurulum veritabanı sunucusu adı kullanılarak yukarı ise, böylece yük devretme sonrasında yeniden yapılandırmanız gerekmez dinleyicisi kullanacak şekilde güncelleştirin.

Dağıtılmış işlemler kullanan uygulamaları için Site Recovery ile dağıttığınız olan öneririz [VMware/fiziksel sunucu siteden siteye çoğaltma](site-recovery-vmware-to-vmware.md).

### <a name="recovery-plan-considerations"></a>Kurtarma planı değerlendirmeleri
1. Bu örnek betik, birincil ve ikincil sitelerde VMM kitaplığına ekleyin.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

1. Uygulama için bir kurtarma planı oluşturduğunuzda, bir ön eylem kullanılabilirlik grupları vermesine komut dosyasını çağıran Grup 1 Script adımı ekleyin.

## <a name="protect-a-standalone-sql-server"></a>Tek başına SQL Server koruma

Bu senaryoda, SQL Server makinesinde korumak için Site Recovery çoğaltma kullanmanızı öneririz. Uygulanacak adımlar SQL Server VM veya fiziksel bir sunucu ve olup Azure'a çoğaltmak istediğiniz veya ikincil bir site şirket içi olup olmadığını bağlıdır. Hakkında bilgi edinin [Site kurtarma senaryoları](site-recovery-overview.md).

## <a name="protect-a-sql-server-cluster-standard-editionwindows-server-2008-r2"></a>Bir SQL Server kümesi (standard edition/Windows Server 2008 R2) koruma

SQL Server Standard edition veya SQL Server 2008 R2 çalıştıran bir küme için SQL Server korumak için Site Recovery çoğaltma kullanmanızı öneririz.

### <a name="on-premises-to-on-premises"></a>Şirket içinden şirket içine

* Uygulama dağıtılmış işlemler kullanıyorsa dağıtmanız önerilir [Site Recovery SAN çoğaltması ile](site-recovery-vmm-san.md) bir Hyper-V ortamı için veya [VMware VMware/fiziksel sunucuya](site-recovery-vmware-to-vmware.md) VMware ortamınız için.
* DTC olmayan uygulamalar için yerel yüksek güvenilirlik DB yansıtma yararlanarak bir tek başına sunucu olarak kümeyi kurtarmak için yukarıdaki yaklaşımı kullanın.

### <a name="on-premises-to-azure"></a>Şirket içi Azure

Site Recovery, Konuk sunmaz Azure'a çoğaltırken küme desteği. SQL Server Standard edition için düşük maliyetli olağanüstü durum kurtarma çözümü aynısını sağlamaz. Bu senaryoda, şirket içi SQL Server kümesini tek başına SQL Server için koruma ve Azure'da kurtarma öneririz.

1. Bir ek tek başına SQL Server örneği şirket içi sitesinde yapılandırın.
1. Korumak istediğiniz veritabanları için bir yansıtma olarak hizmet örneği yapılandırın. Yüksek güvenilirlik modunda yansıtma yapılandırın.
1. Şirket içi sitede Site Recovery için yapılandırma ([Hyper-V](site-recovery-hyper-v-site-to-azure.md) veya [VMware VM'ler/fiziksel sunucular)](site-recovery-vmware-to-azure-classic.md).
1. Yeni SQL Server örneğine Azure'a çoğaltmak için Site Recovery çoğaltma kullanın. Yüksek güvenilirlik yansıtma kopyası olduğundan, birincil kümeyle eşitlenir ancak Site Recovery çoğaltma kullanarak Azure'a çoğaltılır.


![Standart küme](./media/site-recovery-sql/standalone-cluster-local.png)

### <a name="failback-considerations"></a>Yeniden çalışma hakkında dikkat edilecek noktalar

SQL Server Standard kümeler için SQL server yedekleme ve geri yükleme, özgün kümeyle reestablishment yansıtma, yansıtma örneğine geri dönme planlanmamış bir yük devretme sonrasında gerektirir.

## <a name="next-steps"></a>Sonraki adımlar
[Daha fazla bilgi edinin](site-recovery-components.md) Site Recovery mimarisi hakkında.
