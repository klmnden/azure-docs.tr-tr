---
title: "Azure Site Recovery ile ikincil siteye çoğaltma için destek matrisi | Microsoft Docs"
description: "Azure Site Recovery için desteklenen işletim sistemleri ve bileşenler özetler"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/30/2017
ms.author: raynew
ms.openlocfilehash: c0f86e13e21f2af323e0a306b381054b6eb76755
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="support-matrix-for-replication-to-a-secondary-site-with-azure-site-recovery"></a>Azure Site Recovery ile ikincil siteye çoğaltma için destek matrisi

Bu makalede kullandığınızda nelerin desteklendiği özetlenmektedir [Azure Site Recovery](site-recovery-overview.md) ikincil şirket içi siteye çoğaltmak için hizmet.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

**Dağıtım** | **Ayrıntılar** 
--- | ---
**Vmware'den vmware'e** | İkincil VMware sitesi için olağanüstü durum kurtarma şirket içi VMware vm'lerinin.<br/><br/> Karşıdan [Inmage Scout Kullanıcı Kılavuzu](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf)
**Hyper-V'den Hyper-V'ye** | İkincil VMM bulutu için olağanüstü durum kurtarma VMM bulutlarındaki şirket içi Hyper-V vm'lerinde.<br></br> VMM desteklenmiyor.



  

## <a name="host-servers"></a>Ana bilgisayar sunucuları

**Dağıtım** | **Destek**
--- | ---
**VMware sanal/fiziksel sunucu** | vCenter 5.5 veya 6.0 (yalnızca 5.5 özellikleri için destek)
**VMM ile Hyper-V** | Son güncelleştirmeleri içeren Windows Server 2012 R2 ve Windows Server 2016.<br/><br/> Windows Server 2016 konaklar VMM 2016 tarafından yönetilmelidir.<br/><br/> Windows Server 2016 ve 2012 R2 ana bilgisayarları karışımına sahip VMM 2016 Bulutlar şu anda desteklenmiyor.<br/><br/> Şu anda System Center 2016 için mevcut bir VMM 2012 R2'in yükseltmenin dahil dağıtım desteklenmez.


## <a name="support-for-replicated-machine-os-versions"></a>Çoğaltılan makinenin işletim sistemi sürümleri için destek

Site Recovery ile çoğaltılan makineler için işletim sistemi desteği aşağıdaki tabloda özetlenmiştir. Desteklenen işletim sisteminde herhangi bir iş yükü çalıştırabilir.

**VMware/fiziksel sunucu** | **Hyper-V (VMM ile)**
--- | ---
64-bit Windows Server 2012 R2, Windows Server 2012, Itanium tabanlı sistemler için Windows Server 2008 R2 ile en az SP1<br/><br/> Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle 6.4 veya 6.5 çalıştıran Red Hat Enterprise Linux uyumlu çekirdek veya kesilemeyen kurumsal çekirdek sürüm 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 | Herhangi bir işletim sistemi Konuk [Hyper-V tarafından desteklenen](https://technet.microsoft.com/library/mt126277.aspx)

## <a name="linux-machine-storage"></a>Linux makine depolama

Yalnızca aşağıdaki depolama ile Linux makineler çoğaltılabilir:

- Dosya sistemi (EXT3, ETX4, ReiserFS, XFS).
- Çok yollu yazılım aygıtı Eşleştiricisi.
- Birim Yöneticisi (LVM2).
- HP CCISS denetleyicisi depolama ile fiziksel sunucuları desteklenmez.
- ReiserFS dosya sistemi yalnızca SUSE Linux Enterprise Server 11 SP3 üzerinde desteklenir.

## <a name="network-configuration"></a>Ağ yapılandırması

### <a name="hosts"></a>Ana bilgisayarlar

**Yapılandırma** | **VMware/fiziksel sunucu** | **Hyper-V (VMM ile)**
--- | --- | ---
NIC ekibi oluşturma | Evet | Evet
VLAN | Evet | Evet
IPv4 | Evet | Evet
IPv6 | Hayır | Hayır

### <a name="guest-vms"></a>Konuk sanal makineleri

**Yapılandırma** | **VMware/fiziksel sunucu** | **Hyper-V (VMM ile)**
--- | --- | ---
NIC ekibi oluşturma | Hayır | Hayır
IPv4 | Evet | Evet
IPv6 | Hayır | Hayır
Statik IP (Windows) | Evet | Evet
Statik IP (Linux) | Evet | Evet
Multi-NIC | Evet | Evet


## <a name="storage"></a>Depolama

### <a name="host-storage"></a>Ana bilgisayar depolama

**Depolama (ana bilgisayar)** | **VMware/fiziksel sunucu** | **Hyper-V (VMM ile)**
--- | --- | ---
NFS | Evet | Yok
SMB 3.0 | Yok | Evet
SAN (İSCSI) | Evet | Evet
Çok yollu (MPIO) | Evet | Evet

### <a name="guest-or-physical-server-storage"></a>Konuk veya fiziksel sunucu depolama

**Yapılandırma** | **VMware/fiziksel sunucu** | **Hyper-V (VMM ile)**
--- | --- | ---
VMDK | Evet | Yok
VHD/VHDX | Yok | Evet (en fazla 16 disk)
Gen 2 VM | Yok | Evet
Küme diskini paylaşılan | Evet  | Hayır
Şifrelenmiş disk | Hayır | Hayır
UEFI| Hayır | Yok
NFS | Hayır | Hayır
SMB 3.0 | Hayır | Hayır
RDM | Evet | Yok
Disk > 1 TB | Hayır | Evet
Şeritli disk > 1 TB birimle<br/><br/> LVM | Evet | Evet
Depolama alanları | Hayır | Evet
Sık kullanılan Ekle/Kaldır disk | Hayır | Hayır
Diski hariç tutma | Hayır | Evet
Çok yollu (MPIO) | Yok | Evet

## <a name="vaults"></a>kasaları

**Eylem** | **VMware/fiziksel sunucu** | **Hyper-V (VMM ile)**
--- | --- | ---
Kaynak grupları (içinde veya abonelikler arasında) arasında taşıma kasaları | Hayır | Hayır
Depolama, ağ, Azure Vm'leri kaynak grupları (içinde veya abonelikler arasında) arasında taşıma | Hayır | Hayır

## <a name="provider-and-agent"></a>Sağlayıcı ve aracı

**Ad** | **Açıklama** | **En son sürümü** | **Ayrıntılar**
--- | --- | --- | --- | ---
**Azure Site Recovery sağlayıcısı** | Şirket içi sunucular ile Azure arasındaki iletişimi düzenler <br/><br/> VMM sunucusu yoksa şirket içi VMM sunucularında veya Hyper-V sunucularında yüklü | 5.1.19 ([portalından kullanılabilir](http://aka.ms/downloaddra)) | [En son özellikleri ve düzeltmeleri](https://support.microsoft.com/kb/3155002)
**Mobility hizmeti** | Şirket içi VMware sunucuları veya fiziksel sunucuları ve ikincil site arasında çoğaltma koordinatları<br/><br/> VMware VM veya çoğaltmak istediğiniz fiziksel sunucularda yüklü  | Yok (portalından kullanılabilir) | Yok


## <a name="next-steps"></a>Sonraki adımlar

- [VMM bulutlarındaki Hyper-V Vm'lerini ikincil bir siteye çoğaltma](tutorial-vmm-to-vmm.md)
- [VMware VM’lerini ve fiziksel sunucuları ikincil bir siteye çoğaltma](tutorial-vmware-to-vmware.md)
