---
title: Azure VM yedeklemesi için Azure yedekleme destek matrisi
description: Azure Backup hizmeti ile Azure Vm'lerini yedeklerken, destek ayarları ve sınırlamalar genel bir özetini sağlar.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: 2bd499c3afc423047dda9ff3ad657d110dab282a
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2019
ms.locfileid: "56673656"
---
# <a name="support-matrix-for-azure-vm-backup"></a>Azure VM yedeklemesi için destek matrisi
Kullanabileceğiniz [Azure Backup hizmeti](backup-overview.md) şirket içi makinelerin ve iş yükleri ve Azure sanal makinelerini yedeklemek için. Bu makalede, Azure sanal makineleri (VM'ler) Azure yedekleme ile yedeklemeyi olduğunda destek ayarları ve sınırlamaları özetlenmektedir.

Diğer destek matrisi:

- [Genel destek matrisi](backup-support-matrix.md) Azure Backkup için
- [Destek matrisi](backup-support-matrix-mabs-dpm.md) Microsoft Azure Backup sunucusu/DOM yedekleme
- [Destek matrisi](backup-support-matrix-mars-agent.md) MARS Aracısı ile yedekleme



## <a name="supported-scenarios"></a>Desteklenen senaryolar

Nasıl yedeklenir ve Azure Backup hizmeti ile Azure Vm'lerini geri yükleme aşağıda verilmiştir.


**Senaryo** | **Backup** | **Aracı** |**Geri yükleme** 
--- | --- | --- | ---
**Azure sanal makinelerinin doğrudan yedekleme** | Tüm VM'yi yedekleme  | Aracı, Azure sanal makinesinde gereklidir. Azure Backup yükler ve uzantısı kullanan [Azure VM Aracısı](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) VM'de çalışan. | Şu şekilde geri yükleyin:<br/><br/> - **Temel VM oluşturma**. Bu VM, birden çok IP adresleri gibi özel bir yapılandırma sahipse kullanışlıdır.<br/><br/> - **Sanal makine diskini geri yükleme**. Disk geri yükleyin. Daha sonra mevcut bir VM'ye eklemek veya PowerShell kullanarak diskten yeni bir VM oluşturun.<br/><br/> - **VM diskini değiştirme**. Bir VM var. ve yönetilen diskler (şifrelenmemiş) kullanıyorsa, bir diski geri yükleme ve VM üzerindeki var olan bir diski değiştirmek için kullanın.<br/><br/> - **Belirli dosyaların/klasörlerin geri**. Tüm VM yerine bir VM'den dosyaların/klasörlerin geri yükleyebilirsiniz.  
**Doğrudan yedekleme Azure VM (yalnızca Windows)** | Belirli dosya/klasör/birimi yedekleyin | Yükleme [Microsoft Azure kurtarma Hizmetleri (MARS) aracısı](backup-azure-file-folder-backup-faq.md).<br/><br/> MARS aracısının yedekleme uzantısı için dosya/klasör düzeyinde bir sanal makine yedeklemek Azure VM Aracısı ile birlikte çalıştırabilirsiniz. | Belirli klasörleri/dosyaları geri yükleyin.
**Azure VM'yi yedekleme sunucusuna yedekleme** |  Dosya/klasör/birimleri yedekleyin; Sistem durumu/tam metal dosyalarını; Uygulama verileri için System Center DPM veya Microsoft Azure Backup sunucusu (MAB).<br/><br/> DPM/MABS sonra yedekleme kasası için yedekler | MABS/DPM koruma aracısını VM'ye yükleyin. DPM/MABS MARS Aracısı yüklenir.| Dosya/klasör/birim geri yüklenmesi; Sistem durumu/tam metal dosyalarını; Uygulama verileri. 

Yedekleme hakkında daha fazla bilgi [bir yedekleme sunucusu kullanma](backup-architecture.md#architecture-back-up-to-dpmmabs), ve [destek gereksinimlerini](backup-support-matrix-mabs-dpm.md).


## <a name="supported-backup-actions"></a>Desteklenen yedekleme eylemleri

**Eylem** | **Destek**
--- | ---
Bir Windows Azure VM oluşturduğunuzda, yedeklemeyi etkinleştirme | Desteklenen:  Windows Server 2016 (Core veri merkezi/veri merkezi); Windows Server 2012 R2 veri merkezi; Windows Server 2008 R2 SP1
Bir Linux VM oluşturduğunuzda, yedeklemeyi etkinleştirme | Desteklenen:<br/><br/> - Ubuntu Server: 1710, 1704, 1604 (LTS) 1404 (LTS)<br/><br/> -Red Hat: RHEL 6.7, 6.8, 6.9, 7.2, 7.3, 7.4<br/><br/> -SUSE Linux Enterprise Server: 11 SP4, 12 SP2, 12 SP3<br/><br/> -Debian: 8, 9<br/><br/> - CentOS: 6.9, 7.3<br/><br/> Oracle Linux: 6.7, 6.8, 6.9, 7.2, 7.3
Bu VM kapatma/çevrimdışı/arayan bir VM'yi yedekleme | Destekleniyor.<br/><br/> Kilitlenme ile tutarlı anlık görüntü yalnızca, uygulama-tutarlı değil.
Yönetilen diskler geçiş sonra diskleri yedekleme. | Destekleniyor.<br/><br/> Yedekleme çalışmaya devam eder. İşlem yapmanız gerekmez.
Kaynak grubu kilidi etkinleştirdikten sonra yönetilen diskleri yedekleme | Desteklenmiyor.<br/><br/> Yedek eski kaynak noktaları silinemiyor ve yedeklemeleri geri yükleme noktaları üst sınırına ulaşıldığında başarısız olmaya başlar.
Bir VM için yedekleme ilkesini değiştirme | Destekleniyor.<br/><br/> Yeni ilkede zamanlama ve bekletme ayarlarını kullanarak VM yedeklenen. Saklama ayarları uzatıldıysa, var olan kurtarma noktalarının işaretlenmiş ve tutulur. Azaltılmış, var olan kurtarma noktalarının sonraki temizleme işi çıkarılmasını ve sonunda silindi.
Bir yedekleme işi iptal et | Anlık görüntü işlemi sırasında desteklenir.<br/><br/> Anlık görüntü kasaya aktarıldığında desteklenmiyor. 
Farklı bir bölgeye veya aboneliğe VM'yi yedekleme |  Desteklenmiyor.
(Azure VM uzantısı) üzerinden günlük yedekleme | Günde bir zamanlanmış yedekleme.<br/><br/> En fazla dört isteğe bağlı yedeklemeleri her gün sürebilir.
Yedeklemeler günde (aracılığıyla, MARS Aracısı) | Günde üç zamanlanmış yedeklemeler. 
(DPM/MABS) aracılığıyla günlük yedekleme | Günde iki zamanlanmış yedekleme.
Aylık/yıllık yedekleme   | Azure VM uzantısı ile yedeklerken desteklenmiyor. Yalnızca günlük ve haftalık desteklenir.<br/><br/> Aylık/yıllık Bekletme dönemi için günlük/haftalık yedekleri tutma ilkesi ayarlayabilirsiniz.
Otomatik saat ayarlama | Desteklenmiyor.<br/><br/> Azure yedekleme, VM'yi yedeklerken otomatik ayarlama saat gün ışığından tasarruf değişiklikleri dikkate almaz.<br/><br/>  İlkeyi gereken şekilde el ile değiştirin.
[Karma yedekleme için güvenlik özellikleri](https://docs.microsoft.com/azure/backup/backup-azure-security-feature) |  Özellikleri devre dışı bırakma desteklenmiyor.



## <a name="operating-system-support-windows"></a>İşletim sistemi desteği (Windows)

Tabloda, Windows Azure Vm'lerini yedeklerken, desteklenen işletim sistemleri özetlenmektedir.

**Senaryo** | **İşletim sistemi desteği** 
--- | ---
Azure VM Aracısı uzantısı ile yedekleme | Windows İstemcisi: Desteklenmiyor<br/><br/> Windows Server için: Windows Server 2008 R2 veya üzeri 
MARS Aracısı ile yedekleme | [Desteklenen](backup-support-matrix-mars-agent.md#support-for-direct-backups) işletim sistemleri.
DPM/MABS yedekleyin | Desteklenen işletim sistemleri ile yedekleme için [MABS](backup-mabs-protection-matrix.md) ve [DPM](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-1807).

## <a name="support-for-linux-backup"></a>Linux yedekleme desteği

Linux makineleri yedeklemek istiyorsanız, nelerin desteklendiği aşağıda verilmiştir.

**Eylem** | **Destek**
--- | --- 
Linux Azure VM Aracısı ile Linux Azure Vm'lerini yedekleme | Dosya tutarlı yedekleme.<br/><br/> Uygulamayla tutarlı Yedekleme kullanarak [özel betikler](backup-azure-linux-app-consistent.md).<br/><br/> Geri yükleme sırasında yeni bir VM oluşturun, bir diski geri yükleme ve, bir sanal makine oluşturun veya bir diski geri yükleme ve var olan bir sanal diski değiştirmek için kullanmak için kullanın. Ayrıca tek dosyalar ve klasörler geri yükleyebilirsiniz.
MARS Aracısı ile Linux Azure Vm'lerini yedekleme | Desteklenmiyor.<br/><br/> MARS Aracısı, yalnızca Windows makinelerde yüklenebilir.
DPM/MABS Linux Azure sanal makinelerini yedekleme | Desteklenmiyor.

## <a name="operating-system-support-linux"></a>İşletim sistemi desteği (Linux)

Azure VM Linux yedeklemeleri için Azure Backup, Linux listesini destekler [Azure tarafından desteklenen dağıtımı](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros). Şunlara dikkat edin:

- Azure Backup, Core OS Linux desteklememektedir.
- Azure yedekleme, 32-bit işletim sistemlerini desteklemez.
- Diğer Getir kendi Linux dağıtımları da çalışabilir sürece [Linux için Azure VM Aracısı](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) sanal makinede kullanılabilir ve Python desteklenir.



## <a name="backup-frequency-and-retention"></a>Yedekleme sıklığı ve bekletme

**Ayar** | **Limitler** 
--- | --- 
(Makine/iş yükü korumalı örnek başına en fazla kurtarma noktası | 9999
Bir kurtarma noktası için en fazla süre sonu zamanı | Sınırsız
En fazla yedekleme sıklığını kasasına (Azure VM uzantısı) | Günde bir kez
En fazla yedekleme sıklığını kasasına (MARS Aracısı) | Günde üç yedekleme.
DPM/MABS en fazla yedekleme sıklığı | SQL Server için 15 dakikada bir<br/><br/> Bir saatte diğer iş yükleri için.
Kurtarma noktası bekletme | Günlük, haftalık, aylık, yıllık.
En uzun bekletme süresi | Yedekleme sıklığına bağlıdır.
DPM/MABS diskteki kurtarma noktaları | dosya sunucuları, uygulama sunucuları için 448 için 64.<br/><br/> Bant kurtarma noktaları için şirket içi DPM sınırsızdır.


## <a name="supported-restore-methods"></a>Desteklenen geri yükleme yöntemleri

**Geri yükleme yöntemi** | **Ayrıntılar**
--- | ---
**Yeni VM oluşturma** | Geri yükleme işlemi sırasında bir VM oluşturabilirsiniz. <br/><br/> Bu seçenek, çalışmaya temel VM alır. VM adı, kaynak grubu, sanal ağ, alt ağ ve depolama belirtebilirsiniz.  
**Bir diski geri yükleme** | Bir diski geri yükleme ve bir VM oluşturmak için bunu kullanın<br/><br/> Bu seçeneği belirlediğinizde, Azure Backup kasasından verileri bir depolama hesabına kopyalar. Geri yükleme işi, bir şablon oluşturur. Bu şablonu indirin ve özel sanal makine ayarlarını belirtmek için kullanın ve bir VM oluşturun.<br/><br/> Bu seçenek daha fazla ayarları belirtmenize olanak tanır, bir VM oluşturmak için önceki seçeneği.<br/><br/>
**Var olan bir diski Değiştir** | Bir diski geri yükleme ve sonra bir sanal makine üzerinde şu anda var olan bir diski değiştirmek için geri yüklenen diski kullanın.
**Dosyaları geri yükleme** | Seçilen kurtarma noktasından dosyaları kurtarabilirsiniz. Sanal makine disk kurtarma noktasından bağlamak için bir komut dosyası indirin. Ardından kurtarmak ve işiniz bittiğinde, disk çıkarmak istediğiniz dosyaları/klasörleri bulmak için disk birimleri göz atın.

## <a name="support-for-file-level-restore"></a>Dosya düzeyinde geri yükleme desteği

**Geri yükleme** | **Destekleniyor**
--- | --- 
İşletim sistemleri arasında dosyaları geri yükleme | Yedeklenen sanal makine olarak aynı (veya uyumlu) işletim sistemi olan herhangi bir makinede dosyaları geri yükleyebilirsiniz. bkz: [uyumlu işletim sistemi tablo](backup-azure-restore-files-from-vm.md#system-requirements).
Klasik sanal makineleri geri yükleme dosyaları | Desteklenmiyor. 
Şifrelenmiş Vm'leri geri yükleme dosyaları | Desteklenmiyor.
Ağ kısıtlaması olan depolama hesaplarından dosyaları geri yükleme | Desteklenmiyor.
Windows depolama alanları kullanarak Vm'leri geri yükleme dosyaları | Geri yükleme, aynı VM üzerinde desteklenmiyor.<br/><br/> Bunun yerine, uyumlu bir sanal makine dosyaları geri yükle.
LVM'yi/RAID dizileri kullanarak Linux VM dosyaları geri yükle | Geri yükleme, aynı VM üzerinde desteklenmiyor.<br/><br/> Uyumlu bir VM'de geri yükleyin.
Özel ağ ayarlarını içeren dosyaları geri yükleme | Geri yükleme, aynı VM üzerinde desteklenmiyor. <br/><br/> Uyumlu bir VM'de geri yükleyin.

## <a name="support-for-vm-management"></a>VM yönetimi için destek

Tablo ekleme veya değiştirme VM diskleri gibi sanal makine yönetim görevleri sırasında yedekleme desteği özetler.

**Geri yükleme** | **Destekleniyor** 
--- | --- 
Bölge/abonelik/bölge geri yükleyin. | Desteklenmiyor 
Mevcut bir VM'ye geri yükleme | Kullanımı, disk seçeneği değiştirin.
Azure depolama hizmeti şifrelemesi (SSE) için etkinleştirilmiş depolama hesabıyla diski geri yükleme | Desteklenmiyor.<br/><br/> SSE etkin olmayan bir hesap geri yükleyin.
Karma depolama hesaplarına geri yükleme | Desteklenmiyor.<br/><br/> Depolama hesabı türüne bağlı olarak, geri yüklenen tüm diskler, premium veya standart ve karışık değil olacaktır. 
ZRS kullanan depolama hesabına geri yükleme | Desteklenmiyor
VM bir kullanılabilirlik kümesine doğrudan geri yükleme | Yönetilen diskler için disk geri yükleme ve şablonda kullanılabilirlik kümesi seçeneğini kullanın.<br/><br/> Yönetilmeyen diskler için desteklenmiyor. Yönetilmeyen diskler için disk geri yükleme ve sonra kullanılabilirlik kümesine bir VM oluşturun.
Yönetilmeyen sanal makinelerin yedeklenmesi, yönetilen yükselttikten sonra geri yükleme | Destekleniyor.<br/><br/> Diskleri geri yükle ve ardından yönetilen bir sanal makine oluşturun.
VM'yi yönetilen disklere geçirilmeden önce geri yükleme noktası için VM geri yükleme | Destekleniyor.<br/><br/> Yönetilmeyen diskler (varsayılan) geri yükleme, geri yüklenen diski yönetilen diske dönüştürme ve yönetilen disklerle bir VM oluşturun.
Silinmiş bir sanal makine geri yükleyin. | Destekleniyor.<br/><br/> VM bir kurtarma noktasından geri yükleyebilirsiniz.
Bir etki alanı denetleyicisi (DC) Portalı aracılığıyla çoklu DC yapılandırmasının bir parçası olan VM geri yükleme | Diski geri yükleme ve PowerShell kullanarak VM oluşturma desteklenmiyor.
Farklı bir vnet'te VM geri yükleme |  Destekleniyor.<br/><br/> Sanal ağ, aynı abonelik ve aynı bölgede olması gerekir.




## <a name="vm-compute-support"></a>VM işlem desteği

**İşlem** | **Destek** 
--- | --- 
VM boyutu |   Herhangi bir Azure VM boyutu en az 2 CPU Çekirdeği ve 1 GB RAM.<br/><br/> [Daha fazla bilgi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) 
Vm'leri yedekleme [kullanılabilirlik kümeleri](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability#availability-sets) | Destekleniyor.<br/><br/> Hızlı bir şekilde sanal makine oluşturma seçeneğini kullanarak bir kullanılabilir kümesindeki bir VM'nin geri yükleyemezsiniz. Bunun yerine, sanal Makineyi geri yüklerken ya da bir diski geri yükleme ve bir VM'yi dağıtmak veya bir diski geri yükleme ve var olan bir diski değiştirmek için kullanmak için kullanmak gerekir. 
Vm'leri yedekleme [kullanılabilirlik alanları](https://docs.microsoft.com/azure/availability-zones/az-overview) |  Desteklenmiyor.  
Geri ile dağıtılan Vm'leri yedekleme [karma kullanım Avantajı'nı (HUB)](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) | Destekleniyor.
Geri dağıtılan Vm'leri yedekleme bir [ölçek kümesi](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview) |   Desteklenmiyor   
Geri üzerinden dağıtılan Vm'leri yedekleme [Azure Marketi](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?filters=virtual-machine-images)<br/><br/> (Microsoft tarafından yayımlanan üçüncü taraf) |   Destekleniyor.<br/><br/> Sanal Makineyi desteklenen bir işletim sistemi çalıştırmalıdır.<br/><br/> Sanal makine dosyalarını kurtarırken, yalnızca bir uyumlu işletim sistemine (değil bir önceki veya sonraki işletim sistemi) geri yükleyebilirsiniz.
Özel bir görüntü (üçüncü taraf) üzerinden dağıtılan Vm'leri yedekleme |    Destekleniyor.<br/><br/> Sanal Makineyi desteklenen bir işletim sistemi çalıştırmalıdır.<br/><br/> Sanal makine dosyalarını kurtarırken, yalnızca bir uyumlu işletim sistemine (değil bir önceki veya sonraki işletim sistemi) geri yükleyebilirsiniz.
Sanal makinelerini Azure'a geçiş   | Destekleniyor.<br/><br/> Sanal makineyi yedeklemek için geçirilen makinede VM Aracısı yüklenmelidir. 



## <a name="vm-storage-support"></a>VM depolama desteği

**Bileşen** | **Destek**
--- | ---
Azure VM veri diski | Bir VM 16 ya da daha az veri diskleri ile yedekleyin.
Veri diski boyutu | 4095 GB'a kadar tek bir disk olabilir.<br/><br/> Azure VM yedeklemesi (anında geri yükleme bilinir) en son sürümünü çalıştırıyorsanız, en fazla 4 TB disk boyutları desteklenmektedir. [Daha fazla bilgi edinin](backup-instant-restore-capability.md).
Depolama türü | Standart HDD, SSD standart, premium SSD <br/><br/> Standart SSD (anında geri yükleme bilinir) Azure VM yedeklemesi en son sürümünü kullanıyorsanız desteklenir; standart SSD desteklenmiyor. [Daha fazla bilgi edinin](backup-instant-restore-capability.md).
Yönetilen diskler | Desteklenen
Şifrelenmiş diskler | Destekleniyor.<br/><br/> Azure Disk şifrelemesi (ADE) ile etkin olarak, azure sanal makineler (ile veya Azure AD uygulamasını olmadan) yedeklenebilir.<br/><br/> Dosya/klasör düzeyinde şifrelenmiş Vm'leri geri alınamaz. Tüm VM kurtarmanız gerekecektir.<br/><br/> Azure Backup tarafından korunan VM'ler üzerinde şifrelemeyi etkinleştirebilirsiniz.
Yazma Hızlandırıcısı etkinleştirilmiş disklerle | Desteklenmiyor.<br/><br/> Azure VM yedeklemesi en son sürümünü çalıştırıyorsanız, (olarak bilinen [anında geri yükleme](backup-instant-restore-capability.md)), yazma Hızlandırıcısı etkinleştirilmiş yedekten ile diskleri dışarıda tutabilirsiniz.
Yinelenenleri kaldırılmış diskleri yedekleme | Desteklenmiyor.
Korumalı bir VM'ye disk ekleme | Destekleniyor.
Korumalı VM diski yeniden boyutlandırma | Destekleniyor.

## <a name="vm-network-support"></a>VM ağ desteği


**Bileşen** | **Destek**
--- | ---
NIC sayısı | NIC sayısı üst sınırı için belirli bir Azure VM boyutu desteklenen en fazla.<br/><br/> Geri yükleme işlemi sırasında oluşturulan VM NIC oluşturulur.<br/><br/> Koruma etkinleştirildiğinde, geri yüklenen VM üzerindeki NIC sayısı VM üzerindeki NIC sayısını yansıtır. NIC etkinleştirdikten sonra kaldırma sayısı etkilemez.
İç/dış yük dengeleyici |   Destekleniyor. <br/></br> [Daha fazla bilgi edinin](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations) özel ağ ayarlarıyla Vm'leri geri yükleme hakkında.
Birden çok ayrılmış IP adresi |  Destekleniyor. <br/></br> [Daha fazla bilgi edinin](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations) özel ağ ayarlarıyla Vm'leri geri yükleme hakkında.
Vm'leri birden fazla ağ bağdaştırıcısı  | Destekleniyor. <br/></br> [Daha fazla bilgi edinin](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations) özel ağ ayarlarıyla Vm'leri geri yükleme hakkında.
Genel IP adreslerine sahip VM'ler    | Destekleniyor.<br/><br/> Var olan bir genel IP adresini NIC ile ilişkilendirin veya bir adresi oluşturun ve geri yükleme tamamlandıktan sonra NIC ile ilişkilendirin gerekir.
NIC/alt ağ üzerinde ağ güvenlik grubu (NSG). |   Destekleniyor.
Ayrılmış IP adresi (statik) | Desteklenmiyor.<br/><br/> Ayrılmış bir IP adresi ve tanımlanmış uç nokta sahip bir VM yedekleyemezsiniz.
Dinamik IP adresi |    Destekleniyor.<br/><br/> Kaynak NIC VM dinamik IP adresleme kullanıyorsa, geri yüklenen VM üzerindeki NIC varsayılan olarak güncelleştirilecektir.
Traffic Manager | Desteklenen<br/><br/> Yedeklenen VM'ye trafik Yöneticisi'nde ise, geri yüklenen VM aynı Traffic Manager için el ile eklemeniz gerekir. 
Azure DNS | Destekleniyor.
Özel DNS |    Destekleniyor.
HTTP Ara sunucusu üzerinden giden bağlantı | Destekleniyor.<br/><br/> Kimliği doğrulanmış bir ara sunucular desteklenmez. 
Sanal ağ hizmet uç noktaları  | Destekleniyor.<br/><br/> Güvenlik Duvarı ve sanal ağ, depolama hesabı ayarlarını tüm ağlardan erişime izin. 



## <a name="vm-securityencryption-support"></a>VM güvenliği/şifreleme desteği

Azure Backup, aktarım sırasında ve bekleyen veriler için Şifreleme destekler:

Ağ trafiği azure'a:
- Kurtarma Hizmetleri kasası sunucularından yedekleme trafiği, Gelişmiş Şifreleme Standardı 256 kullanılarak şifrelenir.
- Yedekleme verileri güvenli bir HTTPS bağlantısı üzerinden gönderilir.
- Yedekleme verileri, şifreli biçimde kurtarma Hizmetleri kasasında depolanır.
- Yalnızca bu verilerin kilidini açmak için parola gerekir. Microsoft, herhangi bir noktada yedekleme verilerinin şifresini çözemez.
    > [!WARNING]
    > Yalnızca kasası ayarladıktan sonra şifreleme anahtarına erişime sahip. Microsoft hiçbir zaman bir kopyasını tutar ve erişim anahtarına sahip değil. Anahtarın kaybedilmesi durumunda Microsoft Yedekleme verilerini kurtaramaz.
Veri güvenliği:
- Azure Vm'lerini yedeklerken şifrelemesi ayarlamak gereken *içinde* sanal makine.
- Azure Backup, Windows sanal makinelerde BitLocker, Linux sanal makinelerde ise **dm-crypt** kullanan Azure Disk Şifrelemesi özelliğini destekler.
- Azure Backup arka uçta bekleyen verileri koruyan [Azure Depolama Hizmeti şifrelemesini](../storage/common/storage-service-encryption.md) kullanır.


**Makine** | **Yoldaki** | **Bekleyen**
--- | --- | ---
DPM/MABS olmadan şirket içi Windows makineler | ![Evet][green] | ![Evet][green]
Azure VM’leri | ![Evet][green] | ![Evet][green] 
DPM ile şirket içi/Azure Vm'leri | ![Evet][green] | ![Evet][green]
MABS ile şirket içi/Azure Vm'leri | ![Evet][green] | ![Evet][green]



## <a name="vm-compression-support"></a>VM sıkıştırma desteği

Aşağıdaki tabloda özetlendiği gibi yedekleme sıkıştırma, yedekleme trafiğinin destekler. Şunlara dikkat edin:

- Bu trafiğin sıkıştırılması gerekmez, yani Azure Vm'leri için depolama ağı aracılığıyla doğrudan Azure depolama hesabından veri VM uzantısı okur.
- DPM veya MABS kullanıyorsanız, DPM/bant genişliğinden tasarruf etmek MABS için yedeklenir önce verileri sıkıştırabilirsiniz. 

**Makine** | **Sıkıştırma MABS/DPM sunucusuna (TCP)** | **(HTTPS) kasaya Sıkıştır**
--- | --- | ---
DPM/MABS olmadan şirket içi Windows makineler | NA | Evet
Azure VM’leri | NA | NA
DPM ile şirket içi/Azure Vm'leri | ![Evet][green] | ![Evet][green]
MABS ile şirket içi/Azure Vm'leri | ![Evet][green] | ![Evet][green]




## <a name="next-steps"></a>Sonraki adımlar

- [Azure VM'lerini yedekleme](backup-azure-arm-vms-prepare.md)
- [Doğrudan Windows makinelerini yedekleme](tutorial-backup-windows-server-to-azure.md), bir yedek sunucu olmadan.
- [MABS kümesi](backup-azure-microsoft-azure-backup.md) yedekleme Azure ve ardından Yedekleme iş yükleri için MABS için.
- [DPM ayarlama](backup-azure-dpm-introduction.md) Azure'a ve ardından iş yüklerini yedekleme DPM yedekleme.

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png

