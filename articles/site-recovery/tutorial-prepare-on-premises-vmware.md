---
title: "VMware Vm'leri olağanüstü durum kurtarma Azure için şirket içi VMware sunucuları hazırlama | Microsoft Docs"
description: "Olağanüstü durum kurtarma Azure Site Recovery hizmeti kullanılarak azure'a şirket içi VMware sunucuları hazırlama hakkında bilgi edinin."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 90a4582c-6436-4a54-a8f8-1fee806b8af7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 33ec5775a371a04074f07d589d35d1c05bd64d30
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="prepare-on-premises-vmware-servers-for-disaster-recovery-to-azure"></a>Olağanüstü durum kurtarma Azure için şirket içi VMware sunucuları hazırlama

Bu öğretici, VMware Vm'lerini Azure'a çoğaltma istediğinizde, şirket içi VMware altyapınızı hazırlama gösterilmiştir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * VM bulma otomatikleştirmek için vCenter sunucusu veya vSphere ESXi ana bilgisayarında, bir hesap hazırlama
> * Mobility hizmetinin VMware vm'lerinde otomatik yüklemesi için bir hesap hazırlama
> * VMware server gereksinimlerini gözden geçirin
> * VMware VM gereksinimlerini gözden geçirin

Bu öğretici serisinde tek bir VM'yi yedeklemek nasıl Azure Site RECOVERY'yi kullanarak gösteriyoruz. Birden çok VMware Vm'leri korumak planlıyorsanız indirmelisiniz [dağıtım planlayıcısı aracı](https://aka.ms/asr-deployment-planner) VMware çoğaltma için. Bu araç VM uyumluluk, VM başına disk ve disk başına veri dalgalanmasına hakkında bilgi toplayın. Aracı, ağ bant genişliği gereksinimlerini ve başarılı çoğaltma ve test yük devretme için gereken Azure altyapı da kapsar. [Daha fazla bilgi edinin](site-recovery-deployment-planner.md) aracı çalıştırma hakkında.

Bu serideki ikinci öğreticidir. Bilgisayarınızda yüklü olduğundan emin olun [Azure bileşenleri ayarlamak](tutorial-prepare-azure.md) önceki öğreticide açıklandığı gibi.

## <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site kurtarma VMware sunucularına erişimi gerekir:

- Sanal makineleri otomatik olarak bulur. En az bir salt okunur hesabı gereklidir.
- Çoğaltma, yük devretme ve yeniden çalışma yönetirler. Sanal makine oluşturma ve diskleri kaldırma ve başlatırken gibi işlemleri çalıştırılan bir hesap gerekir.

Hesap gibi oluşturun:

1. Adanmış bir hesap kullanmak için vCenter düzeyinde bir rolü oluşturun. Rolü gibi bir ad verin **Azure_Site_Recovery**.
2. Rolü, aşağıdaki tabloda özetlenen izinleri atayın.
3. VCenter sunucusu veya vSphere ana bilgisayarda bir kullanıcı oluşturun. Kullanıcıya rol atayın.

### <a name="vmware-account-permissions"></a>VMware hesabı izinleri

**Görev** | **Rol/izinleri** | **Ayrıntılar**
--- | --- | ---
**VM bulma** | En az bir salt okunur kullanıcı<br/><br/> Veri Merkezi Nesne –> Propagate alt nesne için rol = salt okunur | Kullanıcı veri merkezi düzeyde atanan ve veri merkezinde tüm nesnelere erişimi vardır.<br/><br/> Erişimi kısıtlamak için Ata **erişim yok** rolüyle **alt Propagate** (vSphere ana bilgisayarları, datastores, sanal makineleri ve ağları) alt nesneleri için nesne.
**Tam çoğaltma, yük devretme, yeniden çalışma** |  Gerekli izinlere sahip bir rol (Azure_Site_Recovery) oluşturun ve bir VMware kullanıcı veya grup rolü atayın<br/><br/> Veri Merkezi Nesne –> Propagate alt nesne için rol Azure_Site_Recovery =<br/><br/> Veri deposu alanı Ayır ->, veri deposu, alt düzey dosya işlemleri göz atın, dosyayı kaldırmak, sanal makine dosyalarını güncelleştir<br/><br/> Ağ -> Ağ atama<br/><br/> Kaynak VM atamak için kaynak havuzu ->, VM güç beslemeli geçirmek, VM güç beslemeli geçirme<br/><br/> Görevler oluşturma görevi, güncelleştirme görevi -><br/><br/> Sanal Makine Yapılandırma -><br/><br/> Sanal makine -> etkileşimde bulunma yanıt soru, cihaz bağlantısı ->, CD ortamı yapılandırmak, disket ortamı, kapatma, açma, VMware araçları yükleme yapılandırın<br/><br/> Sanal makine -> Stok Oluştur ->, kaydetme, kaydı<br/><br/> Sanal makine sağlama -> izin sanal makine indirme ->, sanal makine dosyalarını karşıya yükleme izin ver<br/><br/> Sanal makine anlık görüntüleri -> Kaldır anlık görüntüleri -> | Kullanıcı veri merkezi düzeyde atanan ve veri merkezinde tüm nesnelere erişimi vardır.<br/><br/> Erişimi kısıtlamak için Ata **erişim yok** rolüyle **alt Propagate** (vSphere ana bilgisayarları, datastores, sanal makineleri ve ağları) alt nesneleri için nesne.

## <a name="prepare-an-account-for-mobility-service-installation"></a>Mobility hizmeti yüklemesi için bir hesap hazırlama

Mobility hizmetinin çoğaltmak istediğiniz VM üzerinde yüklü olmalıdır. Sanal makine için çoğaltmayı etkinleştirdiğinizde, site kurtarma Bu hizmeti otomatik olarak yüklenir. Otomatik yükleme için Site Recovery VM erişmek için kullanacağı bir hesap hazırlamanız gerekir. Olağanüstü durum kurtarma Azure konsolunda ayarladığınızda, bu hesabı belirtmeniz.

1. Bir etki alanı veya yerel hesap VM'de yüklemek için gerekli izinlere sahip hazırlayın.
2. Bir etki alanı hesabı kullanmıyorsanız Windows Vm'lerinde yüklemek için yerel makine üzerinde uzak kullanıcı erişim denetimi devre dışı bırakın.
   - Kayıt defterinden altında **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, DWORD girdisi eklemek **LocalAccountTokenFilterPolicy**, 1 değerine sahip.
3. Linux VM'ler yüklemek için kaynak Linux sunucusu üzerinde kök hesabı hazırlayın.


## <a name="check-vmware-server-requirements"></a>VMware server gereksinimlerini denetleyin

VMware sunucular aşağıdaki gereksinimleri karşıladığından emin olun.

**Bileşen** | **Gereksinim**
--- | ---
**vCenter sunucusu** | vCenter 6.5, 6.0 veya 5.5
**vSphere ana bilgisayar** | vSphere 6.5, 6.0, 5.5

## <a name="check-vmware-vm-requirements"></a>VMware VM gereksinimlerini denetleyin

VM aşağıdaki tabloda özetlenen Azure gereksinimlere uygun olduğundan emin olun.

**VM gereksinimi** | **Ayrıntılar**
--- | ---
**İşletim sistemi disk boyutu** | 2048 GB'a kadar.
**İşletim sistemi disk sayısı** | 1
**Veri diski sayısı** | 64 veya daha az
**Veri diski VHD boyutu** | 4095 GB'a kadar
**Ağ bağdaştırıcıları** | Birden çok bağdaştırıcı desteklenir
**Paylaşılan VHD** | Desteklenmiyor
**FC disk** | Desteklenmiyor
**Sabit disk biçimi** | VHD veya VHDX.<br/><br/> VHDX şu anda Azure'da desteklenmiyor olsa da, Site Recovery, Azure'a yük otomatik olarak VHDX VHD'ye dönüştürür. Şirket içi sanal makineleri başarısız olduğunda VHDX biçimi kullanmaya devam edin.
**BitLocker'ı** | Desteklenmiyor. Bir sanal makine için çoğaltmayı etkinleştirmeden önce devre dışı bırakın.
**VM adı** | 1 ile 63 karakter.<br/><br/> Harf, rakam ve kısa çizgi için kısıtlanmış. VM adı başlamalı ve bir harf veya sayı ile bitmelidir.
**VM türü** | Nesil 1 - Linux veya Windows<br/><br/>Nesil 2 - yalnızca Windows

VM de desteklenen bir işletim sistemi çalıştırması gerekir. Bkz: [Site Recovery destek matrisi](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) desteklenen sürümleri tam bir listesi.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Bir yük devretme senaryosu sırasında çoğaltılmış VM'ler için Azure'da şirket içi ağınızdan bağlanmak isteyebilirsiniz.

Windows Yük devretme işleminden sonra RDP kullanarak VM'ler bağlanmak için aşağıdakileri yapın:

1. Internet üzerinden erişmek için yük devretmeden önce şirket içi VM üzerinde RDP etkinleştirin. Bu TCP emin olun ve UDP kuralları için eklenir **ortak** profili ve RDP izin verildiğinden emin **Windows Güvenlik Duvarı** > **izin verilen uygulamaları** tüm profiller için.
2. Siteden siteye VPN üzerinden erişmek için şirket içi makinede RDP etkinleştirin. RDP izin içinde **Windows Güvenlik Duvarı** -> **verilen uygulamalar ve Özellikler** için **etki alanı ve özel** ağlar.
   İşletim sisteminin SAN ilkesinin kümesine onay **OnlineAll**. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135). Olmamalıdır bekleyen hiçbir Windows güncelleştirmeleri VM üzerinde bir yük devretme tetiklemek olduğunda. Varsa, güncelleştirme tamamlanana kadar sanal makineye oturum açamaz olmayacaktır.
3. Yük devretme sonrasında Windows Azure VM üzerinde kontrol **önyükleme tanılama** ekran VM görüntüsünü görüntülemek için. Bağlanamıyorsanız, VM çalışıp çalışmadığını denetleyin ve bu gözden [sorun giderme ipuçları](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Yük devretme sonrasında SSH kullanarak Linux VM'ler bağlanmak için aşağıdakileri yapın:

1. Yük devretmeden önce şirket içi makinede Secure Shell hizmetinin sistem önyüklemesinde otomatik olarak başlamaya ayarlanmıştır denetleyin. Güvenlik duvarı kuralları bir SSH bağlantısı izin verdiğinden emin olun.

2. Yük devretme sonrasında Azure VM'de, SSH bağlantı noktası için ağ güvenlik grubu kurallarının devredilen VM'ye ve bağlı olduğu Azure alt ağ için gelen bağlantılara izin verin.
   [Bir ortak IP adresi eklemek](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) VM için. Kontrol edebilirsiniz **önyükleme tanılama** ekran VM görüntüsünü görüntülemek için.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [VMware Vm'leri için olağanüstü durum kurtarma Azure ayarlama](tutorial-vmware-to-azure.md)
