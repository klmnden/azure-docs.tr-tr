---
title: Azure Active Directory B2B için bir kimlik sağlayıcısı olarak Google ekleyin | Microsoft Docs
description: Azure AD uygulamanızın kendi Gmail hesabı ile oturum açmak konuk kullanıcıların sağlamak için Google ile federasyona eklemek
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 08/20/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: mal
ms.openlocfilehash: d36a4071dbbfb52e22a4e0ecc850da68ebeae6e5
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888126"
---
# <a name="add-google-as-an-identity-provider-for-b2b-guest-users"></a>Google B2B Konuk kullanıcılar için kimlik sağlayıcısı Ekle

Google ile Federasyon ayarlayarak, paylaşılan uygulamaları ve kaynakları kendi Google hesapları ile Microsoft Accounts (MSA) veya Azure AD hesapları oluşturmak zorunda kalmadan oturum açmak, davet edilen kullanıcılar izin verebilirsiniz.  
> [!NOTE]
> Örneğin Kiracı bağlamı içeren bir bağlantı kullanarak Google Konuk kullanıcılarınız oturum gerekir `https://myapps.microsoft.com/?tenantid=<tenant id>`. Kiracı bağlam içerirler sürece uygulamalarına ve kaynaklarına doğrudan bağlantılar da çalışır. Konuk kullanıcıları hiçbir Kiracı bağlamına sahip uç noktaları kullanarak oturum şu anda belirleyemiyoruz. Örneğin, kullanarak `https://myapps.microsoft.com`, `https://portal.azure.com`, veya ekipler ortak uç nokta bir hataya neden olur.
 
## <a name="what-is-the-experience-for-the-google-user"></a>Google kullanıcı deneyimini nedir?
Bir Google Gmail kullanıcıya bir davet gönderdiğinizde, Konuk kullanıcının paylaşılan uygulamaları veya Kiracı bağlamını içeren bir bağlantı kullanarak kaynakları erişmelidir. Deneyimlerini olup, henüz Google'da oturum açmadıysanız bağlı olarak değişir:
  - Konuk kullanıcı için Google oturum açmamış, Google'da oturum açmanız istenir.
  - Konuk kullanıcı zaten Google'da oturum açtıysa, kullanmak istediğiniz hesabı seçmeniz istenir. Onları davet etmek için kullanılan hesap seçmeleri gerekir.

Konuk kullanıcı bir "çok uzun üstbilgi" hatası görürse, kendi tanımlama bilgilerini temizleyip deneyin veya bir özel veya gizli penceresi açın ve yeniden oturum açmayı deneyin.

![Google ile oturum aç](media/google-federation/google-sign-in.png)

## <a name="step-1-configure-a-google-developer-project"></a>1. adım: bir Google developer proje yapılandırma
İlk olarak bir istemci kimliği ve daha sonra Azure AD'ye ekleyebileceğiniz bir istemci gizli anahtarını almak amacıyla Google geliştiriciler konsolunda yeni bir proje oluşturun. 
1. Google API'leri Git https://console.developers.google.comve Google hesabınızla oturum açın. Paylaşılan bir takım Google hesabı kullanmanızı öneririz.
2. Yeni bir proje oluşturun: panodaki, select **proje oluştur**ve ardından **Oluştur**. Yeni Proje sayfa üzerinde girin bir **proje adı**ve ardından **Oluştur**.
   
   ![Yeni Google proje](media/google-federation/google-new-project.png)

3. Yeni projeniz Proje menüsünde seçili olduğundan emin olun. Ardından, üst sol menüsünü açın ve seçin **API'leri ve Hizmetleri** > **kimlik bilgilerini**.

   ![Google API kimlik bilgileri](media/google-federation/google-api.png)
 
4. Seçin **Oauth onay ekranı** girin ve sekme bir **kullanıcılara gösterilen ürün adı**. (Diğer ayarları bırakın.) **Kaydet**’i seçin.

   ![Google OAuth onay ekranı](media/google-federation/google-oauth-consent-screen.png)

5. Seçin **kimlik bilgilerini** sekmesi. İçinde **kimlik bilgilerini oluştur** menüsünde seçin **OAuth istemcisi kimliği**.

   ![Google API kimlik bilgileri](media/google-federation/google-api-credentials.png)

6. Altında **uygulama türü**, seçin **Web uygulaması**ve ardından altındaki **yetkili yeniden yönlendirme URI'leri**, aşağıdaki URI girin:
   - `https://login.microsoftonline.com` 
   - `https://login.microsoftonline.com/te/<directory id>/oauth2/authresp` <br>(burada `<directory id>` , dizin kimliği)
   
    > [!NOTE]
    > Dizin Kimliğinizi bulmak için Git https://portal.azure.com, altında **Azure Active Directory**, seçin **özellikleri** ve kopyalama **dizin kimliği**.

   ![OAuth istemcisi kimliği oluşturma](media/google-federation/google-create-oauth-client-id.png)

7. **Oluştur**’u seçin. İstemci Kimliğini ve kimlik sağlayıcısı Azure AD portalında eklediğinizde kullanacağınız istemci gizli anahtarını kopyalayın.

   ![OAuth istemci kimliği ve istemci gizli anahtarı](media/google-federation/google-auth-client-id-secret.png)

## <a name="step-2-configure-google-federation-in-azure-ad"></a>2. adım: Azure AD'de Google Federasyon yapılandırma 
Artık Azure AD portalında girerek veya PowerShell kullanarak Kimliğini ve istemci gizli anahtarı, Google istemci ayarlarsınız. Kendiniz bir Gmail adresi kullanarak ve çalışırken davetini davet edilen Google hesabınızla davet ederek Google Federasyon yapılandırmanızı test etmeyi unutmayın. 

#### <a name="to-configure-google-federation-in-the-azure-ad-portal"></a>Azure AD portalında Google Federasyon yapılandırmak için 
1. [Azure Portal](https://portal.azure.com) gidin. Sol bölmede seçin **Azure Active Directory**. 
2. Seçin **kuruluş ilişkileri**.
3. Seçin **kimlik sağlayıcıları**ve ardından **Google** düğmesi.
4. Bir ad girin. Ardından, istemci Kimliğini ve istemci gizli anahtarı daha önce edindiğiniz girin. **Kaydet**’i seçin. 

   ![Google kimlik sağlayıcısı ekle](media/google-federation/google-identity-provider.png)

#### <a name="to-configure-google-federation-by-using-powershell"></a>PowerShell kullanarak Google Federasyon yapılandırmak için
1. Azure AD PowerShell graf modülü için en son sürümünü yükleyin ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Aşağıdaki komutu çalıştırın: `Connect-AzureAD`.
3. Oturum açma isteminde yönetilen genel yönetici hesabıyla oturum açın.  
4. Şu komutu çalıştırın: 
   
   `New-AzureADMSIdentityProvider -Type Google -Name Google -ClientId [Client ID] -ClientSecret [Client secret]`
 
   > [!NOTE]
   > İstemci kimliğini ve istemci gizli dizi içinde oluşturduğunuz bir uygulamadan kullanın "1. adım: bir Google developer proje yapılandırma." Daha fazla bilgi için [yeni AzureADMSIdentityProvider](https://docs.microsoft.com/en-us/powershell/module/azuread/new-azureadmsidentityprovider?view=azureadps-2.0-preview) makalesi. 
 
## <a name="how-do-i-remove-google-federation"></a>Google Federasyon nasıl kaldırabilirim?
Google Federasyon kurulumunuzu silebilirsiniz. Bunu yaparsanız, davetini zaten yararlandınız Google Konuk kullanıcılar oturum açmanız mümkün olmayacaktır, ancak bunları erişim yeniden kaynaklarınızı dizinden silerek ve bunları yeniden davet tanıyabilirsiniz. 
 
### <a name="to-delete-google-federation-in-the-azure-ad-portal"></a>Azure AD portalında Google Federasyon silmek için: 
1. [Azure Portal](https://portal.azure.com) gidin. Sol bölmede seçin **Azure Active Directory**. 
2. Seçin **kuruluş ilişkileri**.
3. Seçin **kimlik sağlayıcıları**ve ardından **Google** düğmesi.
4. Seçin **Google**ve ardından **Sil**. 
   
   ![Sosyal kimlik sağlayıcısı silindi](media/google-federation/google-social-identity-providers.png)

1. Seçin **Evet** silme işlemini onaylamak için. 

### <a name="to-delete-google-federation-by-using-powershell"></a>PowerShell kullanarak Google Federasyon silmek için: 
1. Azure AD PowerShell graf modülü için en son sürümünü yükleyin ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. `Connect-AzureAD` öğesini çalıştırın.  
4. İsteminde oturum açma işleminde, yönetilen bir genel yönetici hesabıyla oturum açın.  
5. Aşağıdaki komutu girin:

    `Remove-AzureADMSIdentityProvider -Id Google-OAUTH`

   > [!NOTE]
   > Daha fazla bilgi için [Remove-AzureADMSIdentityProvider](https://docs.microsoft.com/en-us/powershell/module/azuread/Remove-AzureADMSIdentityProvider?view=azureadps-2.0-preview). 