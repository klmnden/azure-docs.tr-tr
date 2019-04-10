---
title: Ortak soruları - Hyper-V için Azure Site Recovery ile Azure'a olağanüstü durum kurtarma | Microsoft Docs
description: Bu makalede, Azure Site Kurtarma'ı kullanarak azure'a şirket içi Hyper-V Vm'leri için olağanüstü durum kurtarma ayarlama hakkında sık sorulan sorular özetlenmektedir.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.date: 04/08/2019
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: ce4a6ab24aaa5ed693f8d64782fb025a2ca9ce30
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59357998"
---
# <a name="common-questions---hyper-v-to-azure-disaster-recovery"></a>Sık sorulan sorular - Hyper-V'den Azure'a olağanüstü durum kurtarma

Bu makalede, şirket içi Hyper-V Vm'lerini Azure'a çoğaltırken görüyoruz yaygın soruların yanıtları sağlanır. 

## <a name="general"></a>Genel

### <a name="how-is-site-recovery-priced"></a>Site Recovery nasıl fiyatlandırılır?
Gözden geçirme [Azure Site Recovery fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/) ayrıntıları.

### <a name="how-do-i-pay-for-azure-vms"></a>Azure Vm'leri için nasıl ödeme yapabilirim?
Çoğaltma sırasında veriler Azure depolama alanına çoğaltılır ve herhangi bir VM değişiklik ödeme yapmayın. Azure'a yük devretme çalıştırdığınızda, Site Recovery, Azure Iaas sanal makineleri otomatik olarak oluşturur. Ardından Azure'da kullandığınız işlem kaynakları için faturalandırılırsınız.

## <a name="azure"></a>Azure

### <a name="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery"></a>Hangi Hyper-V Site Recovery ile çoğaltmayı düzenlemek için ihtiyacım var?

Hyper-V ana bilgisayar sunucusu için sahip olmanız gerekenler dağıtım senaryosuna bağlıdır. Aşağıdaki makalelerden Hyper-V önkoşullarını inceleyin:

* [Hyper-V VM'lerini (VMM olmadan) Azure'a çoğaltma](site-recovery-hyper-v-site-to-azure.md)
* [Hyper-V VM'lerini (VMM ile) Azure'a çoğaltma](site-recovery-vmm-to-azure.md)
* [Hyper-V sanal makinelerini ikincil veri merkezine çoğaltma](site-recovery-vmm-to-vmm.md)
* Hakkında bilgi edinin ikincil veri merkezine çoğaltma yapıyorsanız [Hyper-V Vm'leri için desteklenen konuk işletim sistemleri](https://technet.microsoft.com/library/mt126277.aspx).
* Azure'a çoğaltma yapıyorsanız Site Recovery tüm konuk işletim sistemlerini destekler [Azure tarafından desteklenen](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Bir istemci işletim sisteminde Hyper-V çalışırken Vm'leri koruyabilir miyim?
Hayır, VM'lerin desteklenen bir Windows sunucusu makinesinde çalışan Hyper-V ana bilgisayar sunucusunda bulunması gerekir. Bir istemci bilgisayarı korumak gerekiyorsa, bunu bir fiziksel makineye olarak çoğaltabilirsiniz [Azure](site-recovery-vmware-to-azure.md) veya [ikincil veri merkezine](site-recovery-vmware-to-vmware.md).

### <a name="do-hyper-v-hosts-need-to-be-in-vmm-clouds"></a>Hyper-V konakları VMM bulutlarında olması gerekiyor mu?
Hyper-V Vm'leri üzerinde olmalıdır, ikincil bir veri merkezine çoğaltmak istiyorsanız Hyper-V sunucuları bir VMM bulutunda bulunan barındırır. Azure'a çoğaltmak istiyorsanız, ile veya VMM Bulutları olmadan Vm'lerini çoğaltabilirsiniz. [Daha fazla bilgi edinin](tutorial-hyper-v-to-azure.md) Hyper-V'den azure'a çoğaltma hakkındaki.


### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Hyper-V 2.nesil sanal makinelerini Azure'a çoğaltabilir miyim?
Evet. Site Recovery, 1. kuşak yük devretme sırasında 2. dönüştürür. Yeniden çalışmada, geri kuşak 2 makine dönüştürülür. [Daha fazla bilgi edinin](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).


### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Tek bir VMM sunucusuna sahip olsam da Site Recovery'yi VMM ile dağıtabilir miyim?

Evet. Ya da sanal makinelerini azure'a VMM bulutundaki Hyper-V sunucularında çoğaltabilirsiniz veya aynı sunucudaki VMM bulutlarının arasında çoğaltabilirsiniz. Şirket içi için şirket içi çoğaltma, hem birincil hem de ikincil sitelerde VMM sunucusuna sahip olmasını öneririz. 

### <a name="what-do-i-need-in-azure"></a>Azure'da ne yapmalıyım?
Bir Azure aboneliği, bir kurtarma Hizmetleri kasası, bir depolama hesabı ve sanal ağ gerekir. Kasa, depolama hesabı ve ağ aynı bölgede olması gerekir.

### <a name="what-azure-storage-account-do-i-need"></a>Hangi Azure depolama hesabı ihtiyacım var?
Bir LRS veya GRS depolama hesabı gerekir. Bölgesel bir kesintinin meydana gelmesi veya birincil bölgenin kurtarılamaması durumunda verilerin korunması için GRS'yi tavsiye ederiz. Premium depolama desteklenmiyor.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Hesabımdaki Azure sanal makineler oluşturmak için izinler gerekiyor mu?
Bir abonelik yöneticisi değilseniz, ihtiyaç duyduğunuz çoğaltma izinleri sahip. Değilseniz, bir Azure VM kaynak grubu ve Site Recovery yapılandırırken belirttiğiniz sanal ağ oluşturmak için izinler ve seçili depolama hesabına yazma izni gerekir. [Daha fazla bilgi edinin](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines).

### <a name="is-replication-data-sent-to-site-recovery"></a>Çoğaltma verilerini Site Recovery hizmetine gönderilir?
Hayır, Site Recovery çoğaltılan verilere müdahale etmez ve sanal makinelerinizde çalışan ne hakkında herhangi bir bilgi yoktur. Çoğaltma verileri Azure depolama ve Hyper-V konakları arasında paylaşılmaz. Site Recovery bu verilere müdahale edemez. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.  

Site Recovery, ISO 27001: 2013, 27018, HIPAA, DPA sertifikalı ve SOC2 ile FedRAMP JAB değerlendirmelerini sürecinde olduğundan.

### <a name="can-we-keep-on-premises-metadata-within-a-geographic-region"></a>Biz şirket içi meta verileri bir coğrafi bölge içinde tutabilir miyim?
Evet. Bir bölgede bir kasası oluşturduğunuzda, tüm meta verilerini o bölgenin coğrafi sınırları içinde kalır Site Recovery tarafından kullanılan emin olun.

### <a name="does-site-recovery-encrypt-replication"></a>Site Recovery çoğaltma işlemini şifreleyebilir mi?
Evet, hem şifreleme-aktarım sırasında ve [azure'da şifreleme](https://docs.microsoft.com/azure/storage/storage-service-encryption) desteklenir.


## <a name="deployment"></a>Dağıtım

### <a name="what-can-i-do-with-hyper-v-to-azure-replication"></a>Hyper-V ile Azure'a çoğaltma için neler yapabilirim?

- **Olağanüstü durum kurtarma**: Tam olağanüstü durum kurtarması da ayarlayabilirsiniz. Bu senaryoda, Azure Depolama'ya şirket içi Hyper-V Vm'lerini çoğaltma:
    - Sanal makinelerini Azure'a çoğaltabilirsiniz. Şirket içi altyapınızı kullanılamıyorsa, Azure'a yük devretme.
    - Yük devretme, çoğaltılan verileri kullanarak Azure Vm'leri oluşturulur. Uygulamaları ve iş yüklerini Azure sanal makinelerinde erişebilirsiniz.
    - Şirket içi veri merkezinizi yeniden kullanılabilir olduğunda, Azure'dan şirket içi sitenize başarısız olabilir.
- **Geçiş**: Şirket içi Hyper-V Vm'lerini Azure depolama alanına geçirmek için Site RECOVERY'yi kullanabilirsiniz. Ardından, şirket içinden Azure'a yük devretme. Yük devretme sonrasında kullanılabilir ve Azure vm'lerinde çalışan iş yükleri ve uygulamalar.


### <a name="what-do-i-need-on-premises"></a>Ne şirket içi gerekiyor?

Bir veya daha fazla Vm'niz olmalıdır bir veya daha fazla tek başına veya kümelenmiş Hyper-V konakları üzerinde çalışır. Ayrıca, System Center Virtual Machine Manager (VMM) tarafından yönetilen konaklarda çalışan sanal makineleri çoğaltabilirsiniz.
- Site Recovery dağıtımı sırasında VMM, çalıştırmıyorsanız, Hyper-V konakları ve kümeleri Hyper-V sitelerinde bir araya toplayın. Her Hyper-V konağında'de Site Recovery aracıları (Azure Site Recovery sağlayıcısı ve kurtarma Hizmetleri Aracısı) yükleyin.
- Vmm'de çoğaltma, Hyper-V konaklarını bir VMM bulutunda yer alıyorsa düzenleyin. Site kurtarma sağlayıcısı VMM sunucusunu ve her Hyper-V konağında kurtarma Hizmetleri aracısını yükleyin. VMM mantıksal/VM ağları ve Azure sanal ağlar eşleme.
- [Daha fazla bilgi edinin](hyper-v-azure-architecture.md) Hyper-V-Azure arası mimari hakkında.

### <a name="can-i-replicate-vms-located-on-a-hyper-v-cluster"></a>Bir Hyper-V kümesinde yer alan VM'ler çoğaltabilirim?

Evet, Site Recovery, kümelenmiş Hyper-V konakları destekler. Aşağıdakilere dikkat edin:

- Kümenin tüm düğümleri aynı kasaya kayıtlı olması gerekir.
- Kümedeki tüm Hyper-V konakları VMM kullanmıyorsanız, aynı Hyper-V sitesine eklenmesi gerekir.
- Her Hyper-V konak kümesinde Azure Site Recovery sağlayıcısı ve kurtarma Hizmetleri aracısını yükleyin ve her konağı bir Hyper-V sitesine ekleyin.
- Hiçbir özel adım kümesinde yapılması gerekir.
- Hyper-V için dağıtım planlayıcısı aracı, aracın çalıştığı ve sanal Makinenin çalıştığı düğümden profil verilerini toplar. Aracı kapalı bir düğümünden tüm veriler toplanamıyor, ancak bu düğüme izler. Düğüm çalışır duruma geldikten sonra Aracı'nı, (VM profili VM listesini bir parçasıdır ve düğüm üzerinde çalışan varsa) VM profili verileri buradan toplamaya başlar.
- Site Recovery Kasası'nda bir Hyper-V konağındaki VM aynı kümedeki farklı bir Hyper-V konağı veya tek başına bir konağa geçerse, VM için çoğaltma etkilenmiş değil. Hyper-V konağı karşılamalıdır [önkoşulları](hyper-v-azure-support-matrix.md#on-premises-servers)ve Site Recovery Kasası'nda yapılandırılmış olması. 


### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Bir istemci işletim sisteminde Hyper-V çalışırken Vm'leri koruyabilir miyim?
Hayır, VM'lerin desteklenen bir Windows sunucusu makinesinde çalışan Hyper-V ana bilgisayar sunucusunda bulunması gerekir. Bir istemci bilgisayarı korumak gereksinim duyarsanız, verebilir [fiziksel bir makineyi çoğaltmak](physical-azure-disaster-recovery.md) azure'a.

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Hyper-V 2.nesil sanal makinelerini Azure'a çoğaltabilir miyim?
Evet. Site Recovery, 1. kuşak yük devretme sırasında 2. dönüştürür. Yeniden çalışmada, geri kuşak 2 makine dönüştürülür.

### <a name="can-i-automate-site-recovery-scenarios-with-an-sdk"></a>Site Recovery senaryoları bir SDK'sı ile otomatik hale getirebilirim?
Evet. Rest API'si, PowerShell veya Azure SDK kullanarak Site Recovery iş akışlarını otomatikleştirebilirsiniz. PowerShell kullanarak Azure'a Hyper-V çoğaltma için desteklenen senaryolar:

- [PowerShell kullanarak VMM Hyper-V çoğaltma](hyper-v-azure-powershell-resource-manager.md)
- [Powershell kullanarak VMM ile Hyper-V çoğaltma](hyper-v-vmm-powershell-resource-manager.md)

## <a name="replication"></a>Çoğaltma

### <a name="where-do-on-premises-vms-replicate-to"></a>Burada şirket içi Vm'leri için çoğaltma?
Verileri Azure depolama alanına çoğaltır. Bir yük devretme çalıştırdığınızda, Site Recovery depolama hesabından Azure Vm'leri otomatik olarak oluşturur.

### <a name="what-apps-can-i-replicate"></a>Hangi uygulamaların çoğaltabilirim?
Herhangi bir uygulamayı veya ile uyumlu bir Hyper-V VM olarak çalışan iş yüklerini çoğaltabilirsiniz [çoğaltma gereksinimlerini](hyper-v-azure-support-matrix.md#replicated-vms). Site kurtarma uygulamayla tutarlı çoğaltma için destek sağlar, böylece uygulamalar üzerinde başarısız oldu ve akıllı bir duruma başarısız oldu. Site Recovery, SharePoint, Exchange, Dynamics, SQL Server ve Active Directory gibi Microsoft uygulamalarıyla tümleşir ve Oracle, SAP, IBM ve Red Hat gibi önde gelen satıcılarla yakın bir tümleştirmede çalışır. İş yükü koruması hakkında [daha fazla bilgi edinin](site-recovery-workload.md).

### <a name="whats-the-replication-process"></a>Çoğaltma işlemi nedir?

1. İlk çoğaltma tetiklendiğinde bir Hyper-V VM anlık görüntüsü alınır.
2. Tamamı Azure'a kopyalanana kadar birer birer çoğaltılır, VM üzerindeki sanal sabit diskleri olan. Bu VM boyutuna bağlı olarak biraz zaman ve ağ bant genişliği. Ağ bant genişliğini artırın öğrenin.
3. İlk çoğaltma devam ederken disk değişiklikleri meydana gelirse, Hyper-V çoğaltma çoğaltma İzleyicisi bu değişiklikleri Hyper-V çoğaltma günlükleri (.hrl) izler. Bu günlük dosyaları disklerle aynı klasörde yer alır. Her diskin ikincil depolamaya gönderilir bir ilişkili .hrl dosyası vardır. İlk çoğaltma sırasında anlık görüntü ve günlük dosyaları disk kaynaklarını kullanır.
4. İlk çoğaltma tamamlandığında, VM anlık görüntüsü silinir.
5. Günlükteki herhangi bir disk değişiklikleri eşitlenir ve üst diske birleştirilir.
6. İlk çoğaltma sonlandırıldıktan sonra sanal makine işinde Finalize koruma çalıştırır. Sanal makine korunuyor, ağ ve diğer çoğaltma sonrası ayarlarını yapılandırır.
7. Bu aşamada yük devretme için hazır olduğundan emin olmak için VM ayarlarını denetleyebilirsiniz. Üzerinde beklendiği gibi başarısız olduğunu denetlemek için VM için olağanüstü durum kurtarma tatbikatı (yük devretme testi) çalıştırabilirsiniz.
8. İlk çoğaltmadan sonra değişim çoğaltması, çoğaltma ilkesine uygun olarak başlar.
9. Günlüğe kaydedilen .hrl dosyası değişikliklerdir. Çoğaltma için yapılandırılmış her diskin ilişkili bir .hrl dosyası vardır.
10. Günlük müşterinin depolama hesabına gönderilir. Bir günlük azure'a aktarım olduğunda, birincil diskteki değişiklikler aynı klasörde yer alan başka bir günlük dosyasında izlenir.
11. Hem ilk hem de delta çoğaltma sırasında Azure portalında VM'yi izleyebilirsiniz.

[Daha fazla bilgi edinin](hyper-v-azure-architecture.md#replication-process) çoğaltma işlemi hakkında.

### <a name="can-i-replicate-to-azure-with-a-site-to-site-vpn"></a>Siteden siteye VPN ile azure'a çoğaltabilir miyim?

Site kurtarma verileri, şirket içinden genel bir uç nokta veya ExpressRoute genel eşlemesi kullanarak Azure depolama alanına çoğaltır. Siteden siteye VPN ağ üzerinden çoğaltma desteklenmez.

### <a name="can-i-replicate-to-azure-with-expressroute"></a>ExpressRoute kullanarak azure'a çoğaltabilir miyim?

Evet, ExpressRoute Vm'lerini Azure'a çoğaltma için kullanılabilir. Site Recovery genel bir uç nokta bir Azure depolama hesabına veri çoğaltır ve kurmanız gerekecektir [genel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#publicpeering) Site Recovery çoğaltması için. Bir Azure sanal ağı için sanal makineleri yük devretme sonra erişebilirsiniz kullanarak [özel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#privatepeering).


### <a name="why-cant-i-replicate-over-vpn"></a>VPN üzerinden neden çoğaltma yapamaz?

Azure'a çoğalttığınızda, çoğaltma trafiği ortak uç noktalar Azure depolama hesabının ulaşır. Bu nedenle, yalnızca ExpressRoute (genel eşdüzey hizmet sağlama) ile genel internet üzerinden çoğaltma yapabilirsiniz ve VPN çalışmaz. 

### <a name="what-are-the-replicated-vm-requirements"></a>Çoğaltılmış sanal makine gereksinimleri nelerdir?

Çoğaltma için bir Hyper-V VM, desteklenen bir işletim sistemi çalıştırmalıdır. Ayrıca, VM, Azure Vm'leri için gereksinimleri karşılaması gerekir. [Daha fazla bilgi edinin](hyper-v-azure-support-matrix.md#replicated-vms) destek matrisi içinde.

### <a name="how-often-can-i-replicate-to-azure"></a>Azure'a ne sıklıkta çoğaltabilirim?

Hyper-V Vm'lerini her 30 saniye (hariç, premium depolama), 5 dakika veya 15 dakika çoğaltılabilir.

### <a name="can-i-extend-replication"></a>Ben çoğaltma uzatabilir miyim?
Genişletilmiş veya zincir çoğaltma desteklenmez. Bu özelliği isteği [geri bildirim Forumu](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959).

### <a name="can-i-do-an-offline-initial-replication"></a>Çevrimdışı ilk çoğaltma yapabilirim?
Bu özellik desteklenmez. Bu özelliği isteği [geri bildirim Forumu](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-disks"></a>Diskleri çoğaltmanın dışında tutabilirsiniz?
Evet, diskleri çoğaltmadan hariç tutabilirsiniz. 

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>Sanal makineleri dinamik disklerle çoğaltabilir miyim?
Dinamik diskler çoğaltılabilir. İşletim sistemi diskinin bir temel disk olması gerekir.



## <a name="security"></a>Güvenlik

### <a name="what-access-does-site-recovery-need-to-hyper-v-hosts"></a>Hangi erişim Site Recovery, Hyper-V konaklarına gerekiyor mu

Site kurtarma seçeneğini Vm'lerini çoğaltmak için Hyper-V konakları erişmesi gerekir. Site Recovery, Hyper-V konaklarında aşağıdakileri yükler:

- Değil çalıştırıyorsanız, VMM, Azure Site Recovery sağlayıcısı ve kurtarma Hizmetleri aracısını her bir konağa yüklenir.
- VMM çalıştırıyorsanız, Kurtarma Hizmetleri Aracısı'nı her bir konağa yüklenir. Sağlayıcı, VMM sunucusunda çalışır.


### <a name="what-does-site-recovery-install-on-hyper-v-vms"></a>Hangi Site kurtarma Hyper-V Vm'lerinde yüklüyor?

Site Recovery açıkça herhangi Hyper-V sanal makinesi, çoğaltma için etkinleştirilmiş bir yükleme yapmaz.




## <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma


### <a name="how-do-i-fail-over-to-azure"></a>Nasıl miyim Azure'a yük devretme?

Şirket içi Hyper-V Vm'lerinden Azure'a planlanmış veya planlanmamış bir yük devretme çalıştırabilirsiniz.
    - Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır.
    - Birincil sitenizi erişilebilir durumda değilse, planlanmamış yük devretme çalıştırabilirsiniz.
    - Tek bir makine üzerinden yük devredebilir veya kurtarma planları, birden çok makinenin yük devretmelerini düzenlemek üzere oluşturun.
    - Bir yük devretme çalıştırın. Yük devretme ilk aşaması tamamlandıktan sonra oluşturulan kopya Vm'leri azure'da görebilirsiniz. Gerekli olursa VM’ye genel bir IP adresi atayabilirsiniz. Daha sonra kopya Azure VM'SİNDEKİ iş yüküne erişmeye başlamak için yük devretmeyi yürütürsünüz.
   

### <a name="how-do-i-access-azure-vms-after-failover"></a>Yük devretme sonrasında Azure Vm'lerini nasıl erişebilirim?
Yük devretme sonrasında Azure Vm'lerini güvenli bir Internet bağlantısı üzerinden, siteden siteye VPN üzerinden veya Azure ExpressRoute üzerinden erişebilirsiniz. Bağlanmak için etmenizi hazırlamanız gerekir. [Daha fazla bilgi](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>Dayanıklı veriler üzerinde başarısız oldu?
Azure esneklik için tasarlanmıştır. Site kurtarma, ikincil bir Azure veri merkezi, Azure SLA'sı uygun olarak yük devretme için tasarlanmıştır. Yük devretme işlemi gerçekleştiğinde, kasanız için seçtiğiniz aynı coğrafi bölgede kasaları kalır ve biz meta verilerinizi emin olun.

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?
[Yük devretme](site-recovery-failover.md) otomatik değildir. Portaldaki tek tıklamayla yük devretme başlatın veya kullanabileceğiniz [PowerShell](/powershell/module/az.recoveryservices) bir yük devretmeyi tetiklemek için.

### <a name="how-do-i-fail-back"></a>Nasıl miyim geri hata veriyor?

Şirket içi altyapınızı yeniden çalışır duruma geldikten sonra geri dönebilirsiniz. Yeniden çalışma üç aşamada gerçekleşir:

1. Azure planlı yük devretme birkaç farklı seçenek kullanarak şirket içi siteye başlatabilir:

    - Kapalı kalma süresini en aza indirir: Bu seçeneği kullanırsanız, Site Recovery yük devretmeden önce verileri eşitler. Bu işlem için değiştirilen veri bloklarını denetler ve şirket içi sitesine, Azure VM tutar çalışıyor, kapalı kalma süresini en aza indirir. El ile yük devretmeyi tamamlamanız gerekir belirttiğinizde, Azure VM'yi kapatın, son delta değişiklikleri kopyalanır ve yük devretme başlatır.
    - Tam indirme: Bu seçenekle yük devretme sırasında veri eşitlenir. Bu seçenek, tüm diskin yükler. Hiçbir sağlama toplamları hesaplanır, ancak daha fazla kapalı kalma süresi yoktur çünkü daha hızlıdır. Azure Vm'lerini çoğaltma için biraz zaman çalıştırıyorsunuz veya şirket içi VM silinmişse bu seçeneği kullanın.

2. Aynı sanal makine veya alternatif bir VM başarısız seçeneğini belirleyebilirsiniz. Zaten yoksa Site Recovery VM oluşturması gerektiğini belirtebilirsiniz.
3. İlk eşitleme tamamlandıktan sonra yük devretmeyi tamamlamak için seçin. Tamamlandıktan sonra şirket içi VM'ye her şeyin beklendiği gibi çalıştığını denetlemek için oturum açabilirsiniz. Azure portalında Azure Vm'lerini durduruldu mu olduğunu görebilirsiniz.
4. Bitirme ve şirket içi sanal makineden yeniden iş yüküne erişmeye başlamak için yük devretmeyi yürütürsünüz.
5. İş yüklerini geri başarısız olduktan sonra yeniden şirket içi Vm'leri Azure'a çoğaltmak için çoğaltmayı tersine çevirme etkinleştirin.

### <a name="can-i-fail-back-to-a-different-location"></a>Ben, farklı bir konuma başarısız olabilir?
Evet, Azure'a yük devretmesi, ilkinin kullanılamıyorsa farklı bir konuma başarısız olabilir. [Daha fazla bilgi edinin](hyper-v-azure-failback.md#failback-to-an-alternate-location-in-hyper-v-environment).
