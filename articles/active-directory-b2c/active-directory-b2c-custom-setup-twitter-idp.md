---
title: Azure Active Directory B2C'de özel ilkeler kullanarak OAuth1 kimlik sağlayıcısı olarak twitter ekleyin | Microsoft Docs
description: Twitter OAuth1 protokolünü kullanarak bir kimlik sağlayıcısı olarak kullanın.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/23/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 28679ef07c2625908f7b08f808ff49c48ddb625b
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43339877"
---
# <a name="azure-active-directory-b2c-add-twitter-as-an-oauth1-identity-provider-by-using-custom-policies"></a>Azure Active Directory B2C: Özel ilkeler kullanarak Twitter OAuth1 kimlik sağlayıcısı olarak Ekle
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede bir Twitter hesabı kullanıcıları için oturum açma kullanarak etkinleştirmek gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Önkoşullar
Bölümündeki adımları tamamlamanız [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

## <a name="step-1-create-a-twitter-account-application"></a>1. adım: Twitter hesabı uygulaması oluşturma
Azure Active Directory B2C'de kimlik sağlayıcısı olarak twitter'ı kullanmak için (Azure AD B2C), bir Twitter uygulaması oluşturmalı ve doğru parametreleri sağlayın. Bir Twitter uygulaması giderek kaydedebilirsiniz [Twitter kayıt sayfasına](https://twitter.com/signup).

1. Git [Twitter geliştiriciler](https://apps.twitter.com/) Web sitesi, Twitter hesabı kimlik bilgilerinizle oturum açın ve ardından **yeni uygulama oluştur**.

    ![Hesap twitter - yeni uygulama oluşturma](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app1.png)

2. İçinde **uygulama oluşturma** penceresinde aşağıdakileri yapın:
 
    a. Tür **adı** ve **açıklama** yeni uygulamanız için. 

    b. İçinde **Web sitesi** kutusu, yapıştırma **https://{tenant}.b2clogin.com**. Burada **{tenant}** kiracınızın adıdır (örneğin, https://contosob2c.b2clogin.com).

    c. 4. İçin **geri çağırma URL'si**, girin `https://{tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/{policyId}/oauth1/authresp`. Değiştirdiğinizden emin olun **{tenant}** kiracınızın adı (örneğin, contosob2c) ile ve **{Policyıd}** ilke kimliğinizle (örneğin, b2c_1_policy).  **Geri çağırma URL'si tümü küçük harf olması gerekir.** Bir geri çağırma URL'si için Twitter oturum açma kullanan tüm ilkeleri eklemeniz gerekir. Kullandığınızdan emin olun `b2clogin.com` yerine ` login.microsoftonline.com` uygulamanızda kullanıyorsanız.

    d. Sayfanın altındaki okuyun ve koşulları kabul edin ve ardından **kendi Twitter uygulamanızı oluşturun**.

    ![Hesap twitter - yeni bir uygulama ekleme](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app2.png)

3. İçinde **B2C tanıtım** penceresinde **ayarları**seçin **Twitter ile oturum aç imzalamak için bu uygulamanın izin** onay kutusunu işaretleyin ve ardından **güncelleştirme Ayarları**.

4. Seçin **anahtarlar ve erişim belirteçleri**ve Not **tüketici anahtarı (API anahtarı)** ve **tüketici gizli anahtarı (API gizli anahtarı)** değerleri.

    ![Twitter hesabı - uygulama özelliklerini ayarlama](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app3.png)

    >[!NOTE]
    >Tüketici gizli bir önemli güvenlik kimlik bilgisidir. Değil Bu gizli dizi kimseyle paylaşmayın veya uygulamanızla birlikte dağıtmayın.

## <a name="step-2-add-your-twitter-account-application-key-to-azure-ad-b2c"></a>2. adım: Twitter hesabı uygulama anahtarınızı Azure AD B2C'ye ekleyin.
Twitter hesapları ile federasyon güven Azure AD B2C'yi uygulama adına Twitter hesabınıza bir tüketici gizli anahtarı gerektirir. Azure AD B2C kiracınızda uygulama Twitter tüketici gizli anahtarı depolamak için aşağıdakileri yapın: 

1. Azure AD B2C kiracınızı seçin **B2C ayarlarını** > **kimlik deneyimi çerçevesi**.

2. Kiracınızda kullanılabilir anahtarlarını görüntülemek için seçin **ilke anahtarları**.

3. **Add (Ekle)** seçeneğini belirleyin.

4. İçinde **seçenekleri** kutusunda **el ile**.

5. İçinde **adı** kutusunda **TwitterSecret**.  
    Önek *B2C_1A_* otomatik olarak eklenebilir.

6. İçinde **gizli** kutusuna, gelen Microsoft uygulama gizli anahtarı girin [uygulama kayıt portalı](https://apps.dev.microsoft.com).

7. İçin **anahtar kullanımı**, kullanın **şifreleme**.

8. **Oluştur**’u seçin.

9. Oluşturduğunuzu doğrulayın `B2C_1A_TwitterSecret` anahtarı.

## <a name="step-3-add-a-claims-provider-in-your-extension-policy"></a>3. adım: bir talep sağlayıcı uzantısı ilkenizde ekleme

Twitter hesabı kullanarak oturum açmalarını istiyorsanız, bir talep sağlayıcısı olarak Twitter tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C ile iletişim kurabilen bitiş noktası belirtmeniz gerekir. Uç noktaları, Azure AD B2C tarafından belirli bir kullanıcı yapıldığını doğrulamak için kullanılan bir talepler kümesi sağlar.

Ekleyerek bir talep sağlayıcısı olarak twitter tanımlamak `<ClaimsProvider>` ilke uzantısının düğümünde:

1. Çalışma dizininizde açın *TrustFrameworkExtensions.xml* uzantı ilke dosyası. 

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

4. Değiştirin *client_id*' değeri ile Twitter hesabı uygulama tüketici anahtarı.

5. Dosyayı kaydedin.

## <a name="step-4-register-the-twitter-account-claims-provider-to-your-sign-up-or-sign-in-user-journey"></a>4. adım: Twitter hesabı talep sağlayıcısı kullanarak kaydolma veya oturum açma kullanıcı yolculuğu için kaydolun
Kimlik sağlayıcısını ayarlama. Ancak, henüz kaydolma veya oturum açma windows hiçbirinde kullanılamaz. Twitter hesabı kimlik sağlayıcısı, kullanıcıya eklemelisiniz artık `SignUpOrSignIn` kullanıcı yolculuğu.

### <a name="step-41-make-a-copy-of-the-user-journey"></a>4.1. adım: kullanıcı yolculuğu bir kopyasını
Kullanıcı yolculuğu kullanılabilir hale getirmek için var olan bir kullanıcı yolculuğu şablonunun bir kopyasını oluşturun ve ardından Twitter kimlik sağlayıcısı ekleyin:

>[!NOTE]
>Kopyaladığınız varsa `<UserJourneys>` öğesini ilkenizin için temel dosyasından *TrustFrameworkExtensions.xml* uzantı dosyası, sonraki bölüme atlayabilirsiniz.

1. Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.

2. Arama `<UserJourneys>` öğesi içeriğinin tamamını seçmek `<UserJourney>` düğümüne tıklayın ve ardından **Kes** seçili metni panoya taşımak için.

3. Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve ardından arama `<UserJourneys>` öğesi. Öğe yoksa, bunu ekleyin.

4. Tüm içeriğini yapıştırın `<UserJourney>` içine 2. adımda panoya taşınan düğümü `<UserJourneys>` öğesi.

### <a name="step-42-display-the-button"></a>4.2. adım: Ekran "button"
`<ClaimsProviderSelections>` Öğe talep sağlayıcısı seçme seçenekleri ve bunların sırası listesini tanımlar. `<ClaimsProviderSelection>` Düğümüdür kaydolma veya oturum açma sayfasında bir kimlik sağlayıcısı düğmesi benzer. Eklerseniz bir `<ClaimsProviderSelection>` düğümü Twitter hesabıyla, yeni bir düğme için sayfada bir kullanıcı gölünüzdeki olduğunda görüntülenir. Bu öğe eklemek için aşağıdakileri yapın:

1. Arama `<UserJourney>` içeren düğüm `Id="SignUpOrSignIn"` kopyaladığınız kullanıcı giden.

2. Bulun `<OrchestrationStep>` içeren düğüm `Order="1"`.

3. İçinde `<ClaimsProviderSelections>` öğesi, aşağıdaki XML parçacığını ekleyin:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
    ```

### <a name="step-43-link-the-button-to-an-action"></a>4.3. adım: düğme için eylem bağlantı
Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için Twitter hesabıyla iletişim kurmak için Azure AD B2C içindir. Düğmesi, Twitter hesabı talep sağlayıcısı teknik profil bağlayarak bir eyleme bağlantı:

1. Arama `<OrchestrationStep>` içeren düğüm `Order="2"` içinde `<UserJourney>` düğümü.
2. İçinde `<ClaimsExchanges>` öğesi, aşağıdaki XML parçacığını ekleyin:

    ```xml
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAUTH1" />
    ```

    >[!NOTE]
    >* Emin `Id` , aynı değere sahip `TargetClaimsExchangeId` önceki bölümde.
    >* Emin `TechnicalProfileReferenceId` kimliği, önceki (Twitter-OAUTH1) oluşturduğunuz teknik profiline ayarlanır.

## <a name="step-5-upload-the-policy-to-your-tenant"></a>5. adım: ilke kiracınıza karşıya yükleyin.
1. İçinde [Azure portalında](https://portal.azure.com), geçiş [Azure AD B2C kiracınızın bağlamında](active-directory-b2c-navigate-to-b2c-context.md)ve ardından **Azure AD B2C**.

2. Seçin **kimlik deneyimi çerçevesi**.

3. Seçin **tüm ilkeleri**.

4. Seçin **karşıya yükleme İlkesi**.

5. Seçin **ilke varsa üzerine** onay kutusu.

6. Karşıya yükleme *TrustFrameworkBase.xml* ve *TrustFrameworkExtensions.xml* dosyaları ve doğrulamasını geçmesi emin olun.

## <a name="step-6-test-the-custom-policy-by-using-run-now"></a>6. adım: Test Şimdi Çalıştır kullanarak özel ilke

1. Seçin **Azure AD B2C ayarlarını**ve ardından **kimlik deneyimi çerçevesi**.

    >[!NOTE]
    >Çalıştırma artık Kiracı'da önceden kayıtlı için en az bir uygulama gerektirir. Azure AD B2C uygulamaları kaydetme hakkında bilgi için bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makalesi.

2. Açık **B2C_1A_signup_signin**, yüklenmiş ve ardından bağlı olan taraf (RP) özel ilke **Şimdi Çalıştır**.  
    Artık Twitter hesabıyla oturum açabilir olmalıdır.

## <a name="step-7-optional-register-the-twitter-account-claims-provider-to-the-profile-edit-user-journey"></a>7. adım: (İsteğe bağlı) kaydı Twitter hesabı sağlayıcısına profil düzenleme kullanıcı yolculuğunun talep
Twitter hesabı kimlik sağlayıcısına eklemek isteyebilirsiniz, `ProfileEdit` kullanıcı yolculuğu. Kullanılabilir, yineleme "4. adım." yolculuğu kullanıcı yapma Bu kez, select `<UserJourney>` içeren düğüm `Id="ProfileEdit"`. Kaydetme, karşıya yükleme ve ilkeniz test.


## <a name="optional-download-the-complete-policy-files"></a>(İsteğe bağlı) Tüm ilke dosyalarını indirme
Tamamladıktan sonra [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md) izlenecek yol, öneririz senaryonuz kendi özel ilke dosyalarını kullanarak oluşturun. Referans olması açısından sağladık [örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-twitter-app).
