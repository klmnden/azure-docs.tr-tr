---
title: LinkedIn OAuth2 kimlik sağlayıcısı olarak Azure Active Directory B2C'de özel ilkeler kullanarak ekleyin | Microsoft Docs
description: Özel ilkeler ve OAuth2 protokolünü kullanarak bir LinkedIn uygulama ayarlama hakkında bir nasıl yapılır makalesi.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/23/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 334f696d79cf801facf7c5301b2240b69f7134f7
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444387"
---
# <a name="azure-active-directory-b2c-add-linkedin-as-an-identity-provider-by-using-custom-policies"></a>Azure Active Directory B2C: Özel ilkeler kullanarak LinkedIn kimlik sağlayıcısı olarak Ekle
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede bir LinkedIn hesabıyla kullanıcıları için oturum açma kullanarak etkinleştirmek gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Önkoşullar
Bölümündeki adımları tamamlamanız [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

## <a name="step-1-create-a-linkedin-account-application"></a>1. adım: bir LinkedIn hesabı uygulaması oluşturma
LinkedIn Azure Active Directory B2C'de kimlik sağlayıcısı olarak kullanmak için (Azure AD B2C), LinkedIn uygulama oluşturmalı ve doğru parametreleri sağlayın. Bir LinkedIn uygulama giderek kaydedebilirsiniz [LinkedIn kayıt sayfasına](https://www.linkedin.com/start/join).

1. Git [LinkedIn Uygulama Yönetimi](https://www.linkedin.com/secure/developer?newapp=) Web sitesi, LinkedIn hesabı kimlik bilgilerinizle oturum açın ve ardından **uygulama oluşturma**.

    ![LinkedIn hesabı - uygulama oluşturma](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app1.png)

2. Üzerinde **yeni bir uygulama oluşturma** sayfasında, aşağıdakileri yapın:

    a. Tür, **şirket adı**, açıklayıcı bir **adı** şirket için ve bir **açıklama** yeni uygulama.

    b. Karşıya yükleme, **uygulama logosu**.

    c. Seçin bir **uygulama kullanımı**.

    d. İçinde **Web sitesi URL'si** kutusu, yapıştırma **https://login.microsoftonline.com**.

    e. Tür, **iş e-posta** adresi ve **iş telefonu** sayı.

    f. Sayfanın en altında okuyun ve kullanım koşullarını kabul edin ve ardından **Gönder**.

    ![LinkedIn hesabı - uygulama özelliklerini yapılandırma](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app2.png)

3. Seçin **kimlik doğrulaması**ve ardından unutmayın **istemci kimliği** ve **gizli** değerleri.

4. İçinde **yeniden yönlendirme URL'lerini yetkili** kutusu, yapıştırma **https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/oauth2/authresp**. Yerine {*Kiracı*} Kiracı adınızın (örneğin, contosob2c.onmicrosoft.com). HTTPS şeması kullandığınızdan emin olun. 

    ![LinkedIn hesabı - yetkili kümesi yeniden yönlendirme URL'leri](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app3.png)

    >[!NOTE]
    >Gizli bir önemli güvenlik kimlik bilgisidir. Değil Bu gizli dizi kimseyle paylaşmayın veya uygulamanızla birlikte dağıtmayın.

5. **Add (Ekle)** seçeneğini belirleyin.

6. Seçin **ayarları**, değiştirme **uygulama durumu** için **canlı**ve ardından **güncelleştirme**.

    ![LinkedIn hesabı - uygulama durumunu ayarla](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app4.png)

## <a name="step-2-add-your-linkedin-application-key-to-azure-ad-b2c"></a>2. adım: Azure AD B2C'ye, LinkedIn uygulama anahtarı ekleme
LinkedIn hesaplarının ile federasyon güven Azure AD B2C'yi uygulama adına LinkedIn hesabı istemci gizli anahtarı gerektirir. Azure AD B2C kiracınızda LinkedIn uygulama gizli anahtarı depolamak için aşağıdakileri yapın:  

1. Azure AD B2C kiracınızı seçin **B2C ayarlarını** > **kimlik deneyimi çerçevesi**.

2. Kiracınızda kullanılabilir anahtarlarını görüntülemek için seçin **ilke anahtarları**.

3. **Add (Ekle)** seçeneğini belirleyin.

4. İçinde **seçenekleri** kutusunda **karşıya**.

5. İçinde **adı** kutusuna **B2cRestClientCertificate**.  
    Önek *B2C_1A_* otomatik olarak eklenebilir.

6. İçinde **gizli** kutusuna, gelen LinkedIn uygulama gizli anahtarı girin [uygulama kayıt portalı](https://apps.dev.microsoft.com).

7. İçin **anahtar kullanımı**seçin **şifreleme**.

8. **Oluştur**’u seçin. 

9. Oluşturduğunuzu doğrulayın `B2C_1A_LinkedInSecret`anahtarı.

## <a name="step-3-add-a-claims-provider-in-your-extension-policy"></a>3. adım: bir talep sağlayıcı uzantısı ilkenizde ekleme
LinkedIn hesabını kullanarak oturum açmalarını istiyorsanız, bir talep sağlayıcısı olarak LinkedIn tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C ile iletişim kurabilen bitiş noktası belirtmeniz gerekir. Uç noktaları, Azure AD B2C tarafından belirli bir kullanıcı yapıldığını doğrulamak için kullanılan bir talepler kümesi sağlar.

LinkedIn ekleyerek bir talep sağlayıcısı olarak tanımlayan bir `<ClaimsProvider>` ilke uzantısının düğümünde:

1. Çalışma dizininizde açın *TrustFrameworkExtensions.xml* uzantı ilke dosyası. 

2. Arama `<ClaimsProviders>` öğesi.

3. İçinde `<ClaimsProviders>` öğesi, aşağıdaki XML parçacığını ekleyin: 

    ```xml
    <ClaimsProvider>
      <Domain>linkedin.com</Domain>
      <DisplayName>LinkedIn</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="LinkedIn-OAUTH">
          <DisplayName>LinkedIn</DisplayName>
          <Protocol Name="OAuth2" />
          <Metadata>
            <Item Key="ProviderName">linkedin</Item>
            <Item Key="authorization_endpoint">https://www.linkedin.com/oauth/v2/authorization</Item>
            <Item Key="AccessTokenEndpoint">https://www.linkedin.com/oauth/v2/accessToken</Item>
            <Item Key="ClaimsEndpoint">https://api.linkedin.com/v1/people/~:(id,first-name,last-name,email-address,headline)</Item>
            <Item Key="ClaimsEndpointAccessTokenName">oauth2_access_token</Item>
            <Item Key="ClaimsEndpointFormatName">format</Item>
            <Item Key="ClaimsEndpointFormat">json</Item>
            <Item Key="scope">r_emailaddress r_basicprofile</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <Item Key="client_id">Your LinkedIn application client ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_LinkedInSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="emailAddress" />
            <!--<OutputClaim ClaimTypeReferenceId="jobTitle" PartnerClaimType="headline" />-->
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="linkedin.com" />
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

4. Değiştirin *client_id* LinkedIn uygulama istemci kimliğinizi değeriyle

5. Dosyayı kaydedin.

## <a name="step-4-register-the-linkedin-account-claims-provider"></a>4. adım: LinkedIn hesabı talep sağlayıcısı kaydetme
Kimlik sağlayıcısını ayarlama. Ancak, henüz kaydolma veya oturum açma windows hiçbirinde kullanılamaz. LinkedIn hesabı kimlik sağlayıcısı, kullanıcıya eklemelisiniz artık `SignUpOrSignIn` kullanıcı yolculuğu.

### <a name="step-41-make-a-copy-of-the-user-journey"></a>4.1. adım: kullanıcı yolculuğu bir kopyasını
Kullanıcı yolculuğu kullanılabilir hale getirmek için var olan bir kullanıcı yolculuğu şablonunun bir kopyasını oluşturun ve ardından LinkedIn kimlik sağlayıcısı ekleyin:

>[!NOTE]
>Kopyaladığınız varsa `<UserJourneys>` öğesini ilkenizin için temel dosyasından *TrustFrameworkExtensions.xml* uzantı dosyası, bu bölümü atlayabilirsiniz.

1. Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.

2. Arama `<UserJourneys>` öğesi içeriğinin tamamını seçmek `<UserJourney>` düğümüne tıklayın ve ardından **Kes** seçili metni panoya taşımak için.

3. Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve arama `<UserJourneys>` öğesi. Öğe yoksa, bunu ekleyin.

4. Tüm içeriğini yapıştırın `<UserJourney>` içine 2. adımda panoya taşınan düğümü `<UserJourneys>` öğesi.

### <a name="step-42-display-the-button"></a>4.2. adım: Ekran "button"
`<ClaimsProviderSelections>` Öğe talep sağlayıcısı seçme seçenekleri ve bunların sırası listesini tanımlar. `<ClaimsProviderSelection>` Düğümüdür kaydolma veya oturum açma sayfasında bir kimlik sağlayıcısı düğmesi benzer. Eklerseniz bir `<ClaimsProviderSelection>` sayfasında bir kullanıcı gölünüzdeki attığında bir LinkedIn hesabıyla yeni düğme düğümü görüntülenir. Bu öğe eklemek için aşağıdakileri yapın:

1. Arama `<UserJourney>` içeren düğüm `Id="SignUpOrSignIn"` kopyaladığınız kullanıcı giden.

2. Bulun `<OrchestrationStep>` içeren düğüm `Order="1"`.

3. İçinde `<ClaimsProviderSelections>` öğesi, aşağıdaki XML parçacığını ekleyin:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    ```

### <a name="step-43-link-the-button-to-an-action"></a>4.3. adım: düğme için eylem bağlantı
Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için LinkedIn hesabıyla iletişim kurmak için Azure AD B2C içindir. Düğme, LinkedIn hesabı talep sağlayıcısı teknik profil bağlayarak bir eyleme bağlantı:

1. Arama `<OrchestrationStep>` içeren düğüm `Order="2"` içinde `<UserJourney>` düğümü.

2. İçinde `<ClaimsExchanges>` öğesi, aşağıdaki XML parçacığını ekleyin:

    ```xml
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAuth" />
    ```

    >[!NOTE]
    >* Emin `Id` , aynı değere sahip `TargetClaimsExchangeId` önceki bölümde.
    >* Emin `TechnicalProfileReferenceId` kimliği, önceki (LinkedIn-OAuth) oluşturduğunuz teknik profiline ayarlanır.

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
    Şimdi LinkedIn hesabını kullanarak oturum açabilir olmalıdır.

## <a name="step-7-optional-register-the-linkedin-account-claims-provider-to-the-profile-edit-user-journey"></a>7. adım: (İsteğe bağlı) kaydı LinkedIn hesabı sağlayıcısına profil düzenleme kullanıcı yolculuğunun talep
LinkedIn hesabı kimlik sağlayıcısına eklemek isteyebilirsiniz, `ProfileEdit` kullanıcı yolculuğu. Kullanılabilir, yineleme "4. adım." yolculuğu kullanıcı yapma Bu kez, select `<UserJourney>` içeren düğüm `Id="ProfileEdit"`. Kaydetme, karşıya yükleme ve ilkeniz test.

## <a name="optional-download-the-complete-policy-files"></a>(İsteğe bağlı) Tüm ilke dosyalarını indirme
Tamamladıktan sonra [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md) izlenecek yol, öneririz senaryonuz kendi özel ilke dosyalarını kullanarak oluşturun. Referans olması açısından sağladık [örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-li-app).
