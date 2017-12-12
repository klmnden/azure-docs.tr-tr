---
title: "Azure Active Directory B2C: Özel ilkelerini kullanma OAuth2 kimlik sağlayıcısı Google + Ekle"
description: "Örnek Google + OAuth2 protokolünü kullanarak kimlik sağlayıcısı kullanma"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: yoelh
ms.openlocfilehash: 54bf10acfb885042278c4457a70ec86248c96c1c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-add-google-as-an-oauth2-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Özel ilkelerini kullanma OAuth2 kimlik sağlayıcısı Google + Ekle

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu kılavuz size nasıl oturum açma hesabından Google + kullanım yoluyla kullanıcılar için etkinleştirileceğini gösterir [özel ilkeler](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Ön koşullar

Bölümündeki adımları tamamlamanız [özel ilkeleri ile çalışmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

Bu adımlar şunları içerir:

1.  Google + hesabı uygulaması oluşturuluyor.
2.  Azure AD B2C'ye Google + hesap uygulama anahtarı ekleme
3.  Bir ilke ekleme talep sağlayıcısını
4.  Google + hesap talep sağlayıcısı için bir kullanıcı gezisine kaydediliyor
5.  İlke için bir Azure AD B2C karşıya yükleme Kiracı ve test

## <a name="create-a-google-account-application"></a>Google + hesabı uygulaması oluşturma
Google + Azure Active Directory (Azure AD) B2C bir kimlik sağlayıcısı olarak kullanmak için bir Google + uygulaması oluşturmak ve doğru parametrelerle sağlamanız gerekir. Bir Google + uygulama burada kaydedebilirsiniz: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)

1.  Git [Google geliştiriciler konsol](https://console.developers.google.com/) ve Google + hesabı kimlik bilgilerinizle oturum açın.
2.  Tıklatın **proje oluştur**, girin bir **proje adı**ve ardından **oluşturma**.

3.  Tıklayın **projeleri menü**.

    ![Google + hesabı - proje'yi seçin](media/active-directory-b2c-custom-setup-goog-idp/goog-add-new-app1.png)

4.  Tıklayın  **+**  düğmesi.

    ![Google + hesap - yeni proje oluşturma](media/active-directory-b2c-custom-setup-goog-idp//goog-add-new-app2.png)

5.  Girin bir **proje adı**ve ardından **oluşturma**.

    ![Google + hesabı - yeni proje](media/active-directory-b2c-custom-setup-goog-idp//goog-app-name.png)

6.  Proje hazır olana kadar bekleyin ve tıklayın **projeleri menü**.

    ![Google + hesabı - yeni proje kullanıma hazır olana kadar bekleyin](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app1.png)

7.  Proje adına tıklayın.

    ![Google + hesabı - yeni proje seçin](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app2.png)

8.  Tıklatın **API Yöneticisi** ve ardından **kimlik bilgileri** sol gezinti bölmesinde.
9.  Tıklatın **OAuth izni ekran** üst sekmesini.

    ![Google + hesabı - ayarlamak OAuth onay ekranı](media/active-directory-b2c-custom-setup-goog-idp/goog-add-cred.png)

10.  Seçin veya geçerli bir belirtin **e-posta adresi**, sağlayan bir **ürün adı**, tıklatıp **kaydetmek**.

    ![Google + - uygulama kimlik bilgileri](media/active-directory-b2c-custom-setup-goog-idp/goog-consent-screen.png)

11.  Tıklatın **yeni kimlik bilgileri** ve ardından **OAuth istemci kimliği**.

    ![Google + - yeni uygulama kimlik bilgileri oluşturun](media/active-directory-b2c-custom-setup-goog-idp/goog-add-oauth2-client-id.png)

12.  Altında **uygulama türü**seçin **Web uygulaması**.

    ![Google + - uygulama türünü seçme](media/active-directory-b2c-custom-setup-goog-idp/goog-web-app.png)

13.  Sağlayan bir **adı** uygulamanız için girin `https://login.microsoftonline.com` içinde **yetkili JavaScript çıkış** alan, ve `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **yetkili URI'ler yeniden yönlendirme** alan. Değiştir **{tenant}** , kiracının adlı (örneğin, contosob2c.onmicrosoft.com). **{Tenant}** duyarlıdır. **Oluştur**'a tıklayın.

    ![Google + - yetkili JavaScript çıkış sağlayın ve URI'ler yeniden yönlendirme](media/active-directory-b2c-custom-setup-goog-idp/goog-create-client-id.png)

14.  Değerleri kopyalamak **istemci kimliği** ve **gizli**. Google + kimlik sağlayıcısı kiracınızda yapılandırmak için her ikisini de gerekir. **İstemci parolası** önemli güvenlik kimlik bilgileri.

    ![Google + - istemci kimliği ve istemci gizli anahtarı değerlerini kopyalayın](media/active-directory-b2c-custom-setup-goog-idp/goog-client-secret.png)

## <a name="add-the-google-account-application-key-to-azure-ad-b2c"></a>Azure AD B2C'ye Google + hesap uygulama anahtarı Ekle
Google + hesaplarıyla Federasyon Google + hesabına güven Azure AD B2C uygulama adına bir istemci parolası gerektirir. Azure AD B2C kiracısı Google + uygulama gizli anahtarı depolamak gerekir:  

1.  Azure AD B2C kiracınızın gidin ve seçin **B2C ayarlarını** > **kimlik deneyimi Framework**
2.  Seçin **İlkesi anahtarları** kiracınızda kullanılabilir tuşlarını görmek için.
3.  Tıklatın **+ Ekle**.
4.  İçin **seçenekleri**, kullanın **el ile**.
5.  İçin **adı**, kullanmak `GoogleSecret`.  
    Önek `B2C_1A_` otomatik olarak eklenebilir.
6.  İçinde **gizli** kutusuna, Google uygulama parolanızı girin [Google geliştiriciler konsol](https://console.developers.google.com/) yukarıda kopyaladığınız.
7.  İçin **anahtar kullanımı**, kullanın **imza**.
8.  **Oluştur**'a tıklayın
9.  Anahtar oluşturduğunuz onaylayın `B2C_1A_GoogleSecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Bir talep sağlayıcı uzantısı ilkenizde ekleme

Google + hesabını kullanarak oturum açmalarını istiyorsanız, bir talep sağlayıcısı olarak Google + hesap tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C ile iletişim kuran bir uç nokta belirtmeniz gerekir. Uç nokta Azure AD B2C tarafından belirli bir kullanıcı kimliği doğrulanmış olduğunu doğrulamak için kullanılan talep kümesini sağlar.

Google + hesabı ekleyerek bir talep sağlayıcısı olarak tanımlamak `<ClaimsProvider>` İlkesi uzantısının düğümünde:

1.  Uzantı ilke dosyası (TrustFrameworkExtensions.xml) çalışma dizininizi açın. Bir XML Düzenleyici, gerekirse [Visual Studio Code deneyin](https://code.visualstudio.com/download), basit bir platformlar arası Düzenleyici.
2.  Bul `<ClaimsProviders>` bölümü
3.  Altında aşağıdaki XML parçacığını ekleyin `ClaimsProviders` öğesi ve Değiştir `client_id` dosyayı kaydetmeden önce Google + hesabı uygulama istemci kimliği ile değer.  

```xml
<ClaimsProvider>
    <Domain>google.com</Domain>
    <DisplayName>Google</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Google-OAUTH">
        <DisplayName>Google</DisplayName>
        <Protocol Name="OAuth2" />
        <Metadata>
        <Item Key="ProviderName">google</Item>
        <Item Key="authorization_endpoint">https://accounts.google.com/o/oauth2/auth</Item>
        <Item Key="AccessTokenEndpoint">https://accounts.google.com/o/oauth2/token</Item>
        <Item Key="ClaimsEndpoint">https://www.googleapis.com/oauth2/v1/userinfo</Item>
        <Item Key="scope">email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Google+ application ID</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_GoogleSecret" />
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="google.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        <ErrorHandlers>
        <ErrorHandler>
            <ErrorResponseFormat>json</ErrorResponseFormat>
            <ResponseMatch>$[?(@@.error == 'invalid_grant')]</ResponseMatch>
            <Action>Reauthenticate</Action>
            <!--In case of authorization code used error, we don't want the user to select his account again.-->
            <!--AdditionalRequestParameters Key="prompt">select_account</AdditionalRequestParameters-->
        </ErrorHandler>
        </ErrorHandlers>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-the-google-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Kaydolun veya kullanıcı gezisine imzalamak için Google + hesap talep sağlayıcısını Kaydet

Kimlik sağlayıcısı ayarlandığına.  Ancak, oturumu-up/oturum açma ekranları hiçbirinde kullanılabilir değil. Google + hesabı kimlik sağlayıcısı, kullanıcı ekleme `SignUpOrSignIn` kullanıcı gezisine. Kullanılabilir hale getirmek için var olan bir şablonu kullanıcı gezisine tekrarı oluşturuyoruz.  Daha sonra Google + hesabı kimlik sağlayıcısı ekleyin:

>[!NOTE]
>
>Kopyaladığınız varsa `<UserJourneys>` ilkenizin temel dosyanın bir öğeden uzantısının (TrustFrameworkExtensions.xml) için bu bölümü atlayabilirsiniz.

1.  Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.
2.  Bul `<UserJourneys>` öğesi ve tüm içeriğini kopyalayın `<UserJourneys>` düğümü.
3.  Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve Bul `<UserJourneys>` öğesi. Öğe yoksa, ekleyin.
4.  Tüm içeriğini yapıştırın `<UserJournesy>` bir alt öğesi olarak kopyaladığınız düğümü `<UserJourneys>` öğesi.

### <a name="display-the-button"></a>Görüntü düğmesi
`<ClaimsProviderSelections>` Öğesi talep sağlayıcısı seçme seçenekleri ve bunların sırası listesini tanımlar.  `<ClaimsProviderSelection>`öğesi, bir oturumu-up/oturum açma sayfasında bir kimlik sağlayıcısı düğmesini benzerdir. Eklerseniz bir `<ClaimsProviderSelection>` öğesi Google + hesap için yeni bir düğme görüntülenir sayfasında bir kullanıcı adlandırıldığını olduğunda. Bu öğe eklemek için:

1.  Bul `<UserJourney>` içeren düğüm `Id="SignUpOrSignIn"` kopyaladığınız kullanıcı gezisine içinde.
2.  Bulun `<OrchestrationStep>` içeren düğümü`Order="1"`
3.  Aşağıdaki XML parçacığını altında ekleyin `<ClaimsProviderSelections>` düğümü:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem bağlantı kurun
Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için Google + hesabı ile iletişim kurmak Azure AD B2C içindir.

1.  Bul `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
2.  Aşağıdaki XML parçacığını altında ekleyin `<ClaimsExchanges>` düğümü:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

>[!NOTE]
>
> * Olun `Id` , aynı değere sahip `TargetClaimsExchangeId` önceki bölümde
> * Olun `TechnicalProfileReferenceId` kimliği ayarlanmış teknik profiline önceki (Google-OAUTH) oluşturuldu.

## <a name="upload-the-policy-to-your-tenant"></a>İlke kiracınız için karşıya yükleme
1.  İçinde [Azure portal](https://portal.azure.com), içine geçiş [bağlam Azure AD B2C kiracınızın](active-directory-b2c-navigate-to-b2c-context.md), açarak **Azure AD B2C** dikey.
2.  Seçin **kimlik deneyimi Framework**.
3.  Açık **tüm ilkeler** dikey.
4.  Seçin **karşıya İlkesi**.
5.  Denetleme **varsa ilkesi üzerine** kutusu.
6.  **Karşıya yükleme** TrustFrameworkExtensions.xml ve doğrulama başlayabildiğinden emin olun

## <a name="test-the-custom-policy-by-using-run-now"></a>Özel ilke Şimdi Çalıştır kullanarak test
1.  Açık **Azure AD B2C ayarlarını** ve Git **kimlik deneyimi Framework**.

    >[!NOTE]
    >
    >    **Şimdi Çalıştır** Kiracı'preregistered için en az bir uygulama gerekiyor. 
    >    Uygulamaları kaydetmek öğrenmek için Azure AD B2C bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makale.


2.  Açık **B2C_1A_signup_signin**, karşıya yüklediğiniz bağlı olan taraf (RP) özel ilke. Seçin **Şimdi Çalıştır**.
3.  Google + hesabı kullanarak oturum olması gerekir.

## <a name="optional-register-the-google-account-claims-provider-to-profile-edit-user-journey"></a>[İsteğe bağlı] Google + hesap talep sağlayıcısını profil düzenleme kullanıcı gezisine kaydetme
Google + hesabı kimlik sağlayıcısı Ayrıca, kullanıcı eklemek isteyebilirsiniz `ProfileEdit` kullanıcı gezisine. Kullanılabilir hale getirmek için biz son iki adımı yineleyin:

### <a name="display-the-button"></a>Görüntü düğmesi
1.  Uzantı dosyası ilkenizin (örneğin, TrustFrameworkExtensions.xml) açın.
2.  Bul `<UserJourney>` içeren düğüm `Id="ProfileEdit"` kopyaladığınız kullanıcı gezisine içinde.
3.  Bulun `<OrchestrationStep>` içeren düğümü`Order="1"`
4.  Aşağıdaki XML parçacığını altında ekleyin `<ClaimsProviderSelections>` düğümü:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem bağlantı kurun
1.  Bul `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
2.  Aşağıdaki XML parçacığını altında ekleyin `<ClaimsExchanges>` düğümü:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Özel Profil Düzenleme İlkesi Şimdi Çalıştır kullanarak test

1.  Açık **Azure AD B2C ayarlarını** ve Git **kimlik deneyimi Framework**.
2.  Açık **B2C_1A_ProfileEdit**, karşıya yüklediğiniz bağlı olan taraf (RP) özel ilke. Seçin **Şimdi Çalıştır**.
3.  Google + hesabı kullanarak oturum olması gerekir.

## <a name="download-the-complete-policy-files"></a>Tam ilke dosyaları indirme
İsteğe bağlı: Bu örnek dosyalar yerine dosyaları ile çalışmaya başlama özel ilkeler tamamladıktan sonra size yol kendi özel İlkesi kullanarak senaryonuz yapı öneririz.  [Başvuru için örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-goog-app)
