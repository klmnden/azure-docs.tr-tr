---
title: Hyper-V Vm'leri, değerlendirme ve Azure geçişi ile azure'a geçiş için hazırlama | Microsoft Docs
description: Değerlendirme ve Hyper-V Vm'lerini Azure Geçişi'ni kullanarak azure'a geçiş için hazırlanma işlemi açıklanmaktadır.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 9e0d29770aa36f8e79bf08b7c5435ea2dbc4ae38
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840380"
---
# <a name="prepare-for-assessment-and-migration-of-hyper-v-vms-to-azure"></a>Değerlendirme ve Hyper-V Vm'lerini azure'a geçiş için hazırlama

Bu makalede, değerlendirme ve şirket içi Hyper-V Vm'lerini azure'a geçiş için hazırlamak üzere açıklanır [Azure geçişi](migrate-services-overview.md).

[Azure geçişi](migrate-overview.md) hub'ı keşfedin, değerlendirin ve uygulamalar ve altyapı iş yükleri Microsoft Azure'a geçirmek için yardımcı araçlar sağlar. Hub, Azure geçişi araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri içerir. 

Bu öğretici, değerlendirmek ve Hyper-V Vm'lerini Azure'a geçirme işlemini gösteren bir serinin ilk bölümüdür. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure'ı hazırlama. Azure geçişi ile çalışmak için Azure hesabınızı ve kaynaklarınızı için izinleri ayarlayın.
> * Şirket içi Hyper-V konakları ve sanal makineleri server değerlendirmesi için hazırlayın.
> * Şirket içi Hyper-V konakları ve sanal makineleri, server geçiş için hazırlayın.


> [!NOTE]
> Öğreticiler hızlı bir kavram kanıtı ayarlayabilirsiniz bir senaryo için en basit dağıtım yolu gösterir. Öğreticiler, mümkün olduğunda varsayılan seçenekleri kullanın ve tüm olası ayarları ve yol gösterme. Ayrıntılı yönergeler için "nasıl" soruları Hyper-V değerlendirme ve geçiş için gözden geçirin.


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prepare-azure"></a>Azure’u hazırlama

### <a name="azure-permissions"></a>Azure izinleri

Azure geçişi dağıtmak için izinler birkaç ihtiyacınız vardır:

- Azure hesabınızda bir değerlendirme ve geçiş için Azure geçişi projesi oluşturmak için izinler gerekiyor. 
- Azure hesabınızı kullanarak Azure geçişi Gereci kaydetmek için izinler gerekiyor.
    - Değerlendirme için Azure geçişi, Hyper-V Vm'lerini bulur ve Azure geçişi için VM meta verileri ve performans verilerini gönderen basit bir gereç çalıştırır.
    - Gereç kayıt sırasında Azure geçişi, gereç tanıtan iki Azure Active Directory (Azure AD) uygulama oluşturur:
        - İlk uygulamanızı Azure geçişi Hizmeti uç noktaları ile iletişim kurar.
        - İkinci uygulamayı bir Azure kayıt sırasında oluşturulan Azure AD uygulama bilgileri ve gerecini yapılandırma ayarlarını depolamak için Key Vault erişir.
    - Aşağıdaki yöntemlerden birini kullanarak bu Azure AD uygulamaları oluşturmak Azure geçişi için izinler atayabilirsiniz:
        - Kiracı/genel yönetici, oluşturmak ve Azure AD uygulamaları kaydetmek için kiracıdaki kullanıcılar izin verebilirsiniz.
        - Kiracı/genel yönetici hesabına (izinleri) Uygulama geliştirici rolünü atayabilirsiniz.
    - Bu, hatalarının ayıklanabileceğini belirtmekte yarar:
        - Uygulamalar, abonelikte yukarıda açıklanan dışındaki herhangi bir erişim izinleri yok.
        - Yeni bir gereç kaydettiğinizde yalnızca bu izinleri gerekir. Gereç ayarlandıktan sonra izinleri kaldırabilir. 


### <a name="assign-permissions-to-create-project"></a>Proje oluşturma izinleri atama

Bir Azure geçişi projesi oluşturma izniniz denetleyin.

1. Azure portalında abonelik açın ve seçin **erişim denetimi (IAM)** .
2. İçinde **denetleyin erişim**, ilgili hesabını bulun ve izinleri görüntülemek için tıklayın.
3. Olmalıdır **katkıda bulunan** veya **sahibi** izinleri.
    - Ücretsiz Azure hesabı oluşturduysanız, aboneliğinizin sahibine demektir.
    - Abonelik sahibi değilseniz, rol atamak için sahibi ile çalışır.


### <a name="assign-permissions-to-register-the-appliance"></a>Gereç kaydetmek için izinler atama

Sanal makineleri değerlendirme için Azure geçişi Gereci dağıtıyorsanız, onu kaydetmeniz gerekir.

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

Kiracı/genel yönetici gibi izinleri verebilir:

1. Kiracı/genel yönetici Azure AD'de gidin **Azure Active Directory** > **kullanıcılar** > **kullanıcı ayarları**.
2. Yönetici ayarlamalısınız **uygulama kayıtları** için **Evet**.

    ![Azure AD izinleri](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> Bu hassas olmayan varsayılan ayardır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).



#### <a name="assign-application-developer-role"></a>Uygulama geliştiricisi rolü atama 

Kiracı/genel yönetici hesabınız için uygulama geliştiricisinin rol atayabilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).


## <a name="prepare-for-hyper-v-assessment"></a>Hyper-V değerlendirmesi için hazırlama

Hyper-V değerlendirmesi için hazırlamak için Hyper-V konak ve sanal makine ayarları doğrulayın ve gereç dağıtım ayarlarını doğrulayın.

### <a name="verify-hyper-v-host-settings"></a>Hyper-V ana bilgisayar ayarlarını doğrulama

1. Doğrulama [Hyper-V ana bilgisayar gereksinimleri](migrate-support-matrix-hyper-v.md#assessment-hyper-v-host-requirements) server değerlendirmesi için.
2. Emin [gerekli bağlantı noktaları](migrate-support-matrix-hyper-v.md#assessment-port-requirements) Hyper-V konaklarında açıktır.

### <a name="enable-powershell-remoting-on-hosts"></a>Konaklarda PowerShell uzaktan iletişimini etkinleştirme

PowerShell uzaktan iletişimi her konakta şu şekilde ayarlayın:

1. Her konakta yönetici olarak bir PowerShell konsolu açın.
2. Şu komutu çalıştırın:

    ```
    Enable-PSRemoting -force
    ```

### <a name="enable-credssp-on-hosts"></a>Ana bilgisayarlarda Credssp'yi etkinleştirmenizi

VM diskleri SMB paylaşımlarında bulunuyorsa, her ilgili Hyper-V konağında bu adımı tamamlayın. Bu adım, yapılandırma bilgilerini Hyper-V Vm'leri için diskleri ile SMB paylaşımlarında bulmak için kullanılır. VM diskleri SMB paylaşımlarında yoksa, adımı atlayabilirsiniz.

1. Hyper-V Vm'lerini disklerle SMB paylaşımları üzerinde çalışan Hyper-V konakları belirleyin.
2. Tanımlanan her Hyper-V konağında şu komutu çalıştırın:

    ```
    Enable-WSManCredSSP -Role Server -Force
    ```

- Azure geçişi istemci adına kimlik bilgileri temsilcisi Hyper-V konağı CredSSP kimlik doğrulaması sağlar.
- Bu komut tüm Hyper-V konaklarında uzaktan çalıştırabilirsiniz.
- Yeni konak düğümleri eklerseniz bir kümede bulma için otomatik olarak eklenir, ancak gerekirse CredSSP yeni düğümler üzerinde el ile etkinleştirmeniz gerekir.

### <a name="verify-appliance-settings"></a>Gereç ayarlarını doğrulayın

Azure geçişi Gereci ve sonraki öğreticide başına değerlendirmesi ayarlamadan önce gereç dağıtımı için hazırlık yaparsınız.

1. [Doğrulama](migrate-support-matrix-hyper-v.md#assessment-appliance-requirements) Gereci gereksinimleri.
2. [Gözden geçirme](migrate-support-matrix-hyper-v.md#assessment-appliance-url-access) gereç erişmesi gereken Azure URL'lerini.
3. Gereç Keşif ve değerlendirme sırasında topladığınız verilerin gözden geçirin.
4. [Not](migrate-support-matrix-hyper-v.md#assessment-port-requirements) bağlantı noktası, cihaz için erişim gereksinimleri.


### <a name="set-up-an-account-for-vm-discovery"></a>VM bulma için bir hesabı ayarlayın

Azure geçişi, şirket içi Vm'leri bulmak için izinler gerekiyor.

- Bir etki alanı veya Hyper-V konak/küme üzerinde yönetici izinlerine sahip yerel kullanıcı hesabı ayarlayın.

    - Tüm konaklar ve bulmaya dahil etmek istediğiniz küme için tek bir hesap gerekir.
    - Hesap bir yerel veya etki alanı hesabı olabilir. Hyper-V konakları veya kümeleri üzerinde yönetici izinlerine sahip öneririz.
    - Alternatif olarak, yönetici izinleri atamak istemiyorsanız, aşağıdaki izinler gereklidir:
        - Uzak Yönetimi kullanıcıları
        - Hyper-V yöneticileri
        - Performans İzleyicisi kullanıcıları

### <a name="enable-integration-services-on-vms"></a>Integration Services vm'lerde etkinleştir

Böylece Azure geçişi, VM'deki işletim sistemi bilgilerini yakalamak için her sanal makinede tümleştirme hizmetleri etkinleştirilmiş olmalıdır.

- Bulmak ve değerlendirmek istediğiniz sanal makineleri üzerinde etkinleştirmek [Hyper-V tümleştirme hizmetlerini](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) her VM'de. 

## <a name="prepare-for-hyper-v-migration"></a>Hyper-V geçiş için hazırlama

1. [Gözden geçirme](migrate-support-matrix-hyper-v.md#migration-hyper-v-host-requirements) geçiş için Hyper-V ana bilgisayar gereksinimleri.
2. [Gözden geçirme](migrate-support-matrix-hyper-v.md#migration-hyper-v-vm-requirements) Azure'a geçirmek istediğiniz Hyper-V Vm'leri için gereksinimleri.
3. [Not](migrate-support-matrix-hyper-v.md#migration-hyper-v-host-url-access) hangi Hyper-V konakları ve kümeleri erişmesi gereken VM geçişi için Azure URL'lerini.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:
 
> [!div class="checklist"] 
> * Azure hesabı izinlerini ayarlayabilirsiniz.
> * Hazırlanan Hyper-V konakları ve sanal makineleri değerlendirme ve geçiş için.

Azure geçişi projesini oluşturmak için sonraki öğreticiye devam edin ve Hyper-V Vm'lerini Azure'a geçiş için değerlendirme

> [!div class="nextstepaction"] 
> [Hyper-V sanal makineleri değerlendirme](./tutorial-assess-hyper-v.md) 
