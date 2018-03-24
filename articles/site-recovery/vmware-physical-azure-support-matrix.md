---
title: Azure Site Recovery ile azure'a VMware Vm'lerini ve fiziksel sunucuları çoğaltma için destek matrisi | Microsoft Docs
description: Azure Site RECOVERY'yi kullanarak Azure'da VMware Vm'lerini ve fiziksel sunucu çoğaltma için bileşenleri ve desteklenen işletim sistemleri özetlenmektedir.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/20/2018
ms.author: raynew
ms.openlocfilehash: 6f2f28b36fdb3751a469d66f242f9fa2119f9ae8
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="support-matrix-for-vmware-and-physical-server-replication-to-azure"></a>VMware ve fiziksel sunucu çoğaltma Azure için destek matrisi

Bu makalede desteklenen bileşenleri ve VMware Vm'lerini olağanüstü durum kurtarma Azure ayarlarını kullanarak özetler [Azure Site Recovery](site-recovery-overview.md).

## <a name="supported-scenarios"></a>Desteklenen senaryolar

**Senaryo** | **Ayrıntılar**
--- | ---
VMware Sanal Makineleri | Şirket içi VMware Vm'leri için Azure olağanüstü durum kurtarma gerçekleştirebilirsiniz. Bu senaryo Azure portalında veya PowerShell kullanarak dağıtabilirsiniz.
Fiziksel sunucuları | Şirket içi Windows/Linux fiziksel sunucuları için Azure olağanüstü durum kurtarma gerçekleştirebilirsiniz. Bu senaryo Azure portalında dağıtabilirsiniz.

## <a name="on-premises-virtualization-servers"></a>Şirket içi sanallaştırma sunucuları

**Sunucu** | **Gereksinimleri** | **Ayrıntılar**
--- | --- | ---
VMware | vCenter Server 6.5, 6.0 veya 5.5 ya da vSphere 6.5, 6.0 veya 5.5 | Bir vCenter sunucusu kullanmanızı öneririz.
Fiziksel | Yok


## <a name="replicated-machines"></a>Çoğaltılan makineler

VMware Vm'lerini ve fiziksel sunucuları çoğaltma desteği aşağıdaki tabloda özetlenmiştir. Site Recovery, desteklenen bir işletim sistemine sahip bir makine üzerinde çalışan herhangi bir iş yükünü çoğaltmasını destekler.

**Bileşen** | **Ayrıntılar**
--- | ---
Makine ayarları | Azure'a makineler karşılamalıdır [Azure gereksinimleri](#azure-vm-requirements).
Windows işletim sistemi | 64-bit Windows Server 2016 (Sunucu Çekirdeği, masaüstü deneyimi olan sunucu), Windows Server 2012 R2, Windows Server 2012, Itanium tabanlı sistemler için Windows Server 2008 R2 ile en az SP1. Windows 2016 Nano Server desteklenmiyor.
Linux işletim sistemi | Red Hat Enterprise Linux: 5.2 için 5.11 ya, 6.1 için 6.9, 7.0 için 7.4 <br/><br/>CentOS: 5.2 için 5.11 ya, 6.1 için 6.9, 7.0 için 7.4 <br/><br/>Ubuntu 14.04 LTS server[ (çekirdek sürümleri desteklenir)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Ubuntu 16.04 LTS server[ (çekirdek sürümleri desteklenir)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Debian 7/Debian 8<br/><br/>Oracle Enterprise Linux 6.4, Red Hat uyumlu çekirdek veya kesilemeyen kurumsal çekirdek sürüm 3 (UEK3) çalıştıran 6.5 <br/><br/>SUSE Linux Enterprise Server 11 SP3, SUSE Linux Enterprise Server 11 SP4 <br/><br/>Çoğaltılan makineler SP3 SP4'e yükseltme desteklenmiyor. Yükseltmek için çoğaltma devre dışı bırakın ve yükseltmeden sonra yeniden etkinleştirin.

>[!NOTE]
>
> - Linux dağıtımları dağıtım alt sürüm yayın/güncelleştirmesinin bir parçası olan stok tekrar desteklenir.
>
> - Korumalı makineler arasında önemli Linux dağıtım sürümleri desteklenmez yükseltiliyor. Yükseltmek için çoğaltma devre dışı bırakmak, işletim sistemini yükseltin ve çoğaltma işlemini yeniden etkinleştirin.
>

### <a name="ubuntu-kernel-versions"></a>Ubuntu çekirdek sürümleri


**Desteklenen sürüm** | **Azure Site Recovery Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
14.04 LTS | 9.10 | 3.13.0-24-Generic 3.13.0-121-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-81-generic 4.4.0-21-Generic |
14.04 LTS | 9.11 | 3.13.0-24-Generic 3.13.0-128-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-91-generic 4.4.0-21-Generic |
14.04 LTS | 9.12 | 3.13.0-24-Generic 3.13.0-132-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-96-generic 4.4.0-21-Generic |
14.04 LTS | 9.13 | 3.13.0-24-Generic 3.13.0-137-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-104-generic 4.4.0-21-Generic |
16.04 LTS | 9.10 | 4.4.0-21-Generic 4.4.0-81-generic için<br/>4.8.0-34-Generic 4.8.0-56-generic için<br/>4.10.0-24-generic 4.10.0-14-Generic |
16.04 LTS | 9.11 | 4.4.0-21-Generic 4.4.0-91-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-32-generic 4.10.0-14-Generic |
16.04 LTS | 9.12 | 4.4.0-21-Generic 4.4.0-96-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-35-generic 4.10.0-14-Generic |
16.04 LTS | 9.13 | 4.4.0-21-Generic 4.4.0-104-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-42-generic 4.10.0-14-Generic |

## <a name="linux-file-systemsguest-storage"></a>Linux dosya sistemleri/Konuk depolama

**Bileşen** | **Destekleniyor**
--- | ---
Dosya sistemleri | ext3, ext4, ReiserFS (yalnızca Suse Linux Enterprise Server), XFS.
Birim Yöneticisi | LVM2.
Çok yollu yazılım | Cihaz Eşleştiricisi.
Paravirtualized depolama aygıtları | Parasanallaştırılmış sürücüler tarafından dışarı aktarılan cihazlar desteklenmez.
G/ç cihazların çok sıra engelle | Desteklenmiyor.
HP CCISS depolama denetleyicisi ile fiziksel sunucuları | Desteklenmiyor.
Dizinler | Bu dizinleri (varsa ayrı bölümleri/dosya-sistemleri ayarlanmış) tümü aynı işletim sistemi diski kaynak sunucuda olmalıdır: / (kök), / Boot/usr, /usr/local, /var, / etc.</br></br> / Boot bir disk bölümüne ve LVM birim olmaması gerekir.<br/><br/>
Boş alan gereksinimleri| 2 GB/root bölüme <br/><br/> Yükleme klasörü üzerinde 250 MB
XFSv5 | Meta veri sağlama gibi XFS dosya sistemleri XFSv5 özellikleri Mobility hizmeti sürümü 9.10 ileriye doğru desteklenir. Süper blok XFS kullanarak bölümün denetlemek için xfs_info yardımcı programını kullanın. Ftype 1 olarak ayarlanırsa, XFSv5 özellikleri kullanılıyor olabilir.



## <a name="network"></a>Ağ

**Bileşen** | **Destekleniyor**
--- | ---
Konak ağı NIC grubu oluşturma | VMware Vm'leri için desteklenir. <br/><br/>Fiziksel makine çoğaltma için desteklenmiyor.
Konak ağı VLAN | Evet.
Konak ağı IPv4 | Evet.
Konak ağı IPv6 | Hayır.
Konuk/server ağı NIC grubu oluşturma | Hayır.
Konuk/sunucu ağ IPv4 | Evet.
Konuk/sunucu ağ IPv6 | Hayır.
Konuk/sunucu ağ statik IP (Windows) | Evet.
Konuk/sunucu ağ statik IP (Linux) | Evet. <br/><br/>Sanal makineleri yeniden çalışma üzerinde DHCP kullanmak üzere yapılandırılır.
Konuk/sunucu ağ birden çok NIC | Evet.


## <a name="azure-vm-network-after-failover"></a>Azure VM ağı (sonra Yük devretme)

**Bileşen** | **Destekleniyor**
--- | ---
Azure ExpressRoute | Evet
ILB | Evet
ELB | Evet
Azure Traffic Manager | Evet
Multi-NIC | Evet
Ayrılmış IP adresi | Evet
IPv4 | Evet
Kaynak IP adresi koru | Evet
Azure sanal ağ hizmet uç noktaları<br/><br/> (Azure depolama güvenlik duvarları ve sanal ağlar) | Hayır

## <a name="storage"></a>Depolama
**Bileşen** | **Destekleniyor**
--- | ---
Ana bilgisayar NFS | VMware için Evet<br/><br/> Fiziksel sunucuları için Hayır'ı
Konak SAN (İSCSI) | Evet
Ana bilgisayar çok yollu (MPIO) | Evet, CLARiiON için Microsoft DSM, EMC PowerPath 5.7 SP4 EMC PowerPath DSM ile test
Konuk/sunucu VMDK | Evet
Konuk/sunucu EFI/UEFI'ye| Kısmi (Windows Server 2012 için Azure ve sonraki VMware sanal makineleri yalnızca geçiş) </br></br> Tablonun sonundaki nota bakın
Konuk/sunucu paylaşılan küme diski | Hayır
Konuk/sunucu şifreli disk | Hayır
Konuk/sunucu NFS | Hayır
Konuk/sunucu SMB 3.0 | Hayır
Konuk/sunucu RDM | Evet<br/><br/> Fiziksel sunucuları için yok
Konuk/sunucu diski > 1 TB | Evet<br/><br/>4.095 GB'a kadar
Konuk/sunucu diski 4 K mantıksal ve 4 k fiziksel kesim boyutu | Evet
4K mantıksal Konuk/sunucu diskle ve 512 bayt fiziksel kesim boyutu | Evet
Şeritli disk Konuk/sunucu birimle > 4 TB <br><br/>Mantıksal birim yönetimi (LVM)| Evet
Konuk/sunucu - depolama alanları | Hayır
Konuk/sunucu sık kullanılan Ekle/Kaldır disk | Hayır
Konuk/sunucu - exclude disk | Evet
Konuk/sunucu çok yollu (MPIO) | Yok

> [!NOTE]
> UEFI VMware sanal makineleri Windows Server 2012 çalıştıran önyükleme veya Azure'a daha sonra geçirilebilir. Aşağıdaki kısıtlamalar geçerlidir:

> - Yalnızca azure'a geçişini destekler. Şirket içi VMware sitesi için yeniden çalışma desteklenmez.
> - Sunucu işletim sistemi disk üzerinde dörtten fazla bölümü olmamalıdır.
> - Mobility hizmeti sürümü 9.13 veya üstünü gerektirir.
> - Fiziksel sunucuları için desteklenmez.

## <a name="azure-storage"></a>Azure Storage

**Bileşen** | **Destekleniyor**
--- | ---
Yerel olarak yedekli depolama | Evet
Coğrafi Olarak Yedekli Depolama | Evet
Coğrafi olarak yedekli depolamaya okuma erişimi | Evet
Seyrek erişimli depolama | Hayır
Sık erişimli depolama| Hayır
Blok blobları | Hayır
Bekleyen (depolama hizmeti şifrelemesi) şifreleme| Evet
Premium depolama | Evet
İçeri/dışarı aktarma hizmeti | Hayır
Sanal ağ hizmet uç noktaları<br/><br/> Depolama güvenlik duvarları ve hedef depolama/önbelleği depolama hesabı (çoğaltma verilerini depolamak için kullanılır) üzerinde yapılandırılmış sanal ağları | Hayır
Genel amaçlı v2 depolama hesapları (sık erişimli ve seyrek erişimli Katmanlar) | Hayır

## <a name="azure-compute"></a>Azure işlem

**Özellik** | **Destekleniyor**
--- | ---
Kullanılabilirlik kümeleri | Evet
HUB | Evet
Yönetilen diskler | Evet

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Şirket içi sanal makinelerini Azure'a çoğaltma bu tabloda özetlenen Azure VM gereksinimlerini karşılaması gerekir. Site Recovery Önkoşul denetimi çalıştığında, bazı gereksinimleri karşılanmadığı takdirde başarısız olur.

**Bileşen** | **Gereksinimleri** | **Ayrıntılar**
--- | --- | ---
Konuk işletim sistemi | Doğrulama [desteklenen işletim sistemleri](#replicated machines). | Onay desteklenmeyen başarısız olur. 
Konuk işletim sistemi mimarisi | 64-bit. | Onay desteklenmeyen başarısız olur. 
İşletim sistemi disk boyutu | 2.048 GB'a kadar. | Onay desteklenmeyen başarısız olur. 
İşletim sistemi disk sayısı | 1 | Onay desteklenmeyen başarısız olur.  
Veri diski sayısı | 64 veya daha az. | Onay desteklenmeyen başarısız olur.  
Veri diski boyutu | 4.095 GB'a kadar | Onay desteklenmeyen başarısız olur. 
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir. | 
Paylaşılan VHD | Desteklenmiyor. | Onay desteklenmeyen başarısız olur. 
FC disk | Desteklenmiyor. | Onay desteklenmeyen başarısız olur. 
BitLocker | Desteklenmiyor. | Bir makine için çoğaltma etkinleştirmeden önce BitLocker'ı devre dışı bırakılması gerekir. | 
VM adı | 1 63 karakter.<br/><br/> Harfler, sayılar ve kısa çizgilerden oluşabilir.<br/><br/> Makine adı başlamalı ve bir harf veya sayı ile bitmelidir. |  Site Recovery makine özelliklerinde değeri güncelleştirin.


## <a name="vault-tasks"></a>Kasa görevleri

**Eylem** | **Destekleniyor**
--- | ---
Kasa kaynak grupları arasında taşıma<br/><br/> İçinde ve abonelikler arasında | Hayır
Depolama, ağ, Azure Vm'leri kaynak grupları arasında taşıma<br/><br/> İçinde ve abonelikler arasında | Hayır


## <a name="mobility-service"></a>Mobility hizmeti

**Ad** | **Açıklama** | **En son sürümü** | **Ayrıntılar**
--- | --- | --- | --- | ---
Azure Site Recovery Kurulum birleşik | Şirket içi VMware sunucular ile Azure arasındaki iletişimi düzenler <br/><br/> Şirket içi VMware sunucularda yüklü | 9.12.4653.1 (portalından kullanılabilir) | [En son özellikleri ve düzeltmeleri](https://aka.ms/latest_asr_updates)
Mobility hizmeti | Şirket içi VMware sunucularını/fiziksel sunucular ile Azure/ikincil site arasında çoğaltma koordinatları<br/><br/> VMware VM veya fiziksel sunucuları çoğaltmak istediğiniz yüklü | 9.12.4653.1 (portalından kullanılabilir) | [En son özellikleri ve düzeltmeleri](https://aka.ms/latest_asr_updates)


## <a name="next-steps"></a>Sonraki adımlar
[Bilgi nasıl](tutorial-prepare-azure.md) Azure VMware Vm'lerini olağanüstü durum kurtarma için hazırlamak için.
