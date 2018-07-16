---
title: Azure Key Vault’a erişmek için Windows VM MSI kullanma
description: Windows VM Yönetilen Hizmet Kimliği (MSI) kullanarak Azure Key Vault'a erişme işleminde size yol gösteren bir öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: 81509108060b636e47154a8c375f5569cac73648
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37902743"
---
# <a name="tutorial-use-a-windows-vm-managed-service-identity-msi-to-access-azure-key-vault"></a>Öğretici: Azure Key Vault'a erişmek için Windows VM Yönetilen Hizmet Kimliği (MSI) kullanma 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğreticide, Windows Sanal Makinesi için Yönetilen Hizmet Kimliği'ni (MSI) etkinleştirme ve ardından bu kimliği kullanarak Azure Key Vault'a erişme işlemleri gösterilir. Önyükleme işlevi gören Key Vault, istemci uygulamanızın gizli diziyi kullanarak Azure Active Directory (AD) tarafından güvenlik altına alınmamış kaynaklara erişmenize olanak sağlar. Yönetilen Hizmet Kimlikleri Azure tarafından otomatik olarak yönetilir kodunuza kimlik bilgileri girmenize gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmanıza olanak tanır. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:


> [!div class="checklist"]
> * Windows Sanal Makinesinde Yönetilen Hizmet Kimliği'ni etkinleştirme 
> * VM'nize Key Vault'ta depolanan gizli diziye erişim verme 
> * VM kimliği kullanarak erişim belirteci alma ve Key Vault'tan gizli diziyi almak için bunu kullanma 

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda Windows sanal makinesi oluşturma

Bu öğretici için, yeni bir Windows VM oluşturuyoruz. Ayrıca mevcut bir VM'de MSI'yi etkinleştirebilirsiniz.

1.  Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesine tıklayın.
2.  **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3.  Sanal makine bilgilerini girin. Burada oluşturulan **Kullanıcı adı** ve **Parola**, sanal makinede oturum açmak için kullandığınız kimlik bilgileridir.
4.  Açılan listede sanal makine için uygun **Aboneliği** seçin.
5.  İçinde sanal makinenin oluşturulmasını istediğiniz yeni bir **Kaynak Grubu** seçmek için, **Yeni Oluştur**'u seçin. İşlem tamamlandığında **Tamam**’a tıklayın.
6.  VM'nin boyutunu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

    ![Alternatif resim metni](../media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>VM'nizde MSI'yi etkinleştirme 

Sanal Makine MSI'si kodunuza kimlik bilgileri yerleştirmeniz gerekmeden Azure AD'den erişim belirteçlerini almanıza olanak tanır. MSI'nin etkinleştirilmesi Azure'a Sanal Makineniz için bir yönetilen kimlik oluşturmasını bildirir. MSI'nin etkinleştirilmesi arka planda iki işlem yapar: yönetilen kimliğini oluşturmak için VM'nizi Azure Active Directory'ye kaydeder ve kimliği VM'de yapılandırır.

1.  MSI'yi etkinleştirmek istediğiniz **Sanal Makine**'yi seçin.  
2.  Sol gezinti çubuğunda **Yapılandırma**'ya tıklayın. 
3.  **Yönetilen Hizmet Kimliği**'ni görürsünüz. MSI'yi kaydetmek ve etkinleştirmek için **Evet**'i seçin, devre dışı bırakmak istiyorsanız Hayır'ı seçin. 
4.  Yapılandırmayı kaydetmek için **Kaydet**’e tıkladığınızdan emin olun.  

    ![Alternatif resim metni](../media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>VM'nize Key Vault'ta depolanan Gizli Diziye erişim verme 
 
MSI kullanıldığında kodunuz Azure AD kimlik doğrulamasını destekleyen kaynaklarda kimlik doğrulaması yapmak için belirteçlere erişebilir.  Bununla birlikte, Azure hizmetlerinin tümü Azure AD kimlik doğrulamasını desteklemez. Söz konusu hizmetlerle MSI kullanmak için, hizmet kimlik bilgilerini Azure Key Vault'ta depolayın ve MSI'yi kullanarak Key Vault'a erişip kimlik bilgilerini alın. 

İlk olarak, Key Vault'u oluşturmalı ve VM'mize Key Vault üzerinde kimlik erişimi vermeliyiz.   

1. Sol gezinti çubuğunun üst kısmında **Kaynak oluştur** > **Güvenlik + Kimlik** > **Key Vault**'u seçin.  
2. Yeni Key Vault için bir **Ad** belirtin. 
3. Key Vault'u daha önce oluşturduğunuz VM'yle aynı aboneliğe ve kaynak grubuna yerleştirin. 
4. **Erişim ilkeleri**’ni seçin ve **Yeni ekle**'ye tıklayın. 
5. Şablondan yapılandır'da **Gizli Dizi Yönetimi**'ni seçin. 
6. **Sorumlu Seç**'i seçin ve arama alanına daha önce oluşturduğunuz VM'nin adını girin.  Sonuç listesinde VM'yi seçin ve **Seç**'e tıklayın. 
7. Yeni erişim ilkesini ekleme işlemini tamamlamak için **Tamam**'a tıklayın ve erişim ilkesi seçimini bitirmek için de **Tamam**'a tıklayın. 
8. Key Vault oluşturmayı tamamlamak için **Oluştur**'a tıklayın. 

    ![Alternatif resim metni](../media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)


Ardından, Key Vault'a bir gizli dizi ekleyin; böylelikle VM'nizde çalıştırılan kodu kullanarak daha sonra gizli diziyi alabilirsiniz: 

1. **Tüm Kaynaklar**'ı seçin, sonra da oluşturduğunuz Key Vault'u bulun ve seçin. 
2. **Gizli Diziler**'i seçin ve **Ekle**'ye tıklayın. 
3. **Karşıya yükleme seçenekleri**'nden **El ile** seçeneğini belirtin. 
4. Gizli dizi için bir ad ve değer girin.  Değer, istediğiniz herhangi bir şey olabilir. 
5. Etkinleştirme tarihi ile sona erme tarihini boş bırakın ve **Etkin** seçeneğini **Evet** değerinde bırakın. 
6. Gizli diziyi oluşturmak için **Oluştur**'a tıklayın. 
 
## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-retrieve-the-secret-from-the-key-vault"></a>VM kimliğini kullanarak erişim belirteci alma ve Key Vault'tan gizli diziyi almak için bunu kullanma  

PowerShell 4.3.1 veya üstünü yüklemediyseniz, [en son sürümü indirip yüklemeniz gerekir](https://docs.microsoft.com/powershell/azure/overview).

İlk olarak, Key Vault'ta kimlik doğrulaması yapmak üzere erişim belirteci almak için VM’nin MSI'sini kullanırız:
 
1. Portalda, **Sanal Makineler**'e ve Windows sanal makinenize gidin, ardından **Genel Bakış**'ta **Bağlan**'a tıklayın.
2. **Windows VM'sini** oluştururken eklendiğiniz hesabın **Kullanıcı adı** ve **Parola** değerlerini girin.  
3. Artık sanal makineyle **Uzak Masaüstü Bağlantısı**'nı oluşturduğunuza göre, uzak oturumda PowerShell'i açın.  
4. PowerShell'de, VM için belirtilen bağlantı noktasında yerel konağın belirtecini almak üzere kiracıda web isteğini çağırın.  

    PowerShell isteği:
    
    ```powershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -Method GET -Headers @{Metadata="true"} 
    ```
    
    Ardından, $response nesnesinde JavaScript Nesne Gösterimi (JSON) biçimlendirilmiş dizesi olarak depolanan tam yanıtı ayıklayın.  
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json 
    ```
    
    Ardından, yanıttan erişim belirtecini ayıklayın.  
    
    ```powershell
    $KeyVaultToken = $content.access_token 
    ```
    
    Son olarak, PowerShell’in Invoke-WebRequest komutunu kullanarak Authorization üst bilgisindeki erişim belirtecini geçirerek Key Vault'ta daha önce oluşturmuş olduğunuz gizli bilgiyi alın.  Key Vault'unuzun URL'sine ihtiyacınız olacaktır. Bu URL, Key Vault'un **Genel Bakış** sayfasındaki **Temel Parçalar** bölümünde yer alır.  
    
    ```powershell
    (Invoke-WebRequest -Uri https://<your-key-vault-URL>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}).content 
    ```
    
    Yanıt şöyle görünür: 
    
    ```powershell
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Key Vault'tan gizli diziyi aldıktan sonra, bunu kullanarak ad ve parola gerektiren bir hizmette kimlik doğrulaması yapabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Key Vault'a erişmek için Yönetilen Hizmet Kimliği oluşturmayı öğrendiniz.  Azure Key Vault hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
>[Azure Anahtar Kasası.](/azure/key-vault/key-vault-whatis)
