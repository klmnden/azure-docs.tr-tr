---
title: VMware değerlendirme ve geçiş için Azure geçişi destek matrisi
description: Destek ayarları ve değerlendirme ve VMware vm'lerinin Azure geçişi hizmetini kullanarak azure'a geçiş için sınırlamaları özetlenmektedir.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: raynew
ms.openlocfilehash: 567a6582e193208a7ff4aa37bafefe1dec4f4e8f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811589"
---
# <a name="support-matrix-for-vmware-assessment-and-migration"></a>VMware değerlendirme ve geçiş için destek matrisi

Kullanabileceğiniz [Azure geçişi hizmeti](migrate-overview.md) değerlendirmek ve makineleri için Microsoft Azure bulutuna geçirmek için. Bu makalede, destek ayarları ve değerlendirme ve şirket içi VMware Vm'lerini geçiş sınırlamaları özetlenmektedir.


## <a name="vmware-scenarios"></a>VMware senaryoları

Tabloda, VMware Vm'leri için desteklenen senaryolar özetlenmiştir.

**Dağıtım** | **Ayrıntılar** 
--- | --- 
**Şirket içi VMware sanal makineleri değerlendirme** | [Ayarlanan](tutorial-prepare-vmware.md) ilk değerlendirmenizi.<br/><br/> [Çalıştırma](scale-vmware-assessment.md) büyük ölçekli bir değerlendirme.
**VMware Vm'lerini geçirme** | Aracısız geçiş, bazı kısıtlamalarla kullanarak geçiş yapmaya veya aracı tabanlı bir geçiş kullanın. [Daha fazla bilgi edinin](server-migrate-overview.md)


    


## <a name="azure-migrate-projects"></a>Azure geçiş projeleri

**Destek** | **Ayrıntılar**
--- | ---
Azure izinleri | Bir Azure geçişi projesi oluşturmak için katkıda bulunan veya sahip izinleri aboneliği gerekir.
VMware sınırlamaları  | Tek bir projede en fazla 35.000 VMware Vm'lerini değerlendirin.

Bir proje, VMware Vm'lerini hem Hyper-V Vm'lerinden, en fazla değerlendirme sınırları içerebilir.

## <a name="assessment-vmware-server-requirements"></a>Değerlendirme VMware sunucusu gereksinimleri

Bu tabloda değerlendirme desteği ve VMware sanallaştırma sunucularını sınırlamaları özetlenmektedir.

**Destek** | **Ayrıntılar**
--- | ---
**vCenter sunucusu** | Değerlendirmek istediğiniz VMware Vm'leri 5.5, 6.0, 6.5 veya 6.7 çalışan bir veya daha fazla vCenter sunucusu tarafından yönetiliyor olması gerekir.

## <a name="assessment-vcenter-server-permissions"></a>Değerlendirme vCenter Server izinleri

Değerlendirme için yalnızca vCenter Server için salt okunur bir hesap gerekir.

## <a name="assessment-appliance-requirements"></a>Gereç değerlendirme gereksinimleri

**Destek** | **Ayrıntılar**
--- | ---
**ESXi** | 5\.5 veya sonraki sürümünü çalıştıran bir ESXi ana bilgisayarındaki VM Gereci dağıtılması gerekir.
**Azure geçişi projesi** | Gereçlerden biri tek bir proje ile ilişkili olabilir.
**vCenter Server** | Bir vCenter sunucusu en fazla 10.000 VMware Vm'lerinde gereçlerden biri bulabilir.<br/> Gereçlerden biri, bir vCenter Server'a bağlanabilirsiniz.


## <a name="assessment-url-access-requirements"></a>Değerlendirme URL'si erişim gereksinimleri

Azure geçişi gereç, İnternet'e internet bağlantısı gerekir.

- Gereç dağıtırken, Azure geçişi tabloda özetlenen URL'lere bir bağlantı denetimi yapar.
- URL tabanlı bir firewall.proxy kullanıyorsanız, proxy herhangi bir CNAME kayıtları URL'lere aranırken alınan çözümler emin olmak, bu URL'ler erişime izin verin.

**URL** | **Ayrıntılar**  
--- | --- |
*.portal.azure.com  | Azure portalında Azure geçişi gidin.
*.windows.net | Azure aboneliğinizde oturum açın.
*.microsoftonline.com | Gerecin Azure geçişi hizmeti ile iletişim kurmak için Active Directory uygulamalar oluşturun.
Management.Azure.com | Gerecin Azure geçişi hizmeti ile iletişim kurmak için Active Directory uygulamalar oluşturun.
dc.services.visualstudio.com | İç izleme için kullanılan uygulama günlüklerini karşıya yükleyin.
*.vault.azure.net | Azure Key Vault'ta ' gizli diziler yönetin.
*.servicebus.windows.net | Gereç ve Azure geçişi hizmeti arasındaki iletişim.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Azure geçişi hizmeti URL'leri için bağlanın.
*.blob.core.windows.net | Veri depolama hesaplarına karşıya yükleyin.


## <a name="assessment-port-requirements"></a>Değerlendirme-bağlantı noktası gereksinimleri

**cihaz** | **bağlantı**
--- | --- 
Gereç | Gereç için Uzak Masaüstü bağlantılarına izin verecek şekilde 3389 numaralı TCP bağlantı noktasında gelen bağlantılara.<br/> Gelen bağlantılar 44368 Gereci yönetim uygulamayı URL kullanarak uzaktan erişmek için bağlantı noktası: https://<appliance-ip-or-name>:44368 <br/>Azure geçişi için bulma ve performans meta verileri göndermek için bağlantı noktası 443 üzerinden giden bağlantılar.
vCenter sunucusu | Gelen bağlantılar değerlendirmeler için yapılandırma ve performans meta verilerini toplamak gereç izin vermek için TCP bağlantı noktası 443 üzerinden. <br/> Varsayılan olarak 443 numaralı bağlantı noktasında vCenter gereç bağlanır. VCenter sunucusu farklı bir bağlantı noktasında dinliyorsa, discovery'yi ayarlama ayarladığınızda, bağlantı noktasını değiştirebilirsiniz.


## <a name="agentless-migration-vmware-server-requirements"></a>Aracısız geçiş VMware sunucusu gereksinimleri

Bu tabloda değerlendirme desteği ve VMware sanallaştırma sunucularını sınırlamaları özetlenmektedir.

**Destek** | **Ayrıntılar**
--- | ---
**vCenter sunucusu** | VMware Vm'lerini aracısız bir yükseltme kullanarak geçirme 5.5, 6.0, 6.5 veya 6.7 çalışan bir veya daha fazla vCenter sunucusu tarafından yönetiliyor olması gerekir.

## <a name="agentless-migration-vcenter-server-permissions"></a>Aracısız geçiş vCenter Server izinleri

**İzinler** | **Ayrıntılar**
--- | --- 
Datastore.Browse | Anlık görüntü oluşturma ve silme sorunlarını gidermek için VM günlük dosyalarını taramaya izin verme.
Datastore.LowLevelFileOperations | Okuma/yazma/delete/yeniden adlandırma işlemlerini anlık görüntü oluşturma ve silme sorunlarını gidermek için veri deposu tarayıcıda izin verin.
VirtualMachine.Configuration.DiskChangeTracking | Etkinleştirme izin vermek veya VM disklerinin, değiştirilen blokları anlık görüntü arasındaki veri çekmek için değişiklik izlemeyi devre dışı bırak
VirtualMachine.Configuration.DiskLease | VMware vSphere Sanal Disk Geliştirme Seti (VDDK) kullanarak diski okumak için bir sanal makine için disk kira işlemleri sağlar.
VirtualMachine.Provisioning.AllowReadOnlyDiskAccess | Bir disk VDDK kullanarak disk okumak için bir VM üzerinde açma izin verin.
VirtualMachine.Provisioning.AllowVirtualMachineDownload  | Günlükleri indirmek ve arıza durumunda sorun gidermek için bir VM ile ilişkili dosyalar salt okunur işlemlere izin verir. 
VirtualMachine.SnapshotManagement.* | Oluşturulması ve yönetimi için çoğaltma VM anlık görüntüleri sağlar. 
Sanal Machine.Interaction.Power kapalı | Sanal makine, azure'a geçiş sırasında kapatılmasına izin verin.


## <a name="agentless-migration-vmware-vm-requirements"></a>Aracısız geçiş VMware VM gereksinimleri

**Destek** | **Ayrıntılar**
--- | ---
**Desteklenen işletim sistemleri** | [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) Azure tarafından desteklenen işletim sistemleri, aracısız geçiş kullanılarak geçirilebilir.
**Azure için gerekli değişiklikleri** | Azure'da çalıştırabilmeniz için önce bazı sanal değişiklikler gerektirebilir. Azure geçişi, aşağıdaki işletim sistemleri için otomatik olarak bu değişiklikleri yapar:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> - SUSE Linux Enterprise Server 12 SP1+<br/> - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS<br/> - Debian 7,8<br/><br/> Diğer işletim sistemleri için el ile geçiş işleminden önce ayarlamalar yapmanız gerekir. İlgili makaleler, bunun nasıl yapılacağı hakkında yönergeler içerir.
**Linux önyükleme** | Makinesiyse adanmış bir bölüme ise, işletim sistemi diskinde bulunması gereken ve birden çok diske yayılma değil.<br/> Makinesiyse (/) kök bölümünde parçası ise, ardından '/' bölüm işletim sistemi diskinde olması ve diğer disklerin span değil.
**UEFI önyüklemesi** | UEFI boot'a sahip sanal makineleri geçiş için desteklenmiyor.
**Şifrelenmiş diskler/birimler** | Şifrelenmiş diskler/birimleri olan sanal makineleri geçiş için desteklenmiyor.
**RDM/geçiş diskleri** | VM, RDM veya doğrudan geçiş diskleri varsa, bu diskleri Azure'a çoğaltılması olmaz.
**NFS** | NFS birimleri Vm'lerde birimleri olarak bağlanabilir çoğaltılması gerekmez.
**Hedef disk** | Vm'leri azure'da yönetilen disklere (standart HHD, premium SSD) yalnızca geçirilebilir.


## <a name="agentless-migration-appliance-requirements"></a>Aracısız geçiş Gereci gereksinimleri


**Destek** | **Ayrıntılar**
--- | ---
**ESXi** | 5\.5 veya sonraki sürümünü çalıştıran bir ESXi ana bilgisayarındaki VM Gereci dağıtılması gerekir.
**Azure geçişi projesi** | Gereçlerden biri tek bir proje ile ilişkili olabilir.
**vCenter Server** | Bir vCenter sunucusu en fazla 10.000 VMware Vm'lerinde gereçlerden biri bulabilir.<br/> Gereçlerden biri, bir vCenter Server'a bağlanabilirsiniz.
**VDDK** | Azure geçişi sunucusu geçişi ile aracısız bir geçiş çalıştırıyorsanız, VMware vSphere Sanal Disk Geliştirme Seti (VDDK) VM gerecinde yüklenmesi gerekir.

## <a name="agentless-migration-url-access-requirements"></a>Aracısız geçiş-URL erişimi gereksinimleri

Azure geçişi gereç, İnternet'e internet bağlantısı gerekir.

- Gereç dağıtırken, Azure geçişi tabloda özetlenen URL'lere bir bağlantı denetimi yapar.
- URL tabanlı bir firewall.proxy kullanıyorsanız, proxy herhangi bir CNAME kayıtları URL'lere aranırken alınan çözümler emin olmak, bu URL'ler erişime izin verin.

**URL** | **Ayrıntılar**  
--- | --- 
*.portal.azure.com | Azure portalında Azure geçişi gidin.
*.windows.net | Azure aboneliğinizde oturum açın.
*.microsoftonline.com | Gerecin Azure geçişi hizmeti ile iletişim kurmak için Active Directory uygulamalar oluşturun.
Management.Azure.com | Gerecin Azure geçişi hizmeti ile iletişim kurmak için Active Directory uygulamalar oluşturun.
dc.services.visualstudio.com | İç izleme için kullanılan uygulama günlüklerini karşıya yükleyin.
*.vault.azure.net | Azure Key Vault'ta ' gizli diziler yönetin.
*.servicebus.windows.net | Gereç ve Azure geçişi hizmeti arasındaki iletişim.
*.discoverysrv.windowsazure.com<br/> *.migration.windowsazure.com<br/> *.hypervrecoverymanager.windowsazure.com | Azure geçişi hizmeti URL'leri için bağlanın.
*.blob.core.windows.net | Veri depolama hesaplarına karşıya yükleyin.


## <a name="agentless-migration-port-requirements"></a>Aracısız geçiş bağlantı noktası gereksinimleri

**cihaz** | **bağlantı**
--- | --- 
Gereç | Giden TCP bağlantı noktası 3389 çoğaltılan verileri Azure'a karşıya yüklemek ve çoğaltma ve geçiş için Azure geçişi ile iletişim kurmak için.
vCenter sunucusu | Anlık görüntüleri çoğaltmayı düzenlemek - anlık görüntüler, veri kopyalama, oluşturma gereç izin vermek için 443 numaralı TCP bağlantı noktasında gelen bağlantılara sürüm
vSphere/EXSI konağı | 902 anlık görüntülerden veri çoğaltmak gereç için TCP bağlantı noktasında gelen. 


## <a name="agent-based-migration-vmware-server-requirements"></a>Aracı tabanlı geçiş VMware sunucusu gereksinimleri

Bu tabloda değerlendirme desteği ve VMware sanallaştırma sunucularını sınırlamaları özetlenmektedir.

**Destek** | **Ayrıntılar**
--- | ---
**vCenter sunucusuna/ESXI** | VMware Vm'lerini geçiş 5.5, 6.0, 6.5 veya 6.7 çalıştıran veya vSphere sürüm 5.5, 6.0, 6.5 veya 6.7 bir ESXI ana bilgisayarı üzerinde çalışan bir veya daha fazla vCenter sunucusu tarafından yönetiliyor olması gerekir.

### <a name="agent-based-migration-vcenter-server-permissions"></a>Aracı tabanlı geçiş vCenter Server izinleri

**İzinler** | **Ayrıntılar**
--- | --- 
Datastore.AllocateSpace | Alanı ayırma, bir VM için bir veri deposundaki izin anlık görüntü, kopyalama ve sanal disk. 
Datastore.Browse | Anlık görüntü oluşturma ve silme sorunlarını gidermek için VM günlük dosyalarını taramaya izin verme.
Datastore.LowLevelFileOperations | Okuma izin, yazma, silme ve anlık görüntü oluşturma/silme sorunlarını gidermek için veri deposu tarayıcı işlemlerinde yeniden adlandırma.
Datastore.UpdateVirtualMachineFiles | Veri deposu resignatured sonra VM dosyalarını bir veri deposu için yollar güncelleştiriliyor izin verin.
Network.AssignNetwork | Bir ağ, VM kaynak atama izin verin.
AssignVirtualMachineToResourcePool | Bir VM'nin kaynak havuzuna atama izin verin.
Resource.MigratePoweredOffVirtualMachine | Farklı bir kaynak havuzu veya ana bilgisayar için bir desteklenen VM kapalı taşınmasına olanak verir.
Resource.MigratePoweredOnVirtualMachine | Farklı bir kaynak havuzu veya ana bilgisayar için güç açık VM VMotion'ı kullanarak taşınmasına olanak verir.
Tasks.CreateTask | Kullanıcı tanımlı bir görev oluşturmak için bir uzantı sağlar.
Tasks.UpdateTask | Kullanıcı tanımlı bir görevi güncelleştirmek için bir uzantı sağlar.
VirtualMachine.Configuration. | VM seçeneklerini ve cihazların yapılandırma sağlar.
Sanal Machine.Interaction.AnswerQuestion | VM durumu geçişleri veya çalışma zamanı hataları ile ilgili sorunları çözümleme sağlar.
Sanal Machine.Interaction.DeviceConnection | Bir sanal makinenin disconnectable sanal cihazları bağlı durumunu değiştirmeyi sağlar.
Sanal Machine.Interaction.ConfigureCDMedia | Bir sanal DVD veya CD-ROM'u cihazın yapılandırmasını sağlar.
Sanal Machine.Interaction.ConfigureFloppyMedia | Bir sanal disket cihazın yapılandırmasını sağlar.
Sanal Machine.Interaction.PowerOff | Sanal makine, azure'a geçiş sırasında kapatılmasına izin verir.
Sanal Machine.Interaction.PowerOn | Bir güç kapalı VM'de destekleyen ve askıya alınmış bir sanal makine sürdürülüyor izin verilir.
Sanal Machine.Interaction.VMwareToolsInstall | Bağlama ve konuk işletim sistemi için bir CD-ROM olarak VMware Araçları CD yükleyici çıkarma izin verir.
VirtualMachine.Inventory.CreateNew | Bir VM oluşturma ve gerekli kaynakların ayrılması izin verir.
VirtualMachine.Inventory.Register | Var olan VM'ye bir vCenter sunucusu ya da konak stok ekleme izin verin.
VirtualMachine.Inventory.Unregister | Bir vcenter Server'dan VMe veya ana bilgisayar Envanter kaydını sağlar.
VirtualMachine.Provisioning.AllowVirtualMachineFilesUpload | Vmx, diskleri, günlükleri ve nvram dahil olmak üzere bir VM ile ilişkili dosyalar üzerinde yazma işlemleri sağlar.
VirtualMachine.Provisioning.AllowVirtualMachineDownload | Okuma işlemleri için sorun giderme günlükleri indirmek için bir VM ile ilişkili dosyaları sağlar.
VirtualMachine.SnapshotManagement.RemoveSnapshot | Anlık görüntü geçmişinden bir anlık görüntü kaldırılmasını sağlar.

## <a name="agent-based-migration-replication-appliance-requirements"></a>Aracı tabanlı geçiş çoğaltma Gereci gereksinimleri

Gereksinimleri [çoğaltma Gereci](migrate-replication-appliance.md) aracı tabanlı geçişi, VMware Vm'leri ve fiziksel için kullanılan Azure geçişi sunucusu geçişi sunucularıyla tabloda özetlenir.

> [!NOTE]
> Azure geçişi hub'ında sağlanan OVA şablonu kullanarak çoğaltma Gereci ayarladığınızda, gereç, Windows Server 2016 çalıştıran ve Destek gereksinimleriyle uyumlu. Bir fiziksel sunucuda el ile çoğaltma Gereci ayarlarsanız gereksinimlerine uygun olduğundan emin olun.



**Bileşen** | **Gereksinim** 
--- | ---
 | **VMware ayarları** (VMware VM Gereci)
**PowerCLI** | [Powerclı 6.0 sürümünün](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) çoğaltma Gereci bir VMware sanal makine üzerinde çalışıyorsa yüklü olması gerekir.
**NIC türü** | VMXNET3 (gereç bir VMware VM ise)
 | **Donanım ayarları** 
CPU çekirdekleri | 8 
RAM | 16 GB
Disk sayısı | Üç: İşletim sistemi diski, işlem sunucusu önbellek diski ve bekletme sürücüsü.
Boş disk alanı (önbellek) | 600 GB
Boş disk alanı (bekletme diski) | 600 GB
**Yazılım ayarları** | 
İşletim sistemi | Windows Server 2016 veya Windows Server 2012 R2
İşletim sistemi yerel ayarı | İngilizce (en-us)
TLS | TLS 1.2 etkinleştirilmesi gerekir.
.NET Framework | (Etkin güçlü şifreleme ile. makinede .NET framework 4.6 veya üzeri yüklü
MySQL | MySQL gerecinde yüklü olması gerekir.<br/> MySQL yüklü olması gerekir. El ile yükleyebilirsiniz veya Site Recovery dağıtımı sırasında Gereci yükleyebilirsiniz. 
Diğer uygulamalar | Diğer uygulamalara çoğaltma gerecinde çalıştırmamalısınız.
Windows Server rolleri | Bu rolleri etkinleştirmeyin: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V 
Grup İlkeleri | Bu grup ilkeleri etkinleştirme: <br> -Komut istemine erişimi engelleyin. <br> -Kayıt defteri düzenleme araçlarına erişimi engelleyin. <br> -Mantıksal dosya ekleri için güven. <br> -Betik yürütmeyi açma. <br> [Daha fazla bilgi edinin](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | -Önceden mevcut olan varsayılan Web sitesi <br> -Önceden var olan Web sitesi/443 numaralı bağlantı noktasını dinlemeye uygulama <br>-Etkinleştir [anonim kimlik doğrulaması](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Etkinleştir [Fastcgı](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarı 
**Ağ ayarları** | 
IP adresi türü | Statik 
Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı) 
NIC türü | VMXNET3 

### <a name="replication-appliance-url-access"></a>Çoğaltma Gereci URL erişimi

Çoğaltma gereç bu URL'lere erişimi olmalıdır.

**URL** | **Ayrıntılar**
--- | ---
\*.backup.windowsazure.com | Çoğaltılmış veri aktarımı ve düzenlemesi için kullanılır
\*.store.core.windows.net | Çoğaltılmış veri aktarımı ve düzenlemesi için kullanılır
\*.blob.core.windows.net | Çoğaltılan verileri depolayan depolama hesabına erişmek için kullanılan
\*.hypervrecoverymanager.windowsazure.com | Çoğaltma yönetimi işlemleri ve düzenleme için kullanılır
https:\//management.azure.com | Çoğaltma yönetimi işlemleri ve düzenleme için kullanılır 
*.services.visualstudio.com | (İsteğe bağlı) telemetri amaçlar için kullanılır
time.nist.gov | Sistem ile genel saat arasındaki saat eşitlemesini denetlemek için kullanılır.
time.windows.com | Sistem ile genel saat arasındaki saat eşitlemesini denetlemek için kullanılır.
https:\//login.microsoftonline.com <br/> https:\//secure.aadcdn.microsoftonline-p.com <br/> https:\//login.live.com <br/> https:\//graph.windows.net <br/> https:\//login.windows.net <br/> https:\//www.live.com <br/> https:\//www.microsoft.com  | OVF Kurulum, bu URL'lere erişimi olmalıdır. Erişim denetimi ve kimlik yönetimi için Azure Active Directory tarafından kullanılır
https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi | MySQL indirmeyi tamamlamak için


#### <a name="mysql-installation-options"></a>MySQL yükleme seçenekleri

MySQL, aşağıdaki yöntemlerden birini kullanarak çoğaltma gerecinde yüklenebilir.

**Yükleme** | **Ayrıntılar**
--- | ---
İndirip el ile yükleyin | MySQL uygulama indir & C:\Temp\ASRSetup klasöre yerleştirin ve sonra el ile yükleyin.<br/> Olarak yüklü MySQL gösterir gereç ayarladığınızda. 
Çevrimiçi yükleme | MySQL yükleyici uygulamanın C:\Temp\ASRSetup klasöre yerleştirin. Gereç yükleyip MySQL indirip yüklemek için tıklayın Kurulum Yükleyici, eklediğiniz kullanır. 
Azure indirme geçirme | Gereç yükleyip için MySQL istenir seçin **yükleyip**.



## <a name="agent-based-migration-vmware-vm-requirements"></a>Aracı tabanlı geçiş VMware VM gereksinimleri

**Destek** | **Ayrıntılar**
--- | ---
**Makine iş yükü** | Azure geçişi destekleyen tüm iş yükleri (örneğin Active Directory, SQL server vb.) geçişini desteklenen bir makinede çalışıyor.
**İşletim sistemleri** | En son bilgiler için gözden [işletim sistemi desteği](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) Site Recovery için. Azure geçişi, aynı VM işletim sistemi desteği sağlar.
**Linux dosya sistemi/Konuk depolama** | En son bilgiler için gözden [Linux dosya sistemi desteği](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) Site Recovery için. Azure geçişi, aynı Linux dosya sistemi desteği vardır.
**Ağ/depolama** | En son bilgiler için gözden [ağ](../site-recovery/vmware-physical-azure-support-matrix.md#network) ve [depolama](../site-recovery/vmware-physical-azure-support-matrix.md#storage) Site Recovery için Önkoşullar. Azure geçişi, aynı ağ depolama gereksinimlerini sağlar.
**Azure gereksinimleri** | En son bilgiler için gözden [Azure ağı](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [depolama](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage), ve [işlem](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute) Site Recovery için gereksinimleri. Azure geçişi, VMware geçiş için aynı gereksinimler vardır.
**Ulaşım hizmeti** | Geçirmek istediğiniz her sanal makinede Mobility Hizmeti Aracısı yüklenmesi gerekir.
**Hedef disk** | Vm'leri azure'da yönetilen disklere (standart HHD, premium SSD) yalnızca geçirilebilir.

   

## <a name="agent-based-migration-url-access-requirements"></a>Aracı tabanlı geçiş-URL erişimi gereksinimleri

VMware Vm'lerinde çalışan Mobility hizmeti, İnternet'e internet bağlantısı gerekir.

- Mobility hizmetini dağıttığınızda, aşağıdaki tabloda özetlenen URL'lere bir bağlantı denetimi yapar.
- URL tabanlı bir firewall.proxy kullanıyorsanız, proxy herhangi bir CNAME kayıtları URL'lere aranırken alınan çözümler emin olmak, bu URL'ler erişime izin verin.

**URL** | **Ayrıntılar**  
--- | --- 
*.portal.azure.com | Azure portalında Azure geçişi gidin.
*.windows.net | Azure aboneliğinizde oturum açın.
*.microsoftonline.com | Gerecin Azure geçişi hizmeti ile iletişim kurmak için Active Directory uygulamalar oluşturun. 
Management.Azure.com | Gerecin Azure geçişi hizmeti ile iletişim kurmak için Active Directory uygulamalar oluşturun.
dc.services.visualstudio.com | İç izleme için kullanılan uygulama günlüklerini karşıya yükleyin.
*.vault.azure.net | Azure Key Vault'ta ' gizli diziler yönetin.
*.servicebus.windows.net | Gereç ve Azure geçişi hizmeti arasındaki iletişim.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Azure geçişi hizmeti URL'leri için bağlanın.
*.blob.core.windows.net | Veri depolama hesaplarına karşıya yükleyin.

## <a name="agent-based-migration-port-requirements"></a>Aracı tabanlı geçiş bağlantı noktası gereksinimleri

**cihaz** | **bağlantı**
--- | --- 
VM'ler | Vm'lerde çalışan mobilite hizmeti ile şirket içi yapılandırma sunucusu HTTPS 443 numaralı bağlantı noktasında gelen çoğaltma yönetimi için iletişim kurar.<br/><br/> Vm'leri, gelen çoğaltma verilerini HTTPS 9443 numaralı bağlantı noktasında (yapılandırma sunucusu makinesinde çalışan) işlem sunucusuna gönderir. Bu bağlantı noktası değiştirilebilir.
Çoğaltma gereç | Çoğaltma gereç HTTPS 443 giden bağlantı noktası üzerinden Azure ile çoğaltma işlemlerini yönetir.
İşlem sunucusu | İşlem sunucusu çoğaltma verilerini alıp, en iyi duruma getirir ve şifreler ve Azure depolamaya bağlantı noktası 443 üzerinden giden gönderir.<br/> Varsayılan olarak, işlem sunucusu çoğaltma gereç üzerinde çalışır.

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Tüm şirket içi Vm'leri Azure'a çoğaltılan bu tabloda özetlenen Azure VM gereksinimleri karşılaması gerekir. Site Recovery çoğaltması için bir önkoşul denetimi çalıştığında, bazı gereksinimleri karşılanmadığı takdirde denetimi başarısız olur.

**Bileşen** | **Gereksinimler** | **Ayrıntılar**
--- | --- | ---
Konuk işletim sistemi | Desteklenen işletim sistemleri için doğrulama [aracısız çoğaltma kullanarak VMware Vm'lerini](#agentless-migration-vmware-vm-requirements)ve [aracı tabanlı çoğaltmayı kullanarak VMware Vm'lerini](#agent-based-migration-vmware-vm-requirements).<br/> Desteklenen bir işletim sisteminde çalışan tüm iş yüklerini geçirebilirsiniz. | Onay desteklenmeyen başarısız olur.
Konuk işletim sistemi mimarisi | 64-bit. | Onay desteklenmeyen başarısız olur.
İşletim sistemi disk boyutu | 2\.048 GB. | Onay desteklenmeyen başarısız olur.
İşletim sistemi disk sayısı | 1\. | Onay desteklenmeyen başarısız olur.
Veri diski sayısı | 64 veya daha az. | Onay desteklenmeyen başarısız olur.
Veri diski boyutu | 4\.095 GB'a kadar | Onay desteklenmeyen başarısız olur.
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir. | 
Paylaşılan VHD | Desteklenmiyor. | Onay desteklenmeyen başarısız olur.
FC diski | Desteklenmiyor. | Onay desteklenmeyen başarısız olur.
BitLocker | Desteklenmiyor. | Bir makine için çoğaltmayı etkinleştirmeden önce BitLocker'ı devre dışı bırakılması gerekir. 
VM adı | 1 63 karakter.<br/> Harfler, sayılar ve kısa çizgilerden oluşabilir.<br/><br/> Makine adı başlamalı ve bir harf veya sayı ile bitmelidir. |  Site recovery'de makine özellikleri değerini güncelleştirin.
Geçiş Windows sonra Bağlan | Geçişten sonra Windows çalıştıran Azure vm'lerine bağlanmak için:<br/> -Geçiş önce şirket içi VM'de RDP'yi etkinleştirin. TCP ve UDP kurallarının **Ortak** profil için eklendiğinden ve tüm profillerde **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalar** içinde RDP’ye izin verildiğinden emin olun.<br/> Siteden siteye VPN erişim için RDP'yi etkinleştirin ve içinde RDP'ye izin ver **Windows Güvenlik Duvarı** -> **izin verilen uygulamalar ve Özellikler** için **etki alanı ve özel** ağlar. Ayrıca, işletim sisteminin SAN ilkesinin değerine ayarlandığını kontrol **OnlineAll**. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135). | 
Geçiş-Linux bağlanma | SSH kullanarak geçişten sonra Azure Vm'lerine bağlanmak için:<br/> Önce geçiş, şirket içi makinede Secure Shell hizmetinin Başlat'a ayarlanır ve güvenlik duvarı kuralları bir SSH bağlantısına izin verdiğinden emin denetleyin.<br/> Yük devretme sonrasında Azure VM'deki ve VM'nin bağlandığı Azure alt ağındaki ağ güvenlik grubu kuralları yük devredilen VM için SSH bağlantı noktasına gelen bağlantılara izin verin. Ayrıca, VM için genel bir IP adresi ekleyin. |  


## <a name="next-steps"></a>Sonraki adımlar

[VMware için hazırlama](tutorial-prepare-vmware.md) değerlendirme ve geçiş.








