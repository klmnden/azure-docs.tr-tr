---
title: Şirket içi VMware sunucularını VMware VM’lerinden Azure’a olağanüstü durum kurtarmaya hazırlama| Microsoft Docs
description: Azure Site Recovery hizmetini kullanarak şirket içi VMware sunucularını Azure’a olağanüstü durum kurtarmaya hazırlamayı öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 739f1a9a3a75123c0273dc958b4ba1fd7231f3c3
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59268628"
---
# <a name="prepare-on-premises-vmware-servers-for-disaster-recovery-to-azure"></a>Şirket içi VMware sunucularını Azure’a olağanüstü durum kurtarmaya hazırlama

[Azure Site Recovery](site-recovery-overview.md), planlı ve plansız kesintiler sırasında iş uygulamalarınızı çalışır durumda tutarak, iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma işlemlerini yönetir ve düzenler.

- Bu, şirket içi VMware sanal makineleri için Azure’da olağanüstü durum kurtarmanın nasıl ayarlanacağını gösteren serideki ikinci öğreticidir. Birinci öğreticide, VMware olağanüstü durum kurtarma için gerekli [Azure bileşenlerini ayarladık](tutorial-prepare-azure.md).


> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu size göstermek için tasarlanmıştır. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için ilgili senaryonun **Nasıl Yapılır** bölümüne başvurun.

Bu makalede, Azure Site Recovery kullanarak VMware VM'lerini Azure'a çoğaltmak istediğinizde şirket içi VMware ortamınızı nasıl hazırlayacağınızı gösteriyoruz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * VM bulmayı otomatikleştirmek için vCenter sunucusunda veya vSphere ESXi ana bilgisayarında bir hesap hazırlama
> * VMware VM’lerinde Mobility hizmetini otomatik olarak yüklemek için bir hesap hazırlama
> * VMware sunucu ve VM gereksinimlerini gözden geçirme
> * Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma



## <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- VM'leri otomatik olarak bulma. En az bir salt okunur hesap gereklidir.
- Çoğaltma, yük devretme ve yeniden çalıştırmayı yönetme. Diskleri oluşturma ve kaldırma ve VM’leri çalıştırma gibi işlemleri gerçekleştirebilen bir hesabınızın olması gerekir.

Hesabı aşağıdaki gibi oluşturun:

1. Ayrılmış bir hesap kullanmak için, rolü vCenter düzeyinde oluşturun. Role **Azure_Site_Recovery** gibi bir ad verin.
2. Role aşağıdaki tabloda özetlenen izinleri atayın.
3. vCenter sunucusu veya vSphere ana bilgisayarında bir kullanıcı oluşturun. Rolü kullanıcıya atayın.

### <a name="vmware-account-permissions"></a>VMware hesap izinleri

**Görev** | **Rol/izinleri** | **Ayrıntılar**
--- | --- | ---
**VM bulma** | En az bir salt okunur kullanıcı<br/><br/> Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için, **Alt öğeye yay** nesnesi ile **Erişim yok** rolünü alt nesnelere (vSphere ana bilgisayarları, veri depoları, VM’ler ve ağlar) atayın.
**Tam çoğaltma, yük devretme, yeniden çalışma** |  Gerekli izinlere sahip bir rol (Azure_Site_Recovery) oluşturup rolü VMware kullanıcısı veya grubuna atayın<br/><br/> Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Azure_Site_Recovery<br/><br/> Veri deposu -> Alan ayırma, veri deposuna göz atma, düşük düzeyli dosya işlemleri, dosyayı kaldırma, sanal makine dosyalarını güncelleştirme<br/><br/> Ağ -> Ağ ataması<br/><br/> Kaynak -> VM’yi kaynak havuzuna atama, kapalı VM’yi geçirme, açık VM’yi geçirme<br/><br/> Görevler -> Görev oluşturma, görevi güncelleştirme<br/><br/> Sanal makine -> Yapılandırma<br/><br/> Sanal makine -> Etkileşim -> soruyu yanıtlama, cihaz bağlantısı, CD ortamını yapılandırma, disket ortamını yapılandırma, kapatma, açma, VMware araçlarını yükleme<br/><br/> Sanal makine -> Envanter -> Oluşturma, kaydetme, kaydı kaldırma<br/><br/> Sanal makine -> Sağlama -> Sanal makine indirmeye izin verme, Sanal makine dosyalarını karşıya yüklemeye izin verme<br/><br/> Sanal makine -> Anlık görüntüler -> Anlık görüntüleri kaldırma | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için, **Alt öğeye yay** nesnesi ile **Erişim yok** rolünü alt nesnelere (vSphere ana bilgisayarları, veri depoları, VM’ler ve ağlar) atayın.

## <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Çoğaltmak istediğiniz makinelerde Mobility hizmeti yüklü olmalıdır. Makine için çoğaltmayı etkinleştirdiğinizde Site Recovery bu hizmeti göndererek yükleme işlemi yapabileceği gibi, bunu el ile yükleyebilir veya yükleme araçlarını kullanabilirsiniz.

- Bu öğreticide, Mobility hizmetini göndererek yükleme yoluyla yükleyeceğiz.
- Bu göndererek yükleme için, Site Recovery’nin sanal makineye erişmek için kullanabileceği bir hesap hazırlamanız gerekir. Azure konsolunda olağanüstü durum kurtarmayı ayarlarken bu hesabı belirtirsiniz.

Hesabı aşağıdaki gibi hazırlayın:

VM üzerinde yükleme izinleri ile bir etki alanı veya yerel hesap hazırlayın.

- **Windows Vm'leri**: Bir etki alanı hesabı kullanmıyorsanız Windows Vm'lerinde yüklemek için yerel makinede uzak kullanıcı erişim denetimini devre dışı bırakın. Bunu yapmak için kayıt defterinde > **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** altında **LocalAccountTokenFilterPolicy** adlı DWORD girişini 1 değeriyle ekleyin.
- **Linux Vm'leri**: Linux VM üzerinde yüklemek için, kaynak Linux sunucusunda bir kök hesabı hazırlayın.


## <a name="check-vmware-requirements"></a>VMware gereksinimlerini denetleme

VMware sunucularının ve sanal makinelerin gereksinimlerle uyumlu olduğundan emin olun.

1. VMware sunucusu gereksinimlerini [doğrulayın](vmware-physical-azure-support-matrix.md#on-premises-virtualization-servers).
2. Linux VM'leri için dosya sistemini ve depolama gereksinimlerini [denetleyin](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage). 
3. Şirket içi [ağ](vmware-physical-azure-support-matrix.md#network) ve [depolama](vmware-physical-azure-support-matrix.md#storage) desteğini denetleyin. 
4. Yük devretmenin ardından [Azure ağ](vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [depolama](vmware-physical-azure-support-matrix.md#azure-storage) ve [işlem](vmware-physical-azure-support-matrix.md#azure-compute) için nelerin desteklendiğini denetleyin.
5. Azure’a çoğalttığınız şirket içi sanal makineleriniz, [Azure sanal makinesi gereksinimleri](vmware-physical-azure-support-matrix.md#azure-vm-requirements) ile uyumlu olmalıdır.
6. Linux sanal makinelerinin, cihaz adı veya bağlama noktası adı benzersiz olmalıdır. Hiçbir iki cihazları/bağlama noktaları aynı ada sahip olduğundan emin olun. Bu ad büyük küçük harfe duyarlı olmayan unutmayın. Örneğin, aynı VM için iki cihazı adlandırma _cihaz1_ ve _cihaz1_ izin verilmiyor.


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Yük devretmeden sonra, şirket içi ağınızdan Azure VM'lerine bağlanmak isteyebilirsiniz.

Yük devretmeden sonra RDP kullanarak Windows VM’lerine bağlanmak için aşağıdakileri yapın:

- **İnternet erişimi**. Yük devretmeden önce, yük devretmeden önce şirket içi VM’de RDP’yi etkinleştirin. TCP ve UDP kurallarının **Ortak** profil için eklendiğinden ve tüm profillerde **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalar** içinde RDP’ye izin verildiğinden emin olun.
- **Konumdan konuma VPN erişimi**:
    - Yük devretmeden önce, şirket içi makinede RDP’yi etkinleştirin.
    - **Etki Alanı ve Özel** ağlar için **Windows Güvenlik Duvarı** -> **İzin verilen uygulama ve özellikler içinde** RDP’ye izin verilmelidir.
    - İşletim sisteminin SAN ilkesinin **OnlineAll** olarak ayarlandığından emin olun. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135).
- Bir yük devretme tetiklediğinizde VM’de bekleyen Windows güncelleştirmelerinin olmaması gerekir. Varsa, güncelleştirme tamamlanana kadar sanal makinede oturum açmanız mümkün olmayacaktır.
- Yük devretmeden sonra Windows Azure VM’sinde, VM’nin bir ekran görüntüsünü görmek için **Önyükleme tanılaması**’nı kontrol edin. Bağlanamıyorsanız, VM’nin çalıştığından emin olun ve şu [sorun giderme ipuçlarını](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) gözden geçirin.

Yük devretmeden sonra SSH kullanarak Linux VM’lerine bağlanmak için aşağıdakileri yapın:

- Yük devretmeden önce şirket içi makinede Secure Shell hizmetinin sistem önyüklemesinde otomatik olarak başlatılmak için ayarlandığından emin olun.
- Güvenlik duvarı kurallarının SSH bağlantısına izin verdiğinden emin olun.
- Yük devretmeden sonra Azure VM’sinde, yük devredilen VM üzrindeki ağ güvenlik grubu kuralları ve VM’nin bağlı olduğu Azure alt ağı için SSH bağlantı noktasına gelen bağlantılara izin verin.
- VM için bir [ortak IP adresi ekleyin](site-recovery-monitoring-and-troubleshooting.md).
- VM’nin bir ekran görüntüsünü görmek için **Önyükleme tanılaması**’nı kontrol edebilirsiniz.


## <a name="failback-requirements"></a>Yeniden çalışma gereksinimleri
Yeniden şirket içi için başarısız planlıyorsanız, ayrıca emin olmak ihtiyacınız olan belirli [önkoşulların karşılandığından](vmware-azure-reprotect.md##before-you-begin). Ancak bu önkoşullar VM’lerinizde **olağanüstü durum kurtarmayı etkinleştirmeye başlamak için gerekli değildir** ve Azure’a yük devretme sonrasında da yapılabilir.

## <a name="useful-links"></a>Yararlı bağlantılar

Birden çok VM'yi çoğaltıyorsanız, başlamadan önce bir kapasite ve dağıtım planlamanız gerekir. [Daha fazla bilgi edinin](site-recovery-deployment-planner.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [VMware Vm'leri için Azure'da olağanüstü durum kurtarmayı ayarlama](vmware-azure-tutorial.md)
