---
title: Hyper-V çoğaltma Azure için destek matrisi | Microsoft Docs
description: Hyper-V çoğaltma Azure Site Recovery ile azure'a için gereksinimleri ve desteklenen bileşenlerin özetler
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/06/2018
ms.author: raynew
ms.openlocfilehash: d2c637dc742ee854c7787cf7cd883930c4eaa8bc
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="support-matrix-for-hyper-v-replication-to-azure"></a>Hyper-V çoğaltma Azure için destek matrisi


Bu makalede desteklenen bileşenleri ve şirket içi Hyper-V sanal makineleri olağanüstü durum kurtarma Azure ayarlarını kullanarak özetler [Azure Site Recovery](site-recovery-overview.md).


## <a name="supported-scenarios"></a>Desteklenen senaryolar

**Senaryo** | **Ayrıntılar**
--- | --- 
Hyper-V sanal makine Yöneticisi ile | System Center Virtual Machine Manager yapıda yönetilen Hyper-V konakları üzerinde çalışan sanal makineler için Azure olağanüstü durum kurtarma gerçekleştirebilirsiniz.<br/><br/> Bu senaryo Azure portalında veya PowerShell kullanarak dağıtabilirsiniz.<br/><br/> Hyper-V konakları Virtual Machine Manager tarafından yönetildiğinde, aynı zamanda bir ikincil şirket içi siteye olağanüstü durum kurtarma gerçekleştirebilirsiniz. Bu senaryo hakkında daha fazla bilgi için okuma [Bu öğretici](tutorial-vmm-to-vmm.md).
Hyper-V olmadan Sanal Makine Yöneticisi | Virtual Machine Manager tarafından yönetilmeyen Hyper-V konakları üzerinde çalışan sanal makineler için Azure olağanüstü durum kurtarma gerçekleştirebilirsiniz.<br/><br/> Bu senaryo Azure portalında veya PowerShell kullanarak dağıtabilirsiniz. 


## <a name="on-premises-servers"></a>Şirket içi sunucular

**Sunucu** | **Gereksinimleri** | **Ayrıntılar**
--- | --- | ---
Hyper-V (olmadan Sanal Makine Yöneticisi'ni çalıştıran) | Windows Server 2016, en son güncelleştirmeleri içeren Windows Server 2012 R2 | Site kurtarma için bir Hyper-V sitesi yapılandırdığınızda, Windows Server 2016 ve 2012 R2 çalıştıran konaklar karıştırılması desteklenmiyor.<br/><br/> Windows Server 2016 çalıştıran bir konakta bulunan sanal makineleri için farklı bir konuma kurtarma desteklenmez.
Hyper-V (Virtual Machine Manager ile çalıştıran) | Virtual Machine Manager 2016, sanal makine yöneticisi 2012 R2 | Sanal Makine Yöneticisi kullandıysanız, Windows Server 2016 konaklar Sanal Makine Yöneticisi 2016'yönetilmelidir.<br/><br/> Windows Server 2016 ve 2012 R2 çalıştıran Hyper-V konakları karıştırır bir Virtual Machine Manager Bulutu şu anda desteklenmiyor.<br/><br/> Varolan bir sanal makine yöneticisi 2012 R2 sunucusuna 2016 yükseltmesini dahil ortamları desteklenmez.


## <a name="replicated-vms"></a>Çoğaltılan VM'ler


VM desteği aşağıdaki tabloda özetlenmiştir. Site Recovery, desteklenen bir işletim sisteminde çalışan tüm iş yüklerini destekler. 

 **Bileşen** | **Ayrıntılar**
--- | ---
VM yapılandırması | Azure'a VM'ler karşılamalıdır [Azure gereksinimleri](#failed-over-azure-vm-requirements).
Konuk işletim sistemi | Bir konuk işletim sistemi [Azure tarafından desteklenen](https://technet.microsoft.com/library/cc794868.aspx).<br/><br/> Windows Server 2016 Nano Server desteklenmiyor.




## <a name="hyper-v-network-configuration"></a>Hyper-V ağ yapılandırması

**Bileşen** | **Hyper-V sanal makine Yöneticisi ile** | **Hyper-V olmadan Sanal Makine Yöneticisi**
--- | --- | ---
Konak ağ: NIC ekibi oluşturma | Evet
Konak ağ: VLAN | Evet
Konak ağ: IPv4 | Evet
Konak ağ: IPv6 | Hayır
Konuk VM ağı: NIC ekibi oluşturma | Hayır
Konuk VM ağı: IPv4 | Evet
Konuk VM ağı: IPv6 | Hayır
Konuk VM ağı: statik IP (Windows) | Evet
Konuk VM ağı: statik IP (Linux) | Hayır
Konuk VM ağı: Multi-NIC | Evet



## <a name="azure-vm-network-configuration-after-failover"></a>Azure VM ağ yapılandırması (sonra Yük devretme)

**Bileşen** | **Hyper-V sanal makine Yöneticisi ile** | **Hyper-V olmadan Sanal Makine Yöneticisi**
--- | --- | ---
Azure ExpressRoute | Evet | Evet
ILB | Evet | Evet
ELB | Evet | Evet
Azure Traffic Manager | Evet | Evet
Multi-NIC | Evet | Evet
Ayrılmış IP | Evet | Evet
IPv4 | Evet | Evet
Kaynak IP adresi koru | Evet | Evet
Azure sanal ağ hizmet uç noktaları<br/><br/> (Azure depolama güvenlik duvarları ve sanal ağlar) | Hayır | Hayır


## <a name="hyper-v-host-storage"></a>Hyper-V konağı depolama

**Depolama** | **Hyper-V sanal makine Yöneticisi ile** | **Hyper-V olmadan Sanal Makine Yöneticisi**
--- | --- | --- | ---
NFS | NA | NA
SMB 3.0 | Evet | Evet
SAN (İSCSI) | Evet | Evet
Çok yollu (MPIO). İle test edilmiştir:<br></br> Microsoft DSM, EMC PowerPath 5.7 SP4<br/><br/> EMC PowerPath DSM CLARiiON için | Evet | Evet

## <a name="hyper-v-vm-guest-storage"></a>Hyper-V VM Konuk depolama

**Depolama** | **Hyper-V sanal makine Yöneticisi ile** | **Hyper-V olmadan Sanal Makine Yöneticisi**
--- | --- | ---
VMDK | NA | NA
VHD/VHDX | Evet | Evet
2. nesil VM | Evet | Evet
EFI/UEFI'YE| Evet | Evet
Küme diskini paylaşılan | Hayır | Hayır
Şifrelenmiş disk | Hayır | Hayır
NFS | NA | NA
SMB 3.0 | Hayır | Hayır
RDM | NA | NA
Disk > 1 TB | Evet, 4.095 GB'a kadar | Evet, 4.095 GB'a kadar
Disk: mantıksal ve fiziksel 4K kesim | Desteklenmiyor: Gen 1/Gen 2 | Desteklenmiyor: Gen 1/Gen 2
Disk: 4K mantıksal ve fiziksel 512 baytlık kesim | Evet |  Evet
Şeritli disk birimle > 1 TB<br/><br/> Mantıksal birim yönetimi (LVM) | Evet | Evet
Depolama alanları | Evet | Evet
Sık kullanılan Ekle/Kaldır disk | Hayır | Hayır
Diski hariç tutma | Evet | Evet
Çok yollu (MPIO) | Evet | Evet

## <a name="azure-storage"></a>Azure Storage

**Bileşen** | **Hyper-V sanal makine Yöneticisi ile** | **Hyper-V olmadan Sanal Makine Yöneticisi**
--- | --- | ---
Yerel olarak yedekli depolama | Evet | Evet
Coğrafi olarak yedekli depolama | Evet | Evet
Coğrafi olarak yedekli depolamaya okuma erişimi | Evet | Evet
Seyrek erişimli depolama | Hayır | Hayır
Sık erişimli depolama| Hayır | Hayır
Blok blobları | Hayır | Hayır
Bekleyen (SSE) şifreleme| Evet | Evet
Premium depolama | Evet | Evet
İçeri/dışarı aktarma hizmeti | Hayır | Hayır
Azure sanal ağ hizmet uç noktaları (Azure Storage güvenlik duvarları ve sanal ağlar) hedef çoğaltma verileri için kullanılan önbellek depolama hesabı | Hayır | Hayır


## <a name="azure-compute-features"></a>Azure işlem özellikleri

**Özellik** | **Hyper-V sanal makine Yöneticisi ile** | **Hyper-V olmadan Sanal Makine Yöneticisi**
--- | --- | ---
Kullanılabilirlik kümeleri | Evet | Evet
HUB | Evet | Evet  
Yönetilen diskler | Evet, yük devretme için.<br/><br/> Yönetilen disklerin yeniden çalışma desteklenmez. | Evet, yük devretme için.<br/><br/> Yönetilen disklerin yeniden çalışma desteklenmez.

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Şirket içi sanal makinelerini Azure'a çoğaltma bu tabloda özetlenen Azure VM gereksinimlerini karşılaması gerekir.

**Bileşen** | **Gereksinimleri** | **Ayrıntılar**
--- | --- | ---
Konuk işletim sistemi | Site Recovery tüm işletim sistemlerini destekler [Azure tarafından desteklenen](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).  | Önkoşullar desteklenmeyen başarısız kontrol edin.
Konuk işletim sistemi mimarisi | 64 bit | Önkoşullar desteklenmeyen başarısız kontrol edin.
İşletim sistemi disk boyutu | 1. nesil sanal makineleri için 2.048 GB'a kadar.<br/><br/> 2. nesil sanal makineleri için 300 GB.  | Önkoşullar desteklenmeyen başarısız kontrol edin.
İşletim sistemi disk sayısı | 1 | Önkoşullar desteklenmeyen başarısız kontrol edin.
Veri diski sayısı | 16 veya daha az  | Önkoşullar desteklenmeyen başarısız kontrol edin.
Veri diski VHD boyutu | 4.095 GB'a kadar | Önkoşullar desteklenmeyen başarısız kontrol edin.
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir |
Paylaşılan VHD | Desteklenmiyor | Önkoşullar desteklenmeyen başarısız kontrol edin.
FC disk | Desteklenmiyor | Önkoşullar desteklenmeyen başarısız kontrol edin.
Sabit disk biçimi | VHD <br/><br/> VHDX | Azure'a yük devretme, site kurtarma VHDX VHD'ye otomatik olarak dönüştürür. Geri şirket içi başarısız olduğunda, sanal makineler VHDX biçimini kullanacak şekilde devam eder.
BitLocker | Desteklenmiyor | Bir sanal makine için çoğaltmayı etkinleştirmeden önce BitLocker'ı devre dışı bırakılması gerekir.
VM adı | 1-63 karakter. Harfler, sayılar ve kısa çizgilerden oluşabilir. VM adı bir harf veya sayıyla başlamalı ve bitmelidir. | VM Özellikleri'nde Site Recovery değeri güncelleştirin.
VM türü | 1. nesil<br/><br/> Nesil 2--Windows | 2. nesil sanal makineleri (VHDX biçimlendirilmiş bir veya iki veri birimlerini içeren) temel bir işletim sistemi disk türüne sahip ve 300 GB disk alanı daha az desteklenir.<br></br>Linux nesil 2 sanal makineleri desteklenmez. [Daha fazla bilgi edinin](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).|

## <a name="recovery-services-vault-actions"></a>Kurtarma Hizmetleri kasası eylemleri

**Eylem** |  **Hyper-V sanal makine Yöneticisi ile** | **Hyper-V olmadan Sanal Makine Yöneticisi**
--- | --- | --- 
Kasa kaynak grupları arasında taşıma<br/><br/> İçinde ve abonelikler arasında | Hayır | Hayır 
Depolama, ağ, Azure Vm'leri kaynak grupları arasında taşıma<br/><br/> İçinde ve abonelikler arasında | Hayır | Hayır 


## <a name="provider-and-agent"></a>Sağlayıcı ve aracı

Dağıtımınızın bu makaledeki ayarlarla uyumlu olduğundan emin olmak için son sağlayıcı ve Aracı sürümleri çalışmakta olduğunuzdan emin olun.

**Ad** | **Açıklama** | **Ayrıntılar**
--- | --- | --- | --- | ---
Azure Site kurtarma sağlayıcısı | Şirket içi sunucular ile Azure arasındaki iletişimi düzenler <br/><br/> Hyper-V sanal makine Yöneticisi ile: Sanal Makine Yöneticisi sunucularda yüklü<br/><br/> Hyper-V olmadan Sanal Makine Yöneticisi: Hyper-V ana bilgisayarına yüklü| En son sürümü: 5.1.2700.1 (Azure portalından kullanılabilir)<br/><br/> [En son özellikleri ve düzeltmeleri](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery)
Microsoft Azure kurtarma Hizmetleri Aracısı | Hyper-V sanal makineleri ve Azure arasında çoğaltma koordinatları<br/><br/> Şirket içi Hyper-V sunucuları (veya ile olmadan Sanal Makine Yöneticisi) yüklü | En son aracı portaldan kullanılabilir






## <a name="next-steps"></a>Sonraki adımlar
Bilgi nasıl [Azure hazırlama](tutorial-prepare-azure.md) şirket içi Hyper-V sanal makineleri olağanüstü durum kurtarması için. 
