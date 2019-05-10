---
title: Şirket içi Hyper-V Vm'lerini azure'a olağanüstü durum kurtarması için destek matrisi | Microsoft Docs
description: Azure Site Recovery ile azure'a Hyper-V VM'LERİNDE olağanüstü durum kurtarma için gereksinimleri ve desteklenen bileşenlerin özetler
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: raynew
ms.openlocfilehash: e311a328c1c3d78fa8e5ba7065dcc6484006eaaf
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65235884"
---
# <a name="support-matrix-for-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Şirket içi Hyper-V Vm'lerini azure'a olağanüstü durum kurtarması için destek matrisi


Bu makalede kullanarak desteklenen bileşenler ve şirket içi Hyper-V Vm'lerini azure'a olağanüstü durum kurtarması için ayarları özetler [Azure Site Recovery](site-recovery-overview.md).


## <a name="supported-scenarios"></a>Desteklenen senaryolar

**Senaryo** | **Ayrıntılar**
--- | ---
Hyper-V ile Virtual Machine Manager | System Center Virtual Machine Manager dokusunda yönetilen Hyper-V konakları üzerinde çalışan VM'ler için Azure'da olağanüstü durum kurtarma gerçekleştirebilirsiniz.<br/><br/> Bu senaryo Azure portalında veya PowerShell kullanarak dağıtabilirsiniz.<br/><br/> Virtual Machine Manager tarafından yönetilen Hyper-V konağına, aynı zamanda bir ikincil şirket içi siteye olağanüstü durum kurtarma gerçekleştirebilirsiniz. Bu senaryo hakkında daha fazla bilgi edinmek için [Bu öğreticide](hyper-v-vmm-disaster-recovery.md).
Hyper-V olmadan Virtual Machine Manager | Virtual Machine Manager tarafından yönetilmeyen Hyper-V konaklarında çalışan VM'ler için Azure'da olağanüstü durum kurtarma gerçekleştirebilirsiniz.<br/><br/> Bu senaryo Azure portalında veya PowerShell kullanarak dağıtabilirsiniz.


## <a name="on-premises-servers"></a>Şirket içi sunucular

**Sunucu** | **Gereksinimler** | **Ayrıntılar**
--- | --- | ---
(Çalışan Virtual Machine Manager olmadan) Hyper-V |  Windows Server 2019, Windows Server 2016 (dahil olmak üzere Sunucu Çekirdeği yüklemesi), en son güncelleştirmeleri içeren Windows Server 2012 R2 | Windows Server 2012 R2 ile zaten yapılandırdıysanız / veya SCVMM 2012 R2 ile Azure Site Recovery ve işletim sistemi, yükseltmeyi planlıyorsanız, lütfen Kılavuzu izleyin [belgeleri.](upgrade-2012R2-to-2016.md) 
(Çalışan Virtual Machine Manager ile) Hyper-V | 2019, Virtual Machine Manager 2016 Virtual Machine Manager 2012 R2 Virtual Machine Manager | Virtual Machine Manager kullandıysanız, Windows Server 2019 konak Virtual Machine Manager 2019 yönetilmelidir. Benzer şekilde, Virtual Machine Manager 2016'da Windows Server 2016 ana yönetilmelidir.<br/><br/>


## <a name="replicated-vms"></a>Çoğaltılan VM'ler


Aşağıdaki tabloda, VM desteği özetler. Site Recovery, desteklenen bir işletim sisteminde çalışan tüm iş yüklerini destekler.

 **Bileşen** | **Ayrıntılar**
--- | ---
VM yapılandırması | Azure'a çoğaltma Vm'leri karşılamalıdır [Azure gereksinimleri](#azure-vm-requirements).
Konuk işletim sistemi | Tüm konuk işletim sistemi [Azure için desteklenen](https://docs.microsoft.com/azure/cloud-services/cloud-services-guestos-update-matrix#family-5-releases)...<br/><br/> Windows Server 2016 Nano Server desteklenmez.


## <a name="vmdisk-management"></a>VM/Disk Yönetimi

**Eylem** | **Ayrıntılar**
--- | ---
Çoğaltılan Hyper-V VM'de disk yeniden boyutlandırma | Desteklenmiyor. Çoğaltmayı devre dışı bırakın, değişikliği yapın ve ardından sanal makine için çoğaltmayı yeniden etkinleştirin.
Çoğaltılan Hyper-V VM disk ekleme | Desteklenmiyor. Çoğaltmayı devre dışı bırakın, değişikliği yapın ve ardından sanal makine için çoğaltmayı yeniden etkinleştirin.

## <a name="hyper-v-network-configuration"></a>Hyper-V ağ yapılandırması

**Bileşen** | **Hyper-V ile Virtual Machine Manager** | **Hyper-V olmadan Virtual Machine Manager**
--- | --- | ---
Konak ağı: NIC grubu oluşturma | Evet | Evet
Konak ağı: VLAN | Evet | Evet
Konak ağı: IPv4 | Evet | Evet
Konak ağı: IPv6 | Hayır | Hayır
Konuk VM ağı: NIC grubu oluşturma | Hayır | Hayır
Konuk VM ağı: IPv4 | Evet | Evet
Konuk VM ağı: IPv6 | Hayır | Evet
Konuk VM ağı: Statik IP (Windows) | Evet | Evet
Konuk VM ağı: Statik IP (Linux) | Hayır | Hayır
Konuk VM ağı: Multi-NIC | Evet | Evet



## <a name="azure-vm-network-configuration-after-failover"></a>(Yük devretme sonra) Azure sanal makine ağ yapılandırması

**Bileşen** | **Hyper-V ile Virtual Machine Manager** | **Hyper-V olmadan Virtual Machine Manager**
--- | --- | ---
Azure ExpressRoute | Evet | Evet
ILB | Evet | Evet
ELB | Evet | Evet
Azure Traffic Manager | Evet | Evet
Multi-NIC | Evet | Evet
Ayrılmış IP | Evet | Evet
IPv4 | Evet | Evet
Kaynak IP adresini koruma | Evet | Evet
Azure sanal ağ hizmet uç noktaları<br/> (Azure depolama güvenlik duvarlarını) | Evet | Evet
Hızlandırılmış Ağ | Hayır | Hayır


## <a name="hyper-v-host-storage"></a>Hyper-V konağında depolama

**Depolama** | **Hyper-V ile Virtual Machine Manager** | **Hyper-V olmadan Virtual Machine Manager**
--- | --- | --- 
NFS | NA | NA
SMB 3.0 | Evet | Evet
SAN (İSCSI) | Evet | Evet
Çok yollu (MPIO). Test:<br></br> Microsoft DSM, EMC PowerPath 5.7 SP4<br/><br/> EMC PowerPath DSM CLARiiON için | Evet | Evet

## <a name="hyper-v-vm-guest-storage"></a>Hyper-V VM Konuk depolama

**Depolama** | **Hyper-V ile Virtual Machine Manager** | **Hyper-V olmadan Virtual Machine Manager**
--- | --- | ---
VMDK | NA | NA
VHD/VHDX | Evet | Evet
2. nesil VM | Evet | Evet
EFI/UEFI'YE| Evet | Evet
Küme diski paylaşılan | Hayır | Hayır
Şifrelenmiş diski | Hayır | Hayır
NFS | NA | NA
SMB 3.0 | Hayır | Hayır
RDM | NA | NA
Disk > 1 TB | Evet, 4.095 GB'a kadar | Evet, 4.095 GB'a kadar
Disk: 4K mantıksal ve fiziksel kesimi | Desteklenmeyen: Gen 1/Gen 2 | Desteklenmeyen: Gen 1/Gen 2
Disk: 4K mantıksal ve fiziksel 512 baytlık kesim | Evet |  Evet
Mantıksal birim yönetimi (LVM). LVM'yi veri diskleri üzerinde desteklenir. Azure, yalnızca tek bir işletim sistemi diski sağlar. | Evet | Evet
Bölüştürülmüş bir disk birimi > 1 TB | Evet | Evet
Depolama alanları | Evet | Evet
Sık erişimli Ekle/Kaldır disk | Hayır | Hayır
Diski hariç tutma | Evet | Evet
Çok yollu (MPIO) | Evet | Evet

## <a name="azure-storage"></a>Azure Storage

**Bileşen** | **Hyper-V ile Virtual Machine Manager** | **Hyper-V olmadan Virtual Machine Manager**
--- | --- | ---
Yerel olarak yedekli depolama | Evet | Evet
Coğrafi olarak yedekli depolama | Evet | Evet
Okuma erişimli coğrafi olarak yedekli depolama | Evet | Evet
Seyrek erişimli depolama | Hayır | Hayır
Sık erişimli depolama| Hayır | Hayır
Blok blobları | Hayır | Hayır
(SSE) bekleyen şifreleme| Evet | Evet
Premium depolama | Evet | Evet
İçeri/dışarı aktarma hizmeti | Hayır | Hayır
Hedef depolama/önbellek depolama hesabı (çoğaltma verilerini depolamak için kullanılan) üzerinde yapılandırılan sanal ağlar için Azure depolama güvenlik duvarları | Hayır | Hayır


## <a name="azure-compute-features"></a>Azure işlem özellikleri

**Özelliği** | **Hyper-V ile Virtual Machine Manager** | **Hyper-V olmadan Virtual Machine Manager**
--- | --- | ---
Kullanılabilirlik kümeleri | Evet | Evet
HUB | Evet | Evet  
Yönetilen diskler | Evet, yük devretme için.<br/><br/> Yönetilen diskler yeniden çalışma desteklenmez. | Evet, yük devretme için.<br/><br/> Yönetilen diskler yeniden çalışma desteklenmez.

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure'a Çoğalttığınız şirket içi Vm'leri bu tabloda özetlenen Azure VM gereksinimleri karşılaması gerekir.

**Bileşen** | **Gereksinimler** | **Ayrıntılar**
--- | --- | ---
Konuk işletim sistemi | Site Recovery tüm işletim sistemlerini destekler [Azure tarafından desteklenen](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).  | Desteklenmeyen başarısız önkoşulları denetleyin.
Konuk işletim sistemi mimarisi | 64 bit | Desteklenmeyen başarısız önkoşulları denetleyin.
İşletim sistemi disk boyutu | 1. kuşak VM'ler için 2.048 GB.<br/><br/> 2. kuşak VM'ler için en fazla 300 GB.  | Desteklenmeyen başarısız önkoşulları denetleyin.
İşletim sistemi disk sayısı | 1 | Desteklenmeyen başarısız önkoşulları denetleyin.
Veri diski sayısı | 16 ya da daha az  | Desteklenmeyen başarısız önkoşulları denetleyin.
Veri diski VHD boyutu | 4.095 GB'a kadar | Desteklenmeyen başarısız önkoşulları denetleyin.
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir |
Paylaşılan VHD | Desteklenmiyor | Desteklenmeyen başarısız önkoşulları denetleyin.
FC diski | Desteklenmiyor | Desteklenmeyen başarısız önkoşulları denetleyin.
Sabit disk biçimi | VHD <br/><br/> VHDX | Azure'a yük devretme sırasında site Recovery VHDX otomatik olarak VHD'ye dönüştürür. Yeniden şirket içine başarısız olursa, sanal makineler VHDX biçimini kullanmaya devam eder.
BitLocker | Desteklenmiyor | Bir sanal makine için çoğaltmayı etkinleştirmeden önce BitLocker'ı devre dışı bırakılması gerekir.
VM adı | 1-63 karakter. Harfler, sayılar ve kısa çizgilerden oluşabilir. VM adı bir harf veya sayıyla başlamalı ve bitmelidir. | Site Recovery VM özelliklerini değeri güncelleştirin.
VM türü | 1. nesil<br/><br/> Nesil 2--Windows | 2. kuşak Vm'leri bir işletim sistemi disk türüyle (VHDX biçimlendirilmiş bir veya iki veri birimi içeren) temel ve 300 GB'tan az disk alanı desteklenir.<br></br>Linux 2. kuşak VM'ler desteklenmez. [Daha fazla bilgi edinin](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).|

## <a name="recovery-services-vault-actions"></a>Kurtarma Hizmetleri kasası eylemleri

**Eylem** |  **Hyper-V ile Virtual Machine Manager** | **Hyper-V olmadan Virtual Machine Manager**
--- | --- | ---
Kasa kaynak grupları arasında taşıma<br/><br/> İçinde ve arasında abonelikler | Hayır | Hayır
Depolama, ağ, Azure Vm'leri kaynak grupları arasında taşıma<br/><br/> İçinde ve arasında abonelikler | Hayır | Hayır

> [!NOTE]
> (SCVMM olan/olmayan yönetilen) Hyper-Vm'leri, şirket içinden Azure'a çoğaltırken, yalnızca bir AD kiracısına bir belirli ortam - Hyper-V sitesi veya SCVMM uygun olarak çoğaltma yapabilirsiniz.


## <a name="provider-and-agent"></a>Sağlayıcı ve aracı

Dağıtımınız bu makaledeki ayarlarla uyumlu olduğundan emin olmak için en son sağlayıcı ve Aracı sürümleri çalıştırdığınızdan emin olun.

**Ad** | **Açıklama** | **Ayrıntılar**
--- | --- | --- 
Azure Site Recovery sağlayıcısı | Şirket içi sunucular ile Azure arasındaki iletişimi düzenler <br/><br/> Hyper-V Virtual Machine Manager ile: Virtual Machine Manager sunucularında yüklü<br/><br/> Hyper-V Virtual Machine Manager olmadan: Hyper-V konaklarında yüklü| En son sürümü: 5.1.2700.1 (Azure portalından kullanılabilir)<br/><br/> [En son özellikler ve düzeltmeler](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery)
Microsoft Azure kurtarma Hizmetleri Aracısı | Hyper-V Vm'leri ve Azure arasında çoğaltma koordinatları<br/><br/> Şirket içi Hyper-V sunucuları (ile arama veya Virtual Machine Manager olmadan) yüklü | En son aracıyı portalından kullanılabilir






## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [Azure'u hazırlama](tutorial-prepare-azure.md) şirket içi Hyper-V Vm'lerinin olağanüstü durum kurtarması için.
