---
title: "VMware sanal makinelerini çoğaltma veya Azure Site Recovery ile ikincil VMware siteye fiziksel sunucular için destek matrisi | Microsoft Docs"
description: "Azure Site Recovery ile ikincil siteye VMware/fiziksel sunucu çoğaltma desteği özetler"
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: raynew
ms.openlocfilehash: b51a4573ad7a8461b7261f08d94639d2030492d9
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="support-matrix-for-replication-of-vmware-vms-and-physical-servers-to-a-secondary-site"></a>Çoğaltma işlemi VMware Vm'lerini ve fiziksel sunucuları ikincil bir siteye için destek matrisi

Bu makalede kullandığınızda nelerin desteklendiği özetlenmektedir [Azure Site Recovery](site-recovery-overview.md) VMware Vm'leri veya Windows/Linux fiziksel sunucuları ikincil bir VMware siteye çoğaltmak için hizmet.

- VMware Vm'lerini veya fiziksel sunucuları Azure'a çoğaltmak istiyorsanız, gözden [Bu destek matrisi](vmware-physical-azure-support-matrix.md).
- Hyper-V Vm'lerini ikincil bir siteye çoğaltmak istiyorsanız, gözden [Bu destek matrisi](hyper-v-azure-support-matrix.md).

> [!NOTE]
> Şirket içi VMware Vm'lerini ve fiziksel sunucuların çoğaltma Inmage Scout tarafından sağlanır. Inmage Scout Azure Site Recovery hizmeti abonelikte dahil edilir.


## <a name="host-servers"></a>Ana bilgisayar sunucuları

**İşletim Sistemi** | **Ayrıntılar**
--- | ---
vCenter sunucusu | vCenter 5.5, 6.0 ve 6.5<br/><br/> 6.0 veya 6.5 çalıştırırsanız, yalnızca 5.5 özellikleri desteklediğini unutmayın.


## <a name="replicated-vm-support"></a>VM destek çoğaltılan

Site Recovery ile çoğaltılan makineler için işletim sistemi desteği aşağıdaki tabloda özetlenmiştir. Desteklenen işletim sisteminde herhangi bir iş yükü çalıştırabilir.

**İşletim Sistemi** | **Ayrıntılar**
--- | ---
Windows Server | 64-bit Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Itanium tabanlı sistemler için Windows Server 2008 R2 ile en az SP1.
Linux | Red Hat Enterprise Linux 6.7, 6.8, 6.9, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6, 6.7, 6.8, 6.9, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, Red Hat uyumlu çekirdek veya kesilemeyen kurumsal çekirdek sürüm 3 (UEK3) çalıştıran 6,8 <br/><br/> SUSE Linux Enterprise Server 11 SP3, 11 SP4 


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
NFS | Evet 
SMB 3.0 | Yok 
SAN (ISCSI) | Evet 
Çok yollu (MPIO) | Evet 

### <a name="guest-or-physical-server-storage"></a>Konuk veya fiziksel sunucu depolama

**Yapılandırma** | **Destekleniyor** 
--- | --- 
VMDK | Evet 
VHD/VHDX | Yok 
Gen 2 VM | Yok 
Küme diskini paylaşılan | Evet 
Şifrelenmiş disk | Hayır 
UEFI| Evet 
NFS | Hayır 
SMB 3.0 | Hayır 
RDM | Evet 
Disk > 1 TB | Evet 
Şeritli disk > 1 TB birimle<br/><br/> LVM | Evet 
Depolama alanları | Hayır 
Sık kullanılan Ekle/Kaldır disk | Evet 
Diski hariç tutma | Evet 
Çok yollu (MPIO) | Yok 

## <a name="vaults"></a>Vaults

**Eylem** | **Destekleniyor** 
--- | --- 
Kaynak grupları (içinde veya abonelikler arasında) arasında taşıma kasaları | Hayır 
Depolama, ağ, Azure Vm'leri kaynak grupları (içinde veya abonelikler arasında) arasında taşıma | Hayır 

## <a name="mobility-service-and-updates"></a>Mobility hizmeti ve güncelleştirmeleri

Mobility hizmetinin şirket içi VMware sunucuları veya fiziksel sunucuları ve ikincil site arasında çoğaltma düzenler. Çoğaltma ayarladığınızda, Mobility hizmeti ve diğer bileşenleri'nın en son sürümüne sahip emin olmanız gerekir.

**Güncelleştirme** | **Ayrıntılar** 
--- | --- 
Scout güncelleştirmeleri | [Hakkında bilgi edinin ve yükleyin](/vmware-physical-secondary-disaster-recovery.md#updates) en son Scout güncelleştirmeleri | Scout güncelleştirmelerin toplu.
Bileşen güncelleştirmeleri | Scout güncelleştirmeleri RX sunucu, yapılandırma sunucusu, işlem ve ana hedef sunucusu, vContinuum sunucuları ve korumak istediğiniz kaynak sunucuları dahil olmak üzere tüm bileşenler için güncelleştirmeleri içerir.<br/><br/> [Daha fazla bilgi edinin](vmware-physical-secondary-disaster-recovery.md#download-and-install-component-updates).


## <a name="next-steps"></a>Sonraki adımlar

Karşıdan [Inmage Scout Kullanıcı Kılavuzu](https://aka.ms/asr-scout-user-guide)

- [VMM bulutlarındaki Hyper-V Vm'lerini ikincil bir siteye çoğaltma](tutorial-vmm-to-vmm.md)
- [VMware VM’lerini ve fiziksel sunucuları ikincil bir siteye çoğaltma](tutorial-vmware-to-vmware.md)
