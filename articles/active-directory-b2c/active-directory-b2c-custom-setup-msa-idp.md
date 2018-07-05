---
title: Azure Active Directory B2C'de özel ilkeleri kullanarak bir kimlik sağlayıcısı olarak Microsoft hesabı (MSA) ekleme | Microsoft Docs
description: Openıd Connect (OIDC) protokolünü kullanarak kimlik sağlayıcısı olarak Microsoft kullanarak örneği.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7a83ace83176d75abdac03b354c4c4ac71eb4238
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450374"
---
# <a name="azure-active-directory-b2c-add-microsoft-account-msa-as-an-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Özel ilkeleri kullanarak bir kimlik sağlayıcısı olarak Microsoft hesabı (MSA) ekleyin.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede Microsoft hesabı (MSA) kullanıcılar için oturum açma kullanımının nasıl etkinleştirileceği gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Önkoşullar
Bölümündeki adımları tamamlamanız [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

Bu adımlar şunlardır:

1.  Bir Microsoft hesabı uygulaması oluşturuluyor.
2.  Azure AD B2C'ye Microsoft hesabı uygulama anahtarı ekleme
3.  Bir ilke için talep sağlayıcısı ekleniyor
4.  Kullanıcı yolculuğu için Microsoft Account talep sağlayıcısı kaydediliyor
5.  Kiracı için Azure AD B2C İlkesi karşıya yükleme ve test

## <a name="create-a-microsoft-account-application"></a>Bir Microsoft hesabı uygulaması oluşturun
Microsoft hesabı bir Azure Active Directory (Azure AD) B2C kimlik sağlayıcısı olarak kullanmak için bir Microsoft hesabı uygulaması oluşturun ve doğru parametreleri sağlamanız gerekir. Bir Microsoft hesabı gerekir. Yoksa, ziyaret [ https://www.live.com/ ](https://www.live.com/).

1.  Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) ve Microsoft hesabı kimlik bilgilerinizle oturum açın.
2.  Tıklayın **uygulama ekleme**.

    ![Microsoft hesabı - uygulama ekleme](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-new-app.png)

3.  Sağlayan bir **adı** , uygulamanız için **ilgili kişi e-posta**, işaretini kaldırın **bize başlamanıza yardımcı olmak** tıklatıp **Oluştur**.

    ![Microsoft hesabı - uygulama kaydı](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-name.png)

4.  Değerini kopyalayın **uygulama kimliği**. Kiracınızdaki bir kimlik sağlayıcısı olarak Microsoft hesabı yapılandırmak için ihtiyacınız.

    ![Microsoft hesabı - uygulama kimliği değerini Kopyala](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-id.png)

5.  Tıklayarak **platformu Ekle**

    ![Microsoft hesabı - platformu Ekle](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-platform.png)

6.  Platform listeden seçin **Web**.

    ![Microsoft hesabı - platform listeden Web seçin](media/active-directory-b2c-custom-setup-ms-account-idp/msa-web.png)

7.  Girin `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **yeniden yönlendirme URI'leri** alan. Değiştirin **{tenant}** kiracınızın adı (örneğin, contosob2c.onmicrosoft.com) ile.

    ![Microsoft hesabı - kümesi yeniden yönlendirme URL'leri](media/active-directory-b2c-custom-setup-ms-account-idp/msa-redirect-url.png)

8.  Tıklayarak **yeni parola oluştur** altında **uygulama gizli dizilerini** bölümü. Ekranda görüntülenen yeni parolayı kopyalayın. Kiracınızdaki bir kimlik sağlayıcısı olarak Microsoft hesabı yapılandırmak için ihtiyacınız. Bu parola, bir önemli güvenlik kimlik bilgisidir.

    ![Microsoft hesabı - yeni parola oluşturma](media/active-directory-b2c-custom-setup-ms-account-idp/msa-generate-new-password.png)

    ![Microsoft hesabı - kopyalama yeni parola](media/active-directory-b2c-custom-setup-ms-account-idp/msa-new-password.png)

9.  İfadesini içeren kutuyu **Canlı SDK desteği** altında **Gelişmiş Seçenekler** bölümü. **Kaydet**’e tıklayın.

    ![Microsoft hesabı - Canlı SDK desteği](media/active-directory-b2c-custom-setup-ms-account-idp/msa-live-sdk-support.png)

## <a name="add-the-microsoft-account-application-key-to-azure-ad-b2c"></a>Microsoft hesabı uygulama anahtarı Azure AD B2C'ye ekleyin.
Microsoft hesapları ile federasyon güven Azure AD B2C'yi uygulama adına Microsoft hesabı istemci gizli anahtarı gerektirir. Azure AD B2C kiracısında, Microsoft hesabı uygulaması secert depolamak gerekir:   

1.  Azure AD B2C kiracınıza gitmek ve seçin **B2C ayarlarını** > **kimlik deneyimi çerçevesi**
2.  Seçin **ilke anahtarları** kiracınızda kullanılabilir tuşlarını görmek için.
3.  Tıklayın **+ Ekle**.
4.  İçin **seçenekleri**, kullanın **el ile**.
5.  İçin **adı**, kullanın `MSASecret`.  
    Önek `B2C_1A_` otomatik olarak eklenebilir.
6.  İçinde **gizli** kutusuna, gelen Microsoft uygulama gizli anahtarı girin https://apps.dev.microsoft.com
7.  İçin **anahtar kullanımı**, kullanın **imza**.
8.  **Oluştur**'a tıklayın
9.  Anahtar oluşturduğunuzu doğrulayın `B2C_1A_MSASecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Uzantı ilkenizde bir talep Sağlayıcı Ekle
Microsoft Account kullanarak oturum açmalarını istiyorsanız, bir talep sağlayıcısı olarak Microsoft Account tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C ile iletişim kuran bir uç nokta belirtmeniz gerekir. Uç nokta, Azure AD B2C tarafından belirli bir kullanıcı yapıldığını doğrulamak için kullanılan bir talepler kümesi sağlar.

Ekleyerek bir talep sağlayıcısı olarak, Microsoft Account tanımlamak `<ClaimsProvider>` ilke uzantısının düğümünde:

1.  Uzantı ilke dosyası (TrustFrameworkExtensions.xml) çalışma dizininizi açın. Bir XML düzenleyicisini gerekiyorsa [Visual Studio Code deneyin](https://code.visualstudio.com/download), basit bir platformlar arası Düzenleyici.
2.  Bulma `<ClaimsProviders>` bölümü
3.  XML kod parçacığı altında aşağıdaki ekleyin `ClaimsProviders` öğesi:

    ```xml
<ClaimsProvider>
    <Domain>live.com</Domain>
    <DisplayName>Microsoft Account</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="MSA-OIDC">
        <DisplayName>Microsoft Account</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <Metadata>
        <Item Key="ProviderName">https://login.live.com</Item>
        <Item Key="METADATA">https://login.live.com/.well-known/openid-configuration</Item>
        <Item Key="response_types">code</Item>
        <Item Key="response_mode">form_post</Item>
        <Item Key="scope">openid profile email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Microsoft application client id</Item>
        </Metadata>
    <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
    </CryptographicKeys>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="email" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

4.  Değiştirin `client_id` değeri ile Microsoft Account uygulama istemci kimliği

5.  Dosyayı kaydedin.

## <a name="register-the-microsoft-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Yedekleme sağlayıcısı imzalamak için Microsoft Account talep kaydetme veya oturum açma kullanıcı yolculuğu

Bu noktada, kimlik sağlayıcısı ayarlandı, ancak oturumu-kaydolma/oturum açma ekranları hiçbirinde kullanılabilir değil. Artık Microsoft Account kimlik sağlayıcısı için kullanıcı eklemek gerekiyor `SignUpOrSignIn` kullanıcı yolculuğu. Kullanılabilir hale getirmek için mevcut bir şablonu kullanıcı yolculuğunun yinelenen oluştururuz.  Daha sonra Microsoft Account kimlik sağlayıcısı ekleyin:

> [!NOTE]
>
>Daha önce kopyalanmadıysa `<UserJourneys>` uzantısının ilkenizin temel dosyasından öğesine `TrustFrameworkExtensions.xml`, bu bölüme atlayabilirsiniz.

1.  Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.
2.  Bulma `<UserJourneys>` öğenin ve tüm içeriğini kopyalayın `<UserJourneys>` düğümü.
3.  Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve bulun `<UserJourneys>` öğesi. Öğe yoksa bir tane ekleyin.
4.  Tüm içeriğini yapıştırın `<UserJourneys>` alt öğesi olarak kopyaladığınız düğüm `<UserJourneys>` öğesi.

### <a name="display-the-button"></a>Bir düğme görüntülemek
`<ClaimsProviderSelections>` Öğe talep sağlayıcısı seçme seçenekleri ve bunların sırası listesini tanımlar.  `<ClaimsProviderSelection>` öğesi, bir oturumu-kaydolma/oturum açma sayfasında bir kimlik sağlayıcısı düğmesi benzerdir. Eklerseniz bir `<ClaimsProviderSelection>` öğesi Microsoft hesabı için yeni bir düğme gösterilir sayfasında bir kullanıcı gölünüzdeki olduğunda. Bu öğe eklemek için:

1.  Bulma `<UserJourney>` içeren düğüm `Id="SignUpOrSignIn"` kopyaladığınız kullanıcı giden.
2.  Bulun `<OrchestrationStep>` içeren düğüm `Order="1"`
3.  XML kod parçacığı altında aşağıdaki ekleyin `<ClaimsProviderSelections>` düğüm:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MicrosoftAccountExchange" />
```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.
Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için Microsoft Account iletişim kurmak için Azure AD B2C içindir. Düğme, Microsoft Account talep sağlayıcısı teknik profil bağlayarak bir eyleme bağlantı:

1.  Bulma `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
2.  XML kod parçacığı altında aşağıdaki ekleyin `<ClaimsExchanges>` düğüm:

```xml
<ClaimsExchange Id="MicrosoftAccountExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

> [!NOTE]
>
>   * Olun `Id` , aynı değere sahip `TargetClaimsExchangeId` önceki bölümde
>   * Olun `TechnicalProfileReferenceId` kimliği ayarlanır teknik profiline önceki (MSA-OIDC) oluşturulur.

## <a name="upload-the-policy-to-your-tenant"></a>Kiracınız için ilkeyi karşıya yükle
1.  İçinde [Azure portalında](https://portal.azure.com), içine geçiş [Azure AD B2C kiracınızın bağlamında](active-directory-b2c-navigate-to-b2c-context.md)açın **Azure AD B2C** dikey penceresi.
2.  Seçin **kimlik deneyimi çerçevesi**.
3.  Açık **tüm ilkeleri** dikey penceresi.
4.  Seçin **karşıya yükleme İlkesi**.
5.  Denetleme **ilke varsa üzerine** kutusu.
6.  **Karşıya yükleme** TrustFrameworkExtensions.xml ve doğrulama başlayabildiğinden emin olun.

## <a name="test-the-custom-policy-by-using-run-now"></a>Şimdi Çalıştır'i kullanarak özel bir ilkeyi test

1.  Açık **Azure AD B2C ayarlarını** gidin **kimlik deneyimi çerçevesi**.
> [!NOTE]
>
>**Şimdi Çalıştır** en az bir uygulamanın Kiracı'da önceden kayıtlı gerekir. Azure AD B2C uygulamaları kaydetme hakkında bilgi için bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makalesi.
2.  Açık **B2C_1A_signup_signin**, karşıya yüklediğiniz bağlı olan taraf (RP) özel ilke. Seçin **Şimdi Çalıştır**.
3.  Microsoft hesabı kullanarak oturum olması gerekir.

## <a name="optional-register-the-microsoft-account-claims-provider-to-profile-edit-user-journey"></a>[İsteğe bağlı] Profil düzenleme kullanıcı yolculuğu Microsoft Account talep sağlayıcısını Kaydet
Microsoft Account kimlik sağlayıcısı Ayrıca, kullanıcı eklemek isteyebileceğiniz `ProfileEdit` kullanıcı yolculuğu. Kullanılabilir hale getirmek için biz son iki adımı yineleyin:

### <a name="display-the-button"></a>Bir düğme görüntülemek
1.  Uzantı dosyası ilkenizin (örneğin, TrustFrameworkExtensions.xml) açın.
2.  Bulma `<UserJourney>` içeren düğüm `Id="ProfileEdit"` kopyaladığınız kullanıcı giden.
3.  Bulun `<OrchestrationStep>` içeren düğüm `Order="1"`
4.  XML kod parçacığı altında aşağıdaki ekleyin `<ClaimsProviderSelections>` düğüm:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.
1.  Bulma `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
2.  XML kod parçacığı altında aşağıdaki ekleyin `<ClaimsExchanges>` düğüm:

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Şimdi Çalıştır kullanarak test özel profil Düzenleme İlkesi
1.  Açık **Azure AD B2C ayarlarını** gidin **kimlik deneyimi çerçevesi**.
2.  Açık **B2C_1A_ProfileEdit**, karşıya yüklediğiniz bağlı olan taraf (RP) özel ilke. Seçin **Şimdi Çalıştır**.
3.  Microsoft hesabı kullanarak oturum olması gerekir.

## <a name="download-the-complete-policy-files"></a>Tüm ilke dosyalarını indirme
İsteğe bağlı: Bu örnek dosyalar yerine dosyaları ile çalışmaya başlama özel ilkeleri tamamladıktan sonra size yol kendi özel İlkesi kullanarak kendi senaryonuza yapı öneririz.  [Başvuru için örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-msa-app)
