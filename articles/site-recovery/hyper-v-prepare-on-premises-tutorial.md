---
title: Şirket içi Hyper-V sunucularını Hyper-V vm'lerinin azure'a olağanüstü durum kurtarmaya hazırlama | Microsoft Docs
description: Şirket içi Hyper-V Vm'lerini azure'a Azure Site Recovery hizmeti ile olağanüstü durum kurtarma için hazırlamayı öğrenin.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 6e57b629a0007b06af6e37f96e1466e35afafccc
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59361895"
---
# <a name="prepare-on-premises-hyper-v-servers-for-disaster-recovery-to-azure"></a>Şirket içi Hyper-V sunucularını azure'a olağanüstü durum kurtarmaya hazırlama

Bu makalede, şirket içi Hyper-V altyapınızı azure'a, Hyper-Vm'leri olağanüstü durumdan kurtarma kurmak istediğinizde hazırlama kullanarak [Azure Site Recovery](site-recovery-overview.md).


Bu, şirket içi Hyper-V Vm'leri için Azure'da olağanüstü durum kurtarma ayarlama gösteren serideki ikinci öğreticidir. İlk öğreticide, biz [Azure bileşenlerini ayarlarsınız](tutorial-prepare-azure.md) Hyper-V olağanüstü durum kurtarma için gerekli.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Hyper-V konaklarınız System Center VMM tarafından yönetiliyorsa, Hyper-V gereksinimleri ve VMM gereksinimleri gözden geçirin.
> * VMM, varsa hazırlayın.
> * Azure konumları internet erişimini doğrulayın.
> * Sanal makineleri, azure'a yük devredildikten sonra erişebilmeleri hazırlayın.

> [!NOTE]
> Öğreticiler bir senaryo için en basit dağıtım yolu gösterir. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için Site Recovery İçindekiler bölümünde nasıl yapılır makalesine gözden geçirin.

## <a name="before-you-start"></a>Başlamadan önce

Emin hazırladığınız Azure açıklandığı [bu serideki ilk öğreticide](tutorial-prepare-azure.md).

## <a name="review-requirements-and-prerequisites"></a>Gözden geçirme gereksinimleri ve Önkoşullar

Hyper-V konakları olduğundan emin olun ve sanal makinelerin gereksinimlerle uyumlu.

1. [Doğrulama](hyper-v-azure-support-matrix.md#on-premises-servers) şirket içi sunucu gereksinimleri.
2. [Gereksinimleri kontrol](hyper-v-azure-support-matrix.md#replicated-vms) Hyper-V Vm'lerini Azure'a çoğaltmak istediğiniz için.
3. Hyper-V konağı denetleyin [ağ](hyper-v-azure-support-matrix.md#hyper-v-network-configuration); ve konak ve Konuk [depolama](hyper-v-azure-support-matrix.md#hyper-v-host-storage) şirket içi Hyper-V konakları için destek.
4. Yük devretmenin ardından [Azure ağ](hyper-v-azure-support-matrix.md#azure-vm-network-configuration-after-failover), [depolama](hyper-v-azure-support-matrix.md#azure-storage) ve [işlem](hyper-v-azure-support-matrix.md#azure-compute-features) için nelerin desteklendiğini denetleyin.
5. Azure’a çoğalttığınız şirket içi sanal makineleriniz, [Azure sanal makinesi gereksinimleri](hyper-v-azure-support-matrix.md#azure-vm-requirements) ile uyumlu olmalıdır.


## <a name="prepare-vmm-optional"></a>(İsteğe bağlı) VMM hazırlama

Hyper-V konakları VMM tarafından yönetiliyorsa, şirket içi VMM sunucusunu hazırlamanız gerekir. 

- VMM sunucusu bir veya daha fazla konak grubu ile en az bir bulut olduğundan emin olun. VM'ler üzerinde çalışan Hyper-V konağı bulutta bulunmalıdır.
- VMM sunucusu için ağ eşlemesini hazırlama.

### <a name="prepare-vmm-for-network-mapping"></a>VMM ağ eşlemeye hazırlama

VMM, kullanıyorsanız, [ağ eşlemesini](site-recovery-network-mapping.md) şirket içi VMM VM ağları ve Azure sanal ağları arasında eşlemeleri. Yük devretme sonrasında oluşturulan Azure VM'ler doğru ağa bağlı eşleme sağlar.

VMM, aşağıdaki gibi ağ eşlemesi için hazırlık:

1. Olduğundan emin olun bir [VMM mantıksal ağı](https://docs.microsoft.com/system-center/vmm/network-logical) , Hyper-V ana bilgisayarlarının bulunduğu bulutla ilişkilendirdiğiniz.
2. Olduğundan emin olun bir [VM ağı](https://docs.microsoft.com/system-center/vmm/network-virtual) mantıksal ağa bağlı.
3. VMM, sanal makinelerin VM ağına bağlayın.

## <a name="verify-internet-access"></a>İnternet erişimi doğrulayın

1. Öğreticinin amacı doğrultusunda, Hyper-V konaklarını ve VMM sunucusunu kullanarak bir ara sunucu olmadan doğrudan internet erişimini sağlamak en basit yapılandırmadır içindir. 
2. Bu Hyper-V konaklarını ve VMM sunucusu varsa, aşağıdaki gerekli URL'lere erişebildiğinden emin olun.   
3. IP adresine göre erişimi denetleme, emin olun:
    - IP adresi tabanlı güvenlik duvarı kuralları bağlanabilir [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)ve HTTPS (443) bağlantı noktası.
    - Aboneliğinizin Azure bölgesi için IP adresi aralıklarına izin verin.
    
### <a name="required-urls"></a>Gerekli URL


[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Bir yük devretme senaryosunda, çoğaltılan şirket içi ağınıza bağlanmak isteyebilirsiniz.

Yük devretmeden sonra RDP kullanarak Windows Vm'lerine bağlanmak için şu şekilde erişime izin ver:

1. İnternet üzerinden erişmek için, yük devretmeden önce şirket içi VM’de RDP’yi etkinleştirin. TCP ve UDP kurallarının **Ortak** profil için eklendiğinden ve tüm profillerde **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalar** içinde RDP’ye izin verildiğinden emin olun.
2. Siteden siteye VPN üzerinden erişmek için, şirket içi makinede RDP’yi etkinleştirin. **Etki Alanı ve Özel** ağlar için **Windows Güvenlik Duvarı** -> **İzin verilen uygulama ve özellikler içinde** RDP’ye izin verilmelidir.
   İşletim sisteminin SAN ilkesinin **OnlineAll** olarak ayarlandığından emin olun. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135). Bir yük devretme tetiklediğinizde VM’de bekleyen Windows güncelleştirmelerinin olmaması gerekir. Varsa, güncelleştirme tamamlanana kadar sanal makinede oturum açmanız mümkün olmayacaktır.
3. Yük devretmeden sonra Windows Azure VM’sinde, VM’nin bir ekran görüntüsünü görmek için **Önyükleme tanılaması**’nı kontrol edin. Bağlanamıyorsanız, VM’nin çalıştığından emin olun ve şu [sorun giderme ipuçlarını](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) gözden geçirin.

Yük devretme işleminden sonra çoğaltılan şirket içi VM veya farklı bir IP adresi olarak aynı IP adresini kullanarak Azure Vm'lerine erişebilirsiniz. [Daha fazla bilgi edinin](concepts-on-premises-to-azure-networking.md) yük devretme için IP adresini ayarlama hakkında daha fazla.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hyper-V Vm'leri için Azure'da olağanüstü durum kurtarma ayarlama](tutorial-hyper-v-to-azure.md)
> [azure'a olağanüstü durum kurtarma Hyper-V Vm'leri için VMM bulutlarında ayarlama](tutorial-hyper-v-vmm-to-azure.md)
