---
title: Oturum açma bir Twitter hesabıyla Azure Active Directory B2C'de özel ilkeler kullanarak ayarlama | Microsoft Docs
description: Oturum açma bir Twitter hesabıyla Azure Active Directory B2C'de özel ilkeler kullanarak ayarlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: cc657e079949b8217031906efeb84049217d6493
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66510243"
---
# <a name="set-up-sign-in-with-a-twitter-account-by-using-custom-policies-in-azure-active-directory-b2c"></a>Oturum açma bir Twitter hesabıyla Azure Active Directory B2C'de özel ilkeler kullanarak ayarlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede bir Twitter hesabı kullanıcıları için oturum açma kullanarak etkinleştirmek gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md) Azure Active Directory (Azure AD) B2C'de.

## <a name="prerequisites"></a>Önkoşullar

- Bölümündeki adımları tamamlamanız [özel ilkeleri Azure Active Directory B2C kullanmaya başlama](active-directory-b2c-get-started-custom.md).
- Bir Twitter hesabı yoksa, oluşturun, [Twitter kayıt sayfasına](https://twitter.com/signup).

## <a name="create-an-application"></a>Uygulama oluşturma

Bir Azure AD B2C kimlik sağlayıcısı olarak twitter'ı kullanmak için bir Twitter uygulaması oluşturmanız gerekir.

1. Oturum [Twitter geliştiriciler](https://developer.twitter.com/en/apps) Twitter hesabınızın kimlik bilgileriyle Web sitesi.
2. Seçin **uygulama oluşturma**.
3. Girin bir **uygulama adı** ve **uygulama açıklaması**.
4. İçinde **Web sitesi URL'si**, girin `https://your-tenant.b2clogin.com`. Değiştirin `your-tenant` kiracınızın ada sahip. Örneğin, https://contosob2c.b2clogin.com.
5. İçin **geri çağırma URL'si**, girin `https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/your-policy-Id/oauth1/authresp`. Değiştirin `your-tenant` Kiracı adınızın adıyla ve `your-policy-Id` ilkenizin tanımlayıcısına sahip. Örneğin, `b2c_1A_signup_signin_twitter`. Tüm harfleri büyük harflerle Azure AD B2C ile Kiracı tanımlansa bile Kiracı adınızın girerken kullanmanız gerekir.
6. Sayfanın altındaki okuyun ve koşulları kabul edin ve ardından **Oluştur**.
7. Üzerinde **uygulama ayrıntıları** sayfasında, **Düzenle > Ayrıntıları Düzenle**, kutusunu işaretlemeniz **Twitter etkinleştirme oturum**ve ardından **Kaydet**.
8. Seçin **anahtarları ve belirteçleri** ve kayıt **tüketici API anahtarı** ve **tüketici API gizli anahtarı** daha sonra kullanılacak değerler.

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Azure AD B2C kiracınıza daha önce kaydettiğiniz gizli anahtarı depolamak gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure AD B2C kiracınızı içeren dizine kullandığınızdan emin olun. Seçin **dizin ve abonelik filtresi** üst menüdeki ve kiracınız içeren dizini seçin.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Genel bakış sayfasında **kimlik deneyimi çerçevesi - PREVIEW**.
5. Seçin **ilke anahtarları** seçip **Ekle**.
6. İçin **seçenekleri**, seçin `Manual`.
7. Girin bir **adı** ilke anahtarı. Örneğin, `TwitterSecret`. Önek `B2C_1A_` anahtarınızı adına otomatik olarak eklenir.
8. İçinde **gizli**, daha önce kaydettiğiniz istemci gizli anahtarı girin.
9. İçin **anahtar kullanımı**seçin `Encryption`.
10. **Oluştur**’a tıklayın.

## <a name="add-a-claims-provider"></a>Bir talep Sağlayıcı Ekle

Bir Twitter hesabını kullanarak oturum açmasını istiyorsanız, Azure AD B2C'yi bir uç nokta ile iletişim kurabilen bir talep sağlayıcısı olarak hesabı tanımlamanız gerekir. Uç nokta, Azure AD B2C tarafından belirli bir kullanıcı yapıldığını doğrulamak için kullanılan bir talepler kümesi sağlar. 

Bir talep sağlayıcısı olarak Twitter hesabıyla ekleyerek tanımlayabilirsiniz **ClaimsProviders** ilkenizin uzantısı dosyasında öğe.

1. Açık *TrustFrameworkExtensions.xml*.
2. Bulma **ClaimsProviders** öğesi. Yoksa, kök öğe altında ekleyin.
3. Yeni bir **ClaimsProvider** gibi:

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
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="user_id" />
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

4. Değiştirin **client_id** ile daha önce kaydettiğiniz tüketici anahtarı.
5. Dosyayı kaydedin.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını karşıya yükle

Artık Azure AD B2C, LinkedIn hesabınızla iletişim kurma bilebilmesi ilkenizi yapılandırdınız. İlkenizin yalnızca bu sorunları şu ana kadar sahip olmadığını onaylamak için uzantı dosyasını karşıya yüklemeyi deneyin.

1. Üzerinde **özel ilkeleri** sayfa seçin, Azure AD B2C kiracınızın **karşıya yükleme İlkesi**.
2. Etkinleştirme **ilke varsa üzerine**, ardından gözatın ve seçin *TrustFrameworkExtensions.xml* dosya.
3. **Karşıya Yükle**'ye tıklayın.

## <a name="register-the-claims-provider"></a>Talep sağlayıcısı kaydetme

Bu noktada, kimlik sağlayıcısı ayarlandı, ancak herhangi bir kaydolma veya oturum açma ekranları kullanılabilir değil. Kullanılabilir hale getirmek için mevcut bir şablonu kullanıcı yolculuğu bir kopyasını oluşturun ve ayrıca bir Twitter kimlik sağlayıcısına sahip olacak şekilde değiştirin.

1. Açık *TrustFrameworkBase.xml* başlangıç paketi dosyasından.
2. Bul ve tüm içeriğini kopyalayın **UserJourney** içeren öğe `Id="SignUpOrSignIn"`.
3. Açık *TrustFrameworkExtensions.xml* ve bulma **UserJourneys** öğesi. Öğe yoksa bir tane ekleyin.
4. Tüm içeriğini yapıştırın **UserJourney** öğesi alt öğesi olarak kopyaladığınız **UserJourneys** öğesi.
5. Kullanıcı yolculuğu kimliği yeniden adlandırın. Örneğin, `SignUpSignInTwitter`.

### <a name="display-the-button"></a>Bir düğme görüntülemek

**ClaimsProviderSelection** öğedir bir kaydolma veya oturum açma ekranında bir kimlik sağlayıcısı düğmesini benzer. Eklerseniz bir **ClaimsProviderSelection** öğesi bir Twitter hesabı için yeni bir düğme gösterilir sayfasında bir kullanıcı gölünüzdeki olduğunda.

1. Bulma **OrchestrationStep** içeren öğe `Order="1"` , oluşturduğunuz kullanıcı giden.
2. Altında **ClaimsProviderSelects**, şu öğeyi ekleyin. Değerini **TargetClaimsExchangeId** örneğin uygun bir değere `TwitterExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.

Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için bir Twitter hesabıyla iletişim kurmak için Azure AD B2C içindir.

1. Bulma **OrchestrationStep** içeren `Order="2"` kullanıcı giden.
2. Aşağıdaki **ClaimsExchange** öğesi için kullanılan kimliği için aynı değeri kullanın sağlamaktan **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAUTH1" />
    ```
    
    Değerini güncelleştirin **TechnicalProfileReferenceId** daha önce oluşturduğunuz teknik profil kimliği. Örneğin, `Twitter-OAUTH1`.

3. Kaydet *TrustFrameworkExtensions.xml* dosya ve doğrulama için yeniden yükleyin.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C'yi uygulama oluşturma

Azure AD B2c ile iletişim kiracınızda oluşturduğunuz bir uygulama üzerinden gerçekleşir. Bu bölümde zaten yapmadıysanız, bir test uygulaması oluşturmak için tamamlayabilirsiniz isteğe bağlı adımlar listelenir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı içeren dizine kullandığınızdan emin olun. Seçin **dizin ve abonelik filtresi** üst menüdeki ve kiracınız içeren dizini seçin.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından **Ekle**.
5. Uygulama için bir ad girin, örneğin *testapp1*.
6. İçin **Web uygulaması / Web API'sini**seçin `Yes`yazıp enter `https://jwt.ms` için **yanıt URL'si**.
7. **Oluştur**’a tıklayın.

## <a name="update-and-test-the-relying-party-file"></a>Güncelleştirme ve bağlı olan taraf dosyayı test etme

Oluşturduğunuz kullanıcı yolculuğu başlatır bağlı olan taraf (RP) dosyasını güncelleştirin.

1. Bir kopyasını *SignUpOrSignIn.xml* çalışma dizininizdeki ve yeniden adlandırın. Örneğin, yeniden adlandırın *SignUpSignInTwitter.xml*.
2. Yeni dosyayı açın ve değeri güncelleştirme **Policyıd** özniteliğini **TrustFrameworkPolicy** benzersiz bir değere sahip. Örneğin, `SignUpSignInTwitter`.
3. Değerini güncelleştirin **PublicPolicyUri** ilkesi için URI ile. Örneğin,`http://contoso.com/B2C_1A_signup_signin_twitter`
4. Değerini güncelleştirin **Referenceıd** özniteliğini **DefaultUserJourney** (SignUpSignTwitter) oluşturduğunuz yeni kullanıcı yolculuğu kimliği eşleştirmek için.
5. Yaptığınız değişiklikleri kaydedin, dosyayı karşıya yükleyin ve ardından listeden yeni ilkeyi seçin.
6. İçinde oluşturduğunuz bir Azure AD B2C uygulamasını seçili olduğundan emin olun **uygulama seçin** alan ve ardından tıklayarak test **Şimdi Çalıştır**.
