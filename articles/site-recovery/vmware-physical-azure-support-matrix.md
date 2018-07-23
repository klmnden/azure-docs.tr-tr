---
title: VMware Vm'leri ve fiziksel sunucuları Azure Site Recovery ile azure'a çoğaltmak için destek matrisi | Microsoft Docs
description: Azure Site Recovery kullanılarak Azure'da VMware Vm'lerini ve fiziksel sunucu çoğaltma için bileşenleri ve desteklenen işletim sistemleri özetlenmektedir.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/19/2018
ms.author: raynew
ms.openlocfilehash: 516cb69042e923a46168c7655dc3e3010d9557e6
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173801"
---
# <a name="support-matrix-for-vmware-and-physical-server-replication-to-azure"></a>VMware ve fiziksel sunucu çoğaltması azure'a destek matrisi

Bu makalede kullanarak desteklenen bileşenler ve Azure'da VMware vm'lerinin olağanüstü durum kurtarma için ayarları özetler [Azure Site Recovery](site-recovery-overview.md).

## <a name="replication-scenario"></a>Çoğaltma senaryosu

**Senaryo** | **Ayrıntılar**
--- | ---
VMware Sanal Makineleri | Şirket içi VMware Vm'lerini azure'a çoğaltma. Bu senaryo Azure portalında veya kullanarak dağıtabileceğiniz [PowerShell](vmware-azure-disaster-recovery-powershell.md).
Fiziksel sunucular | Şirket içi Windows/Linux fiziksel sunucuları azure'a çoğaltma. Bu senaryoda Azure Portalı'nda dağıtabilirsiniz.

## <a name="on-premises-virtualization-servers"></a>Şirket içi sanallaştırma sunucuları

**Sunucu** | **Gereksinimleri** | **Ayrıntılar**
--- | --- | ---
VMware | vCenter Server 6.7 6.5, 6.0 veya 5.5 veya vSphere 6.7, 6.5, 6.0 veya 5.5 | Bir vCenter sunucusu kullanmanızı öneririz.<br/><br/> VSphere konaklarını ve vCenter sunucularını işlem sunucusu aynı ağda bulunan öneririz. İşlem sunucusu bileşenleri çalıştırır yapılandırma sunucusunda bir adanmış işlem sunucusu ayarlamadıysanız ayarlamadığınız sürece bu yapılandırma sunucusunu, ayarladığınız ağ olur.
Fiziksel | Yok

## <a name="site-recovery-configuration-server"></a>Site Recovery yapılandırma sunucusu

Yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucusu da dahil olmak üzere, Site Recovery bileşenlerini çalıştıran bir şirket içi makineyi yapılandırma sunucusudur. VMware çoğaltması için yapılandırma sunucusu ile tüm gereksinimleri, bir VMware VM oluşturmak için OVF şablonunu kullanarak ayarladığınız. Fiziksel sunucu çoğaltma için yapılandırma sunucusu makine el ile ayarlayabilir.

**Bileşen** | **Gereksinimleri**
--- |---
CPU çekirdekleri | 8
RAM | 16 GB
Disk sayısı | 3 diskler<br/><br/> Diskler, işletim sistemi diski, işlem sunucusu önbellek diski ve yeniden çalışma için bekletme sürücüsü içerir.
Boş disk alanı | İşlem sunucusu önbelleği için gereken alanı 600 GB.
Boş disk alanı | Bekletme sürücüsü için gereken alanı 600 GB.
İşletim sistemi  | Windows Server 2012 R2 veya Windows Server 2016 |
İşletim sistemi yerel ayarı | İngilizce (en-us)
Powerclı | [Powerclı 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "Powerclı 6.0") yüklü olması gerekir.
Windows Server rolleri | Etkinleştirme: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V |
Grup İlkeleri| Etkinleştirme: <br> -Komut istemine erişimi engelleyin. <br> -Kayıt defteri düzenleme araçlarına erişimi engelleyin. <br> -Mantıksal dosya ekleri için güven. <br> -Betik yürütmeyi açma. <br> [Daha fazla bilgi](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
IIS | Emin olun:<br/><br/> -Önceden var olan bir varsayılan Web sitesi yok <br> -Etkinleştir [anonim kimlik doğrulaması](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Etkinleştir [Fastcgı](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarı  <br> -Önceden var olan Web sitesi/uygulama bağlantı noktası 443 üzerinde dinleme yok<br>
NIC türü | VMXNET3 (VMware VM olarak dağıtıldığında)
IP adresi türü | Statik
Bağlantı Noktaları | denetim kanalı düzenleme için kullanılan 443)<br>Veri taşıma için kullanılan 9443

## <a name="replicated-machines"></a>Çoğaltılan makineler

Site Recovery, desteklenen bir makinede çalışan tüm iş yüklerini çoğaltılmasını destekler.

**Bileşen** | **Ayrıntılar**
--- | ---
Makine ayarları | Azure'a çoğaltılan makineler karşılamalıdır [Azure gereksinimleri](#azure-vm-requirements).
Windows işletim sistemi | 64 bit Windows Server 2016 (Sunucu Çekirdeği, masaüstü deneyimi ile sunucu), Windows Server 2012 R2, Windows Server 2012, Itanium tabanlı sistemler için Windows Server 2008 R2 ile en az SP1. </br></br>  [Windows Server 2008 ile en az SP2 - 32 bit ve 64 bit](migrate-tutorial-windows-server-2008.md) (yalnızca geçiş). </br></br> Windows 2016 Nano sunucu desteklenmiyor.
Linux işletim sistemi | Red Hat Enterprise Linux: 5.2 için 5.11 ya, 6.1 için 6.9, 7.0 için 7.5 <br/><br/>CentOS: 5.2 için 5.11 ya, 6.1 için 6.9, 7.0 için 7.5 <br/><br/>Ubuntu 14.04 LTS server[ (çekirdek sürümleri desteklenir)](#ubuntu-kernel-versions)<br/><br/>Ubuntu 16.04 LTS server[ (çekirdek sürümleri desteklenir)](#ubuntu-kernel-versions)<br/><br/>Debian 7/Debian 8[ (çekirdek sürümleri desteklenir)](#debian-kernel-versions)<br/><br/>SUSE Linux Enterprise Server 12 SP1, SP2 SP3 [ (çekirdek sürümleri desteklenir)](#suse-linux-enterprise-server-12-supported-kernel-versions)<br/><br/>SUSE Linux Enterprise Server 11 SP3, SUSE Linux Enterprise Server 11 SP4 * </br></br>Oracle Enterprise Linux 6.4, 6.5 Red Hat uyumlu çekirdek veya kesilemeyen Enterprise çekirdeği sürüm 3 (UEK3) yüklü <br/><br/></br>* *Çoğaltılan makineler SP4 için SUSE Linux Enterprise Server 11 SP3 ' yükseltme desteklenmez. Yükseltmek için çoğaltmayı devre dışı bırakın ve yükseltmeden sonra yeniden etkinleştirin.*


>[!NOTE]
>
> - Linux dağıtımlarında, dağıtım podverze yayın/güncelleştirmenin parçası olan stok çekirdekler desteklenir.
>
> - Korumalı makineler arasında önemli Linux dağıtım sürümleri desteklenmez yükseltiliyor. Yükseltmek için çoğaltmayı devre dışı bırak, işletim sistemini yükseltin ve ardından çoğaltmayı yeniden etkinleştirin.
>
> - Red Hat Enterprise Linux 5.2 5.11 veya CentOS 5.2 için 5.11 çalıştıran sunucular, Linux Tümleştirme Services(LIS) bileşenlerini makineleri Azure'da önyüklemesini yapmak için sırayla yüklemiş olmanız gerekir.

### <a name="ubuntu-kernel-versions"></a>Ubuntu çekirdek sürümleri


**Desteklenen sürüm** | **Azure Site Recovery Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
14.04 LTS | 9.18 | 3.13.0-24-Generic 3.13.0-153-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-130-generic için |
14.04 LTS | 9.17 | 3.13.0-24-Generic 3.13.0-149-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-127-generic için |
14.04 LTS | 9.16 | 3.13.0-24-Generic 3.13.0-144-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-119-generic için |
14.04 LTS | 9.15 | 3.13.0-24-Generic 3.13.0-144-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-119-generic için |
|||
16.04 LTS | 9.18 | 4.4.0-21-Generic 4.4.0-130-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-45-generic için |
16.04 LTS | 9.17 | 4.4.0-21-Generic 4.4.0-127-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-43-generic için |
16.04 LTS | 9.16 | 4.4.0-21-Generic 4.4.0-119-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-38-generic için |
16.04 LTS | 9.15 | 4.4.0-21-Generic 4.4.0-119-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-38-generic için |


### <a name="debian-kernel-versions"></a>Debian çekirdek sürümleri


**Desteklenen sürüm** | **Azure Site Recovery Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
Debian 7 | 9.17, 9.18 | 3.2.0-4-AMD64 3.2.0-6-amd64 için 3.16.0-0.bpo.4-amd64 |
Debian 7 | 9.15, 9.16 | 3.2.0-4-AMD64 3.2.0-5-amd64 için 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | 9.17, 9.18 | 3.16.0-4-AMD64 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 4.9.0-0.bpo.6-amd64 için için |
Debian 8 | 9.16 | 3.16.0-4-AMD64 3.16.0-5-amd64, 4.9.0-0.bpo.4-amd64 4.9.0-0.bpo.6-amd64 için için |
Debian 8 | 9.15 | 3.16.0-4-AMD64 3.16.0-5-amd64, 4.9.0-0.bpo.4-amd64 4.9.0-0.bpo.5-amd64 için için |

### <a name="suse-linux-enterprise-server-12-supported-kernel-versions"></a>SUSE Linux Enterprise Server 12 çekirdeği sürümlerinde desteklenir.

**Yayın** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
SUSE Linux Enterprise Server (SP1, SP2 SP3) 12 | 9.18 | SP1 3.12.49-11-default 3.12.74-60.64.40-default için</br></br> SP1(LTSS) 3.12.74-60.64.45-default 3.12.74-60.64.96-default için</br></br> SP2 4.4.21-69-default 4.4.120-92.70-default için</br></br>SP2(LTSS) 4.4.121-92.73-default 4.4.121-92.85-default için</br></br>SP3 4.4.73-5-default 4.4.138-94.39-default için |

## <a name="linux-file-systemsguest-storage"></a>Linux dosya sistemleri/Konuk depolama

**Bileşen** | **Destekleniyor**
--- | ---
Dosya sistemleri | ext3, ext4, XFS.
Birim Yöneticisi | LVM2.
Parasanallaştırılmış depolama cihazları | Parasanallaştırılmış sürücüler tarafından dışarı aktarılan cihazlar desteklenmez.
Birden fazla kuyruk blok g/ç cihazları | Desteklenmiyor.
HP CCISS depolama denetleyicisi ile fiziksel sunucuları | Desteklenmiyor.
Dizinler | Bu dizinleri (varsa ayrı bölümler/dosya-sistemleri ayarlanmış) tüm kaynak sunucuyla aynı işletim sistemi diskinde olmalıdır: / (root), makinesiyse, / usr, / usr/local, /var, / etc.</br></br> makinesiyse bir disk bölümünde olmalı ve LVM birim olmaması gerekir.<br/><br/>
Boş alan gereksinimleri| 2 GB/root bölümdeki <br/><br/> Yükleme klasöründeki 250 MB
XFSv5 | Mobilite hizmeti sürümünden 9.10 ileriye doğru XFS dosya sistemleri gibi meta veri sağlama XFSv5 özellikleri desteklenir. Süper blok XFS kullanarak bölümü için denetlenecek xfs_info yardımcı programını kullanın. Ftype 1 olarak ayarlarsanız, XFSv5 özellikleri kullanılıyor olabilir.

## <a name="vmdisk-management"></a>VM/Disk Yönetimi

**Eylem** | **Ayrıntılar**
--- | ---
Çoğaltılmış sanal diski yeniden boyutlandırma | Destekleniyor.
Çoğaltılmış VM'ye disk ekleme | Sanal makine için çoğaltmayı devre dışı bırakmak, disk ekleyin ve ardından çoğaltmayı yeniden etkinleştirin. Çoğaltma bir VM'ye disk ekleme şu anda desteklenmemektedir.

## <a name="network"></a>Ağ

**Bileşen** | **Destekleniyor**
--- | ---
Konak ağı NIC grubu oluşturma | VMware Vm'leri için desteklenmiyor. <br/><br/>Fiziksel makine için çoğaltma desteklenmiyor.
Konak ağ VLAN | Evet.
Konak ağı IPv4 | Evet.
Konak ağ IPv6 | Hayır.
Konuk/server ağı NIC grubu oluşturma | Hayır.
Konuk/sunucu ağ IPv4 | Evet.
Konuk/sunucu ağ IPv6 | Hayır.
Konuk/sunucu ağ statik IP (Windows) | Evet.
Konuk/sunucu ağ statik IP (Linux) | Evet. <br/><br/>VM'ler yeniden çalışma üzerinde DHCP kullanmak üzere yapılandırılır.
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
Kaynak IP adresini koruma | Evet
Azure sanal ağ hizmet uç noktaları<br/> (Azure depolama güvenlik duvarlarını) | Evet
Hızlandırılmış Ağ | Hayır

## <a name="storage"></a>Depolama
**Bileşen** | **Destekleniyor**
--- | ---
Konak NFS | VMware için Evet<br/><br/> Fiziksel sunucular için Hayır
Konak SAN (iSCSI/FC) | Evet
Konak vsan'ı | VMware için Evet<br/><br/> Fiziksel sunucular için yok
Konak çok yollu (MPIO) | Evet, Microsoft DSM EMC PowerPath 5.7 SP4, EMC PowerPath DSM ile CLARiiON için test
Konak sanal birimler (VVols) | VMware için Evet<br/><br/> Fiziksel sunucular için yok
Konuk/sunucu VMDK | Evet
Konuk/server EFI/UEFI'ye| Kısmi (Windows Server 2012 için Azure ve sonraki VMware sanal makinelerini yalnızca geçiş) </br></br> Tablonun sonundaki nota bakın
Konuk/sunucu paylaşılan küme diskine | Hayır
Konuk/sunucu şifreli disk | Hayır
Konuk/sunucu NFS | Hayır
Konuk/sunucu SMB 3.0 | Hayır
Konuk/sunucu RDM | Evet<br/><br/> Fiziksel sunucular için yok
Konuk/sunucu disk > 1 TB | Evet<br/><br/>4.095 GB'a kadar
Konuk/sunucu disk ile 4 K mantıksal ve 4 k fiziksel kesim boyutu | Evet
Konuk/sunucu disk ile 4K mantıksal ve fiziksel kesim boyutu 512 bayt | Evet
Bölüştürülmüş bir disk ile Konuk/sunucusu birimi > 4 TB <br><br/>Mantıksal birim yönetimi (LVM)| Evet
Konuk/sunucu - depolama alanları | Hayır
Konuk/sunucu sık erişimli Ekle/Kaldır disk | Hayır
Konuk/sunucu - disk dışlama | Evet
Konuk/sunucu çok yollu (MPIO) | Hayır

> [!NOTE]
> UEFI önyükleme Windows Server 2012 çalıştıran VMware sanal makinelerini veya daha sonra Azure'a geçirilebilir. Aşağıdaki kısıtlamalar uygulanır:

> - Azure'a geçiş desteklenir. Şirket içi VMware sitesinde yeniden çalışma desteklenmez.
> - Sunucu işletim sistemi diskinde dörtten fazla bölümler olmamalıdır.
> - Mobility hizmeti sürümü 9.13 veya üstü gerektirir.
> - Fiziksel sunucuları için desteklenmez.

## <a name="azure-storage"></a>Azure Storage

**Bileşen** | **Destekleniyor**
--- | ---
Yerel olarak yedekli depolama | Evet
Coğrafi olarak yedekli depolama | Evet
Okuma erişimli coğrafi olarak yedekli depolama | Evet
Seyrek erişimli depolama | Hayır
Sık erişimli depolama| Hayır
Blok blobları | Hayır
(Depolama hizmeti şifrelemesi) bekleyen şifreleme| Evet
Premium depolama | Evet
İçeri/dışarı aktarma hizmeti | Hayır
Hedef depolama/önbellek depolama hesabı (çoğaltma verilerini depolamak için kullanılan) üzerinde yapılandırılan sanal ağlar için Azure depolama güvenlik duvarları | Hayır
Genel amaçlı v2 depolama hesaplarının (sık erişimli ve seyrek erişimli Katmanlar) | Hayır

## <a name="azure-compute"></a>Azure işlem

**Özellik** | **Destekleniyor**
--- | ---
Kullanılabilirlik kümeleri | Evet
HUB | Evet
Yönetilen diskler | Evet

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure'a Çoğalttığınız şirket içi Vm'leri bu tabloda özetlenen Azure VM gereksinimleri karşılaması gerekir. Site Recovery, bir önkoşul denetimi çalıştığında, bazı gereksinimleri karşılanmadığı takdirde başarısız olur.

**Bileşen** | **Gereksinimleri** | **Ayrıntılar**
--- | --- | ---
Konuk işletim sistemi | Doğrulama [desteklenen işletim sistemleri](#replicated-machines) çoğaltılan makineler için. | Onay desteklenmeyen başarısız olur.
Konuk işletim sistemi mimarisi | 64-bit. | Onay desteklenmeyen başarısız olur.
İşletim sistemi disk boyutu | 2.048 GB. | Onay desteklenmeyen başarısız olur.
İşletim sistemi disk sayısı | 1 | Onay desteklenmeyen başarısız olur.  
Veri diski sayısı | 64 veya daha az. | Onay desteklenmeyen başarısız olur.  
Veri diski boyutu | 4.095 GB'a kadar | Onay desteklenmeyen başarısız olur.
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir. |
Paylaşılan VHD | Desteklenmiyor. | Onay desteklenmeyen başarısız olur.
FC diski | Desteklenmiyor. | Onay desteklenmeyen başarısız olur.
BitLocker | Desteklenmiyor. | Bir makine için çoğaltmayı etkinleştirmeden önce BitLocker'ı devre dışı bırakılması gerekir. |
VM adı | 1 63 karakter.<br/><br/> Harfler, sayılar ve kısa çizgilerden oluşabilir.<br/><br/> Makine adı başlamalı ve bir harf veya sayı ile bitmelidir. |  Site recovery'de makine özellikleri değerini güncelleştirin.


## <a name="vault-tasks"></a>Kasa görevleri

**Eylem** | **Destekleniyor**
--- | ---
Kasa kaynak grupları arasında taşıma<br/><br/> İçinde ve arasında abonelikler | Hayır
Depolama, ağ, Azure Vm'leri kaynak grupları arasında taşıma<br/><br/> İçinde ve arasında abonelikler | Hayır


## <a name="mobility-service"></a>Mobility hizmeti

**Ad** | **Açıklama** | **En son sürümü** | **Ayrıntılar**
--- | --- | --- | --- | ---
Azure Site Recovery birleşik Kurulumu | Şirket içi VMware sunucularını ve Azure arasındaki iletişimleri koordine eder <br/><br/> Şirket içi VMware sunucularında yüklü | 9.12.4653.1 (portalında kullanılabilir) | [En son özellikler ve düzeltmeler](https://aka.ms/latest_asr_updates)
Mobility hizmeti | Şirket içi VMware sunucuları/fiziksel sunucular ile Azure/ikincil site arasında çoğaltma koordinatları<br/><br/> VMware VM veya fiziksel sunucuları çoğaltmak istediğiniz yüklü | 9.12.4653.1 (portalında kullanılabilir) | [En son özellikler ve düzeltmeler](https://aka.ms/latest_asr_updates)


## <a name="next-steps"></a>Sonraki adımlar
[Bilgi nasıl](tutorial-prepare-azure.md) Azure VMware vm'lerinin olağanüstü durum kurtarmasına hazırlanmak için.
