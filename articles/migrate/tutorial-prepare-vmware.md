---
title: VMware Vm'leri, değerlendirme ve Azure geçişi ile azure'a geçiş için hazırlama | Microsoft Docs
description: Değerlendirme ve şirket içi VMware vm'lerinin Azure Geçişi'ni kullanarak azure'a geçiş için hazırlanma işlemi açıklanmaktadır.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 10f559295ff0598dea26fb30b089f020e2985889
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840335"
---
# <a name="prepare-vmware-vms-for-assessment-and-migration-to-azure"></a>VMware Vm'lerini değerlendirme ve azure'a geçiş için hazırlama

Bu makalede, değerlendirme ve şirket içi VMware vm'lerinin azure'a geçiş için hazırlamak üzere açıklanır kullanarak [Azure geçişi](migrate-services-overview.md).

[Azure geçişi](migrate-overview.md) hub'ı keşfedin, değerlendirin ve uygulamalar ve altyapı iş yükleri Microsoft Azure'a geçirmek için yardımcı araçlar sağlar. Hub, Azure geçişi araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri içerir. 


Bu öğretici, değerlendirmek ve VMware Vm'leri geçirme işlemini gösterir bir serinin ilk bölümüdür. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure'ı hazırlama. Azure geçişi ile çalışmak için Azure hesabınızı ve kaynaklarınızı için izinleri ayarlayın.
> * Şirket içi VMware sunucularını ve Vm'leri, sanal makine değerlendirmesi için hazırlayın.
> * Şirket içi VMware sunucularını ve Vm'leri, VM geçişi için hazırlayın.

> [!NOTE]
> Öğreticiler hızlı bir kavram kanıtı ayarlayabilirsiniz bir senaryo için en basit dağıtım yolu gösterir. Öğreticiler, mümkün olduğunda varsayılan seçenekleri kullanın ve tüm olası ayarları ve yol gösterme. Ayrıntılı yönergeler için "nasıl" soruları VMware değerlendirme ve geçiş için gözden geçirin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prepare-azure"></a>Azure’u hazırlama

Azure bu izinlere ihtiyacınız vardır:

- Azure hesabınızda bir değerlendirme ve geçiş için Azure geçişi projesi oluşturmak için izinler gerekiyor. 
- Azure geçişi, değerlendirme ve VMware vm'lerinin aracısız geçiş için Vm'leri bulur ve Azure geçişi için VM meta verileri ve performans verilerini gönderen basit bir gereç çalıştırır. Azure'da, Azure geçişi Gereci kaydetme izni gerekir.
- VMware Vm'lerini Azure geçişi sunucusu geçişi kullanan geçirmek için Azure geçişi bir Key Vault aboneliğinizdeki çoğaltma depolama hesabı erişim anahtarlarını yönetmek için kaynak grubu oluşturur. Kasa oluşturmak için Azure geçişi projesi içinde bulunduğu kaynak grubunun üzerinde rol atama izinleri gerekir. 


### <a name="assign-permissions-to-create-project"></a>Proje oluşturma izinleri atama

1. Azure portalında abonelik açın ve seçin **erişim denetimi (IAM)** .
2. İçinde **denetleyin erişim**, ilgili hesabını bulun ve izinleri görüntülemek için tıklayın.
3. Olmalıdır **katkıda bulunan** veya **sahibi** izinleri.
    - Ücretsiz Azure hesabı oluşturduysanız, aboneliğinizin sahibine demektir.
    - Abonelik sahibi değilseniz, rol atamak için sahibi ile çalışır.

### <a name="assign-permissions-to-register-the-appliance"></a>Gereç kaydetmek için izinler atama

Değerlendirme veya VM aracısız bir geçiş çalıştırmak için Azure geçişi Gereci dağıtıyorsanız, onu kaydetmeniz gerekir.

- Gereç kayıt sırasında Azure geçişi gereç tanıtan iki Azure Active Directory (Azure AD) uygulaması oluşturur
    - İlk uygulamanızı Azure geçişi Hizmeti uç noktaları ile iletişim kurar.
    - İkinci uygulamayı bir Azure kayıt sırasında oluşturulan Azure AD uygulama bilgileri ve gerecini yapılandırma ayarlarını depolamak için Key Vault erişir.
- Aşağıdaki yöntemlerden birini kullanarak bu Azure AD uygulamaları oluşturmak Azure geçişi için izinler atayabilirsiniz:
    - Kiracı/genel yönetici, oluşturmak ve Azure AD uygulamaları kaydetmek için kiracıdaki kullanıcılar izin verebilirsiniz.
    - Kiracı/genel yönetici hesabına (izinleri) Uygulama geliştirici rolünü atayabilirsiniz.

Bu, hatalarının ayıklanabileceğini belirtmekte yarar:

- Uygulamalar, abonelikte yukarıda açıklanan dışındaki herhangi bir erişim izinleri yok.
- Yeni bir gereç kaydettiğinizde yalnızca bu izinleri gerekir. Gereç ayarlandıktan sonra izinleri kaldırabilir. 


#### <a name="grant-account-permissions"></a>Hesabı izinleri verme

Kiracı/genel yönetici gibi izinleri verebilir

1. Kiracı/genel yönetici Azure AD'de gidin **Azure Active Directory** > **kullanıcılar** > **kullanıcı ayarları**.
2. Yönetici ayarlamalısınız **uygulama kayıtları** için **Evet**.

    ![Azure AD izinleri](./media/tutorial-prepare-vmware/aad.png)

> [!NOTE]
> Bu hassas olmayan varsayılan ayardır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).



#### <a name="assign-application-developer-role"></a>Uygulama geliştiricisi rolü atama 

Kiracı/genel yönetici hesabınız için uygulama geliştiricisinin rol atayabilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

## <a name="assign-role-assignment-permissions"></a>Rol atama izinleri atama

Azure geçişi projesi, şu şekilde bulunduğu kaynak grubu rol atama izinleri atayın:

1. Azure portalında kaynak grubunu seçtikten **erişim denetimi (IAM)** .
2. İçinde **denetleyin erişim**, ilgili hesabını bulun ve izinleri görüntülemek için tıklayın.

    - Server değerlendirmesi çalıştırılacak **katkıda bulunan** izinler yeterli değil.
    - Aracısız sunucusu geçişi çalıştırmak için olmalıdır **sahibi** (veya **katkıda bulunan** ve **kullanıcı erişimi Yöneticisi**) izinleri.

3. Gerekli izinlere sahip değilseniz, bunları kaynak grubu sahibinden isteyin. 



## <a name="prepare-for-vmware-vm-assessment"></a>VMware VM değerlendirmesi için hazırlama

VMware VM değerlendirmesi için hazırlamak için Hyper-V konak ve sanal makine ayarlarını doğrulayın ve gereç dağıtım için ayarları doğrulamak gerekir.

### <a name="verify-vmware-settings"></a>VMware ayarlarını doğrulayın

1. [Doğrulama](migrate-support-matrix-vmware.md#assessment-vmware-server-requirements) VMware VM değerlendirmesi için sunucu gereksinimleri.
2. [Emin](migrate-support-matrix-vmware.md#assessment-port-requirements) vCenter sunucularında gerekli bağlantı noktaları açıktır.


### <a name="set-up-an-account-for-assessment"></a>Değerlendirme için bir hesabı ayarlayın

Azure geçişi, vCenter Server değerlendirme ve aracısız geçiş için Vm'leri bulmak için erişim gerekir. Değerlendirme için yalnızca vCenter Server için salt okunur bir hesap gerekir.

URL tabanlı bir firewall.proxy kullanıyorsanız, gerekli erişmesine izin vermek [Azure URL'lerini](migrate-support-matrix-vmware.md#assessment-url-access-requirements).

Proxy URL'leri aranırken herhangi bir CNAME kayıt alınan çözümler emin olun.


### <a name="verify-appliance-settings-for-assessment"></a>Değerlendirme için gereç ayarlarını doğrulayın

Azure geçişi Gereci ve sonraki öğreticide başına değerlendirmesi ayarlamadan önce gereç dağıtımı için hazırlık yaparsınız.

1. Doğrulama [doğrulama](migrate-support-matrix-vmware.md#assessment-appliance-requirements) VMware Azure geçişi Gereci Kurma gereksinimleri.
2. [Gözden geçirme](migrate-support-matrix-vmware.md#assessment-url-access-requirements) gereç erişmesi gereken Azure URL'lerini.
3. Gereç Keşif ve değerlendirme sırasında topladığınız verilerin gözden geçirin.
4. [Not](migrate-support-matrix-vmware.md#assessment-port-requirements) bağlantı noktası, cihaz için erişim gereksinimleri.
5. OVA dosyasını kullanarak bir VMware VM Azure geçişi Gereci dağıttığınız. VCenter Server'da, hesabınızın OVA dosyasını kullanarak bir VM oluşturmak için izinlere sahip olduğundan emin olun.


## <a name="prepare-for-agentless-vmware-migration"></a>Aracısız VMware geçiş için hazırlama

Aracısız geçiş VMware VM gereksinimlerini gözden geçirin.

1. [Gözden geçirme](migrate-support-matrix-vmware.md#agentless-migration-vmware-server-requirements) aracısız geçiş için VMware sunucu gereksinimlerini.
2. VCenter Server'a erişmek için bir hesap ayarlama ile [gerekli izinler](migrate-support-matrix-vmware.md#agentless-migration-vcenter-server-permissions) aracısız geçiş.
3. [Not](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements) aracısız geçiş kullanarak Azure'a geçirmek istediğiniz VMware Vm'leri için gereksinimleri.
4. [Gözden geçirme](migrate-support-matrix-vmware.md#agentless-migration-appliance-requirements) aracısız geçiş Gereci gereksinimleri.]
5. Not Gereci [URL erişimi](migrate-support-matrix-vmware.md#agentless-migration-url-access-requirements) ve [bağlantı noktası erişim](migrate-support-matrix-vmware.md#agentless-migration-port-requirements) aracısız Geçiş gereksinimleri.


## <a name="prepare-for-agent-based-vmware-migration"></a>Aracı tabanlı VMware geçiş için hazırlama

Aracı tabanlı geçiş VMware VM gereksinimlerini gözden geçirin.

1. [Gözden geçirme](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements) aracısız geçiş için VMware sunucu gereksinimlerini. 
2. VCenter Server'a erişmek için bir hesap ayarlama ile [gerekli izinler](migrate-support-matrix-vmware.md#agent-based-migration-vcenter-server-permissions) aracısız geçiş.
3. [Not](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements) mobilite dahil olmak üzere, aracı tabanlı geçiş kullanarak Azure'a geçirmek istediğiniz VMware Vm'leri için gereksinimleri, geçirmek istediğiniz her VM'de hizmet.
4. Not [URL erişimi](migrate-support-matrix-vmware.md#agent-based-migration-url-access-requirements).
5. Gözden geçirme [bağlantı noktası erişim](migrate-support-matrix-vmware.md#agent-based-migration-port-requirements) her sanal makinede çalışan mobilite hizmeti ve Azure geçişi yapılandırma sunucusu gereksinimleri.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:
 
> [!div class="checklist"] 
> * Azure izinlerini ayarlayabilirsiniz.
> * VMware, değerlendirme ve geçiş için hazır.


Azure geçişi projesini ayarlarsınız için ikinci öğreticisiyle devam edin ve VMware Vm'leri Azure'a geçiş için değerlendirme.

> [!div class="nextstepaction"] 
> [VMware sanal makineleri değerlendirme](./tutorial-migrate-vmware.md) 

