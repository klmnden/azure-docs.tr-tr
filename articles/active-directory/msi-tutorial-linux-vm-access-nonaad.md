---
title: "Azure anahtar kasası erişmek için bir Linux VM MSI kullanın"
description: "Azure Resource Manager erişmek için bir Linux VM yönetilen hizmet kimliği (MSI) kullanma sürecinde anlatan öğretici."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2017
ms.author: elkuzmen
ms.openlocfilehash: cd07cf69616fd33b6efcbcc3b2c97c025de67fe6
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="use-a-linux-vm-managed-service-identity-msi-to-access-azure-key-vault"></a>Azure anahtar kasası erişmek için bir Linux VM yönetilen hizmet kimliği (MSI) kullanın 

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Bu öğretici bir Linux sanal makine için Yönetilen hizmet kimliği (MSI) etkinleştirmek ve ardından Azure anahtar kasası erişmek için bu kimliği kullanan gösterilmektedir. Bir önyükleme hizmet veren, anahtar kasası istemci uygulamanızı Azure Active Directory (AD tarafından) güvenli olmayan kaynaklara erişmek için gizli anahtar'ı kullanacak şekilde sağlar. Yönetilen hizmet kimliği Azure tarafından otomatik olarak yönetilir ve Azure AD kimlik doğrulaması, kimlik bilgileri kodunuza eklemek zorunda kalmadan destekleyen hizmetler için kimlik doğrulaması sağlar. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir Linux sanal makinede MSI etkinleştir 
> * Bir anahtar kasasında depolanan bir gizli anahtar, VM erişim 
> * VM kimliğini kullanarak bir erişim belirteci alın ve gizli anahtar Kasası'nı almak için kullanın 
 


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="sign-in-to-azure"></a>Azure'da oturum açma
Oturum açmak için Azure portalında [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Linux sanal makine oluşturun

Bu öğretici için yeni bir Linux VM oluşturun. Mevcut bir VM'yi üzerinde MSI de etkinleştirebilirsiniz.

1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.
2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. Sanal makine bilgilerini girin. İçin **kimlik doğrulama türü**seçin **SSH ortak anahtarını** veya **parola**. Oluşturulan kimlik bilgileri, VM'ye oturum açmak izin verir.

    ![Alt görüntü metin](media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Seçin bir **abonelik** sanal makine açılır.
5. Yeni bir seçmek için **kaynak grubu** sanal makinenin oluşturulması, seçmek istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6. VM boyutunu seçin. Daha fazla boyutları görmek için seçin **tüm görüntüle** veya desteklenen disk türü filtresini değiştirin. Ayarlar sayfasında, Varsayılanları tutun ve **Tamam**.

## <a name="enable-msi-on-your-vm"></a>MSI VM üzerinde etkinleştir

Bir sanal makine MSI erişim belirteçleri, kimlik bilgileri kodunuza koyma gereksinimi olmadan Azure AD'den almanızı sağlar. Perde arkasında MSI etkinleştirme iki işlemi yapar: MSI VM uzantısı, VM yükler ve VM için MSI sağlar.  

1. Seçin **sanal makine** MSI etkinleştirmek istediğiniz.
2. Sol gezinti çubuğunda **yapılandırma**.
3. Gördüğünüz **yönetilen hizmet kimliği**. Kaydolun ve MSI etkinleştirmek için seçin **Evet**, devre dışı bırakmak istiyorsanız seçin No
4. Tıklattığınız olun **kaydetmek** yapılandırmayı kaydetmek için.

    ![Alt görüntü metin](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Bu bilgisayarda hangi uzantıların olduğunu denetlemek istiyorsanız **Linux VM**, tıklatın **uzantıları**. MSI etkinleştirilirse, **ManagedIdentityExtensionforLinux** listede görünür.

    ![Alt görüntü metin](media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)


## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>Bir anahtar kasasında depolanan bir gizli anahtar, VM erişim  

MSI kullanarak kodunuzu Azure Active Directory kimlik doğrulamasını destekleyen kaynaklar için kimlik doğrulaması için erişim belirteçleri elde edebilirsiniz. Ancak, tüm Azure Hizmetleri Azure AD kimlik doğrulamasını destekler. MSI bu hizmetleri ile kullanmak için hizmet kimlik bilgilerini Azure anahtar kasası depolayın ve kimlik bilgilerini almak için anahtar kasası erişmek için MSI kullanın. 

İlk olarak, kimliğinizi bir anahtar kasası oluşturma ve anahtar Kasası'na bizim VM'in kimlik yetkisi vermek gerekiyor.   

1. Sol gezinti çubuğu üstünde seçin **+ yeni** sonra **güvenlik + kimlik** sonra **anahtar kasası**.  
2. Sağlayan bir **adı** yeni anahtar kasası için. 
3. Anahtar kasası ile aynı abonelik ve kaynak grubunda daha önce oluşturduğunuz VM bulun. 
4. Seçin **erişim ilkeleri** tıklatıp **yeni Ekle**. 
5. Şablondan Yapılandır seçin **gizli Yönetim**. 
6. Seçin **seçin asıl**ve arama alanında daha önce oluşturduğunuz VM adını girin.  Sonuç listesinden VM seçin ve tıklatın **seçin**. 
7. Tıklatın **Tamam** için yeni bir erişim ilkesi ekleme tamamlama ve **Tamam** erişim ilkesi seçimi tamamlamak için. 
8. Tıklatın **oluşturma** anahtar kasası oluşturmayı tamamlayın. 

    ![Alt görüntü metin](media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)

Ardından, daha sonra VM içinde çalışan kodu kullanarak gizli alabilmeleri bir gizli anahtar Kasası'na ekleyin: 

1. Seçin **tüm kaynakları**, bulma ve oluşturduğunuz anahtar kasası seçin. 
2. Seçin **gizli**, tıklatıp **Ekle**. 
3. Seçin **el ile**, gelen **karşıya yükleme seçenekleri**. 
4. Bir ad ve değer için parolayı girin.  Değer, istediğiniz herhangi bir şey olabilir. 
5. Sona erme tarihi Temizle ve etkinleştirme tarihi bırakın ve bırakın **etkin** olarak **Evet**. 
6. Tıklatın **oluşturma** gizli anahtarı oluşturmak için. 
 
## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-retrieve-the-secret-from-the-key-vault"></a>VM kimliğini kullanarak bir erişim belirteci alın ve gizli anahtar Kasası'nı almak için kullanın  

Bu adımları tamamlamak için bir SSH istemcisi gerekir.  Windows kullanıyorsanız, SSH İstemcisi'nde kullanabileceğiniz [Linux için Windows alt](https://msdn.microsoft.com/commandline/wsl/about). SSH istemcinin anahtarları yapılandırma yardıma gereksinim duyarsanız, bkz: [kullanmak SSH anahtarları nasıl Windows Azure üzerinde ile](../virtual-machines/linux/ssh-from-windows.md), veya [nasıl oluşturulacağı ve Linux VM'ler için Azure'da bir SSH ortak ve özel anahtar çifti kullanılmak](../virtual-machines/linux/mac-create-ssh-keys.md).
 
1. Linux VM hem de Portalı'nda gidin **genel bakış**, tıklatın **Bağlan**. 
2. **Connect** tercih ettiğiniz SSH istemcisi ile VM. 
3. Terminal penceresinde CURL, kullanarak Azure anahtar kasası için bir erişim belirteci almak üzere yerel MSI uç nokta için bir isteği oluşturun.  
 
    Erişim belirteci CURL talebi aşağıdadır.  
    
    ```bash
    curl http://localhost:50342/oauth2/token --data "resource=https://vault.azure.net" -H Metadata:true  
    ```
    Yanıt, Resource Manager erişim için gereken erişim belirteci içeriyor. 
    
    Yanıtı:  
    
    ```bash
    {"access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IkhIQnlLVS0wRHFBcU1aaDZaRlBkMlZXYU90ZyIsImtpZCI6IkhIQnlLVS0wRHFBcU1aaDZaRlBkMlZXYU90ZyJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3LyIsImlhdCI6MTUwNDEyNjYyNywibmJmIjoxNTA0MTI2NjI3LCJleHAiOjE1MDQxMzA1MjcsImFpbyI6IlkyRmdZTGg2dENWSzRkSDlGWGtuZzgyQ21ZNVdBZ0E9IiwiYXBwaWQiOiI2ZjJmNmU2OS04MGExLTQ3NmEtOGRjZi1mOTgzZDZkMjUxYjgiLCJhcHBpZGFjciI6IjIiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwib2lkIjoiMTEyODJiZDgtMDNlMi00NGVhLTlmYjctZTQ1YjVmM2JmNzJlIiwic3ViIjoiMTEyODJiZDgtMDNlMi00NGVhLTlmYjctZTQ1YjVmM2JmNzJlIiwidGlkIjoiNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3IiwidXRpIjoib0U5T3JVZFJMMHVKSEw4UFdvOEJBQSIsInZlciI6IjEuMCJ9.J6KS7b9kFgDkegJ-Vfff19LMnu3Cfps4dL2uNGucb5M76rgDM5f73VO-19wZSRhQPxWmZLETzN3SljnIMQMkYWncp79MVdBud_xqXYyLdQpGkNinpKVJhTo1j1dY27U_Cjl4yvvpBTrtH3OX9gG0GtQs7PBFTTLznqcH3JR9f-bTSEN4wUhalaIPHPciVDtJI9I24_vvMfVqxkXOo6gkL0mEPfpXZRLwrBNd607AzX0KVmLFrwA1vYJnCV-sSV8bwTh2t6CVEj240t0iyeVWVc2usJ0NY2rxPzKd_UckQ_zzrECG3kS4vuYePKz6GqNJFVzm2w2c61lX0-O1CwvQ9w","refresh_token":"","expires_in":"3599","expires_on":"1504130527","not_before":"1504126627","resource":"https://vault.azure.net","token_type":"Bearer"} 
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

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](../active-directory/msi-overview.md).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.




