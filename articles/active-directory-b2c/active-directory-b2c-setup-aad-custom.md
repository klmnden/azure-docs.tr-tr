---
title: Özel ilkeleri kullanarak, Azure Active Directory B2C, bir Azure Active Directory hesabı ile oturum açma ayarlama | Microsoft Docs
description: Özel ilkeleri kullanarak, Azure Active Directory B2C, bir Azure Active Directory hesabı ile oturum ayarlayın.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: d323a93773a8459d097c1fe3502d2ccd88ae9695
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64687902"
---
# <a name="set-up-sign-in-with-an-azure-active-directory-account-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeleri kullanarak bir Azure Active Directory hesabı ile oturum açma özelliğini ayarlama 

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede bir Azure Active Directory (Azure AD) kuruluştan kullanıcılar için oturum açma kullanarak etkinleştirmek gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md) Azure Active Directory (Azure AD) B2C'de.

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [özel ilkeleri Azure Active Directory B2C kullanmaya başlama](active-directory-b2c-get-started-custom.md).

## <a name="register-an-application"></a>Bir uygulamayı kaydetme

Belirli kullanıcılar için oturum açma etkinleştirmek için Azure AD kuruluş ihtiyacınız kuruluş içinde bir uygulamayı kaydetmek Azure AD kiracısı.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Kuruluş Azure içeren dizine kullandığınızdan emin olun tıklayarak AD Kiracı (contoso.com) **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **uygulama kayıtları**.
4. **Yeni uygulama kaydı**’nı seçin.
5. Uygulamanız için bir ad girin. Örneğin, `Azure AD B2C App`.
6. İçin **uygulama türü**seçin `Web app / API`.
7. İçin **oturum açma URL'si**, tüm küçük harfleri, aşağıdaki URL'yi girin burada `your-B2C-tenant-name` Azure AD B2C kiracınızın adı ile değiştirilir:

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Örneğin, `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.

8. **Oluştur**’a tıklayın. Kopyalama **uygulama kimliği** daha sonra kullanılacak.
9. Uygulamayı seçin ve ardından **ayarları**.
10. Seçin **anahtarları**anahtar için bir açıklama girin, bir süre seçin ve ardından **Kaydet**. Daha sonra kullanılmak üzere görüntülenen anahtarının değerini kopyalayın.

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Azure AD B2C kiracınızda oluşturduğunuz uygulama anahtarı depolamak gerekir.

1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
2. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
3. Genel bakış sayfasında **kimlik deneyimi çerçevesi - PREVIEW**.
4. Seçin **ilke anahtarları** seçip **Ekle**.
5. İçin **seçenekleri**, seçin `Manual`.
6. Girin bir **adı** ilke anahtarı. Örneğin, `ContosoAppSecret`.  Önek `B2C_1A_` anahtarınızı adına otomatik olarak eklenir.
7. İçinde **gizli**, daha önce kaydettiğiniz uygulamayı anahtarınızı girin.
8. İçin **anahtar kullanımı**seçin `Signature`.
9. **Oluştur**’a tıklayın.

## <a name="add-a-claims-provider"></a>Bir talep Sağlayıcı Ekle

Azure AD kullanarak oturum açmalarını isterseniz, Azure AD B2C'yi bir uç nokta ile iletişim kurabilen bir talep sağlayıcısı olarak Azure AD'ye tanımlamanız gerekir. Uç nokta, Azure AD B2C tarafından belirli bir kullanıcı yapıldığını doğrulamak için kullanılan bir talepler kümesi sağlar. 

Azure AD'ye ekleyerek bir talep sağlayıcısı olarak Azure AD'ye tanımlayabilirsiniz **ClaimsProvider** ilkenizin uzantısı dosyasında öğe.

1. Açık *TrustFrameworkExtensions.xml*.
2. Bulma **ClaimsProviders** öğesi. Yoksa, kök öğe altında ekleyin.
3. Yeni bir **ClaimsProvider** gibi:

    ```XML
    <ClaimsProvider>
      <Domain>Contoso</Domain>
      <DisplayName>Login using Contoso</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="ContosoProfile">
          <DisplayName>Contoso Employee</DisplayName>
          <Description>Login with your Contoso account</Description>
          <Protocol Name="OpenIdConnect"/>
          <OutputTokenFormat>JWT</OutputTokenFormat>
          <Metadata>
            <Item Key="METADATA">https://login.windows.net/your-AD-tenant-name.onmicrosoft.com/.well-known/openid-configuration</Item>
            <Item Key="ProviderName">https://sts.windows.net/00000000-0000-0000-0000-000000000000/</Item>
            <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
            <Item Key="IdTokenAudience">00000000-0000-0000-0000-000000000000</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
            <Item Key="response_types">code</Item>
            <Item Key="scope">openid</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="HttpBinding">POST</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="oid"/>
            <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="AzureADContoso" AlwaysUseDefaultValue="true" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. Altında **ClaimsProvider** öğesi için değeri güncelleştirin **etki alanı** diğer kimlik sağlayıcılardan ayırmak için kullanılan benzersiz bir değer. Örneğin, `Contoso`. Put olmayan bir `.com` sonunda, bu etki alanı ayarı.
5. Altında **ClaimsProvider** öğesi için değeri güncelleştirin **DisplayName** için talep sağlayıcısı için bir kolay ad. Bu değer şu anda kullanılmıyor.

### <a name="update-the-technical-profile"></a>Teknik profilini güncelleştir

Azure AD uç noktasından bir belirteç almak için Azure AD B2C, Azure AD ile iletişim kurmak için kullanması gereken protokolleri tanımlamanız gerekir. İçinde yapıldığını **TechnicalProfile** öğesinin **ClaimsProvider**.

1. Güncelleştirme kimliği **TechnicalProfile** öğesi. Bu kimlik, ilkenin diğer bölümlerden Bu teknik profile başvurmak için kullanılır.
2. Değerini güncelleştirin **DisplayName**. Bu değer, oturum açma ekranında oturum açın düğmesinde görüntülenir.
3. Değerini güncelleştirin **açıklama**.
4. Azure AD, Openıd Connect protokolünü kullanır, böylece emin değeri **Protokolü** olduğu `OpenIdConnect`.
5. Değerini ayarlamak **meta verileri** için `https://login.windows.net/your-AD-tenant-name.onmicrosoft.com/.well-known/openid-configuration`burada `your-AD-tenant-name` , Azure AD Kiracı adı. Örneğin, `https://login.windows.net/fabrikam.onmicrosoft.com/.well-known/openid-configuration`
6. Tarayıcınızı açın ve gidin **meta verileri** güncelleştirdiğiniz, arama için URL **veren** nesne, kopyalama ve değer için değerine yapıştırın **ProviderName** XML dosyasında.
8. Ayarlama **client_id** ve **IdTokenAudience** için uygulama kaydı uygulama kimliği.
9. Altında **CryptograhicKeys**, değerini güncelleştirin **Storagereferenceıd** tanımladığınız ilke anahtarı. Örneğin, `ContosoAppSecret`.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını karşıya yükle

Artık Azure AD B2C, Azure AD dizini ile iletişim kurma bilebilmesi ilkenizi yapılandırdınız. İlkenizin yalnızca bu sorunları şu ana kadar sahip olmadığını onaylamak için uzantı dosyasını karşıya yüklemeyi deneyin.

1. Üzerinde **özel ilkeleri** sayfa seçin, Azure AD B2C kiracınızın **karşıya yükleme İlkesi**.
2. Etkinleştirme **ilke varsa üzerine**, ardından gözatın ve seçin *TrustFrameworkExtensions.xml* dosya.
3. **Karşıya Yükle**'ye tıklayın.

## <a name="register-the-claims-provider"></a>Talep sağlayıcısı kaydetme

Bu noktada, kimlik sağlayıcısı ayarlandı, ancak oturumu-kaydolma/oturum açma ekranları hiçbirinde kullanılabilir değil. Kullanılabilir hale getirmek için mevcut bir şablonu kullanıcı yolculuğu bir kopyasını oluşturun ve ayrıca Azure AD kimlik sağlayıcısına sahip olacak şekilde değiştirin:

1. Açık *TrustFrameworkBase.xml* başlangıç paketi dosyasından.
2. Bul ve tüm içeriğini kopyalayın **UserJourney** içeren öğe `Id="SignUpOrSignIn"`.
3. Açık *TrustFrameworkExtensions.xml* ve bulma **UserJourneys** öğesi. Öğe yoksa bir tane ekleyin.
4. Tüm içeriğini yapıştırın **UserJourney** öğesi alt öğesi olarak kopyaladığınız **UserJourneys** öğesi.
5. Kullanıcı yolculuğu kimliği yeniden adlandırın. Örneğin, `SignUpSignInContoso`.

### <a name="display-the-button"></a>Bir düğme görüntülemek

**ClaimsProviderSelection** öğedir oturumu-kaydolma/oturum açma ekranı bir kimlik sağlayıcısının düğmesine benzer. Eklerseniz bir **ClaimsProviderSelection** öğesi Azure AD için yeni bir düğme gösterilir sayfasında bir kullanıcı gölünüzdeki olduğunda.

1. Bulma **OrchestrationStep** içeren öğe `Order="1"` , oluşturduğunuz kullanıcı giden.
2. Altında **ClaimsProviderSelections**, şu öğeyi ekleyin. Değerini **TargetClaimsExchangeId** örneğin uygun bir değere `ContosoExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.

Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için Azure AD ile iletişim kurmak için Azure AD B2C içindir. Düğme, Azure AD talep sağlayıcısı teknik profil bağlayarak bir eyleme bağlantı:

1. Bulma **OrchestrationStep** içeren `Order="2"` kullanıcı giden.
2. Aşağıdaki **ClaimsExchange** öğe için aynı değeri kullanın sağlamaktan **kimliği** için kullanılan **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```
    
    Değerini güncelleştirin **TechnicalProfileReferenceId** için **kimliği** daha önce oluşturduğunuz teknik profil. Örneğin, `ContosoProfile`.

3. Kaydet *TrustFrameworkExtensions.xml* dosya ve doğrulama için yeniden yükleyin.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C'yi uygulama oluşturma

Azure AD B2c ile iletişim kiracınızda oluşturduğunuz bir uygulama üzerinden gerçekleşir. Bu bölümde zaten yapmadıysanız, bir test uygulaması oluşturmak için tamamlayabilirsiniz isteğe bağlı adımlar listelenir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından **Ekle**.
5. Uygulama için bir ad girin, örneğin *testapp1*.
6. İçin **Web uygulaması / Web API'sini**seçin `Yes`yazıp enter `https://jwt.ms` için **yanıt URL'si**.
7. **Oluştur**’a tıklayın.

## <a name="update-and-test-the-relying-party-file"></a>Güncelleştirme ve bağlı olan taraf dosyayı test etme

Oluşturduğunuz kullanıcı yolculuğu başlatır bağlı olan taraf (RP) dosyasını güncelleştirin.

1. Bir kopyasını *SignUpOrSignIn.xml* çalışma dizininizdeki ve yeniden adlandırın. Örneğin, yeniden adlandırın *SignUpSignInContoso.xml*.
2. Yeni dosyayı açın ve değeri güncelleştirme **Policyıd** özniteliğini **TrustFrameworkPolicy** benzersiz bir değere sahip. Örneğin, `SignUpSignInContoso`.
3. Değerini güncelleştirin **PublicPolicyUri** ilkesi için URI ile. Örneğin,`http://contoso.com/B2C_1A_signup_signin_contoso`
4. Değerini güncelleştirin **Referenceıd** özniteliğini **DefaultUserJourney** (SignUpSignInContoso) oluşturduğunuz yeni kullanıcı yolculuğu kimliği eşleştirmek için.
5. Yaptığınız değişiklikleri kaydedin, dosyayı karşıya yükleyin ve ardından listeden yeni ilkeyi seçin.
6. İçinde oluşturduğunuz bir Azure AD B2C uygulamasını seçili olduğundan emin olun **uygulama seçin** alan ve ardından tıklayarak test **Şimdi Çalıştır**.

