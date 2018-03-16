---
title: "Hyper-V sanal makineleri olağanüstü durum kurtarma Azure için şirket içi Hyper-V server hazırlama | Microsoft Docs"
description: "Şirket içi Hyper-V Vm'lerini azure'a olağanüstü durum kurtarma Azure Site Recovery hizmeti ile System Center VMM tarafından yönetilmeyen hazırlama hakkında bilgi edinin."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 03/15/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 1290a186ca8e83b09f53b286e80c5ce75f08d88c
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="prepare-on-premises-hyper-v-servers-for-disaster-recovery-to-azure"></a>Olağanüstü durum kurtarma Azure için şirket içi Hyper-V sunucuları hazırlama

Bu öğretici, Hyper-V sanal makineleri, olağanüstü durum kurtarma amacıyla azure'a istediğinizde, şirket içi Hyper-V altyapınızı hazırlama gösterilmiştir. Hyper-V konakları System Center Virtual Machine Manager (VMM) tarafından yönetilebilir, ancak gerekli değildir.  Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Hyper-V gereksinimleri ve varsa VMM gereksinimleri gözden geçirin.
> * VMM hazırlamanız
> * Internet erişimi Azure konumlara doğrulayın
> * Böylece Azure için yük devretme sonrasında erişebilir VM'ler hazırlama

Bu, serideki ikinci öğreticidir. Önceki öğreticide açıklandığı gibi [Azure bileşenlerini ayarladığınızdan](tutorial-prepare-azure.md) emin olun.



## <a name="review-requirements-and-prerequisites"></a>Gözden geçirme gereksinimleri ve Önkoşullar

Emin Hyper-V konakları ve VM'ler gereksinimleriyle uyumlu hale getirir.

1. [Doğrulama](hyper-v-azure-support-matrix.md#on-premises-servers) şirket içi sunucu gereksinimleri.
2. [Gereksinimleri kontrol](hyper-v-azure-support-matrix.md#replicated-vms) Hyper-V sanal makinelerini Azure'a çoğaltmak istediğiniz için.
3. Hyper-V konağı denetleyin [ağ](hyper-v-azure-support-matrix.md#hyper-v-network-configuration); ve ana hem de Konuk [depolama](hyper-v-azure-support-matrix.md#hyper-v-host-storage) şirket içi Hyper-V konakları için destek.
4. İçin desteklenen denetleyin [Azure ağ](hyper-v-azure-support-matrix.md#azure-vm-network-configuration-after-failover), [depolama](hyper-v-azure-support-matrix.md#azure-storage), ve [işlem](hyper-v-azure-support-matrix.md#azure-compute-features), yük devretme sonrasında.
5. Şirket içi Vm'leriniz Azure'a çoğaltmak uymalıdır [Azure VM gereksinimleri](hyper-v-azure-support-matrix.md#azure-vm-requirements).


## <a name="prepare-vmm-optional"></a>VMM (isteğe bağlı) hazırlama

Hyper-V konakları VMM tarafından yönetiliyorsa, şirket içi VMM sunucusu hazırlamanız gerekir. 

- VMM sunucusu bir veya daha fazla konak grubu olan en az bir buluta sahip olduğundan emin olun. Sanal makineleri çalıştırdığınız Hyper-V konağı bulutta bulunmalıdır.
- VMM sunucusu, ağ eşlemesi için hazırlayın.

### <a name="prepare-vmm-for-network-mapping"></a>VMM ağ eşlemesi için hazırlanma

VMM, kullanıyorsanız [ağ eşlemesi](site-recovery-network-mapping.md) şirket içi VMM VM ağları ve Azure sanal ağlar arasındaki eşlemeleri. Eşleme, yük devretme sonrasında oluşturulduğunda Azure VM'ler için doğru ağ bağlanmasını sağlar.

VMM, ağ eşlemesi için aşağıdaki gibi hazırlayın:

1. Olduğundan emin olun bir [VMM mantıksal ağı](https://docs.microsoft.com/system-center/vmm/network-logical) , Hyper-V konaklarının bulunduğu bulutla ilişkili.
2. Olduğundan emin olun bir [VM ağı](https://docs.microsoft.com/system-center/vmm/network-virtual) mantıksal ağa bağlı.
3. VMM, sanal makinelerin VM ağına bağlayın.

## <a name="verify-internet-access"></a>Internet erişimi doğrulayın

1. Öğreticinin amaçları doğrultusunda basit Hyper-V konakları ve VMM sunucusu için uygunsa, bir proxy kullanmadan doğrudan internet erişimi sağlamak için yapılandırmadır. 
2. Bu Hyper-V konakları ve VMM sunucusu varsa, bu URL'ler erişebildiğinden emin olun: 

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
3. Emin olun:
    - Herhangi bir IP adresi tabanlı güvenlik duvarı kuralı Azure ile iletişim kurmaya izin vermelidir.
    - [Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)'na ve HTTPS (443) bağlantı noktasına izin verin.
    - IP adres aralıklarını aboneliğinizin Azure bölgesi ve Batı ABD (erişim denetimi ve kimlik yönetimi için kullanılan) izin verir.


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Bir yük devretme senaryosu sırasında çoğaltılmış şirket içi ağınıza bağlamak isteyebilirsiniz.

Windows Yük devretme işleminden sonra RDP kullanarak VM'ler bağlanmak için aşağıdaki gibi erişime izin ver:

1. İnternet üzerinden erişmek için, yük devretmeden önce şirket içi VM’de RDP’yi etkinleştirin. TCP ve UDP kurallarının **Ortak** profil için eklendiğinden ve tüm profillerde **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalar** içinde RDP’ye izin verildiğinden emin olun.
2. Siteden siteye VPN üzerinden erişmek için, şirket içi makinede RDP’yi etkinleştirin. **Etki Alanı ve Özel** ağlar için **Windows Güvenlik Duvarı** -> **İzin verilen uygulama ve özellikler içinde** RDP’ye izin verilmelidir.
   İşletim sisteminin SAN ilkesinin **OnlineAll** olarak ayarlandığından emin olun. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135). Bir yük devretme tetiklediğinizde VM’de bekleyen Windows güncelleştirmelerinin olmaması gerekir. Varsa, güncelleştirme tamamlanana kadar sanal makinede oturum açmanız mümkün olmayacaktır.
3. Yük devretmeden sonra Windows Azure VM’sinde, VM’nin bir ekran görüntüsünü görmek için **Önyükleme tanılaması**’nı kontrol edin. Bağlanamıyorsanız, VM’nin çalıştığından emin olun ve şu [sorun giderme ipuçlarını](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) gözden geçirin.

Yük devretme sonrasında Azure Vm'lerinin çoğaltılmış şirket içi VM veya farklı bir IP adresi olarak aynı IP adresini kullanarak erişebilirsiniz. [Daha fazla bilgi edinin](concepts-on-premises-to-azure-networking.md) yük devretme için IP adresleme yedeklemek için ayarlama hakkında.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hyper-V VM'ler için olağanüstü durum kurtarma Azure ayarlama](tutorial-hyper-v-to-azure.md)
> [Azure olağanüstü durum kurtarma için Hyper-V sanal makineleri VMM bulutlarındaki ayarlama](tutorial-hyper-v-vmm-to-azure.md)
