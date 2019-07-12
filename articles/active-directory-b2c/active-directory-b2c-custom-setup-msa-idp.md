---
title: Azure Active Directory B2C'de özel ilkeleri kullanarak bir kimlik sağlayıcısı olarak Microsoft hesabı (MSA) Ekle
description: Openıd Connect (OIDC) protokolünü kullanarak kimlik sağlayıcısı olarak Microsoft kullanarak örneği.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 9ae944a9d587e71c4be83bd524cf3875a7b52dd1
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67654145"
---
# <a name="set-up-sign-in-with-a-microsoft-account-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeleri kullanarak bir Microsoft hesabıyla oturum açma özelliğini ayarlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede bir Microsoft hesabı kullanıcıları için oturum açma kullanarak etkinleştirmek gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md) Azure Active Directory (Azure AD) B2C'de.

## <a name="prerequisites"></a>Önkoşullar

- Bölümündeki adımları tamamlamanız [özel ilkeleri Azure Active Directory B2C kullanmaya başlama](active-directory-b2c-get-started-custom.md).
- Bir Microsoft hesabınız yoksa, tek tek oluşturun [ https://www.live.com/ ](https://www.live.com/).

## <a name="add-an-application"></a>Uygulama ekleme

Microsoft hesabı olan kullanıcılar için oturum açma etkinleştirmek için uygulamanın Azure AD kiracısı içindeki'ı kaydetmeniz gerekir. Azure AD kiracısı, Azure AD B2C kiracısı ile aynı değil.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure AD kiracınıza tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve Azure AD kiracınıza içeren dizine seçme.
1. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **uygulama kayıtları**.
1. Seçin **yeni kayıt**.
1. Girin bir **adı** uygulamanız için. Örneğin, *MSAapp1*.
1. Altında **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları (örneğin, Skype, Xbox, Outlook.com) hesaplarında**.
1. Altında **yeniden yönlendirme URI'si (isteğe bağlı)** seçin **Web** girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` metin kutusuna. Değiştirin `your-tenant-name` Azure AD B2C Kiracı adınızla.
1. Seçin **kaydetme**
1. Kayıt **uygulama (istemci) kimliği** uygulama genel bakış sayfasında gösterilir. Talep sağlayıcısı bir sonraki bölümde yapılandırırken gerekir.
1. Seçin **sertifikalarını ve gizli anahtarları**
1. Tıklayın **yeni istemci gizli anahtarı**
1. Girin bir **açıklama** örneğin gizliliği için *MSA uygulama istemci gizli anahtarı*ve ardından **Ekle**.
1. Gösterilen uygulama parolası kayıt **değer** sütun. Bu değeri sonraki bölümde kullanırsınız.

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Azure AD kiracınızda uygulama oluşturduktan sonra Azure AD B2C kiracınıza bu uygulama istemci gizli anahtarını depolamak gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Azure AD B2C kiracınızı içeren dizine kullandığınızdan emin olun. Seçin **dizin ve abonelik filtresi** üst menüdeki ve kiracınız içeren dizini seçin.
1. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
1. Genel bakış sayfasında **kimlik deneyimi çerçevesi**.
1. Seçin **ilke anahtarları** seçip **Ekle**.
1. İçin **seçenekleri**, seçin `Manual`.
1. Girin bir **adı** ilke anahtarı. Örneğin: `MSASecret`. Önek `B2C_1A_` anahtarınızı adına otomatik olarak eklenir.
1. İçinde **gizli**, önceki bölümde kaydettiğiniz istemci gizli anahtarını girin.
1. İçin **anahtar kullanımı**seçin `Signature`.
1.           **Oluştur**'a tıklayın.

## <a name="add-a-claims-provider"></a>Bir talep Sağlayıcı Ekle

Bir Microsoft hesabı kullanarak oturum açmalarına etkinleştirmek için Azure AD B2C'yi bir uç nokta ile iletişim kurabilen bir talep sağlayıcısı olarak hesabı tanımlamanız gerekir. Uç nokta, Azure AD B2C tarafından belirli bir kullanıcı yapıldığını doğrulamak için kullanılan bir talepler kümesi sağlar.

Bir talep sağlayıcısı olarak Azure AD'ye ekleyerek tanımlayabilirsiniz **ClaimsProvider** ilkenizin uzantısı dosyasında öğe.

1. Açık *TrustFrameworkExtensions.xml* ilke dosyası.
1. Bulma **ClaimsProviders** öğesi. Yoksa, kök öğe altında ekleyin.
1. Yeni bir **ClaimsProvider** gibi:

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
            <Item Key="client_id">Your Microsoft application client ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="sub" />
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

1. Değiştirin **client_id** ile Azure AD uygulama *uygulama (istemci) kimliği* daha önce kaydettiğiniz.
1. Dosyayı kaydedin.

Azure AD B2C'yi, Microsoft hesabı uygulamanızda Azure AD ile iletişim kurma bilebilmesi ilkenizi artık yapılandırdınız.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını karşıya yükle

Devam etmeden önce sorunları şu ana kadar sahip olmadığını onaylamak için değiştirilmiş ilkeyi karşıya yükleyin.

1. Azure portal ve select Azure AD B2C kiracınıza gitmek **kimlik deneyimi çerçevesi**.
1. Üzerinde **özel ilkeleri** sayfasında **karşıya özel ilke**.
1. Etkinleştirme **ilke varsa üzerine**, ardından gözatın ve seçin *TrustFrameworkExtensions.xml* dosya.
1. **Karşıya Yükle**'ye tıklayın.

Portalda gösterilmediğini, sonraki bölüme geçin.

## <a name="register-the-claims-provider"></a>Talep sağlayıcısı kaydetme

Bu noktada, kimlik sağlayıcısını ayarladınız, ancak henüz kaydolma veya oturum açma ekranları hiçbirinde kullanılabilir değil. Kullanılabilir hale getirmek için mevcut bir şablonu kullanıcı yolculuğu bir kopyasını oluşturun, sonra da Microsoft hesabı kimlik sağlayıcısı sahip olacak şekilde değiştirin.

1. Açık *TrustFrameworkBase.xml* başlangıç paketi dosyasından.
1. Bul ve tüm içeriğini kopyalayın **UserJourney** içeren öğe `Id="SignUpOrSignIn"`.
1. Açık *TrustFrameworkExtensions.xml* ve bulma **UserJourneys** öğesi. Öğe yoksa bir tane ekleyin.
1. Tüm içeriğini yapıştırın **UserJourney** öğesi alt öğesi olarak kopyaladığınız **UserJourneys** öğesi.
1. Kullanıcı yolculuğu kimliği yeniden adlandırın. Örneğin: `SignUpSignInMSA`.

### <a name="display-the-button"></a>Bir düğme görüntülemek

**ClaimsProviderSelection** öğedir bir kaydolma veya oturum açma ekranında bir kimlik sağlayıcısı düğmesini benzer. Eklerseniz bir **ClaimsProviderSelection** sayfasında bir kullanıcı gölünüzdeki attığında öğesi için bir Microsoft hesabı, yeni bir düğme görüntülenir.

1. İçinde *TrustFrameworkExtensions.xml* dosya, bulma **OrchestrationStep** içeren öğe `Order="1"` , oluşturduğunuz kullanıcı giden.
1. Altında **ClaimsProviderSelects**, şu öğeyi ekleyin. Değerini **TargetClaimsExchangeId** örneğin uygun bir değere `MicrosoftAccountExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="MicrosoftAccountExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.

Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için bir Microsoft hesabıyla iletişim kurmak için Azure AD B2C içindir.

1. Bulma **OrchestrationStep** içeren `Order="2"` kullanıcı giden.
1. Aşağıdaki **ClaimsExchange** öğesi için kullanılan kimliği için aynı değeri kullanın sağlamaktan **TargetClaimsExchangeId**:

    ```xml
    <ClaimsExchange Id="MicrosoftAccountExchange" TechnicalProfileReferenceId="MSA-OIDC" />
    ```

    Değerini güncelleştirin **TechnicalProfileReferenceId** eşleşmesi için `Id` değerini **TechnicalProfile** daha önce eklediğiniz talep sağlayıcısı öğesidir. Örneğin: `MSA-OIDC`.

1. Kaydet *TrustFrameworkExtensions.xml* dosya ve doğrulama için yeniden yükleyin.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C'yi uygulama oluşturma

Azure AD B2C kiracınızda oluşturduğunuz bir uygulama aracılığıyla Azure AD B2C ile iletişim gerçekleşir. Bu bölümde zaten yapmadıysanız, bir test uygulaması oluşturmak için tamamlayabilirsiniz isteğe bağlı adımlar listelenir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure AD B2C kiracınızı içeren dizine kullandığınızdan emin olun. Seçin **dizin ve abonelik filtresi** üst menüdeki ve kiracınız içeren dizini seçin.
1. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
1. Seçin **uygulamaları**ve ardından **Ekle**.
1. Uygulama için bir ad girin, örneğin *testapp1*.
1. İçin **Web uygulaması / Web API'sini**seçin `Yes`yazıp enter `https://jwt.ms` için **yanıt URL'si**.
1.           **Oluştur**'a tıklayın.

## <a name="update-and-test-the-relying-party-file"></a>Güncelleştirme ve bağlı olan taraf dosyayı test etme

Oluşturduğunuz kullanıcı yolculuğu başlatır bağlı olan taraf (RP) dosyasını güncelleştirin.

1. Bir kopyasını *SignUpOrSignIn.xml* çalışma dizininizdeki ve yeniden adlandırın. Örneğin, yeniden adlandırın *SignUpSignInMSA.xml*.
1. Yeni dosyayı açın ve değeri güncelleştirme **Policyıd** özniteliğini **TrustFrameworkPolicy** benzersiz bir değere sahip. Örneğin: `SignUpSignInMSA`.
1. Değerini güncelleştirin **PublicPolicyUri** ilkesi için URI ile. Örneğin,`http://contoso.com/B2C_1A_signup_signin_msa`
1. Değerini güncelleştirin **Referenceıd** özniteliğini **DefaultUserJourney** önceki (SignUpSignInMSA) oluşturduğunuz kullanıcı yolculuğu kimliği eşleştirmek için.
1. Yaptığınız değişiklikleri kaydedin, dosyayı karşıya yükleyin ve ardından listeden yeni ilkeyi seçin.
1. Önceki bölümde oluşturduğunuz bu Azure AD B2C uygulamasını emin olun (veya örneğin, önkoşulları tamamlayarak *webapp1* veya *testapp1*) seçili olduğundan **seçin Uygulama** alan ve ardından tıklayarak test **Şimdi Çalıştır**.
1. Seçin **Microsoft Account** düğmesine tıklayın ve oturum açın.

    Oturum açma işlemi başarılı olursa için yönlendirilirsiniz `jwt.ms` görüntüleyen çözülmüş belirteç benzer:

    ```json
    {
      "typ": "JWT",
      "alg": "RS256",
      "kid": "<key-ID>"
    }.{
      "exp": 1562365200,
      "nbf": 1562361600,
      "ver": "1.0",
      "iss": "https://your-b2c-tenant.b2clogin.com/10000000-0000-0000-0000-000000000000/v2.0/",
      "sub": "20000000-0000-0000-0000-000000000000",
      "aud": "30000000-0000-0000-0000-000000000000",
      "acr": "b2c_1a_signupsigninmsa",
      "nonce": "defaultNonce",
      "iat": 1562361600,
      "auth_time": 1562361600,
      "idp": "live.com",
      "name": "Azure User",
      "email": "azureuser@contoso.com",
      "tid": "6fc3b573-7b38-4c0c-b627-2e8684f6c575"
    }.[Signature]
    ```
