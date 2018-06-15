---
title: Sık sorulan sorular - Azure Site Recovery ile Azure çoğaltma VMware | Microsoft Docs
description: Azure Site Recovery Azure'u şirket içi VMware sanal makinelerini çoğalttığınızda, bu makalede sık sorulan sorular özetlenmektedir.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/15/2018
ms.author: raynew
ms.openlocfilehash: 345b73db423c6e12b56bb3308f7700917a372dda
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30185229"
---
# <a name="common-questions---vmware-to-azure-replication"></a>Sık sorulan sorular - VMware Azure çoğaltma

Bu makale size şirket içi VMware Vm'lerini Azure'a çoğaltırken bkz: sık sorulan soruların yanıtlarını sağlar. Bu makaleyi okuduktan sonra sorularınız varsa, yayınlayın [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Genel
### <a name="how-is-site-recovery-priced"></a>Site Recovery nasıl fiyatlandırılır?
Gözden geçirme [Azure Site Recovery fiyatlandırma](https://azure.microsoft.com/en-in/pricing/details/site-recovery/) ayrıntıları.

### <a name="how-do-i-pay-for-azure-vms"></a>Azure VM'ler için nasıl ödeme yaparım?
Çoğaltma sırasında veriler Azure depolama alanına çoğaltılır ve herhangi bir VM değişiklik ödemeniz gerekmez. Azure'a yük devretme çalıştırdığınızda, Site Recovery Azure Iaas sanal makineleri otomatik olarak oluşturur. Bundan sonra Azure'da tüketen işlem kaynakları faturalandırılır.

### <a name="what-can-i-do-with-vmware-to-azure-replication"></a>VMware ile Azure çoğaltma ne yapabilirim?
- **Olağanüstü durum kurtarma**: tam olağanüstü durum kurtarma ayarlayabilirsiniz. Bu senaryoda, şirket içi VMware sanal makineleri Azure depolama alanına çoğaltabilir. Şirket içi altyapınızı kullanılamıyorsa, daha sonra üzerinden Azure'a yük devredebilir. Üzerinden başarısız olduğunda, Azure Vm'leri çoğaltılan veriler kullanılarak oluşturulur. Şirket içi veri merkeziniz yeniden kullanılabilir hale gelene kadar uygulamaları ve iş yüklerini Azure vm'lerinde erişebilirsiniz. Ardından, şirket içi sitenize yeniden başarısız olabilir.
- **Geçiş**: şirket içi VMware sanal makinelerini Azure'a geçirmek için Site RECOVERY'yi kullanabilirsiniz. Bu senaryoda, şirket içi VMware sanal makineleri Azure depolama alanına çoğaltabilir. Ardından, şirket içinden Azure'a yük. Yük devretme sonrasında olduğunuz uygulamaları ve iş yüklerini kullanılabilir ve Azure vm'lerinde çalışıyor.



## <a name="azure"></a>Azure
### <a name="what-do-i-need-in-azure"></a>Azure'da ne yapmalıyım?
Bir Azure aboneliği, bir kurtarma Hizmetleri kasası, bir depolama hesabı ve sanal ağ gerekir. Kasa, depolama hesabı ve ağ aynı bölgede olması gerekir.

### <a name="what-azure-storage-account-do-i-need"></a>Hangi Azure depolama hesabı ihtiyacım var mı?
Bir LRS veya GRS depolama hesabınızın olması gerekir. Bölgesel bir kesintinin meydana gelmesi veya birincil bölgenin kurtarılamaması durumunda verilerin korunması için GRS'yi tavsiye ederiz. Premium depolama desteklenir.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Azure Hesabımı VM'ler oluşturmak için izinlere ihtiyaç duyuyor mu?
Bir abonelik yöneticisi değilseniz, gereksinim duyduğunuz çoğaltma izinlerine sahip. Siz değilseniz, kaynak grubu ve sanal ağ sitesi Reocvery yapılandırırken belirttiğiniz bir Azure VM oluşturmak için izinlere ve seçilen depolama hesap yazma izinleri gerekir. [Daha fazla bilgi edinin](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines).



## <a name="on-premises"></a>Şirket içi 

### <a name="what-do-i-need-on-premises"></a>Ne şirket içi gerekir?
Şirket içi Site Recovery bileşenleri, tek bir VMware VM yüklü gerekir. Ayrıca VMware altyapı ile en az bir ESXi ana gerekir ve bir vCenter sunucusu önerilir. Ayrıca, çoğaltmak için bir veya daha fazla VMware Vm'lerini gerekir. [Daha fazla bilgi edinin](vmware-azure-architecture.md) Azure Mimarisi için VMware hakkında.

Şirket içi yapılandırma sunucusu aşağıdaki iki yöntemden birini dağıtılabilir

1. Yapılandırma sunucusu önceden yüklü bir VM şablonu kullanarak dağıtın. [Şuradan daha fazla okuma](vmware-azure-tutorial.md#download-the-vm-template).
2. Tercih ettiğiniz bir Windows Server 2016 makinede Kurulum kullanarak dağıtın. [Şuradan daha fazla okuma](physical-azure-disaster-recovery.md#set-up-the-source-environment).

Başlarken, kendi Windows Server makinelerinizde etkinleştir koruma koruma hedef yapılandırma sunucusunda dağıtma adımları keşfetmek için seçin **için Azure > değil sanallaştırılmış/diğer**.

### <a name="where-do-on-premises-vms-replicate-to"></a>Burada şirket içi Vm'lerini çoğaltma?
Verileri Azure depolama alanına çoğaltır. Bir yük devretme çalıştırdığınızda, Site Recovery Azure VM'ler depolama hesabından otomatik olarak oluşturur.

### <a name="what-apps-can-i-replicate"></a>Hangi uygulamaların çoğaltabilirim?
Herhangi bir uygulama veya ile uyumlu bir VMware VM üzerinde çalışan iş yükünü çoğaltabilir [çoğaltma gereksinimlerini](vmware-physical-azure-support-matrix.md##replicated-machines). Böylece uygulamaları devredilir ve akıllı bir duruma başarısız site Recovery uygulamaya duyarlı çoğaltma için destek sağlar. Site Recovery, SharePoint, Exchange, Dynamics, SQL Server ve Active Directory gibi Microsoft uygulamalarıyla tümleşir ve Oracle, SAP, IBM ve Red Hat gibi lider satıcılarla yakın bir tümleştirmede çalışır. İş yükü koruması hakkında [daha fazla bilgi edinin](site-recovery-workload.md).

### <a name="can-i-replicate-to-azure-with-a-site-to-site-vpn"></a>Siteden siteye VPN ile azure'a çoğaltabilir miyim?
Site kurtarma verileri, şirket içi genel bir uç nokta veya ExpressRoute genel eşliği kullanarak üzerinden Azure depolama alanına çoğaltır. Siteden siteye VPN ağ üzerinden çoğaltma desteklenmez.

### <a name="can-i-replicate-to-azure-with-expressroute"></a>ExpressRoute ile azure'a çoğaltabilir miyim?
Evet, ExpressRoute Vm'lerini Azure'a çoğaltma için kullanılabilir. Site Recovery genel bir uç nokta bir Azure depolama hesabı veri yinelenir ve ayarlamanız gereken [ortak eşleme](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) Site Recovery çoğaltma için. Bir Azure sanal ağı üzerinden VM'ler başarısız olduktan sonra bunlara erişebileceği kullanarak [özel eşleme](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).


### <a name="why-cant-i-replicate-over-vpn"></a>VPN üzerinden neden çoğaltma yapamaz?

Azure'a çoğaltma, çoğaltma trafiği ortak uç noktalar bir Azure depolama hesabının ulaştığında, bu nedenle, yalnızca ExpressRoute (ortak eşleme) ile ortak internet üzerinden çoğaltabilirsiniz ve VPN çalışmıyor. 



## <a name="what-are-the-replicated-vm-requirements"></a>Çoğaltılmış VM gereksinimleri nelerdir?

Çoğaltma için bir VMware VM desteklenen bir işletim sistemi çalıştırması gerekir. Ayrıca, VM Azure VM'ler için gereksinimleri karşılaması gerekir. [Daha fazla bilgi edinin](vmware-physical-azure-support-matrix.md##replicated-machines) destek matrisi içinde.

## <a name="how-often-can-i-replicate-to-azure"></a>Azure'a ne sıklıkta çoğaltabilirim?
Çoğaltma, VMware Vm'lerini Azure'a çoğaltırken sürekli.

## <a name="can-i-extend-replication"></a>Çoğaltma genişletebilir miyim?
Genişletilmiş veya zincir çoğaltma desteklenmez. Bu özellik isteği [geri bildirim Forumunda](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).

## <a name="can-i-do-an-offline-initial-replication"></a>Çevrimdışı ilk çoğaltma yapabilirim?
Bu özellik desteklenmez. Bu özellik isteği [geri bildirim Forumunda](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-disks"></a>Diskleri hariç tutabilirsiniz?
Evet, diskleri çoğaltmanın dışında bırakabilirsiniz. 

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>Sanal makineleri dinamik disklerle çoğaltabilir miyim?
Dinamik diskler çoğaltılabilir. İşletim sistemi diski temel disk olması gerekir.

### <a name="can-i-add-a-new-vm-to-an-existing-replication-group"></a>Var olan çoğaltma grubuna yeni bir VM ekleyebilir miyim?
Evet.

## <a name="configuration-server"></a>Yapılandırma sunucusu

### <a name="what-does-the-configuration-server-do"></a>Yapılandırma sunucusu ne yapar?
Yapılandırma sunucusu Site Recovery bileşenlerini de dahil olmak üzere, şirket içi çalıştırır: 
- Şirket içi ve Azure arasındaki iletişimi düzenler ve veri çoğaltma yöneten yapılandırma sunucusu.
- Bir çoğaltma ağ geçidi olarak davranır işlem sunucusu. Çoğaltma verileri alan; önbelleğe alma, sıkıştırma ve şifreleme iyileştirir; ve Azure depolama., işlem sunucusu da Mobility hizmeti çoğaltmak istediğiniz ve şirket içi VMware vm'lerinin otomatik bulma işlemini gerçekleştirir Vm'lerinde yükler gönderir.
- Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler ana hedef sunucusu.

### <a name="where-do-i-set-up-the-configuration-server"></a>Yapılandırma sunucusunu ayarlamayı belirlendiği?
Yapılandırma sunucusu tek yüksek oranda kullanılabilir şirket içi VMware VM gerekir.

### <a name="what-are-the-requirements-for-the-configuration-server"></a>Yapılandırma sunucusu için gereksinimleri nelerdir?

Gözden geçirme [Önkoşullar](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>I el ile bir şablon kullanmak yerine yapılandırma sunucusu ayarlayabilirsiniz?
OVF şablona en son sürümünü kullanmanızı öneririz [yapılandırma sunucusu VM oluşturma](vmware-azure-deploy-configuration-server.md). Yapamazsınız bir nedenden dolayı Örneğin, VMware sunucusuna erişimi yoksa, şunları yapabilirsiniz [birleşik kurulum dosyasını karşıdan](physical-azure-set-up-source.md) Portal'dan ve bir VM üzerinde çalıştırın. 

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>Yapılandırma sunucusu, birden fazla bölgeye çoğaltabilir miyim?
Hayır. Bunu yapmak için her bölgede bir yapılandırma sunucusu ayarlamanız gerekir.

### <a name="can-i-host-a-configuration-server-in-azure"></a>Yapılandırma sunucusu Azure barındırmak?
Olası, şirket içi VMware altyapınızı ve VM'ler ile iletişim kurmak yapılandırma sunucusu çalıştıran Azure VM gerekir. Ek yükü büyük olasılıkla uygun değildir.


### <a name="where-can-i-get-the-latest-version-of-the-configuration-server-template"></a>Yapılandırma sunucusu şablonu en son sürümünü nereden alabilirim?
En son sürümü [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

### <a name="how-do-i-update-the-configuration-server"></a>Yapılandırma sunucusu nasıl güncelleştiririm?
Güncelleştirme paketleri yükleyin. En son güncelleştirme bilgileri bulabilirsiniz [wiki güncelleştirmeleri sayfası](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

## <a name="mobility-service"></a>Mobility hizmeti

### <a name="where-can-i-find-the-mobility-service-installers"></a>Mobility hizmeti yükleyicileri nereden bulabilirim?
Yükleyicileri içinde tutulur **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository** yapılandırma sunucusundaki klasör.

## <a name="how-do-i-install-the-mobility-service"></a>Mobility hizmetinin nasıl yükleme yaparım?
Çoğaltmak istediğiniz her bir VM kullanarak yüklediğiniz bir [anında yükleme](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery), veya el ile yükleme [UI](vmware-azure-install-mobility-service.md#install-mobility-service-manually-by-using-the-gui), veya [PowerShell kullanarak](vmware-azure-install-mobility-service.md#install-mobility-service-manually-at-a-command-prompt). Alternatif olarak, bir dağıtım aracı gibi kullanarak dağıtabilirsiniz [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md), veya [Azure Automation ve DSC](vmware-azure-mobility-deploy-automation-dsc.md).



## <a name="security"></a>Güvenlik

### <a name="what-access-does-site-recovery-need-to-vmware-servers"></a>Hangi erişim Site Recovery VMware sunucularına gerekiyor mu?
Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- Yapılandırma sunucusu ve diğer şirket içi Site Recovery bileşenlerini çalıştıran bir VMware VM ayarlayın. [Daha fazla bilgi](vmware-azure-deploy-configuration-server.md)
- Otomatik olarak VM'ler için çoğaltma bulur. Otomatik bulma için bir hesap hazırlama hakkında bilgi edinin. [Daha fazla bilgi](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery)


### <a name="what-access-does-site-recovery-need-to-vmware-vms"></a>Hangi erişim için VMware sanal makinelerini Site Recovery gerekiyor mu?

- Çoğaltma için bir VMware VM yüklü ve çalışan Site Recovery Mobility hizmeti olması gerekir. Aracı el ile dağıtma veya bir sanal makine için çoğaltmayı etkinleştirdiğinizde, Site Recovery hizmeti bir itme yüklemesini yapması gerektiğini belirtin. Gönderme yüklemesi için Site Recovery hizmeti bileşeni yüklemek için kullanabileceğiniz bir hesaba ihtiyacı vardır. [Daha fazla bilgi edinin](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-mobility-service-installation). (Varsayılan olarak yapılandırma sunucusu üzerinde çalışan) işlem sunucusu bu yükleme gerçekleştirir. [Daha fazla bilgi edinin](vmware-azure-install-mobility-service.md) Mobility hizmeti yüklemesi hakkında.
- Çoğaltma sırasında sanal makineleri gibi Site Recovery ile iletişim:
    - Sanal makineleri çoğaltma yönetimi için HTTPS 443 numaralı bağlantı noktasında yapılandırma sunucusuyla iletişim kurar.
    - Sanal makineleri (değiştirilebilir) bağlantı noktası HTTPS 9443 işlem sunucusu çoğaltma verileri gönderin.
    - Çoklu VM tutarlılığını etkinleştirmek, VM'lerin birbirleri ile 20004 bağlantı noktası üzerinden iletişim kurar.


### <a name="is-replication-data-sent-to-site-recovery"></a>Çoğaltma verilerini Site Recovery hizmetine gönderilir mi?
Hayır, Site Recovery çoğaltılan verilere müdahale etmez ve Vm'leriniz çalıştıran hakkında herhangi bir bilgi yoktur. Çoğaltma verilerinin VMware hiper yöneticileri ve Azure depolama arasında paylaşılmaz. Site Recovery bu verilere müdahale edemez. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.  

Site Recovery is ISO 27001:2013, 27018, HIPAA, DPA certified, and is in the process of SOC2 and FedRAMP JAB assessments.

### <a name="can-we-keep-on-premises-metadata-within-a-geographic-regions"></a>Biz, bir coğrafi bölge içinde şirket içi meta verileri tutabilir miyim?
Evet. Bir bölgede bir kasayı oluşturduğunuzda tüm meta veriler Site Recovery kalır bölgenin coğrafi sınırları içinde tarafından kullanılan emin olun.

### <a name="does-site-recovery-encrypt-replication"></a>Site Recovery çoğaltma işlemini şifreleyebilir mi?
Evet, hem şifreleme-aktarım sırasında ve [azure'da şifreleme](https://docs.microsoft.com/azure/storage/storage-service-encryption) desteklenir.


## <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma
### <a name="how-far-back-can-i-recover"></a>Ne kadar geri ı kurtarabilir miyim?
Azure için VMware kullanabileceğiniz eski kurtarma noktası 72 saat içindir.

### <a name="how-do-i-access-azure-vms-after-failover"></a>Yük devretme sonrasında Azure Vm'lerinin nasıl erişirim?
Yük devretme sonrasında Azure Vm'lerinin güvenli bir Internet bağlantısı üzerinden, siteden siteye VPN üzerinden veya Azure ExpressRoute üzerinden erişebilirsiniz. Bağlanmak için değişik hazırlamanız gerekir. [Daha fazla bilgi](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>Esnek veri üzerinde başarısız oldu?
Azure esneklik için tasarlanmıştır. Site kurtarma, ikincil bir Azure veri merkezine, Azure SLA uygun olarak yük devretme için tasarlanmıştır. Yük devretme durumunda biz meta verileriniz emin olun ve kasanız için seçtiğiniz aynı coğrafi bölge içindeki kasalarını kalır.

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?
[Yük devretme](site-recovery-failover.md) otomatik değildir. Yük devretme işlemlerini portalda tek tıklatmayla başlatmak veya kullanabilirsiniz [ PowerShell](/powershell/module/azurerm.siterecovery) bir yük devretme tetiklemek için.

### <a name="can-i-fail-back-to-a-different-location"></a>Farklı bir konuma başarısız olabilir?
Evet, Azure üzerinden başarısız olursa, özgün biri kullanılamıyorsa, farklı bir konuma başarısız olabilir. [Daha fazla bilgi edinin](concepts-types-of-failback.md#alternate-location-recovery-alr).

### <a name="why-do-i-need-a-vpn-or-expressroute-to-fail-back"></a>Bir VPN ya da ExpressRoute geri başarısız olmasına neden ihtiyacım var mı?

Azure'dan başarısız olduğunda, şirket içi VM Azure verilerinden kopyalanır ve özel erişim gereklidir. 



## <a name="automation-and-scripting"></a>Otomasyon ve komut dosyası

### <a name="can-i-set-up-replication-with-scripting"></a>Komut dosyası ile çoğaltma ayarlayabilir miyim?
Evet. Rest API'si, PowerShell veya Azure SDK'sını kullanarak Site Recovery iş akışlarını otomatikleştirebilirsiniz. [Daha fazla bilgi edinin](vmware-azure-disaster-recovery-powershell.md).

## <a name="performance-and-capacity"></a>Performans ve kapasite
### <a name="can-i-throttle-replication-bandwidth"></a>Çoğaltma bant genişliğini kısıtlayabilir miyim?
Evet. [Daha fazla bilgi edinin](site-recovery-plan-capacity-vmware.md).


## <a name="next-steps"></a>Sonraki adımlar
* [Gözden geçirme](vmware-physical-azure-support-matrix.md) gereksinimlerini destekler.
* [Ayarlanan](vmware-azure-tutorial.md) VMware Azure çoğaltma.
