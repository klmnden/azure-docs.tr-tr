---
title: VMware Vm'lerini ve fiziksel sunucuları Azure Site Recovery ile azure'a olağanüstü durum kurtarma için destek matrisi | Microsoft Docs
description: VMware Vm'lerini ve fiziksel sunucudan azure'a Azure Site Recovery ile olağanüstü durum kurtarma desteği özetler.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: raynew
ms.openlocfilehash: 5dc98048099264942552862498b5137b4954c200
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491639"
---
# <a name="support-matrix-for-disaster-recovery--of-vmware-vms-and-physical-servers-to-azure"></a>VMware Vm'lerini ve fiziksel sunucuları azure'a olağanüstü durum kurtarma için destek matrisi

Bu makalede desteklenen bileşenlerin ve VMware vm'lerinin olağanüstü durum kurtarma için ayarları ve fiziksel sunucuları aracılığıyla Azure'a özetlenmektedir [Azure Site Recovery](site-recovery-overview.md).

- [Daha fazla bilgi edinin](vmware-azure-architecture.md) VMware VM'LERİNİ/fiziksel sunucu olağanüstü durum kurtarma mimarisi hakkında.
- İzleyin bizim [öğreticiler](tutorial-prepare-azure.md) olağanüstü durum kurtarma denemek için.

## <a name="deployment-scenarios"></a>Dağıtım senaryoları

**Senaryo** | **Ayrıntılar**
--- | ---
VMware vm'lerinin olağanüstü durum kurtarma | Şirket içi VMware Vm'lerini azure'a çoğaltma. Bu senaryo Azure portalında veya kullanarak dağıtabileceğiniz [PowerShell](vmware-azure-disaster-recovery-powershell.md).
Fiziksel sunucuları olağanüstü durum kurtarma | Şirket içi Windows/Linux fiziksel sunucuları azure'a çoğaltma. Bu senaryoda Azure Portalı'nda dağıtabilirsiniz.

## <a name="on-premises-virtualization-servers"></a>Şirket içi sanallaştırma sunucuları

**Sunucu** | **Gereksinimler** | **Ayrıntılar**
--- | --- | ---
vCenter Server | Sürüm 6.7, 6.5, 6.0 veya 5.5 | Olağanüstü durum kurtarma dağıtımınızda bir vCenter sunucusu kullanmanızı öneririz.
vSphere konakları | Sürüm 6.7, 6.5, 6.0 veya 5.5 | VSphere konaklarını ve vCenter sunucularını işlem sunucusu aynı ağda bulunan öneririz. Varsayılan olarak, işlem sunucusu yapılandırma sunucusunda çalışır. [Daha fazla bilgi edinin](vmware-physical-azure-config-process-server-overview.md).


## <a name="site-recovery-configuration-server"></a>Site Recovery yapılandırma sunucusu

Yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucusu da dahil olmak üzere, Site Recovery bileşenlerini çalıştıran bir şirket içi makineyi yapılandırma sunucusudur.

- VMware Vm'leri için yapılandırma sunucusunu bir VMware VM oluşturmak için OVF şablonunu indirerek ayarlayın.
- Fiziksel sunucular için yapılandırma sunucusu makineyi el ile ayarlayın.

**Bileşen** | **Gereksinimler**
--- |---
CPU çekirdekleri | 8
RAM | 16 GB
Disk sayısı | 3 diskler<br/><br/> Diskler, işletim sistemi diski, işlem sunucusu önbellek diski ve yeniden çalışma için bekletme sürücüsü içerir.
Boş disk alanı | İşlem sunucusu önbelleği için alanı 600 GB.
Boş disk alanı | Bekletme sürücüsü için alanı 600 GB.
İşletim sistemi  | Windows Server 2012 R2 veya Windows Server 2016 masaüstü deneyimi ile |
İşletim sistemi yerel ayarı | İngilizce (en-us)
[PowerCLI](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) | Configuration server sürümü için gerekli değildir [9.14](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery) veya üzeri. 
Windows Server rolleri | Active Directory Domain Services etkinleştirme; Internet Information Services (IIS) veya Hyper-V. 
Grup İlkeleri| -Komut istemine erişimi engelleyin. <br/> -Kayıt defteri düzenleme araçlarına erişimi engelleyin. <br/> -Mantıksal dosya ekleri için güven. <br/> -Betik yürütmeyi açma. <br/> - [Daha fazla bilgi edinin](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
IIS | Emin olun:<br/><br/> -Önceden var olan bir varsayılan Web sitesi yok <br/> -Etkinleştir [anonim kimlik doğrulaması](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br/> -Etkinleştir [Fastcgı](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarı  <br/> -Önceden var olan Web sitesi/uygulama bağlantı noktası 443 üzerinde dinleme yok<br/>
NIC türü | VMXNET3 (VMware VM olarak dağıtıldığında)
IP adresi türü | Statik
Bağlantı Noktaları | denetim kanalı düzenleme için kullanılan 443<br/>veri aktarımı için 9443

## <a name="replicated-machines"></a>Çoğaltılan makineler

Site Recovery, desteklenen bir makinede çalışan tüm iş yüklerini çoğaltılmasını destekler.

**Bileşen** | **Ayrıntılar**
--- | ---
Makine ayarları | Azure'a çoğaltılan makineler karşılamalıdır [Azure gereksinimleri](#azure-vm-requirements).
Makine iş yükü | Site Recovery, desteklenen bir makinede çalışan tüm iş yüklerini çoğaltılmasını destekler. [Daha fazla bilgi edinin](https://aka.ms/asr_workload).
Windows | -Windows Server 2019 (desteklenir [güncelleştirme paketi 34](https://support.microsoft.com/help/4490016) (sürüm 9.22 Mobility hizmetinin) ve sonraki sürümler.<br/> -Windows Server 2016 (64 bit Sunucu Çekirdeği, masaüstü deneyimi ile sunucu)<br/> - Windows Server 2012 R2, Windows Server 2012<br/> -Itanium tabanlı sistemler için Windows Server 2008 R2 ile en az SP1.<br/> -Windows Server 2008, 64 ve 32-bitten en azından SP2]. Geçiş için desteklenmiyor. [Daha fazla bilgi edinin](migrate-tutorial-windows-server-2008.md).<br/> -Windows 10, Windows 8.1, Windows 8, Windows 7 64-bit (desteklenir [güncelleştirme paketi 36](https://support.microsoft.com/help/4503156) (9.22 Mobility hizmetinin sürümü ve üzeri). Windows 7 RTM desteklenmez. 
Linux | Yalnızca 64 bit sistem desteklenir. 32-bit sistem desteklenmez.<br/><br/>Her bir Linux sunucusu olmalıdır [Linux Integration Services (LIS) bileşenleri](https://www.microsoft.com/download/details.aspx?id=55106) yüklü. Test yük devretme/yük devretmeden sonra Azure'da sunucu önyüklemesi için gereklidir. Yüklemek için LIS bileşenleri eksikse olun [bileşenleri](https://www.microsoft.com/download/details.aspx?id=55106) Azure'da önyükleme makineler için çoğaltmayı etkinleştirmeden önce. <br/><br/> Site Recovery Azure'da Linux sunucuları çalıştırmak için yük devretme işlemlerini yönetir. Ancak Linux satıcılar yalnızca son yaşam geçmediği dağıtım sürümleri için destek sınırlayabilir.<br/><br/> Linux dağıtımlarında, dağıtım podverze yayın/güncelleştirmenin parçası olan stok çekirdekler desteklenir.<br/><br/> Korumalı makineler arasında önemli Linux dağıtım sürümleri desteklenmez yükseltiliyor. Yükseltmek için çoğaltmayı devre dışı bırak, işletim sistemini yükseltin ve ardından çoğaltmayı yeniden etkinleştirin.<br/><br/> [Daha fazla bilgi edinin](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure) Linux ve açık kaynak teknolojisi azure'da desteği hakkında.
Linux Red Hat Enterprise | 5.2 için 5.11</b><br/> 6.1 için 6.10</b><br/> 7.0 için 7.6<br/> <br/> 5\.2-5.11 & 6.1 6.10 Red Hat Enterprise Linux çalıştıran sunucular gerekmez [Linux Integration Services (LIS) bileşenleri](https://www.microsoft.com/download/details.aspx?id=55106) önceden yüklenmiş. Yüklemek için olun [bileşenleri](https://www.microsoft.com/download/details.aspx?id=55106) Azure'da önyükleme makineler için çoğaltmayı etkinleştirmeden önce.
Linux: CentOS | 5.2 için 5.11</b><br/> 6.1 için 6.10</b><br/> 7.0 için 7.6<br/> <br/> 5\.2-5.11 & 6.1 6.10 CentOS çalıştıran sunucular gerekmez [Linux Integration Services (LIS) bileşenleri](https://www.microsoft.com/download/details.aspx?id=55106) önceden yüklenmiş. Yüklemek için olun [bileşenleri](https://www.microsoft.com/download/details.aspx?id=55106) Azure'da önyükleme makineler için çoğaltmayı etkinleştirmeden önce.
Ubuntu | Ubuntu 14.04 LTS server [(desteklenen gözden geçirme çekirdek sürümleri)](#ubuntu-kernel-versions)<br/><br/>Ubuntu 16.04 LTS server [(desteklenen gözden geçirme çekirdek sürümleri)](#ubuntu-kernel-versions)
Debian | Debian 7/Debian 8 [(desteklenen gözden geçirme çekirdek sürümleri)](#debian-kernel-versions)
SUSE Linux | SUSE Linux Enterprise Server 12 SP1, SP2, SP3, SP4 [(desteklenen gözden geçirme çekirdek sürümleri)](#suse-linux-enterprise-server-12-supported-kernel-versions)<br/> SUSE Linux Enterprise Server 11 SP3, SUSE Linux Enterprise Server 11 SP4<br/> Çoğaltılan makineler SP4 için SUSE Linux Enterprise Server 11 SP3 ' yükseltme desteklenmez. Yükseltmek için çoğaltmayı devre dışı bırakın ve yükseltmeden sonra yeniden etkinleştirin.
Oracle Linux | 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6<br/><br/> Red Hat uyumlu çekirdek veya kesilemeyen kurumsal çekirdek sürümü 3, 4 ve 5 (UEK3, UEK4, UEK5) çalıştıran 


### <a name="ubuntu-kernel-versions"></a>Ubuntu çekirdek sürümleri


**Desteklenen sürüm** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
14.04 LTS | [9.24][9.25 UR]  | 3.13.0-24-Generic 3.13.0-169-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-146-generic için<br/>4.15.0-1023-Azure 4.15.0-1042-azure için |
14.04 LTS | [9.24][9.24 UR] | 3.13.0-24-Generic 3.13.0-167-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-143-generic için<br/>4.15.0-1023-Azure 4.15.0-1040-azure için |
14.04 LTS | [9.23][9.23 UR] | 3.13.0-24-Generic 3.13.0-165-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-142-generic için<br/>4.15.0-1023-Azure 4.15.0-1037-azure için |
14.04 LTS | [9.22][9.22 UR] | 3.13.0-24-Generic 3.13.0-164-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-140-generic için<br/>4.15.0-1023-Azure 4.15.0-1036-azure için |
|||
16.04 LTS | [9.25][9.25 UR] | 4.4.0-21-Generic 4.4.0-146-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-45-generic için<br/>4.15.0-13-Generic 4.15.0-48-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1018-azure için <br/>4.15.0-1012-Azure 4.15.0-1042-azure için|
16.04 LTS | [9.24][9.24 UR] | 4.4.0-21-Generic 4.4.0-143-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-45-generic için<br/>4.15.0-13-Generic 4.15.0-46-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1018-azure için <br/>4.15.0-1012-Azure 4.15.0-1040-azure için|
16.04 LTS | [9.23][9.23 UR] | 4.4.0-21-Generic 4.4.0-142-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-45-generic için<br/>4.15.0-13-Generic 4.15.0-45-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1018-azure için <br/>4.15.0-1012-Azure 4.15.0-1037-azure için|
16.04 LTS | [9.22][9.22 UR] | 4.4.0-21-Generic 4.4.0-140-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-45-generic için<br/>4.15.0-13-Generic 4.15.0-43-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1018-azure için <br/>4.15.0-1012-Azure 4.15.0-1036-azure için|

### <a name="debian-kernel-versions"></a>Debian çekirdek sürümleri


**Desteklenen sürüm** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
Debian 7 | [9.21][9.21 UR], [9.22][9.22 UR],[9.23][9.23 UR], [9,24][9.24 UR]| 3.2.0-4-AMD64 3.2.0-6-amd64 için 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | [9.25][9.25 UR] | 3.16.0-4-AMD64 3.16.0-8-amd64, 4.9.0-0.bpo.4-amd64 4.9.0-0.bpo.8-amd64 için için |
Debian 8 | [9.22][9.22 UR],[9.23][9.23 UR], [9,24][9.24 URL] | 3.16.0-4-AMD64 3.16.0-7-amd64, 4.9.0-0.bpo.4-amd64 4.9.0-0.bpo.8-amd64 için için |


### <a name="suse-linux-enterprise-server-12-supported-kernel-versions"></a>SUSE Linux Enterprise Server 12 çekirdeği sürümlerinde desteklenir.

**Yayın** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
SUSE Linux Enterprise Server (SP1, SP2, SP3, SP4) 12 | [9.25][9.25 UR] | SP1 3.12.49-11-default 3.12.74-60.64.40-default için</br></br> SP1(LTSS) 3.12.74-60.64.45-default 3.12.74-60.64.107-default için</br></br> SP2 4.4.21-69-default 4.4.120-92.70-default için</br></br>SP2(LTSS) 4.4.121-92.73-default 4.4.121-92.104-default için</br></br>SP3 4.4.73-5-default 4.4.176-94.88-default için</br></br>SP3 4.4.138-4.7-azure 4.4.176-4.25-azure için</br></br>SP4 4.12.14-94.41-default 4.12.14-95.13-default için</br>SP4 4.12.14-6.3-azure 4.12.14-6.9-azure için |
SUSE Linux Enterprise Server (SP1, SP2, SP3, SP4) 12 | [9.24][9.24 UR] | SP1 3.12.49-11-default 3.12.74-60.64.40-default için</br></br> SP1(LTSS) 3.12.74-60.64.45-default 3.12.74-60.64.107-default için</br></br> SP2 4.4.21-69-default 4.4.120-92.70-default için</br></br>SP2(LTSS) 4.4.121-92.73-default 4.4.121-92.101-default için</br></br>SP3 4.4.73-5-default 4.4.175-94.79-default için</br></br>SP4 4.12.14-94.41-default 4.12.14-95.6-default için |
SUSE Linux Enterprise Server (SP1, SP2, SP3, SP4) 12 | [9.23][9.23 UR] | SP1 3.12.49-11-default 3.12.74-60.64.40-default için</br></br> SP1(LTSS) 3.12.74-60.64.45-default 3.12.74-60.64.107-default için</br></br> SP2 4.4.21-69-default 4.4.120-92.70-default için</br></br>SP2(LTSS) 4.4.121-92.73-default 4.4.121-92.101-default için</br></br>SP3 4.4.73-5-default 4.4.162-94.69-default için</br></br>SP4 4.12.14-94.41-default 4.12.14-95.6-default için |
SUSE Linux Enterprise Server (SP1, SP2 SP3) 12 | [9.22][9.22 UR] | SP1 3.12.49-11-default 3.12.74-60.64.40-default için</br></br> SP1(LTSS) 3.12.74-60.64.45-default 3.12.74-60.64.107-default için</br></br> SP2 4.4.21-69-default 4.4.120-92.70-default için</br></br>SP2(LTSS) 4.4.121-92.73-default 4.4.121-92.98-default için</br></br>SP3 4.4.73-5-default 4.4.162-94.72-default için |


## <a name="linux-file-systemsguest-storage"></a>Linux dosya sistemleri/Konuk depolama

**Bileşen** | **Destekleniyor**
--- | ---
Dosya sistemleri | ext3, ext4, XFS
Birim Yöneticisi | -LVM desteklenir.<br/> - / LVM önyükleme desteklenir [güncelleştirme toplaması 31](https://support.microsoft.com/help/4478871/) (sürüm 9.20 Mobility hizmetinin) ve sonraki sürümler. Önceki Mobility hizmeti sürümlerinde desteklenmez.<br/> -Birden çok işletim sistemi diskleri desteklenmez.
Parasanallaştırılmış depolama cihazları | Parasanallaştırılmış sürücüler tarafından dışarı aktarılan cihazlar desteklenmez.
Birden fazla kuyruk blok g/ç cihazları | Desteklenmiyor.
HP CCISS depolama denetleyicisi ile fiziksel sunucuları | Desteklenmiyor.
Cihaz/bağlama noktası adlandırma kuralı | Cihaz adı veya bağlama noktası adı benzersiz olmalıdır.<br/> Hiçbir iki cihazları/bağlama noktaları büyük küçük harfe duyarlı adlara sahip olduğundan emin olun. Örnek aynı VM için cihazları adlandırma *cihaz1* ve *cihaz1* desteklenmiyor.
Dizinler | Mobility hizmetinin sürümü 9.20'den önceki bir sürümünü çalıştırıyorsanız, (, çıkan [güncelleştirme toplaması 31](https://support.microsoft.com/help/4478871/)), sonra da bu sınırlamalar:<br/><br/> -Bu dizinleri (bölümler/dosya-sistemleri ayarlama olarak ayırarak,) kaynak sunucudaki aynı işletim sistemi diski olması gerekir: / (root), makinesiyse, / usr, / usr/local, /var, / etc.</br> -Makinesiyse dizin, bir disk bölümünde olmalı ve LVM birim olmaması gerekir.<br/><br/> Sürümünden 9.20 ve sonraki sürümlerde, bu kısıtlama geçerli değildir. 
Önyükleme dizini | -Birden fazla önyükleme diski bir sanal makine üzerinde desteklenir <br/> - / birden fazla disk arasında LVM birimdeki önyüklemesi desteklenmez.<br/> -Bir makine önyükleme diski olmadan yinelenemez.
Boş alan gereksinimleri| 2 GB/root bölümdeki <br/><br/> Yükleme klasöründeki 250 MB
XFSv5 | Meta veri sağlama gibi XFS dosya sistemlerindeki XFSv5 özellikler, desteklenen (Mobility hizmeti sürümü 9.10 ve sonraki sürümler) olabilir.<br/> Süper blok XFS kullanarak bölümü için denetlenecek xfs_info yardımcı programını kullanın. Varsa `ftype` XFSv5 özellikleri kullanımda olan 1 olarak ayarlanmışsa.
BTRFS | BTRFS desteklenir [güncelleştirme paketi 34](https://support.microsoft.com/help/4490016) (sürüm 9.22 Mobility hizmetinin) ve sonraki sürümler. BTRFS durumunda desteklenmez:<br/><br/> -BTRFS dosya sistemi subvolume koruma etkinleştirildikten sonra değişir.</br> -BTRFS dosya sistemi birden çok disk yayılır.</br> -RAID BTRFS dosya sistemini destekler.

## <a name="vmdisk-management"></a>VM/Disk Yönetimi

**Eylem** | **Ayrıntılar**
--- | ---
Çoğaltılmış sanal diski yeniden boyutlandırma | Destekleniyor.
Çoğaltılmış VM'ye disk ekleme | Desteklenmiyor.<br/> Sanal makine için çoğaltmayı devre dışı bırakmak, disk ekleyin ve ardından çoğaltmayı yeniden etkinleştirin.

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
Hızlandırılmış ağ iletişimi | Hayır

## <a name="storage"></a>Depolama
**Bileşen** | **Destekleniyor**
--- | ---
Dinamik disk | İşletim sistemi diski, bir temel disk olması gerekir. <br/><br/>Veri diskleri dinamik diskleri olabilir.
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
Konuk/sunucu iSCSI | Hayır
Konuk/sunucu SMB 3.0 | Hayır
Konuk/sunucu RDM | Evet<br/><br/> Fiziksel sunucular için yok
Konuk/sunucu disk > 1 TB | Evet<br/><br/>4\.095 GB'a kadar<br/><br/> Disk 1024 MB değerinden daha büyük olmalıdır.
Konuk/sunucu disk ile 4 K mantıksal ve 4 k fiziksel kesim boyutu | Hayır
Konuk/sunucu disk ile 4K mantıksal ve fiziksel kesim boyutu 512 bayt | Hayır
Bölüştürülmüş bir disk ile Konuk/sunucusu birimi > 4 TB <br/><br/>Mantıksal birim yönetimi (LVM)| Evet
Konuk/sunucu - depolama alanları | Hayır
Konuk/sunucu sık erişimli Ekle/Kaldır disk | Hayır
Konuk/sunucu - disk dışlama | Evet
Konuk/sunucu çok yollu (MPIO) | Hayır
Konuk/sunucu GPT bölüm | Beş bölümler desteklenen [güncelleştirme paketi 37](https://support.microsoft.com/help/4508614/) (sürüm 9,25 Mobility hizmetinin) ve sonraki sürümler. Dört daha önce desteklenen.
Konuk/server EFI/UEFI'ye önyükleme | -Mobility hizmeti sürümü 9.13 veya üstü çalıştırırken desteklenmiyor.<br/> -VMware Vm'lerini veya fiziksel sunucuları Windows Server 2012 çalıştıran geçiş yaparken ya da daha sonra Azure'a desteklenir.<br/> -Sanal makineleri geçiş için yalnızca çoğaltabilirsiniz. Şirket içine yeniden çalışma desteklenmez.<br/> -Yalnızca NTFS destekli & güvenli UEFI önyükleme türü desteklenmiyor. <br/> -Disk sektör boyutu 512 bayt / kesim fiziksel olmalıdır.

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
Bekleyen şifreleme (SSE)| Evet
Premium depolama | Evet
İçeri/dışarı aktarma hizmeti | Hayır
Sanal ağlar için Azure depolama güvenlik duvarları | Evet.<br/> Hedef depolama/önbellek depolama hesabı (çoğaltma verilerini depolamak için kullanılan) üzerinde yapılandırılmış.
Genel amaçlı v2 depolama hesaplarının (sık erişimli ve seyrek erişimli Katmanlar) | Evet (maliyetleri V2 V1 kıyasla önemli ölçüde daha yüksek işlem)

## <a name="azure-compute"></a>Azure işlem

**Özelliği** | **Destekleniyor**
--- | ---
Kullanılabilirlik kümeleri | Evet
Kullanılabilirlik alanları | Hayır
HUB | Evet
Yönetilen diskler | Evet

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure'a çoğaltılan VM'ler bu tabloda özetlenen Azure VM gereksinimleri karşılamalıdır şirket içi. Site Recovery çoğaltması için bir önkoşul denetimi çalıştığında, bazı gereksinimleri karşılanmadığı takdirde denetimi başarısız olur.

**Bileşen** | **Gereksinimler** | **Ayrıntılar**
--- | --- | ---
Konuk işletim sistemi | Doğrulama [desteklenen işletim sistemleri](#replicated-machines) çoğaltılan makineler için. | Onay desteklenmeyen başarısız olur.
Konuk işletim sistemi mimarisi | 64-bit. | Onay desteklenmeyen başarısız olur.
İşletim sistemi disk boyutu | 2\.048 GB. | Onay desteklenmeyen başarısız olur.
İşletim sistemi disk sayısı | 1 | Onay desteklenmeyen başarısız olur.
Veri diski sayısı | 64 veya daha az. | Onay desteklenmeyen başarısız olur.
Veri diski boyutu | 4\.095 GB'a kadar | Onay desteklenmeyen başarısız olur.
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir. |
Paylaşılan VHD | Desteklenmiyor. | Onay desteklenmeyen başarısız olur.
FC diski | Desteklenmiyor. | Onay desteklenmeyen başarısız olur.
BitLocker | Desteklenmiyor. | Bir makine için çoğaltmayı etkinleştirmeden önce BitLocker'ı devre dışı bırakılması gerekir. |
VM adı | 1 63 karakter.<br/><br/> Harfler, sayılar ve kısa çizgilerden oluşabilir.<br/><br/> Makine adı başlamalı ve bir harf veya sayı ile bitmelidir. |  Site recovery'de makine özellikleri değerini güncelleştirin.

## <a name="churn-limits"></a>Sınırları değişim sıklığı

Aşağıdaki tablo, Azure Site Recovery sınırlarını sağlar. 
- Bu limitler yaptığımız testleri temel alarak, ancak tüm olası uygulama g/ç birleşimlerini kapsamaz.
- Gerçek sonuçlar, uygulamanızın G/Ç karışımına göre değişebilir.
- En iyi sonuçlar için çalıştırmanızı öneririz [dağıtım Planlayıcısı aracını](site-recovery-deployment-planner.md), kullanarak kapsamlı uygulama testi gerçekleştirin uygulamanız için gerçek performans görüntüsünü almak için yük devretme testi.

**Çoğaltma hedefi** | **Ortalama kaynak disk G/Ç boyutu** |**Ortalama kaynak disk veri değişim sıklığı** | **Günlük toplam kaynak disk veri değişim sıklığı**
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

- Bunlar yüzde 30 G/Ç çakışmasını varsayan ortalama sayılardır.
- Site Recovery; çakışma oranı, büyük yazma boyutları ve gerçek iş yükü G/Ç davranışına göre daha yüksek aktarım hızını işleyebilir.
- Bu sayı yaklaşık beş dakikalık tipik bir kapsamı varsayar. Diğer bir deyişle, veriler karşıya yüklendikten sonra işlenir ve beş dakika içinde kurtarma noktası oluşturulur.

## <a name="vault-tasks"></a>Kasa görevleri

**Eylem** | **Destekleniyor**
--- | ---
Kasa kaynak grupları arasında taşıma | Hayır
Kasa içinde ve abonelikler arasında taşıma | Hayır
Depolama, ağ, Azure Vm'leri kaynak grupları arasında taşıma | Hayır
Depolama, ağ, Azure Vm'lerinin içinde ve abonelikler arasında taşıyın. | Hayır


## <a name="obtain-latest-components"></a>En yeni bileşenleri alın

**Name** | **Açıklama** | **Ayrıntılar**
--- | --- | ---
Yapılandırma sunucusu | Yüklü şirket içi.<br/> Şirket içi VMware sunucularını veya fiziksel makineler ve Azure arasındaki iletişimleri koordine eder. | - [Hakkında bilgi edinin](vmware-physical-azure-config-process-server-overview.md) yapılandırma sunucusu.<br/> - [Hakkında bilgi edinin](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) en son sürüme yükseltme.<br/> - [Hakkında bilgi edinin](vmware-azure-deploy-configuration-server.md) yapılandırma sunucusunu ayarlama. 
İşlem sunucusu | Varsayılan olarak yapılandırma sunucusuna yüklenir.<br/> Çoğaltma verilerini alıp, önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure'a gönderir.<br/> Dağıtımınız büyüdükçe, daha büyük çoğaltma trafiği hacimlerini idare etmek ek işlem sunucuları ekleyebilirsiniz. | - [Hakkında bilgi edinin](vmware-physical-azure-config-process-server-overview.md) işlem sunucusu.<br/> - [Hakkında bilgi edinin](vmware-azure-manage-process-server.md#upgrade-a-process-server) en son sürüme yükseltme.<br/> - [Hakkında bilgi edinin](vmware-physical-large-deployment.md#set-up-a-process-server) genişleme işlem sunucuları kurma.
Mobility hizmeti | VMware VM veya fiziksel sunucuları çoğaltmak istediğiniz yüklü.<br/> Şirket içi VMware sunucuları/fiziksel sunucular ile Azure arasında çoğaltma düzenler.| - [Hakkında bilgi edinin](vmware-physical-mobility-service-overview.md) Mobility hizmeti.<br/> - [Hakkında bilgi edinin](vmware-physical-manage-mobility-service.md#update-mobility-service-from-azure-portal) en son sürüme yükseltme.<br/> 



## <a name="next-steps"></a>Sonraki adımlar
[Bilgi nasıl](tutorial-prepare-azure.md) Azure VMware vm'lerinin olağanüstü durum kurtarmasına hazırlanmak için.

[9.25 UR]: https://support.microsoft.com/en-in/help/4508614/update-rollup-37-for-azure-site-recovery
[9.24 UR]: https://support.microsoft.com/en-in/help/4503156
[9.23 UR]: https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery
[9.22 UR]: https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery
[9.21 UR]: https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery
[9.20 UR]: https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery
[9.19 UR]: https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30
[9.18 UR]: https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery
