---
title: Azure Key Vault'a erişmek için bir Windows VM MSI kullanma
description: Öğretici Azure Key Vault'a erişmesi için bir Windows VM yönetilen hizmet kimliği (MSI) kullanma işlemi gösterilmektedir.
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
ms.openlocfilehash: b3d334edd770ac381a7e0ae6aaa1a9db8c91b961
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39002948"
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-key-vault"></a>Azure Key Vault'a erişmesi için bir Windows VM yönetilen hizmet kimliği (MSI) kullanma 

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğreticide bir Windows sanal makinesi için Yönetilen hizmet kimliği (MSI) etkinleştirdikten sonra Azure Key Vault'a erişmek için bu kimliği kullanın gösterilmektedir. Bir önyükleme hizmet veren, anahtar kasası istemci uygulamanızı Azure Active Directory (AD tarafından) güvenli olmayan kaynaklara erişmek için parola kullanacak şekilde sağlar. Yönetilen hizmet kimlikleri, Azure tarafından otomatik olarak yönetilir ve Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuza eklemek zorunda kalmadan destekleyen hizmetler için kimlik doğrulaması sağlar. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:


> [!div class="checklist"]
> * Bir Windows sanal makine üzerinde yönetilen hizmet kimliğini etkinleştirme 
> * Bir anahtar Kasası'nda depolanan gizli dizi, VM erişimi verme 
> * VM kimliğini kullanarak bir erişim belirteci alma ve gizli anahtar Kasasından almak için kullanın 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Windows sanal makine oluşturun

Bu öğretici için yeni bir Windows VM'yi oluştururuz. Mevcut VM'yi MSI de etkinleştirebilirsiniz.

1.  Tıklayın **kaynak Oluştur** sol üst köşesinde Azure portal'ın üzerinde.
2.  **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3.  Sanal makine bilgilerini girin. **Kullanıcıadı** ve **parola** için kullandığınız kimlik bilgilerini İşte oluşturulan sanal makineye oturum açma.
4.  Uygun seçin **abonelik** sanal makinenin açılır.
5.  Yeni bir seçilecek **kaynak grubu** oluşturulması, seçmek üzere sanal makineye istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6.  Sanal makine için boyutu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

    ![Alt resim metni](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Vm'nizde MSI etkinleştir 

Bir sanal makine MSI, kimlik bilgilerini kodunuza koyma gereksinimi olmadan Azure AD'den erişim belirteci alma olanak tanır. MSI etkinleştirmesine sanal makineniz için yönetilen bir kimlik oluşturmak için Azure'da söyler. Perde MSI etkinleştirmesine iki şeyi yapar: VM'NİZDE MSI VM uzantısı yükler ve Azure Resource Manager'daki MSI sağlar.

1.  Seçin **sanal makine** MSI etkinleştirmek istiyorsanız.  
2.  Sol gezinti çubuğunda Koruma'ya tıklayın **yapılandırma**. 
3.  Gördüğünüz **yönetilen hizmet kimliği**. Kaydolun ve MSI etkinleştirmek için **Evet**, devre dışı bırakmak istiyorsanız seçin No 
4.  Tıkladığınız olun **Kaydet** yapılandırmayı kaydetmek için.  

    ![Alt resim metni](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Denetleyin ve bu VM üzerinde hangi uzantıları doğrulamak isterseniz tıklayın **uzantıları**. MSI, ardından etkinse **ManagedIdentityExtensionforWindows** listesinde görünür.

    ![Alt resim metni](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>Bir anahtar Kasası'nda depolanan gizli dizi, VM erişim 
 
MSI kullanarak kodunuzu Azure AD kimlik doğrulamasını destekleyen kaynakların kimliğini doğrulamak için erişim belirteçleri elde edebilirsiniz.  Ancak, tüm Azure Hizmetleri, Azure AD kimlik doğrulamasını destekler. Bu hizmetlerle MSI kullanmak için Azure anahtar Kasası'nda hizmet kimlik bilgilerini depolamak ve kimlik bilgilerini almak için anahtar kasasına erişmek için MSI kullanma. 

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
 
## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-retrieve-the-secret-from-the-key-vault"></a>VM kimliğini kullanarak bir erişim belirteci alma ve gizli anahtar Kasasından almak için kullanın  

PowerShell 4.3.1 yoksa veya sonraki sürümünün kurulu ise gerekecektir [en son sürümü indirmek ve yüklemek](https://docs.microsoft.com/powershell/azure/overview).

İlk olarak, sanal makinenin MSI anahtar Kasası'na kimlik doğrulaması için erişim belirteci almak için kullanırız:
 
1. Portalda gidin **sanal makineler** ve Windows sanal makinenizi gidin ve buna **genel bakış**, tıklayın **Connect**.
2. Girin, **kullanıcıadı** ve **parola** oluşturduğunuz zaman, eklediğiniz için **Windows VM**.  
3. Sizin oluşturduğunuz bir **Uzak Masaüstü Bağlantısı** sanal makineyle PowerShell uzak oturumu açın.  
4. PowerShell'de, belirteç yerel ana bilgisayar için belirli bağlantı noktasına VM için almak üzere Kiracı web isteği çağırır.  

    PowerShell isteği:
    
    ```powershell
    PS C:\> $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://vault.azure.net"} -Headers @{Metadata="true"} 
    ```
    
    Ardından, $response nesneyi JavaScript nesne gösterimi (JSON) biçimlendirilen dizesinde depolanan tam yanıtı ayıklayın.  
    
    ```powershell
    PS C:\> $content = $response.Content | ConvertFrom-Json 
    ```
    
    Ardından, erişim belirtecini yanıttan ayıklayın.  
    
    ```powershell
    PS C:\> $KeyVaultToken = $content.access_token 
    ```
    
    Son olarak, yetkilendirme üst bilgisinde erişim belirteci geçirme anahtar Kasası'nda daha önce oluşturduğunuz parolayı almak için PowerShell'in Invoke-WebRequest komutunu kullanın.  İçinde anahtar kasası, URL'si gerekir **Essentials** bölümünü **genel bakış** anahtar kasası sayfasında.  
    
    ```powershell
    PS C:\> (Invoke-WebRequest -Uri https://<your-key-vault-URL>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}).content 
    ```
    
    Yanıt şöyle görünür: 
    
    ```powershell
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Key Vault'tan gizli dizi alınan sonra adı ve parola gerektiren bir hizmet için kimlik doğrulaması için kullanabilirsiniz. 

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz. [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.
