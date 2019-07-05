---
title: 'Azure Site Kurtarma: Sık sorulan sorular | Microsoft Docs'
description: Bu makalede, Azure Site Recovery hakkında sık kullanılan sorular açıklanmaktadır.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 6/27/2019
ms.author: raynew
ms.openlocfilehash: a9c7aa2be945e4fbaa65bdd2a145d576422c5539
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491762"
---
# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure Site Recovery: sık sorulan sorular (SSS)
Bu makalede, Azure Site Recovery hakkında sık sorulan sorular özetlenmektedir.</br>
Farklı ASR üzerindeki özel sorgular için senaryoları senaryo başvurun belirli SSS.<br>

- [Azure VM'LERİNDE olağanüstü durum kurtarma için Azure](azure-to-azure-common-questions.md)
- [Azure'a VMware VM olağanüstü durum kurtarma](vmware-azure-common-questions.md)
- [Azure'a Hyper-V VM'LERİNDE olağanüstü durum kurtarma](hyper-v-azure-common-questions.md)
 
## <a name="general"></a>Genel

### <a name="what-does-site-recovery-do"></a>Site Recovery ne işe yarar?
Site Recovery bölgeler, şirket içi sanal makineleri ve fiziksel sunucuları azure'a arasında Azure VM çoğaltmayı düzenleyip otomatikleştirmek düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkı sağlayan ve şirket içi makineler için bir İkincil veri merkezi. [Daha fazla bilgi edinin](site-recovery-overview.md).

### <a name="can-i-protect-a-virtual-machine-that-has-a-docker-disk"></a>Bir Docker diski olan bir sanal makine koruyabilirim?

Hayır, desteklenmeyen bir senaryo budur.

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

## <a name="pricing"></a>Fiyatlandırma

### <a name="where-can-i-find-pricing-information"></a>Nereden bulabilirim fiyatlandırma bilgileri?
Gözden geçirme [Site Recovery fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/) ayrıntıları.


### <a name="how-can-i-calculate-approximate-charges-during-the-use-of-site-recovery"></a>Nasıl miyim yaklaşık ücretleri Site Recovery kullanım sırasında hesaplayabilirsiniz?

Kullanabileceğiniz [fiyatlandırma hesaplayıcısını](https://aka.ms/asr_pricing_calculator) Site RECOVERY'yi kullanırken maliyetlerini tahmin etmek için.

Maliyetleri ayrıntılı tahmin için dağıtım planlayıcısı aracı çalıştırma [VMware](https://aka.ms/siterecovery_deployment_planner) veya [Hyper-V](https://aka.ms/asr-deployment-planner)ve [maliyet tahmini raporunu](https://aka.ms/asr_DP_costreport).


### <a name="managed-disks-are-now-used-to-replicate-vmware-vms-and-physical-servers-do-i-incur-additional-charges-for-the-cache-storage-account-with-managed-disks"></a>Yönetilen diskler artık VMware Vm'lerini ve fiziksel sunucuları çoğaltmak için kullanılır. Yönetilen diskler ile önbellek depolama hesabı için ek ücret ödemem mi?

Hayır, önbellek için herhangi bir ek ücret yoktur. Standart depolama hesabına çoğaltırken, bu önbellek depolama aynı hedef depolama hesabındaki bir parçasıdır.

### <a name="i-have-been-an-azure-site-recovery-user-for-over-a-month-do-i-still-get-the-first-31-days-free-for-every-protected-instance"></a>Bir aydan uzun süredir Azure Site Recovery kullanıcısıyım. Yine de korunan her örnek için ilk 31 gün ücretsiz mi olacak?

Evet. Korunan örneklerden ilk 31 gün boyunca Azure Site Recovery ücreti alınmaz. Örneğin, son 6 ay boyunca 10 örnek korudunuz ve Azure Site Recovery'ye 11. örneği bağlanmak, ücretsizdir 11. örnek için ilk 31 gün boyunca. İlk 10 örnek 31 günden fazla bir süre için korunan beri Azure Site Recovery ücreti alınmaya devam edilir.

### <a name="during-the-first-31-days-will-i-incur-any-other-azure-charges"></a>İlk 31 gün boyunca başka herhangi bir Azure hizmeti için ücretlendirilir miyim?

Site Recovery korunan bir örnek ilk 31 gün boyunca ücretsiz olsa da, Evet, Azure depolama, depolama işlemleri ve veri aktarımı için ücretlendirilmeye devam edebilirsiniz. Korunan bir sanal makine için de Azure işlem ücretleri alınabilir.


### <a name="is-there-a-cost-associated-to-perform-disaster-recovery-drillstest-failover"></a>Olağanüstü durum kurtarma tatbikatları/yük devretme gerçekleştirmek için ilişkili bir maliyeti var mı?

DR tatbikatı için ayrı bir ücret ödemeden yoktur. Test yük devretme sonrasında VM oluşturulduktan sonra işlem ücret uygulanmaz.



## <a name="security"></a>Güvenlik

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Çoğaltılan veriler Site Recovery hizmetine gönderilir mi?
Hayır, Site Recovery çoğaltılan verilere müdahale etmez ve sanal makineleri veya fiziksel sunucuları üzerinde çalışan ne hakkında herhangi bir bilgi yoktur.
Çoğaltma verileri şirket içi Hyper-V ana bilgisayarları, VMware hiper yöneticileri veya fiziksel sunucular ile Azure depolama alanı ya da ikincil siteniz arasında değiştirilir. Site Recovery bu verilere müdahale edemez. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.  

Site Recovery, ISO 27001: 2013, 27018, HIPAA, DPA sertifikalı ve SOC2 ile FedRAMP JAB değerlendirmelerini sürecinde olduğundan.

### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Uyumluluk nedenleriyle, hatta bizim şirket içi meta verileri aynı coğrafi bölge içinde kalması gerekir. Site Recovery bize yardımcı olabilir?
Evet. Bir bölgede Site Recovery kasası oluşturduğunuzda, etkinleştirmek ve çoğaltmayı düzenlemek ihtiyacımız ve yük devretme bu bölge içinde kalan tüm meta verileri coğrafi olun sınır.

### <a name="does-site-recovery-encrypt-replication"></a>Site Recovery çoğaltma işlemini şifreleyebilir mi?
Şirket içi siteler şifreleme-aktarım sırasında arasında çoğaltılan sanal makineler ve fiziksel sunucular için desteklenir. Sanal makineler ve fiziksel sunucuları Azure'da, hem şifreleme-aktarım sırasında çoğaltma ve [bekleyen şifreleme (azure'da)](https://docs.microsoft.com/azure/storage/storage-service-encryption) desteklenir.




## <a name="disaster-recovery"></a>Olağanüstü durum kurtarma

### <a name="what-can-site-recovery-protect"></a>Hangi Site Recovery koruyabilir miyim?
* **Azure sanal makineleri**: Site Recovery desteklenen bir Azure sanal makinesinde çalışan tüm iş yüklerini çoğaltabilirsiniz.
* **Hyper-V sanal makinelerini**: Site Recovery, Hyper-V sanal makinesinde çalışan tüm iş yüklerini koruyabilir.
* **Fiziksel sunucuları**: Site Recovery, Windows veya Linux çalıştıran fiziksel sunucuları koruyabilir.
* **VMware sanal makinelerini**: Site Recovery, VMware VM içinde çalışan tüm iş yüklerini koruyabilir.

### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Site Recovery ile hangi iş yüklerini koruyabilirim?
Desteklenen VM veya fiziksel sunucu üzerinde çalışan çoğu iş yüklerini korumak için Site RECOVERY'yi kullanabilirsiniz. Böylece uygulamaları için akıllı duruma kurtarılabilir site Recovery uygulamayla tutarlı çoğaltma için destek sağlar. SharePoint, Exchange, Dynamics, SQL Server ve Active Directory gibi Microsoft uygulamalarıyla tümleşir ve Oracle, SAP, IBM ve Red Hat gibi önde gelen satıcılarla yakın bir tümleştirmede çalışır. İş yükü koruması hakkında [daha fazla bilgi edinin](site-recovery-workload.md).

### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Site Recovery ile şubelerim için olağanüstü durum kurtarma işlemini yönetebilir miyim?
Evet. Şube ofislerinizde çoğaltma ve yük devretme düzenlemek için Site Recovery kullandığınızda, merkezi bir konumda bir birleşik düzenleme ve tüm dal office iş yüklerinizi görünümünü elde edersiniz. Şubelerinizi ziyaret etmenize gerek kalmadan kolaylıkla tüm şubelerinizin olağanüstü durum kurtarma işlemlerini yönetebilir ve yük devretmeler çalıştırabilirsiniz.


### <a name="is-disaster-recovery-supported-for-azure-vms"></a>Azure Vm'leri için olağanüstü durum kurtarma desteklenir?

Evet, Site Recovery Azure bölgeleri arasında Azure Vm'leri için olağanüstü durum destekler. [Sık sorulan soruları gözden](azure-to-azure-common-questions.md) Azure VM'LERİNDE olağanüstü durum kurtarma hakkında.

### <a name="is-disaster-recovery-supported-for-vmware-vms"></a>VMware Vm'leri için olağanüstü durum kurtarma desteklenir?

Evet, Site Recovery, şirket içi VMware vm'lerinin olağanüstü durum kurtarma destekler. [Sık sorulan soruları gözden](vmware-azure-common-questions.md) VMware vm'lerinin olağanüstü durum kurtarma için.

### <a name="is-disaster-recovery-supported-for-hyper-v-vms"></a>Hyper-V Vm'leri için olağanüstü durum kurtarma desteklenir?
Evet, Site Recovery, şirket içi Hyper-V Vm'leri olağanüstü durumdan kurtarma destekler. [Sık sorulan soruları gözden](hyper-v-azure-common-questions.md) olağanüstü durum kurtarma Hyper-V sanal makineleri için.

## <a name="is-disaster-recovery-supported-for-physical-servers"></a>Fiziksel sunucular için olağanüstü durum kurtarma desteklenir?
Evet, Site Recovery, şirket içi fiziksel sunucuların azure'a veya ikincil bir siteye Windows ve Linux çalıştıran olağanüstü durum kurtarmayı destekler. Olağanüstü durum kurtarma için gereksinimleri hakkında bilgi edinin [Azure](vmware-physical-azure-support-matrix.md#replicated-machines)ve[ikincil bir siteye](vmware-physical-secondary-support-matrix.md#replicated-vm-support).
Yük devretme işleminden sonra olan fiziksel sunucuları azure'da VM olarak çalışacağını unutmayın. Azure'dan şirket içi fiziksel sunucu şu anda desteklenmemektedir. Yalnızca bir VMware sanal makinesini geri dönebilirsiniz.





## <a name="replication"></a>Çoğaltma

### <a name="can-i-replicate-over-a-site-to-site-vpn-to-azure"></a>Bir siteden siteye VPN azure'a çoğaltabilir miyim?
Azure Site Recovery, genel bir uç nokta verileri Azure depolama hesabınız ya da yönetilen diskler, çoğaltır. Çoğaltma, siteden siteye VPN üzerinden değildir. 

### <a name="why-cant-i-replicate-over-vpn"></a>VPN üzerinden neden çoğaltma yapamaz?

Azure'a çoğalttığınızda, çoğaltma trafiği ortak uç noktalar Azure Depolama'nın ulaşır. Bu nedenle, yalnızca ExpressRoute (Microsoft eşdüzey hizmet sağlama veya bir mevcut genel eşdüzey hizmet sağlama) ile genel internet üzerinden çoğaltma yapabilirsiniz ve VPN çalışmaz.

### <a name="can-i-use-riverbed-steelheads-for-replication"></a>Riverbed SteelHeads çoğaltma için kullanabilir miyim?

İş, Riverbed, Azure Site Recovery ile çalışma hakkında ayrıntılı kılavuz sağlar. Gözden geçirme, [çözüm Kılavuzu](https://community.riverbed.com/s/article/DOC-4627).

### <a name="can-i-use-expressroute-to-replicate-virtual-machines-to-azure"></a>Sanal makinelerini Azure'a çoğaltma için ExpressRoute kullanabilir miyim?
Evet, [ExpressRoute kullanılabilir](concepts-expressroute-with-site-recovery.md) şirket içi sanal makinelerini Azure'a çoğaltma için.

- Azure Site Recovery, genel bir uç nokta verileri bir Azure depolama alanına çoğaltır. Kurmanız gerekecektir [Microsoft eşlemesi](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) veya mevcut bir [genel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#publicpeering) (yeni bağlantı hatları için kullanım dışı) Site Recovery çoğaltması için ExpressRoute kullanmak için.
- Microsoft eşlemesi, çoğaltma için önerilen Yönlendirme etki alanıdır.
- Çoğaltma özel eşdüzey hizmet sağlama üzerinden desteklenmiyor.
- VMware makineleri veya fiziksel makineleri koruyorsanız, emin [ağ gereksinimleri](vmware-azure-configuration-server-requirements.md#network-requirements) için yapılandırma sunucusunu da karşılandığından. Belirli URL'lere bağlantısı yapılandırma sunucusu tarafından Site Recovery çoğaltma düzenleme için gereklidir. ExpressRoute için bağlantı kullanılamaz.
- Sanal makineler üzerinde bir Azure sanal ağı için başarısız olduktan sonra bunları erişebilirsiniz kullanarak [özel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#privatepeering) kurulum ile Azure sanal ağı.


### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-or-managed-disk-do-i-need"></a>Azure'a çoğaltma, ne tür bir depolama hesabı veya yönetilen disk ihtiyacım var?

Bir LRS veya GRS depolama gerekir. Bölgesel bir kesintinin meydana gelmesi veya birincil bölgenin kurtarılamaması durumunda verilerin korunması için GRS'yi tavsiye ederiz. Hesabın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir. Premium depolama, Azure portalında Site Recovery dağıttığınızda VMware VM, Hyper-V VM ve fiziksel sunucu çoğaltma için desteklenir. Yönetilen diskler yalnızca LRS'yi desteklediğini.

### <a name="how-often-can-i-replicate-data"></a>Verileri ne sıklıkta çoğaltabilirim?
* **Hyper-V:** Hyper-V Vm'lerini o 30 saniye (hariç, premium depolama) her beş dakikada çoğaltılabilir
* **Azure Vm'leri, VMware Vm'lerinden, fiziksel sunucuları:** Bir çoğaltma sıklığı burada geçerli değildir. Çoğaltma sürekli olarak yapılır.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Ben üçüncül başka bir siteye çoğaltma var olan kurtarma sitesinden uzatabilir miyim?
Genişletilmiş veya zincir çoğaltma desteklenmez. Bu özelliği isteği [geri bildirim Forumu](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959).

### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Azure'a ilk kez çoğaltma yaparken çevrimdışı bir çoğaltma gerçekleştirebilir miyim?
Bu özellik desteklenmez. Bu özelliği isteği [geri bildirim Forumu](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-specific-disks-from-replication"></a>Belirli diskleri çoğaltmanın dışında tutabilir miyim?
Bu, VMware Vm'leri ve Hyper-V Vm'lerini Azure portalını kullanarak Azure'a çoğaltırken desteklenir.

### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Sanal makineleri dinamik disklerle çoğaltabilirim?
Dinamik diskler, Hyper-V sanal makineleri çoğaltırken ve VMware Vm'leri ve fiziksel makineleri Azure'a çoğaltırken desteklenir. İşletim sistemi diskinin bir temel disk olması gerekir.


### <a name="can-i-throttle-bandwidth-allotted-for-replication-traffic"></a>Çoğaltma trafiği için ayrılan bant genişliği kullanımını azaltmak?
Evet. Daha fazla bilgi Bu makaleler, bant genişliği azaltma hakkında:

* [Kapasite VMware Vm'lerini ve fiziksel sunucuları çoğaltmak için planlama](site-recovery-plan-capacity-vmware.md)
* [Kapasite Hyper-V Vm'lerini Azure'a çoğaltma için planlama](site-recovery-capacity-planning-for-hyper-v-replication.md)



## <a name="failover"></a>Yük devretme
### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-vms-after-failover"></a>Nasıl miyim üzerinden Azure'a geçemiyorum, yük devretme sonrasında Azure Vm'lerini erişim sağlanır?

Azure VM'lerine güvenli bir İnternet bağlantısı, siteden siteye VPN veya Azure ExpressRoute üzerinden erişebilirsiniz. Bağlanmak için etmenizi hazırlamanız gerekir. [Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Verilerim dayanıklı ise miyim nasıl Azure emin Azure'a yük?
Azure esneklik için tasarlanmıştır. Site Recovery, zaten ikincil bir Azure veri merkezi, Azure SLA'sı uygun olarak yük devretme için tasarlanmıştır. Böyle bir durumda, biz meta verilerinizi emin olun ve Kasaları kasanız için seçtiğiniz aynı coğrafi bölge içinde kalır.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>İki veri merkezi arasında çoğaltma yapıyorsam my birincil veri merkezinde beklenmeyen bir kesinti oluşursa ne olur?
İkincil siteden planlanmamış bir yük devretme tetikleyebilirsiniz. Bu yük devretme işlemini gerçekleştirmek için Site Recovery'nin birincil siteye bağlanması gerekmez.

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?
Yük devretme işlemi otomatik değildir. Portaldaki tek tıklamayla yük devretme başlatın veya kullanabileceğiniz [Site Recovery PowerShell](/powershell/module/az.recoveryservices) bir yük devretmeyi tetiklemek için. İlk duruma döndürmeden, Site Recovery portalında basit bir işlemdir.

Otomatik hale getirmek için, bir sanal makine hatasını algılamak için şirket içi Orchestrator veya Operations Manager'ı kullanın ve ardından SDK'sını kullanarak yük devretmeyi tetiklersiniz.

* [Daha fazla bilgi edinin](site-recovery-create-recovery-plans.md) kurtarma planları hakkında.
* [Daha fazla bilgi edinin](site-recovery-failover.md) yük devretme hakkında.
* [Daha fazla bilgi edinin](site-recovery-failback-azure-to-vmware.md) VMware Vm'lerini ve fiziksel sunucuları döndürme hakkında geri

### <a name="if-my-on-premises-host-is-not-responding-or-crashed-can-i-fail-back-to-a-different-host"></a>Farklı bir konak, My şirket içi konak yanıt veya çöken değilse başarısız olabilir?
Evet, alternatif konuma kurtarma, azure'dan yeniden çalışma için farklı bir konak için kullanabilirsiniz.

* [VMware sanal makinelerini](concepts-types-of-failback.md#alternate-location-recovery-alr)
* [Hyper-V sanal makineleri için](hyper-v-azure-failback.md#perform-failback)

## <a name="automation"></a>Otomasyon

### <a name="can-i-automate-site-recovery-scenarios-with-an-sdk"></a>Site Recovery senaryoları bir SDK'sı ile otomatik hale getirebilirim?
Evet. Rest API'si, PowerShell veya Azure SDK kullanarak Site Recovery iş akışlarını otomatikleştirebilirsiniz. PowerShell kullanarak Site Recovery dağıtımı için desteklenen senaryolar:

* [Azure PowerShell Resource Manager'a VMMs bulutlarındaki Hyper-V Vm'lerini çoğaltma](hyper-v-vmm-powershell-resource-manager.md)
* [Azure PowerShell Resource Manager'a VMM olmadan Hyper-V Vm'lerini çoğaltma](hyper-v-azure-powershell-resource-manager.md)
* [PowerShell Resource Manager ile azure'a VMware çoğaltma](vmware-azure-disaster-recovery-powershell.md)

## <a name="componentprovider-upgrade"></a>Bileşen/sağlayıcısı yükseltme

### <a name="where-can-i-find-the-release-notesupdate-rollups-of-site-recovery-upgrades"></a>Site Recovery yükseltme sürüm notları/güncelleştirme paketlerini nerede bulabilirim

[Bilgi](site-recovery-whats-new.md) yeni güncelleştirmeler hakkında ve [toplaması bilgileri alma](service-updates-how-to.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Site Recovery'ye genel bakış](site-recovery-overview.md) başlığını okuyun.

