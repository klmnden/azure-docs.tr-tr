---
title: "Azure Active Directory B2C: Özel ilkeler kullanarak LinkedIn OAuth2 kimlik sağlayıcısı olarak ekleyin"
description: "Özel ilkeler ve OAuth2 protokolü kullanarak bir LinkedIn uygulamasını ayarlama hakkında nasıl yapılır makalesi"
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
ms.date: 10/23/2017
ms.author: yoelh
ms.openlocfilehash: 77e2b9b283e4051370ffb905681135c27512834e
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2018
---
# <a name="azure-active-directory-b2c-add-linkedin-as-an-identity-provider-by-using-custom-policies"></a>Azure Active Directory B2C: LinkedIn özel ilkeler kullanarak bir kimlik sağlayıcısı ekleyin
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede kullanarak oturum açma LinkedIn hesabı kullanıcıları için etkinleştirmek gösterilmiştir [özel ilkeler](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Önkoşullar
Bölümündeki adımları tamamlamanız [özel ilkelerini kullanmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

## <a name="step-1-create-a-linkedin-account-application"></a>1. adım: LinkedIn hesap uygulama oluşturma
Azure Active Directory B2C, kimlik sağlayıcısı LinkedIn kullanmak için (Azure AD B2C) LinkedIn uygulama oluşturmalı ve doğru parametreleri sağlayın. Bir LinkedIn uygulaması giderek kaydedebilirsiniz [LinkedIn kayıt sayfasına](https://LinkedIn.com/signup).

1. Git [LinkedIn Uygulama Yönetimi](https://www.linkedin.com/secure/developer?newapp=) Web sitesi, LinkedIn hesabı kimlik bilgilerinizle oturum açın ve ardından **uygulama oluştur**.

    ![LinkedIn hesap - uygulama oluşturma](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app1.png)

2. Üzerinde **yeni bir uygulama oluşturmak** sayfasında, aşağıdakileri yapın:

    a. Türü, **şirket adı**, bir açıklayıcı **adı** şirket için ve bir **açıklama** yeni uygulamanızın.

    b. Karşıya yükleme, **uygulama logosu**.

    c. Seçin bir **uygulama kullanımı**.

    d. İçinde **Web sitesi URL'si** kutusunda, yapıştırma  **https://login.microsoftonline.com** .

    e. Türü, **iş e-posta** adresi ve **iş telefonu** numarası.

    f. Sayfanın alt kısmındaki okuyun ve kullanım koşullarını kabul edin ve ardından **gönderme**.

    ![LinkedIn hesap - uygulama özelliklerini yapılandırma](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app2.png)

3. Seçin **kimlik doğrulaması**ve ardından Not **istemci kimliği** ve **gizli** değerleri.

4. İçinde **yönlendirme URL'si yetkili** kutusunda, yapıştırma  **https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/oauth2/authresp** . Yerine {*Kiracı*} Kiracı adınız (örneğin, contosob2c.onmicrosoft.com) sahip. HTTPS şeması kullandığınızdan emin olun. 

    ![LinkedIn hesabı - yetkili kümesi yeniden yönlendirme URL'leri](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app3.png)

    >[!NOTE]
    >Gizli bir önemli güvenlik kimlik bilgisidir. Bu gizli kimseyle paylaşmayın değil veya uygulamanızla dağıtabilirsiniz.

5. **Add (Ekle)** seçeneğini belirleyin.

6. Seçin **ayarları**, değiştirme **uygulama durumu** için **canlı**ve ardından **güncelleştirme**.

    ![LinkedIn hesabı - uygulamanın durumunu ayarla](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app4.png)

## <a name="step-2-add-your-linkedin-application-key-to-azure-ad-b2c"></a>2. adım: Azure AD B2C'ye LinkedIn uygulama anahtarınızı ekleme
LinkedIn hesaplarıyla federasyon güven Azure AD B2C uygulama adına LinkedIn hesabına istemci gizli anahtarı gerektirir. Azure AD B2C kiracınızda LinkedIn uygulama gizli anahtarı depolamak için aşağıdakileri yapın:  

1. Azure AD B2C kiracınızda seçin **B2C ayarlarını** > **kimlik deneyimi Framework**.

2. Kiracınızda kullanılabilir anahtarlarını görüntülemek için seçin **İlkesi anahtarları**.

3. **Add (Ekle)** seçeneğini belirleyin.

4. İçinde **seçenekleri** kutusunda **karşıya**.

5. İçinde **adı** kutusuna **B2cRestClientCertificate**.  
    Önek *B2C_1A_* otomatik olarak eklenebilir.

6. İçinde **gizli** kutusuna, LinkedIn uygulama parolanızı girin [uygulama kayıt portalı](https://apps.dev.microsoft.com).

7. İçin **anahtar kullanımı**seçin **şifreleme**.

8. **Oluştur**’u seçin. 

9. Oluşturduğunuz olduğunu onaylayın `B2C_1A_LinkedInSecret`anahtarı.

## <a name="step-3-add-a-claims-provider-in-your-extension-policy"></a>3. adım: bir talep sağlayıcı uzantısı ilkenizde ekleme
LinkedIn hesabını kullanarak oturum açmalarını istiyorsanız, bir talep sağlayıcısı olarak LinkedIn tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C iletişim kurduğu uç noktaları belirtmeniz gerekir. Uç noktaları Azure AD B2C tarafından belirli bir kullanıcı kimliği doğrulanmış olduğunu doğrulamak için kullanılan talep kümesi sağlar.

LinkedIn ekleyerek bir talep sağlayıcısı olarak tanımlayan bir `<ClaimsProvider>` İlkesi uzantısının düğümünde:

1. Çalışma dizininizi açın *TrustFrameworkExtensions.xml* uzantı ilke dosyası. 

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

4. Değiştir *client_id* LinkedIn uygulamanın istemci kimliğini değerle

5. Dosyayı kaydedin.

## <a name="step-4-register-the-linkedin-account-claims-provider"></a>4. adım: LinkedIn hesap talep sağlayıcısını Kaydet
Kimlik sağlayıcısı'nı ayarlama. Ancak, henüz windows oturum açma kayıt olması veya hiçbirinde kullanılabilir değil. LinkedIn hesabı kimlik sağlayıcısı, kullanıcı eklemelisiniz artık `SignUpOrSignIn` kullanıcı gezisine.

### <a name="step-41-make-a-copy-of-the-user-journey"></a>4.1. adım: kullanıcı gezisine bir kopyasını oluşturun
Kullanıcı gezisine kullanılabilir yapmak için var olan bir kullanıcı gezisine şablonunun bir kopyasını oluşturun ve LinkedIn kimlik sağlayıcısı ekleyin:

>[!NOTE]
>Kopyaladığınız varsa `<UserJourneys>` ilkenizin için temel dosyanın öğesinden *TrustFrameworkExtensions.xml* uzantı dosyası, bu bölümü atlayabilirsiniz.

1. Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.

2. Arama `<UserJourneys>` öğesi, tüm içeriğini seçin `<UserJourney>` düğümünü ve ardından **Kes** seçili metni panoya taşımak için.

3. Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve arama `<UserJourneys>` öğesi. Öğe yoksa, ekleyin.

4. Tüm içeriğini yapıştırın `<UserJourney>` 2. adımda panoya içine taşıdığınız düğümü `<UserJourneys>` öğesi.

### <a name="step-42-display-the-button"></a>4.2. adım: Ekran "button"
`<ClaimsProviderSelections>` Öğesi talep sağlayıcısı seçme seçenekleri ve bunların sırası listesini tanımlar. `<ClaimsProviderSelection>` Düğümdür kaydolma veya oturum açma sayfasında bir kimlik sağlayıcısı düğmesini benzer. Eklerseniz bir `<ClaimsProviderSelection>` düğümü LinkedIn hesabı, yeni bir düğme için bir kullanıcı sayfada adlandırıldığını olduğunda görüntülenir. Bu öğe eklemek için aşağıdakileri yapın:

1. Arama `<UserJourney>` içeren düğümü `Id="SignUpOrSignIn"` kopyaladığınız kullanıcı gezisine içinde.

2. Bulun `<OrchestrationStep>` içeren düğüm `Order="1"`.

3. İçinde `<ClaimsProviderSelections>` öğesi, aşağıdaki XML parçacığını ekleyin:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    ```

### <a name="step-43-link-the-button-to-an-action"></a>4.3. adım: Bağlantı için bir eylem düğmesi
Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için LinkedIn hesabıyla iletişim kurmak Azure AD B2C içindir. Düğme, LinkedIn hesap talep sağlayıcısı teknik profili bağlayarak bir eyleme bağlantı:

1. Arama `<OrchestrationStep>` içeren düğümü `Order="2"` içinde `<UserJourney>` düğümü.

2. İçinde `<ClaimsExchanges>` öğesi, aşağıdaki XML parçacığını ekleyin:

    ```xml
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAuth" />
    ```

    >[!NOTE]
    >* Emin `Id` , aynı değere sahip `TargetClaimsExchangeId` önceki bölümde.
    >* Emin `TechnicalProfileReferenceId` önceki (LinkedIn-OAuth) oluşturulan kimliği teknik profiline ayarlanır.

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
    Artık LinkedIn hesabını kullanarak oturum açabilir olması gerekir.

## <a name="step-7-optional-register-the-linkedin-account-claims-provider-to-the-profile-edit-user-journey"></a>7. adım: (İsteğe bağlı) kaydı LinkedIn hesabı profil düzenleme kullanıcı gezisine sağlayıcısına talepleri
LinkedIn hesabı kimlik sağlayıcısı eklemek isteyebilirsiniz, `ProfileEdit` kullanıcı gezisine. Kullanılabilir, yineleme "Adım 4." gezisine kullanıcı yapma Bu süre, select `<UserJourney>` içeren düğümü `Id="ProfileEdit"`. Kaydetme, karşıya yükleme ve ilkeniz test.

## <a name="optional-download-the-complete-policy-files"></a>(İsteğe bağlı) Tam ilke dosyaları indirme
Tamamlandıktan sonra [özel ilkelerini kullanmaya başlama](active-directory-b2c-get-started-custom.md) gözden geçirme, öneririz, kendi özel ilke dosyalarını kullanarak senaryonuz derleme. Başvuru için sağladık [örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-li-app).
