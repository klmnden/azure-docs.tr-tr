---
title: "Azure anahtar kasası erişmek için bir Windows VM MSI kullanın"
description: "Azure anahtar kasası erişim için bir Windows VM yönetilen hizmet kimliği (MSI) kullanma sürecinde anlatan öğretici."
services: active-directory
documentationcenter: 
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
ms.openlocfilehash: 5dd90d527afd81ad225b9693b126f48e48bde884
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-key-vault"></a>Azure anahtar kasası erişmek için bir Windows VM yönetilen hizmet kimliği (MSI) kullanın 

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Bu öğretici bir Windows sanal makine için Yönetilen hizmet kimliği (MSI) etkinleştirmek ve ardından Azure anahtar kasası erişmek için bu kimliği kullanan gösterilmektedir. Bir önyükleme hizmet veren, anahtar kasası istemci uygulamanızı Azure Active Directory (AD tarafından) güvenli olmayan kaynaklara erişmek için gizli anahtar'ı kullanacak şekilde sağlar. Yönetilen hizmet kimliği Azure tarafından otomatik olarak yönetilir ve Azure AD kimlik doğrulaması, kimlik bilgileri kodunuza eklemek zorunda kalmadan destekleyen hizmetler için kimlik doğrulaması sağlar. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:


> [!div class="checklist"]
> * Yönetilen hizmet kimliği Windows sanal makine üzerinde etkinleştir 
> * Bir anahtar kasasında depolanan bir gizli anahtar, VM erişim 
> * VM kimliğini kullanarak bir erişim belirteci alın ve gizli anahtar Kasası'nı almak için kullanın 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresindeki Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Windows sanal makine oluşturma

Bu öğretici için yeni bir Windows VM oluşturun. Mevcut bir VM'yi üzerinde MSI de etkinleştirebilirsiniz.

1.  Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.
2.  **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3.  Sanal makine bilgilerini girin. **Kullanıcıadı** ve **parola** için kullandığınız kimlik bilgileri İşte oluşturulan sanal makineye oturum açma.
4.  Uygun seçin **abonelik** sanal makine açılır.
5.  Yeni bir seçmek için **kaynak grubu** oluşturulması, seçmek için sanal makine için istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6.  VM boyutunu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

    ![Alt görüntü metin](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI VM üzerinde etkinleştir 

Bir sanal makine MSI erişim belirteçleri, kimlik bilgileri kodunuza koyma gereksinimi olmadan Azure AD'den almanızı sağlar. MSI etkinleştirme, sanal makine için yönetilen bir kimlik oluşturmak üzere Azure söyler. Perde arkasında MSI etkinleştirme iki işlemi yapar: MSI VM uzantısı, VM yükler ve MSI Azure Kaynak Yöneticisi'nde sağlar.

1.  Seçin **sanal makine** MSI etkinleştirmek istediğiniz.  
2.  Sol gezinti çubuğunda **yapılandırma**. 
3.  Gördüğünüz **yönetilen hizmet kimliği**. Kaydolun ve MSI etkinleştirmek için seçin **Evet**, devre dışı bırakmak istiyorsanız seçin No 
4.  Tıklattığınız olun **kaydetmek** yapılandırmayı kaydetmek için.  

    ![Alt görüntü metin](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Denetleyin ve bu VM uzantıları doğrulamak isterseniz, tıklatın **uzantıları**. MSI, ardından etkinse **ManagedIdentityExtensionforWindows** listede görüntülenir.

    ![Alt görüntü metin](media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>Bir anahtar kasasında depolanan bir gizli anahtar, VM erişim 
 
MSI kullanarak kodunuzu Azure AD kimlik doğrulamasını destekleyen kaynaklar için kimlik doğrulaması için erişim belirteçleri elde edebilirsiniz.  Ancak, tüm Azure Hizmetleri Azure AD kimlik doğrulamasını destekler. MSI bu hizmetleri ile kullanmak için hizmet kimlik bilgilerini Azure anahtar kasası depolayın ve kimlik bilgilerini almak için anahtar kasası erişmek için MSI kullanın. 

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
 
## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-retrieve-the-secret-from-the-key-vault"></a>VM kimliğini kullanarak bir erişim belirteci alın ve gizli anahtar Kasası'nı almak için kullanın  

PowerShell 4.3.1 yoksa veya üstü yüklü değilse gerekecektir [en son sürümünü karşıdan yükleyip](https://docs.microsoft.com/powershell/azure/overview).

İlk olarak, VM'ın MSI anahtar Kasası'na kimlik doğrulaması için erişim belirteci almak için kullanırız:
 
1. Portalı'nda gidin **sanal makineleri** ve Windows sanal makinenizi gidin ve buna **genel bakış**, tıklatın **Bağlan**.
2. Girin, **kullanıcıadı** ve **parola** oluştururken, eklediğiniz için **Windows VM**.  
3. Oluşturduğunuza göre bir **Uzak Masaüstü Bağlantısı** sanal makineyle PowerShell uzak oturum açın.  
4. PowerShell'de, belirteç yerel ana bilgisayar için belirli bağlantı noktasına VM için almak için Kiracı web isteği çağırır.  

    PowerShell isteği:
    
    ```powershell
    PS C:\> $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://vault.azure.net"} -Headers @{Metadata="true"} 
    ```
    
    Ardından, bir JavaScript nesne gösterimi (JSON) biçimlendirilmiş dize $response nesnesi olarak depolanan tam yanıtı ayıklayın.  
    
    ```powershell
    PS C:\> $content = $response.Content | ConvertFrom-Json 
    ```
    
    Ardından, erişim belirteci yanıttan ayıklayın.  
    
    ```powershell
    PS C:\> $KeyVaultToken = $content.access_token 
    ```
    
    Son olarak, yetkilendirme üst bilgisinde erişim belirteci geçirme anahtar kasası içinde daha önce oluşturduğunuz parolayı almak için PowerShell'ın Invoke-WebRequest komutunu kullanın.  İçinde yer anahtar kasası, URL gerekir **Essentials** bölümünü **genel bakış** anahtar kasası sayfasında.  
    
    ```powershell
    PS C:\> (Invoke-WebRequest -Uri https://<your-key-vault-URL>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}).content 
    ```
    
    Yanıtı şuna benzeyecektir: 
    
    ```powershell
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Anahtar Kasası'nı gizli alınan sonra bir adı ve parola gerektiren bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](../active-directory/msi-overview.md).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.
