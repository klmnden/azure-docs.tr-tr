---
title: B2B - Azure Active Directory için bir kimlik sağlayıcısı olarak Google ekleyin | Microsoft Docs
description: Azure AD uygulamanızın kendi Gmail hesabı ile oturum açmak konuk kullanıcıların sağlamak için Google ile federasyona eklemek
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 735c3db14963c1f3cfe700a97dee9fedb70e29f5
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67441118"
---
# <a name="add-google-as-an-identity-provider-for-b2b-guest-users-preview"></a>B2B Konuk kullanıcılar (Önizleme) için bir kimlik sağlayıcısı olarak Google Ekle

|     |
| --- |
| Google Federasyon, Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Google ile Federasyon ayarlayarak, paylaşılan uygulamaları ve kaynakları kendi Google hesapları ile Microsoft Accounts (MSA) veya Azure AD hesapları oluşturmak zorunda kalmadan oturum açmak, davet edilen kullanıcılar izin verebilirsiniz.  
> [!NOTE]
> Google Konuk kullanıcılarınızın Kiracı bağlamını içeren bir bağlantıyı kullanarak oturum açmanız gerekir (örneğin, `https://myapps.microsoft.com/?tenantid=<tenant id>` veya `https://portal.azure.com/<tenant id>`, doğrulanmış bir etki alanı söz konusu olduğunda veya `https://myapps.microsoft.com/<verified domain>.onmicrosoft.com`). Kiracı bağlam içerirler sürece uygulamalarına ve kaynaklarına doğrudan bağlantılar da çalışır. Konuk kullanıcıları hiçbir Kiracı bağlamına sahip uç noktaları kullanarak oturum şu anda belirleyemiyoruz. Örneğin, kullanarak `https://myapps.microsoft.com`, `https://portal.azure.com`, veya ekipler ortak uç nokta bir hataya neden olur.
 
## <a name="what-is-the-experience-for-the-google-user"></a>Google kullanıcı deneyimini nedir?
Bir Google Gmail kullanıcıya bir davet gönderdiğinizde, Konuk kullanıcının paylaşılan uygulamaları veya Kiracı bağlamını içeren bir bağlantı kullanarak kaynakları erişmelidir. Deneyimlerini olup, henüz Google'da oturum açmadıysanız bağlı olarak değişir:
  - Konuk kullanıcı için Google oturum açmamış, Google'da oturum açmanız istenir.
  - Konuk kullanıcı zaten Google'da oturum açtıysa, kullanmak istediğiniz hesabı seçmeniz istenir. Onları davet etmek için kullanılan hesap seçmeleri gerekir.

Konuk kullanıcı bir "çok uzun üstbilgi" hatası görürse, kendi tanımlama bilgilerini temizleyip deneyin veya bir özel veya gizli penceresi açın ve yeniden oturum açmayı deneyin.

![Google oturum açma sayfasını gösteren ekran görüntüsü](media/google-federation/google-sign-in.png)

## <a name="step-1-configure-a-google-developer-project"></a>1\. adım: Bir Google developer proje yapılandırma
İlk olarak bir istemci kimliği ve daha sonra Azure AD'ye ekleyebileceğiniz bir istemci gizli anahtarını almak amacıyla Google geliştiriciler konsolunda yeni bir proje oluşturun. 
1. Google API'leri Git https://console.developers.google.com ve Google hesabınızla oturum açın. Paylaşılan bir takım Google hesabı kullanmanızı öneririz.
2. Yeni bir proje oluşturun: Panoda seçin **proje oluştur**ve ardından **Oluştur**. Yeni Proje sayfa üzerinde girin bir **proje adı**ve ardından **Oluştur**.
   
   ![Yeni Proje sayfa için Google gösteren ekran görüntüsü](media/google-federation/google-new-project.png)

3. Yeni projeniz Proje menüsünde seçili olduğundan emin olun. Ardından, üst sol menüsünü açın ve seçin **API'leri ve Hizmetleri** > **kimlik bilgilerini**.

   ![Google API gösteren ekran görüntüsü seçeneği kimlik bilgileri](media/google-federation/google-api.png)
 
4. Seçin **OAuth onay ekranı** girin ve sekme bir **uygulama adı**. (Diğer ayarları bırakın.)

   ![Google OAuth onay ekranı seçeneğini gösteren ekran görüntüsü](media/google-federation/google-oauth-consent-screen.png)

5. Kaydırma **etki alanlarında yetkilendirilmiş** bölümünde ve microsoftonline.com girin.

   ![Yetkili etki alanı bölümünü gösteren ekran görüntüsü](media/google-federation/google-oauth-authorized-domains.png)

6. **Kaydet**’i seçin.

7. Seçin **kimlik bilgilerini** sekmesi. İçinde **kimlik bilgilerini oluştur** menüsünde seçin **OAuth istemcisi kimliği**.

   ![Kimlik bilgileri seçeneği Google API'leri gösteren ekran görüntüsü oluşturma](media/google-federation/google-api-credentials.png)

8. Altında **uygulama türü**, seçin **Web uygulaması**ve ardından altındaki **yetkili yeniden yönlendirme URI'leri**, aşağıdaki URI girin:
   - `https://login.microsoftonline.com` 
   - `https://login.microsoftonline.com/te/<directory id>/oauth2/authresp` <br>(burada `<directory id>` , dizin kimliği)
   
     > [!NOTE]
     > Dizin Kimliğinizi bulmak için Git https://portal.azure.com , altında **Azure Active Directory**, seçin **özellikleri** ve kopyalama **dizin kimliği**.

   ![Yetkili gösteren ekran görüntüsü yeniden yönlendirme URI'leri bölümü](media/google-federation/google-create-oauth-client-id.png)

9. **Oluştur**’u seçin. İstemci Kimliğini ve kimlik sağlayıcısı Azure AD portalında eklediğinizde kullanacağınız istemci gizli anahtarını kopyalayın.

   ![OAuth istemcisi Kimliğini ve istemci gizli anahtarı gösteren ekran görüntüsü](media/google-federation/google-auth-client-id-secret.png)

## <a name="step-2-configure-google-federation-in-azure-ad"></a>2\. adım: Azure AD'de Google Federasyonu yapılandırma 
Artık Azure AD portalında girerek veya PowerShell kullanarak Kimliğini ve istemci gizli anahtarı, Google istemci ayarlarsınız. Kendiniz bir Gmail adresi kullanarak ve çalışırken davetini davet edilen Google hesabınızla davet ederek Google Federasyon yapılandırmanızı test etmeyi unutmayın. 

#### <a name="to-configure-google-federation-in-the-azure-ad-portal"></a>Azure AD portalında Google Federasyon yapılandırmak için 
1. [Azure Portal](https://portal.azure.com) gidin. Sol bölmede **Azure Active Directory**’yi seçin. 
2. Seçin **kuruluş ilişkileri**.
3. Seçin **kimlik sağlayıcıları**ve ardından **Google** düğmesi.
4. Bir ad girin. Ardından, istemci Kimliğini ve istemci gizli anahtarı daha önce edindiğiniz girin. **Kaydet**’i seçin. 

   ![Ekleme Google kimlik sağlayıcısı sayfasını gösteren ekran görüntüsü](media/google-federation/google-identity-provider.png)

#### <a name="to-configure-google-federation-by-using-powershell"></a>PowerShell kullanarak Google Federasyon yapılandırmak için
1. Azure AD PowerShell graf modülü için en son sürümünü yükleyin ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Aşağıdaki komutu çalıştırın: `Connect-AzureAD`.
3. Oturum açma isteminde yönetilen genel yönetici hesabıyla oturum açın.  
4. Şu komutu çalıştırın: 
   
   `New-AzureADMSIdentityProvider -Type Google -Name Google -ClientId [Client ID] -ClientSecret [Client secret]`
 
   > [!NOTE]
   > İstemci kimliğini ve istemci gizli dizi içinde oluşturduğunuz bir uygulamadan kullanın "1. adım: Bir Google developer proje yapılandırma." Daha fazla bilgi için [yeni AzureADMSIdentityProvider](https://docs.microsoft.com/powershell/module/azuread/new-azureadmsidentityprovider?view=azureadps-2.0-preview) makalesi. 
 
## <a name="how-do-i-remove-google-federation"></a>Google Federasyon nasıl kaldırabilirim?
Google Federasyon kurulumunuzu silebilirsiniz. Bunu yaparsanız, davetini zaten yararlandınız Google Konuk kullanıcılar oturum açmanız mümkün olmayacaktır, ancak bunları erişim yeniden kaynaklarınızı dizinden silerek ve bunları yeniden davet tanıyabilirsiniz. 
 
### <a name="to-delete-google-federation-in-the-azure-ad-portal"></a>Azure AD portalında Google Federasyon silmek için: 
1. [Azure Portal](https://portal.azure.com) gidin. Sol bölmede **Azure Active Directory**’yi seçin. 
2. Seçin **kuruluş ilişkileri**.
3. Seçin **kimlik sağlayıcıları**.
4. Üzerinde **Google** satır, bağlam menüsünü ( **...** ) ve ardından **Sil**. 
   
   ![Sosyal kimlik sağlayıcısı silme seçeneğini gösteren ekran görüntüsü](media/google-federation/google-social-identity-providers.png)

1. Seçin **Evet** silme işlemini onaylamak için. 

### <a name="to-delete-google-federation-by-using-powershell"></a>PowerShell kullanarak Google Federasyon silmek için: 
1. Azure AD PowerShell graf modülü için en son sürümünü yükleyin ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. `Connect-AzureAD` öğesini çalıştırın.  
4. İsteminde oturum açma işleminde, yönetilen bir genel yönetici hesabıyla oturum açın.  
5. Aşağıdaki komutu girin:

    `Remove-AzureADMSIdentityProvider -Id Google-OAUTH`

   > [!NOTE]
   > Daha fazla bilgi için [Remove-AzureADMSIdentityProvider](https://docs.microsoft.com/powershell/module/azuread/Remove-AzureADMSIdentityProvider?view=azureadps-2.0-preview). 
