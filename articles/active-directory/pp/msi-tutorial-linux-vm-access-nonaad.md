---
title: Azure Key Vault'a erişmek için bir Linux VM MSI kullanma
description: Öğretici, Azure Resource Manager'a erişmek için bir Linux VM yönetilen hizmet kimliği (MSI) kullanma işlemi gösterilmektedir.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 95a9530c02bbf7b1cd9d137129f96ff4ee016966
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007674"
---
# <a name="use-a-linux-vm-managed-service-identity-msi-to-access-azure-key-vault"></a>Azure Key Vault'a erişmesi için bir Linux VM yönetilen hizmet kimliği (MSI) kullanma 

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğreticide bir Linux sanal makinesi için Yönetilen hizmet kimliği (MSI) etkinleştirdikten sonra Azure Key Vault'a erişmek için bu kimliği kullanın gösterilmektedir. Bir önyükleme hizmet veren, anahtar kasası istemci uygulamanızı Azure Active Directory (AD tarafından) güvenli olmayan kaynaklara erişmek için parola kullanacak şekilde sağlar. Yönetilen hizmet kimlikleri, Azure tarafından otomatik olarak yönetilir ve Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuza eklemek zorunda kalmadan destekleyen hizmetler için kimlik doğrulaması sağlar. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure'da bir Linux sanal makinesine MSI etkinleştir 
> * Bir anahtar Kasası'nda depolanan gizli dizi, VM erişimi verme 
> * VM kimliğini kullanarak bir erişim belirteci alma ve gizli anahtar Kasasından almak için kullanın 
 
## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Linux sanal makinesi oluşturma

Bu öğretici için yeni bir Linux VM'yi oluştururuz. Mevcut VM'yi MSI de etkinleştirebilirsiniz.

1. Tıklayın **kaynak Oluştur** sol üst köşesinde Azure portal'ın üzerinde.
2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. Sanal makine bilgilerini girin. İçin **kimlik doğrulama türü**seçin **SSH ortak anahtarı** veya **parola**. Oluşturulan kimlik bilgilerini, VM'de oturum açmak izin verin.

    ![Alt resim metni](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Seçin bir **abonelik** sanal makinenin açılır.
5. Yeni bir seçilecek **kaynak grubu** sanal makinenin oluşturulması, seçmek istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6. Sanal makine için boyutu seçin. Daha fazla boyut görmek için seçin **tümünü görüntüle** veya desteklenen disk türü filtresini değiştirin. Ayarları sayfasında, varsayılan değerleri koruyun ve **Tamam**.

## <a name="enable-msi-on-your-vm"></a>Vm'nizde MSI etkinleştir

Bir sanal makine MSI, kimlik bilgilerini kodunuza koyma gereksinimi olmadan Azure AD'den erişim belirteci alma olanak tanır. Perde MSI etkinleştirmesine iki şeyi yapar: VM'NİZDE MSI VM uzantısı yükler ve VM için MSI sağlar.  

1. Seçin **sanal makine** MSI etkinleştirmek istiyorsanız.
2. Sol gezinti çubuğunda Koruma'ya tıklayın **yapılandırma**.
3. Gördüğünüz **yönetilen hizmet kimliği**. Kaydolun ve MSI etkinleştirmek için **Evet**, devre dışı bırakmak istiyorsanız seçin No
4. Tıkladığınız olun **Kaydet** yapılandırmayı kaydetmek için.

    ![Alt resim metni](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Hangi uzantıların bu olduğunu denetlemek isterseniz **Linux VM**, tıklayın **uzantıları**. MSI etkin olduğunda **ManagedIdentityExtensionforLinux** listede görüntülenir.

    ![Alt resim metni](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)


## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>Bir anahtar Kasası'nda depolanan gizli dizi, VM erişim  

MSI kullanarak kodunuzu Azure Active Directory kimlik doğrulamasını destekleyen kaynakların kimliğini doğrulamak için erişim belirteçleri elde edebilirsiniz. Ancak, tüm Azure Hizmetleri, Azure AD kimlik doğrulamasını destekler. Bu hizmetlerle MSI kullanmak için Azure anahtar Kasası'nda hizmet kimlik bilgilerini depolamak ve kimlik bilgilerini almak için anahtar kasasına erişmek için MSI kullanma. 

İlk olarak, bir Key Vault oluşturma ve anahtar Kasası'na bizim VM'nin kimlik erişim vermek ihtiyacımız var.   

1. Sol gezinti çubuğunun üstündeki seçin **+ yeni** ardından **güvenlik + kimlik** ardından **Key Vault**.  
2. Sağlayan bir **adı** yeni Key Vault için. 
3. Daha önce oluşturduğunuz sanal makine aynı abonelik ve kaynak grubunda bir Key Vault bulun. 
4. Seçin **erişim ilkeleri** tıklatıp **yeni Ekle**. 
5. Şablondan Yapılandır seçin **gizli dizi Yönetimi**. 
6. Seçin **sorumlu Seç**ve arama alanına daha önce oluşturduğunuz sanal makine adını girin.  Sonuç listesinden VM'yi seçin ve tıklayın **seçin**. 
7. Tıklayın **Tamam** için yeni bir erişim ilkesi ekleme tamamlama ve **Tamam** erişim ilkesi seçimi tamamlamak için. 
8. Tıklayın **Oluştur** Key Vault oluşturmayı tamamlayın. 

    ![Alt resim metni](~/articles/active-directory/media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)

Ardından, daha sonra VM'de çalışan kod kullanarak gizli alabilmeleri bir gizli anahtar Kasası'na ekleyin: 

1. Seçin **tüm kaynakları**, bulmak ve oluşturduğunuz anahtar Kasası'nı seçin. 
2. Seçin **gizli dizileri**, tıklatıp **Ekle**. 
3. Seçin **el ile**, gelen **karşıya yükleme seçenekleri**. 
4. Bir ad ve değer için parolayı girin.  Değer, istediğiniz herhangi bir şey olabilir. 
5. Etkinleştirme Tarihi ve sona erme tarihi açık bırakın ve bırakın **etkin** olarak **Evet**. 
6. Tıklayın **Oluştur** gizli dizi oluşturmak için. 
 
## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-retrieve-the-secret-from-the-key-vault"></a>Sanal makinenin kimliğini kullanarak bir erişim belirteci alma ve gizli anahtar Kasasından almak için kullanın  

Bu adımları tamamlamak için bir SSH istemcisi gerekir.  Windows kullanıyorsanız, SSH İstemcisi'nde kullanabileceğiniz [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about). SSH istemcinizin anahtarları yapılandırılıyor yardıma ihtiyacınız varsa bkz [azure'da Windows ile SSH kullanma anahtarları nasıl](~/articles/virtual-machines/linux/ssh-from-windows.md), veya [oluşturmak ve azure'da Linux VM'ler için SSH ortak ve özel anahtar çifti kullanmak nasıl](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).
 
1. Portalda hem de Linux vm'nize gidin **genel bakış**, tıklayın **Connect**. 
2. **Connect** VM'ye SSH istemcisine sahip. 
3. Terminal penceresinde CURL, kullanarak Azure Key Vault için erişim belirteci almak için yerel MSI uç noktasına bir istek olun.  
 
    CURL istek için erişim belirteci aşağıda verilmiştir.  
    
    ```bash
    curl http://localhost:50342/oauth2/token --data "resource=https://vault.azure.net" -H Metadata:true  
    ```
    Yanıt, Resource Manager'a erişmek için gereken erişim belirteci içeriyor. 
    
    Yanıt:  
    
    ```bash
    {"access_token":"eyJ0eXAi...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://vault.azure.net",
    "token_type":"Bearer"} 
    ```
    
    Azure anahtar Kasası'na kimliğini doğrulamak için bu erişim belirteci kullanabilirsiniz.  Sonraki CURL isteği CURL ve anahtar kasası REST API kullanarak Key Vault'tan bir gizli dizi okumak nasıl gösterir.  İçinde anahtar kasası, URL'si gerekir **Essentials** bölümünü **genel bakış** anahtar kasası sayfasında.  Ayrıca, önceki çağrıda aldığınız erişim belirtecini gerekir. 
        
    ```bash
    curl https://<YOUR-KEY-VAULT-URL>/secrets/<secret-name>?api-version=2016-10-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```
    
    Yanıt şöyle görünür: 
    
    ```bash
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Key Vault'tan gizli dizi alınan sonra adı ve parola gerektiren bir hizmet için kimlik doğrulaması için kullanabilirsiniz.


## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz. [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.




