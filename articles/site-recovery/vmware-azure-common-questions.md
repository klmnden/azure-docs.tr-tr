---
title: Azure Site Recovery ile Azure'a olağanüstü durum kurtarma için VMware hakkında sık sorulan sorular | Microsoft Docs
description: Azure Site Recovery kullanılarak Azure'da şirket içi VMware vm'lerinin olağanüstü durum kurtarma ile ilgili sık sorulan soruların yanıtlarını alın.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.date: 05/30/2019
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: 59be8e0585f0bedcafc868ee42f5113509c9c4ef
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66417783"
---
# <a name="common-questions-about-vmware-to-azure-replication"></a>Azure'a çoğaltma VMware hakkında sık sorulan sorular

Bu makalede, olağanüstü durum kurtarma şirket içi VMware sanal makinelerini (VM) Azure'a dağıtırken, gelen sık sorulan soruları yanıtlar.

## <a name="general"></a>Genel

### <a name="what-do-i-need-for-vmware-vm-disaster-recovery"></a>VMware VM'LERİNDE olağanüstü durum kurtarma için ne gerekir?

[Dahil olan bileşenleri hakkında bilgi edinin](vmware-azure-architecture.md) VMware vm'lerinin olağanüstü durum kurtarmasında.

### <a name="can-i-use-site-recovery-to-migrate-vmware-vms-to-azure"></a>VMware Vm'lerini Azure'a geçirmek için Site RECOVERY'yi kullanabilir miyim?

Evet. VMware Vm'leri için eksiksiz olağanüstü durum kurtarma ayarlama için site RECOVERY'yi kullanarak ek olarak, Site RECOVERY'yi şirket içi VMware Vm'leri Azure'a geçirmek için de kullanabilirsiniz. Bu senaryoda, Azure Depolama'ya şirket içi VMware Vm'lerini çoğaltma. Ardından, şirket içinden Azure'a yük devretme. Yük devretme sonrasında kullanılabilir ve Azure vm'lerinde çalışan iş yükleri ve uygulamalar. Geçiş işleminde Azure'dan yeniden çalışma özelliğini kullanamazsınız dışında tam olağanüstü durum kurtarma işlemini ayarlama gibi işlemidir.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Hesabımdaki Azure sanal makineler oluşturmak için izinler gerekiyor mu?

Bir abonelik yöneticisi değilseniz, ihtiyaç duyduğunuz çoğaltma izinleri sahip. Bir yönetici değilseniz, bu eylemleri için izinler gerekir:

- Azure VM kaynak oluşturma, Site Recovery yapılandırırken belirttiğiniz ağ, bu grup ve sanal.
- Seçili depolama hesabına veya yönetilen disk yapılandırmanızı temel alarak yazın.

[Daha fazla bilgi edinin](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) gerekli izinler hakkında.

### <a name="what-applications-can-i-replicate"></a>Hangi uygulamaların çoğaltabilirim?

Herhangi bir uygulamayı veya karşılayan bir VMware VM üzerinden çalışan iş yüklerini çoğaltabilirsiniz [çoğaltma gereksinimlerini](vmware-physical-azure-support-matrix.md#replicated-machines).

- Böylece uygulamaları devredilir ve akıllı bir duruma başarısız oldu, site Recovery uygulamayla tutarlı çoğaltma destekler.
- Site Recovery, SharePoint, Exchange, Dynamics, SQL Server ve Active Directory gibi Microsoft uygulamalarıyla tümleşir. Ayrıca yakından Oracle, SAP, IBM ve Red Hat gibi önde gelen satıcılarla çalışır.

İş yükü koruması hakkında [daha fazla bilgi edinin](site-recovery-workload.md).

### <a name="can-i-use-a-guest-os-server-license-on-azure"></a>Bir konuk işletim sistemi server lisansınız Azure'da kullanabilir miyim?

Evet, Microsoft Yazılım Güvencesi müşterileri kullanabilir [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) maliyetlerini Azure'a geçirilen Windows Server makineleri lisanslama kaydetmeye veya olağanüstü durum kurtarma için Azure'u kullanın.

## <a name="security"></a>Güvenlik

### <a name="what-access-to-vmware-servers-does-site-recovery-need"></a>Hangi VMware sunucularına erişimi, Site Recovery gerekiyor mu?

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- Site Recovery yapılandırma sunucusu çalıştıran bir VMware VM ayarlayın.
- Otomatik olarak VM için çoğaltma keşfedin.

### <a name="what-access-to-vmware-vms-does-site-recovery-need"></a>VMware Vm'leri için hangi erişim, Site Recovery gerekiyor mu?

- Bir VMware VM çoğaltmak için Site Recovery Mobility hizmeti yüklü ve çalışır olması gerekir. Aracı el ile dağıtabilirsiniz veya bir sanal makine için çoğaltmayı etkinleştirdiğinizde Site Recovery hizmeti göndererek yükleme yapmak belirtebilirsiniz.
- Çoğaltma sırasında VM'ler gibi Site Recovery ile iletişim:
    - VM'ler, çoğaltma yönetimi için HTTPS bağlantı noktası 443 üzerindeki yapılandırma sunucusu ile iletişim kurar.
    - Vm'leri çoğaltma verilerini HTTPS bağlantı noktası 9443 üzerinden işlem sunucusuna gönderir. (Bu ayar değiştirilebilir.)
    - Çoklu VM tutarlılığını etkinleştirirseniz, VM'ler birbiriyle 20004 bağlantı noktası üzerinden iletişim kurar.

### <a name="is-replication-data-sent-to-site-recovery"></a>Çoğaltma verilerini Site Recovery hizmetine gönderilir?

Hayır, Site Recovery çoğaltılan verilere müdahale etmez ve sanal makinelerinizde çalışan ne hakkında herhangi bir bilgi yoktur. Çoğaltma verilerini, VMware hiper yöneticileri ve Azure depolama değiştirilir. Site Recovery bu verilere müdahale edemez. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.  

Site Recovery, ISO 27001: 2013 ve 27018, HIPAA ve DPA için sertifikalıdır. SOC2 ile FedRAMP JAB değerlendirmelerini aşamasında olduğu.

## <a name="pricing"></a>Fiyatlandırma

### <a name="how-do-i-calculate-approximate-charges-for-vmware-disaster-recovery"></a>VMware olağanüstü durum kurtarma için yaklaşık ücretleri nasıl hesaplar?

Kullanım [fiyatlandırma hesaplayıcısını](https://aka.ms/asr_pricing_calculator) Site RECOVERY'yi kullanırken maliyetlerini tahmin etmek için.

Dağıtım planlayıcısı aracı için ayrıntılı tahmini maliyetleri çalıştırmak [VMware](https://aka.ms/siterecovery_deployment_planner) ve [maliyet tahmini raporunu](https://aka.ms/asr_DP_costreport).

### <a name="is-there-any-difference-in-cost-between-replicating-to-storage-or-directly-to-managed-disks"></a>Depolama veya doğrudan yönetilen diskler çoğaltma arasında bir fark maliyet var mı?

Yönetilen diskler, biraz daha farklı depolama hesaplarından ücretlendirilir. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/managed-disks/) yönetilen disk fiyatları hakkında.

## <a name="mobility-service"></a>Mobility hizmeti

### <a name="where-can-i-find-the-mobility-service-installers"></a>Mobility hizmeti yükleyicileri nerede bulabilirim?

Yapılandırma sunucusundaki %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository klasöründe yükleyicileri bulunmaktadır.

## <a name="how-do-i-install-the-mobility-service"></a>Mobility hizmeti nasıl yüklerim?

Çoğaltmak istediğiniz her VM'de hizmet birkaç yöntemden birini kullanarak yükleyin:

- [Push yüklemesi](vmware-physical-mobility-service-overview.md#push-installation)
- [El ile yükleme](vmware-physical-mobility-service-overview.md#install-mobility-agent-through-ui) kullanıcı Arabirimi veya PowerShell
- Dağıtım Aracı gibi kullanarak dağıtım [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md)

## <a name="managed-disks"></a>Yönetilen diskler

### <a name="where-does-site-recovery-replicate-data-to"></a>Site Recovery, veri nerede çoğaltabilir?

Site Recovery, şirket içi VMware Vm'leri ve fiziksel sunucuları azure'da yönetilen disklere çoğaltır.

- Site Recovery işlem sunucusu, hedef bölgedeki önbellek depolama hesabına çoğaltma günlükleri yazar.
- Bu günlükler önekine sahip Azure yönetilen diskler üzerinde kurtarma noktaları oluşturmak için kullanılan **asrseeddisk**.
- Yük devretme işlemi gerçekleştiğinde, seçtiğiniz kurtarma noktası hedefi yeni bir yönetilen disk oluşturmak için kullanılır. Bu yönetilen disk, azure'daki sanal makineye eklenir.
- Daha önce (Mart 2019 önce) bir depolama hesabına çoğaltılan sanal makineler etkilenmez.

### <a name="can-i-replicate-new-machines-to-storage-accounts"></a>Yeni makineleri depolama hesaplarına çoğaltabilir miyim?

Hayır. Azure portalında Mart 2019 ' başlayarak yalnızca Azure'a çoğaltabilirsiniz yönetilen diskler.

Bir depolama hesabı için yeni sanal makinelerin çoğaltmasını yalnızca PowerShell veya REST API'yi (2018-01-10 veya 2016-08-10 sürümü) kullanılarak kullanılabilir.

### <a name="what-are-the-benefits-of-replicating-to-managed-disks"></a>Yönetilen disklere çoğaltma avantajları nelerdir?

[Bilgi nasıl](https://azure.microsoft.com/blog/simplify-disaster-recovery-with-managed-disks-for-vmware-and-physical-servers/) Site Recovery, yönetilen diskler ile olağanüstü durum kurtarma basitleştirir.

### <a name="can-i-change-the-managed-disk-type-after-a-machine-is-protected"></a>Yönetilen disk türü, bir makine korunduktan sonra değiştirebilir miyim?

Evet, kolayca kullanabilirsiniz [yönetilen disk türünü değiştirme](https://docs.microsoft.com/azure/virtual-machines/windows/convert-disk-storage) sürekli çoğaltma için. Türü değiştirmeden önce yönetilen disk üzerinde oluşturulan URL hiç paylaşılan erişim imzası emin olun:

1. Git **yönetilen Disk** Azure portalında kaynak ve bir paylaşılan erişim imzası URL'si başlık sahip olup olmadığını denetleyin **genel bakış** dikey penceresi.
1. Başlık varsa, sürekli dışarı aktarma iptal etmek için seçin.
1. Disk türünü, birkaç dakika içinde değiştirin. Yönetilen disk türünü değiştirirseniz, yeni kurtarma noktaları, Azure Site Recovery tarafından oluşturulmasını bekleyin.
1. Yeni kurtarma noktaları, gelecekte herhangi bir yük devretme testi veya yük devretme için kullanın.

### <a name="can-i-switch-replication-from-managed-disks-to-unmanaged-disks"></a>Yönetilmeyen diskler için yönetilen diskleri çoğaltmadan geçiş yapabilir miyim?

Hayır. Geçiş için yönetilmeyen yönetilen desteklenmiyor.

## <a name="replication"></a>Çoğaltma

### <a name="what-are-the-replicated-vm-requirements"></a>Çoğaltılmış sanal makine gereksinimleri nelerdir?

[Daha fazla bilgi edinin](vmware-physical-azure-support-matrix.md#replicated-machines) VMware Vm'lerini ve fiziksel sunucular için destek gereksinimleri hakkında.

### <a name="how-often-can-i-replicate-to-azure"></a>Azure'a ne sıklıkta çoğaltabilirim?

VMware Vm'lerini Azure'a çoğaltırken çoğaltma sürekli olarak yapılır.

### <a name="can-i-extend-replication"></a>Ben çoğaltma uzatabilir miyim?

Genişletilmiş veya zincir çoğaltma desteklenmez. Bu özelliği isteği [geri bildirim Forumu](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959).

### <a name="can-i-do-an-offline-initial-replication"></a>Çevrimdışı ilk çoğaltma yapabilirim?

Çevrimdışı çoğaltma desteklenmez. Bu özelliği isteği [geri bildirim Forumu](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="what-is-asrseeddisk"></a>Asrseeddisk nedir?

Her kaynak disk için azure'da yönetilen disk için veriler çoğaltılır. Bu disk önekine sahip **asrseeddisk**. Bu, kaynak disk ve tüm kurtarma noktası anlık görüntüleri kopyasını depolar.

### <a name="can-i-exclude-disks-from-replication"></a>Diskleri çoğaltmanın dışında tutabilir miyim?

Evet, diskleri dışarıda tutabilirsiniz.

### <a name="can-i-replicate-vms-that-have-dynamic-disks"></a>Dinamik disklere sahip Vm'leri çoğaltabilirim?

Dinamik diskler çoğaltılabilir. İşletim sistemi diskinin bir temel disk olması gerekir.

### <a name="if-i-use-replication-groups-for-multi-vm-consistency-can-i-add-a-new-vm-to-an-existing-replication-group"></a>Çoklu VM tutarlılığı için çoğaltma gruplarını kullanıyorsanız, var olan bir çoğaltma grubuna yeni bir sanal makine ekleyebilirim?

Evet, bunlar için çoğaltmayı etkinleştirdiğinizde, mevcut bir çoğaltma grubuna yeni VM'ler ekleyebilirsiniz. Ancak:

- Çoğaltma başladıktan sonra mevcut bir çoğaltma grubuna VM ekleyemezsiniz.
- Var olan VM'ler için bir çoğaltma grubu oluşturulamıyor.

### <a name="can-i-modify-vms-that-are-replicating-by-adding-or-resizing-disks"></a>Ekleme veya diskleri yeniden boyutlandırma çoğaltılan VM'ler değiştirebiliyorum?

Azure'a VMware çoğaltma için disk boyutunu değiştirebilirsiniz. Yeni bir disk eklemek istiyorsanız, disk ekleyin ve VM için korumayı yeniden etkinleştirin.

### <a name="can-i-migrate-on-premises-machines-to-a-new-vcenter-server-without-impacting-ongoing-replication"></a>Şirket içi makineleri yeni bir vCenter Server sürmekte olan çoğaltmayı etkilemeden geçişini sağlayabilir miyim?

Hayır. VMware Vcenter veya geçiş, bir değişiklik, devam eden çoğaltma etkiler. Yeni vCenter Server ile site kurtarma işlemini ayarlama ve makineler için çoğaltmayı yeniden etkinleştirin.

### <a name="can-i-replicate-to-a-cache-or-target-storage-account-that-has-a-virtual-network-with-azure-firewalls-configured-on-it"></a>Yapılandırılmış bir sanal ağ (ile Azure güvenlik duvarları) sahip bir önbellek ya da hedef depolama hesabına çoğaltabilir miyim?

Hayır, Site Recovery, Azure depolama çoğaltma sanal ağlarda desteklememektedir.

## <a name="component-upgrade"></a>Bileşen yükseltme

### <a name="my-version-of-the-mobility-services-agent-or-configuration-server-is-old-and-my-upgrade-failed-what-do-i-do"></a>My Mobility Hizmetleri aracısı veya yapılandırma sunucusu sürümü eski olduğundan ve benim yükseltme başarısız oldu. Ne yapmalıyım?

Site Recovery N-4 destek modeli izler. [Daha fazla bilgi edinin](https://aka.ms/asr_support_statement) çok eski sürümlerinden yükseltme hakkında.

### <a name="where-can-i-find-the-release-notes-and-update-rollups-for-azure-site-recovery"></a>Burada miyim sürüm notlarını bulabilir ve Azure Site Recovery için güncelleştirme paketleri?

[Yeni güncelleştirmeler hakkında bilgi edinin](site-recovery-whats-new.md), ve [toplaması bilgileri alma](service-updates-how-to.md).

### <a name="where-can-i-find-upgrade-information-for-disaster-recovery-to-azure"></a>Azure'da olağanüstü durum kurtarma için yükseltme bilgileri nerede bulabilirim?

[Yükseltme hakkında bilgi edinmek](https://aka.ms/asr_vmware_upgrades).

## <a name="do-i-need-to-reboot-source-machines-for-each-upgrade"></a>Kaynak makineler her yükseltme için yeniden başlatma gerekiyor mu?

Yeniden başlatma, önerilen fakat her yükseltme için zorunlu değildir. [Daha fazla bilgi edinin](https://aka.ms/asr_vmware_upgrades).

## <a name="configuration-server"></a>Yapılandırma sunucusu

### <a name="what-does-the-configuration-server-do"></a>Yapılandırma sunucusu ne yapar?

Yapılandırma sunucusu da dahil olmak üzere, Site Recovery bileşenlerini şirket içi çalıştırır:

- Yapılandırma sunucusu kendisini. Sunucu, şirket içi bileşenler ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.
- İşlem sunucusu çoğaltma ağ geçidi davranır. Bu sunucu:
    1. Çoğaltma verilerini alır.
    2. Veri önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir.
    3. Verileri Azure depolamaya gönderir.
  İşlem sunucusu, ayrıca bir vm'lerinde Mobility hizmetinin göndererek yükleme yapar ve şirket içi VMware Vm'lerini otomatik olarak bulunmasını gerçekleştirir.
- Ana hedef sunucusu azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.

[Daha fazla bilgi edinin](vmware-azure-architecture.md) yapılandırma sunucusu bileşenleri ve süreçleri hakkında.

### <a name="where-do-i-set-up-the-configuration-server"></a>Yapılandırma sunucusunu ayarladıktan nereden ayarlayabilirim?

Tek ve yüksek oranda kullanılabilir şirket içi VMware VM için yapılandırma sunucusunu gerekir. Fiziksel sunucu olağanüstü durum kurtarma için yapılandırma sunucusunu bir fiziksel makineye yükleyin.

### <a name="what-do-i-need-for-the-configuration-server"></a>İçin yapılandırma sunucusunu ne yapmalıyım?

Gözden geçirme [önkoşulları](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>El ile bir şablonu kullanmak yerine yapılandırma sunucusunu ayarlayabilirim?

Olmasını öneririz, [yapılandırma sunucusu VM'si oluşturma](vmware-azure-deploy-configuration-server.md) açık sanal makine Format (OVF) şablonu en son sürümünü kullanarak. (Örneğin VMware sunucusuna erişiminiz yoksa), şablonu kullanamıyorsanız [indirme](physical-azure-set-up-source.md) kurulum dosyasını portalı ve yapılandırma sunucusunu ayarlayın.

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>Yapılandırma sunucusu, birden fazla bölgeye çoğaltabilir miyim?

Hayır. Birden fazla bölgeye çoğaltmak için her bölgede bir yapılandırma sunucusu gerekir.

### <a name="can-i-host-a-configuration-server-in-azure"></a>Azure'da bir yapılandırma sunucusu ana bilgisayar?

Mümkün olsa da, şirket içi VMware altyapınızı ve Vm'leri ile iletişim kurmak yapılandırma sunucusunu çalıştıran Azure VM gerekir. Bu iletişim, gecikme süresi ekler ve devam eden çoğaltma etkiler.

### <a name="how-do-i-update-the-configuration-server"></a>Yapılandırma sunucusu nasıl güncelleştirebilirim?

[Bilgi](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) nasıl yapılandırma sunucusunu güncelleştir.

- En son güncelleştirme bilgileri bulabilirsiniz [Azure güncelleştirmeleri sayfası](https://azure.microsoft.com/updates/?product=site-recovery).
- Portaldan en son sürümünü indirebilirsiniz. Ya da doğrudan yapılandırma sunucusunu en son sürümünü indirebilirsiniz [Microsoft Download Center](https://aka.ms/asrconfigurationserver).
- Sürümünüzü dörtten fazla sürüm geçerli sürümden daha eski ise bkz [destek deyimi](https://aka.ms/asr_support_statement) yükseltme yönergeleri için.

### <a name="should-i-back-up-the-configuration-server"></a>Ben yapılandırma sunucusunu ayarladıktan yedeklemelisiniz?

Yapılandırma sunucusunun düzenli zamanlanmış yedeklemeleri almaya öneririz.

- Başarıyla yeniden çalışma için geri başarısız VM yapılandırma sunucusu veritabanında mevcut olmalıdır.
- Yapılandırma sunucusunun çalıştığından ve bağlı durumda olması gerekir.
- [Daha fazla bilgi edinin](vmware-azure-manage-configuration-server.md) ortak yapılandırma sunucusu yönetim görevleri hakkında.

### <a name="when-im-setting-up-the-configuration-server-can-i-download-and-install-mysql-manually"></a>Yapılandırma sunucusu ayarlama, ı indirebilir ve MySQL el ile yükleme?

Evet. MySQL indirin ve C:\Temp\ASRSetup klasörüne yerleştirin. Daha sonra el ile yükleyin. VM yapılandırma sunucusu ayarlarsınız ve koşullarını kabul edin, MySQL olarak listelenecek **yüklü** içinde **yükleyip**.

### <a name="can-i-avoid-downloading-mysql-but-let-site-recovery-install-it"></a>Miyim MySQL yüklenmesini önlemek ancak Site Recovery yükleme izin?

Evet. MySQL yükleyiciyi indirir ve C:\Temp\ASRSetup klasörüne yerleştirin. Yapılandırma sunucusu VM'SİNİN ayarladığınızda koşullarını kabul edin ve seçin **yükleyip**. MySQL yüklemek için eklediğiniz yükleyici portalı kullanır.

### <a name="can-i-use-the-configuration-server-vm-for-anything-else"></a>Yapılandırma sunucusu sanal makine başka bir şey için kullanabilir miyim?

Hayır. VM için yapılandırma sunucusunu kullanın.

### <a name="can-i-clone-a-configuration-server-and-use-it-for-orchestration"></a>Bir yapılandırma Sunucusu'na kopyalayın ve miyim düzenleme işlemi için kullanılmakta?

Hayır. Kayıt sorunlarını önlemek için bir yeni yapılandırma sunucusunu ayarlayın.

### <a name="can-i-change-the-vault-in-which-the-configuration-server-is-registered"></a>İçinde yapılandırma sunucusunun kayıtlı olduğu kasayı değiştirebilir miyim?

Hayır. Kasa yapılandırma sunucusuyla ilişkili olduktan sonra değiştirilemez. [Bilgi](vmware-azure-manage-configuration-server.md#register-a-configuration-server-with-a-different-vault) yapılandırma sunucusunu farklı bir kasaya kaydetme hakkında.

### <a name="can-i-use-the-same-configuration-server-for-disaster-recovery-of-both-vmware-vms-and-physical-servers"></a>Aynı yapılandırma sunucusu VMware Vm'lerini ve fiziksel sunucuları olağanüstü durum kurtarma için kullanabilir miyim?

Evet, ancak bu fiziksel makine başarısız Not geriye yalnızca bir VMware VM.

### <a name="where-can-i-download-the-passphrase-for-the-configuration-server"></a>Yapılandırma sunucusu için parola nereden indirebilirim?

[Bilgi](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) nasıl parola indirin.

### <a name="where-can-i-download-vault-registration-keys"></a>Kasa kayıt anahtarlarının nereden indirebilirim?

Kurtarma Hizmetleri Kasası'nda seçin **Configuration Servers** içinde **Site Recovery altyapısı** > **Yönet**. Ardından **sunucuları**seçin **indirme kayıt anahtarı** kasa kimlik bilgileri dosyası indirilemedi.

## <a name="process-server"></a>İşlem sunucusu

### <a name="why-am-i-unable-to-select-the-process-server-when-i-enable-replication"></a>Neden miyim çoğaltmayı etkinleştirdiğinizde, işlem sunucusunu seçin oluşturamıyorum?

9,24 sürümleri ve üzeri şimdi görünen güncelleştirmeleri [çoğaltmayı etkinleştirdiğinizde işlem sunucusu durumunu](vmware-azure-enable-replication.md#enable-replication). Bu özellik, işlem sunucusu azaltmayı önlemek ve iyi durumda olmayan işlem sunucularının kullanımını en aza indirmek için yardımcı olur.

### <a name="how-do-i-update-the-process-server-to-version-924-or-later-for-accurate-health-information"></a>Nasıl işlem sunucusu 9.24 veya daha sonra doğru sistem durumu bilgileri için sürüm güncelleştirebilirim?

İle başlayarak [sürüm 9,24](service-updates-how-to.md#links-to-currently-supported-update-rollups), işlem sunucusunun sistem durumunu göstermek için daha fazla uyarı eklenmiştir. [9.24 veya sonraki bir sürümü için Site Recovery bileşenlerini güncelleştirin] (service-updates-how-to.md#links-to-currently-supported-update-rollups), böylece tüm uyarıları üretilir.

## <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma

### <a name="can-i-use-the-on-premises-process-server-for-failback"></a>Şirket içi işlem sunucusunu yeniden çalışma için kullanabilir miyim?

Veri aktarımı gecikmeleri önlemek için Azure'da yeniden çalışma amacıyla, bir işlem sunucusu oluşturmanızı kesinlikle öneririz. Ayrıca, Azure yönelik ağ yapılandırma sunucusu ile kaynak VM ağına ayrılmış durumda Azure'da yeniden çalışma için oluşturulan işlem sunucusu kullanabilmek için gerekli.

### <a name="can-i-keep-the-ip-address-on-failover"></a>Yük devretme sırasında IP adresini tutabilir miyim?

Evet, yük devretme sırasında IP adresini tutabilirsiniz. Hedef IP adresi belirttiğinizden emin olun **işlem ve ağ** yük devretmeden önce sanal makine için ayarlar. Ayrıca, yük devretme, yeniden çalışma sırasında IP adresi çakışmaları önlemek için zamanında makineleri kapat.

### <a name="can-i-change-the-target-vm-size-or-vm-type-before-failover"></a>Hedef VM boyutu veya VM türü yük devretmeden önce değiştirebilir miyim?

Evet, yük devretmeden önce herhangi bir zamanda VM boyutunu ve türünü değiştirebilirsiniz. Portalı'nda **işlem ve ağ** çoğaltılan sanal makine için ayarlar.

### <a name="how-far-back-can-i-recover"></a>Ne kadar geri kurtarma gerçekleştirebilir miyim?

VMware-Azure arası için kullanabileceğiniz en eski kurtarma noktası değer 72 saattir.

### <a name="how-do-i-access-azure-vms-after-failover"></a>Yük devretme sonrasında Azure Vm'lerini nasıl erişebilirim?

Yük devretme sonrasında Azure Vm'lerini güvenli bir internet bağlantısı üzerinden, siteden siteye VPN üzerinden veya Azure ExpressRoute üzerinden erişebilirsiniz. Bağlanmak için birkaç şey hazırlamanız gerekir. [Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).

### <a name="is-failed-over-data-resilient"></a>Devredilen veri esnek mi?

Azure esneklik için tasarlanmıştır. Site kurtarma, ikincil bir Azure veri merkezi, Azure hizmet düzeyi sözleşmesi (SLA) gerektirdiği şekilde yük devretme için tasarlanmıştır. Yük devretme işlemi gerçekleştiğinde, kasanız için seçtiğiniz aynı coğrafi bölgede kasaları kalır ve size meta verilerinizi emin olun.

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?

[Yük devretme](site-recovery-failover.md) otomatik değildir. Portalda tek bir seçim yaparak bir yük devretme başlatın ve kullanabileceğiniz [PowerShell](/powershell/module/az.recoveryservices) bir yük devretmeyi tetiklemek için.

### <a name="can-i-fail-back-to-a-different-location"></a>Ben, farklı bir konuma başarısız olabilir?

Evet. Azure'a yük devretmesi, ilkinin kullanılamıyorsa farklı bir konuma başarısız olabilir. [Daha fazla bilgi edinin](concepts-types-of-failback.md#alternate-location-recovery-alr).

### <a name="why-do-i-need-a-vpn-or-expressroute-with-private-peering-to-fail-back"></a>Neden bir VPN veya ExpressRoute özel yeniden çalışma için eşdüzey hizmet sağlama ile ihtiyacım var?

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

- [Gözden geçirme](vmware-physical-azure-support-matrix.md) gereksinimlerini destekler.
- [Ayarlanan](vmware-azure-tutorial.md) Vmware'den Azure'a çoğaltma.
