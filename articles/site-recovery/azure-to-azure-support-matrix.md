---
title: "Azure'dan Azure'a çoğaltmak için azure Site Recovery destek matrisi | Microsoft Docs"
description: "Desteklenen işletim sistemleri ve yapılandırmalar Azure sanal makineleri (VM'ler) Azure Site Recovery çoğaltma için olağanüstü durum kurtarma (DR) ihtiyaçları için başka bir tek bölgesinden özetler."
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: sujayt
ms.openlocfilehash: 4383286285f02bad1645344fab43f8b6bdb145cb
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="azure-site-recovery-support-matrix-for-replicating-from-azure-to-azure"></a>Azure'dan Azure'a çoğaltmak için azure Site Recovery destek matrisi


>[!NOTE]
>
> Azure sanal makineler için Site Recovery çoğaltma şu anda önizlemede değil.

Bu makalede, Azure Site Recovery çoğaltma ve Azure sanal makineleri başka bir bölgeye bir bölgesinden kurtarmak için desteklenen yapılandırmalar ve bileşenleri özetlenmektedir.

## <a name="user-interface-options"></a>Kullanıcı arabirimi seçenekleri

**Kullanıcı arabirimi** |  **Desteklenen / desteklenmeyen**
--- | ---
**Azure portalı** | Desteklenen
**Klasik portal** | Desteklenmiyor
**PowerShell** | Şu anda desteklenmiyor
**REST API** | Şu anda desteklenmiyor
**CLI** | Şu anda desteklenmiyor


## <a name="resource-move-support"></a>Kaynak taşıma desteği

**Kaynak taşıma türü** | **Desteklenen / desteklenmeyen** | **Açıklamalar**  
--- | --- | ---
**Kasa kaynak grupları arasında taşıma** | Desteklenmiyor |Kurtarma Hizmetleri kasası kaynak gruplarında taşınamıyor.
**İşlem, depolama ve ağ kaynak grupları arasında taşıma** | Desteklenmiyor |Çoğaltma etkinleştirdikten sonra bir sanal makine (veya ilişkili bileşenlerini depolama ve ağ gibi) taşırsanız çoğaltmasını devre dışı bırakın ve sanal makine için çoğaltma işlemini yeniden etkinleştirmeniz gerekir.



## <a name="support-for-deployment-models"></a>Dağıtım modelleri için destek

**Dağıtım modeli** | **Desteklenen / desteklenmeyen** | **Açıklamalar**  
--- | --- | ---
**Klasik** | Desteklenen | Yalnızca klasik bir sanal makine çoğaltabilir ve klasik sanal makine olarak kurtarın. Resource Manager sanal makine olarak kurtaramazsınız. Klasik bir VM sanal ağ olmadan ve doğrudan bir Azure bölgesi dağıtırsanız, desteklenmiyor.
**Resource Manager** | Desteklenen |

>[!NOTE]
>
> 1. Olağanüstü durum kurtarma senaryoları için başka bir Azure sanal makineleri bir abonelikten çoğaltma desteklenmiyor.
> 2. Geçirme Azure abonelikleri sanal makinelerde desteklenmiyor.
> 3. Geçirme Azure sanal makineleri aynı bölge içinde desteklenmiyor.
> 4. Azure sanal makineleri Klasik dağıtım modelinden manager dağıtım modeli desteklenmiyor kaynağa geçirme.

## <a name="support-for-replicated-machine-os-versions"></a>Çoğaltılan makinenin işletim sistemi sürümleri için destek

Destek sözü edilen işletim sisteminde çalışan herhangi bir iş yükü için geçerlidir.

#### <a name="windows"></a>Windows

- Windows Server 2016 (Sunucu Çekirdeği, masaüstü deneyimi olan sunucu) *
- Windows Server 2012 R2
- Windows Server 2012
- Itanium tabanlı sistemler için Windows Server 2008 R2 ile en az SP1

>[!NOTE]
>
> \* Windows Server 2016 Nano Server desteklenmiyor.

#### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 6.7, 6.8, 6.9, 7.0, 7.1, 7.2, 7.3,7.4
- CentOS 6.5, 6.6, 6.7, 6.8, 6.9, 7.0, 7.1, 7.2, 7.3,7.4
- Ubuntu 14.04 LTS Server [ (çekirdek sürümleri desteklenir)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Ubuntu 16.04 LTS Server [ (çekirdek sürümleri desteklenir)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Debian 7
- Debian 8
- Oracle Enterprise Linux 6.4, Red Hat uyumlu çekirdek ya da kesilemeyen kurumsal çekirdek sürüm 3 (UEK3) çalıştıran 6.5
- SUSE Linux Enterprise Server 11 SP3
- SUSE Linux Enterprise Server 11 SP4

(Makineler SLES 11 SP4 ' SLES 11 SP3 çoğaltılan yükseltme desteklenmez. Çoğaltılmış bir makineden SLES 11 SP4 ' SLES 11SP3 yükseltildiyse, çoğaltmayı devre dışı bırakın ve makine yükseltme sonrası yeniden korumak gerekir.)

>[!NOTE]
>
> Parola tabanlı kimlik doğrulama ve oturum açma kullanarak ve bulut sanal makineleri yapılandırmak için bulut init paketini kullanarak Ubuntu sunucuları parola tabanlı oturum açma (yapılandırmasına bağlı olarak cloudinit.) yük devretme sırasında devre dışı olabilir Parola tabanlı oturum açma olabilir sanal makinede yeniden etkin Ayarlar menüsünden parolayı sıfırlayarak (Destek + sorun giderme altında bölümünde), başarısız sanal makineyi Azure portalı üzerinden.

### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Azure sanal makineler için desteklenen Ubuntu çekirdek sürümleri

**Sürüm** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
14.04 LTS | 9.10 | 3.13.0-24-Generic 3.13.0-121-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-81-generic 4.4.0-21-Generic |
14.04 LTS | 9.11 | 3.13.0-24-Generic 3.13.0-125-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-83-generic 4.4.0-21-Generic |
14.04 LTS | 9.12 | 3.13.0-24-Generic 3.13.0-132-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-96-generic 4.4.0-21-Generic |
14.04 LTS | 9.13 | 3.13.0-24-Generic 3.13.0-137-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-104-generic 4.4.0-21-Generic |
16.04 LTS | 9.10 | 4.4.0-21-Generic 4.4.0-81-generic için<br/>4.8.0-34-Generic 4.8.0-56-generic için<br/>4.10.0-24-generic 4.10.0-14-Generic |
16.04 LTS | 9.11 | 4.4.0-21-Generic 4.4.0-83-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-27-generic 4.10.0-14-Generic |
16.04 LTS | 9.12 | 4.4.0-21-Generic 4.4.0-96-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-35-generic 4.10.0-14-Generic |
16.04 LTS | 9.13 | 4.4.0-21-Generic 4.4.0-104-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-42-generic 4.10.0-14-Generic |

## <a name="supported-file-systems-and-guest-storage-configurations-on-azure-virtual-machines-running-linux-os"></a>Desteklenen dosya sistemleri ve Linux işletim sistemi çalıştıran Azure sanal makinelerinde Konuk depolama yapılandırmaları

* Dosya sistemleri: ext3, ext4, ReiserFS (Suse Linux Enterprise Server yalnızca), XFS
* Volume manager: LVM2
* Çok yollu yazılım: cihaz Eşleyici

## <a name="region-support"></a>Bölge desteği

Çoğaltma ve aynı coğrafi küme içinde iki tüm bölgeler arasında sanal makineleri kurtarın.

**Coğrafi küme** | **Azure bölgeleri**
-- | --
Amerika | Doğu Kanada, Kanada Merkezi, Güney Orta ABD, Batı Orta ABD, Doğu ABD, Doğu ABD 2, Batı ABD, Batı ABD 2, Orta ABD, Kuzey Orta ABD
Avrupa | Birleşik Krallık Batı, Birleşik Krallık Güney, Kuzey Avrupa, Batı Avrupa
Asya | Güney Hindistan, Orta Hindistan, Güneydoğu Asya, Doğu Asya, Japonya Doğu, Kore Orta, Kore Güney Batı, Japonya
Avustralya   | Avustralya Doğu, Avustralya Güneydoğu
Azure Kamu    | ABD kamu Virginia, ABD kamu Iowa, ABD kamu Arizona, ABD kamu Texas, Doğu ABD Savunma Bakanlığı, ABD Savunma Bakanlığı Orta
Almanya | Almanya Orta Almanya Kuzeydoğu
Çin | Çin, Doğu, Çin Kuzey

>[!NOTE]
>
> Brezilya Güney bölgesi için yalnızca çoğaltma ve bir orta Güney ABD, Batı Orta ABD, Doğu ABD, Doğu ABD 2, Batı ABD, Batı ABD 2 ve Kuzey Orta ABD bölgeler yük devri ve geri dönecek.


## <a name="support-for-compute-configuration"></a>İşlem yapılandırması için destek

**Yapılandırma** | **Desteklenen/desteklenmeyen** | **Açıklamalar**
--- | --- | ---
Boyut | Tüm Azure VM boyutu en az 2 CPU çekirdekleri ve 1 GB RAM | Başvurmak [Azure sanal makine boyutları](../virtual-machines/windows/sizes.md)
Kullanılabilirlik kümeleri | Desteklenen | Portalı'nda 'çoğaltmasını Etkinleştir' adımı sırasında varsayılan seçeneği kullanırsanız, kullanılabilirlik otomatik olarak Kaynak bölgesi yapılandırmasına göre oluşturulan kümesidir. Hedef kullanılabilirlik kümesi değiştirebileceğiniz ' yinelenmiş öğesi > Ayarlar > işlem ve ağ > kullanılabilirlik kümesi ' dilediğiniz zaman.
Karma kullanımı Avantajı (HUB) VM'ler | Desteklenen | Kaynak VM etkin HUB lisansı varsa, yük devretme testi veya yük devretme VM ayrıca HUB lisans kullanır.
Sanal makine ölçek kümeleri | Desteklenmiyor |
Microsoft Azure galeri görüntüleri - yayımlandı | Desteklenen | Site Recovery tarafından desteklenen bir işletim sisteminde VM çalıştığı sürece desteklenen
-Üçüncü taraf yayımlanan Azure galeri görüntüleri | Desteklenen | Site Recovery tarafından desteklenen bir işletim sisteminde VM çalıştığı sürece desteklenir.
Özel görüntü - üçüncü taraf yayımlanan | Desteklenen | Site Recovery tarafından desteklenen bir işletim sisteminde VM çalıştığı sürece desteklenir.
Site Recovery kullanarak sanal makineleri geçişi | Desteklenen | Site Kurtarma'yı kullanarak Azure geçirilen VMware/fiziksel makineyi ise, mobility hizmeti eski sürümü kaldırın ve başka bir Azure bölgesine çoğaltma önce makineyi yeniden gerekir.

## <a name="support-for-storage-configuration"></a>Depolama yapılandırması için destek

**Yapılandırma** | **Desteklenen/desteklenmeyen** | **Açıklamalar**
--- | --- | ---
En yüksek işletim sistemi disk boyutu | 2048 GB | Başvurmak [VM'ler tarafından kullanılan diskler.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
En fazla veri diski boyutu | 4095 GB | Başvurmak [VM'ler tarafından kullanılan diskler.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Veri diski sayısı | Fazla belirli bir Azure VM boyutu tarafından desteklenen gibi 64 | Başvurmak [Azure sanal makine boyutları](../virtual-machines/windows/sizes.md)
Geçici disk | Her zaman Çoğaltmada hariç | Geçici disk her zaman çoğaltmadan dışlandı. Herhangi bir kalıcı veri Azure guida un göredir geçici diskteki moduna geçirmelisiniz değil. Başvurmak [Azure vm'lerinde geçici disk](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk) daha fazla ayrıntı için.
Disk üzerinde veri değişiklik oranı | En fazla 10 MB/sn Premium depolama için disk başına ve standart depolama için disk başına 2 MB/sn | Ortalama veri değişikliği hızını diskteki 10 MB/sn (için Premium) ile 2 MB/sn (için standart) sürekli olarak ise, çoğaltma yakalar değildir. Ancak, bazen veri veri bloğu ise ve veri değişikliği hızını 10 MB/sn (için Premium) ve 2 için MB/sn (standart) belirli bir süre için değerinden daha büyük ve gelir, çoğaltma Yakala. Bu durumda, biraz Gecikmeli kurtarma noktalarını görebilirsiniz.
Standart depolama hesapları disklerde | Desteklenen |
Premium depolama hesapları disklerde | Desteklenen | Bir VM premium ve standart depolama hesapları üzerinden yayılan diskler varsa, hedef bölgede aynı depolama yapılandırmasına sahip olmak her disk için farklı bir hedef depolama hesabı seçin
Standart yönetilen disk | Azure Site Recovery desteklendiği Azure bölgelerinde desteklenir. Kamu Bulutlar şu anda desteklenmemektedir.  |  
Premium yönetilen diskleri | Azure Site Recovery desteklendiği Azure bölgelerinde desteklenir. Kamu Bulutlar şu anda desteklenmemektedir. |
Depolama alanları | Desteklenen |         
Bekleyen (SSE) şifreleme | Desteklenen | Önbellek ve hedef depolama hesapları için etkin SSE depolama hesabı seçebilirsiniz.     
Azure Disk şifrelemesi (ADE) | Desteklenmiyor |
Sık kullanılan Ekle/Kaldır disk | Desteklenmiyor | Veri diski VM'de ekleyip, çoğaltmayı devre dışı bırakın ve yeniden sanal makine için çoğaltmayı etkinleştirmek gerekir.
Diski hariç tutma | Desteklenmiyor|   Geçici disk varsayılan olarak çıkarılır.
LRS | Desteklenen |
GRS | Desteklenen |
RA-GRS | Desteklenen |
ZRS | Desteklenmiyor |  
Seyrek erişimli ve sık erişimli depolama | Desteklenmiyor | Sanal makine disklerini seyrek erişimli ve sık erişimli depolama üzerinde desteklenmez.
Sanal ağ hizmet uç noktaları (Azure Storage güvenlik duvarları ve sanal ağlar)  | Hayır | Çoğaltılan verileri depolamak için kullanılan önbellek depolama hesaplarında erişimine izin belirli Azure sanal ağlar desteklenmez.
Genel amaçlı V2 depolama hesapları (her ikisini de sık erişimli ve seyrek katman) | Hayır | İşlem maliyetleri artış, genel amaçlı önemli ölçüde V1 depolama hesapları ile karşılaştırıldığında.

>[!IMPORTANT]
> VM disk ölçeklenebilirlik ve performans hedefleri gözlemlemek olun [Linux](../virtual-machines/linux/disk-scalability-targets.md) veya [Windows](../virtual-machines/windows/disk-scalability-targets.md) tüm performans sorunlarını önlemek için sanal makineler. Varsayılan ayarları izlerseniz, Site Recovery gerekli diskler ve depolama hesapları kaynak yapılandırmasını temel alarak oluşturur. Özelleştirme ve kendi ayarlarınızı seçerseniz, kaynak VM'ler disk ölçeklenebilirlik ve performans hedefleri izleyin emin olun.

## <a name="support-for-network-configuration"></a>Ağ yapılandırması için destek
**Yapılandırma** | **Desteklenen/desteklenmeyen** | **Açıklamalar**
--- | --- | ---
Ağ arabirimi (NIC) | Belirli bir Azure VM boyutu tarafından desteklenen NIC'ler fazla sayısı | VM yük devretme testi veya yük devretme işlemi kapsamında oluşturulduğunda NIC'ler oluşturulur. VM yük devretme NIC'ler sayısına çoğaltma etkinleştirme sırasında VM sahip kaynak NIC'ler sayısına bağlıdır. Ekleyin veya NIC çoğaltma etkinleştirdikten sonra Kaldır, yük devretme VM NIC sayısına etkilemez.
İnternet Yük Dengeleyici | Desteklenen | Önceden yapılandırılmış yük dengeleyici bir azure Otomasyonu komut dosyası kullanarak bir kurtarma planı ilişkilendirmeniz gerekir.
İç yük dengeleyici | Desteklenen | Önceden yapılandırılmış yük dengeleyici bir azure Otomasyonu komut dosyası kullanarak bir kurtarma planı ilişkilendirmeniz gerekir.
Genel IP| Desteklenen | NIC zaten var olan bir ortak IP ilişkilendirmek veya bir tane oluşturun ve bir kurtarma planı bir azure Otomasyonu komut dosyası kullanarak NIC için ilişkilendirmeniz gerekir.
NSG üzerinde NIC'ye (Resource Manager)| Desteklenen | NSG'yi bir kurtarma planı bir azure Otomasyonu komut dosyası kullanarak NIC ilişkilendirmeniz gerekir.  
NSG alt (Resource Manager ve klasik)| Desteklenen | NSG'yi bir kurtarma planı bir azure Otomasyonu komut dosyası kullanarak NIC ilişkilendirmeniz gerekir.
NSG VM'ye (Klasik)| Desteklenen | NSG'yi bir kurtarma planı bir azure Otomasyonu komut dosyası kullanarak NIC ilişkilendirmeniz gerekir.
Ayrılmış IP (statik IP) / kaynak IP koru | Desteklenen | Statik IP yapılandırması NIC kaynak VM üzerinde varsa ve aynı IP kullanılabilir hedef alt ağa sahip, yük devretme VM atanır. Hedef alt aynı IP yoksa kullanılabilir IP alt ağda biri bu VM için ayrılmıştır. Tercih ettiğiniz bir sabit IP belirtebilirsiniz ' yinelenmiş öğesi > Ayarlar > işlem ve ağ > ağ arabirimleri. NIC seçin ve tercih ettiğiniz IP ve alt ağ belirtin.
Dynamic IP| Desteklenen | NIC kaynak VM üzerinde dinamik IP yapılandırması varsa, yük devretme NIC'nin VM de varsayılan olarak dinamik bir işlemdir. Tercih ettiğiniz bir sabit IP belirtebilirsiniz ' yinelenmiş öğesi > Ayarlar > işlem ve ağ > ağ arabirimleri. NIC seçin ve tercih ettiğiniz IP ve alt ağ belirtin.
Traffic Manager tümleştirmesi | Desteklenen | Önceden, trafik Yöneticisi trafiği düzenli olarak kaynak bölgede uç noktasına ve yük devretme durumunda hedef bölgesi uç yönlendirilir şekilde yapılandırabilirsiniz.
Azure DNS yönetilen | Desteklenen |
Özel DNS  | Desteklenen |    
Kimliği doğrulanmamış Proxy | Desteklenen | Başvurmak [Ağ Kılavuzu belge.](site-recovery-azure-to-azure-networking-guidance.md)    
Doğrulanmış bir Proxy | Desteklenmiyor | VM için giden bağlantı doğrulanmış bir proxy kullanıyorsa, Azure Site RECOVERY'yi kullanarak yinelenemez.    
Siteden siteye VPN ile şirket içi (ile veya ExpressRoute olmadan)| Desteklenen | Nsg'ler ve Udr'ler Site kurtarma trafiği şirket içi yönlendirilmedi şekilde yapılandırıldığından emin olun. Başvurmak [Ağ Kılavuzu belge.](site-recovery-azure-to-azure-networking-guidance.md)  
VNET'e bağlantı | Desteklenen | Başvurmak [Ağ Kılavuzu belge.](site-recovery-azure-to-azure-networking-guidance.md)  


## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [Azure Vm'lerini çoğaltma kılavuz ağ oluşturma](site-recovery-azure-to-azure-networking-guidance.md)
- İş yükleri tarafından korumaya başlamak [Azure Vm'lerini çoğaltma](site-recovery-azure-to-azure.md)
