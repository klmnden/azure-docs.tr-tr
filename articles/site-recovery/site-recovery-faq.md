---
title: "Azure Site Recovery: Sık sorulan sorular | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery hakkında yaygın sorular açıklanır."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5cdc4bcd-b4fe-48c7-8be1-1db39bd9c078
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/19/2017
ms.author: raynew
ms.openlocfilehash: ad6f70cf9c2f420e887031c8b240d2f831e6c359
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure Site Recovery: Sık sorulan sorular (SSS)
Bu makale, Azure Site Recovery hakkında sık sorulan sorular içermektedir. Bu makaleyi okuduktan sonra sorularınız varsa, yayınlayın [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).

## <a name="general"></a>Genel
### <a name="what-does-site-recovery-do"></a>Site Recovery ne işe yarar?
Site Recovery yönetme ve Azure sanal makinelerini çoğaltma bölgeler, şirket içi sanal makineleri ve fiziksel sunucuları azure'a, arasında otomatikleştirme, iş devamlılığı ve olağanüstü durum kurtarma (BCDR) stratejinize katkı ve şirket içi makineler için bir İkincil veri merkezi. [Daha fazla bilgi edinin](site-recovery-overview.md).

### <a name="what-can-site-recovery-protect"></a>Hangi Site Recovery koruyabilir miyim?
* **Azure VM'ler**: Site Recovery, desteklenen bir Azure VM'de çalışan herhangi bir iş yükünü çoğaltabilir
* **Hyper-V sanal makineleri**: Site Recovery, bir Hyper-V VM'de çalışan tüm iş yüklerini koruyabilir.
* **Fiziksel sunucuları**: Site Recovery, Windows veya Linux çalıştıran fiziksel sunucuları koruyabilir.
* **VMware sanal makineleri**: Site Recovery, bir VMware VM içinde çalışan tüm iş yüklerini koruyabilir.



### <a name="can-i-replicate-azure-vms"></a>Azure VM'ler çoğaltabilir miyim?
Evet, desteklenen Azure sanal Azure bölgeler arasında çoğaltabilirsiniz. [Daha fazla bilgi edinin](site-recovery-azure-to-azure.md).

### <a name="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery"></a>Ne Hyper-V Site Recovery ile çoğaltmayı düzenlemek için ihtiyacım var mı?
Hyper-V ana bilgisayar sunucusu için sahip olmanız gerekenler dağıtım senaryosuna bağlıdır. Aşağıdaki makalelerden Hyper-V önkoşullarını inceleyin:

* [Hyper-V Vm'lerini (VMM olmadan) Azure'a çoğaltma](site-recovery-hyper-v-site-to-azure.md)
* [Hyper-V Vm'lerini (VMM ile) Azure'a çoğaltma](site-recovery-vmm-to-azure.md)
* [Hyper-V Vm'lerini ikincil bir veri merkezine çoğaltma](site-recovery-vmm-to-vmm.md)
* Bilgiyi ikincil veri merkezine çoğaltma yapıyorsanız [desteklenen konuk işletim sistemleri için Hyper-V sanal makineleri](https://technet.microsoft.com/library/mt126277.aspx).
* Azure'a çoğaltma yapıyorsanız Site Recovery tüm konuk işletim sistemlerini destekler [Azure tarafından desteklenen](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Hyper-V istemci işletim sisteminde çalışırken Vm'leri koruyabilir miyim?
Hayır, VM'lerin desteklenen bir Windows sunucusu makinesinde çalışan Hyper-V ana bilgisayar sunucusunda bulunması gerekir. Bir istemci bilgisayarı korumanız gerekirse, bu fiziksel bir makine olarak çoğaltabilirsiniz [Azure](site-recovery-vmware-to-azure.md) veya [ikincil veri merkezine](site-recovery-vmware-to-vmware.md).

### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Site Recovery ile hangi iş yüklerini koruyabilirim?
Site Recovery, desteklenen VM veya fiziksel sunucu üzerinde çalışan çoğu iş yüklerini korumak için kullanabilirsiniz. Site Recovery uygulamaya duyarlı çoğaltma için destek sunar, böylece uygulamalar akıllı duruma kurtarılabilir. SharePoint, Exchange, Dynamics, SQL Server ve Active Directory gibi Microsoft uygulamalarıyla tümleşir ve Oracle, SAP, IBM ve Red Hat gibi lider satıcılarla yakın bir tümleştirmede çalışır. İş yükü koruması hakkında [daha fazla bilgi edinin](site-recovery-workload.md).

### <a name="do-hyper-v-hosts-need-to-be-in-vmm-clouds"></a>Hyper-V konakları VMM bulutlarındaki olması gerekiyor mu?
Hyper-V sanal makineleri olmalıdır sonra ikincil bir veri merkezine çoğaltma istiyorsanız, Hyper-V VMM bulutta konumlandırılmış sunucular barındırır. Azure'a çoğaltmak istiyorsanız, VMM Bulutları olmadan veya ile sanal makineleri çoğaltabilirsiniz. [Daha fazla bilgi](tutorial-hyper-v-to-azure.md) Hyper-V çoğaltma Azure için hakkında.

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Tek bir VMM sunucusuna sahip olsam da Site Recovery'yi VMM ile dağıtabilir miyim?

Evet. Sanal ya da azure'a VMM bulutundaki Hyper-V sunucularında çoğaltabilirsiniz veya aynı sunucudaki VMM bulutlarının arasında çoğaltabilirsiniz. Şirket içi için şirket içi çoğaltma, hem birincil ve ikincil siteler VMM sunucusuna sahip olmanızı öneririz.  

### <a name="what-physical-servers-can-i-protect"></a>Hangi fiziksel sunucuları koruyabilirim?
Azure'a veya ikincil bir siteye Windows ve Linux çalıştıran fiziksel sunucuları çoğaltabilirsiniz. [Hakkında bilgi edinin](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) işletim sistemi gereksinimleri.  Fiziksel sunucuların azure'a veya ikincil bir siteye çoğaltma yapıyorsanız olup olmadığını aynı gereksinimleri geçerlidir.


Şirket içi sunucu kullanılamaz hale gelirse fiziksel sunucuları azure'da VM olarak çalışacağını unutmayın. Yeniden çalışma için bir şirket içi fiziksel sunucu şu anda desteklenmiyor. Fiziksel olarak korumalı bir makine için yalnızca VMware sanal makinesi yeniden çalışma olabilir.

### <a name="what-vmware-vms-can-i-protect"></a>Hangi VMware VM'lerini koruyabilirim?

VMware VM'leri korumak için bir vSphere hiper yöneticisine ve VMware araçlarında çalışan sanal makinelere sahip olmanız gerekir. Ayrıca, hiper yöneticileri yönetmek için bir VMware vCenter sunucusuna sahip olmanızı öneririz. [Daha fazla bilgi edinin](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) VMware sunucularını ve Vm'leri azure'a veya ikincil bir siteye çoğaltmak için tam gereksinimleri hakkında.


### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Site Recovery ile şubelerim için olağanüstü durum kurtarma işlemini yönetebilir miyim?
Evet. Çoğaltma ve yük devretme şubelerinizde düzenlemek için Site Recovery kullandığınızda, merkezi bir konumda bir birleşik orchestration ve tüm dal office yükleri görünümünü elde edersiniz. Şubelerinizi ziyaret etmenize gerek kalmadan kolaylıkla tüm şubelerinizin olağanüstü durum kurtarma işlemlerini yönetebilir ve yük devretmeler çalıştırabilirsiniz.

## <a name="pricing"></a>Fiyatlandırma
İle ilgili sorular fiyatlandırma için SSS lütfen [Azure Site Recovery fiyatlandırma](https://azure.microsoft.com/en-in/pricing/details/site-recovery/).

## <a name="security"></a>Güvenlik
### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Çoğaltılan veriler Site Recovery hizmetine gönderilir mi?
Hayır, Site Recovery çoğaltılan verilere müdahale etmez ve hangi sanal makineleri veya fiziksel sunucuları üzerinde çalışan hakkında herhangi bir bilgi yoktur.
Çoğaltma verileri şirket içi Hyper-V ana bilgisayarları, VMware hiper yöneticileri veya fiziksel sunucular ile Azure depolama alanı ya da ikincil siteniz arasında değiştirilir. Site Recovery bu verilere müdahale edemez. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.  

Site Recovery ISO 27001: 2013, 27018, HIPAA, DPA sertifikalı ve SOC2 ile FedRAMP JAB değerlendirmelerini sürecinde olduğundan ' dir.

### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Uyumluluk nedenleriyle bile bizim şirket içi meta verileri aynı coğrafi bölge içinde kalması gerekir. Site Recovery bize yardımcı olabilir?
Evet. Bir bölgede Site Recovery kasası oluşturduğunuzda, etkinleştirmek ve çoğaltmayı düzenlemek ihtiyacımız ve yük devretme kalır, bölge içinde tüm meta veriler coğrafi olun sınır.

### <a name="does-site-recovery-encrypt-replication"></a>Site Recovery çoğaltma işlemini şifreleyebilir mi?
Şirket içi siteler şifreleme-aktarım sırasında arasında çoğaltma sanal makineleri ve fiziksel sunucular için desteklenir. Sanal makineler ve fiziksel sunucuları Azure'a, hem şifreleme-aktarım sırasında çoğaltma ve [rest belirli konumunda şifreleme (azure'da)](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) desteklenir.

## <a name="replication"></a>Çoğaltma

### <a name="can-i-replicate-over-a-site-to-site-vpn-to-azure"></a>Azure için siteden siteye VPN üzerinden çoğaltabilir miyim?
Azure Site Recovery, bir Azure depolama hesabı için genel bir uç nokta verilerini çoğaltır. Çoğaltma, siteden siteye VPN üzerinden değil. Bir Azure sanal ağı ile bir siteden siteye VPN oluşturabilirsiniz. Bu Site Recovery çoğaltma ile engellemez.

### <a name="can-i-use-expressroute-to-replicate-virtual-machines-to-azure"></a>Sanal makineleri Azure'a çoğaltmak için ExpressRoute kullanabilir miyim?
Evet, ExpressRoute sanal makinelerini Azure'a çoğaltma için kullanılabilir. Azure Site Recovery, bir Azure depolama hesabı için genel bir uç nokta verilerini çoğaltır. Ayarlamanız gereken [ortak eşleme](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) ExpressRoute Site Recovery çoğaltma için kullanılacak. Sanal makineler üzerinde bir Azure sanal ağı için başarısız sonra bunları erişebilirsiniz kullanarak [özel eşleme](../expressroute/expressroute-circuit-peerings.md#azure-private-peering) Kurulum Azure sanal ağı ile.

### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-to-azure"></a>Sanal makinelerin Azure'a çoğaltılması için herhangi bir önkoşul var mı?
Azure'a çoğaltmak istediğiniz sanal makineleri ile uyumlu olması gerekir [Azure gereksinimleri](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

Azure kullanıcı hesabınızın belirli sahip olması [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) yeni bir sanal makineye Azure çoğaltmayı etkinleştirmek için.

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Hyper-V 2.nesil sanal makinelerini Azure'a çoğaltabilir miyim?
Evet. Site kurtarma 1. nesil yük devretme sırasında 2 kuşaktan dönüştürür. Yeniden çalışmada geri nesil 2 makine dönüştürülür. [Daha fazla bilgi edinin](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-to-azure-how-do-i-pay-for-azure-vms"></a>Azure'a çoğaltma işlemi gerçekleştirirken Azure VM'leri için nasıl ödeme yaparım?
Normal çoğaltma sırasında veriler coğrafi olarak yedekli Azure depolama alanına çoğaltılır ve önemli bir avantajı sağlayan herhangi bir Azure Iaas sanal makine ücret ödemeniz gerekmez. Azure'a yük devretme işleminde ise Site Recovery otomatik olarak Azure IaaS sanal makineleri oluşturur ve ardından Azure'da kullandığınız işlem kaynakları için faturalandırılırsınız.

### <a name="can-i-automate-site-recovery-scenarios-with-an-sdk"></a>Site kurtarma senaryolarına bir SDK'sı otomatik hale getirebilirsiniz?
Evet. Rest API'si, PowerShell veya Azure SDK kullanarak Site Recovery iş akışlarını otomatikleştirebilirsiniz. Site Recovery PowerShell kullanarak dağıtmak için şu anda desteklenen senaryolar:

* [Azure PowerShell'i Resource Manager VMMs bulutlarındaki Hyper-V Vm'lerini çoğaltma](site-recovery-vmm-to-azure-powershell-resource-manager.md)
* [Azure PowerShell Resource Manager'da VMM olmadan Hyper-V Vm'lerini çoğaltma](site-recovery-deploy-with-powershell-resource-manager.md)

### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-do-i-need"></a>Azure'a çoğaltma yaparsam ne tür bir depolama hesabına sahip olmam gerekir?
Bir LRS veya GRS depolama hesabınızın olması gerekir. Bölgesel bir kesintinin meydana gelmesi veya birincil bölgenin kurtarılamaması durumunda verilerin korunması için GRS'yi tavsiye ederiz. Hesabın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir. Azure portalında Site Recovery dağıttığınızda premium depolama VMware VM, Hyper-V VM ve fiziksel sunucu çoğaltma için desteklenir.

### <a name="how-often-can-i-replicate-data"></a>Verileri ne sıklıkta çoğaltabilirim?
* **Hyper-V:** her (dışında premium depolama) 30 saniye, 5 dakika veya 15 dakika Hyper-V sanal makineleri'nin çoğaltılabilir. SAN çoğaltması ayarladıysanız çoğaltma uyumludur.
* **VMware ve fiziksel sunucular:** Burada çoğaltma sıklığı geçerli değildir. Çoğaltma sürekli ' dir.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Çoğaltma var olan kurtarma sitesinden başka bir üçüncül sitesine genişletebilir miyim?
Genişletilmiş veya zincir çoğaltma desteklenmez. Bu özellik isteği [geri bildirim Forumunda](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).

### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Azure'a ilk kez çoğaltma yaparken çevrimdışı bir çoğaltma gerçekleştirebilir miyim?
Bu özellik desteklenmez. Bu özellik isteği [geri bildirim Forumunda](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-specific-disks-from-replication"></a>Belirli diskleri çoğaltmanın dışında tutabilir miyim?
Bunu olduğunuzda destekleniyorsa [VMware Vm'lerini ve Hyper-V sanal makineleri çoğaltma](site-recovery-exclude-disk.md) Azure için Azure portalını kullanarak.

### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Sanal makineleri dinamik disklerle çoğaltabilir miyim?
Dinamik diskler, Hyper-V sanal makineleri çoğaltılırken desteklenir. VMware Vm'lerini ve fiziksel makineleri Azure'a çoğaltırken de desteklenir. İşletim sistemi diski temel disk olması gerekir.

### <a name="can-i-add-a-new-machine-to-an-existing-replication-group"></a>Yeni bir makine var olan çoğaltma grubuna ekleyebilir miyim?
Mevcut çoğaltma gruplarına yeni makineler eklemeye desteklenir. Bunu yapmak için çoğaltma grubundan ('Çoğaltılan öğeler' dikey) ve çoğaltma grubunu sağ tıklatın/seçmek bağlam menüsünde seçin ve uygun seçeneği belirleyin.

![Çoğaltma grubuna Ekle](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>Hyper-V çoğaltma trafiği için ayrılan bant genişliğini kısıtlayabilir miyim?
Evet. Daha fazla bilgiyi dağıtım makalelerinin bant genişliği azaltma hakkında:

* [Kapasite VMware Vm'lerini ve fiziksel sunucuları çoğaltmak için planlama](site-recovery-plan-capacity-vmware.md)
* [Kapasite Hyper-V Vm'lerini Azure'a çoğaltma için planlama](site-recovery-capacity-planning-for-hyper-v-replication.md)

## <a name="failover"></a>Yük Devretme
### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-virtual-machines-after-failover"></a>I Azure'a devretmek, nasıl Azure sanal makinelerini yük devretme sonrasında erişirim?
Azure VM'lerine güvenli bir İnternet bağlantısı, siteden siteye VPN veya Azure ExpressRoute üzerinden erişebilirsiniz. Bağlanmak için değişik hazırlamanız gerekir. [Daha fazla bilgi](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>T nasıl Azure emin Azure'a yük durumunda verilerim esnek mi?
Azure esneklik için tasarlanmıştır. Gerektiğinde site Kurtarma zaten bir ikincil Azure veri merkezinde Azure SLA uygun olarak bir yük devretme için tasarlanmıştır. Bu durumda, biz meta verileriniz emin olun ve kasanız için seçtiğiniz aynı coğrafi bölge içindeki kasalarını kalır.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>İki veri merkezi arasında çoğaltma yapıyorsam my birincil veri merkezi beklenmeyen bir kesinti oluşursa ne olur?
İkincil siteden planlanmamış bir yük devretme tetikleyebilirsiniz. Bu yük devretme işlemini gerçekleştirmek için Site Recovery'nin birincil siteye bağlanması gerekmez.

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?
Yük devretme işlemi otomatik değildir. Yük devretme işlemlerini portalda tek tıklatmayla başlatmak veya kullanabilirsiniz [Site Recovery PowerShell](/powershell/module/azurerm.siterecovery) bir yük devretme tetiklemek için. Geri başarısız, Site Recovery portalında basit bir işlemdir.

Otomatik hale getirmek için, bir sanal makine hatasını algılamak için şirket içi Orchestrator veya Operations Manager'ı kullanın ve SDK kullanarak yük devretmeyi tetikler.

* [Daha fazla bilgi](site-recovery-create-recovery-plans.md) kurtarma planları hakkında.
* [Daha fazla bilgi](site-recovery-failover.md) yük devretme hakkında.
* [Daha fazla bilgi](site-recovery-failback-azure-to-vmware.md) VMware Vm'lerini ve fiziksel sunucuları hakkında başarısız geri

### <a name="if-my-on-premises-host-is-not-responding-or-crashed-can-i-failover-back-to-a-different-host"></a>My şirket içi konak yanıt vermeyen veya çöken değilse yapabilirim geri yük devretme işlemi farklı bir konak?
Evet, azure'dan farklı bir konağa için alternatif konuma kurtarma kullanabilirsiniz. Seçenekler hakkında daha fazla bilgiyi aşağıdaki VMware ve Hyper-v sanal makineleri için bağlantıları.

* [VMware sanal makineleri için](site-recovery-how-to-failback-azure-to-vmware.md#fail-back-to-the-original-or-alternate-location)
* [Hyper-v sanal makineleri için](site-recovery-failback-from-azure-to-hyper-v.md#failback-to-an-alternate-location)

## <a name="service-providers"></a>Hizmet sağlayıcıları
### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Bir hizmet sağlayıcısı ben. Site Recovery adanmış ve paylaşılan altyapı modelleri için çalışıyor mu?
Evet, Site Recovery hem adanmış hem de paylaşılan altyapı modellerini destekler.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>Bir hizmet sağlayıcısı için Site Recovery hizmetiyle paylaşılır kimliğini mi?
Hayır. Kiracı kimlik anonim kalır. Kiracılarınızın Site Recovery portalına erişmesi gerekmez. Yalnızca hizmet sağlayıcı yöneticisi portal ile etkileşim sağlar.

### <a name="will-tenant-application-data-ever-go-to-azure"></a>Kiracı uygulama verileri Azure'a gider?
Hizmet sağlayıcının sahip olduğu siteler arasında çoğaltma yapılırken uygulama verileri asla Azure'a gitmez. Veriler aktarım sırasında ve doğrudan hizmet sağlayıcı siteleri arasında çoğaltılan şifrelenir.

Azure'da çoğaltma yaparken, uygulama verileri Azure depolama alanına gönderilir ancak Site Recovery hizmetine gönderilmez. Veriler şifrelenmiş aktarım sırasında ve Azure'da şifrelenmiş olarak kalır.

### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Kiracılarıma herhangi bir Azure hizmeti için fatura gönderilir mi?
Hayır. Azure'ın faturalama bağlantısı doğrudan hizmet sağlayıcı ile kurulur. Hizmet sağlayıcılar, kiracıları için belirli faturaları oluşturmaktan sorumludur.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Azure'a çoğaltma işlemi gerçekleştirirken sanal makinelerin Azure'da sürekli olarak çalışması gerekir mi?
Hayır, verileri, aboneliğinizde bir Azure depolama hesabına çoğaltılır. Yük devretme testi (DR detayı) veya gerçek bir yük devretme gerçekleştirdiğinizde, Site Recovery otomatik olarak aboneliğinizde sanal makineler oluşturur.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Azure'a çoğaltma yaptığımda kiracı düzeyinde yalıtım sağlanır mı?
Evet.

### <a name="what-platforms-do-you-currently-support"></a>Şu anda hangi platformları destekliyorsanız?
Azure Pack, Cloud Platform System destekliyoruz ve System Center tabanlı (2012 ve üstü) dağıtımlarının. [Daha fazla bilgi edinin](https://technet.microsoft.com/library/dn850370.aspx) Azure Pack ve Site Recovery tümleştirmesi hakkında.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Tek bir Azure Paketi'ni ve tek bir VMM sunucusu dağıtımını destekliyor musunuz?
Evet, Hyper-V sanal makinelerini azure'a veya hizmet sağlayıcı siteleri arasında çoğaltabilirsiniz.  Hizmet sağlayıcı siteleri arasında çoğaltma yaparsanız Azure runbook tümleştirmesinin kullanılamayacağını unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Site Recovery'ye genel bakış](site-recovery-overview.md) başlığını okuyun.
* [Site Recovery mimarisi](site-recovery-components.md) hakkında bilgi edinin.  
