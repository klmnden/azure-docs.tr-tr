---
title: Salesforce SAML sağlayıcısı ile oturum açma Azure Active Directory B2C'de özel ilkeler kullanarak ayarlama
description: Salesforce SAML sağlayıcısı ile oturum açma Azure Active Directory B2C'de özel ilkeler kullanarak ayarlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 7cbde2beb03c174facbd145954387a31f6158a9a
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67654186"
---
# <a name="set-up-sign-in-with-a-salesforce-saml-provider-by-using-custom-policies-in-azure-active-directory-b2c"></a>Salesforce SAML sağlayıcısı ile oturum açma Azure Active Directory B2C'de özel ilkeler kullanarak ayarlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede kullanıcıların Salesforce kullanmasını kuruluş için oturum açma olanağı gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md) Azure Active Directory (Azure AD) B2C'de. Oturum açma ekleyerek etkinleştirdiğiniz bir [SAML teknik profili](saml-technical-profile.md) özel bir ilke için.

## <a name="prerequisites"></a>Önkoşullar

- Bölümündeki adımları tamamlamanız [özel ilkeleri Azure Active Directory B2C kullanmaya başlama](active-directory-b2c-get-started-custom.md).
- Zaten yapmadıysanız, kaydolmaya bir [ücretsiz Geliştirici sürümü hesap](https://developer.salesforce.com/signup). Bu makalede [Salesforce ışık deneyimi](https://developer.salesforce.com/page/Lightning_Experience_FAQ).
- [My etki alanını ayarlama](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) Salesforce kuruluşunuz için.

### <a name="set-up-salesforce-as-an-identity-provider"></a>Salesforce kimlik sağlayıcısı olarak ayarlayın.

1. [Salesforce oturum](https://login.salesforce.com/).
2. Soldaki menünün altındaki **ayarları**, genişletin **kimlik**ve ardından **kimlik sağlayıcısı**.
3. Seçin **etkinleştirme kimlik sağlayıcısı**.
4. Altında **sertifikayı seçin**, Salesforce, Azure AD B2C ile iletişim kurmak için kullanmak istediğiniz sertifikayı seçin. Varsayılan sertifikayı kullanabilirsiniz.
5. **Kaydet**’e tıklayın.

### <a name="create-a-connected-app-in-salesforce"></a>Salesforce'ta bağlı bir uygulama oluşturun

1. Üzerinde **kimlik sağlayıcısı** sayfasında **hizmet sağlayıcıları aracılığıyla bağlı uygulamalar artık oluşturulur. Buraya tıklayın.**
2. Altında **temel bilgileri**, bağlı uygulamanız için gerekli değerleri girin.
3. Altında **Web uygulaması ayarları**, kontrol **etkinleştirme SAML** kutusu.
4. İçinde **varlık kimliği** aşağıdaki URL'yi girin. Değerini değiştirdiğinizden emin olun `your-tenant` Azure AD B2C kiracınızın adı.

      ```
      https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```

6. İçinde **ACS URL** aşağıdaki URL'yi girin. Değerini değiştirdiğinizden emin olun `your-tenant` Azure AD B2C kiracınızın adı.

      ```
      https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. Listenin sonuna kaydırın ve ardından **Kaydet**.

### <a name="get-the-metadata-url"></a>Meta veri URL'sini alın

1. Bağlı uygulama genel bakış sayfasında tıklayın **Yönet**.
2. Değeri kopyalamak **meta veri bulma uç noktası**ve kaydedin. Bu makalenin sonraki bölümlerinde kullanacaksınız.

### <a name="set-up-salesforce-users-to-federate"></a>Salesforce kullanıcıları federasyona eklemek için ayarlama

1. Üzerinde **Yönet** sayfa bağlı uygulamanızın tıklayın **Spravovat Profily**.
2. Profilleri (veya kullanıcı gruplarına) seçin, Azure AD B2C ile federasyona eklemek istediğiniz. Bir sistem yöneticisi olarak seçin **Sistem Yöneticisi** onay kutusunu böylece Salesforce hesabınıza kullanarak ad'sini birleştirebilir.

## <a name="generate-a-signing-certificate"></a>Bir imzalama sertifikası oluşturma

Salesforce'a gönderilen istekler, Azure AD B2C tarafından imzalanması gerekir. İmzalama sertifikası oluşturmak için Azure PowerShell'i açın ve ardından aşağıdaki komutları çalıştırın.

> [!NOTE]
> Kiracı adını ve parolasını ilk iki satırları güncelleştirdiğinizden emin olun.

```powershell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Azure AD B2C kiracınızda oluşturduğunuz sertifika deposuna gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Genel bakış sayfasında **kimlik deneyimi çerçevesi**.
5. Seçin **ilke anahtarları** seçip **Ekle**.
6. İçin **seçenekleri**, seçin `Upload`.
7. İlke için bir **Ad** girin. Örneğin, SAMLSigningCert. Önek `B2C_1A_` anahtarınızı adına otomatik olarak eklenir.
8. Gidin ve oluşturduğunuz B2CSigningCert.pfx sertifikayı seçin.
9. Girin **parola** sertifikası.
3.           **Oluştur**'a tıklayın.

## <a name="add-a-claims-provider"></a>Bir talep Sağlayıcı Ekle

Salesforce hesabı kullanarak oturum açmasını istiyorsanız, Azure AD B2C'yi bir uç nokta ile iletişim kurabilen bir talep sağlayıcısı olarak hesabı tanımlamanız gerekir. Uç nokta, Azure AD B2C tarafından belirli bir kullanıcı yapıldığını doğrulamak için kullanılan bir talepler kümesi sağlar.

Bir talep sağlayıcısı olarak bir Salesforce hesabına ekleyerek tanımlayabilirsiniz **ClaimsProviders** ilkenizin uzantısı dosyasında öğe.

1. Açık *TrustFrameworkExtensions.xml*.
2. Bulma **ClaimsProviders** öğesi. Yoksa, kök öğe altında ekleyin.
3. Yeni bir **ClaimsProvider** gibi:

    ```XML
    <ClaimsProvider>
      <Domain>salesforce</Domain>
      <DisplayName>Salesforce</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="salesforce">
          <DisplayName>Salesforce</DisplayName>
          <Description>Login with your Salesforce account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="WantsSignedAssertions">false</Item>
            <Item Key="PartnerEntity">https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp.xml</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userId"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="username"/>
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="SAMLIdp" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. Değerini güncelleştirin **PartnerEntity** ile daha önce kopyaladığınız Salesforce meta veri URL'si.
5. Her iki değeri güncelleştirme **Storagereferenceıd** olarak anahtar imzalama sertifikanızın adıdır. Örneğin, B2C_1A_SAMLSigningCert.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını karşıya yükle

Artık Azure AD B2C ile Salesforce hesabınıza iletişim kurma bilebilmesi ilkenizi yapılandırdınız. İlkenizin yalnızca bu sorunları şu ana kadar sahip olmadığını onaylamak için uzantı dosyasını karşıya yüklemeyi deneyin.

1. Üzerinde **özel ilkeleri** sayfa seçin, Azure AD B2C kiracınızın **karşıya yükleme İlkesi**.
2. Etkinleştirme **ilke varsa üzerine**, ardından gözatın ve seçin *TrustFrameworkExtensions.xml* dosya.
3. **Karşıya Yükle**'ye tıklayın.

## <a name="register-the-claims-provider"></a>Talep sağlayıcısı kaydetme

Bu noktada, kimlik sağlayıcısı ayarlandı, ancak herhangi bir kaydolma veya oturum açma ekranları kullanılabilir değil. Kullanılabilir hale getirmek için mevcut bir şablonu kullanıcı yolculuğu bir kopyasını oluşturun ve ayrıca Salesforce kimlik sağlayıcısına sahip olacak şekilde değiştirin.

1. Açık *TrustFrameworkBase.xml* başlangıç paketi dosyasından.
2. Bul ve tüm içeriğini kopyalayın **UserJourney** içeren öğe `Id="SignUpOrSignIn"`.
3. Açık *TrustFrameworkExtensions.xml* ve bulma **UserJourneys** öğesi. Öğe yoksa bir tane ekleyin.
4. Tüm içeriğini yapıştırın **UserJourney** öğesi alt öğesi olarak kopyaladığınız **UserJourneys** öğesi.
5. Kullanıcı yolculuğu kimliği yeniden adlandırın. Örneğin: `SignUpSignInSalesforce`.

### <a name="display-the-button"></a>Bir düğme görüntülemek

**ClaimsProviderSelection** öğedir bir kaydolma veya oturum açma ekranında bir kimlik sağlayıcısı düğmesini benzer. Eklerseniz bir **ClaimsProviderSelection** öğesi bir LinkedIn hesabı için yeni bir düğme gösterilir sayfasında bir kullanıcı gölünüzdeki olduğunda.

1. Bulma **OrchestrationStep** içeren öğe `Order="1"` oluşturduğunuz kullanıcı yolculuğu içinde.
2. Altında **ClaimsProviderSelects**, şu öğeyi ekleyin. Değerini **TargetClaimsExchangeId** örneğin uygun bir değere `SalesforceExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="SalesforceExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.

Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için Salesforce hesabı ile iletişim kurmak için Azure AD B2C içindir.

1. Bulma **OrchestrationStep** içeren `Order="2"` kullanıcı giden.
2. Aşağıdaki **ClaimsExchange** öğe için aynı değeri kullanın sağlamaktan **kimliği** için kullanılan **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="SalesforceExchange" TechnicalProfileReferenceId="salesforce" />
    ```

    Değerini güncelleştirin **TechnicalProfileReferenceId** için **kimliği** daha önce oluşturduğunuz teknik profil. Örneğin: `LinkedIn-OAUTH`.

3. Kaydet *TrustFrameworkExtensions.xml* dosya ve doğrulama için yeniden yükleyin.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C'yi uygulama oluşturma

Azure AD B2c ile iletişim kiracınızda oluşturduğunuz bir uygulama üzerinden gerçekleşir. Bu bölümde zaten yapmadıysanız, bir test uygulaması oluşturmak için tamamlayabilirsiniz isteğe bağlı adımlar listelenir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından **Ekle**.
5. Uygulama için bir ad girin, örneğin *testapp1*.
6. İçin **Web uygulaması / Web API'sini**seçin `Yes`yazıp enter `https://jwt.ms` için **yanıt URL'si**.
7.           **Oluştur**'a tıklayın.

## <a name="update-and-test-the-relying-party-file"></a>Güncelleştirme ve bağlı olan taraf dosyayı test etme

Yeni oluşturduğunuz kullanıcı yolculuğu başlatan bağlı olan taraf (RP) dosyasını güncelleştirin:

1. Bir kopyasını *SignUpOrSignIn.xml* çalışma dizininizdeki ve yeniden adlandırın. Örneğin, yeniden adlandırın *SignUpSignInSalesforce.xml*.
2. Yeni dosyayı açın ve değeri güncelleştirme **Policyıd** özniteliğini **TrustFrameworkPolicy** benzersiz bir değere sahip. Örneğin: `SignUpSignInSalesforce`.
3. Değerini güncelleştirin **PublicPolicyUri** ilkesi için URI ile. Örneğin,`http://contoso.com/B2C_1A_signup_signin_salesforce`
4. Değerini güncelleştirin **Referenceıd** özniteliğini **DefaultUserJourney** (SignUpSignInSalesforce) oluşturduğunuz yeni kullanıcı yolculuğu kimliği eşleştirmek için.
5. Yaptığınız değişiklikleri kaydedin, dosyayı karşıya yükleyin ve ardından listeden yeni ilkeyi seçin.
6. İçinde oluşturduğunuz bir Azure AD B2C uygulamasını seçili olduğundan emin olun **uygulama seçin** alan ve ardından tıklayarak test **Şimdi Çalıştır**.
