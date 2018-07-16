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
ms.openlocfilehash: 95f54cf9cda2bf6dede6e7bfb72471b22707c1c2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056081"
---
# <a name="use-a-linux-vm-managed-service-identity-msi-to-access-azure-key-vault"></a>Azure Key Vault'a erişmesi için bir Linux VM yönetilen hizmet kimliği (MSI) kullanma 

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğreticide bir Linux sanal makinesi için Yönetilen hizmet kimliği (MSI) etkinleştirdikten sonra Azure Key Vault'a erişmek için bu kimliği kullanın gösterilmektedir. Önyükleme işlevi gören Key Vault, istemci uygulamanızın gizli diziyi kullanarak Azure Active Directory (AD) tarafından güvenlik altına alınmamış kaynaklara erişmenize olanak sağlar. Yönetilen Hizmet Kimlikleri Azure tarafından otomatik olarak yönetilir kodunuza kimlik bilgileri girmenize gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmanıza olanak tanır. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure'da bir Linux sanal makinesine MSI etkinleştir 
> * VM'nize Key Vault'ta depolanan gizli diziye erişim verme 
> * VM kimliğini kullanarak erişim belirteci alma ve Key Vault'tan gizli diziyi almak için bunu kullanma 
 
## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Linux sanal makinesi oluşturma

Bu öğretici için, yeni bir Linux VM oluşturuyoruz. Ayrıca mevcut bir VM'de MSI'yi etkinleştirebilirsiniz.

1. Tıklayın **kaynak Oluştur** sol üst köşesinde Azure portal'ın üzerinde.
2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. Sanal makine bilgilerini girin. **Kimlik doğrulama türü** olarak **SSH ortak anahtarı**'nı veya **Parola**'yı seçin. Oluşturulan kimlik bilgileri VM'de oturum açmanıza olanak tanır.

    ![Alternatif resim metni](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Seçin bir **abonelik** sanal makinenin açılır.
5. Yeni bir seçilecek **kaynak grubu** sanal makinenin oluşturulması, seçmek istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6. VM'nin boyutunu seçin. Daha fazla boyut görmek için seçin **tümünü görüntüle** veya desteklenen disk türü filtresini değiştirin. Ayarları sayfasında, varsayılan değerleri koruyun ve **Tamam**.

## <a name="enable-msi-on-your-vm"></a>VM'nizde MSI'yi etkinleştirme

Sanal Makine MSI'si kodunuza kimlik bilgileri yerleştirmeniz gerekmeden Azure AD'den erişim belirteçlerini almanıza olanak tanır. Perde MSI etkinleştirmesine iki şeyi yapar: VM'NİZDE MSI VM uzantısı yükler ve VM için MSI sağlar.  

1. MSI'yi etkinleştirmek istediğiniz **Sanal Makine**'yi seçin.
2. Sol gezinti çubuğunda **Yapılandırma**'ya tıklayın.
3. **Yönetilen Hizmet Kimliği**'ni görürsünüz. MSI'yi kaydetmek ve etkinleştirmek için **Evet**'i seçin, devre dışı bırakmak istiyorsanız Hayır'ı seçin.
4. Yapılandırmayı kaydetmek için **Kaydet**’e tıkladığınızdan emin olun.

    ![Alternatif resim metni](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Hangi uzantıların bu olduğunu denetlemek isterseniz **Linux VM**, tıklayın **uzantıları**. MSI etkin olduğunda **ManagedIdentityExtensionforLinux** listede görüntülenir.

    ![Alternatif resim metni](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)


## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>VM'nize Key Vault'ta depolanan Gizli Diziye erişim verme  

MSI kullanarak kodunuzu Azure Active Directory kimlik doğrulamasını destekleyen kaynakların kimliğini doğrulamak için erişim belirteçleri elde edebilirsiniz. Bununla birlikte, Azure hizmetlerinin tümü Azure AD kimlik doğrulamasını desteklemez. Söz konusu hizmetlerle MSI kullanmak için, hizmet kimlik bilgilerini Azure Key Vault'ta depolayın ve MSI'yi kullanarak Key Vault'a erişip kimlik bilgilerini alın. 

İlk olarak, Key Vault'u oluşturmalı ve VM'mize Key Vault üzerinde kimlik erişimi vermeliyiz.   

1. Sol gezinti çubuğunun üstündeki seçin **+ yeni** ardından **güvenlik + kimlik** ardından **Key Vault**.  
2. Yeni Key Vault için bir **Ad** belirtin. 
3. Key Vault'u daha önce oluşturduğunuz VM'yle aynı aboneliğe ve kaynak grubuna yerleştirin. 
4. **Erişim ilkeleri**’ni seçin ve **Yeni ekle**'ye tıklayın. 
5. Şablondan yapılandır'da **Gizli Dizi Yönetimi**'ni seçin. 
6. **Sorumlu Seç**'i seçin ve arama alanına daha önce oluşturduğunuz VM'nin adını girin.  Sonuç listesinde VM'yi seçin ve **Seç**'e tıklayın. 
7. Yeni erişim ilkesini ekleme işlemini tamamlamak için **Tamam**'a tıklayın ve erişim ilkesi seçimini bitirmek için de **Tamam**'a tıklayın. 
8. Key Vault oluşturmayı tamamlamak için **Oluştur**'a tıklayın. 

    ![Alternatif resim metni](../managed-service-identity/media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)

Ardından, Key Vault'a bir gizli dizi ekleyin; böylelikle VM'nizde çalıştırılan kodu kullanarak daha sonra gizli diziyi alabilirsiniz: 

1. **Tüm Kaynaklar**'ı seçin, sonra da oluşturduğunuz Key Vault'u bulun ve seçin. 
2. **Gizli Diziler**'i seçin ve **Ekle**'ye tıklayın. 
3. **Karşıya yükleme seçenekleri**'nden **El ile** seçeneğini belirtin. 
4. Gizli dizi için bir ad ve değer girin.  Değer, istediğiniz herhangi bir şey olabilir. 
5. Etkinleştirme tarihi ile sona erme tarihini boş bırakın ve **Etkin** seçeneğini **Evet** değerinde bırakın. 
6. Gizli diziyi oluşturmak için **Oluştur**'a tıklayın. 
 
## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-retrieve-the-secret-from-the-key-vault"></a>Sanal makinenin kimliğini kullanarak bir erişim belirteci alma ve gizli anahtar Kasasından almak için kullanın  

Bu adımları tamamlamak bir SSH istemciniz olmalıdır.  Windows kullanıyorsanız, [Linux için Windows Alt Sistemi](https://msdn.microsoft.com/commandline/wsl/about)'ndeki SSH istemcisini kullanabilirsiniz. SSH istemcinizin anahtarlarını yapılandırmak için yardıma ihtiyacınız olursa, bkz. [Azure'da Windows ile SSH anahtarlarını kullanma](~/articles/virtual-machines/linux/ssh-from-windows.md) veya [Azure’da Linux VM’ler için SSH ortak ve özel anahtar çifti oluşturma](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).
 
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
    
    Azure anahtar Kasası'na kimliğini doğrulamak için bu erişim belirteci kullanabilirsiniz.  Sonraki CURL isteği CURL ve anahtar kasası REST API kullanarak Key Vault'tan bir gizli dizi okumak nasıl gösterir.  Key Vault'unuzun URL'sine ihtiyacınız olacaktır. Bu URL, Key Vault'un **Genel Bakış** sayfasındaki **Temel Parçalar** bölümünde yer alır.  Ayrıca, önceki çağrıda aldığınız erişim belirtecini gerekir. 
        
    ```bash
    curl https://<YOUR-KEY-VAULT-URL>/secrets/<secret-name>?api-version=2016-10-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```
    
    Yanıt şöyle görünür: 
    
    ```bash
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Key Vault'tan gizli diziyi aldıktan sonra, bunu kullanarak ad ve parola gerektiren bir hizmette kimlik doğrulaması yapabilirsiniz.


## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz. [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.




