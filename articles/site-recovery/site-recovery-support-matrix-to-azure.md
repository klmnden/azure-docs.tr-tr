---
title: "Azure'a çoğaltmak için azure Site Recovery destek matrisi | Microsoft Docs"
description: "Azure Site Recovery için desteklenen işletim sistemleri ve bileşenler özetler"
services: site-recovery
documentationcenter: 
author: Rajani-Janaki-Ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/30/2017
ms.author: rajanaki
ms.openlocfilehash: 1c65c32457c2311304abf07983f698289f67bbc2
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="azure-site-recovery-support-matrix-for-replicating-from-on-premises-to-azure"></a>Şirket içinden Azure'a çoğaltmak için azure Site Recovery destek matrisi


Bu makalede, çoğaltma ve kurtarma için Azure, Azure Site Recovery için desteklenen yapılandırmalar ve bileşenleri özetlenmektedir. Azure Site Recovery gereksinimleri hakkında daha fazla bilgi için bkz: [Önkoşullar](site-recovery-prereq.md).

> [!NOTE]
> Site Recovery sağlayıcısı ve aracı uyumluluk için destek matrisi Güncelleştirmeler ile elde etmek için en son sürümüne güncelleştirdiğinizden emin olun.


## <a name="support-for-deployment-options"></a>Dağıtım seçenekleri için destek

**Dağıtım** | **VMware/fiziksel sunucu** | **Hyper-V (ile arama/olmadan Sanal Makine Yöneticisi)** |
--- | --- | ---
**Azure portalı** | VMware Vm'leri Azure storage, Azure Resource Manager veya Klasik depolama ve ağ ile şirket içi.<br/><br/> Resource Manager tabanlı ya da Klasik VM'ler için yük devretme. | Hyper-V sanal makineleri Azure storage, Resource Manager veya Klasik depolama ve ağ ile şirket içi.<br/><br/> Resource Manager tabanlı ya da Klasik VM'ler için yük devretme.
**Klasik portal** | Yalnızca Bakım modunda. Yeni kasa oluşturulamıyor. | Yalnızca Bakım modunda.
**PowerShell** | Desteklenen | Desteklenen


## <a name="support-for-datacenter-management-servers"></a>Veri Merkezi Yönetimi sunucuları desteği

### <a name="virtualization-management-entities"></a>Sanallaştırma Yönetimi varlıklar

**Dağıtım** | **Destek**
--- | ---
**VMware sanal/fiziksel sunucu** | vCenter 6.5, 6.0 veya 5.5
**Hyper-V (Virtual Machine Manager ile)** | System Center Virtual Machine Manager 2016 ve System Center Virtual Machine Manager 2012 R2

  >[!Note]
  > Windows Server 2016 ve 2012 R2 konakları bileşimi ile System Center Virtual Machine Manager 2016 bulut şu anda desteklenmiyor.
  > Yükseltme 2016 için varolan bir SCVMM 2012 R2'in içeren yapılandırmalar şu anda desteklenmiyor.
### <a name="host-servers"></a>Ana bilgisayar sunucuları

**Dağıtım** | **Destek**
--- | ---
**VMware sanal/fiziksel sunucu** | vSphere 6.5, 6.0, 5.5
**Hyper-V (ile arama/olmadan Sanal Makine Yöneticisi)** | Windows Server 2016, en son güncelleştirmeleri içeren Windows Server 2012 R2.<br></br>SCVMM kullandıysanız, Windows Server 2016 konakları SCVMM 2016 tarafından yönetilmelidir.


  >[!Note]
  >Windows Server 2016 ve 2012 R2 çalıştıran konaklar karıştırır bir Hyper-V sitesi şu anda desteklenmiyor. Bir Windows Server 2016 konaktaki VM'ler için farklı bir konuma kurtarma şu anda desteklenmiyor.

## <a name="support-for-replicated-machine-os-versions"></a>Çoğaltılan makinenin işletim sistemi sürümleri için destek

Korunan sanal makineler karşılamalıdır [Azure gereksinimleri](#failed-over-azure-vm-requirements) Azure'a çoğaltırken.
Aşağıdaki tabloda, Azure Site Recovery kullanırken çeşitli dağıtım senaryolarında çoğaltılmış işletim sistemi desteği özetler. Bu destek, sözü edilen işletim sisteminde çalışan herhangi bir iş yükü için geçerlidir.

 **VMware/fiziksel sunucu** | **Hyper-V (ile/VMM olmadan)** |
--- | --- |
64-bit Windows Server 2016 (Sunucu Çekirdeği, masaüstü deneyimi olan sunucu)\*, Windows Server 2012 R2, Windows Server 2012, Itanium tabanlı sistemler için Windows Server 2008 R2 ile en az SP1<br/><br/> Red Hat Enterprise Linux: 5.2 için 5.11 ya, 6.1 için 6.9, 7.0 için 7.3 <br/><br/>CentOS: 5.2 için 5.11 ya, 6.1 için 6.9, 7.0 için 7.3 <br/><br/>Ubuntu 14.04 LTS server[ (çekirdek sürümleri desteklenir)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Ubuntu 16.04 LTS server[ (çekirdek sürümleri desteklenir)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Debian 7 <br/><br/>Debian 8<br/><br/>Oracle Enterprise Linux 6.4, Red Hat uyumlu çekirdek ya da kesilemeyen kurumsal çekirdek sürüm 3 (UEK3) çalıştıran 6.5 <br/><br/>SUSE Linux Enterprise Server 11 SP3 <br/><br/>SUSE Linux Enterprise Server 11 SP4 <br/>(Makineler SLES 11 SP4 ' SLES 11 SP3 çoğaltılan yükseltme desteklenmez. Çoğaltılmış bir makineden SLES 11 SP4 ' SLES 11SP3 yükseltildiyse, çoğaltmayı devre dışı bırakın ve makine yükseltme sonrası yeniden korumak gerekir.) | Bir konuk işletim sistemi [Azure tarafından desteklenen](https://technet.microsoft.com/library/cc794868.aspx)

>[!NOTE]
>
> \*Windows Server 2016 Nano Server desteklenmiyor.

>[!IMPORTANT]
>(VMware/fiziksel sunucularını Azure'a çoğaltma için geçerlidir)
>
> Red Hat Enterprise Linux Server 7 + ve CentOS 7 + sunucularda, çekirdek sürüm 3.10.0-514 Azure Site Recovery Mobility hizmeti 9.8 sürümünden itibaren desteklenmektedir.<br/><br/>
> Mobility hizmetinin 9.8 sürümünden daha düşük bir sürümü ile 3.10.0-514 çekirdek müşteriler çoğaltmasını devre dışı bırakın, Mobility hizmeti sürümü 9.8 sürüme güncelleştirin ve ardından çoğaltma işlemini yeniden etkinleştirmek için gereklidir.


### <a name="supported-ubuntu-kernel-versions-for-vmwarephysical-servers"></a>VMware/fiziksel sunucular için desteklenen Ubuntu çekirdek sürümleri

**Sürüm** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
14.04 LTS | 9.9 | 3.13.0-24-Generic 3.13.0-117-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-75-generic 4.4.0-21-Generic |
14.04 LTS | 9.10 | 3.13.0-24-Generic 3.13.0-121-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-81-generic 4.4.0-21-Generic |
14.04 LTS | 9.11 | 3.13.0-24-Generic 3.13.0-128-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-91-generic 4.4.0-21-Generic |
14.04 LTS | 9.12 | 3.13.0-24-Generic 3.13.0-132-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-96-generic 4.4.0-21-Generic |
16.04 LTS | 9.10 | 4.4.0-21-Generic 4.4.0-81-generic için<br/>4.8.0-34-Generic 4.8.0-56-generic için<br/>4.10.0-24-generic 4.10.0-14-Generic |
16.04 LTS | 9.11 | 4.4.0-21-Generic 4.4.0-91-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-32-generic 4.10.0-14-Generic |
16.04 LTS | 9.12 | 4.4.0-21-Generic 4.4.0-96-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-35-generic 4.10.0-14-Generic |

## <a name="supported-file-systems-and-guest-storage-configurations-on-linux-vmwarephysical-servers"></a>Desteklenen dosya sistemleri ve Linux (VMware/fiziksel sunucuları) üzerinde Konuk depolama yapılandırmaları

Aşağıdaki dosya sistemleri ve depolama yapılandırması yazılım VMware veya fiziksel sunucuları üzerinde çalışan Linux sunucularda desteklenir:
* Dosya sistemleri: ext3, ext4, ReiserFS (Suse Linux Enterprise Server yalnızca), XFS
* Birim Yöneticisi: LVM2
* Çok yollu yazılım: cihaz Eşleyici

Paravirtualized depolama aygıtları (paravirtualized sürücüleri tarafından dışarı aktarılan) desteklenmez.<br/>
Birden çok sıra blok g/ç cihazları desteklenmez.<br/>
HP CCISS depolama denetleyicisi ile fiziksel sunucuları desteklenmez.<br/>

>[!Note]
> Linux sunuculara aşağıdaki dizinleri (varsa ayrı bölümleri/dosya-sistemleri ayarlanmış) tüm kaynak sunucudaki aynı disk (işletim sistemi disk) üzerinde olmalıdır: / (kök), / Boot/usr, /usr/local, /var, / etc<br/><br/>
> Meta veri sağlama toplamı gibi XFS bağlanan dosya sistemlerinin XFSv5 özellikleri mobilite hizmetinin 9.10 sürümünden başlayarak desteklenir. XFSv5 özellikleri kullanıyorsanız, Mobility hizmeti sürümü 9.10 veya sonraki sürümünü çalıştırdığınızdan emin olun. Süper blok XFS kullanarak bölümün denetlemek için xfs_info yardımcı programını kullanabilirsiniz. Ftype 1 olarak ayarlanırsa, XFSv5 özellikleri kullanılıyor.
>


## <a name="support-for-network-configuration"></a>Ağ yapılandırması için destek
Aşağıdaki tablolarda ağ yapılandırma desteği Azure'a çoğaltmak için Azure Site Recovery kullanan çeşitli dağıtım senaryolarında özetlenmektedir.

### <a name="host-network-configuration"></a>Konak ağ yapılandırması

**Yapılandırma** | **VMware/fiziksel sunucu** | **Hyper-V (ile arama/olmadan Sanal Makine Yöneticisi)**
--- | --- | ---
NIC ekibi oluşturma | Evet<br/><br/>Fiziksel makineleri çoğaltıldığında desteklenmiyor| Evet
VLAN | Evet | Evet
IPv4 | Evet | Evet
IPv6 | Hayır | Hayır

### <a name="guest-vm-network-configuration"></a>Konuk VM ağ yapılandırması

**Yapılandırma** | **VMware/fiziksel sunucu** | **Hyper-V (ile arama/olmadan Sanal Makine Yöneticisi)**
--- | --- | ---
NIC ekibi oluşturma | Hayır | Hayır
IPv4 | Evet | Evet
IPv6 | Hayır | Hayır
Statik IP (Windows) | Evet | Evet
Statik IP (Linux) | Evet <br/><br/>Sanal makineler yeniden çalışma üzerinde DHCP kullanmak üzere yapılandırılmış  | Hayır
Multi-NIC | Evet | Evet

### <a name="failed-over-azure-vm-network-configuration"></a>Başarısız üzerinden Azure VM ağ yapılandırması

**Azure ağı** | **VMware/fiziksel sunucu** | **Hyper-V (ile arama/olmadan Sanal Makine Yöneticisi)**
--- | --- | ---
Express Route | Evet | Evet
ILB | Evet | Evet
ELB | Evet | Evet
Traffic Manager | Evet | Evet
Multi-NIC | Evet | Evet
Ayrılmış IP | Evet | Evet
IPv4 | Evet | Evet
Kaynak IP koru | Evet | Evet
Sanal ağ hizmet uç noktaları (Azure Storage güvenlik duvarları ve sanal ağlar) | Hayır | Hayır


## <a name="support-for-storage"></a>Depolama için destek
Aşağıdaki tablolarda depolama yapılandırma desteği Azure'a çoğaltmak için Azure Site Recovery kullanan çeşitli dağıtım senaryolarında özetlenmektedir.

### <a name="host-storage-configuration"></a>Ana bilgisayar depolama yapılandırması

**Yapılandırma** | **VMware/fiziksel sunucu** | **Hyper-V (ile arama/olmadan Sanal Makine Yöneticisi)**
--- | --- | --- | ---
NFS | VMware için Evet<br/><br/> Fiziksel sunucuları için Hayır'ı | Yok
SMB 3.0 | Yok | Evet
SAN (İSCSI) | Evet | Evet
Çok yollu (MPIO)<br></br>İle test: Microsoft DSM, EMC PowerPath 5.7 SP4, EMC PowerPath DSM CLARiiON için | Evet | Evet

### <a name="guest-or-physical-server-storage-configuration"></a>Konuk veya fiziksel sunucu depolama yapılandırması

**Yapılandırma** | **VMware/fiziksel sunucu** | **Hyper-V (ile arama/olmadan Sanal Makine Yöneticisi)**
--- | --- | ---
VMDK | Evet | Yok
VHD/VHDX | Yok | Evet
Gen 2 VM | Yok | Evet
EFI/UEFI'YE| Hayır | Evet
Küme diskini paylaşılan | Hayır | Hayır
Şifrelenmiş disk | Hayır | Hayır
NFS | Hayır | Yok
SMB 3.0 | Hayır | Hayır
RDM | Evet<br/><br/> Fiziksel sunucuları için yok | Yok
Disk > 1 TB | Evet<br/><br/>Fazla 4095 GB | Evet<br/><br/>Fazla 4095 GB
Disk 4K mantıksal ve 4 k fiziksel kesim boyutu | Evet | Nesil 1 VM'ler için desteklenmiyor<br/><br/>Nesil 2 sanal makineleri için desteklenmez.
4K mantıksal disk ve 512 bayt fiziksel kesim boyutu | Evet |  Evet
Şeritli disk > 1 TB birimle<br/><br/> LVM mantıksal birim yönetimi | Evet | Evet
Depolama alanları | Hayır | Evet
Sık kullanılan Ekle/Kaldır disk | Hayır | Hayır
Diski hariç tutma | Evet | Evet
Çok yollu (MPIO) | Yok | Evet

**Azure depolama alanı** | **VMware/fiziksel sunucu** | **Hyper-V (ile arama/olmadan Sanal Makine Yöneticisi)**
--- | --- | ---
LRS | Evet | Evet
GRS | Evet | Evet
RA-GRS | Evet | Evet
Seyrek erişimli depolama | Hayır | Hayır
Sık erişimli depolama| Hayır | Hayır
Blok Blobları | Hayır | Hayır
Rest(SSE) şifreleme| Evet | Evet
Premium depolama | Evet | Evet
İçeri/dışarı aktarma hizmeti | Hayır | Hayır
Sanal ağ hizmeti hedef depolama alanında yapılandırılmış uç noktaları (Azure Storage güvenlik duvarları ve sanal ağlar) hesap veya depolama hesabı çoğaltma verilerini depolamak için kullanılan önbellek | Hayır | Hayır
Genel amaçlı V2 depolama hesapları (her ikisini de sık erişimli ve seyrek katman) | Hayır | Hayır


## <a name="support-for-azure-compute-configuration"></a>Azure işlem yapılandırma desteği

**Özellik işlem** | **VMware/fiziksel sunucu** | **Hyper-V (ile arama/olmadan Sanal Makine Yöneticisi)**
--- | --- | ---
Kullanılabilirlik kümeleri | Evet | Evet
HUB | Evet | Evet  
Yönetilen diskler | Evet | Evet<br/><br/>Şirket içi yeniden çalışma yönetilen disklerle Azure VM'den şu anda desteklenmiyor.

## <a name="failed-over-azure-vm-requirements"></a>Başarısız üzerinden Azure VM gereksinimleri

Azure tarafından desteklenen herhangi bir işletim sistemi çalıştıran sanal makineleri ve fiziksel sunucuları çoğaltmak için Site Recovery’yi dağıtabilirsiniz. Buna çoğu Windows ve Linux sürümü dahildir. Çoğaltmak istediğiniz sanal makineleri Azure'a çoğaltılırken aşağıdaki Azure gereksinimlere uymalıdır şirket içi.

**Varlık** | **Gereksinimleri** | **Ayrıntılar**
--- | --- | ---
**Konuk işletim sistemi** | Hyper-V Azure çoğaltma: Site Recovery tüm işletim sistemlerini destekler [Azure tarafından desteklenen](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> VMware ve fiziksel sunucu çoğaltma için: Windows ve Linux denetleyin [önkoşulları](site-recovery-vmware-to-azure-classic.md) | Önkoşul denetimi desteklenmeyen başarısız olur.
**Konuk işletim sistemi mimarisi** | 64 bit | Önkoşul denetimi desteklenmeyen başarısız olur
**İşletim sistemi disk boyutu** | 2048 çoğaltma yapıyorsanız GB'a kadar **VMware Vm'lerini veya fiziksel sunucuları azure'a**.<br/><br/>İçin fazla 2048 GB **Hyper-V nesil 1** VM'ler.<br/><br/>Fazla için 300 GB **Hyper-V 2. nesil sanal makineleri**.  | Önkoşul denetimi desteklenmeyen başarısız olur
**İşletim sistemi disk sayısı** | 1 | Önkoşul denetimi desteklenmeyen başarısız olur.
**Veri diski sayısı** | 64 veya daha az çoğaltma yapıyorsanız **VMware Vm'lerini azure'a**; 16 veya daha az çoğaltma yapıyorsanız **Hyper-V Vm'lerini azure'a** | Önkoşul denetimi desteklenmeyen başarısız olur
**Veri diski VHD boyutu** | 4095 GB'a kadar | Önkoşul denetimi desteklenmeyen başarısız olur
**Ağ bağdaştırıcıları** | Birden çok bağdaştırıcı desteklenir |
**Paylaşılan VHD** | Desteklenmiyor | Önkoşul denetimi desteklenmeyen başarısız olur
**FC disk** | Desteklenmiyor | Önkoşul denetimi desteklenmeyen başarısız olur
**Sabit disk biçimi** | VHD <br/><br/> VHDX | VHDX şu anda Azure'da desteklenmiyor olsa da, Site Recovery, Azure'a yük otomatik olarak VHDX VHD'ye dönüştürür. Geri şirket içi başarısız olduğunda sanal makineler VHDX biçimini kullanacak şekilde devam eder.
**BitLocker'ı** | Desteklenmiyor | Bir sanal makine korumadan önce BitLocker'ı devre dışı bırakılmalıdır.
**VM adı** | 1 ile 63 karakter. Harf, rakam ve kısa çizgi için kısıtlanmış. VM adı başlamalı ve bir harf veya sayı ile bitmelidir. | Site Recovery sanal makine özelliklerinde değeri güncelleştirin.
**VM türü** | 1. nesil<br/><br/> Nesil 2--Windows | 2. nesil sanal makineleri (VHDX biçimlendirilmiş bir veya iki veri birimlerini içeren) temel bir işletim sistemi disk türüne sahip ve 300 GB disk alanı daha az desteklenir.<br></br>Linux nesil 2 sanal makineleri desteklenmez. [Daha fazla bilgi](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="support-for-recovery-services-vault-actions"></a>Kurtarma Hizmetleri kasası eylemler için destek

**Eylem** | **VMware/fiziksel sunucu** | **Hyper-V (Sanal Makine Yöneticisi)** | **Hyper-V (Virtual Machine Manager ile)**
--- | --- | --- | ---
Kasa kaynak grupları arasında taşıma<br/><br/> İçinde ve abonelikler arasında | Hayır | Hayır | Hayır
Depolama, ağ, Azure Vm'leri kaynak grupları arasında taşıma<br/><br/> İçinde ve abonelikler arasında | Hayır | Hayır | Hayır


## <a name="support-for-provider-and-agent"></a>Sağlayıcı ve aracı için destek

**Ad** | **Açıklama** | **En son sürümü** | **Ayrıntılar**
--- | --- | --- | --- | ---
**Azure Site Recovery sağlayıcısı** | Şirket içi sunucular ile Azure arasındaki iletişimi düzenler <br/><br/> Sanal Makine Yöneticisi sunucusu yoksa şirket içi Sanal Makine Yöneticisi sunucular veya Hyper-V sunucusunda yüklü | 5.1.2700.1 (portalından kullanılabilir) | [En son özellikleri ve düzeltmeleri](https://aka.ms/latest_asr_updates)
**Azure Site Recovery kurulumu (VMware Azure) birleşik** | Şirket içi VMware sunucular ile Azure arasındaki iletişimi düzenler <br/><br/> Şirket içi VMware sunucularda yüklü | 9.12.4653.1 (portalından kullanılabilir) | [En son özellikleri ve düzeltmeleri](https://aka.ms/latest_asr_updates)
**Mobility hizmeti** | Şirket içi VMware sunucularını/fiziksel sunucular ile Azure/ikincil site arasında çoğaltma koordinatları<br/><br/> VMware VM veya fiziksel sunucuları çoğaltmak istediğiniz yüklü  | 9.12.4653.1 (portalından kullanılabilir) | [En son özellikleri ve düzeltmeleri](https://aka.ms/latest_asr_updates)
**Microsoft Azure kurtarma Hizmetleri (MARS) aracısı** | Hyper-V sanal makineleri ve Azure arasında çoğaltma koordinatları<br/><br/> Şirket içi Hyper-V sunucuları (ile veya bir Sanal Makine Yöneticisi sunucusu olmadan) yüklü | En son Aracı (portalından kullanılabilir) |






## <a name="next-steps"></a>Sonraki adımlar
[Önkoşulları denetleme](site-recovery-prereq.md)
