---
title: "Azure Active Directory B2C: Özel ilkeler kullanarak Twitter OAuth1 kimlik sağlayıcısı olarak ekleyin"
description: "Twitter OAuth1 protokolünü kullanarak bir kimlik sağlayıcısı kullanın."
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
ms.date: 10/23/2017
ms.author: yoelh
ms.openlocfilehash: f3a7936a468df7b0a2713f1f30c5b91e74d1d917
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-b2c-add-twitter-as-an-oauth1-identity-provider-by-using-custom-policies"></a>Azure Active Directory B2C: Özel ilkeler kullanarak Twitter OAuth1 kimlik sağlayıcısı olarak ekleyin
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede kullanarak oturum açma bir Twitter hesabı kullanıcıları için etkinleştirmek gösterilmiştir [özel ilkeler](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Ön koşullar
Bölümündeki adımları tamamlamanız [özel ilkelerini kullanmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

## <a name="step-1-create-a-twitter-account-application"></a>1. adım: bir Twitter hesabı uygulaması oluşturma
Azure Active Directory B2C, kimlik sağlayıcısı twitter kullanmak için (Azure AD B2C), bir Twitter uygulaması oluşturmalı ve doğru parametreleri sağlayın. Bir Twitter uygulaması giderek kaydedebilirsiniz [Twitter kayıt sayfasına](https://twitter.com/signup).

1. Git [Twitter geliştiriciler](https://apps.twitter.com/) Web sitesi, Twitter hesabı kimlik bilgilerinizle oturum açın ve ardından **yeni uygulama oluşturma**.

    ![Hesap twitter - yeni uygulama oluşturma](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app1.png)

2. İçinde **uygulama oluşturma** penceresinde aşağıdakileri yapın:
 
    a. Tür **adı** ve **açıklama** yeni uygulamanız için. 

    b. İçinde **Web sitesi** kutusunda, yapıştırma **https://login.microsoftonline.com**. 

    c. İçinde **geri çağırma URL'si** kutusunda, yapıştırma **https://login.microsoftonline.com/te/ {tenant}.onmicrosoft.com/oauth2/authresp**. Yerine {*Kiracı*} Kiracı adınız (örneğin, contosob2c.onmicrosoft.com) sahip. HTTPS şeması kullandığınızdan emin olun. 

    d. Sayfanın altındaki okuyun ve koşulları kabul edin ve ardından **Twitter uygulamanızı oluşturma**.

    ![Hesap twitter - yeni bir uygulama ekleme](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app2.png)

3. İçinde **B2C demo** penceresinde, seçin **ayarları**seçin **Twitter ile oturum aç imzalamak için kullanılan bu uygulamanın izin** onay kutusunu işaretleyin ve ardından **güncelleştirme Ayarları**.

4. Seçin **anahtarları ve erişim belirteçleri**ve not edin **tüketici anahtarı (API anahtarı)** ve **tüketici gizli anahtarı (API gizli)** değerleri.

    ![Twitter hesabı - uygulama özelliklerini ayarlama](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app3.png)

    >[!NOTE]
    >Tüketici gizli bir önemli güvenlik kimlik bilgisidir. Bu gizli kimseyle paylaşmayın değil veya uygulamanızla dağıtabilirsiniz.

## <a name="step-2-add-your-twitter-account-application-key-to-azure-ad-b2c"></a>2. adım: Azure AD B2C'ye Twitter hesabı uygulama anahtarınızı ekleme
Twitter hesaplarıyla federasyon güveni Azure AD B2C uygulama adına Twitter hesabına bir tüketici gizli anahtarı gerektirir. Azure AD B2C kiracınızda Twitter uygulamanın tüketici gizli anahtarı depolamak için aşağıdakileri yapın: 

1. Azure AD B2C kiracınızda seçin **B2C ayarlarını** > **kimlik deneyimi Framework**.

2. Kiracınızda kullanılabilir anahtarlarını görüntülemek için seçin **İlkesi anahtarları**.

3. **Add (Ekle)** seçeneğini belirleyin.

4. İçinde **seçenekleri** kutusunda **el ile**.

5. İçinde **adı** kutusunda **TwitterSecret**.  
    Önek *B2C_1A_* otomatik olarak eklenebilir.

6. İçinde **gizli** kutusuna, Microsoft uygulama parolanızı girin [uygulama kayıt portalı](https://apps.dev.microsoft.com).

7. İçin **anahtar kullanımı**, kullanın **şifreleme**.

8. **Oluştur**’u seçin.

9. Oluşturduğunuz olduğunu onaylayın `B2C_1A_TwitterSecret` anahtarı.

## <a name="step-3-add-a-claims-provider-in-your-extension-policy"></a>3. adım: bir talep sağlayıcı uzantısı ilkenizde ekleme

Twitter hesabı kullanarak oturum açmalarını istiyorsanız, bir talep sağlayıcısı olarak Twitter tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C iletişim kurduğu uç noktaları belirtmeniz gerekir. Uç noktaları Azure AD B2C tarafından belirli bir kullanıcı kimliği doğrulanmış olduğunu doğrulamak için kullanılan talep kümesi sağlar.

Twitter ekleyerek bir talep sağlayıcısı olarak tanımlamak `<ClaimsProvider>` İlkesi uzantısının düğümünde:

1. Çalışma dizininizi açın *TrustFrameworkExtensions.xml* uzantı ilke dosyası. 

2. Arama `<ClaimsProviders>` bölümü.

3. İçinde `<ClaimsProviders>` düğümü, aşağıdaki XML parçacığını ekleyin:  

    ```xml
    <ClaimsProvider>
        <Domain>twitter.com</Domain>
        <DisplayName>Twitter</DisplayName>
        <TechnicalProfiles>
        <TechnicalProfile Id="Twitter-OAUTH1">
            <DisplayName>Twitter</DisplayName>
            <Protocol Name="OAuth1" />
            <Metadata>
            <Item Key="ProviderName">Twitter</Item>
            <Item Key="authorization_endpoint">https://api.twitter.com/oauth/authenticate</Item>
            <Item Key="access_token_endpoint">https://api.twitter.com/oauth/access_token</Item>
            <Item Key="request_token_endpoint">https://api.twitter.com/oauth/request_token</Item>
            <Item Key="ClaimsEndpoint">https://api.twitter.com/1.1/account/verify_credentials.json?include_email=true</Item>
            <Item Key="ClaimsResponseFormat">json</Item>
            <Item Key="client_id">Your Twitter application consumer key</Item>
            </Metadata>
            <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_TwitterSecret" />
            </CryptographicKeys>
            <InputClaims />
            <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="user_id" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="screen_name" />
            <OutputClaim ClaimTypeReferenceId="email" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="twitter.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
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

4. Değiştir *client_id*' Twitter hesabı uygulama tüketici anahtarınız ile değer.

5. Dosyayı kaydedin.

## <a name="step-4-register-the-twitter-account-claims-provider-to-your-sign-up-or-sign-in-user-journey"></a>4. adım: kullanıcı oturum açma veya kaydolma Yolculuğunuzun Twitter hesabı talep sağlayıcısını Kaydet
Kimlik sağlayıcısı'nı ayarlama. Ancak, henüz windows oturum açma kayıt olması veya hiçbirinde kullanılabilir değil. Twitter hesabı kimlik sağlayıcısı, kullanıcı eklemelisiniz artık `SignUpOrSignIn` kullanıcı gezisine.

### <a name="step-41-make-a-copy-of-the-user-journey"></a>4.1. adım: kullanıcı gezisine bir kopyasını oluşturun
Kullanıcı gezisine kullanılabilir yapmak için var olan bir kullanıcı gezisine şablonunun bir kopyasını oluşturun ve Twitter kimlik sağlayıcısı ekleyin:

>[!NOTE]
>Kopyaladığınız varsa `<UserJourneys>` ilkenizin için temel dosyanın öğesinden *TrustFrameworkExtensions.xml* uzantı dosyası, sonraki bölüme atlayabilirsiniz.

1. Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.

2. Arama `<UserJourneys>` öğesi, tüm içeriğini seçin `<UserJourney>` düğümünü ve ardından **Kes** seçili metni panoya taşımak için.

3. Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve ardından aramak `<UserJourneys>` öğesi. Öğe yoksa, ekleyin.

4. Tüm içeriğini yapıştırın `<UserJourney>` 2. adımda panoya içine taşıdığınız düğümü `<UserJourneys>` öğesi.

### <a name="step-42-display-the-button"></a>4.2. adım: Ekran "button"
`<ClaimsProviderSelections>` Öğesi talep sağlayıcısı seçme seçenekleri ve bunların sırası listesini tanımlar. `<ClaimsProviderSelection>` Düğümdür kaydolma veya oturum açma sayfasında bir kimlik sağlayıcısı düğmesini benzer. Eklerseniz bir `<ClaimsProviderSelection>` düğümü yeni düğmesi bir Twitter hesabı için bir kullanıcı sayfada adlandırıldığını olduğunda görüntülenir. Bu öğe eklemek için aşağıdakileri yapın:

1. Arama `<UserJourney>` içeren düğümü `Id="SignUpOrSignIn"` kopyaladığınız kullanıcı gezisine içinde.

2. Bulun `<OrchestrationStep>` içeren düğümü `Order="1"`.

3. İçinde `<ClaimsProviderSelections>` öğesi, aşağıdaki XML parçacığını ekleyin:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
    ```

### <a name="step-43-link-the-button-to-an-action"></a>4.3. adım: Bağlantı için bir eylem düğmesi
Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için Twitter hesabıyla iletişim kurmak Azure AD B2C içindir. Düğmesi, Twitter hesabı talep sağlayıcısı teknik profili bağlayarak bir eyleme bağlantı:

1. Arama `<OrchestrationStep>` içeren düğümü `Order="2"` içinde `<UserJourney>` düğümü.
2. İçinde `<ClaimsExchanges>` öğesi, aşağıdaki XML parçacığını ekleyin:

    ```xml
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAUTH1" />
    ```

    >[!NOTE]
    >* Emin `Id` , aynı değere sahip `TargetClaimsExchangeId` önceki bölümde.
    >* Emin `TechnicalProfileReferenceId` önceki (Twitter-OAUTH1) oluşturulan kimliği teknik profiline ayarlanır.

## <a name="step-5-upload-the-policy-to-your-tenant"></a>5. adım: ilke kiracınız için karşıya yükleme
1. İçinde [Azure portal](https://portal.azure.com), geçiş [bağlamı Azure AD B2C kiracınızın](active-directory-b2c-navigate-to-b2c-context.md)ve ardından **Azure AD B2C**.

2. Seçin **kimlik deneyimi Framework**.

3. Seçin **tüm ilkeleri**.

4. Seçin **karşıya İlkesi**.

5. Seçin **varsa ilkesi üzerine** onay kutusu.

6. Karşıya yükleme *TrustFrameworkBase.xml* ve *TrustFrameworkExtensions.xml* dosyaları ve doğrulama geçirirler emin olun.

## <a name="step-6-test-the-custom-policy-by-using-run-now"></a>6. adım: Test Şimdi Çalıştır kullanarak özel İlkesi

1. Seçin **Azure AD B2C ayarlarını**ve ardından **kimlik deneyimi Framework**.

    >[!NOTE]
    >Çalışma şimdi Kiracı'preregistered için en az bir uygulama için gereklidir. Uygulamaları kaydetmek öğrenmek için Azure AD B2C bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makale.

2. Açık **B2C_1A_signup_signin**, karşıya ve ardından bağlı olan taraf (RP) özel ilkesini **Şimdi Çalıştır**.  
    Artık Twitter hesabı kullanarak oturum açabilir olması gerekir.

## <a name="step-7-optional-register-the-twitter-account-claims-provider-to-the-profile-edit-user-journey"></a>7. adım: (İsteğe bağlı) kaydı Twitter hesabı profil düzenleme kullanıcı gezisine sağlayıcısına talepleri
Twitter hesabı kimlik sağlayıcısı eklemek isteyebilirsiniz, `ProfileEdit` kullanıcı gezisine. Kullanılabilir, yineleme "Adım 4." gezisine kullanıcı yapma Bu süre, select `<UserJourney>` içeren düğümü `Id="ProfileEdit"`. Kaydetme, karşıya yükleme ve ilkeniz test.


## <a name="optional-download-the-complete-policy-files"></a>(İsteğe bağlı) Tam ilke dosyaları indirme
Tamamlandıktan sonra [özel ilkelerini kullanmaya başlama](active-directory-b2c-get-started-custom.md) gözden geçirme, öneririz, kendi özel ilke dosyalarını kullanarak senaryonuz derleme. Başvuru için sağladık [örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-twitter-app).
