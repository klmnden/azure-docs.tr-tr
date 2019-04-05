---
title: 'Azure Site Kurtarma: Sık sorulan sorular | Microsoft Docs'
description: Bu makalede, Azure Site Recovery hakkında sık kullanılan sorular açıklanmaktadır.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 3/18/2019
ms.author: raynew
ms.openlocfilehash: 231533f9609a4cf8cc11bedf88aafdfd37d1cb7e
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59050127"
---
# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure Site Recovery: sık sorulan sorular (SSS)
Bu makalede, Azure Site Recovery hakkında sık sorulan sorular özetlenmektedir. 

## <a name="general"></a>Genel
### <a name="what-does-site-recovery-do"></a>Site Recovery ne işe yarar?
Site Recovery bölgeler, şirket içi sanal makineleri ve fiziksel sunucuları azure'a arasında Azure VM çoğaltmayı düzenleyip otomatikleştirmek düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkı sağlayan ve şirket içi makineler için bir İkincil veri merkezi. [Daha fazla bilgi edinin](site-recovery-overview.md).

### <a name="what-can-site-recovery-protect"></a>Hangi Site Recovery koruyabilir miyim?
* **Azure sanal makineleri**: Site Recovery desteklenen bir Azure sanal makinesinde çalışan tüm iş yüklerini çoğaltabilirsiniz.
* **Hyper-V sanal makinelerini**: Site Recovery, Hyper-V sanal makinesinde çalışan tüm iş yüklerini koruyabilir.
* **Fiziksel sunucuları**: Site Recovery, Windows veya Linux çalıştıran fiziksel sunucuları koruyabilir.
* **VMware sanal makinelerini**: Site Recovery, VMware VM içinde çalışan tüm iş yüklerini koruyabilir.



### <a name="can-i-replicate-azure-vms"></a>Azure Vm'leri çoğaltabilirim?
Evet, desteklenen Azure Vm'lerinin Azure bölgeleri arasında çoğaltabilirsiniz. [Daha fazla bilgi edinin](site-recovery-azure-to-azure.md).

### <a name="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery"></a>Hangi Hyper-V Site Recovery ile çoğaltmayı düzenlemek için ihtiyacım var?
Hyper-V ana bilgisayar sunucusu için sahip olmanız gerekenler dağıtım senaryosuna bağlıdır. Aşağıdaki makalelerden Hyper-V önkoşullarını inceleyin:

* [Hyper-V VM'lerini (VMM olmadan) Azure'a çoğaltma](site-recovery-hyper-v-site-to-azure.md)
* [Hyper-V VM'lerini (VMM ile) Azure'a çoğaltma](site-recovery-vmm-to-azure.md)
* [Hyper-V sanal makinelerini ikincil veri merkezine çoğaltma](site-recovery-vmm-to-vmm.md)
* Hakkında bilgi edinin ikincil veri merkezine çoğaltma yapıyorsanız [Hyper-V Vm'leri için desteklenen konuk işletim sistemleri](https://technet.microsoft.com/library/mt126277.aspx).
* Azure'a çoğaltma yapıyorsanız Site Recovery tüm konuk işletim sistemlerini destekler [Azure tarafından desteklenen](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Bir istemci işletim sisteminde Hyper-V çalışırken Vm'leri koruyabilir miyim?
Hayır, VM'lerin desteklenen bir Windows sunucusu makinesinde çalışan Hyper-V ana bilgisayar sunucusunda bulunması gerekir. Bir istemci bilgisayarı korumak gerekiyorsa, bunu bir fiziksel makineye olarak çoğaltabilirsiniz [Azure](site-recovery-vmware-to-azure.md) veya [ikincil veri merkezine](site-recovery-vmware-to-vmware.md).

### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Site Recovery ile hangi iş yüklerini koruyabilirim?
Desteklenen VM veya fiziksel sunucu üzerinde çalışan çoğu iş yüklerini korumak için Site RECOVERY'yi kullanabilirsiniz. Böylece uygulamaları için akıllı duruma kurtarılabilir site Recovery uygulamayla tutarlı çoğaltma için destek sağlar. SharePoint, Exchange, Dynamics, SQL Server ve Active Directory gibi Microsoft uygulamalarıyla tümleşir ve Oracle, SAP, IBM ve Red Hat gibi önde gelen satıcılarla yakın bir tümleştirmede çalışır. İş yükü koruması hakkında [daha fazla bilgi edinin](site-recovery-workload.md).

### <a name="do-hyper-v-hosts-need-to-be-in-vmm-clouds"></a>Hyper-V konakları VMM bulutlarında olması gerekiyor mu?
Hyper-V Vm'leri üzerinde olmalıdır, ikincil bir veri merkezine çoğaltmak istiyorsanız Hyper-V sunucuları bir VMM bulutunda bulunan barındırır. Azure'a çoğaltmak istiyorsanız, ile veya VMM Bulutları olmadan Vm'lerini çoğaltabilirsiniz. [Daha fazla bilgi edinin](tutorial-hyper-v-to-azure.md) Hyper-V'den azure'a çoğaltma hakkındaki.

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Tek bir VMM sunucusuna sahip olsam da Site Recovery'yi VMM ile dağıtabilir miyim?

Evet. Ya da sanal makinelerini azure'a VMM bulutundaki Hyper-V sunucularında çoğaltabilirsiniz veya aynı sunucudaki VMM bulutlarının arasında çoğaltabilirsiniz. Şirket içi için şirket içi çoğaltma, hem birincil hem de ikincil sitelerde VMM sunucusuna sahip olmasını öneririz.  

### <a name="what-physical-servers-can-i-protect"></a>Hangi fiziksel sunucuları koruyabilirim?
Fiziksel sunucuları azure'a veya ikincil bir siteye Windows ve Linux çalıştıran çoğaltabilirsiniz. Gereksinimleri hakkında bilgi edinin [azure'a çoğaltma](vmware-physical-azure-support-matrix.md#replicated-machines), ve [ikincil bir siteye çoğaltma](vmware-physical-secondary-support-matrix.md#replicated-vm-support).


Şirket içi sunucunuz arıza yaparsa fiziksel sunucuları azure'da VM olarak çalışacağını unutmayın. Azure'dan şirket içi fiziksel sunucu şu anda desteklenmemektedir. Fiziksel olarak korumalı bir makine için yalnızca bir VMware sanal makinesi için yeniden çalışma gerçekleştirebilirsiniz.

### <a name="what-vmware-vms-can-i-protect"></a>Hangi VMware VM'lerini koruyabilirim?

VMware VM'leri korumak için bir vSphere hiper yöneticisine ve VMware araçlarında çalışan sanal makinelere sahip olmanız gerekir. Ayrıca, hiper yöneticileri yönetmek için bir VMware vCenter sunucusuna sahip olmanızı öneririz. Gereksinimleri hakkında daha fazla bilgi [azure'a çoğaltma](vmware-physical-azure-support-matrix.md#replicated-machines), veya [ikincil bir siteye çoğaltma](vmware-physical-secondary-support-matrix.md#replicated-vm-support).


### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Site Recovery ile şubelerim için olağanüstü durum kurtarma işlemini yönetebilir miyim?
Evet. Şube ofislerinizde çoğaltma ve yük devretme düzenlemek için Site Recovery kullandığınızda, merkezi bir konumda bir birleşik düzenleme ve tüm dal office iş yüklerinizi görünümünü elde edersiniz. Şubelerinizi ziyaret etmenize gerek kalmadan kolaylıkla tüm şubelerinizin olağanüstü durum kurtarma işlemlerini yönetebilir ve yük devretmeler çalıştırabilirsiniz.

## <a name="pricing"></a>Fiyatlandırma
Fiyatlandırma ile ilgili sorular SSS için lütfen bakın [Azure Site Recovery fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/).

## <a name="security"></a>Güvenlik
### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Çoğaltılan veriler Site Recovery hizmetine gönderilir mi?
Hayır, Site Recovery çoğaltılan verilere müdahale etmez ve sanal makineleri veya fiziksel sunucuları üzerinde çalışan ne hakkında herhangi bir bilgi yoktur.
Çoğaltma verileri şirket içi Hyper-V ana bilgisayarları, VMware hiper yöneticileri veya fiziksel sunucular ile Azure depolama alanı ya da ikincil siteniz arasında değiştirilir. Site Recovery bu verilere müdahale edemez. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.  

Site Recovery, ISO 27001: 2013, 27018, HIPAA, DPA sertifikalı ve SOC2 ile FedRAMP JAB değerlendirmelerini sürecinde olduğundan.

### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Uyumluluk nedenleriyle, hatta bizim şirket içi meta verileri aynı coğrafi bölge içinde kalması gerekir. Site Recovery bize yardımcı olabilir?
Evet. Bir bölgede Site Recovery kasası oluşturduğunuzda, etkinleştirmek ve çoğaltmayı düzenlemek ihtiyacımız ve yük devretme bu bölge içinde kalan tüm meta verileri coğrafi olun sınır.

### <a name="does-site-recovery-encrypt-replication"></a>Site Recovery çoğaltma işlemini şifreleyebilir mi?
Şirket içi siteler şifreleme-aktarım sırasında arasında çoğaltılan sanal makineler ve fiziksel sunucular için desteklenir. Sanal makineler ve fiziksel sunucuları Azure'da, hem şifreleme-aktarım sırasında çoğaltma ve [bekleyen şifreleme (azure'da)](https://docs.microsoft.com/azure/storage/storage-service-encryption) desteklenir.

## <a name="replication"></a>Çoğaltma

### <a name="can-i-replicate-over-a-site-to-site-vpn-to-azure"></a>Bir siteden siteye VPN azure'a çoğaltabilir miyim?
Azure Site Recovery, genel bir uç nokta verileri Azure depolama hesabınız ya da yönetilen diskler, çoğaltır. Çoğaltma, siteden siteye VPN üzerinden değildir. Bir Azure sanal ağı ile siteden siteye VPN, oluşturabilirsiniz. Bu, Site Recovery çoğaltma ile engellemez.

### <a name="can-i-use-expressroute-to-replicate-virtual-machines-to-azure"></a>Sanal makinelerini Azure'a çoğaltma için ExpressRoute kullanabilir miyim?
Evet, [ExpressRoute kullanılabilir](concepts-expressroute-with-site-recovery.md) şirket içi sanal makinelerini Azure'a çoğaltma için. Azure Site Recovery, genel bir uç nokta verileri bir Azure depolama alanına çoğaltır. Kurmanız gerekecektir [genel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#publicpeering) veya [Microsoft eşlemesi](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) Site Recovery çoğaltması için ExpressRoute kullanmak için. Microsoft eşlemesi, çoğaltma için önerilen Yönlendirme etki alanıdır. Sanal makineler üzerinde bir Azure sanal ağı için başarısız olduktan sonra bunları erişebilirsiniz kullanarak [özel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#privatepeering) kurulum ile Azure sanal ağı. Çoğaltma özel eşdüzey hizmet sağlama üzerinden desteklenmiyor. VMware makineleri veya fiziksel makineler koruduğunuz durumda olduğundan emin olun [ağ gereksinimleri](vmware-azure-configuration-server-requirements.md#network-requirements) çoğaltma için de karşılandığından. 

### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-to-azure"></a>Sanal makinelerin Azure'a çoğaltılması için herhangi bir önkoşul var mı?
[VMware Vm'lerini](vmware-physical-azure-support-matrix.md#replicated-machines) ve [Hyper-V Vm'lerini](hyper-v-azure-support-matrix.md#replicated-vms) çoğaltmak istediğiniz Azure Azure gereksinimleriyle uyumlu olması gerekir.

Azure kullanıcı hesabınızın belirli olması gerekiyorsa [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) yeni bir sanal makinenin azure'a çoğaltılmasını sağlamak.

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Hyper-V 2.nesil sanal makinelerini Azure'a çoğaltabilir miyim?
Evet. Site Recovery, 1. kuşak yük devretme sırasında 2. dönüştürür. Yeniden çalışmada, geri kuşak 2 makine dönüştürülür. [Daha fazla bilgi edinin](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-to-azure-how-do-i-pay-for-azure-vms"></a>Azure'a çoğaltma işlemi gerçekleştirirken Azure VM'leri için nasıl ödeme yaparım?
Normal çoğaltma sırasında veriler coğrafi olarak yedekli Azure depolama alanına çoğaltılır ve önemli bir avantaj sağlayan herhangi bir Azure Iaas sanal makine ücreti ödeme yapmak gerekmez. Azure'a yük devretme işleminde ise Site Recovery otomatik olarak Azure IaaS sanal makineleri oluşturur ve ardından Azure'da kullandığınız işlem kaynakları için faturalandırılırsınız.

### <a name="can-i-automate-site-recovery-scenarios-with-an-sdk"></a>Site Recovery senaryoları bir SDK'sı ile otomatik hale getirebilirim?
Evet. Rest API'si, PowerShell veya Azure SDK kullanarak Site Recovery iş akışlarını otomatikleştirebilirsiniz. PowerShell kullanarak Site Recovery dağıtımı için desteklenen senaryolar:

* [Azure PowerShell Resource Manager'a VMMs bulutlarındaki Hyper-V Vm'lerini çoğaltma](hyper-v-vmm-powershell-resource-manager.md)
* [Azure PowerShell Resource Manager'a VMM olmadan Hyper-V Vm'lerini çoğaltma](hyper-v-azure-powershell-resource-manager.md)
* [PowerShell Resource Manager ile azure'a VMware çoğaltma](vmware-azure-disaster-recovery-powershell.md)

### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-or-managed-disk-do-i-need"></a>Azure'a çoğaltma, ne tür bir depolama hesabı veya yönetilen disk ihtiyacım var?
Bir LRS veya GRS depolama gerekir. Bölgesel bir kesintinin meydana gelmesi veya birincil bölgenin kurtarılamaması durumunda verilerin korunması için GRS'yi tavsiye ederiz. Hesabın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir. Premium depolama, Azure portalında Site Recovery dağıttığınızda VMware VM, Hyper-V VM ve fiziksel sunucu çoğaltma için desteklenir. Yönetilen diskler yalnızca LRS'yi desteklediğini.

### <a name="how-often-can-i-replicate-data"></a>Verileri ne sıklıkta çoğaltabilirim?
* **Hyper-V:** Hyper-V Vm'lerini her 30 saniye (hariç, premium depolama), 5 dakika veya 15 dakika çoğaltılabilir. SAN çoğaltması ayarladıysanız çoğaltma uyumludur.
* **Azure Vm'leri, VMware ve fiziksel sunucuları:** Bir çoğaltma sıklığı burada geçerli değildir. Çoğaltma sürekli olarak yapılır.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Ben üçüncül başka bir siteye çoğaltma var olan kurtarma sitesinden uzatabilir miyim?
Genişletilmiş veya zincir çoğaltma desteklenmez. Bu özelliği isteği [geri bildirim Forumu](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959).

### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Azure'a ilk kez çoğaltma yaparken çevrimdışı bir çoğaltma gerçekleştirebilir miyim?
Bu özellik desteklenmez. Bu özelliği isteği [geri bildirim Forumu](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-specific-disks-from-replication"></a>Belirli diskleri çoğaltmanın dışında tutabilir miyim?
Bu, VMware Vm'leri ve Hyper-V Vm'lerini Azure portalını kullanarak Azure'a çoğaltırken desteklenir.

### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Sanal makineleri dinamik disklerle çoğaltabilirim?
Dinamik diskler, Hyper-V sanal makineleri çoğaltılırken desteklenir. VMware Vm'lerini ve fiziksel makineleri Azure'a çoğaltırken de desteklenir. İşletim sistemi diskinin bir temel disk olması gerekir.

### <a name="can-i-add-a-new-machine-to-an-existing-replication-group"></a>Mevcut bir çoğaltma grubuna yeni bir makine ekleyebilir miyim?
Mevcut çoğaltma grupları için yeni makineler eklemeye desteklenir. Bunu yapmak için çoğaltma grubundan ('Çoğaltılan öğeler' dikey penceresi) ve çoğaltma grubuna sağ tıklayın seçin bağlam menüsünü seçin ve uygun seçeneği belirleyin.

![Çoğaltma grubuna Ekle](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>Hyper-V çoğaltma trafiği için ayrılan bant genişliğini kısıtlayabilir miyim?
Evet. Daha fazla dağıtım makalelerinde bant genişliği azaltma hakkında:

* [Kapasite VMware Vm'lerini ve fiziksel sunucuları çoğaltmak için planlama](site-recovery-plan-capacity-vmware.md)
* [Kapasite Hyper-V Vm'lerini Azure'a çoğaltma için planlama](site-recovery-capacity-planning-for-hyper-v-replication.md)

## <a name="failover"></a>Yük devretme
### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-virtual-machines-after-failover"></a>Nasıl miyim üzerinden Azure'a geçemiyorum, yük devretme sonrasında Azure sanal makinelerini erişim sağlanır?
Azure VM'lerine güvenli bir İnternet bağlantısı, siteden siteye VPN veya Azure ExpressRoute üzerinden erişebilirsiniz. Bağlanmak için etmenizi hazırlamanız gerekir. [Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Verilerim dayanıklı ise miyim nasıl Azure emin Azure'a yük?
Azure esneklik için tasarlanmıştır. Gerekirse site Kurtarma zaten ikincil bir Azure veri merkezi, Azure SLA'sı uygun olarak yük devretme için tasarlanmıştır. Böyle bir durumda, biz meta verilerinizi emin olun ve Kasaları kasanız için seçtiğiniz aynı coğrafi bölge içinde kalır.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>İki veri merkezi arasında çoğaltma yapıyorsam my birincil veri merkezinde beklenmeyen bir kesinti oluşursa ne olur?
İkincil siteden planlanmamış bir yük devretme tetikleyebilirsiniz. Bu yük devretme işlemini gerçekleştirmek için Site Recovery'nin birincil siteye bağlanması gerekmez.

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?
Yük devretme işlemi otomatik değildir. Portaldaki tek tıklamayla yük devretme başlatın veya kullanabileceğiniz [Site Recovery PowerShell](/powershell/module/az.recoveryservices) bir yük devretmeyi tetiklemek için. İlk duruma döndürmeden, Site Recovery portalında basit bir işlemdir.

Otomatik hale getirmek için, bir sanal makine hatasını algılamak için şirket içi Orchestrator veya Operations Manager'ı kullanın ve ardından SDK'sını kullanarak yük devretmeyi tetiklersiniz.

* [Daha fazla bilgi edinin](site-recovery-create-recovery-plans.md) kurtarma planları hakkında.
* [Daha fazla bilgi edinin](site-recovery-failover.md) yük devretme hakkında.
* [Daha fazla bilgi edinin](site-recovery-failback-azure-to-vmware.md) VMware Vm'lerini ve fiziksel sunucuları döndürme hakkında geri

### <a name="if-my-on-premises-host-is-not-responding-or-crashed-can-i-failover-back-to-a-different-host"></a>My şirket içi konak yanıt veya çöken değilse kullanabilir miyim geri farklı bir konak yük devretme?
Evet, alternatif konuma kurtarma, azure'dan yeniden çalışma için farklı bir konak için kullanabilirsiniz. Seçenekler hakkında daha fazla bilgiyi aşağıdaki VMware ve Hyper-V sanal makineleri için bağlantıları.

* [VMware sanal makinelerini](concepts-types-of-failback.md#alternate-location-recovery-alr)
* [Hyper-V sanal makineleri için](hyper-v-azure-failback.md#perform-failback)

## <a name="service-providers"></a>Hizmet sağlayıcıları
### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Bir hizmet sağlayıcısı ortağıyım. Site Recovery adanmış ve paylaşılan altyapı modelleri için çalışıyor mu?
Evet, Site Recovery hem adanmış hem de paylaşılan altyapı modellerini destekler.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>Bir hizmet sağlayıcısı için kimliğini kiracımı Site Recovery hizmetiyle paylaşılır mı?
Hayır. Kiracı kimliği anonim kalır. Kiracılarınızın Site Recovery portalına erişmesi gerekmez. Yalnızca hizmet sağlayıcı yöneticisi portal ile etkileşim sağlar.

### <a name="will-tenant-application-data-ever-go-to-azure"></a>Şimdiye kadar Kiracı uygulama verileri Azure'a gider mi?
Hizmet sağlayıcının sahip olduğu siteler arasında çoğaltma yapılırken uygulama verileri asla Azure'a gitmez. Aktarım sırasında ve doğrudan hizmet sağlayıcının siteleri arasında çoğaltılan veriler şifrelenir.

Azure'da çoğaltma yaparken, uygulama verileri Azure depolama alanına gönderilir ancak Site Recovery hizmetine gönderilmez. Veriler şifrelenmiş aktarım sırasında ve Azure'da şifrelenmiş olarak kalır.

### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Kiracılarıma herhangi bir Azure hizmeti için fatura gönderilir mi?
Hayır. Azure'ın faturalama bağlantısı doğrudan hizmet sağlayıcı ile kurulur. Hizmet sağlayıcılar, kiracıları için belirli faturaları oluşturmaktan sorumludur.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Azure'a çoğaltma işlemi gerçekleştirirken sanal makinelerin Azure'da sürekli olarak çalışması gerekir mi?
Hayır, veriler, aboneliğinizdeki Azure depolamaya çoğaltılır. Yük devretme testi (DR detayı) veya gerçek bir yük devretme gerçekleştirdiğinizde, Site Recovery otomatik olarak aboneliğinizde sanal makineler oluşturur.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Azure'a çoğaltma yaptığımda kiracı düzeyinde yalıtım sağlanır mı?
Evet.

### <a name="what-platforms-do-you-currently-support"></a>Şu anda hangi platformları destekliyorsanız?
Azure Pack, Cloud Platform System ile destekliyoruz ve System Center (2012 ve üzeri) dağıtımları temel. [Daha fazla bilgi edinin](https://technet.microsoft.com/library/dn850370.aspx) Azure Pack ve Site Recovery tümleştirmesi hakkında.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Tek bir Azure Paketi'ni ve tek bir VMM sunucusu dağıtımını destekliyor musunuz?
Evet, Hyper-V sanal makinelerini azure'a veya hizmet sağlayıcı siteleri arasında çoğaltabilirsiniz.  Hizmet sağlayıcı siteleri arasında çoğaltma yaparsanız, Azure runbook tümleştirmesinin kullanılamayacağını unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Site Recovery'ye genel bakış](site-recovery-overview.md) başlığını okuyun.
* [Site Recovery mimarisi](site-recovery-components.md) hakkında bilgi edinin.  
