---
title: Sık sorulan soruları - VMware-Azure Site Recovery ile Azure'a olağanüstü durum kurtarma | Microsoft Docs
description: Azure Site Recovery kullanılarak Azure'da şirket içi VMware vm'lerinin olağanüstü durum kurtarma oluşturduğunuzda bu makalede, sık sorulan sorular özetler.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.date: 03/21/2019
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: cdb8fe5deb71c014f7e0af01d070e5004d8c9994
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58418803"
---
# <a name="common-questions---vmware-to-azure-replication"></a>Sık sorulan sorular - Vmware'den Azure'a çoğaltma

Bu makalede, şirket içi VMware vm'lerinin olağanüstü durum kurtarma Azure'a dağıtırken görüyoruz yaygın soruların yanıtları sağlanır. Bu makaleyi okuduktan sonra sorularınız varsa gönderin [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Genel
### <a name="how-is-site-recovery-priced"></a>Site Recovery nasıl fiyatlandırılır?
Gözden geçirme [Azure Site Recovery fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/) ayrıntıları.

### <a name="how-do-i-pay-for-azure-vms"></a>Azure Vm'leri için nasıl ödeme yapabilirim?
Çoğaltma sırasında veriler Azure depolama alanına çoğaltılır ve herhangi bir VM değişiklik ödeme yapmayın. Azure'a yük devretme çalıştırdığınızda, Site Recovery, Azure Iaas sanal makineleri otomatik olarak oluşturur. Ardından Azure'da kullandığınız işlem kaynakları için faturalandırılırsınız.

### <a name="what-can-i-do-with-vmware-to-azure-replication"></a>VMware ile Azure'a çoğaltma için neler yapabilirim?
- **Olağanüstü durum kurtarma**: Tam olağanüstü durum kurtarması da ayarlayabilirsiniz. Bu senaryoda, şirket içi VMware Vm'lerini Azure depolama alanına çoğaltın. Şirket içi altyapınızı kullanılamıyorsa, daha sonra Azure'a devredebilir. Yük devretme, çoğaltılan verileri kullanarak Azure Vm'leri oluşturulur. Şirket içi veri merkezinizi tekrar kullanılabilir hale gelene kadar uygulamaları ve iş yüklerini Azure vm'lerinde erişebilirsiniz. Ardından, Azure'dan şirket içi sitenize başarısız olabilir.
- **Geçiş**: Şirket içi VMware Vm'leri Azure'a geçirmek için Site RECOVERY'yi kullanabilirsiniz. Bu senaryoda, şirket içi VMware Vm'lerini Azure depolama alanına çoğaltın. Ardından, şirket içinden Azure'a yük devretme. Yük devretme sonrasında kullanılabilir ve Azure vm'lerinde çalışan iş yükleri ve uygulamalar.

## <a name="azure"></a>Azure
### <a name="what-do-i-need-in-azure"></a>Azure'da ne yapmalıyım?
Bir Azure aboneliği, bir kurtarma Hizmetleri kasası, bir önbellek depolama hesabı, yönetilen diskleri ve sanal ağ gerekir. Önbellek depolama hesabı, kasa yönetilen diskleri ve ağ aynı bölgede olması gerekir.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Hesabımdaki Azure sanal makineler oluşturmak için izinler gerekiyor mu?
Bir abonelik yöneticisi değilseniz, ihtiyaç duyduğunuz çoğaltma izinleri sahip. Değilseniz, bir Azure VM kaynak grubu ve Site Recovery ve seçili depolama hesabına yazma izinleri yapılandırın veya yönetilen disk yapılandırmanızı temel alarak belirttiğiniz sanal ağ oluşturmak için izinler gerekir. [Daha fazla bilgi edinin](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines).

### <a name="can-i-use-guest-os-server-license-on-azure"></a>Konuk işletim sistemi sunucu lisansı Azure üzerinde kullanabilir miyim?
Evet, Microsoft Yazılım Güvencesi müşterileri kullanabilir [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) lisanslama maliyetlerini kaydetmek için **Windows Server makineleri** azure'a veya Azure olağanüstü durum kurtarma için kullanılacak geçirilir.

## <a name="pricing"></a>Fiyatlandırma

### <a name="how-are-licensing-charges-handled-during-replication-after-failover"></a>Lisans ücretleri, yük devretme sonrasında çoğaltma sırasında nasıl işlenir?

Lisanslama SSS bölümümüzü edinmek [burada](https://aka.ms/asr_pricing_FAQ) daha fazla bilgi için.

### <a name="how-can-i-calculate-approximate-charges-during-the-use-of-site-recovery"></a>Nasıl miyim yaklaşık ücretleri Site Recovery kullanım sırasında hesaplayabilirsiniz?

Kullanabileceğiniz [fiyatlandırma hesaplayıcısını](https://aka.ms/asr_pricing_calculator) Azure Site RECOVERY'yi kullanırken maliyetlerini tahmin etmek için. Maliyetleri ayrıntılı tahmin için dağıtım Planlayıcısı aracını çalıştırın (https://aka.ms/siterecovery_deployment_planner) ve analiz [maliyet tahmini raporunu](https://aka.ms/asr_DP_costreport).

### <a name="is-there-any-difference-in-cost-when-i-replicate-directly-to-managed-disk"></a>Doğrudan yönetilen diske çoğaltırım olduğunda maliyet herhangi bir fark var mı?

Yönetilen diskler, depolama hesapları biraz farklı ücretlendirilir. Lütfen 100 GiB boyutu kaynak diski için aşağıdaki örneğe bakın. Örneğin, fark depolama maliyetine özeldir. Bu maliyet anlık görüntüler, önbellek depolama ve işlem maliyetini içermez.

* Standart depolama hesabı Vs. HDD standart yönetilen Disk

    - **Azure Site Recovery tarafından sağlanan depolama diskini**: S10
    - **Standart depolama hesabı üzerinde ücret kullanılan birim**: 5 ABD Doları / ay
    - **Standart yönetilen disk üzerinde sağlanan birimin ücret**: 5.89 başına aylık

* Premium depolama hesabı Vs. Premium SSD yönetilen Disk 
    - **Azure Site Recovery tarafından sağlanan depolama diskini**: P10
    - **Premium depolama hesabı ücretlendirilir sağlanan birimde**: 17.92 başına aylık
    - **Premium yönetilen disk üzerinde sağlanan birimin ücret**: 17.92 başına aylık

Daha fazla bilgi [yönetilen disklere yönelik fiyatlandırma ayrıntılarını](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="do-i-incur-additional-charges-for-cache-storage-account-with-managed-disks"></a>Ek ücretler önbellek depolama hesabına yönetilen disklerle ödemem?

Hayır, önbelleği için ek ücrete tabi değildir. Önbellek her zaman VMware-Azure arası mimari parçasıdır. Standart depolama hesabına çoğaltırken, bu önbellek depolama aynı hedef depolama hesabındaki bir parçasıdır.

### <a name="i-have-been-an-azure-site-recovery-user-for-over-a-month-do-i-still-get-the-first-31-days-free-for-every-protected-instance"></a>Bir aydan uzun süredir Azure Site Recovery kullanıcısıyım. Yine de korunan her örnek için ilk 31 gün ücretsiz mi olacak?

Evet, Azure Site Recovery'yi ne kadar süredir kullandığınız önemli değildir. Korunan örneklerden ilk 31 gün boyunca Azure Site Recovery ücreti alınmaz. Örneğin, son 6 ay boyunca 10 örnek koruduysanız ve Azure Site Recovery’ye 11. örneği bağlıyorsanız, 11. örnek için ilk 31 gün boyunca Azure Site Recovery ücreti alınmaz. İlk 10 örnek 31 günden uzun süredir korunduğundan, bunlar için Azure Site Recovery ücreti alınmaya devam edilir.

### <a name="during-the-first-31-days-will-i-incur-any-other-azure-charges"></a>İlk 31 gün boyunca başka herhangi bir Azure hizmeti için ücretlendirilir miyim?

Evet. Korunan bir örnek için ilk 31 gün boyunca Azure Site Recovery ücretsizdir ancak Azure Depolama, depolama işlemleri ve veri aktarımı için ücret alınabilir. Korunan bir sanal makine için de Azure işlem ücretleri alınabilir.

### <a name="what-charges-do-i-incur-while-using-azure-site-recovery"></a>Azure Site Recovery’yi kullanırken ödemem gereken ücretler nelerdir?

Başvurmak bizim [maliyetleri hakkında SSS](https://aka.ms/asr_pricing_FAQ) ayrıntılı bilgi için.

### <a name="is-there-a-cost-associated-to-perform-dr-drillstest-failover"></a>DR Tatbikatları/yük devretme testi gerçekleştirmek için ilişkili bir maliyeti var mı?

DR tatbikatı için ayrı bir ücret ödemeden yoktur. Test yük devretme sonrasında sanal makine oluşturulduktan sonra işlem ücret uygulanmaz.

## <a name="azure-site-recovery-components-upgrade"></a>Azure Site Recovery bileşenlerini yükseltme

### <a name="my-mobility-agentconfiguration-serverprocess-server-version-is-very-old-and-my-upgrade-has-failed-how-should-i-upgrade-to-latest-version"></a>My Mobility Aracısı/yapılandırma sunucusu/işlem sunucusu sürümü çok eski ve benim yükseltme başarısız oldu. En son sürüme nasıl yükseltme?

Azure Site Recovery N-4 destek modeli izler. Başvurmak bizim [destek deyimi](https://aka.ms/asr_support_statement) anlamak çok eski sürümlerinden yükseltme hakkında ayrıntılar için.

### <a name="where-can-i-find-the-release-notesupdate-rollups-of-azure-site-recovery"></a>Azure Site kurtarma sürümünün sürüm notları/güncelleştirme paketlerini nerede bulabilirim?

Başvurmak [belge](https://aka.ms/asr_update_rollups) için sürüm notları bilgileri. Her güncelleştirme dökümü içinde ilgili bileşenlerin yükleme bağlantılarını bulabilirsiniz.

### <a name="how-should-i-upgrade-site-recovery-components-for-on-premises-vmware-or-physical-site-to-azure"></a>Site Recovery bileşenlerini şirket içi VMware veya fiziksel sitesinin azure'a nasıl yükseltmeliyim?

Sağlanan kılavuzumuzu başvuran [burada](https://aka.ms/asr_vmware_upgrades) bileşenlerinizi yükseltmeyi.

## <a name="is-reboot-of-source-machine-mandatory-for-each-upgrade"></a>Kaynak makinenin yeniden başlatılması, her bir yükseltme için zorunlu mu?

Önerilen da, her bir yükseltme için zorunlu değildir. Başvuru [burada](https://aka.ms/asr_vmware_upgrades) yönergeleri temizleyin.

## <a name="on-premises"></a>Şirket içi

### <a name="what-do-i-need-on-premises"></a>Ne şirket içi gerekiyor?

Şirket içi şunlar gerekir:
- Recovery bileşenleri, tek bir VMware VM'de yüklü site.
- Bir VMware altyapısı, en az bir ESXi ana bilgisayar ve bir vCenter sunucusunun kullanılması önerilir.
- Bir veya daha fazla VMware çoğaltılacak VM'ler.

[Daha fazla bilgi edinin](vmware-azure-architecture.md) VMware-Azure arası mimari hakkında.

Şirket içi yapılandırma sunucusu şu şekilde dağıtılabilir:

- Yapılandırma sunucusunu bir VMware VM olarak dağıtma OVA Şablon önceden yüklenmiş bir yapılandırma sunucusu ile kullanmanızı öneririz.
- Herhangi bir nedenle bir şablon kullanamıyorsanız, yapılandırma sunucusunu el ile ayarlayabilirsiniz. [Daha fazla bilgi edinin](physical-azure-disaster-recovery.md#set-up-the-source-environment).



### <a name="where-do-on-premises-vms-replicate-to"></a>Burada şirket içi Vm'leri için çoğaltma?
Verileri Azure depolama alanına çoğaltır. Bir yük devretme çalıştırdığınızda, Site Recovery otomatik olarak Azure Vm'leri depolama hesabından oluşturur veya yönetilen disk yapılandırmanızı temel alarak.

## <a name="replication"></a>Çoğaltma

### <a name="what-applications-can-i-replicate"></a>Hangi uygulamaların çoğaltabilirim?
Herhangi bir uygulamayı veya ile uyumlu bir VMware VM'de çalışan iş yüklerini çoğaltabilirsiniz [çoğaltma gereksinimlerini](vmware-physical-azure-support-matrix.md##replicated-machines). Site kurtarma uygulamayla tutarlı çoğaltma için destek sağlar, böylece uygulamalar üzerinde başarısız oldu ve akıllı bir duruma başarısız oldu. Site Recovery, SharePoint, Exchange, Dynamics, SQL Server ve Active Directory gibi Microsoft uygulamalarıyla tümleşir ve Oracle, SAP, IBM ve Red Hat gibi önde gelen satıcılarla yakın bir tümleştirmede çalışır. İş yükü koruması hakkında [daha fazla bilgi edinin](site-recovery-workload.md).

### <a name="can-i-protect-a-virtual-machine-that-has-docker-disk-configuration"></a>Docker disk yapılandırması olan bir sanal makine koruyabilirim?

Hayır, desteklenmeyen bir senaryo budur.

### <a name="can-i-replicate-to-azure-with-a-site-to-site-vpn"></a>Siteden siteye VPN ile azure'a çoğaltabilir miyim?
Site kurtarma verileri, şirket içinden genel bir uç nokta veya ExpressRoute genel eşlemesi kullanarak Azure depolama alanına çoğaltır. Siteden siteye VPN ağ üzerinden çoğaltma desteklenmez.

### <a name="can-i-replicate-to-azure-with-expressroute"></a>ExpressRoute kullanarak azure'a çoğaltabilir miyim?
Evet, ExpressRoute Vm'lerini Azure'a çoğaltma için kullanılabilir. Site Recovery, veri genel bir uç nokta Azure depolama alanına çoğaltır. Kurmanız gerekecektir [genel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#publicpeering) veya [Microsoft eşlemesi](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) Site Recovery çoğaltması için ExpressRoute kullanmak için. Microsoft eşlemesi, çoğaltma için önerilen Yönlendirme etki alanıdır. Emin [ağ gereksinimleri](vmware-azure-configuration-server-requirements.md#network-requirements) çoğaltma için de karşılandığından. Bir Azure sanal ağı için sanal makineleri yük devretme sonra erişebilirsiniz kullanarak [özel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#privatepeering).

### <a name="how-can-i-change-storage-account-after-machine-is-protected"></a>Makine korunduktan sonra depolama hesabını nasıl değiştirebilirim?

Devre dışı bırakın ve çoğaltma yükseltin veya depolama hesabı türü düşürmek etkinleştirmeniz gerekir.

### <a name="can-i-replicate-to-storage-accounts-for-new-machine"></a>Yeni makine için depolama hesaplarına çoğaltabilir miyim?

Hayır, Mar'19 başlayarak, Azure üzerinde yönetilen disklere portaldan çoğaltma yapabilirsiniz. Yeni bir makine için depolama hesaplarına çoğaltma, yalnızca REST API ve Powershell kullanılabilir. API sürümü 2016-08-10 veya 2018-01-10 depolama hesaplarına çoğaltma için kullanın.

### <a name="what-are-the-benefits-in-replicating-to-managed-disks"></a>Yönetilen disklere çoğaltma içinde yararları nelerdir?

İlgili makaleyi okuyun [Azure Site Recovery, yönetilen diskler ile olağanüstü durum kurtarma basitleştirir](https://azure.microsoft.com/blog/simplify-disaster-recovery-with-managed-disks-for-vmware-and-physical-servers/).

### <a name="how-can-i-change-managed-disk-type-after-machine-is-protected"></a>Makine korunduktan sonra yönetilen Disk türünü nasıl değiştirebilirim?

Evet, yönetilen disk türünü kolayca değiştirebilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/convert-disk-storage). Ancak, yönetilen disk türünü değiştirdikten sonra Yük devretme test etmeniz veya yük devretme sonrası bu etkinlik oluşturulduğunda, yeni kurtarma noktaları için beklemeniz emin olun.

### <a name="can-i-switch-the-replication-from-managed-disks-to-unmanaged-disks"></a>Yönetilmeyen diskleri yönetilen disklere çoğaltma geçiş yapabilir miyim?

Hayır, geçiş için yönetilmeyen yönetilen desteklenmiyor.

### <a name="why-cant-i-replicate-over-vpn"></a>VPN üzerinden neden çoğaltma yapamaz?

Azure'a çoğalttığınızda, çoğaltma trafiği ortak uç noktalar Azure Depolama'nın ulaştığında, bu nedenle, yalnızca ExpressRoute (genel eşdüzey hizmet sağlama) ile genel internet üzerinden çoğaltma yapabilirsiniz ve VPN çalışmaz.

### <a name="can-i-use-riverbed-steelheads-for-replication"></a>Riverbed SteelHeads çoğaltma için kullanabilir miyim?

İş, Riverbed, Azure Site Recovery ile çalışma hakkında ayrıntılı bir kılavuz sağlar. Lütfen kendi [çözüm Kılavuzu](https://community.riverbed.com/s/article/DOC-4627).

### <a name="what-are-the-replicated-vm-requirements"></a>Çoğaltılmış sanal makine gereksinimleri nelerdir?

Çoğaltma için bir VMware VM, desteklenen bir işletim sistemi çalıştırmalıdır. Ayrıca, VM, Azure Vm'leri için gereksinimleri karşılaması gerekir. [Daha fazla bilgi edinin](vmware-physical-azure-support-matrix.md##replicated-machines) destek matrisi içinde.

### <a name="how-often-can-i-replicate-to-azure"></a>Azure'a ne sıklıkta çoğaltabilirim?
VMware Vm'lerini Azure'a çoğaltırken çoğaltma sürekli olarak yapılır.

### <a name="can-i-retain-the-ip-address-on-failover"></a>Ben, yük devretme IP adresi tutabilir miyim?
Evet, yük devretme IP adresi tutabilirsiniz. 'İşlem ve ağ' dikey penceresinde yük devretmeden önce hedef IP adresi belirttiğinizden emin olun. Ayrıca, yük devretme, yeniden çalışma sırasındaki IP çakışmalarını önlemek için zamanında makineleri kapatmayı emin olun.

### <a name="can-i-extend-replication"></a>Ben çoğaltma uzatabilir miyim?
Genişletilmiş veya zincir çoğaltma desteklenmez. Bu özelliği isteği [geri bildirim Forumu](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959).

### <a name="can-i-do-an-offline-initial-replication"></a>Çevrimdışı ilk çoğaltma yapabilirim?
Bu özellik desteklenmez. Bu özelliği isteği [geri bildirim Forumu](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-disks"></a>Diskleri çoğaltmanın dışında tutabilirsiniz?
Evet, diskleri çoğaltmadan hariç tutabilirsiniz.

### <a name="can-i-change-the-target-vm-size-or-vm-type-before-failover"></a>Hedef VM boyutu veya VM türü yük devretmeden önce değiştirebilir miyim?
Evet, işlem ve ağ ayarlarını portalından çoğaltma öğesinin giderek dilediğiniz zaman yük devretmeden önce VM'nin boyutunu ve türünü değiştirebilirsiniz.

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>Sanal makineleri dinamik disklerle çoğaltabilir miyim?
Dinamik diskler çoğaltılabilir. İşletim sistemi diskinin bir temel disk olması gerekir.

### <a name="if-i-use-replication-groups-for-multi-vm-consistency-can-i-add-a-new-vm-to-an-existing-replication-group"></a>Çoklu VM tutarlılığı için çoğaltma gruplarını kullanıyorsanız, var olan bir çoğaltma grubuna yeni bir sanal makine ekleyebilirim?
Evet, bunlar için çoğaltmayı etkinleştirdiğinizde, mevcut bir çoğaltma grubuna yeni VM'ler ekleyebilirsiniz. Çoğaltma başlatılır ve var olan VM'ler için bir çoğaltma grubu oluşturamazsınız sonra mevcut bir çoğaltma grubuna VM ekleyemezsiniz.

### <a name="can-i-modify-vms-that-are-replicating-by-adding-or-resizing-disks"></a>Ekleme veya diskleri yeniden boyutlandırma çoğaltılan VM'ler değiştirebiliyorum?

Azure'a VMware çoğaltma için disk boyutunu değiştirebilirsiniz. Yeni diskler eklemek istiyorsanız disk ekleyin ve VM için korumayı etkinleştirmeniz gerekir.

### <a name="can-i-migrate-on-premises-machines-to-a-new-vcenter-without-impacting-ongoing-replication"></a>Şirket içi makinede yeni bir Vcenter sürmekte olan çoğaltmayı etkilemeden geçişini sağlayabilir miyim?
Hayır, Vcenter veya geçiş değişiklik sürmekte olan çoğaltmayı etkiler. Azure Site Recovery ile yeni Vcenter ayarlama ve makineler için çoğaltma etkinleştirmeniz gerekir.

### <a name="can-i-replicate-to-cachetarget-storage-account-which-has-a-vnet-with-azure-storage-firewalls-configured-on-it"></a>Yapılandırılmış bir Vnet'e (Azure depolama güvenlik duvarları) sahip bir önbellek/hedef depolama hesabına çoğaltabilir miyim?
Hayır, Azure Site Recovery, Vnet üzerinde depolama çoğaltmayı desteklemez.

## <a name="configuration-server"></a>Yapılandırma sunucusu

### <a name="what-does-the-configuration-server-do"></a>Yapılandırma sunucusu ne yapar?
Yapılandırma sunucusu da dahil olmak üzere, Site Recovery bileşenlerini şirket içi çalıştırır:
- Şirket içi ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir yapılandırma sunucusu.
- İşlem sunucusu çoğaltma ağ geçidi davranır. Bu çoğaltma verilerini alıp; Bu, önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir; ve Azure depolama., işlem sunucusu ayrıca Mobility hizmetini şirket içi VMware Vm'lerini otomatik olarak bulunmasını gerçekleştirir ve çoğaltmak istediğiniz Vm'lere yükler gönderir.
- Ana hedef sunucu azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.

[Daha fazla bilgi edinin](vmware-azure-architecture.md) yapılandırma sunucusu bileşenleri ve süreçleri hakkında.

### <a name="where-do-i-set-up-the-configuration-server"></a>Yapılandırma sunucusunu ayarladıktan nereden ayarlayabilirim?
İçin yapılandırma sunucusunu tek yüksek oranda kullanılabilir şirket içi VMware VM ihtiyacınız var.

### <a name="what-are-the-requirements-for-the-configuration-server"></a>Yapılandırma sunucusu için gereksinimler nelerdir?

Gözden geçirme [önkoşulları](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>El ile bir şablonu kullanmak yerine yapılandırma sunucusunu ayarlayabilirim?
OVF şablonu için en son sürümünü kullanmanızı öneririz [yapılandırma sunucusu VM'si oluşturma](vmware-azure-deploy-configuration-server.md). Şunları yapamazsınız herhangi bir nedenle Örneğin, VMware sunucusuna erişimi yoksa, şunları yapabilirsiniz [birleşik kurulum dosyasını indirirsiniz](physical-azure-set-up-source.md) Portal'dan ve bir sanal makine üzerinde çalıştırın.

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>Yapılandırma sunucusu, birden fazla bölgeye çoğaltabilir miyim?
Hayır. Bunu yapmak için her bölgede bir yapılandırma sunucusu ayarlamanız gerekir.

### <a name="can-i-host-a-configuration-server-in-azure"></a>Azure'da bir yapılandırma sunucusu ana bilgisayar?
Olası, şirket içi VMware altyapınızı ve Vm'leri ile iletişim kurmak yapılandırma sunucusunu çalıştıran Azure VM gerekir. Bu gecikme ekleyebilir ve sürmekte olan çoğaltmayı etkilemeden.

### <a name="how-do-i-update-the-configuration-server"></a>Yapılandırma sunucusu nasıl güncelleştirebilirim?
[Hakkında bilgi edinin](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) yapılandırma sunucusu güncelleştiriliyor. En son güncelleştirme bilgileri bulabilirsiniz [Azure güncelleştirmeleri sayfası](https://azure.microsoft.com/updates/?product=site-recovery). Ayrıca doğrudan yapılandırma sunucusunun en son sürümünü indirebilirsiniz [Microsoft Download Center](https://aka.ms/asrconfigurationserver). Sürümünüzü 4 sürüm geçerli sürümden daha eski ise başvurmak bizim [destek deyimi](https://aka.ms/asr_support_statement) yükseltme yönergeleri için.

### <a name="should-i-backup-the-deployed-configuration-server"></a>Ben, dağıtılan yapılandırma sunucusu yedeklemeniz gerekir?
Yapılandırma sunucusunun düzenli zamanlanmış yedeklemeleri almaya öneririz. Başarıyla yeniden çalışma için geri başarısız sanal makine yapılandırma sunucusu veritabanında bulunmalı ve yapılandırma sunucusunun çalıştığından ve bağlı durumda olması gerekir. Genel yapılandırma sunucusu yönetim görevleri hakkında daha fazla bilgi [burada](vmware-azure-manage-configuration-server.md).

### <a name="when-im-setting-up-the-configuration-server-can-i-download-and-install-mysql-manually"></a>Yapılandırma sunucusu ayarlama, ı indirebilir ve MySQL el ile yükleme?

Evet. MySQL indirin ve yerleştirebilir **C:\Temp\ASRSetup** klasör. Daha sonra el ile yükleyin. VM yapılandırma sunucusu ayarlarsınız ve koşullarını kabul edin, MySQL olarak listelenecek **yüklü** içinde **yükleyip**.

### <a name="can-i-avoid-downloading-mysql-but-let-site-recovery-install-it"></a>Miyim MySQL yüklenmesini önlemek ancak Site Recovery yükleme izin?

Evet. MySQL yükleyiciyi indirir ve yerleştirebilir **C:\Temp\ASRSetup** klasör.  Yapılandırma sunucusu VM'SİNİN ayarladığınızda, koşulları kabul edin ve tıklayarak **yükleyip**, eklediğiniz MySQL yüklemek için yükleyici portalını kullanacaksınız.
 
### <a name="can-i-use-the-configuration-server-vm-for-anything-else"></a>Yapılandırma sunucusu sanal makine başka bir şey için kullanabilir miyim?
Hayır, yapılandırma sunucusu için yalnızca VM kullanmanız gerekir. 

### <a name="can-i-clone-a-configuration-server-and-use-it-for-orchestration"></a>Bir yapılandırma Sunucusu'na kopyalayın ve miyim düzenleme işlemi için kullanılmakta?
Hayır, kayıt sorunlarını önlemek için yeni yapılandırma sunucusunu ayarlayın.

### <a name="can-i-change-the-vault-registered-in-the-configuration-server"></a>Yapılandırma sunucusunda kayıtlı kasayı değiştirebilir miyim?
Hayır. Yapılandırma sunucusu ile bir kasaya kaydedildikten sonra değiştirilemez. Gözden geçirme [bu makalede](vmware-azure-manage-configuration-server.md#register-a-configuration-server-with-a-different-vault) yeniden kayıt adımları için.

### <a name="can-i-use-the-same-configuration-server-for-disaster-recovery-of-both-vmware-vms-and-physical-servers"></a>Aynı yapılandırma sunucusu VMware Vm'lerini ve fiziksel sunucuları olağanüstü durum kurtarma için kullanabilir miyim
Evet, ancak bu fiziksel makine olması yalnızca olması başarısız bir VMware VM'sine geri unutmayın.

### <a name="where-can-i-download-the-passphrase-for-the-configuration-server"></a>Yapılandırma sunucusu için parola nereden indirebilirim?
[Bu makaleyi gözden geçirin](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) parola indirme hakkında bilgi edinmek için.

### <a name="where-can-i-download-vault-registration-keys"></a>Kasa kayıt anahtarlarının nereden indirebilirim?

İçinde **kurtarma Hizmetleri kasası**, **yönetme** > **Site Recovery altyapısı** > **yapılandırmasunucusu**. İçinde **sunucuları**seçin **indirme kayıt anahtarı** kasa kimlik bilgileri dosyası indirilemedi.



## <a name="mobility-service"></a>Mobility hizmeti

### <a name="where-can-i-find-the-mobility-service-installers"></a>Mobility hizmeti yükleyicileri nerede bulabilirim?
Yükleyicileri tutulur **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository** yapılandırma sunucusundaki klasör.

## <a name="how-do-i-install-the-mobility-service"></a>Mobility hizmeti nasıl yüklerim?
Çoğaltmak istediğiniz her sanal makinede kullanarak yüklediğiniz bir [gönderme yüklemesi](vmware-physical-mobility-service-overview.md#push-installation), veya [el ile yükleme](vmware-physical-mobility-service-overview.md#install-mobility-agent-through-ui) kullanıcı Arabirimi veya Powershell. Alternatif olarak, bir dağıtım aracı gibi kullanarak dağıtabileceğiniz [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md).



## <a name="security"></a>Güvenlik

### <a name="what-access-does-site-recovery-need-to-vmware-servers"></a>Hangi erişim Site Recovery, VMware sunucuları için ihtiyaç duyuyor mu?
Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- Yapılandırma sunucusunu ve diğer şirket içi Site Recovery bileşenlerini çalıştıran bir VMware VM ayarlayın. [Daha fazla bilgi](vmware-azure-deploy-configuration-server.md)
- Otomatik olarak VM için çoğaltma keşfedin. Otomatik bulma için bir hesap hazırlama hakkında bilgi edinin. [Daha fazla bilgi](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery)


### <a name="what-access-does-site-recovery-need-to-vmware-vms"></a>Hangi erişim Site Recovery, VMware Vm'lerinde gerekiyor mu?

- Bir VMware VM çoğaltmak için Site Recovery Mobility hizmeti yüklü ve çalışıyor olması gerekir. Aracı el ile dağıtmak veya bir sanal makine için çoğaltmayı etkinleştirdiğinizde Site Recovery hizmeti göndererek yükleme yapmalısınız belirtin. Gönderim yüklemesi için Site Recovery hizmet bileşeni yüklemek için kullanabileceği bir hesap gerekir. [Daha fazla bilgi edinin](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-mobility-service-installation). (Varsayılan olarak yapılandırma sunucusunda çalışan) işlem sunucusu, bu yükleme gerçekleştirir. [Daha fazla bilgi edinin](vmware-azure-install-mobility-service.md) Mobility hizmeti yüklemesi hakkında.
- Çoğaltma sırasında VM'ler gibi Site Recovery ile iletişim:
    - VM'ler, çoğaltma yönetimi için HTTPS 443 numaralı bağlantı noktasındaki yapılandırma sunucusuyla iletişim kurar.
    - Vm'leri çoğaltma verilerini HTTPS 9443 gelen bağlantı (değiştirilebilir) işlem sunucusu gönderin.
    - Çoklu VM tutarlılığını etkinleştirirseniz, VM'ler birbiriyle 20004 bağlantı noktası üzerinden iletişim kurar.


### <a name="is-replication-data-sent-to-site-recovery"></a>Çoğaltma verilerini Site Recovery hizmetine gönderilir?
Hayır, Site Recovery çoğaltılan verilere müdahale etmez ve sanal makinelerinizde çalışan ne hakkında herhangi bir bilgi yoktur. Çoğaltma verilerini, VMware hiper yöneticileri ve Azure depolama değiştirilir. Site Recovery bu verilere müdahale edemez. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.  

Site Recovery, ISO 27001: 2013, 27018, HIPAA, DPA sertifikalı ve SOC2 ile FedRAMP JAB değerlendirmelerini sürecinde olduğundan.

### <a name="can-we-keep-on-premises-metadata-within-a-geographic-regions"></a>Biz şirket içi meta verileri bir coğrafi bölge içinde tutabilir miyim?
Evet. Bir bölgede bir kasası oluşturduğunuzda, tüm meta verilerini o bölgenin coğrafi sınırları içinde kalır Site Recovery tarafından kullanılan emin olun.

### <a name="does-site-recovery-encrypt-replication"></a>Site Recovery çoğaltma işlemini şifreleyebilir mi?
Evet, hem şifreleme-aktarım sırasında ve [azure'da şifreleme](https://docs.microsoft.com/azure/storage/storage-service-encryption) desteklenir.


## <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma
### <a name="can-i-use-the-process-server-at-on-premises-for-failback"></a>İşlem sunucusu şirket içi yeniden çalışma için kullanabilir miyim?
İşlem sunucusu veri aktarımını gecikmeleri önlemek için Azure'da yeniden çalışma amaçlı oluşturmak için önemle tavsiye edilir. Yapılandırma sunucusu kullanıma yönelik Azure ağı ile kaynak VM ağına ayrılmış durumda ek olarak, ardından bunu Azure'da yeniden çalışma için oluşturulan işlem sunucusu kullanabilmek için gereklidir.

### <a name="how-far-back-can-i-recover"></a>Ne kadar geri kurtarma gerçekleştirebilir miyim?
VMware-Azure arası için kullanabileceğiniz en eski kurtarma noktası değer 72 saattir.

### <a name="how-do-i-access-azure-vms-after-failover"></a>Yük devretme sonrasında Azure Vm'lerini nasıl erişebilirim?
Yük devretme sonrasında Azure Vm'lerini güvenli bir Internet bağlantısı üzerinden, siteden siteye VPN üzerinden veya Azure ExpressRoute üzerinden erişebilirsiniz. Bağlanmak için etmenizi hazırlamanız gerekir. [Daha fazla bilgi](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>Dayanıklı veriler üzerinde başarısız oldu?
Azure esneklik için tasarlanmıştır. Site kurtarma, ikincil bir Azure veri merkezi, Azure SLA'sı uygun olarak yük devretme için tasarlanmıştır. Yük devretme işlemi gerçekleştiğinde, kasanız için seçtiğiniz aynı coğrafi bölgede kasaları kalır ve biz meta verilerinizi emin olun.

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?
[Yük devretme](site-recovery-failover.md) otomatik değildir. Portaldaki tek tıklamayla yük devretme başlatın veya kullanabileceğiniz [PowerShell](/powershell/module/azurerm.siterecovery) bir yük devretmeyi tetiklemek için.

### <a name="can-i-fail-back-to-a-different-location"></a>Ben, farklı bir konuma başarısız olabilir?
Evet, Azure'a yük devretmesi, ilkinin kullanılamıyorsa farklı bir konuma başarısız olabilir. [Daha fazla bilgi edinin](concepts-types-of-failback.md#alternate-location-recovery-alr).

### <a name="why-do-i-need-a-vpn-or-expressroute-to-fail-back"></a>Bir VPN veya ExpressRoute geri başarısız olmasına neden ihtiyacım var?
Azure'dan yeniden çalışma, verileri azure'dan şirket içi Makinenize geri kopyalanır ve özel erişim gereklidir.

### <a name="can-i-resize-the-azure-vm-after-failover"></a>Yük devretmeden sonra Azure VM'nin boyutunu değiştirebilirsiniz?
Hayır, yük devretme sonrasında hedef sanal makine türünü ve boyutunu değiştiremezsiniz.


## <a name="automation-and-scripting"></a>Otomasyon ve betik oluşturma

### <a name="can-i-set-up-replication-with-scripting"></a>Komut dosyası ile çoğaltma ayarlayabilir miyim?
Evet. Rest API, PowerShell veya Azure SDK'sını kullanarak Site Recovery iş akışlarını otomatik hale getirebilirsiniz. [Daha fazla bilgi edinin](vmware-azure-disaster-recovery-powershell.md).

## <a name="performance-and-capacity"></a>Performans ve kapasite
### <a name="can-i-throttle-replication-bandwidth"></a>Çoğaltma bant genişliği kullanımını azaltmak?
Evet. [Daha fazla bilgi edinin](site-recovery-plan-capacity-vmware.md).


## <a name="next-steps"></a>Sonraki adımlar
* [Gözden geçirme](vmware-physical-azure-support-matrix.md) gereksinimlerini destekler.
* [Ayarlanan](vmware-azure-tutorial.md) Vmware'den Azure'a çoğaltma.
