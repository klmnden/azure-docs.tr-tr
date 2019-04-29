---
description: include dosyası
author: tomarcher
manager: rloutlaw
ms.service: multiple
ms.workload: web
ms.devlang: na
ms.topic: include
ms.date: 03/12/2018
ms.author: tarcher
ms.custom: Jenkins
ms.openlocfilehash: 5439de30b02b0ce05853c8112f9e29239743ef98
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60642432"
---
1. Tarayıcınızda açın [Jenkins için Azure Market görüntüsü](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview).

1. Seçin **alma şimdi**.

    ![Jenkins Market görüntüsü için yükleme işlemini başlatmak için artık GIT BT'yi seçin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. Fiyatlandırma ayrıntıları ve koşulları bilgileri gözden geçirdikten sonra seçin **devam**.

    ![Jenkins Market görüntü fiyatlandırması ve koşulları bilgileri.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. Seçin **Oluştur** Azure portalında Jenkins sunucusunu yapılandırmak için. 

    ![Jenkins Market görüntüsü yükleyin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-create.png)

1. İçinde **Temelleri** sekmesinde, aşağıdaki değerleri belirtin:

   - **Adı** -girin `Jenkins`.
   - **Kullanıcı adı** -Jenkins üzerinde çalıştığı sanal makineye oturum açarken kullanılacak kullanıcı adını girin. Kullanıcı adı [belirli gereksinimleri](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm) karşılamalıdır.
   - **Kimlik doğrulama türü** - seçin **SSH ortak anahtarı**.
   - **SSH ortak anahtarı** -kopyalama ve yapıştırma RSA ortak anahtarını tek satırlı biçimde (başlayarak `ssh-rsa`) veya çok satırlı PEM biçimi. Ssh-keygen Linux ve macOS veya Windows üzerinde puttygen araçlarını kullanarak SSH anahtarları oluşturabilirsiniz. SSH anahtarları ve Azure hakkında daha fazla bilgi için bkz [azure'da Windows ile SSH anahtarlarını kullanma nasıl](/azure/virtual-machines/linux/ssh-from-windows).
   - **Abonelik** -Jenkins yüklemek istediğiniz Azure aboneliğini seçin.
   - **Kaynak grubu** - seçin **Yeni Oluştur**, Jenkins yüklemenizin olun kaynak koleksiyonu için mantıksal kapsayıcı görevi gören bir kaynak grubu için bir ad girin.
   - **Konum** - seçin **Doğu ABD**.

     ![Jenkins için kimlik doğrulama ve kaynak grubu bilgileri temel sekmede girin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. Seçin **Tamam** geçmek için **ek ayarlar** sekmesi. 

1. İçinde **ek ayarlar** sekmesinde, aşağıdaki değerleri belirtin:

   - **Boyutu** -Jenkins sanal makineniz için uygun boyutlandırma seçeneğini belirleyin.
   - **VM disk türü** - ya da HDD (sabit disk sürücüsü) belirtin veya Jenkins sanal makinesi için hangi depolama disk türüne belirtmek için SSD (katı hal sürücüsü) izin verilir.
   - **Sanal ağ** -(isteğe bağlı) seçin **sanal ağ** varsayılan ayarlarını değiştirmek için.
   - **Alt ağlar** - seçin **alt ağlar**bilgileri doğrulayın ve seçin **Tamam**.
   - **Genel IP adresi** -IP adresi adı varsayılan olarak bir sonek - IP önceki sayfada belirtilen Jenkins adı. Bu varsayılanı değiştirmek için seçeneğini kullanabilirsiniz.
   - **Etki alanı adı etiketi** -Jenkins sanal makinesi için tam URL değeri belirtin.
   - **Jenkins yayın türünü** -istenen sürüm türü seçenekleri: `LTS`, `Weekly build`, veya `Azure Verified`. `LTS` Ve `Weekly build` seçenekleri makalesinde açıklanan [Jenkins LTS yayın satırı](https://jenkins.io/download/lts/). `Azure Verified` Seçeneği başvurduğu bir [Jenkins LTS sürümünü](https://jenkins.io/download/lts/) Azure'da çalışmak üzere doğrulandı. 
   - **JDK türü** -JDK yüklenecek. Zulu test edilmiş, sertifikalı derlemelerini OpenJDK varsayılandır.

     ![Ayarlar sekmesinde Jenkins için sanal makine ayarlarını girin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. Seçin **Tamam** geçmek için **Tümleştirme ayarlarını** sekmesi.

1. İçinde **Tümleştirme ayarlarını** sekmesinde, aşağıdaki değerleri belirtin:

    - **Hizmet sorumlusu** -hizmet sorumlusu, Jenkins ile Azure ile kimlik doğrulaması için bir kimlik bilgisi olarak eklenir. `Auto` Asıl MSI (yönetilen hizmet kimliği) tarafından oluşturulan anlamına gelir. `Manual` Asıl sizin tarafınızdan oluşturulması gerektiğini anlamına gelir. 
        - **Uygulama Kimliği** ve **gizli** - seçerseniz `Manual` seçeneğini **hizmet sorumlusu** seçeneğini belirtmeniz gerekir `Application ID` ve `Secret` için Hizmet sorumlusu. Zaman [bir hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli), varsayılan rolü Not **katkıda bulunan**, Azure kaynaklarıyla çalışmak için yeterli olduğu.
    - **Bulut aracıları etkinleştirmek** -için aracıları varsayılan bulut şablonu belirtin burada `ACI` Azure Container Instance için ifade eder ve `VM` sanal makinelere ifade eder. Ayrıca belirtebileceğiniz `No` bulut Aracısını etkinleştirmek istemiyorsanız.

1. Seçin **Tamam** geçmek için **özeti** sekmesi.

1. Zaman **özeti** sekmesini görüntüler, girdiğiniz bilgileri doğrulandı. Gördüğünüzde **doğrulama başarılı** seçin (sekmenin en üstündeki), ileti **Tamam**. 

     ![Özet sekmesi, görüntüler ve seçili seçeneklerinizi doğrular.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. Zaman **Oluştur** sekmesini görüntüler, seçin **Oluştur** Jenkins sanal makinesi oluşturmak için. Sunucunuz hazır olduğunda, Azure portalında bir bildirim görüntülenir.

     ![Jenkins hazır bildirimidir.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-notification.png)
