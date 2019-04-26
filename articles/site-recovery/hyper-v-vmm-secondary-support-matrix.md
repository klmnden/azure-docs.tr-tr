---
title: Vmm'de Hyper-V Vm'lerinin olağanüstü durum kurtarması için destek matrisi bulut Azure Site Recovery ile ikincil bir siteye | Microsoft Docs
description: Azure Site Recovery ile ikincil bir siteye VMM bulutlarındaki Hyper-V VM çoğaltması için desteği özetler.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: raynew
ms.openlocfilehash: 60ca12e5b362a37eb9f85c9a0d1fc23ca99e9edc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60362348"
---
# <a name="support-matrix-for-disaster-recovery-of-hyper-v-vms-to-a-secondary-site"></a>Hyper-V Vm'lerini ikincil bir siteye olağanüstü durum kurtarması için destek matrisi

Bu makalede, kullandığınız zaman desteklenen özellikler özetlenmektedir [Azure Site Recovery](site-recovery-overview.md) Hyper-V Vm'lerini çoğaltmak için hizmet ikincil bir siteyi System Center Virtual Machine Manager (VMM) bulutlarında yönetilen. Hyper-V Vm'lerini Azure'a çoğaltmak istiyorsanız, gözden [Bu destek matrisi](hyper-v-azure-support-matrix.md).

> [!NOTE]
> Hyper-V konaklarınız VMM bulutlarında yönetilen yalnızca ikincil bir siteye çoğaltabilirsiniz.

  

## <a name="host-servers"></a>Ana bilgisayar sunucuları

**İşletim sistemi** | **Ayrıntılar**
--- | ---
Windows Server 2012 R2 | Sunucularının, en son güncelleştirmeleri çalıştırıyor olması gerekir.
Windows Server 2016 |  VMM 2016'yı Windows Server 2016 ve 2012 R2 konaklarının bir karışımını bulutlarıyla şu anda desteklenmemektedir.<br/><br/> System Center 2012 R2 VMM 2012 R2'den System Center 2016 için yükseltme dağıtımı şu anda desteklenmemektedir.


## <a name="replicated-vm-support"></a>Çoğaltılan VM desteği

Aşağıdaki tabloda, Site Recovery'ye çoğaltılan makineler için işletim sistemi desteği özetler. Tüm iş yüklerini desteklenen bir işletim sisteminde çalışıyor olabilir.

**Windows sürümü** | **Hyper-V (VMM ile)**
--- | ---
Windows Server 2016 | Tüm konuk işletim sistemi [Hyper-V tarafından desteklenen](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows) Windows Server 2016 
Windows Server 2012 R2 | Tüm konuk işletim sistemi [Hyper-V tarafından desteklenen](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027%28v%3dws.11%29) Windows Server 2012 R2

## <a name="linux-machine-storage"></a>Linux makine depolama

Yalnızca Linux makineleri aşağıdaki depolama ile çoğaltılmış olabilir:

- Dosya sistemi (EXT3, ETX4, ReiserFS, XFS).
- Çok yollu yazılım cihaz Eşleştirici.
- Birim Yöneticisi (LVM2).
- HP CCISS denetleyicisi depolama ile fiziksel sunucuları desteklenmez.
- ReiserFS dosya sistemi yalnızca SUSE Linux Enterprise Server 11 SP3 üzerinde desteklenir.

## <a name="network-configuration---hostguest-vm"></a>Ağ yapılandırması - konak/Konuk VM

**Yapılandırma** | **Destekleniyor**  
--- | --- 
-Konak NIC ekibi oluşturma | Evet 
Host - VLAN | Evet 
Ana bilgisayar - IPv4 | Evet 
Ana bilgisayar - IPv6 | Hayır 
Konuk VM - NIC grubu oluşturma | Hayır
Konuk VM - IPv4 | Evet
Konuk VM - IPv6 | Hayır
Konuk VM Windows/Linux - statik IP adresi | Evet
Konuk VM - Multi-NIC | Evet


## <a name="storage"></a>Depolama

### <a name="host-storage"></a>Konak depolama alanı

**Depolama (ana bilgisayarı)** | **Destekleniyor**
--- | --- 
NFS | Yok
SMB 3.0 |  Evet
SAN (İSCSI) | Evet
Çok yollu (MPIO) | Evet

### <a name="guest-or-physical-server-storage"></a>Konuk veya fiziksel sunucu depolama

**Yapılandırma** | **Destekleniyor**
--- | --- | 
VMDK |  Yok
VHD/VHDX | Evet (en fazla 16 disk)
Gen 2 VM | Evet
Küme diski paylaşılan | Hayır
Şifrelenmiş diski | Hayır
UEFI| Yok
NFS | Hayır
SMB 3.0 | Hayır
RDM | Yok
Disk > 1 TB | Evet
Bölüştürülmüş bir disk > 1 TB birim<br/><br/> LVM | Evet
Depolama alanları | Evet
Sık erişimli Ekle/Kaldır disk | Hayır
Diski hariç tutma | Evet
Çok yollu (MPIO) | Evet

## <a name="vaults"></a>Kasalar

**Eylem** | **Destekleniyor**
--- | --- 
Kasa (içinde veya abonelikler arasında) kaynak grupları arasında taşıma |  Hayır
Depolama, ağ, Azure Vm'leri (içinde veya abonelikler arasında) kaynak grupları arasında taşıma | Hayır

## <a name="azure-site-recovery-provider"></a>Azure Site Recovery sağlayıcısı

Sağlayıcı, VMM sunucuları arasındaki iletişimi düzenler. 

**en son** | **Güncelleştirmeler**
--- | --- 
5.1.19 ([portalında kullanılabilir](https://aka.ms/downloaddra) | [En son özellikler ve düzeltmeler](https://support.microsoft.com/kb/3155002)



## <a name="next-steps"></a>Sonraki adımlar

[VMM bulutlarındaki Hyper-V Vm'lerini ikincil bir siteye çoğaltma](tutorial-vmm-to-vmm.md)

