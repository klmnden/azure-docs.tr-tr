---
title: Özel ilkeleri kullanarak Azure Active Directory B2C bir LinkedIn hesabı ile oturum açma ayarlama | Microsoft Docs
description: Özel ilkeleri kullanarak, Azure Active Directory B2C, bir Google hesabı ile oturum açma ayarlayın.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 4817ad918af66080cec1faead96c6c9448387556
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181219"
---
# <a name="set-up-sign-in-with-a-linkedin-account-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeleri kullanarak bir LinkedIn hesabıyla oturum açma özelliğini ayarlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede bir LinkedIn hesabıyla kullanıcılar için oturum açma kullanarak etkinleştirmek gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md) Azure Active Directory (Azure AD) B2C'de.

## <a name="prerequisites"></a>Önkoşullar

- Bölümündeki adımları tamamlamanız [özel ilkeleri Azure Active Directory B2C kullanmaya başlama](active-directory-b2c-get-started-custom.md).
- Bir LinkedIn hesabınız yoksa, tek tek oluşturun [LinkedIn kayıt sayfasına](https://www.linkedin.com/start/join).
- Bir LinkedIn uygulama uygulamanızı temsil etmek için 80 X 80 piksel logosu görüntü vermenizi gerektirir.

## <a name="create-an-application"></a>Uygulama oluşturma

LinkedIn bir Azure AD B2C kimlik sağlayıcısı olarak kullanmak için LinkedIn uygulama oluşturmanız gerekir.

1. Oturum [LinkedIn Uygulama Yönetimi](https://www.linkedin.com/secure/developer?newapp=) LinkedIn hesabınızın kimlik bilgileriyle Web sitesi.
2. Seçin **uygulama oluşturma**.
3. Girin, **şirket adı**e **uygulama adı**ve bir **uygulama açıklaması**.
4. Karşıya yükleme **uygulama logosu** oluşturduğunuz.
5. Seçin bir **uygulama kullanımı** sağlanan listeden.
6. İçin **Web sitesi URL'si**, girin `https://your-tenant.b2clogin.com`.  Değiştirin `your-tenant` Azure AD B2C kiracınızın adı. Örneğin, contoso.b2clogin.com.
7. Girin, **iş e-posta** adresi ve **iş telefonu** sayı.
8. Sayfanın en altında okuyun ve kullanım koşullarını kabul edin ve ardından **Gönder**.
9. Seçin **kimlik doğrulaması**ve ardından kaydı **istemci kimliği** ve **gizli** değerleri daha sonra kullanmak üzere.
10. İçinde **yeniden yönlendirme URL'lerini yetkili**, girin `https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/oauth2/authresp`. Değiştirin `your-tenant-name` kiracınızın ada sahip. Tüm harfleri büyük harflerle Azure AD B2C ile Kiracı tanımlansa bile Kiracı adınızın girerken kullanmanız gerekir. 
11. Seçin **güncelleştirme**.
12. Seçin **ayarları**, değiştirme **uygulama durumu** için **canlı**ve ardından **güncelleştirme**.

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Azure AD B2C kiracınıza daha önce kaydettiğiniz istemci gizli anahtarı depolamak gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Genel bakış sayfasında **kimlik deneyimi çerçevesi - PREVIEW**.
5. Seçin **ilke anahtarları** seçip **Ekle**.
6. İçin **seçenekleri**, seçin `Manual`.
7. Girin bir **adı** ilke anahtarı. Örneğin, `LinkedInSecret`. Önek `B2C_1A_` anahtarınızı adına otomatik olarak eklenir.
8. İçinde **gizli**, daha önce kaydettiğiniz istemci gizli anahtarı girin.
9. İçin **anahtar kullanımı**seçin `Signature`.
10. **Oluştur**’a tıklayın.

## <a name="add-a-claims-provider"></a>Bir talep Sağlayıcı Ekle

LinkedIn hesabınızı kullanarak oturum açmasını istiyorsanız, Azure AD B2C'yi bir uç nokta ile iletişim kurabilen bir talep sağlayıcısı olarak hesabı tanımlamanız gerekir. Uç nokta, Azure AD B2C tarafından belirli bir kullanıcı yapıldığını doğrulamak için kullanılan bir talepler kümesi sağlar. 

Bir talep sağlayıcısı olarak ekleyerek bir LinkedIn hesabıyla tanımlayabilirsiniz **ClaimsProviders** ilkenizin uzantısı dosyasında öğe.

1. Açık *TrustFrameworkExtensions.xml*.
2. Bulma **ClaimsProviders** öğesi. Yoksa, kök öğe altında ekleyin.
3. Yeni bir **ClaimsProvider** gibi:

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

4. Değiştirin **client_id** daha önce kaydettiğiniz istemci kimliği.
5. Dosyayı kaydedin.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını karşıya yükle

Artık Azure AD B2C, LinkedIn hesabınızla iletişim kurma bilebilmesi ilkenizi yapılandırdınız. İlkenizin yalnızca bu sorunları şu ana kadar sahip olmadığını onaylamak için uzantı dosyasını karşıya yüklemeyi deneyin.

1. Üzerinde **özel ilkeleri** sayfa seçin, Azure AD B2C kiracınızın **karşıya yükleme İlkesi**.
2. Etkinleştirme **ilke varsa üzerine**, ardından gözatın ve seçin *TrustFrameworkExtensions.xml* dosya.
3. **Karşıya Yükle**'ye tıklayın.

## <a name="register-the-claims-provider"></a>Talep sağlayıcısı kaydetme

Bu noktada, kimlik sağlayıcısı ayarlandı, ancak herhangi bir kaydolma veya oturum açma ekranları kullanılabilir değil. Kullanılabilir hale getirmek için mevcut bir şablonu kullanıcı yolculuğu bir kopyasını oluşturun ve ayrıca LinkedIn kimlik sağlayıcısına sahip olacak şekilde değiştirin.

1. Açık *TrustFrameworkBase.xml* başlangıç paketi dosyasından.
2. Bul ve tüm içeriğini kopyalayın **UserJourney** içeren öğe `Id="SignUpOrSignIn"`.
3. Açık *TrustFrameworkExtensions.xml* ve bulma **UserJourneys** öğesi. Öğe yoksa bir tane ekleyin.
4. Tüm içeriğini yapıştırın **UserJourney** öğesi alt öğesi olarak kopyaladığınız **UserJourneys** öğesi.
5. Kullanıcı yolculuğu kimliği yeniden adlandırın. Örneğin, `SignUpSignInLinkedIn`.

### <a name="display-the-button"></a>Bir düğme görüntülemek

**ClaimsProviderSelection** öğedir bir kaydolma veya oturum açma ekranında bir kimlik sağlayıcısı düğmesini benzer. Eklerseniz bir **ClaimsProviderSelection** öğesi bir LinkedIn hesabı için yeni bir düğme gösterilir sayfasında bir kullanıcı gölünüzdeki olduğunda.

1. Bulma **OrchestrationStep** içeren öğe `Order="1"` , oluşturduğunuz kullanıcı giden.
2. Altında **ClaimsProviderSelects**, şu öğeyi ekleyin. Değerini **TargetClaimsExchangeId** örneğin uygun bir değere `LinkedInExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.

Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için bir LinkedIn hesabıyla iletişim kurmak için Azure AD B2C içindir.

1. Bulma **OrchestrationStep** içeren `Order="2"` kullanıcı giden.
2. Aşağıdaki **ClaimsExchange** öğe için aynı değeri kullanın sağlamaktan **kimliği** için kullanılan **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAUTH" />
    ```
    
    Değerini güncelleştirin **TechnicalProfileReferenceId** için **kimliği** daha önce oluşturduğunuz teknik profil. Örneğin, `LinkedIn-OAUTH`.

3. Kaydet *TrustFrameworkExtensions.xml* dosya ve doğrulama için yeniden yükleyin.

## <a name="update-and-test-the-relying-party-file"></a>Güncelleştirme ve bağlı olan taraf dosyayı test etme

Oluşturduğunuz kullanıcı yolculuğu başlatır bağlı olan taraf (RP) dosyasını güncelleştirin.

1. Bir kopyasını *SignUpOrSignIn.xml* çalışma dizininizdeki ve yeniden adlandırın. Örneğin, yeniden adlandırın *SignUpSignInLinkedIn.xml*.
2. Yeni dosyayı açın ve değeri güncelleştirme **Policyıd** özniteliğini **TrustFrameworkPolicy** benzersiz bir değere sahip. Örneğin, `SignUpSignInLinkedIn`.
3. Değerini güncelleştirin **PublicPolicyUri** ilkesi için URI ile. Örneğin,`http://contoso.com/B2C_1A_signup_signin_linkedin`
4. Değerini güncelleştirin **Referenceıd** özniteliğini **DefaultUserJourney** (SignUpSignLinkedIn) oluşturduğunuz yeni kullanıcı yolculuğu kimliği eşleştirmek için.
5. Yaptığınız değişiklikleri kaydedin, dosyayı karşıya yükleyin ve açıp tıklatarak test **Şimdi Çalıştır**.
