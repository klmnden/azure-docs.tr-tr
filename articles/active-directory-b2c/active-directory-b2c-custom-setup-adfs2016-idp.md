---
title: ADFS, Azure Active Directory B2C'de özel ilkeleri kullanarak bir SAML kimlik sağlayıcısı ekleyin | Microsoft Docs
description: AD FS 2016'yı Azure Active Directory B2C'de SAML protokolü ve özel ilkeler kullanarak ayarlama
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/07/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 2c469b333c6896d33b440bfadf0ebbdbeece71a3
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272139"
---
# <a name="add-adfs-as-a-saml-identity-provider-using-custom-policies-in-azure-active-directory-b2c"></a>ADFS, Azure Active Directory B2C'de özel ilkeleri kullanarak SAML kimlik sağlayıcısı olarak Ekle

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede bir ADFS kullanıcı hesabı için oturum açma kullanarak etkinleştirmek gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md) Azure Active Directory (Azure AD) B2C'de. Oturum açma ekleyerek etkinleştirdiğiniz bir [SAML teknik profili](saml-technical-profile.md) özel bir ilke için.

## <a name="prerequisites"></a>Önkoşullar

- Bölümündeki adımları tamamlamanız [özel ilkeleri Azure Active Directory B2C kullanmaya başlama](active-directory-b2c-get-started-custom.md).
- Bir özel anahtara sahip bir sertifika .pfx dosyasına erişimi olduğundan emin olun. Kendi imzalı bir sertifika oluşturur ve Azure AD B2C'ye yükleyin. Azure AD B2C, SAML kimlik sağlayıcısına gönderilen SAML isteğini imzalamak için bu sertifikayı kullanır.

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Azure AD B2C kiracınızda sertifikanızın depolanacağı gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure AD B2C kiracınızı içeren dizine kullandığınızdan emin olun. Seçin **dizin ve abonelik filtresi** üst menüdeki ve kiracınız içeren dizini seçin.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Genel bakış sayfasında **kimlik deneyimi çerçevesi**.
5. Seçin **ilke anahtarları** seçip **Ekle**.
6. İçin **seçenekleri**, seçin `Upload`.
7. Girin bir **adı** ilke anahtarı. Örneğin, `SamlCert`. Önek `B2C_1A_` anahtarınızı adına otomatik olarak eklenir.
8. Göz atın ve özel anahtarla birlikte, sertifika .pfx dosyasını seçin.
9. **Oluştur**’a tıklayın.

## <a name="add-a-claims-provider"></a>Bir talep Sağlayıcı Ekle

ADFS hesabını kullanarak oturum açmasını istiyorsanız, Azure AD B2C'yi bir uç nokta ile iletişim kurabilen bir talep sağlayıcısı olarak hesabı tanımlamanız gerekir. Uç nokta, Azure AD B2C tarafından belirli bir kullanıcı yapıldığını doğrulamak için kullanılan bir talepler kümesi sağlar. 

Bir talep sağlayıcısı olarak ekleyerek bir ADFS hesap tanımlayabilirsiniz **ClaimsProviders** ilkenizin uzantısı dosyasında öğe.

1. Açık *TrustFrameworkExtensions.xml*.
2. Bulma **ClaimsProviders** öğesi. Yoksa, kök öğe altında ekleyin.
3. Yeni bir **ClaimsProvider** gibi:

    ```xml
    <ClaimsProvider>
      <Domain>contoso.com</Domain>
      <DisplayName>Contoso ADFS</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Contoso-SAML2">
          <DisplayName>Contoso ADFS</DisplayName>
          <Description>Login with your ADFS account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="PartnerEntity">https://your-ADFS-domain/federationmetadata/2007-06/federationmetadata.xml</Item>
            <Item Key="XmlSignatureAlgorithm">Sha256</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="userPrincipalName" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
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

4. Değiştirin `your-ADFS-domain` ADFS etki alanınızın adını ve değerini değiştirin **Identityprovider** çıkış talebi ile DNS (etki alanınızı gösteren rastgele değer).
5. Dosyayı kaydedin.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını karşıya yükle

Artık Azure AD B2C ADFS hesabıyla iletişim kurmak nasıl bilebilmesi ilkenizi yapılandırdınız. İlkenizin yalnızca bu sorunları şu ana kadar sahip olmadığını onaylamak için uzantı dosyasını karşıya yüklemeyi deneyin.

1. Üzerinde **özel ilkeleri** sayfa seçin, Azure AD B2C kiracınızın **karşıya yükleme İlkesi**.
2. Etkinleştirme **ilke varsa üzerine**, ardından gözatın ve seçin *TrustFrameworkExtensions.xml* dosya.
3. **Karşıya Yükle**'ye tıklayın.

> [!NOTE]
> Visual Studio kod uzantısını B2C kullanır "socialIdpUserId." Bir sosyal İlkesi de ADFS için gereklidir.
>

## <a name="register-the-claims-provider"></a>Talep sağlayıcısı kaydetme

Bu noktada, kimlik sağlayıcısı ayarlandı, ancak herhangi bir kaydolma veya oturum açma ekranları kullanılabilir değil. Kullanılabilir hale getirmek için mevcut bir şablonu kullanıcı yolculuğu bir kopyasını oluşturun ve ayrıca bir ADFS kimlik sağlayıcısına sahip olacak şekilde değiştirin.

1. Açık *TrustFrameworkBase.xml* başlangıç paketi dosyasından.
2. Bul ve tüm içeriğini kopyalayın **UserJourney** içeren öğe `Id="SignUpOrSignIn"`.
3. Açık *TrustFrameworkExtensions.xml* ve bulma **UserJourneys** öğesi. Öğe yoksa bir tane ekleyin.
4. Tüm içeriğini yapıştırın **UserJourney** öğesi alt öğesi olarak kopyaladığınız **UserJourneys** öğesi.
5. Kullanıcı yolculuğu kimliği yeniden adlandırın. Örneğin, `SignUpSignInADFS`.

### <a name="display-the-button"></a>Bir düğme görüntülemek

**ClaimsProviderSelection** öğedir bir kaydolma veya oturum açma ekranında bir kimlik sağlayıcısı düğmesini benzer. Eklerseniz bir **ClaimsProviderSelection** öğesi ADFS hesabınız için yeni bir düğme gösterilir sayfasında bir kullanıcı gölünüzdeki olduğunda.

1. Bulma **OrchestrationStep** içeren öğe `Order="1"` , oluşturduğunuz kullanıcı giden.
2. Altında **ClaimsProviderSelections**, şu öğeyi ekleyin. Değerini **TargetClaimsExchangeId** örneğin uygun bir değere `ContosoExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.

Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için bir ADFS hesabıyla iletişim kurmak için Azure AD B2C içindir.

1. Bulma **OrchestrationStep** içeren `Order="2"` kullanıcı giden.
2. Aşağıdaki **ClaimsExchange** öğesi için kullanılan kimliği için aynı değeri kullanın sağlamaktan **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```
    
    Değerini güncelleştirin **TechnicalProfileReferenceId** daha önce oluşturduğunuz teknik profil kimliği. Örneğin, `Contoso-SAML2`.

3. Kaydet *TrustFrameworkExtensions.xml* dosya ve doğrulama için yeniden yükleyin.


## <a name="configure-an-adfs-relying-party-trust"></a>Bir AD FS bağlı olan taraf güveninde yapılandırma

ADFS, Azure AD B2C'de kimlik sağlayıcısı olarak kullanmak için bir AD FS bağlı olan taraf güveni oluşturma Azure AD B2C SAML meta verileriyle gerekir. Aşağıdaki örnek, bir URL adresi için bir Azure AD B2C'Teknik profili SAML meta verilerini gösterir:

```
https://your-tenant-name.b2clogin.com/your-tenant-name/your-policy/samlp/metadata?idptp=your-technical-profile
```

Aşağıdaki değerleri değiştirin:

- **Kiracı Your** Kiracı adınızla tenant.onmicrosoft.com bilgisayarınızı gibi.
- **ilke Your** ilke adınızla. Örneğin, B2C_1A_signup_signin_adfs.
- **Teknik profil Your** , SAML kimlik sağlayıcısı teknik profili adı. Örneğin, Contoso SAML2.
 
Bir tarayıcı açın ve URL'ye gidin. Doğru URL'yi yazın ve XML meta veri dosyası erişiminiz olduğundan emin olun. AD FS Yönetimi ek bileşenini kullanarak yeni bir bağlı olan taraf güveni eklemek ve ayarları el ile yapılandırmak için federasyon sunucusunda aşağıdaki yordamı gerçekleştirin. Üyelik **Yöneticiler** veya eşdeğer yerel bilgisayarda bu yordamı tamamlamak için gereken en düşük gereksinimdir.

1. Sunucu Yöneticisi'nde **Araçları**ve ardından **ADFS Yönetim**.
2. Seçin **bağlı olan taraf güveni ekleme**.
3. Üzerinde **Hoş Geldiniz** sayfasında **talep kullanan**ve ardından **Başlat**.
4. Üzerinde **veri kaynağı Seç** sayfasında **bağlı olan taraf hakkındaki verileri içe aktar yayımlama çevrimiçi veya yerel ağda**, Azure AD B2C meta veri URL'sini girin ve ardından **sonraki**.
5. Üzerinde **görünen adı belirt** want bir **görünen ad**altında **notları**bu bağlı olan taraf güveni için bir açıklama girin ve ardından **sonraki**.
6. Üzerinde **erişim denetimi ilkesini seçin** sayfasında, bir ilkeyi seçin ve ardından **sonraki**.
7. Üzerinde **güven eklemeye hazır** sayfasında, ayarları gözden geçirin ve ardından **sonraki** kaydetmek bağlı olan taraf güven bilgilerinizi.
8. Üzerinde **son** sayfasında **Kapat**, bu eylem otomatik olarak görüntüler **talep kurallarını Düzenle** iletişim kutusu.
9. Seçin **Kuralı Ekle**.  
10. İçinde **talep kuralı şablonu**seçin **Gönder LDAP özniteliklerini talep olarak**.
11. Sağlayan bir **talep kuralı adı**. İçin **öznitelik deposu**seçin **seçin Active Directory**, aşağıdaki talep ekleyin ve ardından tıklayın **son** ve **Tamam**.

    | LDAP özniteliği | Giden talep türü |
    | -------------- | ------------------- |
    | Kullanıcı asıl adı | userPrincipalName |
    | Soyadı | family_name |
    | Verilen ad | given_name |
    | E-posta adresi | email |
    | Görünen ad | name |
    
12.  Sertifika türüne bağlı olarak, KARMA algoritması ayarlamanız gerekebilir. Bağlı olan taraf güveni (B2C Demo) Özellikler penceresinde, seçin **Gelişmiş** sekmesini ve değiştirme **güvenli karma algoritması** için `SHA-256`, tıklatıp **Tamam**.  
13. Sunucu Yöneticisi'nde **Araçları**ve ardından **ADFS Yönetim**.
14. Oluşturduğunuz bağlı taraf güvenini seçin, **Federasyon meta verilerini güncelleştirmesi**ve ardından **güncelleştirme**. 

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C'yi uygulama oluşturma

Azure AD B2c ile iletişim kiracınızda oluşturduğunuz bir uygulama üzerinden gerçekleşir. Bu bölümde zaten yapmadıysanız, bir test uygulaması oluşturmak için tamamlayabilirsiniz isteğe bağlı adımlar listelenir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından **Ekle**.
5. Uygulama için bir ad girin, örneğin *testapp1*.
6. İçin **Web uygulaması / Web API'sini**seçin `Yes`yazıp enter `https://jwt.ms` için **yanıt URL'si**.
7. **Oluştur**’a tıklayın.

### <a name="update-and-test-the-relying-party-file"></a>Güncelleştirme ve bağlı olan taraf dosyayı test etme

Oluşturduğunuz kullanıcı yolculuğu başlatır bağlı olan taraf (RP) dosyasını güncelleştirin.

1. Bir kopyasını *SignUpOrSignIn.xml* çalışma dizininizdeki ve yeniden adlandırın. Örneğin, yeniden adlandırın *SignUpSignInADFS.xml*.
2. Yeni dosyayı açın ve değeri güncelleştirme **Policyıd** özniteliğini **TrustFrameworkPolicy** benzersiz bir değere sahip. Örneğin, `SignUpSignInADFS`.
3. Değerini güncelleştirin **PublicPolicyUri** ilkesi için URI ile. Örneğin,`http://contoso.com/B2C_1A_signup_signin_adfs`
4. Değerini güncelleştirin **Referenceıd** özniteliğini **DefaultUserJourney** (SignUpSignInADFS) oluşturduğunuz yeni kullanıcı yolculuğu kimliği eşleştirmek için.
5. Yaptığınız değişiklikleri kaydedin, dosyayı karşıya yükleyin ve ardından listeden yeni ilkeyi seçin.
6. İçinde oluşturduğunuz bir Azure AD B2C uygulamasını seçili olduğundan emin olun **uygulama seçin** alan ve ardından tıklayarak test **Şimdi Çalıştır**.

