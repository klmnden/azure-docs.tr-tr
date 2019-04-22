---
title: Sık sorulan soruları - VMware-Azure Site Recovery ile Azure'a olağanüstü durum kurtarma | Microsoft Docs
description: Bu makalede yaygın sorular alanının azure'a olağanüstü durum kurtarmaya şirket içi VMware vm'lerinin Azure Site Recovery kullanarak özetlenmektedir.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.date: 04/08/2019
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: 2ab29c6e41204104320f4c2f583a24e53786bf3c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59360520"
---
# <a name="common-questions---vmware-to-azure-replication"></a>Sık sorulan sorular - Vmware'den Azure'a çoğaltma

Bu makalede, Azure'da şirket içi VMware vm'lerinin olağanüstü durum kurtarma dağıtırken yaygın soruların yanıtları sağlanır. 

## <a name="general"></a>Genel 
### <a name="what-do-i-need-for-vmware-vm-disaster-recovery"></a>VMware VM'LERİNDE olağanüstü durum kurtarma için ne gerekir?

[Hakkında bilgi edinin](vmware-azure-architecture.md) VMware vm'lerinin olağanüstü durum kurtarma ilgili bileşenleri. 

### <a name="can-i-use-site-recovery-to-migrate-vmware-vms-to-azure"></a>VMware Vm'lerini Azure'a geçirmek için Site RECOVERY'yi kullanabilir miyim?

Evet, VMware Vm'leri için eksiksiz olağanüstü durum kurtarma ayarlama için Site Recovery kullanmanın yanı sıra, ayrıca Site Recovery şirket içi VMware Vm'leri Azure'a geçirmek için kullanabilirsiniz. Bu senaryoda, şirket içi VMware Vm'lerini Azure depolama alanına çoğaltın. Ardından, şirket içinden Azure'a yük devretme. Yük devretme sonrasında kullanılabilir ve Azure vm'lerinde çalışan iş yükleri ve uygulamalar. Geçiş işleminde Azure'dan yeniden çalışma özelliğini kullanamazsınız işlemi tam olağanüstü durum kurtarma ayarlama ayarlayarak benzer olmasıdır.


### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Hesabımdaki Azure sanal makineler oluşturmak için izinler gerekiyor mu?
Bir abonelik yöneticisi değilseniz, ihtiyaç duyduğunuz çoğaltma izinleri sahip. Değilseniz, bir Azure VM kaynak grubu ve Site Recovery ve seçili depolama hesabına yazma izinleri yapılandırın veya yönetilen disk yapılandırmanızı temel alarak belirttiğiniz sanal ağ oluşturmak için izinler gerekir. [Daha fazla bilgi edinin](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines).

### <a name="what-applications-can-i-replicate"></a>Hangi uygulamaların çoğaltabilirim?
Herhangi bir uygulamayı veya ile uyumlu bir VMware VM'de çalışan iş yüklerini çoğaltabilirsiniz [çoğaltma gereksinimlerini](vmware-physical-azure-support-matrix.md##replicated-machines).
- Site kurtarma uygulamayla tutarlı çoğaltma için destek sağlar, böylece uygulamalar üzerinde başarısız oldu ve akıllı bir duruma başarısız oldu.
- Site Recovery, SharePoint, Exchange, Dynamics, SQL Server ve Active Directory gibi Microsoft uygulamalarıyla tümleşir ve Oracle, SAP, IBM ve Red Hat gibi önde gelen satıcılarla yakın bir tümleştirmede çalışır.
- İş yükü koruması hakkında [daha fazla bilgi edinin](site-recovery-workload.md).

### <a name="can-i-use-a-guest-os-server-license-on-azure"></a>Bir konuk işletim sistemi server lisansınız Azure'da kullanabilir miyim?
Evet, Microsoft Yazılım Güvencesi müşterileri kullanabilir [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) lisanslama maliyetlerini kaydetmek için **Windows Server makineleri** azure'a veya Azure olağanüstü durum kurtarma için kullanılacak geçirilir.

## <a name="security"></a>Güvenlik

### <a name="what-access-does-site-recovery-need-to-vmware-servers"></a>Hangi erişim Site Recovery, VMware sunucuları için ihtiyaç duyuyor mu?
Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- Site Recovery yapılandırma sunucusu çalıştıran VMware VM'i ayarlayın,
- Otomatik olarak VM için çoğaltma keşfedin. 


### <a name="what-access-does-site-recovery-need-to-vmware-vms"></a>Hangi erişim Site Recovery, VMware Vm'lerinde gerekiyor mu?

- Bir VMware VM çoğaltmak için Site Recovery Mobility hizmeti yüklü ve çalışıyor olması gerekir. Aracı el ile dağıtmak veya bir sanal makine için çoğaltmayı etkinleştirdiğinizde Site Recovery hizmeti göndererek yükleme yapmalısınız belirtin. 
- Çoğaltma sırasında VM'ler gibi Site Recovery ile iletişim:
    - VM'ler, çoğaltma yönetimi için HTTPS 443 numaralı bağlantı noktasındaki yapılandırma sunucusuyla iletişim kurar.
    - Vm'leri çoğaltma verilerini HTTPS 9443 gelen bağlantı (değiştirilebilir) işlem sunucusu gönderin.
    - Çoklu VM tutarlılığını etkinleştirirseniz, VM'ler birbiriyle 20004 bağlantı noktası üzerinden iletişim kurar.


### <a name="is-replication-data-sent-to-site-recovery"></a>Çoğaltma verilerini Site Recovery hizmetine gönderilir?
Hayır, Site Recovery çoğaltılan verilere müdahale etmez ve sanal makinelerinizde çalışan ne hakkında herhangi bir bilgi yoktur. Çoğaltma verilerini, VMware hiper yöneticileri ve Azure depolama değiştirilir. Site Recovery bu verilere müdahale edemez. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.  

Site Recovery, ISO 27001: 2013, 27018, HIPAA, DPA sertifikalı ve SOC2 ile FedRAMP JAB değerlendirmelerini sürecinde olduğundan.


## <a name="pricing"></a>Fiyatlandırma
### <a name="how-can-i-calculate-approximate-charges-for-vmware-disaster-recovery"></a>VMware olağanüstü durum kurtarma için yaklaşık ücretleri hesaplamak nasıl?

Kullanabileceğiniz [fiyatlandırma hesaplayıcısını](https://aka.ms/asr_pricing_calculator) Site RECOVERY'yi kullanırken maliyetlerini tahmin etmek için.

Maliyetleri ayrıntılı tahmin için dağıtım planlayıcısı aracı çalıştırma [VMware](https://aka.ms/siterecovery_deployment_planner)ve [maliyet tahmini raporunu](https://aka.ms/asr_DP_costreport).

### <a name="is-there-any-difference-in-cost-between-replicating-to-storage-or-directly-to-managed-disks"></a>Depolama veya doğrudan yönetilen diskler çoğaltma arasında bir fark maliyet var mı?

Yönetilen diskler, depolama hesapları biraz farklı ücretlendirilir. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/managed-disks/) yönetilen disk fiyatları hakkında.

## <a name="mobility-service"></a>Mobility hizmeti

### <a name="where-can-i-find-the-mobility-service-installers"></a>Mobility hizmeti yükleyicileri nerede bulabilirim?
Yükleyicileri bulunan **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository** yapılandırma sunucusundaki klasör.

## <a name="how-do-i-install-the-mobility-service"></a>Mobility hizmeti nasıl yüklerim?
Her sanal makinede çoğaltmak için birkaç yöntem kullanarak istediğiniz yükleyin:
- [Push yüklemesi](vmware-physical-mobility-service-overview.md#push-installation)
- [El ile yükleme](vmware-physical-mobility-service-overview.md#install-mobility-agent-through-ui) kullanıcı Arabirimi veya Powershell.
- Dağıtım bir dağıtım aracı gibi kullanarak [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md).

## <a name="managed-disks"></a>Yönetilen diskler

### <a name="where-does-site-recovery-replicate-data-to"></a>Site Recovery, veri nerede çoğaltabilir?

Site Recovery, şirket içi VMware Vm'leri ve fiziksel sunucuları azure'da yönetilen disklere çoğaltır.
- Site Recovery işlem sunucusu, hedef bölgedeki önbellek depolama hesabına çoğaltma günlükleri yazar.
- Bu günlükler, yönetilen diskler üzerindeki kurtarma noktaları oluşturmak için kullanılır.
- Yük devretme işlemi gerçekleştiğinde, seçtiğiniz kurtarma noktası hedefi yönetilen disk oluşturmak için kullanılır.
- Daha önce (Mart 2019 önce) bir depolama hesabına çoğaltılan sanal makineler etkilenmez.


### <a name="can-i-replicate-new-machines-to-storage-accounts"></a>Yeni makineleri depolama hesaplarına çoğaltabilir miyim?

Hayır, portalda Mart 2019 başlayarak, yalnızca Azure'a çoğaltabilirsiniz yönetilen diskler. 

PowerShell veya REST API (sürümü 2018-01-10 ya da 2016-08-10) kullanarak bir depolama hesabı için yeni sanal makinelerin çoğaltmasını yalnızca kullanılabilir.

### <a name="what-are-the-benefits-in-replicating-to-managed-disks"></a>Yönetilen disklere çoğaltma içinde yararları nelerdir?

[Bilgi nasıl](https://azure.microsoft.com/blog/simplify-disaster-recovery-with-managed-disks-for-vmware-and-physical-servers/) Site Recovery, yönetilen diskler ile olağanüstü durum kurtarma basitleştirir.


### <a name="can-i-change-the-managed-disk-type-after-machine-is-protected"></a>Yönetilen disk türü, makine korunduktan sonra değiştirebilir miyim?

Evet, kolayca kullanabilirsiniz [yönetilen disk türünü değiştirme](https://docs.microsoft.com/azure/virtual-machines/windows/convert-disk-storage). Yönetilen disk türü olabilir, ancak değişiklikten sonra testi, yük devretme veya yük devretme gerekiyorsa oluşturulacak yeni kurtarma noktaları için bekleyin.

### <a name="can-i-switch-replication-from-managed-disks-to-unmanaged-disks"></a>Yönetilmeyen diskler için yönetilen diskleri çoğaltmadan geçiş yapabilir miyim?

Hayır, geçiş için yönetilmeyen yönetilen desteklenmiyor.

## <a name="replication"></a>Çoğaltma


### <a name="what-are-the-replicated-vm-requirements"></a>Çoğaltılmış sanal makine gereksinimleri nelerdir?

[Daha fazla bilgi edinin](vmware-physical-azure-support-matrix.md##replicated-machines) VMware VM'LERİNİ/fiziksel sunucu gereksinimleri ve destek hakkında.

### <a name="how-often-can-i-replicate-to-azure"></a>Azure'a ne sıklıkta çoğaltabilirim?
VMware Vm'lerini Azure'a çoğaltırken çoğaltma sürekli olarak yapılır.


### <a name="can-i-extend-replication"></a>Ben çoğaltma uzatabilir miyim?
Genişletilmiş veya zincir çoğaltma desteklenmez. Bu özelliği isteği [geri bildirim Forumu](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959).

### <a name="can-i-do-an-offline-initial-replication"></a>Çevrimdışı ilk çoğaltma yapabilirim?
Bu özellik desteklenmez. Bu özelliği isteği [geri bildirim Forumu](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-disks-from-replication"></a>Diskleri çoğaltmanın dışında tutabilir miyim?
Evet, diskleri dışarıda tutabilirsiniz.

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>Sanal makineleri dinamik disklerle çoğaltabilir miyim?
Dinamik diskler çoğaltılabilir. İşletim sistemi diskinin bir temel disk olması gerekir.

### <a name="if-i-use-replication-groups-for-multi-vm-consistency-can-i-add-a-new-vm-to-an-existing-replication-group"></a>Çoklu VM tutarlılığı için çoğaltma gruplarını kullanıyorsanız, var olan bir çoğaltma grubuna yeni bir sanal makine ekleyebilirim?
Evet, bunlar için çoğaltmayı etkinleştirdiğinizde, mevcut bir çoğaltma grubuna yeni VM'ler ekleyebilirsiniz.
- Çoğaltma başlatıldıktan sonra mevcut bir çoğaltma grubuna VM ekleyemezsiniz.
- Var olan VM'ler için bir çoğaltma grubu oluşturulamıyor.

### <a name="can-i-modify-vms-that-are-replicating-by-adding-or-resizing-disks"></a>Ekleme veya diskleri yeniden boyutlandırma çoğaltılan VM'ler değiştirebiliyorum?

Azure'a VMware çoğaltma için disk boyutunu değiştirebilirsiniz. Yeni diskler eklemek istiyorsanız disk ekleyin ve VM için korumayı etkinleştirmeniz gerekir.

### <a name="can-i-migrate-on-premises-machines-to-a-new-vcenter-server-without-impacting-ongoing-replication"></a>Şirket içi makineleri yeni bir vCenter Server sürmekte olan çoğaltmayı etkilemeden geçişini sağlayabilir miyim?
Hayır, Vcenter veya geçiş değişiklik sürmekte olan çoğaltmayı etkiler. Yeni vCenter Server ile Site kurtarma işlemini ayarlama ve makineler için çoğaltmayı yeniden etkinleştirmeniz gerekir.

### <a name="can-i-replicate-to-a-cachetarget-storage-account-which-has-a-vnet-with-azure-storage-firewalls-configured-on-it"></a>Yapılandırılmış bir Vnet'e (Azure depolama güvenlik duvarları) sahip bir önbellek/hedef depolama hesabına çoğaltabilir miyim?
Hayır, Site Recovery çoğaltma depolama Vnet üzerinde desteklememektedir.





## <a name="component-upgrade"></a>Bileşen yükseltme

### <a name="my-version-of-the-mobility-services-agent-or-configuration-server-is-old-and-my-upgrade-failed-what-do-i-do"></a>Kullandığım Mobility Hizmetleri aracısı veya yapılandırma sunucusu sürümü, eski ve benim yükseltme başarısız oldu ' dir. Ne yapmalıyım?  

Site Recovery N-4 destek modeli izler. [Daha fazla bilgi edinin](https://aka.ms/asr_support_statement) çok eski sürümlerinden yükseltme hakkında.

### <a name="where-can-i-find-the-release-notesupdate-rollups-of-azure-site-recovery"></a>Azure Site kurtarma sürümünün sürüm notları/güncelleştirme paketlerini nerede bulabilirim?

[Bilgi](site-recovery-whats-new.md) yeni güncelleştirmeler hakkında ve [toplaması bilgileri alma](service-updates-how-to.md).

### <a name="where-can-i-find-upgrade-information-for-disaster-recovery-to-azure"></a>Azure'da olağanüstü durum kurtarma için yükseltme bilgileri nerede bulabilirim?

[Hakkında bilgi edinin](https://aka.ms/asr_vmware_upgrades) yükseltme.

## <a name="do-i-need-to-reboot-source-machines-for-each-upgrade"></a>Kaynak makineler her yükseltme için yeniden başlatma gerekiyor mu?

Önerilen da, her bir yükseltme için zorunlu değildir. [Daha fazla bilgi edinin](https://aka.ms/asr_vmware_upgrades).


## <a name="configuration-server"></a>Yapılandırma sunucusu

### <a name="what-does-the-configuration-server-do"></a>Yapılandırma sunucusu ne yapar?

Yapılandırma sunucusu da dahil olmak üzere, Site Recovery bileşenlerini şirket içi çalıştırır:
- Şirket içi ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir yapılandırma sunucusunun kendisi.
- İşlem sunucusu çoğaltma ağ geçidi davranır. Çoğaltma verilerini alıp bunları önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir. İşlem sunucusu ayrıca bir vm'lerinde Mobility hizmetinin göndererek yükleme yapar ve şirket içi VMware Vm'lerini otomatik olarak bulunmasını gerçekleştirir.
- Ana hedef sunucu azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.

[Daha fazla bilgi edinin](vmware-azure-architecture.md) yapılandırma sunucusu bileşenleri ve süreçleri hakkında.

### <a name="where-do-i-set-up-the-configuration-server"></a>Yapılandırma sunucusunu ayarladıktan nereden ayarlayabilirim?
İçin yapılandırma sunucusunu tek yüksek oranda kullanılabilir şirket içi VMware VM ihtiyacınız var. Fiziksel sunucu olağanüstü durum kurtarma için fiziksel bir makinede yapılandırma sunucusu yükleyebilirsiniz.

### <a name="what-do-i-need-for-the-configuration-server"></a>İçin yapılandırma sunucusunu ne yapmalıyım?

Gözden geçirme [önkoşulları](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>El ile bir şablonu kullanmak yerine yapılandırma sunucusunu ayarlayabilirim?
Olmasını öneririz, [yapılandırma sunucusu VM'si oluşturma](vmware-azure-deploy-configuration-server.md) OVF şablonunu en son sürümü ile. Yapamazsınız herhangi bir nedenle, örneğin, VMware sunucusuna erişimi yoksa, şunları yapabilirsiniz [indirme](physical-azure-set-up-source.md) kurulum dosyasını portalı ve yapılandırma sunucusunu ayarlayın.

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>Yapılandırma sunucusu, birden fazla bölgeye çoğaltabilir miyim?
Hayır. Bunu yapmak için her bölgede bir yapılandırma sunucusu gerekir.

### <a name="can-i-host-a-configuration-server-in-azure"></a>Azure'da bir yapılandırma sunucusu ana bilgisayar?
Olası, şirket içi VMware altyapınızı ve Vm'leri ile iletişim kurmak yapılandırma sunucusunu çalıştıran Azure VM gerekir. Bu gecikme süresi ekler ve devam eden çoğaltma etkiler.

### <a name="how-do-i-update-the-configuration-server"></a>Yapılandırma sunucusu nasıl güncelleştirebilirim?

[Bilgi](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) nasıl yapılandırma sunucusunu güncelleştir.
- En son güncelleştirme bilgileri bulabilirsiniz [Azure güncelleştirmeleri sayfası](https://azure.microsoft.com/updates/?product=site-recovery).
- Portaldan en son sürümünü indirebilirsiniz. Alternatif olarak, yapılandırma sunucusunun en son sürümünü doğrudan indirebileceğiniz [Microsoft Download Center](https://aka.ms/asrconfigurationserver).
- Sürümünüzü dörtten fazla sürüm geçerli sürümden daha eski ise, başvurmak bizim [destek deyimi](https://aka.ms/asr_support_statement) yükseltme yönergeleri için.

### <a name="should-i-back-up-the-configuration-server"></a>Ben yapılandırma sunucusunu ayarladıktan yedeklemelisiniz?
Yapılandırma sunucusunun düzenli zamanlanmış yedeklemeleri almaya öneririz.
- Başarıyla yeniden çalışma için geri başarısız VM yapılandırma sunucusu veritabanında mevcut olmalıdır.
- Yapılandırma sunucusunun çalıştığından ve bağlı durumda olması gerekir.
- [Daha fazla bilgi edinin](vmware-azure-manage-configuration-server.md) ortak yapılandırma sunucusu yönetim görevleri hakkında.

### <a name="when-im-setting-up-the-configuration-server-can-i-download-and-install-mysql-manually"></a>Yapılandırma sunucusu ayarlama, ı indirebilir ve MySQL el ile yükleme?

Evet. MySQL indirin ve yerleştirebilir **C:\Temp\ASRSetup** klasör. Daha sonra el ile yükleyin. VM yapılandırma sunucusu ayarlarsınız ve koşullarını kabul edin, MySQL olarak listelenecek **yüklü** içinde **yükleyip**.

### <a name="can-i-avoid-downloading-mysql-but-let-site-recovery-install-it"></a>Miyim MySQL yüklenmesini önlemek ancak Site Recovery yükleme izin?

Evet. MySQL yükleyiciyi indirir ve yerleştirebilir **C:\Temp\ASRSetup** klasör.  Yapılandırma sunucusu VM'SİNİN ayarladığınızda, koşulları kabul edin ve tıklayarak **yükleyip**. MySQL yüklemek için eklediğiniz yükleyici portalı kullanır.
 
### <a name="can-i-use-the-configuration-server-vm-for-anything-else"></a>Yapılandırma sunucusu sanal makine başka bir şey için kullanabilir miyim?
Hayır, yapılandırma sunucusu için yalnızca VM kullanmanız gerekir. 

### <a name="can-i-clone-a-configuration-server-and-use-it-for-orchestration"></a>Bir yapılandırma Sunucusu'na kopyalayın ve miyim düzenleme işlemi için kullanılmakta?
Hayır, kayıt sorunlarını önlemek için yeni yapılandırma sunucusunu ayarladıktan ayarlamanız gerekir.

### <a name="can-i-change-the-vault-in-which-the-configuration-server-is-registered"></a>İçinde yapılandırma sunucusunun kayıtlı olduğu kasayı değiştirebilir miyim?
Hayır. Kasa yapılandırma sunucusuyla ilişkili olduktan sonra değiştirilemez. Gözden geçirme [bu makalede](vmware-azure-manage-configuration-server.md#register-a-configuration-server-with-a-different-vault) yeniden kaydetme hakkında bilgi edinmek için.

### <a name="can-i-use-the-same-configuration-server-for-disaster-recovery-of-both-vmware-vms-and-physical-servers"></a>Aynı yapılandırma sunucusu VMware Vm'lerini ve fiziksel sunucuları olağanüstü durum kurtarma için kullanabilir miyim
Evet, ancak bu fiziksel makine yalnızca bir VMware VM'sine geri çalışılabilir unutmayın.

### <a name="where-can-i-download-the-passphrase-for-the-configuration-server"></a>Yapılandırma sunucusu için parola nereden indirebilirim?
[Bilgi edinmek için nasıl](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) parola indirin.

### <a name="where-can-i-download-vault-registration-keys"></a>Kasa kayıt anahtarlarının nereden indirebilirim?

Kurtarma Hizmetleri Kasası'nda tıklatın **Configuration Servers** içinde **Site Recovery altyapısı** > **Yönet**. Ardından **sunucuları**seçin **indirme kayıt anahtarı** kasa kimlik bilgileri dosyası indirilemedi.







## <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma
### <a name="can-i-use-the-process-server-at-on-premises-for-failback"></a>İşlem sunucusu şirket içi yeniden çalışma için kullanabilir miyim?
Veri aktarımı gecikmeleri önlemek için Azure'da yeniden çalışma amacıyla, bir işlem sunucusu oluşturma önerilir. Ayrıca, Azure yönelik ağ yapılandırma sunucusu ile kaynak VM ağına ayrılmış durumda Azure'da yeniden çalışma için oluşturulan işlem sunucusu kullanabilmek için gerekli.

### <a name="can-i-retain-the-ip-address-on-failover"></a>Ben, yük devretme IP adresi tutabilir miyim?
Evet, yük devretme IP adresi tutabilirsiniz. 'İşlem ve ağ' ayarları için yük devretme gerçekleştirmeden önce VM'yi bir hedef IP adresi belirttiğinizden emin olun. Ayrıca, yük devretme, yeniden çalışma sırasında IP adresi çakışmaları önlemek için zamanında makineleri kapat.

### <a name="can-i-change-the-target-vm-size-or-vm-type-before-failover"></a>Hedef VM boyutu veya VM türü yük devretmeden önce değiştirebilir miyim?
Evet, türe veya dilediğiniz zaman yük devretmeden önce şirket Portalı'nda, çoğaltılan sanal makinenin işlem ve ağ ayarlarına VM'nin boyutunu değiştirebilirsiniz.

### <a name="how-far-back-can-i-recover"></a>Ne kadar geri kurtarma gerçekleştirebilir miyim?
VMware-Azure arası için kullanabileceğiniz en eski kurtarma noktası değer 72 saattir.

### <a name="how-do-i-access-azure-vms-after-failover"></a>Yük devretme sonrasında Azure Vm'lerini nasıl erişebilirim?
Yük devretme sonrasında Azure Vm'lerini güvenli bir Internet bağlantısı üzerinden, siteden siteye VPN üzerinden veya Azure ExpressRoute üzerinden erişebilirsiniz. Bağlanmak için etmenizi hazırlamanız gerekir. [Daha fazla bilgi](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>Dayanıklı veriler üzerinde başarısız oldu?

Azure esneklik için tasarlanmıştır. Site kurtarma, ikincil bir Azure veri merkezi, Azure SLA'sı uygun olarak yük devretme için tasarlanmıştır. Yük devretme işlemi gerçekleştiğinde, kasanız için seçtiğiniz aynı coğrafi bölgede kasaları kalır ve biz meta verilerinizi emin olun.

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?
[Yük devretme](site-recovery-failover.md) otomatik değildir. Portaldaki tek tıklamayla yük devretme başlatın veya kullanabileceğiniz [PowerShell](/powershell/module/az.recoveryservices) bir yük devretmeyi tetiklemek için.

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
