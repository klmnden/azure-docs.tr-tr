---
title: VMware Vm'lerini ve fiziksel sunucuları Azure Site Recovery ile azure'a olağanüstü durum kurtarma için destek matrisi | Microsoft Docs
description: VMware Vm'lerini ve fiziksel sunucudan azure'a Azure Site Recovery ile olağanüstü durum kurtarma için bileşenleri ve desteklenen işletim sistemleri özetlenmektedir.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: raynew
ms.openlocfilehash: dc455b5256f9c04e1e0af2c1ff3fea04af54d90b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60565420"
---
# <a name="support-matrix-for-disaster-recovery--of-vmware-vms-and-physical-servers-to-azure"></a>VMware Vm'lerini ve fiziksel sunucuları azure'a olağanüstü durum kurtarma için destek matrisi

Bu makalede kullanarak desteklenen bileşenler ve Azure'da VMware vm'lerinin olağanüstü durum kurtarma için ayarları özetler [Azure Site Recovery](site-recovery-overview.md).

En basit dağıtım senaryosu ile Azure Site Recovery kullanmaya başlamak için ziyaret bizim [öğreticiler](tutorial-prepare-azure.md). Azure Site Recovery mimarisi hakkında daha fazla bilgi [burada](vmware-azure-architecture.md).

## <a name="deployment-scenario"></a>Dağıtım senaryosu

**Senaryo** | **Ayrıntılar**
--- | ---
VMware vm'lerinin olağanüstü durum kurtarma | Şirket içi VMware Vm'lerini azure'a çoğaltma. Bu senaryo Azure portalında veya kullanarak dağıtabileceğiniz [PowerShell](vmware-azure-disaster-recovery-powershell.md).
Fiziksel sunucuları olağanüstü durum kurtarma | Şirket içi Windows/Linux fiziksel sunucuları azure'a çoğaltma. Bu senaryoda Azure Portalı'nda dağıtabilirsiniz.

## <a name="on-premises-virtualization-servers"></a>Şirket içi sanallaştırma sunucuları

**Sunucu** | **Gereksinimler** | **Ayrıntılar**
--- | --- | ---
VMware | vCenter Server 6.7 6.5, 6.0 veya 5.5 veya vSphere 6.7, 6.5, 6.0 veya 5.5 | Bir vCenter sunucusu kullanmanızı öneririz.<br/><br/> VSphere konaklarını ve vCenter sunucularını işlem sunucusu aynı ağda bulunan öneririz. İşlem sunucusu bileşenleri çalıştırır yapılandırma sunucusunda bir adanmış işlem sunucusu ayarlamadıysanız ayarlamadığınız sürece bu yapılandırma sunucusunu, ayarladığınız ağ olur.
Fiziksel | Yok

## <a name="site-recovery-configuration-server"></a>Site Recovery yapılandırma sunucusu

Yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucusu da dahil olmak üzere, Site Recovery bileşenlerini çalıştıran bir şirket içi makineyi yapılandırma sunucusudur. VMware çoğaltması için yapılandırma sunucusu ile tüm gereksinimleri, bir VMware VM oluşturmak için OVF şablonunu kullanarak ayarladığınız. Fiziksel sunucu çoğaltma için yapılandırma sunucusu makine el ile ayarlayabilir.

**Bileşen** | **Gereksinimler**
--- |---
CPU çekirdekleri | 8
RAM | 16 GB
Disk sayısı | 3 diskler<br/><br/> Diskler, işletim sistemi diski, işlem sunucusu önbellek diski ve yeniden çalışma için bekletme sürücüsü içerir.
Boş disk alanı | İşlem sunucusu önbelleği için gereken alanı 600 GB.
Boş disk alanı | Bekletme sürücüsü için gereken alanı 600 GB.
İşletim sistemi  | Windows Server 2012 R2 veya Windows Server 2016 |
İşletim sistemi yerel ayarı | İngilizce (en-us)
PowerCLI | [Powerclı 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "Powerclı 6.0") sürümlerinden ile yapılandırma sunucusu için gerekli değildir [9.14](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery).
Windows Server rolleri | Etkinleştirme: <br/> - Active Directory Domain Services <br/>- İnternet Bilgi Hizmetleri <br/> - Hyper-V |
Grup İlkeleri| Etkinleştirme: <br/> -Komut istemine erişimi engelleyin. <br/> -Kayıt defteri düzenleme araçlarına erişimi engelleyin. <br/> -Mantıksal dosya ekleri için güven. <br/> -Betik yürütmeyi açma. <br/> [Daha fazla bilgi](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
IIS | Emin olun:<br/><br/> -Önceden var olan bir varsayılan Web sitesi yok <br/> -Etkinleştir [anonim kimlik doğrulaması](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br/> -Etkinleştir [Fastcgı](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarı  <br/> -Önceden var olan Web sitesi/uygulama bağlantı noktası 443 üzerinde dinleme yok<br/>
NIC türü | VMXNET3 (VMware VM olarak dağıtıldığında)
IP adresi türü | Statik
Bağlantı Noktaları | denetim kanalı düzenleme için kullanılan 443)<br/>Veri taşıma için kullanılan 9443

## <a name="replicated-machines"></a>Çoğaltılan makineler

Site Recovery, desteklenen bir makinede çalışan tüm iş yüklerini çoğaltılmasını destekler.

**Bileşen** | **Ayrıntılar**
--- | ---
Makine ayarları | Azure'a çoğaltılan makineler karşılamalıdır [Azure gereksinimleri](#azure-vm-requirements).
Makine iş yükü | Site Recovery tüm iş yükleri (örneğin, Active Directory, SQL server vb.) çoğaltılmasını destekler desteklenen bir makinede çalışıyor. [Daha fazla bilgi edinin](https://aka.ms/asr_workload).
Windows işletim sistemi | 64 bit Windows Server 2016 (Sunucu Çekirdeği, masaüstü deneyimi ile sunucu), Windows Server 2012 R2, Windows Server 2012, Itanium tabanlı sistemler için Windows Server 2008 R2 ile en az SP1. </br></br>  [Windows Server 2008 ile en az SP2 - 32 bit ve 64 bit](migrate-tutorial-windows-server-2008.md) (yalnızca geçiş). </br></br> Windows 2016 Nano sunucu desteklenmiyor.
Linux işletim sistemi mimarisi | Yalnızca 64 bit sistem desteklenir. 32-bit sistem desteklenmiyor
Linux işletim sistemi | Red Hat Enterprise Linux: 5.2 için 5.11<b>\*\*</b>, 6.1 için 6.10<b>\*\*</b>, 7.0 için 7.6 <br/><br/>CentOS: 5.2 için 5.11<b>\*\*</b>, 6.1 için 6.10<b>\*\*</b>, 7.0 için 7.6 <br/><br/>Ubuntu 14.04 LTS server [(çekirdek sürümleri desteklenir)](#ubuntu-kernel-versions)<br/><br/>Ubuntu 16.04 LTS server [(çekirdek sürümleri desteklenir)](#ubuntu-kernel-versions)<br/><br/>Debian 7/Debian 8 [(çekirdek sürümleri desteklenir)](#debian-kernel-versions)<br/><br/>SUSE Linux Enterprise Server 12 SP1, SP2 SP3 [(çekirdek sürümleri desteklenir)](#suse-linux-enterprise-server-12-supported-kernel-versions)<br/><br/>SUSE Linux Enterprise Server 11 SP3<b>\*\*</b>, SUSE Linux Enterprise Server 11 SP4 * </br></br>Oracle Linux 6.4, 6.5, 6.6, 6.7, 6,8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, Red Hat uyumlu çekirdek veya kesilemeyen Enterprise çekirdeği sürüm 3 (UEK3) çalıştıran 7.5 <br/><br/></br>-Çoğaltılan makineler için SP4 SUSE Linux Enterprise Server 11 SP3 ' yükseltme desteklenmez. Yükseltmek için çoğaltmayı devre dışı bırakın ve yükseltmeden sonra yeniden etkinleştirin.</br></br> - [Daha fazla bilgi edinin](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure) Linux ve açık kaynak teknolojisi azure'da desteği hakkında. Site Recovery Azure'da Linux sunucuları çalıştırmak için yük devretme işlemlerini yönetir. Ancak Linux satıcılar yalnızca son yaşam geçmediği dağıtım sürümleri için destek sınırlayabilir.<br/><br/> -Linux dağıtımlarında dağıtım podverze yayın/güncelleştirmenin parçası olan stok çekirdekler desteklenir.<br/><br/> -Korumalı makineler arasında önemli Linux dağıtım sürümleri desteklenmez yükseltme. Yükseltmek için çoğaltmayı devre dışı bırak, işletim sistemini yükseltin ve ardından çoğaltmayı yeniden etkinleştirin.<br/><br/> -Red Hat Enterprise Linux 5.2-5.11 veya CentOS 5.2-5.11 çalıştıran sunucular olmalıdır [Linux Integration Services (LIS) bileşenleri](https://www.microsoft.com/download/details.aspx?id=55106) makineler Azure'da önyüklemesini yapmak için yüklü.


### <a name="ubuntu-kernel-versions"></a>Ubuntu çekirdek sürümleri


**Desteklenen sürüm** | **Azure Site Recovery Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
14.04 LTS | [9,24] [9,24 UR] | 3.13.0-24-Generic 3.13.0-167-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-143-generic için<br/>4.15.0-1023-Azure 4.15.0-1040-azure için |
14.04 LTS | [9.23][9.23 UR] | 3.13.0-24-Generic 3.13.0-165-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-142-generic için<br/>4.15.0-1023-Azure 4.15.0-1037-azure için |
14.04 LTS | [9.22][9.22 UR] | 3.13.0-24-Generic 3.13.0-164-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-140-generic için<br/>4.15.0-1023-Azure 4.15.0-1036-azure için |
14.04 LTS | [9.21][9.21 UR] | 3.13.0-24-Generic 3.13.0-163-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-140-generic için<br/>4.15.0-1023-Azure 4.15.0-1035-azure için |
|||
16.04 LTS | [9.23] [9,24 UR] | 4.4.0-21-Generic 4.4.0-143-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-45-generic için<br/>4.15.0-13-Generic 4.15.0-46-generic için<br/>4.11.0-1009-Azure 4.11.0-1018-azure için<br/>4.13.0-1005-Azure 4.13.0-1018-azure için <br/>4.15.0-1012-Azure 4.15.0-1040-azure için|
16.04 LTS | [9.23][9.23 UR] | 4.4.0-21-Generic 4.4.0-142-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-45-generic için<br/>4.15.0-13-Generic 4.15.0-45-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1018-azure için <br/>4.15.0-1012-Azure 4.15.0-1037-azure için|
16.04 LTS | [9.22][9.22 UR] | 4.4.0-21-Generic 4.4.0-140-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-45-generic için<br/>4.15.0-13-Generic 4.15.0-43-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1018-azure için <br/>4.15.0-1012-Azure 4.15.0-1036-azure için|
16.04 LTS | [9.21][9.21 UR] | 4.4.0-21-Generic 4.4.0-140-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-45-generic için<br/>4.15.0-13-Generic 4.15.0-42-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1018-azure için <br/>4.15.0-1012-Azure 4.15.0-1035-azure için|
16.04 LTS | [9.20][9.20 UR] | 4.4.0-21-Generic 4.4.0-138-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-45-generic için<br/>4.15.0-13-Generic 4.15.0-38-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1018-azure için <br/>4.15.0-1012-Azure 4.15.0-1025-azure için|

### <a name="debian-kernel-versions"></a>Debian çekirdek sürümleri


**Desteklenen sürüm** | **Azure Site Recovery Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
Debian 7 | [9.21][9.21 UR], [9.22][9.22 UR],[9.23][9.23 UR], [9,24] [9.24 UR]| 3.2.0-4-AMD64 3.2.0-6-amd64 için 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | [9.21][9.21 UR],[9.22][9.22 UR],[9.23][9.23 UR], [9,24] [9.24 UR] | 3.16.0-4-AMD64 3.16.0-7-amd64, 4.9.0-0.bpo.4-amd64 4.9.0-0.bpo.8-amd64 için için |


### <a name="suse-linux-enterprise-server-12-supported-kernel-versions"></a>SUSE Linux Enterprise Server 12 çekirdeği sürümlerinde desteklenir.

**Yayın** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
SUSE Linux Enterprise Server (SP1, SP2, SP3, SP4) 12 | [9,24] [9,24 UR] | SP1 3.12.49-11-default 3.12.74-60.64.40-default için</br></br> SP1(LTSS) 3.12.74-60.64.45-default 3.12.74-60.64.107-default için</br></br> SP2 4.4.21-69-default 4.4.120-92.70-default için</br></br>SP2(LTSS) 4.4.121-92.73-default 4.4.121-92.101-default için</br></br>SP3 4.4.73-5-default 4.4.175-94.79-default için</br></br>SP4 4.12.14-94.41-default 4.12.14-95.6-default için |
SUSE Linux Enterprise Server (SP1, SP2, SP3, SP4) 12 | [9.23][9.23 UR] | SP1 3.12.49-11-default 3.12.74-60.64.40-default için</br></br> SP1(LTSS) 3.12.74-60.64.45-default 3.12.74-60.64.107-default için</br></br> SP2 4.4.21-69-default 4.4.120-92.70-default için</br></br>SP2(LTSS) 4.4.121-92.73-default 4.4.121-92.101-default için</br></br>SP3 4.4.73-5-default 4.4.162-94.69-default için</br></br>SP4 4.12.14-94.41-default 4.12.14-95.6-default için |
SUSE Linux Enterprise Server (SP1, SP2 SP3) 12 | [9.22][9.22 UR] | SP1 3.12.49-11-default 3.12.74-60.64.40-default için</br></br> SP1(LTSS) 3.12.74-60.64.45-default 3.12.74-60.64.107-default için</br></br> SP2 4.4.21-69-default 4.4.120-92.70-default için</br></br>SP2(LTSS) 4.4.121-92.73-default 4.4.121-92.98-default için</br></br>SP3 4.4.73-5-default 4.4.162-94.72-default için |
SUSE Linux Enterprise Server (SP1, SP2 SP3) 12 | [9.21][9.21 UR] | SP1 3.12.49-11-default 3.12.74-60.64.40-default için</br></br> SP1(LTSS) 3.12.74-60.64.45-default 3.12.74-60.64.107-default için</br></br> SP2 4.4.21-69-default 4.4.120-92.70-default için</br></br>SP2(LTSS) 4.4.121-92.73-default 4.4.121-92.98-default için</br></br>SP3 4.4.73-5-default 4.4.156-94.72-default için |


## <a name="linux-file-systemsguest-storage"></a>Linux dosya sistemleri/Konuk depolama

**Bileşen** | **Destekleniyor**
--- | ---
Dosya sistemleri | ext3, ext4, XFS
Birim Yöneticisi | Önce [9.20 sürüm](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), <br/> 1. LVM'yi desteklenir. <br/> 2. makinesiyse LVM birimde desteklenmez. <br/> 3. Birden çok işletim sistemi diskleri desteklenmez.<br/><br/>Gelen [9.20 sürüm](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery) makinesiyse LVM üzerinde ve sonraki sürümlerde desteklenir. Birden çok işletim sistemi diskleri desteklenmez.
Parasanallaştırılmış depolama cihazları | Parasanallaştırılmış sürücüler tarafından dışarı aktarılan cihazlar desteklenmez.
Birden fazla kuyruk blok g/ç cihazları | Desteklenmiyor.
HP CCISS depolama denetleyicisi ile fiziksel sunucuları | Desteklenmiyor.
Cihaz/bağlama noktası adlandırma kuralı | Cihaz adı veya bağlama noktası adı benzersiz olmalıdır. Hiçbir iki cihazları/bağlama noktaları büyük/küçük harfe duyarlı adlara sahip olduğundan emin olun. </br> Örnek: Aynı sanal makineye iki cihazını adlandırma *cihaz1* ve *cihaz1* izin verilmiyor.
Dizinler | Önce [9.20 sürüm](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), <br/> 1. Aşağıdaki dizinleri (varsa ayrı bölümler/dosya-sistemleri ayarlanmış) tüm kaynak sunucuyla aynı işletim sistemi diskinde olmalıdır: / (root), makinesiyse, / usr, / usr/local, /var, / etc.</br>2. makinesiyse bir disk bölümünde olmalı ve LVM birim olmaması gerekir.<br/><br/> Gelen [9.20 sürüm](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery) ve sonraki sürümlerde, kısıtlama geçerli değildir. birden fazla disklerde LVM birimdeki makinesiyse desteklenmiyor.
Önyükleme dizini | Bir sanal makinede birden fazla önyükleme diski desteklenmiyor <br/><br/> Önyükleme diski olmadan bir makine korunamaz.

Boş alanı gereksinimleri | 2 GB/root bölümdeki <br/><br/> Yükleme klasöründeki XFSv5 250 MB | Mobilite hizmeti sürümünden 9.10 ileriye doğru XFS dosya sistemleri gibi meta veri sağlama XFSv5 özellikleri desteklenir. Süper blok XFS kullanarak bölümü için denetlenecek xfs_info yardımcı programını kullanın. Varsa `ftype` XFSv5 özellikleri kullanımda olan 1 olarak ayarlanmışsa.

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
Azure sanal ağ hizmet uç noktaları<br/> | Evet
Hızlandırılmış Ağ | Hayır

## <a name="storage"></a>Depolama
**Bileşen** | **Destekleniyor**
--- | ---
Dinamik disk | İşlemi sistem diski temel disk olması gerekir. <br/><br/>Veri diskleri dinamik diskleri olabilir.
Docker disk yapılandırması | Hayır
Konak NFS | VMware için Evet<br/><br/> Fiziksel sunucular için Hayır
Konak SAN (iSCSI/FC) | Evet
Konak vsan'ı | VMware için Evet<br/><br/> Fiziksel sunucular için yok
Konak çok yollu (MPIO) | Evet, Microsoft DSM EMC PowerPath 5.7 SP4, EMC PowerPath DSM ile CLARiiON için test
Konak sanal birimler (VVols) | VMware için Evet<br/><br/> Fiziksel sunucular için yok
Konuk/sunucu VMDK | Evet
Konuk/sunucu paylaşılan küme diskine | Hayır
Konuk/sunucu şifreli disk | Hayır
Konuk/sunucu NFS | Hayır
Konuk/sunucu SMB 3.0 | Hayır
Konuk/sunucu RDM | Evet<br/><br/> Fiziksel sunucular için yok
Konuk/sunucu disk > 1 TB | Evet<br/><br/>4.095 GB'a kadar<br/><br/> Disk 1024 MB değerinden daha büyük olmalıdır.
Konuk/sunucu disk ile 4 K mantıksal ve 4 k fiziksel kesim boyutu | Evet
Konuk/sunucu disk ile 4K mantıksal ve fiziksel kesim boyutu 512 bayt | Evet
Bölüştürülmüş bir disk ile Konuk/sunucusu birimi > 4 TB <br/><br/>Mantıksal birim yönetimi (LVM)| Evet
Konuk/sunucu - depolama alanları | Hayır
Konuk/sunucu sık erişimli Ekle/Kaldır disk | Hayır
Konuk/sunucu - disk dışlama | Evet
Konuk/sunucu çok yollu (MPIO) | Hayır
Konuk/server EFI/UEFI'ye önyükleme | VMware Vm'lerini veya fiziksel sunucuları Windows Server 2012 çalıştıran geçiş yaparken ya da daha sonra Azure'a desteklenir.<br/><br/> Yalnızca, geçiş için Vm'lerini çoğaltabilirsiniz. Şirket içine yeniden çalışma desteklenmez.<br/><br/> Sunucu işletim sistemi diskinde dörtten fazla bölümler olmamalıdır.<br/><br/> Mobility hizmeti sürümü 9.13 veya üstü gerektirir.<br/><br/> Yalnızca NTFS desteklenmiyor.

## <a name="replication-channels"></a>Çoğaltma kanallar

|**Çoğaltma türü**   |**Destekleniyor**  |
|---------|---------|
|Boşaltılan veri aktarımları (ODX)    |       Hayır  |
|Çevrimdışı Dengeli Dağıtım        |   Hayır      |
| Azure Data Box | Hayır


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
Hedef depolama/önbellek depolama hesabı (çoğaltma verilerini depolamak için kullanılan) üzerinde yapılandırılan sanal ağlar için Azure depolama güvenlik duvarları | Evet
Genel amaçlı v2 depolama hesaplarının (sık erişimli ve seyrek erişimli Katmanlar) | Hayır

## <a name="azure-compute"></a>Azure işlem

**Özellik** | **Destekleniyor**
--- | ---
Kullanılabilirlik kümeleri | Evet
Kullanılabilirlik alanları | Hayır
HUB | Evet
Yönetilen diskler | Evet

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure'a Çoğalttığınız şirket içi Vm'leri bu tabloda özetlenen Azure VM gereksinimleri karşılaması gerekir. Site Recovery, bir önkoşul denetimi çalıştığında, bazı gereksinimleri karşılanmadığı takdirde başarısız olur.

**Bileşen** | **Gereksinimler** | **Ayrıntılar**
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

## <a name="azure-site-recovery-churn-limits"></a>Azure Site Recovery karmaşıklığı sınırları

Aşağıdaki tablo, Azure Site Recovery sınırlarını sağlar. Bu limitler yaptığımız testleri temel alsa da mümkün olan tüm uygulama G/Ç birleşimlerini kapsamamaktadır. Gerçek sonuçlar, uygulamanızın G/Ç karışımına göre değişebilir. En iyi sonuçlar için kesinlikle öneririz [dağıtım Planlayıcısı aracını çalıştırma](site-recovery-deployment-planner.md) ve uygulamanın gerçek performans görüntüsünü elde etmek üzere kapsamlı uygulama testleri bir test yük devretmesi göndererek gerçekleştirin.

**Çoğaltma depolama hedefi** | **Ortalama kaynak disk G/Ç boyutu** |**Ortalama kaynak disk veri değişim sıklığı** | **Günlük toplam kaynak disk veri değişim sıklığı**
---|---|---|---
Standart depolama | 8 KB | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 8 KB  | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 16 KB | 4 MB/sn |  Disk başına 336 GB
Premium P10 veya P15 disk | 32 KB veya daha büyük | 8 MB/sn | Disk başına 672 GB
Premium P20 veya P30 veya P40 veya P50 disk | 8 KB    | 5 MB/sn | Disk başına 421 GB
Premium P20 veya P30 veya P40 veya P50 disk | 16 KB veya daha büyük |20 MB/sn | Disk başına 1684 GB

**Kaynak veri değişim sıklığı** | **Üst Sınır**
---|---
VM başına ortalama veri değişim sıklığı| 25 MB/sn
VM üzerindeki tüm disklerde en yüksek veri değişim sıklığı | 54 MB/sn
İşlem Sunucusu tarafından desteklenen günlük en fazla veri değişim sıklığı | 2 TB

Bunlar yüzde 30 G/Ç çakışmasını varsayan ortalama sayılardır. Site Recovery; çakışma oranı, büyük yazma boyutları ve gerçek iş yükü G/Ç davranışına göre daha yüksek aktarım hızını işleyebilir. Yukarıdaki sayılar yaklaşık beş dakikalık tipik bir kapsamı varsayar. Diğer bir deyişle, veriler karşıya yüklendikten sonra işlenir ve beş dakika içinde kurtarma noktası oluşturulur.

## <a name="vault-tasks"></a>Kasa görevleri

**Eylem** | **Destekleniyor**
--- | ---
Kasa kaynak grupları arasında taşıma<br/><br/> İçinde ve arasında abonelikler | Hayır
Depolama, ağ, Azure Vm'leri kaynak grupları arasında taşıma<br/><br/> İçinde ve arasında abonelikler | Hayır


## <a name="download-latest-azure-site-recovery-components"></a>En son Azure Site Recovery bileşenlerini yükle

**Ad** | **Açıklama** | **En son sürümü indirme yönergeleri**
--- | --- | ---
Yapılandırma sunucusu | Şirket içi VMware sunucularını ve Azure arasındaki iletişimleri koordine eder <br/><br/> Şirket içi VMware sunucularında yüklü | Daha fazla bilgi için ziyaret edip kılavuzumuzu [yüklemesidir](vmware-azure-deploy-configuration-server.md) ve [mevcut bileşenin en son sürüme yükseltme](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server).
İşlem sunucusu|Varsayılan olarak yapılandırma sunucusuna yüklenir. Bu çoğaltma verilerini alıp; Bu, önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir; ve Azure depolamaya gönderir. Dağıtımınız büyüdükçe, daha büyük çoğaltma trafiği hacimlerini idare etmek ayrı, ek işlem sunucuları ekleyebilirsiniz.| Daha fazla bilgi için ziyaret edip kılavuzumuzu [yüklemesidir](vmware-azure-set-up-process-server-scale.md) ve [mevcut bileşenin en son sürüme yükseltme](vmware-azure-manage-process-server.md#upgrade-a-process-server).
Mobility hizmeti | Şirket içi VMware sunucuları/fiziksel sunucular ile Azure/ikincil site arasında çoğaltma koordinatları<br/><br/> VMware VM veya fiziksel sunucuları çoğaltmak istediğiniz yüklü | Daha fazla bilgi için ziyaret edip kılavuzumuzu [yüklemesidir](vmware-azure-install-mobility-service.md) ve [mevcut bileşenin en son sürüme yükseltme](vmware-physical-manage-mobility-service.md#update-mobility-service-from-azure-portal).

En son özellikler hakkında daha fazla bilgi edinmek için [en son sürüm notları](https://aka.ms/ASR_latest_release_notes).


## <a name="next-steps"></a>Sonraki adımlar
[Bilgi nasıl](tutorial-prepare-azure.md) Azure VMware vm'lerinin olağanüstü durum kurtarmasına hazırlanmak için.

[9.23 UR]: https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery
[9.22 UR]: https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery
[9.21 UR]: https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery
[9.20 UR]: https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery
[9.19 UR]: https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30
[9.18 UR]: https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery
