---
title: Azure Active Directory B2C'de özel ilkeleri kullanarak bir çok kiracılı Azure AD kimlik sağlayıcısı için oturum açma ayarlama | Microsoft Docs
description: Özel ilkeleri - Azure Active Directory B2C kullanarak bir çok kiracılı Azure AD kimlik sağlayıcısı ekleyin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 72d01d6927ee421d01a831244acf65c44a084354
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66508658"
---
# <a name="set-up-sign-in-for-multi-tenant-azure-active-directory-using-custom-policies-in-azure-active-directory-b2c"></a>Oturum açma için çok kiracılı Azure özel ilkeleri Azure Active Directory B2C kullanarak Active Directory ayarlayın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, oturum açma kullanarak Azure Active Directory (Azure AD) çok Kiracı uç noktası kullanan kullanıcılar için etkinleştirme işlemini göstermektedir [özel ilkeler](active-directory-b2c-overview-custom.md) Azure AD B2C'de. Bu, kullanıcıların Azure AD B2C'ye her Kiracı için teknik bir sağlayıcı yapılandırması olmadan oturum açmak için birden çok Azure AD kiracısından sağlar. Ancak, bu kiracının tüm üyeleri Konuk **yapmamayı** oturum açmak kullanabilirsiniz. Bunun için şunları yapmanız [tek tek her Kiracı yapılandırma](active-directory-b2c-setup-aad-custom.md).

>[!NOTE]
>`Contoso.com` Kuruluş için kullanılan Azure AD kiracısı ve `fabrikamb2c.onmicrosoft.com` aşağıdaki yönergeler, Azure AD B2C kiracısı olarak kullanılır.

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
7. İçin **oturum açma URL'si**, tüm küçük harfleri, aşağıdaki URL'yi girin burada `your-tenant` (fabrikamb2c.onmicrosoft.com) Azure AD B2C kiracınızın adı ile değiştirilir:

    ```
    https://yourtenant.b2clogin.com/your-tenant.onmicrosoft.com/oauth2/authresp
    ```
    
8. **Oluştur**’a tıklayın. Kopyalama **uygulama kimliği** daha sonra kullanılacak.
9. Uygulamayı seçin ve ardından **ayarları**.
10. Seçin **anahtarları**anahtar için bir açıklama girin, bir süre seçin ve ardından **Kaydet**. Daha sonra kullanılmak üzere görüntülenen anahtarının değerini kopyalayın.
11. Altında **ayarları**seçin **özellikleri**ayarlayın **çok kiracılı** için `Yes`ve ardından **Kaydet**

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
      <Domain>commonaad</Domain>
      <DisplayName>Common AAD</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Common-AAD">
          <DisplayName>Multi-Tenant AAD</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <!-- Update the Client ID below to the Application ID -->
            <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <Item Key="METADATA">https://login.microsoftonline.com/common/.well-known/openid-configuration</Item>
            <Item Key="response_types">code</Item>
            <Item Key="scope">openid</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="DiscoverMetadataByTokenIssuer">true</Item>
        
            <!-- The key below allows you to specify each of the Azure AD tenants that can be used to sign in. Update the GUIDs below for each tenant. -->
            <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/00000000-0000-0000-0000-000000000000,https://sts.windows.net/11111111-1111-1111-1111-111111111111</Item>

            <!-- The commented key below specifies that users from any tenant can sign-in. Uncomment if you would like anyone with an Azure AD account to be able to sign in. -->
            <!-- <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/</Item> -->
          </Metadata>
          <CryptographicKeys>
            <!-- Make sure to update the reference ID of the client secret below you just created (B2C_1A_AADAppSecret) -->
            <Key Id="client_secret" StorageReferenceId="B2C_1A_AADAppSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
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

4. Altında **ClaimsProvider** öğesi için değeri güncelleştirin **etki alanı** diğer kimlik sağlayıcılardan ayırmak için kullanılan benzersiz bir değer.
5. Altında **TechnicalProfile** öğesi için değeri güncelleştirin **DisplayName**. Bu değer, oturum açma ekranında oturum açın düğmesinde görüntülenir.
6. Ayarlama **client_id** için Azure AD Kiracı mulity uygulama kaydı uygulama kimliği.

### <a name="restrict-access"></a>Erişimi kısıtlama

> [!NOTE]
> Kullanarak `https://sts.windows.net` değeri olarak **ValidTokenIssuerPrefixes** uygulamanıza oturum açmak tüm Azure AD kullanıcılarının sağlar.

Geçerli bir belirteç verenler listesini güncelleştirmek ve oturum açarak Azure AD Kiracı kullanıcılar belirli bir listesi için erişimi gerekir. Meta veriler her özel bir ara gerek değerlerini almak için gelen kullanıcıların istediğiniz Azure AD kiracılarıyla oturum açın. Veri biçimi aşağıdaki gibi görünür: `https://login.windows.net/your-tenant/.well-known/openid-configuration`burada `your-tenant` , Azure AD Kiracı adı (contoso.com veya diğer Azure AD kiracısı).

1. Tarayıcınızı açın ve gidin **meta verileri** URL ve Ara **veren** nesne ve değerini kopyalayın. Aşağıdaki gibi görünmelidir: `https://sts.windows.net/tenant-id/`.
2. Değerini kopyalayıp **ValidTokenIssuerPrefixes** anahtarı. Bunları virgülle ayırarak birden fazla ekleyebilirsiniz. Bunun bir örneği XML yukarıdaki örnekte bırakılmıştır.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını karşıya yükle

Artık Azure AD B2C, Azure AD dizini ile iletişim kurma bilebilmesi ilkenizi yapılandırdınız. İlkenizin yalnızca bu sorunları şu ana kadar sahip olmadığını onaylamak için uzantı dosyasını karşıya yüklemeyi deneyin.

1. Üzerinde **özel ilkeleri** sayfa seçin, Azure AD B2C kiracınızın **karşıya yükleme İlkesi**.
2. Etkinleştirme **ilke varsa üzerine**, ardından gözatın ve seçin *TrustFrameworkExtensions.xml* dosya.
3. **Karşıya Yükle**'ye tıklayın.

## <a name="register-the-claims-provider"></a>Talep sağlayıcısı kaydetme

Bu noktada, kimlik sağlayıcısı ayarlandı, ancak oturumu-kaydolma/oturum açma ekranları hiçbirinde kullanılabilir değil. Kullanılabilir hale getirmek için mevcut bir şablonu kullanıcı yolculuğu bir kopyasını oluşturun ve ayrıca Azure AD kimlik sağlayıcısına sahip olacak şekilde değiştirin.

1. Açık *TrustFrameworkBase.xml* başlangıç paketi dosyasından.
2. Bul ve tüm içeriğini kopyalayın **UserJourney** içeren öğe `Id="SignUpOrSignIn"`.
3. Açık *TrustFrameworkExtensions.xml* ve bulma **UserJourneys** öğesi. Öğe yoksa bir tane ekleyin.
4. Tüm içeriğini yapıştırın **UserJourney** öğesi alt öğesi olarak kopyaladığınız **UserJourneys** öğesi.
5. Kullanıcı yolculuğu kimliği yeniden adlandırın. Örneğin, `SignUpSignInContoso`.

### <a name="display-the-button"></a>Bir düğme görüntülemek

**ClaimsProviderSelection** öğedir oturumu-kaydolma/oturum açma ekranı bir kimlik sağlayıcısının düğmesine benzer. Eklerseniz bir **ClaimsProviderSelection** öğesi Azure AD için yeni bir düğme gösterilir sayfasında bir kullanıcı gölünüzdeki olduğunda.

1. Bulma **OrchestrationStep** içeren öğe `Order="1"` , oluşturduğunuz kullanıcı giden.
2. Altında **ClaimsProviderSelects**, şu öğeyi ekleyin. Değerini **TargetClaimsExchangeId** örneğin uygun bir değere `AzureADExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="AzureADExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.

Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için Azure AD ile iletişim kurmak için Azure AD B2C içindir. Düğme için eylem, Azure AD talep sağlayıcısı teknik profil bağlayarak bağlayın.

1. Bulma **OrchestrationStep** içeren `Order="2"` kullanıcı giden.
2. Aşağıdaki **ClaimsExchange** öğe için aynı değeri kullanın sağlamaktan **kimliği** için kullanılan **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="AzureADExchange" TechnicalProfileReferenceId="Common-AAD" />
    ```
    
    Değerini güncelleştirin **TechnicalProfileReferenceId** için **kimliği** daha önce oluşturduğunuz teknik profil. Örneğin, `Common-AAD`.

3. Kaydet *TrustFrameworkExtensions.xml* dosya ve doğrulama için yeniden yükleyin.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C'yi uygulama oluşturma

Azure AD B2C ile iletişim kiracınızda oluşturduğunuz bir uygulama üzerinden gerçekleşir. Bu bölümde zaten yapmadıysanız, bir test uygulaması oluşturmak için tamamlayabilirsiniz isteğe bağlı adımlar listelenir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından **Ekle**.
5. Uygulama için bir ad girin, örneğin *testapp1*.
6. İçin **Web uygulaması / Web API'sini**seçin `Yes`yazıp enter `https://jwt.ms` için **yanıt URL'si**.
7. **Oluştur**’a tıklayın.

## <a name="update-and-test-the-relying-party-file"></a>Güncelleştirme ve bağlı olan taraf dosyayı test etme

Oluşturduğunuz kullanıcı yolculuğu başlatır bağlı olan taraf (RP) dosyasını güncelleştirin.

1. Bir kopyasını *SignUpOrSignIn.xml* çalışma dizininizdeki ve yeniden adlandırın. Örneğin, yeniden adlandırın *SignUpSignContoso.xml*.
2. Yeni dosyayı açın ve değeri güncelleştirme **Policyıd** özniteliğini **TrustFrameworkPolicy** benzersiz bir değere sahip. Örneğin, `SignUpSignInContoso`.
3. Değerini güncelleştirin **PublicPolicyUri** ilkesi için URI ile. Örneğin,`http://contoso.com/B2C_1A_signup_signin_contoso`
4. Değerini güncelleştirin **Referenceıd** özniteliğini **DefaultUserJourney** (SignUpSignContoso) oluşturduğunuz yeni kullanıcı yolculuğu kimliği eşleştirmek için.
5. Yaptığınız değişiklikleri kaydedin, dosyayı karşıya yükleyin ve ardından listeden yeni ilkeyi seçin.
6. İçinde oluşturduğunuz bir Azure AD B2C uygulamasını seçili olduğundan emin olun **uygulama seçin** alan ve ardından tıklayarak test **Şimdi Çalıştır**.
