---
title: "Azure Active Directory B2C: Microsoft hesabı (MSA) özel ilkelerini kullanarak bir kimlik sağlayıcısı ekleyin."
description: "Openıd Connect (OIDC) protokolünü kullanarak kimlik sağlayıcısı Microsoft kullanılarak örnek"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: joroja
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: yoelh
ms.openlocfilehash: 8c981046ff41d3927ff60d6dc4f40366ae25ba74
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-add-microsoft-account-msa-as-an-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Microsoft hesabı (MSA) özel ilkelerini kullanarak bir kimlik sağlayıcısı ekleyin.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, oturum açmak için Microsoft hesabı (MSA) kullanıcılardan kullanılarak nasıl etkinleştirileceği gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Ön koşullar
Bölümündeki adımları tamamlamanız [özel ilkeleri ile çalışmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

Bu adımlar şunları içerir:

1.  Bir Microsoft hesabı uygulaması oluşturuluyor.
2.  Azure AD B2C'ye Microsoft hesabı uygulama anahtarı ekleme
3.  Bir ilke ekleme talep sağlayıcısını
4.  Bir kullanıcı gezisine için Microsoft Account talep sağlayıcısı kaydediliyor
5.  İlke için bir Azure AD B2C karşıya yükleme Kiracı ve test

## <a name="create-a-microsoft-account-application"></a>Bir Microsoft hesabı uygulaması oluşturma
Microsoft hesabı kimlik sağlayıcısı Azure Active Directory (Azure AD) B2C içinde kullanmak için bir Microsoft hesabı uygulaması oluşturmak ve doğru parametrelerle sağlamanız gerekir. Bir Microsoft hesabı gerekir. Yoksa, ziyaret [https://www.live.com/](https://www.live.com/).

1.  Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) ve Microsoft hesabı kimlik bilgilerinizle oturum açın.
2.  Tıklatın **bir uygulama ekleyin**.

    ![Microsoft hesabı - bir uygulama ekleyin](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-new-app.png)

3.  Sağlamak bir **adı** , uygulamanız için **kişi e-posta**, seçeneğinin işaretini kaldırın **başlamanıza yardım bize** tıklatıp **oluşturma**.

    ![Microsoft hesabı - uygulamanızı kaydetme](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-name.png)

4.  Değerini kopyalayın **uygulama kimliği**. Microsoft hesabı kimlik sağlayıcısı kiracınızda yapılandırmak için ihtiyacınız.

    ![Microsoft hesabı - kopyalama uygulama kimliği değeri](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-id.png)

5.  Tıklayın **Ekle platformu**

    ![Microsoft hesabı - platform ekleme](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-platform.png)

6.  Platform listeden seçin **Web**.

    ![Microsoft hesabı - platform listeden Web seçin](media/active-directory-b2c-custom-setup-ms-account-idp/msa-web.png)

7.  Girin `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **yeniden yönlendirme URI'ler** alan. Değiştir **{tenant}** , kiracının adlı (örneğin, contosob2c.onmicrosoft.com).

    ![Microsoft hesabı - kümesi yeniden yönlendirme URL'leri](media/active-directory-b2c-custom-setup-ms-account-idp/msa-redirect-url.png)

8.  Tıklayın **yeni bir parola oluşturmak** altında **uygulama parolaları** bölümü. Ekranda görüntülenen yeni parolayı kopyalayın. Microsoft hesabı kimlik sağlayıcısı kiracınızda yapılandırmak için ihtiyacınız. Bu parolayı bir önemli güvenlik kimlik bilgisidir.

    ![Microsoft hesabı - yeni bir parola oluştur](media/active-directory-b2c-custom-setup-ms-account-idp/msa-generate-new-password.png)

    ![Microsoft hesabı - kopyalama yeni parola](media/active-directory-b2c-custom-setup-ms-account-idp/msa-new-password.png)

9.  Belirten kutuyu **Live SDK'sı desteği** altında **Gelişmiş Seçenekler** bölümü. **Kaydet** düğmesine tıklayın.

    ![Microsoft hesabı - Live SDK'sı desteği](media/active-directory-b2c-custom-setup-ms-account-idp/msa-live-sdk-support.png)

## <a name="add-the-microsoft-account-application-key-to-azure-ad-b2c"></a>Azure AD B2C'ye Microsoft hesabı uygulama anahtarı Ekle
Microsoft hesapları ile federasyon güven Azure AD B2C uygulama adına için Microsoft hesabı istemci gizli anahtarı gerektirir. Azure AD B2C kiracısı, Microsoft hesabı uygulama secert saklamanız gerekir:   

1.  Azure AD B2C kiracınızın gidin ve seçin **B2C ayarlarını** > **kimlik deneyimi Framework**
2.  Seçin **İlkesi anahtarları** kiracınızda kullanılabilir tuşlarını görmek için.
3.  Tıklatın **+ Ekle**.
4.  İçin **seçenekleri**, kullanın **el ile**.
5.  İçin **adı**, kullanmak `MSASecret`.  
    Önek `B2C_1A_` otomatik olarak eklenebilir.
6.  İçinde **gizli** kutusuna, https://apps.dev.microsoft.com Microsoft uygulama gizli anahtarı girin
7.  İçin **anahtar kullanımı**, kullanın **imza**.
8.  **Oluştur**'a tıklayın
9.  Anahtar oluşturduğunuz onaylayın `B2C_1A_MSASecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Bir talep sağlayıcı uzantısı ilkenizde ekleme
Microsoft Account kullanarak oturum açmalarını istiyorsanız, bir talep sağlayıcısı olarak Microsoft Account tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C ile iletişim kuran bir uç nokta belirtmeniz gerekir. Uç nokta Azure AD B2C tarafından belirli bir kullanıcı kimliği doğrulanmış olduğunu doğrulamak için kullanılan talep kümesini sağlar.

Microsoft Account ekleyerek bir talep sağlayıcısı olarak tanımlamak `<ClaimsProvider>` İlkesi uzantısının düğümünde:

1.  Uzantı ilke dosyası (TrustFrameworkExtensions.xml) çalışma dizininizi açın. Bir XML Düzenleyici, gerekirse [Visual Studio Code deneyin](https://code.visualstudio.com/download), basit bir platformlar arası Düzenleyici.
2.  Bul `<ClaimsProviders>` bölümü
3.  Aşağıdaki XML parçacığını altında ekleyin `ClaimsProviders` öğe:

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

4.  Değiştir `client_id` Microsoft Account uygulama istemci kimliği değeri

5.  Dosyayı kaydedin.

## <a name="register-the-microsoft-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Microsoft Account yukarı oturum sağlayıcısına talep Kaydet veya oturum açma kullanıcı gezisine

Bu noktada, kimlik sağlayıcısı ayarlandı, ancak oturumu-up/oturum açma ekranları hiçbirinde kullanılabilir değil. Microsoft Account kimlik sağlayıcısı, kullanıcı eklemek gereken artık `SignUpOrSignIn` kullanıcı gezisine. Kullanılabilir hale getirmek için var olan bir şablonu kullanıcı gezisine tekrarı oluşturuyoruz.  Daha sonra Microsoft Account kimlik sağlayıcısı ekleyin:

> [!NOTE]
>
>Daha önce kopyaladıysanız `<UserJourneys>` uzantısının ilkenizin temel dosyasından öğesine `TrustFrameworkExtensions.xml`, bu bölümü atlayabilirsiniz.

1.  Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.
2.  Bul `<UserJourneys>` öğesi ve tüm içeriğini kopyalayın `<UserJourneys>` düğümü.
3.  Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve Bul `<UserJourneys>` öğesi. Öğe yoksa, ekleyin.
4.  Tüm içeriğini yapıştırın `<UserJournesy>` bir alt öğesi olarak kopyaladığınız düğümü `<UserJourneys>` öğesi.

### <a name="display-the-button"></a>Görüntü düğmesi
`<ClaimsProviderSelections>` Öğesi talep sağlayıcısı seçme seçenekleri ve bunların sırası listesini tanımlar.  `<ClaimsProviderSelection>`öğesi, bir oturumu-up/oturum açma sayfasında bir kimlik sağlayıcısı düğmesini benzerdir. Eklerseniz bir `<ClaimsProviderSelection>` öğesi Microsoft hesabı için yeni bir düğme görüntülenir sayfasında bir kullanıcı adlandırıldığını olduğunda. Bu öğe eklemek için:

1.  Bul `<UserJourney>` içeren düğüm `Id="SignUpOrSignIn"` kopyaladığınız kullanıcı gezisine içinde.
2.  Bulun `<OrchestrationStep>` içeren düğümü`Order="1"`
3.  Aşağıdaki XML parçacığını altında ekleyin `<ClaimsProviderSelections>` düğümü:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem bağlantı kurun
Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için Microsoft Account iletişim kurmak Azure AD B2C içindir. Düğme, Microsoft Account talep sağlayıcısı teknik profili bağlayarak bir eyleme bağlantı:

1.  Bul `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
2.  Aşağıdaki XML parçacığını altında ekleyin `<ClaimsExchanges>` düğümü:

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

> [!NOTE]
>
>   * Olun `Id` , aynı değere sahip `TargetClaimsExchangeId` önceki bölümde
>   * Olun `TechnicalProfileReferenceId` kimliği ayarlanmış teknik profiline oluşturduğunuz önceki (MSA-OIDC).

## <a name="upload-the-policy-to-your-tenant"></a>İlke kiracınız için karşıya yükleme
1.  İçinde [Azure portal](https://portal.azure.com), içine geçiş [bağlam Azure AD B2C kiracınızın](active-directory-b2c-navigate-to-b2c-context.md), açarak **Azure AD B2C** dikey.
2.  Seçin **kimlik deneyimi Framework**.
3.  Açık **tüm ilkeler** dikey.
4.  Seçin **karşıya İlkesi**.
5.  Denetleme **varsa ilkesi üzerine** kutusu.
6.  **Karşıya yükleme** TrustFrameworkExtensions.xml ve doğrulama başlayabildiğinden emin olun

## <a name="test-the-custom-policy-by-using-run-now"></a>Özel ilke Şimdi Çalıştır kullanarak test

1.  Açık **Azure AD B2C ayarlarını** ve Git **kimlik deneyimi Framework**.
> [!NOTE]
>
>**Şimdi Çalıştır** Kiracı'preregistered için en az bir uygulama gerekiyor. Uygulamaları kaydetmek öğrenmek için Azure AD B2C bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makale.
2.  Açık **B2C_1A_signup_signin**, karşıya yüklediğiniz bağlı olan taraf (RP) özel ilke. Seçin **Şimdi Çalıştır**.
3.  Microsoft hesabı kullanarak oturum olması gerekir.

## <a name="optional-register-the-microsoft-account-claims-provider-to-profile-edit-user-journey"></a>[İsteğe bağlı] Profil düzenleme kullanıcı gezisine Microsoft Account talep sağlayıcısını Kaydet
Microsoft Account kimlik sağlayıcısı Ayrıca, kullanıcı eklemek isteyebilirsiniz `ProfileEdit` kullanıcı gezisine. Kullanılabilir hale getirmek için biz son iki adımı yineleyin:

### <a name="display-the-button"></a>Görüntü düğmesi
1.  Uzantı dosyası ilkenizin (örneğin, TrustFrameworkExtensions.xml) açın.
2.  Bul `<UserJourney>` içeren düğüm `Id="ProfileEdit"` kopyaladığınız kullanıcı gezisine içinde.
3.  Bulun `<OrchestrationStep>` içeren düğümü`Order="1"`
4.  Aşağıdaki XML parçacığını altında ekleyin `<ClaimsProviderSelections>` düğümü:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem bağlantı kurun
1.  Bul `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
2.  Aşağıdaki XML parçacığını altında ekleyin `<ClaimsExchanges>` düğümü:

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Özel Profil Düzenleme İlkesi Şimdi Çalıştır kullanarak test
1.  Açık **Azure AD B2C ayarlarını** ve Git **kimlik deneyimi Framework**.
2.  Açık **B2C_1A_ProfileEdit**, karşıya yüklediğiniz bağlı olan taraf (RP) özel ilke. Seçin **Şimdi Çalıştır**.
3.  Microsoft hesabı kullanarak oturum olması gerekir.

## <a name="download-the-complete-policy-files"></a>Tam ilke dosyaları indirme
İsteğe bağlı: Bu örnek dosyalar yerine dosyaları ile çalışmaya başlama özel ilkeler tamamladıktan sonra size yol kendi özel İlkesi kullanarak senaryonuz yapı öneririz.  [Başvuru için örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-msa-app)
