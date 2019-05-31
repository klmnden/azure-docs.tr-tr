---
title: VMware Vm'lerini veya fiziksel sunucuları Azure Site Recovery ile ikincil VMware sitesindeki olağanüstü durum kurtarma için destek matrisi | Microsoft Docs
description: VMware Vm'lerini ve fiziksel sunucuları Azure Site Recovery ile ikincil bir siteye olağanüstü durum kurtarma desteği özetler.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 742f1359d928aa05a8b8d36bde2ccf022db93b79
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418244"
---
# <a name="support-matrix-for-disaster-recovery-of-vmware-vms-and-physical-servers-to-a-secondary-site"></a>VMware Vm'lerini ve fiziksel sunucuları ikincil bir siteye olağanüstü durum kurtarma için destek matrisi

Bu makalede, kullandığınız zaman desteklenen özellikler özetlenmektedir [Azure Site Recovery](site-recovery-overview.md) fiziksel sunucuları ikincil VMware sitesindeki VMware Vm'leri veya Windows/Linux için olağanüstü durum kurtarma hizmeti.

- VMware Vm'lerini veya fiziksel sunucuları Azure'a çoğaltmak istiyorsanız, gözden [Bu destek matrisi](vmware-physical-azure-support-matrix.md).
- Hyper-V Vm'lerini ikincil bir siteye çoğaltmak istiyorsanız, gözden [Bu destek matrisi](hyper-v-azure-support-matrix.md).

> [!NOTE]
> Şirket içi VMware Vm'leri ve fiziksel sunucuların çoğaltma, Inmage Scout tarafından sağlanır. Inmage Scout Azure Site Recovery hizmeti aboneliğinize dahildir.


## <a name="host-servers"></a>Ana bilgisayar sunucuları

**İşletim sistemi** | **Ayrıntılar**
--- | ---
vCenter sunucusu | vCenter 5.5, 6.0 ve 6.5<br/><br/> 6.0 veya 6.5 çalıştırırsanız, yalnızca 5.5 özellikleri desteklediğini unutmayın.


## <a name="replicated-vm-support"></a>Çoğaltılan VM desteği

Aşağıdaki tabloda, Site Recovery'ye çoğaltılan makineler için işletim sistemi desteği özetler. Tüm iş yüklerini desteklenen bir işletim sisteminde çalışıyor olabilir.

**İşletim sistemi** | **Ayrıntılar**
--- | ---
Windows Server | 64 bit Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Itanium tabanlı sistemler için Windows Server 2008 R2 ile en az SP1.
Linux | Red Hat Enterprise Linux 6.7, 6.8, 6.9, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6, 6.7, 6.8, 6.9, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, Red Hat uyumlu çekirdek veya kesilemeyen Enterprise çekirdeği sürüm 3 (UEK3) çalıştıran 6,8 <br/><br/> SUSE Linux Enterprise Server 11 SP3, 11 SP4 


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
NFS | Evet 
SMB 3.0 | Yok 
SAN (İSCSI) | Evet 
Çok yollu (MPIO) | Evet 

### <a name="guest-or-physical-server-storage"></a>Konuk veya fiziksel sunucu depolama

**Yapılandırma** | **Destekleniyor** 
--- | --- 
VMDK | Evet 
VHD/VHDX | Yok 
Gen 2 VM | Yok 
Küme diski paylaşılan | Evet 
Şifrelenmiş diski | Hayır 
UEFI| Evet 
NFS | Hayır 
SMB 3.0 | Hayır 
RDM | Evet 
Disk > 1 TB | Evet 
Bölüştürülmüş bir disk > 1 TB birim<br/><br/> LVM | Evet 
Depolama alanları | Hayır 
Sık erişimli Ekle/Kaldır disk | Evet 
Diski hariç tutma | Evet 
Çok yollu (MPIO) | Yok 

## <a name="vaults"></a>Kasalar

**Eylem** | **Destekleniyor** 
--- | --- 
Kasa (içinde veya abonelikler arasında) kaynak grupları arasında taşıma | Hayır 
Depolama, ağ, Azure Vm'leri (içinde veya abonelikler arasında) kaynak grupları arasında taşıma | Hayır 

## <a name="mobility-service-and-updates"></a>Mobility hizmeti ve güncelleştirmeler

Mobility hizmeti, şirket içi VMware sunucularını veya fiziksel sunucuları ve ikincil site arasında çoğaltma düzenler. Çoğaltma ayarlamadan, Mobility hizmetinin ve diğer bileşenlerin en son sürümü kullandığınızdan emin olmanız gerekir.

| **Güncelleştirme** | **Ayrıntılar** |
| --- | --- |
|Scout güncelleştirmeleri | Scout güncelleştirmeleri birikmeli özelliktedir. <br/><br/> [Öğrenin ve indirin](vmware-physical-secondary-disaster-recovery.md#updates) Scout Haberleri |
|Bileşen güncelleştirmeleri | Scout güncelleştirmeleri RX sunucu, yapılandırma sunucusu, işlem ve ana hedef sunucular, vContinuum sunucuları ve korumak istediğiniz kaynak sunucular da dahil olmak üzere tüm bileşenler için güncelleştirmeleri içerir.<br/><br/> [Daha fazla bilgi edinin](vmware-physical-secondary-disaster-recovery.md#download-and-install-component-updates).|


## <a name="next-steps"></a>Sonraki adımlar

İndirme [Inmage Scout Kullanıcı Kılavuzu](https://aka.ms/asr-scout-user-guide)

- [VMM bulutlarındaki Hyper-V Vm'lerini ikincil bir siteye çoğaltma](tutorial-vmm-to-vmm.md)
- [VMware VM’lerini ve fiziksel sunucuları ikincil bir siteye çoğaltma](tutorial-vmware-to-vmware.md)
