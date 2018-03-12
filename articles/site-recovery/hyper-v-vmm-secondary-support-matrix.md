---
title: "Vmm'de Hyper-V sanal makineleri çoğaltma için destek matrisi bulut Azure Site Recovery ile ikincil siteye | Microsoft Docs"
description: "Azure Site Recovery ile ikincil siteye VMM bulutlarındaki Hyper-V VM çoğaltma desteği özetler."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: raynew
ms.openlocfilehash: 767b0e76b73c082ddb75374f51700b85272f713e
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="support-matrix-for-replication-of-hyper-v-vms-to-a-secondary-site"></a>Hyper-V sanal makineleri çoğaltma ikincil bir site için destek matrisi

Bu makalede kullandığınızda nelerin desteklendiği özetlenmektedir [Azure Site Recovery](site-recovery-overview.md) ikincil bir siteyi System Center Virtual Machine Manager (VMM) bulutlarında yönetilen Hyper-V sanal makineleri çoğaltmak için hizmeti. Hyper-V Vm'lerini Azure'a çoğaltmak istiyorsanız, gözden [Bu destek matrisi](hyper-v-azure-support-matrix.md).

> [!NOTE]
> VMM bulutlarındaki Hyper-V konakları yönetildiğinde, ikincil bir siteye yalnızca çoğaltabilirsiniz.

  

## <a name="host-servers"></a>Ana bilgisayar sunucuları

**İşletim Sistemi** | **Ayrıntılar**
--- | ---
Windows Server 2012 R2 | Sunucularının en son güncelleştirmeleri çalıştırıyor olması gerekir.
Windows Server 2016 |  Windows Server 2016 ve 2012 R2 ana bilgisayarları karışımına sahip VMM 2016 Bulutlar şu anda desteklenmiyor.<br/><br/> System Center 2016 System Center 2012 R2 VMM 2012 R2'den yükseltilmiş dağıtımları şu anda desteklenmiyor.


## <a name="replicated-vm-support"></a>VM destek çoğaltılan

Site Recovery ile çoğaltılan makineler için işletim sistemi desteği aşağıdaki tabloda özetlenmiştir. Desteklenen işletim sisteminde herhangi bir iş yükü çalıştırabilir.

**Windows sürümü** | **Hyper-V (VMM ile)**
--- | ---
Windows Server 2016 | Herhangi bir işletim sistemi Konuk [Hyper-V tarafından desteklenen](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows) Windows Server 2016 
Windows Server 2012 R2 | Herhangi bir işletim sistemi Konuk [Hyper-V tarafından desteklenen](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027%28v%3dws.11%29) Windows Server 2012 R2

## <a name="linux-machine-storage"></a>Linux makine depolama

Yalnızca aşağıdaki depolama ile Linux makineler çoğaltılabilir:

- Dosya sistemi (EXT3, ETX4, ReiserFS, XFS).
- Çok yollu yazılım aygıtı Eşleştiricisi.
- Birim Yöneticisi (LVM2).
- HP CCISS denetleyicisi depolama ile fiziksel sunucuları desteklenmez.
- ReiserFS dosya sistemi yalnızca SUSE Linux Enterprise Server 11 SP3 üzerinde desteklenir.

## <a name="network-configuration---hostguest-vm"></a>Ağ yapılandırması - konak/Konuk VM

**Yapılandırma** | **Destekleniyor**  
--- | --- 
Ana bilgisayar - NIC ekibi oluşturma | Evet 
Ana bilgisayar - VLAN | Evet 
Ana bilgisayar - IPv4 | Evet 
Ana bilgisayar - IPv6 | Hayır 
Konuk VM - NIC ekibi oluşturma | Hayır
Konuk VM - IPv4 | Evet
Konuk VM - IPv6 | Hayır
Gues VM Windows/Linux - statik IP adresi | Evet
Guest VM - Multi-NIC | Evet


## <a name="storage"></a>Depolama

### <a name="host-storage"></a>Ana bilgisayar depolama

**Depolama (ana bilgisayar)** | **Destekleniyor**
--- | --- 
NFS | Yok
SMB 3.0 |  Evet
SAN (ISCSI) | Evet
Çok yollu (MPIO) | Evet

### <a name="guest-or-physical-server-storage"></a>Konuk veya fiziksel sunucu depolama

**Yapılandırma** | **Destekleniyor**
--- | --- | 
VMDK |  Yok
VHD/VHDX | Evet (en fazla 16 disk)
Gen 2 VM | Evet
Küme diskini paylaşılan | Hayır
Şifrelenmiş disk | Hayır
UEFI| Yok
NFS | Hayır
SMB 3.0 | Hayır
RDM | Yok
Disk > 1 TB | Evet
Şeritli disk > 1 TB birimle<br/><br/> LVM | Evet
Depolama alanları | Evet
Sık kullanılan Ekle/Kaldır disk | Hayır
Diski hariç tutma | Evet
Çok yollu (MPIO) | Evet

## <a name="vaults"></a>Vaults

**Eylem** | **Destekleniyor**
--- | --- 
Kaynak grupları (içinde veya abonelikler arasında) arasında taşıma kasaları |  Hayır
Depolama, ağ, Azure Vm'leri kaynak grupları (içinde veya abonelikler arasında) arasında taşıma | Hayır

## <a name="azure-site-recovery-provider"></a>Azure Site Recovery sağlayıcısı

Sağlayıcı VMM sunucuları arasındaki iletişimi düzenler. 

**en son** | **Güncelleştirmeler**
--- | --- | --- | --- | ---
5.1.19 ([portalından kullanılabilir](http://aka.ms/downloaddra) | [En son özellikleri ve düzeltmeleri](https://support.microsoft.com/kb/3155002)



## <a name="next-steps"></a>Sonraki adımlar

[VMM bulutlarındaki Hyper-V Vm'lerini ikincil bir siteye çoğaltma](tutorial-vmm-to-vmm.md)

