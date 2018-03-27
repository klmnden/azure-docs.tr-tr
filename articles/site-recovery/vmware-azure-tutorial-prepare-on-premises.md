---
title: Şirket içi VMware sunucularını VMware VM’lerinden Azure’a olağanüstü durum kurtarmaya hazırlama| Microsoft Docs
description: Azure Site Recovery hizmetini kullanarak şirket içi VMware sunucularını Azure’a olağanüstü durum kurtarmaya hazırlamayı öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/15/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 6898f725d1d3cbf3f8d9d90faeafc13fbc8cb201
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="prepare-on-premises-vmware-servers-for-disaster-recovery-to-azure"></a>Şirket içi VMware sunucularını Azure’a olağanüstü durum kurtarmaya hazırlama

Bu öğreticide VMware VM’lerini Azure’a çoğaltırken şirket içi VMware altyapınızı nasıl hazırlayacağınız gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * VM bulmayı otomatikleştirmek için vCenter sunucusunda veya vSphere ESXi ana bilgisayarında bir hesap hazırlama
> * VMware VM’lerinde Mobility hizmetini otomatik olarak yüklemek için bir hesap hazırlama
> * VMware sunucu gereksinimlerini gözden geçirme
> * VMware VM gereksinimlerini gözden geçirme

Bu öğretici serisinde, Azure Site Recovery kullanarak tek bir VM’yi yedekleme gösterilmektedir. Birden çok VMware VM’sini korumayı planlıyorsanız, VMware çoğaltması için [Dağıtım Planlayıcısı aracını](https://aka.ms/asr-deployment-planner) indirmelisiniz. Bu araç VM uyumluluğu, VM başına disk sayısı ve disk başına veri değişim sıklığı hakkında bilgiler toplar. Araç ayrıca ağ bant genişliği gereksinimlerini ve başarılı çoğaltma ve test yük devretmesi için gereken Azure altyapısını da kapsar. Aracı çalıştırma hakkında [daha fazla bilgi edinin](site-recovery-deployment-planner.md).

Bu, serideki ikinci öğreticidir. Önceki öğreticide açıklandığı gibi [Azure bileşenlerini ayarladığınızdan](tutorial-prepare-azure.md) emin olun.

## <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- VM’leri otomatik olarak bulma. En az bir salt okunur hesap gereklidir.
- Çoğaltma, yük devretme ve yeniden çalıştırmayı yönetme. Diskleri oluşturma ve kaldırma ve VM’leri çalıştırma gibi işlemleri gerçekleştirebilen bir hesabınızın olması gerekir.

Hesabı aşağıdaki gibi oluşturun:

1. Ayrılmış bir hesap kullanmak için, rolü vCenter düzeyinde oluşturun. Role **Azure_Site_Recovery** gibi bir ad verin.
2. Role aşağıdaki tabloda özetlenen izinleri atayın.
3. vCenter sunucusu veya vSphere ana bilgisayarında bir kullanıcı oluşturun. Rolü kullanıcıya atayın.

### <a name="vmware-account-permissions"></a>VMware hesap izinleri

**Görev** | **Rol/İzinler** | **Ayrıntılar**
--- | --- | ---
**VM bulma** | En az bir salt okunur kullanıcı<br/><br/> Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için, **Alt öğeye yay** nesnesi ile **Erişim yok** rolünü alt nesnelere (vSphere ana bilgisayarları, veri depoları, VM’ler ve ağlar) atayın.
**Tam çoğaltma, yük devretme, yeniden çalışma** |  Gerekli izinlere sahip bir rol (Azure_Site_Recovery) oluşturup rolü VMware kullanıcısı veya grubuna atayın<br/><br/> Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Azure_Site_Recovery<br/><br/> Veri deposu -> Alan ayırma, veri deposuna göz atma, düşük düzeyli dosya işlemleri, dosyayı kaldırma, sanal makine dosyalarını güncelleştirme<br/><br/> Ağ -> Ağ ataması<br/><br/> Kaynak -> VM’yi kaynak havuzuna atama, kapalı VM’yi geçirme, açık VM’yi geçirme<br/><br/> Görevler -> Görev oluşturma, görevi güncelleştirme<br/><br/> Sanal makine -> Yapılandırma<br/><br/> Sanal makine -> Etkileşim -> soruyu yanıtlama, cihaz bağlantısı, CD ortamını yapılandırma, disket ortamını yapılandırma, kapatma, açma, VMware araçlarını yükleme<br/><br/> Sanal makine -> Envanter -> Oluşturma, kaydetme, kaydı kaldırma<br/><br/> Sanal makine -> Sağlama -> Sanal makine indirmeye izin verme, Sanal makine dosyalarını karşıya yüklemeye izin verme<br/><br/> Sanal makine -> Anlık görüntüler -> Anlık görüntüleri kaldırma | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için, **Alt öğeye yay** nesnesi ile **Erişim yok** rolünü alt nesnelere (vSphere ana bilgisayarları, veri depoları, VM’ler ve ağlar) atayın.

## <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Çoğaltmak istediğiniz sanal makinede Mobility hizmeti yüklü olmalıdır. Sanal makine için çoğaltmayı etkinleştirdiğinizde Site Recovery bu hizmeti otomatik olarak yükler. Otomatik yükleme için, Site Recovery’nin sanal makineye erişmek için kullanacağı bir hesap hazırlamanız gerekir. Azure konsolunda olağanüstü durum kurtarmayı ayarlarken bu hesabı belirteceksiniz.

1. VM üzerinde yükleme izinleri ile bir etki alanı veya yerel hesap hazırlayın.
2. Windows sanal makinelerine yüklemek için, bir etki alanı hesabı kullanmıyorsanız yerel makinede Uzak Kullanıcı Erişim denetimini devre dışı bırakın.
   - Kayıt defterinde **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** altında **LocalAccountTokenFilterPolicy** adlı DWORD girişini 1 değeriyle ekleyin.
3. Linux VM üzerinde yüklemek için, kaynak Linux sunucusunda bir kök hesabı hazırlayın.


## <a name="check-vmware-requirements"></a>VMware gereksinimlerini denetleme

VMware sunucularının ve sanal makinelerin gereksinimlerle uyumlu olduğundan emin olun.

1. VMware sunucusu gereksinimlerini [doğrulayın](vmware-physical-azure-support-matrix.md#on-premises-virtualization-servers).
2. Linux için dosya sistemini ve depolama gereksinimlerini [denetleyin](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage). 
3. Şirket içi [ağ](vmware-physical-azure-support-matrix.md#network) ve [depolama](vmware-physical-azure-support-matrix.md#storage) desteğini denetleyin. 
4. Yük devretmenin ardından [Azure ağ](vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [depolama](vmware-physical-azure-support-matrix.md#azure-storage) ve [işlem](vmware-physical-azure-support-matrix.md#azure-compute) için nelerin desteklendiğini denetleyin.
5. Azure’a çoğalttığınız şirket içi sanal makineleriniz, [Azure sanal makinesi gereksinimleri](vmware-physical-azure-support-matrix.md#azure-vm-requirements) ile uyumlu olmalıdır.


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Bir yük devretme senaryosunda, Azure’daki çoğaltılan VM’lerinize şirket içi ağınızdan bağlanmak isteyebilirsiniz.

Yük devretmeden sonra RDP kullanarak Windows VM’lerine bağlanmak için aşağıdakileri yapın:

1. İnternet üzerinden erişmek için, yük devretmeden önce şirket içi VM’de RDP’yi etkinleştirin. TCP ve UDP kurallarının **Ortak** profil için eklendiğinden ve tüm profillerde **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalar** içinde RDP’ye izin verildiğinden emin olun.
2. Siteden siteye VPN üzerinden erişmek için, şirket içi makinede RDP’yi etkinleştirin. **Etki Alanı ve Özel** ağlar için **Windows Güvenlik Duvarı** -> **İzin verilen uygulama ve özellikler içinde** RDP’ye izin verilmelidir.
   İşletim sisteminin SAN ilkesinin **OnlineAll** olarak ayarlandığından emin olun. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135). Bir yük devretme tetiklediğinizde VM’de bekleyen Windows güncelleştirmelerinin olmaması gerekir. Varsa, güncelleştirme tamamlanana kadar sanal makinede oturum açmanız mümkün olmayacaktır.
3. Yük devretmeden sonra Windows Azure VM’sinde, VM’nin bir ekran görüntüsünü görmek için **Önyükleme tanılaması**’nı kontrol edin. Bağlanamıyorsanız, VM’nin çalıştığından emin olun ve şu [sorun giderme ipuçlarını](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) gözden geçirin.

Yük devretmeden sonra SSH kullanarak Linux VM’lerine bağlanmak için aşağıdakileri yapın:

1. Yük devretmeden önce şirket içi makinede Secure Shell hizmetinin sistem önyüklemesinde otomatik olarak başlatılmak için ayarlandığından emin olun. Güvenlik duvarı kurallarının SSH bağlantısına izin verdiğinden emin olun.

2. Yük devretmeden sonra Azure VM’sinde, yük devredilen VM üzrindeki ağ güvenlik grubu kuralları ve VM’nin bağlı olduğu Azure alt ağı için SSH bağlantı noktasına gelen bağlantılara izin verin.
   VM için bir [ortak IP adresi ekleyin](site-recovery-monitoring-and-troubleshooting.md). VM’nin bir ekran görüntüsünü görmek için **Önyükleme tanılaması**’nı kontrol edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [VMware VM’leri için Azure’da olağanüstü durum kurtarmayı ayarlama](vmware-azure-tutorial.md)
