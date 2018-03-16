---
description: "include dosyası"
author: tomarcher
manager: rloutlaw
ms.service: multiple
ms.workload: web
ms.devlang: na
ms.topic: include
ms.date: 03/12/2018
ms.author: tarcher
ms.custom: Jenkins
ms.openlocfilehash: 552e93e9bd1b17c73fb1638fbae2ac30b051c261
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
1. Tarayıcınızda açın [Azure Market görüntüsü için Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview).

1. Seçin **almak artık BT**.

    ![Jenkins Market görüntüsü için yükleme işlemini başlatmak için GIT BT şimdi seçin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. Fiyatlandırma ayrıntıları ve koşulları bilgileri gözden geçirdikten sonra Seç **devam**.

    ![Jenkins Market görüntü fiyatlandırması ve koşulları bilgileri.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. Seçin **oluşturma** Azure portalında Jenkins sunucusunu yapılandırmak için. 

    ![Jenkins Market görüntüsü yükleyin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-create.png)

1. İçinde **Temelleri** sekmesinde, aşağıdaki değerleri belirtin:

    - **Ad** -girin `Jenkins`.
    - **Kullanıcı adı** -Jenkins çalışan sanal makinede oturum açarken kullanılacak kullanıcı adını girin. Kullanıcı adı [belirli gereksinimleri](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm) karşılamalıdır.
    - **Kimlik doğrulama türü** - seçin **SSH ortak anahtarını**.
    - **SSH ortak anahtarını** -Kopyala ve Yapıştır tek satırlı biçimindeki bir RSA ortak anahtarı (başlayarak `ssh-rsa`) veya birden çok satırlı PEM biçimi. Linux ve macOS ya da Windows'da PuTTYGen ssh-keygen kullanarak SSH anahtarları oluşturabilir. SSH anahtarları ve Azure hakkında daha fazla bilgi için, bkz: [Windows Azure üzerinde ile SSH kullanma anahtarları nasıl](/azure/virtual-machines/linux/ssh-from-windows).
    - **Abonelik** -içine Jenkins yüklemek istediğiniz Azure aboneliğini seçin.
    - **Kaynak grubu** - seçin **Yeni Oluştur**ve Jenkins yüklemenizi olun kaynakları gruplandırması için mantıksal bir kapsayıcı görevi görür kaynak grubu için bir ad girin.
    - **Konum** - seçin **Doğu ABD**.

    ![Kimlik doğrulama ve kaynak grubu bilgileri Jenkins için temel sekmede girin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. Seçin **Tamam** geçmek için **ek ayarlar** sekmesi. 

1. İçinde **ek ayarlar** sekmesinde, aşağıdaki değerleri belirtin:

    - **Boyutu** -Jenkins sanal makineniz için uygun boyutlandırma seçeneği belirleyin.
    - **VM disk türü** - iki HDD (sabit disk sürücüsü) belirtin veya Jenkins sanal makine için hangi depolama disk türünü belirtmek için (katı hal sürücüsü) SSD izin verilir.
    - **Sanal ağ** -(isteğe bağlı) seçin **sanal ağ** varsayılan ayarlarını değiştirmek için.
    - **Alt ağlar** - seçin **alt ağlar**bilgileri doğrulayın ve seçin **Tamam**.
    - **Genel IP adresi** -IP adresi adı varsayılan olarak belirttiğiniz - IP soneki önceki sayfasındaki Jenkins adı. Bu varsayılanı değiştirmek için seçeneğini kullanabilirsiniz.
    - **Etki alanı adı etiketi** -Jenkins sanal makine için tam bir URL için değeri belirtin.
    - **Jenkins yayın türü** -istenen sürüm seçeneklerden birini seçin: `LTS`, `Weekly build`, veya `Azure Verified`. `LTS` Ve `Weekly build` seçenekleri makalesinde açıklanmıştır [Jenkins LTS yayın satır](https://jenkins.io/download/lts/). `Azure Verified` Seçeneği başvurduğu bir [Jenkins LTS sürüm](https://jenkins.io/download/lts/) Azure üzerinde çalışmak üzere doğrulandı. 

    ![Ayarlar sekmesinde Jenkins için sanal makine ayarlarını girin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. Seçin **Tamam** geçmek için **Tümleştirme ayarlarını** sekmesi.

1. İçinde **Tümleştirme ayarlarını** sekmesinde, aşağıdaki değerleri belirtin:

    - **Hizmet sorumlusu** -hizmet sorumlusu Jenkins Azure ile kimlik doğrulaması için kimlik bilgisi olarak eklenir. `Auto` Asıl MSI (yönetilen hizmet kimliği) tarafından oluşturulacak anlamına gelir. `Manual` Asıl sizin tarafınızdan oluşturulması gerektiğini anlamına gelir. 
        - **Uygulama Kimliği** ve **gizli** - seçerseniz `Manual` seçenek için **hizmet sorumlusu** seçeneğini belirtmeniz gerekir `Application ID` ve `Secret` için Hizmet sorumlusu. Zaman [bir hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli), varsayılan rolü Not **katkıda bulunan**, Azure kaynakları ile çalışmak için yeterli olduğu.
    - **Bulut aracıları etkinleştirmek** -aracılar için varsayılan bulut şablonu belirtin nerede `ACI` Azure kapsayıcı örneğine başvurur ve `VM` sanal makinelere başvuruyor. Ayrıca belirtebilirsiniz `No` bulut Aracısını etkinleştirmek istemiyorsanız.

1. Seçin **Tamam** geçmek için **Özet** sekmesi.

1. Zaman **Özet** sekmesini görüntüler, girdiğiniz bilgileri doğrulanır. Gördüğünüz sonra **geçirilen doğrulama** seçin (sekmesinin en üstünde), ileti **Tamam**. 

    ![Özet sekmesi görüntüler ve seçili seçeneklerinizi doğrular.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. Zaman **oluşturma** seçin, sekmesini görüntüler **oluşturma** Jenkins sanal makine oluşturmak için. Sunucunuz hazır olduğunda, Azure portalında bir bildirim görüntüler.

    ![Jenkins hazır bildirimidir.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-notification.png)