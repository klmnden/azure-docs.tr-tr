---
title: Azure anahtar kasası erişmek için bir Linux VM MSI kullanın
description: Azure Resource Manager erişmek için bir Linux VM yönetilen hizmet kimliği (MSI) kullanma sürecinde anlatan öğretici.
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
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: fe38a423ffc40da21299b727c37532b9f0001d59
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="use-a-linux-vm-managed-service-identity-msi-to-access-azure-key-vault"></a>Azure anahtar kasası erişmek için bir Linux VM yönetilen hizmet kimliği (MSI) kullanın 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğretici bir Linux sanal makine için Yönetilen hizmet kimliği (MSI) etkinleştirmek ve ardından Azure anahtar kasası erişmek için bu kimliği kullanan gösterilmektedir. Bir önyükleme hizmet veren, anahtar kasası istemci uygulamanızı Azure Active Directory (AD tarafından) güvenli olmayan kaynaklara erişmek için gizli anahtar'ı kullanacak şekilde sağlar. Yönetilen hizmet kimliği Azure tarafından otomatik olarak yönetilir ve Azure AD kimlik doğrulaması, kimlik bilgileri kodunuza eklemek zorunda kalmadan destekleyen hizmetler için kimlik doğrulaması sağlar. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir Linux sanal makinede MSI etkinleştir 
> * Bir anahtar kasasında depolanan bir gizli anahtar, VM erişim 
> * VM kimliğini kullanarak bir erişim belirteci alın ve gizli anahtar Kasası'nı almak için kullanın 
 
## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Linux sanal makine oluşturun

Bu öğretici için yeni bir Linux VM oluşturun. Mevcut bir VM'yi üzerinde MSI de etkinleştirebilirsiniz.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesine tıklayın.
2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. Sanal makine bilgilerini girin. İçin **kimlik doğrulama türü**seçin **SSH ortak anahtarını** veya **parola**. Oluşturulan kimlik bilgileri, VM'ye oturum açmak izin verir.

    ![Alt görüntü metin](../media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Seçin bir **abonelik** sanal makine açılır.
5. Yeni bir seçmek için **kaynak grubu** sanal makinenin oluşturulması, seçmek istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6. VM boyutunu seçin. Daha fazla boyutları görmek için seçin **tüm görüntüle** veya desteklenen disk türü filtresini değiştirin. Ayarlar sayfasında, Varsayılanları tutun ve **Tamam**.

## <a name="enable-msi-on-your-vm"></a>MSI VM üzerinde etkinleştir

Bir sanal makine MSI erişim belirteçleri, kimlik bilgileri kodunuza koyma gereksinimi olmadan Azure AD'den almanızı sağlar. Yönetilen hizmet kimliği bir VM'de etkinleştirme iki şey yapar: yazmaçlar yönetilen kimliğini ve oluşturmak için Azure Active Directory ile VM VM kimliğini yapılandırır.

1. Seçin **sanal makine** MSI etkinleştirmek istediğiniz.
2. Sol gezinti çubuğunda **yapılandırma**.
3. Gördüğünüz **yönetilen hizmet kimliği**. Kaydolun ve MSI etkinleştirmek için seçin **Evet**, devre dışı bırakmak istiyorsanız seçin No
4. Tıklattığınız olun **kaydetmek** yapılandırmayı kaydetmek için.

    ![Alt görüntü metin](../media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>Bir anahtar kasasında depolanan bir gizli anahtar, VM erişim  

MSI kullanarak kodunuzu Azure Active Directory kimlik doğrulamasını destekleyen kaynaklar için kimlik doğrulaması için erişim belirteçleri elde edebilirsiniz. Ancak, tüm Azure Hizmetleri Azure AD kimlik doğrulamasını destekler. MSI bu hizmetleri ile kullanmak için hizmet kimlik bilgilerini Azure anahtar kasası depolayın ve kimlik bilgilerini almak için anahtar kasası erişmek için MSI kullanın. 

İlk olarak, kimliğinizi bir anahtar kasası oluşturma ve anahtar Kasası'na bizim VM'in kimlik yetkisi vermek gerekiyor.   

1. Sol gezinti çubuğu üstünde seçin **kaynak oluşturma** > **güvenlik + kimlik** > **anahtar kasası**.  
2. Sağlayan bir **adı** yeni anahtar kasası için. 
3. Anahtar kasası ile aynı abonelik ve kaynak grubunda daha önce oluşturduğunuz VM bulun. 
4. Seçin **erişim ilkeleri** tıklatıp **yeni Ekle**. 
5. Şablondan Yapılandır seçin **gizli Yönetim**. 
6. Seçin **seçin asıl**ve arama alanında daha önce oluşturduğunuz VM adını girin.  Sonuç listesinden VM seçin ve tıklatın **seçin**. 
7. Tıklatın **Tamam** için yeni bir erişim ilkesi ekleme tamamlama ve **Tamam** erişim ilkesi seçimi tamamlamak için. 
8. Tıklatın **oluşturma** anahtar kasası oluşturmayı tamamlayın. 

    ![Alt görüntü metin](../media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)

Ardından, daha sonra VM içinde çalışan kodu kullanarak gizli alabilmeleri bir gizli anahtar Kasası'na ekleyin: 

1. Seçin **tüm kaynakları**, bulma ve oluşturduğunuz anahtar kasası seçin. 
2. Seçin **gizli**, tıklatıp **Ekle**. 
3. Seçin **el ile**, gelen **karşıya yükleme seçenekleri**. 
4. Bir ad ve değer için parolayı girin.  Değer, istediğiniz herhangi bir şey olabilir. 
5. Sona erme tarihi Temizle ve etkinleştirme tarihi bırakın ve bırakın **etkin** olarak **Evet**. 
6. Tıklatın **oluşturma** gizli anahtarı oluşturmak için. 
 
## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-retrieve-the-secret-from-the-key-vault"></a>VM kimliğini kullanarak bir erişim belirteci alın ve gizli anahtar Kasası'nı almak için kullanın  

Bu adımları tamamlamak için bir SSH istemcisi gerekir.  Windows kullanıyorsanız, SSH İstemcisi'nde kullanabileceğiniz [Linux için Windows alt](https://msdn.microsoft.com/commandline/wsl/about). SSH istemcinin anahtarları yapılandırma yardıma gereksinim duyarsanız, bkz: [kullanmak SSH anahtarları nasıl Windows Azure üzerinde ile](../../virtual-machines/linux/ssh-from-windows.md), veya [nasıl oluşturulacağı ve Linux VM'ler için Azure'da bir SSH ortak ve özel anahtar çifti kullanılmak](../../virtual-machines/linux/mac-create-ssh-keys.md).
 
1. Linux VM hem de Portalı'nda gidin **genel bakış**, tıklatın **Bağlan**. 
2. **Connect** tercih ettiğiniz SSH istemcisi ile VM. 
3. Terminal penceresinde CURL, kullanarak Azure anahtar kasası için bir erişim belirteci almak üzere yerel MSI uç nokta için bir isteği oluşturun.  
 
    Erişim belirteci CURL talebi aşağıdadır.  
    
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -H Metadata:true  
    ```
    Yanıt, Resource Manager erişim için gereken erişim belirteci içeriyor. 
    
    Yanıtı:  
    
    ```bash
    {"access_token":"eyJ0eXAi...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://vault.azure.net",
    "token_type":"Bearer"} 
    ```
    
    Azure anahtar Kasası'na kimliğini doğrulamak için bu erişim belirteci kullanabilirsiniz.  Sonraki CURL isteğinde bir gizli anahtar Kasası'nı okuyun CURL ve anahtar kasası REST API kullanarak gösterilmektedir.  İçinde yer anahtar kasası, URL gerekir **Essentials** bölümünü **genel bakış** anahtar kasası sayfasında.  Önceki aldığınız erişim belirtecini de gerekir. 
        
    ```bash
    curl https://<YOUR-KEY-VAULT-URL>/secrets/<secret-name>?api-version=2016-10-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```
    
    Yanıtı şuna benzeyecektir: 
    
    ```bash
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Anahtar Kasası'nı gizli alınan sonra bir adı ve parola gerektiren bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz.


## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](overview.md).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.




