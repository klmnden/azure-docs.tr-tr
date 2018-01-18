---
title: "VMware Vm'lerini ve fiziksel sunucuları Azure'a çoğaltma için azure Site Recovery destek matrisi | Microsoft Docs"
description: "VMware Vm'lerini Azure'da Azure Site Recovery hizmeti ile çoğaltma için bileşenleri ve desteklenen işletim sistemleri özetlenmektedir."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 01/11/2018
ms.author: raynew
ms.openlocfilehash: 857bbd42fda4abddd9a7551f4de016cecae03868
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="support-matrix-for-vmware-and-physical-server-replication-to-azure"></a>VMware ve fiziksel sunucu çoğaltma Azure için destek matrisi


Bu makalede desteklenen bileşenleri ve Azure, VMware vm'lerinin olağanüstü durum kurtarma için ayarları özetler kullanarak [Azure Site Recovery](site-recovery-overview.md) hizmet.


## <a name="supported-scenarios"></a>Desteklenen senaryolar

**Senaryo** | **Ayrıntılar** 
--- | --- 
**VMware Vm'leri** | Şirket içi VMware Vm'leri için Azure olağanüstü durum kurtarma gerçekleştirebilirsiniz. Bu senaryoda Azure portal veya PowerShell kullanarak dağıtabilirsiniz.
**Fiziksel sunucuları** | Şirket içi Windows/Linux fiziksel sunucuları için Azure olağanüstü durum kurtarma gerçekleştirebilirsiniz. Bu senaryo Azure portalında dağıtabilirsiniz.

## <a name="on-premises-virtualization-servers"></a>Şirket içi sanallaştırma sunucuları

**Sunucu** | **Gereksinimleri** | **Ayrıntılar**
--- | --- | ---
**VMware** | vCenter Server 6.5, 6.0 or 5.5 or vSphere 6.5, 6.0, 5.5 | Bir vCenter sunucusu kullanmanızı öneririz
**Fiziksel sunucuları** | NA


## <a name="replicated-machines"></a>Çoğaltılan makineler

Makineler için çoğaltma desteği aşağıdaki tabloda özetlenmiştir. Site Recovery, desteklenen bir işletim sistemine sahip bir makine üzerinde çalışan herhangi bir iş yükünü çoğaltmasını destekler.

**Bileşen** | **Ayrıntılar**
--- | ---
Makine Yapılandırması | Azure'a makineler karşılamalıdır [Azure gereksinimleri](#failed-over-azure-vm-requirements).
Makine işletim sistemini (Windows) | 64-bit Windows Server 2016 (Sunucu Çekirdeği, masaüstü deneyimi olan sunucu)\*, Windows Server 2012 R2, Windows Server 2012, Itanium tabanlı sistemler için Windows Server 2008 R2 ile en az SP1
Makine işletim sistemi (Linux) | Red Hat Enterprise Linux: 5.2 için 5.11 ya, 6.1 için 6.9, 7.0 için 7.3 <br/><br/>CentOS: 5.2 için 5.11 ya, 6.1 için 6.9, 7.0 için 7.3 <br/><br/>Ubuntu 14.04 LTS server[ (çekirdek sürümleri desteklenir)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Ubuntu 16.04 LTS server[ (çekirdek sürümleri desteklenir)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Debian 7 <br/><br/>Debian 8<br/><br/>Oracle Enterprise Linux 6.4, Red Hat uyumlu çekirdek ya da kesilemeyen kurumsal çekirdek sürüm 3 (UEK3) çalıştıran 6.5 <br/><br/>SUSE Linux Enterprise Server 11 SP3 <br/><br/>SUSE Linux Enterprise Server 11 SP4 <br/>(Makineler SLES 11 SP4 ' SLES 11 SP3 çoğaltılan yükseltme desteklenmez. Çoğaltılmış bir makineden SLES 11 SP4 ' SLES 11SP3 yükseltildiyse, çoğaltmayı devre dışı bırakın ve makine yükseltme sonrası yeniden korumak gerekir.)
Linux çekirdek sürüm | Red Hat Enterprise Linux Server 7 + ve CentOS 7 + sunucularda, çekirdek sürüm 3.10.0-514 Azure Site Recovery Mobility hizmeti 9.8 sürümünden itibaren desteklenmektedir.<br/><br/> Mobility hizmetinin 9.8 sürümünden daha düşük bir sürümü ile 3.10.0-514 çekirdek müşteriler çoğaltmasını devre dışı bırakın, Mobility hizmeti sürümü 9.8 sürüme güncelleştirin ve ardından çoğaltma işlemini yeniden etkinleştirmek için gereklidir.


### <a name="ubuntu-kernel-versions"></a>Ubuntu çekirdek sürümleri


**Desteklenen sürüm** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
14.04 LTS | 9.9 | 3.13.0-24-Generic 3.13.0-117-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-75-generic 4.4.0-21-Generic |
14.04 LTS | 9.10 | 3.13.0-24-Generic 3.13.0-121-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-81-generic 4.4.0-21-Generic |
14.04 LTS | 9.11 | 3.13.0-24-Generic 3.13.0-128-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-91-generic 4.4.0-21-Generic |
14.04 LTS | 9.12 | 3.13.0-24-Generic 3.13.0-132-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-96-generic 4.4.0-21-Generic |
16.04 LTS | 9.10 | 4.4.0-21-Generic 4.4.0-81-generic için<br/>4.8.0-34-Generic 4.8.0-56-generic için<br/>4.10.0-24-generic 4.10.0-14-Generic |
16.04 LTS | 9.11 | 4.4.0-21-Generic 4.4.0-91-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-32-generic 4.10.0-14-Generic |
16.04 LTS | 9.12 | 4.4.0-21-Generic 4.4.0-96-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-35-generic 4.10.0-14-Generic |

## <a name="linux-file-systemsguest-storage-configurations"></a>Linux dosya sistemleri/Konuk depolama yapılandırmaları

**Bileşen** | **Destekleniyor**
--- | ---
Dosya sistemleri | ext3, ext4, ReiserFS (yalnızca Suse Linux Enterprise Server), XFS
Birim Yöneticisi | LVM2
Çok yollu yazılım | Cihaz Eşleyici
Paravirtualized depolama aygıtları | Paravirtualized sürücüleri tarafından dışarı aktarılan cihazlar desteklenmez.
G/ç cihazların çok sıra engelle | Desteklenmiyor.
HP CCISS depolama denetleyicisi ile fiziksel sunucuları | Desteklenmiyor.
Dizinler | Bu dizinleri (varsa ayrı bölümleri/dosya-sistemleri ayarlanmış) tümü aynı işletim sistemi diski kaynak sunucuda olmalıdır: / (kök), / Boot/usr, /usr/local, /var, / etc
XFSv5 | Sürümünden başlayarak Mobility hizmeti 9.10 XFS dosya sistemleri gibi meta veri sağlama toplamı XFSv5 özellikleri desteklenir. Süper blok XFS kullanarak bölümün denetlemek için xfs_info yardımcı programını kullanın. Ftype 1 olarak ayarlanırsa, XFSv5 özellikleri kullanılıyor olabilir.



## <a name="network"></a>Ağ

**Bileşen** | **Destekleniyor** 
--- | --- 
Konak ağ NIC ekibi oluşturma | VMware Vm'leri için desteklenen <br/><br/>Fiziksel makine çoğaltma için desteklenmiyor
Konak ağı VLAN | Evet 
Konak ağı IPv4 | Evet 
Konak ağı IPv6 | Hayır 
Konuk/sunucu ağ NIC ekibi oluşturma | Hayır 
Konuk/sunucu ağ IPv4 | Evet 
Konuk/sunucu ağ IPv6 | Hayır 
Konuk/sunucu ağ statik IP (Windows) | Evet 
Konuk/sunucu ağ statik IP (Linux) | Evet <br/><br/>Sanal makineleri yeniden çalışma üzerinde DHCP kullanmak üzere yapılandırılmış  
Konuk/sunucu ağ birden çok NIC | Evet 


## <a name="azure-vm-network-after-failover"></a>Azure VM ağı (sonra Yük devretme)

**Bileşen** | **Destekleniyor** 
--- | --- 
Express Route | Evet 
ILB | Evet 
ELB | Evet 
Traffic Manager | Evet 
Multi-NIC | Evet 
Ayrılmış IP adresi | Evet 
IPv4 | Evet 
Kaynak IP adresi koru | Evet 
Sanal ağ hizmet uç noktaları<br/><br/> (Azure depolama güvenlik duvarları ve sanal ağlar) | Hayır 


## <a name="storage"></a>Depolama


**Bileşen** | **Destekleniyor** 
--- | --- 
Ana bilgisayar NFS | VMware için Evet<br/><br/> Fiziksel sunucuları için Hayır'ı 
Konak SAN (İSCSI) | Evet
Ana bilgisayar çok yollu (MPIO) | Evet - ile test: Microsoft DSM, EMC PowerPath 5.7 SP4 EMC PowerPath DSM CLARiiON için
Konuk/sunucu VMDK | Evet 
Konuk/sunucu EFI/UEFI'ye| Hayır
Konuk/sunucu paylaşılan küme diski | Hayır 
Konuk/sunucu şifreli disk | Hayır 
Konuk/sunucu NFS | Hayır 
Konuk/sunucu SMB 3.0 | Hayır
Konuk/sunucu RDM | Evet<br/><br/> Fiziksel sunucuları için yok 
Konuk/sunucu diski > 1 TB | Evet<br/><br/>4095 GB'a kadar 
Konuk/sunucu diski 4 K mantıksal ve 4 k fiziksel kesim boyutu | Evet
4K mantıksal Konuk/sunucu diskle ve 512 bayt fiziksel kesim boyutu | Evet 
Şeritli disk > 1 TB Konuk/sunucu birimle<br/><br/> Konuk/sunucu LVM mantıksal birim yönetimi - depolama alanları | Konuk/sunucu sık kullanılan Ekle/Kaldır disk | / Sunucu - Konuk hariç disk | Konuk/sunucu çok yollu (MPIO) Evet | YOK 

## <a name="azure-storage"></a>Azure Storage

**Bileşen** | **Destekleniyor** 
--- | --- 
LRS | Evet 
GRS | Evet 
RA-GRS | Evet 
Seyrek erişimli depolama | Hayır 
Sık erişimli depolama| Hayır 
Blok Blobları | Hayır 
Rest(SSE) şifreleme| Evet 
Premium depolama | Evet 
İçeri/dışarı aktarma hizmeti | Hayır 
Sanal ağ hizmet uç noktaları<br/><br/> Azure depolama güvenlik duvarları ve hedef depolama/önbelleği depolama hesabı (çoğaltma verilerini depolamak için kullanılır) üzerinde yapılandırılmış sanal ağlar | Hayır 
Genel amaçlı V2 depolama hesapları (her ikisini de sık erişimli ve seyrek katman) | Hayır 


## <a name="azure-compute"></a>Azure işlem

**Featuree** | **Destekleniyor** 
--- | --- 
Kullanılabilirlik kümeleri | Evet 
HUB | Evet   
Yönetilen diskler | Evet 

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Şirket içi sanal makinelerini Azure'a çoğaltma bu tabloda özetlenen Azure VM gereksinimlerini karşılaması gerekir.

**Bileşen** | **Gereksinimleri** | **Ayrıntılar**
--- | --- | ---
**Konuk işletim sistemi** | Doğrulama [desteklenen işletim sistemleri](#replicated machines) | Önkoşullar desteklenmeyen başarısız kontrol edin.
**Konuk işletim sistemi mimarisi** | 64 bit | Desteklenmeyen, önkoşul denetimi başarısız olur
**İşletim sistemi disk boyutu** | 2048 GB'a kadar | Desteklenmeyen, önkoşul denetimi başarısız olur
**İşletim sistemi disk sayısı** | 1 | Önkoşul denetimi desteklenmeyen başarısız olur.
**Veri diski sayısı** | 64 veya daha az çoğaltma yapıyorsanız **VMware Vm'lerini azure'a**; 16 veya daha az çoğaltma yapıyorsanız **Hyper-V Vm'lerini azure'a** | Önkoşul denetimi desteklenmeyen başarısız olur
**Veri diski VHD boyutu** | 4095 GB'a kadar | Desteklenmeyen, önkoşul denetimi başarısız olur
**Ağ bağdaştırıcıları** | Birden çok bağdaştırıcı desteklenir | 
**Paylaşılan VHD** | Desteklenmiyor | Desteklenmeyen, önkoşul denetimi başarısız olur
**FC disk** | Desteklenmiyor | Desteklenmeyen, önkoşul denetimi başarısız olur
**Sabit disk biçimi** | VHD <br/><br/> VHDX | VHDX şu anda Azure'da desteklenmiyor olsa da, Site Recovery, Azure'a yük otomatik olarak VHDX VHD'ye dönüştürür. Geri şirket içi başarısız olduğunda sanal makineler VHDX biçimini kullanacak şekilde devam eder.
**Bitlocker** | Desteklenmiyor | Bir makine için çoğaltma etkinleştirmeden önce BitLocker'ı devre dışı bırakılması gerekir.
**VM adı** | 1 - 63 karakter.<br/><br/> Harf, rakam ve kısa çizgi için kısıtlanmış.<br/><br/> Makine adı başlamalı ve bir harf veya sayı ile bitmelidir. | Site Recovery makine özelliklerinde değeri güncelleştirin.
**VM türü** | 1. nesil<br/><br/> Nesil 2--Windows | 2. nesil sanal makineleri (VHDX biçimlendirilmiş bir veya iki veri birimlerini içeren) temel bir işletim sistemi disk türüne sahip ve 300 GB disk alanı daha az desteklenir.<br></br>Linux nesil 2 sanal makineleri desteklenmez. [Daha fazla bilgi](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="vault-tasks"></a>Kasa görevleri

**Eylem** | **Destekleniyor** 
--- | --- 
Kasa kaynak grupları arasında taşıma<br/><br/> İçinde ve abonelikler arasında | Hayır 
Depolama, ağ, Azure Vm'leri kaynak grupları arasında taşıma<br/><br/> İçinde ve abonelikler arasında | Hayır 


## <a name="mobility-service"></a>Mobility hizmeti

**Ad** | **Açıklama** | **En son sürümü** | **Ayrıntılar**
--- | --- | --- | --- | ---
** Azure Site Recovery birleşik Kurulum ** | Şirket içi VMware sunucular ile Azure arasındaki iletişimi düzenler <br/><br/> Şirket içi VMware sunucularda yüklü | 9.12.4653.1 (portalından kullanılabilir) | [En son özellikleri ve düzeltmeleri](https://aka.ms/latest_asr_updates)
**Mobility hizmeti** | Şirket içi VMware sunucularını/fiziksel sunucular ile Azure/ikincil site arasında çoğaltma koordinatları<br/><br/> VMware VM veya fiziksel sunucuları çoğaltmak istediğiniz yüklü  | 9.12.4653.1 (portalından kullanılabilir) | [En son özellikleri ve düzeltmeleri](https://aka.ms/latest_asr_updates)


## <a name="next-steps"></a>Sonraki adımlar
[Bilgi nasıl](tutorial-prepare-azure.md) Azure VMware Vm'lerini olağanüstü durum kurtarma için hazırlamak için.
