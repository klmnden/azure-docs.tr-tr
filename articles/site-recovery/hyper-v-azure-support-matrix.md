---
title: "Hyper-V çoğaltma Azure için destek matrisi | Microsoft Docs"
description: "Hyper-V çoğaltma Azure Site Recovery ile azure'a için gereksinimleri ve desteklenen bileşenlerin özetler"
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 02/14/2018
ms.author: raynew
ms.openlocfilehash: 58d54c1e0e6aa88878b45400b9211396f5d1b9d5
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="support-matrix-for-hyper-v-replication-to-azure"></a>Hyper-V çoğaltma Azure için destek matrisi


Bu makalede Azure, şirket içi Hyper-V sanal makineleri, olağanüstü durum kurtarma için ayarları ve desteklenen bileşenlerin özetlenmektedir kullanarak [Azure Site Recovery](site-recovery-overview.md) hizmet.


## <a name="supported-scenarios"></a>Desteklenen senaryolar

Senaryo | **Ayrıntılar**
--- | --- 
**VMM ile Hyper-V** | System Center Virtual Machine Manager (VMM) yapıda yönetilen Hyper-V konakları üzerinde çalışan sanal makineler için Azure olağanüstü durum kurtarma gerçekleştirebilirsiniz<br/><br/> Bu senaryoda Azure portal veya PowerShell kullanarak dağıtabilirsiniz.<br/><br/> Hyper-V konakları VMM tarafından yönetildiğinde zamanda bir ikincil şirket içi siteye olağanüstü durum kurtarma işlemlerini gerçekleştirebilirsiniz. Okuma [bu makalede](tutorial-vmm-to-vmm.md) bu senaryo hakkında daha fazla bilgi için.
**VMM olmadan Hyper-V** | VMM tarafından yönetilmeyen Hyper-V konakları üzerinde çalışan sanal makineler için Azure olağanüstü durum kurtarma gerçekleştirebilirsiniz.<br/><br/> Bu senaryoda Azure portal veya Powershell kullanarak dağıtabilirsiniz. 


## <a name="on-premises-servers"></a>Şirket içi sunucular

**Sunucu** | Gereksinimleri | **Ayrıntılar**
--- | --- | ---
**Hyper-V (VMM çalıştıran)** | Windows Server 2016, en son güncelleştirmeleri içeren Windows Server 2012 R2. | Site kurtarma için bir Hyper-V sitesi yapılandırdığınızda, Windows Server 2016 ve 2012 R2 çalıştıran konaklar karıştırılması desteklenmiyor.<br/><br/> Windows Server 2016 çalıştıran bir konakta bulunan sanal makineleri için farklı bir konuma kurtarma desteklenmez.
**Hyper-V (VMM ile çalıştıran)** | VMM 2016, VMM 2012 R2 | VMM kullandıysanız, Windows Server 2016 konaklar VMM 2016'yönetilmelidir.<br/><br/> Windows Server 2016 ve 2012 R2 çalıştıran Hyper-V konakları karıştırır VMM Bulutları özellik şu anda desteklenmiyor.<br/><br/> Mevcut bir VMM 2012 R2 sunucusuna 2016 yükseltmesini dahil ortamları desteklenmez.


## <a name="replicated-vms"></a>Çoğaltılan VM'ler


VM desteği aşağıdaki tabloda özetlenmiştir. Site Recovery, desteklenen bir işletim sisteminde çalışan tüm iş yüklerini destekler. 

 **Bileşen** | **Ayrıntılar**
--- | ---
VM yapılandırması | Azure'a VM'ler karşılamalıdır [Azure gereksinimleri](#failed-over-azure-vm-requirements).
Konuk işletim sistemi | Bir konuk işletim sistemi [Azure tarafından desteklenen](https://technet.microsoft.com/library/cc794868.aspx).<br/><br/> Windows Server 2016 Nano Server desteklenmiyor.




## <a name="hyper-v-network-configuration"></a>Hyper-V ağ yapılandırması

**Bileşen** | **VMM ile Hyper-V** | **VMM olmadan Hyper-V**
--- | --- | ---
Konak ağ: NIC ekibi oluşturma | Evet
Konak ağ: VLAN | Evet
Konak ağ: IPv4 | Evet
Host network:IPv6 | Hayır
Konuk VM ağ: NIC ekibi oluşturma | Hayır
Konuk VM ağ: IPv4 | Evet
Konuk VM ağ: IPv6 | Hayır
Konuk VM ağ: statik IP (Windows) | Evet
Konuk VM ağ: statik IP (Linux) | Hayır
Konuk VM ağ: Multi-NIC | Evet



## <a name="azure-vm-network-configuration-after-failover"></a>Azure VM ağ yapılandırması (sonra Yük devretme)

**Bileşen** | **VMM ile Hyper-V** | **VMM olmadan Hyper-V**
--- | --- | ---
ExpressRoute | Evet | Evet
ILB | Evet | Evet
ELB | Evet | Evet
Traffic Manager | Evet | Evet
Multi-NIC | Evet | Evet
Ayrılmış IP | Evet | Evet
IPv4 | Evet | Evet
Kaynak IP adresi koru | Evet | Evet
VNET Hizmeti uç noktaları<br/><br/> (Azure depolama güvenlik duvarları ve sanal ağlar) | Hayır | Hayır


## <a name="hyper-v-host-storage"></a>Hyper-V konağı depolama

**Depolama** | **VMM ile Hyper-V** | VMM olmadan Hyper-V
--- | --- | --- | ---
NFS | NA | NA
SMB 3.0 | Evet | Evet
SAN (ISCSI) | Evet | Evet
Çok yollu (MPIO). İle test edilmiştir:<br></br> Microsoft DSM, EMC PowerPath 5.7 SP4<br/><br/> EMC PowerPath DSM CLARiiON için | Evet | Evet

## <a name="hyper-v-vm-guest-storage"></a>Hyper-V VM Konuk depolama

**Depolama** | **VMM ile Hyper-V** | VMM olmadan Hyper-V
--- | --- | ---
VMDK | NA | NA
VHD/VHDX | Evet | Evet
2. nesil VM | Evet | Evet
EFI/UEFI| Evet | Evet
Küme diskini paylaşılan | Hayır | Hayır
Şifrelenmiş disk | Hayır | Hayır
NFS | NA | NA
SMB 3.0 | Hayır | Hayır
RDM | NA | NA
Disk > 1 TB | Evet, 4095 GB'a kadar | Evet, 4095 GB'a kadar
Disk: mantıksal ve fiziksel 4K kesim | Desteklenmiyor: Gen 1/Gen 2 | Desteklenmiyor: Gen 1/Gen 2
Disk: 4K mantıksal ve fiziksel 512 baytlık kesim | Evet |  Evet
Şeritli disk > 1 TB birimle<br/><br/> LVM mantıksal birim yönetimi | Evet | Evet
Depolama alanları | Evet | Evet
Sık kullanılan Ekle/Kaldır disk | Hayır | Hayır
Diski hariç tutma | Evet | Evet
Çok yollu (MPIO) | Evet | Evet

## <a name="azure-storage"></a>Azure Storage

**Bileşen** | **VMM ile Hyper-V** | **Hyper-V VMM olmadan)**
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
VNET Hizmeti uç noktalarını (Azure depolama güvenlik duvarları ve sanal ağlar), hedef çoğaltma verileri için kullanılan önbellek depolama hesabı | Hayır | Hayır


## <a name="azure-compute-features"></a>Azure işlem özellikleri

**Özellik** | **VMM ile Hyper-V** | **VMM olmadan Hyper-V**
--- | --- | ---
Kullanılabilirlik kümeleri | Evet | Evet
HUB | Evet | Evet  
Yönetilen diskler | Evet, yük devretme için<br/><br/> Yeniden çalışma yönetilen disklerin desteklenmiyor | Evet, yük devretme için<br/><br/> Yeniden çalışma yönetilen disklerin desteklenmiyor

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Şirket içi sanal makinelerini Azure'a çoğaltma bu tabloda özetlenen Azure VM gereksinimlerini karşılaması gerekir.

**Bileşen** | Gereksinimleri | **Ayrıntılar**
--- | --- | ---
Konuk işletim sistemi | Site Recovery tüm işletim sistemlerini destekler [Azure tarafından desteklenen](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).  | Önkoşul denetimi desteklenmeyen başarısız olur.
**Konuk işletim sistemi mimarisi** | 64 bit | Önkoşul denetimi desteklenmeyen başarısız olur.
**İşletim sistemi disk boyutu** | 1. nesil sanal makineleri için 2048 GB'a kadar.<br/><br/> 2. nesil sanal makineleri için 300 GB.  | Önkoşul denetimi desteklenmeyen başarısız olur.
**İşletim sistemi disk sayısı** | 1 | Önkoşul denetimi desteklenmeyen başarısız olur.
**Veri diski sayısı** | 16 veya daha az  | Önkoşul denetimi desteklenmeyen başarısız olur.
**Veri diski VHD boyutu** | 4095 GB'a kadar | Önkoşul denetimi desteklenmeyen başarısız olur.
**Ağ bağdaştırıcıları** | Birden çok bağdaştırıcı desteklenir |
**Paylaşılan VHD** | Desteklenmiyor | Önkoşul denetimi desteklenmeyen başarısız olur.
**FC diski** | Desteklenmiyor | Önkoşul denetimi desteklenmeyen başarısız olur.
**Sabit disk biçimi** | VHD <br/><br/> VHDX | Azure'a yük devretme, site kurtarma VHDX VHD'ye otomatik olarak dönüştürür. Geri şirket içi başarısız olduğunda sanal makineler VHDX biçimini kullanacak şekilde devam eder.
**Bitlocker** | Desteklenmiyor | Bir sanal makine için çoğaltmayı etkinleştirmeden önce BitLocker'ı devre dışı bırakılması gerekir.
**VM adı** | 1-63 karakter. Harfler, sayılar ve kısa çizgilerden oluşabilir. VM adı bir harf veya sayıyla başlamalı ve bitmelidir. | VM Özellikleri'nde Site Recovery değeri güncelleştirin.
**VM türü** | 1. nesil<br/><br/> Nesil 2--Windows | 2. nesil sanal makineleri (VHDX biçimlendirilmiş bir veya iki veri birimlerini içeren) temel bir işletim sistemi disk türüne sahip ve 300 GB disk alanı daha az desteklenir.<br></br>Linux nesil 2 sanal makineleri desteklenmez. [Daha fazla bilgi](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="recovery-services-vault-actions"></a>Kurtarma Hizmetleri kasası eylemleri

**Eylem** |  **VMM ile Hyper-V** | **VMM olmadan Hyper-V**
--- | --- | --- 
Kasa kaynak grupları arasında taşıma<br/><br/> İçinde ve abonelikler arasında | Hayır | Hayır 
Depolama, ağ, Azure Vm'leri kaynak grupları arasında taşıma<br/><br/> İçinde ve abonelikler arasında | Hayır | Hayır 


## <a name="provider-and-agent"></a>Sağlayıcı ve aracı

Dağıtımınızın bu makaledeki ayarlarla uyumlu olduğundan emin olmak için son sağlayıcı ve Aracı sürümleri çalışmakta olduğunuzdan emin olun.

**Ad** | **Açıklama** | **Ayrıntılar**
--- | --- | --- | --- | ---
**Azure Site Recovery sağlayıcısı** | Şirket içi sunucular ile Azure arasındaki iletişimi düzenler <br/><br/> Hyper-V VMM ile: VMM sunucularında yüklü<br/><br/> Hyper-V VMM olmadan: Hyper-V ana bilgisayarına yüklü| En son sürümü: 5.1.2700.1 (portalından kullanılabilir)<br/><br/> [En son özellikleri ve düzeltmeleri](https://aka.ms/latest_asr_updates)
**Microsoft Azure kurtarma Hizmetleri (MARS) aracısı** | Hyper-V sanal makineleri ve Azure arasında çoğaltma koordinatları<br/><br/> Şirket içi Hyper-V sunucuları (veya ile VMM olmadan) yüklü | En son aracı portalından kullanılabilir






## <a name="next-steps"></a>Sonraki adımlar
Bilgi nasıl [Azure hazırlama](tutorial-prepare-azure.md) şirket içi Hyper-V sanal makineleri olağanüstü durum kurtarması için. 
