---
title: Google + Azure Active Directory B2C'de özel ilkeleri kullanarak OAuth2 kimlik sağlayıcısı Ekle | Microsoft Docs
description: Örnek, Google + OAuth2 protokolünü kullanarak kimlik sağlayıcısı olarak kullanma.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 19b7f708d43907ac45450a64f988b2a517293511
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446716"
---
# <a name="azure-active-directory-b2c-add-google-as-an-oauth2-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Özel ilkeleri kullanarak OAuth2 kimlik sağlayıcısı olarak Google + Ekle

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu kılavuz, kullanıcıların Google + kullanımının hesabı için oturum açma olanağı tanıma göstermektedir [özel ilkeler](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

Bu adımlar şunlardır:

1.  Google + hesabı uygulaması oluşturuluyor.
2.  Azure AD B2C'ye Google + hesabı uygulama anahtarı ekleme
3.  Bir ilke için talep sağlayıcısı ekleniyor
4.  Google + hesabı talep sağlayıcısı kullanıcı yolculuğu için kaydetme
5.  Kiracı için Azure AD B2C İlkesi karşıya yükleme ve test

## <a name="create-a-google-account-application"></a>Google + hesabı uygulaması oluşturma
Google + Azure Active Directory (Azure AD) B2C'de kimlik sağlayıcısı olarak kullanmak için Google + uygulama oluşturabilir ve doğru parametreleri sağlamanız gerekir. Bir Google + uygulama burada kaydedebilirsiniz: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)

1.  Git [Google geliştiriciler konsol](https://console.developers.google.com/) ve Google + hesabı kimlik bilgilerinizle oturum açın.
2.  Tıklayın **proje oluştur**, girin bir **proje adı**ve ardından **Oluştur**.

3.  Tıklayarak **projeleri menü**.

    ![Google + hesabı - proje'yi seçin](media/active-directory-b2c-custom-setup-goog-idp/goog-add-new-app1.png)

4.  Tıklayarak **+** düğmesi.

    ![Google + hesap - yeni proje oluşturma](media/active-directory-b2c-custom-setup-goog-idp//goog-add-new-app2.png)

5.  Girin bir **proje adı**ve ardından **Oluştur**.

    ![Google + hesabı - yeni proje](media/active-directory-b2c-custom-setup-goog-idp//goog-app-name.png)

6.  Proje hazır olana kadar bekleyin ve tıklayarak **projeleri menü**.

    ![Google + hesabı - yeni proje kullanıma hazır olana kadar bekleyin](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app1.png)

7.  Proje adınıza tıklayın.

    ![Google + hesabı - yeni proje seçin](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app2.png)

8.  Tıklayın **API Yöneticisi** ve ardından **kimlik bilgilerini** sol gezinti bölmesinde.
9.  Tıklayın **OAuth onay ekranı** en üstteki sekmedeki.

    ![Google + hesabı - ayarla OAuth onay ekranı](media/active-directory-b2c-custom-setup-goog-idp/goog-add-cred.png)

10.  Seçin veya geçerli **e-posta adresi**, sağlayan bir **ürün adı**, tıklatıp **Kaydet**.

    ![Google + - uygulama kimlik bilgileri](media/active-directory-b2c-custom-setup-goog-idp/goog-consent-screen.png)

11.  Tıklayın **yeni kimlik bilgileri** seçip **OAuth istemcisi kimliği**.

    ![Google + - yeni uygulama kimlik bilgileri oluşturma](media/active-directory-b2c-custom-setup-goog-idp/goog-add-oauth2-client-id.png)

12.  Altında **uygulama türü**seçin **Web uygulaması**.

    ![Google + - uygulama türünü seçme](media/active-directory-b2c-custom-setup-goog-idp/goog-web-app.png)

13.  Sağlayan bir **adı** uygulamanız için girin `https://login.microsoftonline.com` içinde **yetkili JavaScript kaynakları** alan ve `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **yetkili yeniden yönlendirme URI'leri** alan. Değiştirin **{tenant}** kiracınızın adı (örneğin, contosob2c.onmicrosoft.com) ile. **{Tenant}** duyarlıdır. **Oluştur**’a tıklayın.

    ![Google + - yetkili JavaScript kaynakları sağlayın ve yeniden yönlendirme URI'leri](media/active-directory-b2c-custom-setup-goog-idp/goog-create-client-id.png)

14.  Değerlerini kopyalamayı **istemci kimliği** ve **gizli**. Google + kiracınızdaki bir kimlik sağlayıcısı olarak yapılandırmak için her ikisi de gerekir. **İstemci gizli anahtarı** bir önemli güvenlik kimlik bilgisidir.

    ![Google + - istemci kimliğini ve istemci gizli dizisi değerlerini kopyalayın.](media/active-directory-b2c-custom-setup-goog-idp/goog-client-secret.png)

## <a name="add-the-google-account-application-key-to-azure-ad-b2c"></a>Azure AD B2C'ye Google + hesabı uygulama anahtarı Ekle
Google + hesapları ile Federasyon, Google + uygulama adına güven Azure AD B2C hesabı için bir istemci gizli anahtarı gerektirir. Azure AD B2C kiracısında Google + uygulama gizli anahtarı depolamak gerekir:  

1.  Azure AD B2C kiracınıza gitmek ve seçin **B2C ayarlarını** > **kimlik deneyimi çerçevesi**
2.  Seçin **ilke anahtarları** kiracınızda kullanılabilir tuşlarını görmek için.
3.  Tıklayın **+ Ekle**.
4.  İçin **seçenekleri**, kullanın **el ile**.
5.  İçin **adı**, kullanın `GoogleSecret`.  
    Önek `B2C_1A_` otomatik olarak eklenebilir.
6.  İçinde **gizli** kutusuna, gelen Google uygulama gizli anahtarı girin [Google geliştiriciler konsol](https://console.developers.google.com/) yukarıda kopyaladığınız.
7.  İçin **anahtar kullanımı**, kullanın **imza**.
8.  **Oluştur**'a tıklayın
9.  Anahtar oluşturduğunuzu doğrulayın `B2C_1A_GoogleSecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Uzantı ilkenizde bir talep Sağlayıcı Ekle

Google + hesabını kullanarak oturum açmalarını istiyorsanız, bir talep sağlayıcısı olarak Google + hesabı tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C ile iletişim kuran bir uç nokta belirtmeniz gerekir. Uç nokta, Azure AD B2C tarafından belirli bir kullanıcı yapıldığını doğrulamak için kullanılan bir talepler kümesi sağlar.

Google + hesabı ekleyerek bir talep sağlayıcısı olarak tanımlamak `<ClaimsProvider>` ilke uzantısının düğümünde:

1.  Uzantı ilke dosyası (TrustFrameworkExtensions.xml) çalışma dizininizi açın. Bir XML düzenleyicisini gerekiyorsa [Visual Studio Code deneyin](https://code.visualstudio.com/download), basit bir platformlar arası Düzenleyici.
2.  Bulma `<ClaimsProviders>` bölümü
3.  Altında aşağıdaki XML parçacığını ekleyin `ClaimsProviders` öğesi ve Değiştir `client_id` dosyayı kaydetmeden önce Google + hesabı uygulama istemci kimliği ile değeri.  

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

## <a name="register-the-google-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Kaydolma veya oturum kullanıcı yolculuğunda Google + hesabı talep sağlayıcısını Kaydet

Kimlik sağlayıcısı ayarlanmış olması.  Ancak, oturumu-kaydolma/oturum açma ekranları hiçbirinde kullanılabilir değil. Google + hesabı kimlik sağlayıcısı için kullanıcı ekleme `SignUpOrSignIn` kullanıcı yolculuğu. Kullanılabilir hale getirmek için mevcut bir şablonu kullanıcı yolculuğunun yinelenen oluştururuz.  Daha sonra Google + hesabı kimlik sağlayıcısı ekleyin:

>[!NOTE]
>
>Kopyaladığınız varsa `<UserJourneys>` uzantı dosyası (TrustFrameworkExtensions.xml) öğesine ilkenizin temel dosyasından bu bölüme atlayabilirsiniz.

1.  Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.
2.  Bulma `<UserJourneys>` öğenin ve tüm içeriğini kopyalayın `<UserJourneys>` düğümü.
3.  Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve bulun `<UserJourneys>` öğesi. Öğe yoksa bir tane ekleyin.
4.  Tüm içeriğini yapıştırın `<UserJourney>` alt öğesi olarak kopyaladığınız düğüm `<UserJourneys>` öğesi.

### <a name="display-the-button"></a>Bir düğme görüntülemek
`<ClaimsProviderSelections>` Öğe talep sağlayıcısı seçme seçenekleri ve bunların sırası listesini tanımlar.  `<ClaimsProviderSelection>` öğesi, bir oturumu-kaydolma/oturum açma sayfasında bir kimlik sağlayıcısı düğmesi benzerdir. Eklerseniz bir `<ClaimsProviderSelection>` öğesi Google +'hesabı için yeni bir düğme gösterilir sayfasında bir kullanıcı gölünüzdeki olduğunda. Bu öğe eklemek için:

1.  Bulma `<UserJourney>` içeren düğüm `Id="SignUpOrSignIn"` kopyaladığınız kullanıcı giden.
2.  Bulun `<OrchestrationStep>` içeren düğüm `Order="1"`
3.  XML kod parçacığı altında aşağıdaki ekleyin `<ClaimsProviderSelections>` düğüm:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.
Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için Google + hesabı ile iletişim kurmak için Azure AD B2C içindir.

1.  Bulma `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
2.  XML kod parçacığı altında aşağıdaki ekleyin `<ClaimsExchanges>` düğüm:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

>[!NOTE]
>
> * Olun `Id` , aynı değere sahip `TargetClaimsExchangeId` önceki bölümde
> * Olun `TechnicalProfileReferenceId` kimliği ayarlanır teknik profiline önceki (Google-OAUTH) oluşturulur.

## <a name="upload-the-policy-to-your-tenant"></a>Kiracınız için ilkeyi karşıya yükle
1.  İçinde [Azure portalında](https://portal.azure.com), içine geçiş [Azure AD B2C kiracınızın bağlamında](active-directory-b2c-navigate-to-b2c-context.md)açın **Azure AD B2C** dikey penceresi.
2.  Seçin **kimlik deneyimi çerçevesi**.
3.  Açık **tüm ilkeleri** dikey penceresi.
4.  Seçin **karşıya yükleme İlkesi**.
5.  Denetleme **ilke varsa üzerine** kutusu.
6.  **Karşıya yükleme** TrustFrameworkExtensions.xml ve doğrulama başlayabildiğinden emin olun.

## <a name="test-the-custom-policy-by-using-run-now"></a>Şimdi Çalıştır'i kullanarak özel bir ilkeyi test
1.  Açık **Azure AD B2C ayarlarını** gidin **kimlik deneyimi çerçevesi**.

    >[!NOTE]
    >
    >    **Şimdi Çalıştır** en az bir uygulamanın Kiracı'da önceden kayıtlı gerekir. 
    >    Azure AD B2C uygulamaları kaydetme hakkında bilgi için bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makalesi.


2.  Açık **B2C_1A_signup_signin**, karşıya yüklediğiniz bağlı olan taraf (RP) özel ilke. Seçin **Şimdi Çalıştır**.
3.  Google + hesabını kullanarak oturum açabilir olması gerekir.

## <a name="optional-register-the-google-account-claims-provider-to-profile-edit-user-journey"></a>[İsteğe bağlı] Google + hesabı talep sağlayıcısı profil düzenleme kullanıcı yolculuğu için kaydolun
Google + hesabı kimlik sağlayıcısı Ayrıca, kullanıcı eklemek isteyebileceğiniz `ProfileEdit` kullanıcı yolculuğu. Kullanılabilir hale getirmek için biz son iki adımı yineleyin:

### <a name="display-the-button"></a>Bir düğme görüntülemek
1.  Uzantı dosyası ilkenizin (örneğin, TrustFrameworkExtensions.xml) açın.
2.  Bulma `<UserJourney>` içeren düğüm `Id="ProfileEdit"` kopyaladığınız kullanıcı giden.
3.  Bulun `<OrchestrationStep>` içeren düğüm `Order="1"`
4.  XML kod parçacığı altında aşağıdaki ekleyin `<ClaimsProviderSelections>` düğüm:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.
1.  Bulma `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
2.  XML kod parçacığı altında aşağıdaki ekleyin `<ClaimsExchanges>` düğüm:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

### <a name="upload-the-policy-to-your-tenant"></a>Kiracınız için ilkeyi karşıya yükle
1.  İçinde [Azure portalında](https://portal.azure.com), içine geçiş [Azure AD B2C kiracınızın bağlamında](active-directory-b2c-navigate-to-b2c-context.md)açın **Azure AD B2C** dikey penceresi.
2.  Seçin **kimlik deneyimi çerçevesi**.
3.  Açık **tüm ilkeleri** dikey penceresi.
4.  Seçin **karşıya yükleme İlkesi**.
5.  Denetleme **ilke varsa üzerine** kutusu.
6.  **Karşıya yükleme** TrustFrameworkExtensions.xml ve doğrulama başlayabildiğinden emin olun.

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Şimdi Çalıştır kullanarak test özel profil Düzenleme İlkesi

1.  Açık **Azure AD B2C ayarlarını** gidin **kimlik deneyimi çerçevesi**.
2.  Açık **B2C_1A_ProfileEdit**, karşıya yüklediğiniz bağlı olan taraf (RP) özel ilke. Seçin **Şimdi Çalıştır**.
3.  Google + hesabını kullanarak oturum açabilir olması gerekir.

## <a name="download-the-complete-policy-files"></a>Tüm ilke dosyalarını indirme
İsteğe bağlı: Bu örnek dosyalar yerine dosyaları ile çalışmaya başlama özel ilkeleri tamamladıktan sonra size yol kendi özel İlkesi kullanarak kendi senaryonuza yapı öneririz.  [Başvuru için örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-goog-app)
