---
title: Azure Active Directory B2C'de özel ilkeler kullanarak Salesforce SAML sağlayıcısı ekleme | Microsoft Docs
description: Azure Active Directory B2C özel ilkeleri oluşturma ve yönetme hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/15/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5b7621bde0be02b4656c4678438b94499bb82b5b
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43345046"
---
# <a name="azure-active-directory-b2c-sign-in-by-using-salesforce-accounts-via-saml"></a>Azure Active Directory B2C: SAML aracılığıyla Salesforce hesaplarını kullanarak oturum açın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede nasıl kullanılacağını gösterir [özel ilkeler](active-directory-b2c-overview-custom.md) oturum açmak için belirli bir Salesforce kuruluşun kullanıcıları ayarlamak için.

## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-ad-b2c-setup"></a>Azure AD B2C Kurulumu

Şunları nasıl yapacağınız tüm adımları tamamladığınızdan emin olmak için [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md) Azure Active Directory B2C'de (Azure AD B2C).

Bunlar:

* Azure AD B2C kiracısı oluşturun.
* Bir Azure AD B2C uygulaması oluşturun.
* İki ilke altyapısı uygulama kaydedin.
* Anahtarlarını ayarlayın.
* Başlangıç paketi ayarlayın.

### <a name="salesforce-setup"></a>Salesforce Kurulum

Bu makalede, aşağıdaki yapmış olduğunuz olduğunu varsayın:

* Bir Salesforce hesabına kaydolmanız. Oturum açabileceğiniz bir [ücretsiz Geliştirici sürümü hesap](https://developer.salesforce.com/signup).
* [My etki alanını ayarlama](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) Salesforce kuruluşunuz için.

## <a name="set-up-salesforce-so-users-can-federate"></a>Salesforce kullanıcılar devredebilir şekilde ayarlayın

Azure AD B2C Salesforce ile iletişim kurmasına yardımcı olmak için Salesforce meta veri URL'sini almanız gerekir.

### <a name="set-up-salesforce-as-an-identity-provider"></a>Salesforce kimlik sağlayıcısı olarak ayarlayın.

> [!NOTE]
> Bu makalede, kullanmakta olduğunuz olan varsayıyoruz [Salesforce ışık deneyimi](https://developer.salesforce.com/page/Lightning_Experience_FAQ).

1. [Salesforce oturum](https://login.salesforce.com/). 
2. Soldaki menünün altındaki **ayarları**, genişletme **kimlik**ve ardından **kimlik sağlayıcısı**.
3. Tıklayın **etkinleştirme kimlik sağlayıcısı**.
4. Altında **sertifikayı seçin**, Salesforce, Azure AD B2C ile iletişim kurmak için kullanmak istediğiniz sertifikayı seçin. (Varsayılan sertifikayı kullanabilirsiniz.) **Kaydet**’e tıklayın. 

### <a name="create-a-connected-app-in-salesforce"></a>Salesforce'ta bağlı bir uygulama oluşturun

1. Üzerinde **kimlik sağlayıcısı** sayfasında, Git **hizmet sağlayıcıları**.
2. Tıklayın **hizmet sağlayıcıları aracılığıyla bağlı uygulamalar artık oluşturulur. Buraya tıklayın.**
3. Altında **temel bilgileri**, bağlı uygulamanız için gerekli değerleri girin.
4. Altında **Web uygulaması ayarları**seçin **etkinleştirme SAML** onay kutusu.
5. İçinde **varlık kimliği** aşağıdaki URL'yi girin. Değeri değiştirme emin olun `tenantName`.
      ```
      https://tenantName.b2clogin.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```
6. İçinde **ACS URL** aşağıdaki URL'yi girin. Değeri değiştirme emin olun `tenantName`.
      ```
      https://tenantName.b2clogin.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. Tüm diğer ayarlar için varsayılan değerleri bırakın.
8. Listenin sonuna kaydırın ve ardından **Kaydet**.

### <a name="get-the-metadata-url"></a>Meta veri URL'sini alın

1. Bağlı uygulama genel bakış sayfasında tıklayın **Yönet**.
2. Değeri kopyalamak **meta veri bulma uç noktası**ve kaydedin. Bu makalenin sonraki bölümlerinde kullanacaksınız.

### <a name="set-up-salesforce-users-to-federate"></a>Salesforce kullanıcıları federasyona eklemek için ayarlama

1. Üzerinde **Yönet** sayfasında, bağlı uygulama, Git **profilleri**.
2. Tıklayın **profillerini yönetme**.
3. Profilleri (veya kullanıcı gruplarına) seçin, Azure AD B2C ile federasyona eklemek istediğiniz. Bir sistem yöneticisi olarak seçin **Sistem Yöneticisi** onay kutusunu böylece Salesforce hesabınıza kullanarak ad'sini birleştirebilir.

## <a name="generate-a-signing-certificate-for-azure-ad-b2c"></a>Azure AD B2C için imzalama sertifikası oluşturma

Salesforce'a gönderilen istekler, Azure AD B2C tarafından imzalanması gerekir. İmzalama sertifikası oluşturmak için Azure PowerShell'i açın ve ardından aşağıdaki komutları çalıştırın.

> [!NOTE]
> Kiracı adını ve parolasını ilk iki satırları güncelleştirdiğinizden emin olun.

```PowerShell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="add-the-saml-signing-certificate-to-azure-ad-b2c"></a>Azure AD B2C'ye SAML imzalama sertifikası ekleme

İmzalama sertifikası, Azure AD B2C kiracınıza karşıya yükleyin: 

1. Azure AD B2C kiracınıza gidin. Tıklayın **ayarları** > **kimlik deneyimi çerçevesi** > **ilke anahtarları**.
2. Tıklayın **+ Ekle**ve ardından:
    1. Tıklayın **seçenekleri** > **karşıya**.
    2. Girin bir **adı** (örneğin, SAMLSigningCert). Önek *B2C_1A_* anahtarınızı adına otomatik olarak eklenir.
    3. Sertifikanızı seçmek için **dosya denetimi karşıya**. 
    4. PowerShell komut dosyasında ayarlanan sertifikanın parolasını girin.
3. **Oluştur**’a tıklayın.
4. Bir anahtar (örneğin, B2C_1A_SAMLSigningCert) oluşturduğunuzu doğrulayın. Tam adını not edin (dahil olmak üzere *B2C_1A_*). Bu anahtarı daha sonra ilkeyi başvuracaktır.

## <a name="create-the-salesforce-saml-claims-provider-in-your-base-policy"></a>Temel ilkenizde Salesforce SAML talep sağlayıcısı oluşturma

Kullanıcıların Salesforce kullanarak oturum açabilmesi Salesforce bir talep sağlayıcısı olarak tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C ile iletişim kurar uç noktası belirtmeniz gerekir. Uç nokta olacak *sağlamak* birtakım *talep* , belirli bir kullanıcı yapıldığını doğrulamak için Azure AD B2C kullanır. Bunu yapmak için bir `<ClaimsProvider>` uzantısının ilkenizin Salesforce'ta için:

1. Çalışma dizininizde uzantı dosyası (TrustFrameworkExtensions.xml) açın.
2. Bulma `<ClaimsProviders>` bölümü. Yoksa, kök düğümünde oluşturun.
3. Yeni bir `<ClaimsProvider>`:

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
            <Item Key="RequestsSigned">false</Item>
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

Altında `<ClaimsProvider>` düğüm:

1. Değerini `<Domain>` ayırt eden benzersiz bir değere `<ClaimsProvider>` diğer kimlik sağlayıcılardan gelen.
2. Değerini güncelleştirin `<DisplayName>` için talep sağlayıcısı için bir görünen ad. Şu anda, bu değeri kullanılmaz.

### <a name="update-the-technical-profile"></a>Teknik profilini güncelleştir

Salesforce'tan SAML belirteci almak için Azure AD B2C, Azure Active Directory (Azure AD) ile iletişim kurmak için kullanacağı iletişim kuralları tanımlayın. Bunu yapmak `<TechnicalProfile>` öğesinin `<ClaimsProvider>`:

1. Güncelleştirme kimliği `<TechnicalProfile>` düğümü. Bu kimlik, ilkenin diğer bölümlerden Bu teknik profile başvurmak için kullanılır.
2. Değerini güncelleştirin `<DisplayName>`. Bu değer, oturum açma sayfasında oturum açın düğmesinde görüntülenir.
3. Değerini güncelleştirin `<Description>`.
4. Salesforce, SAML 2.0 protokolünü kullanır. Değeri emin `<Protocol>` olduğu **SAML2**.

Güncelleştirme `<Metadata>` belirli Salesforce hesabınız için ayarları yansıtacak şekilde önceki XML bölümünde. XML'de, meta veri değerlerini güncelleştirin:

1. Değerini güncelleştirin `<Item Key="PartnerEntity">` ile daha önce kopyaladığınız Salesforce meta veri URL'si. Bunu, aşağıdaki biçime sahiptir: 

    `https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp/connectedapp.xml`

2. İçinde `<CryptographicKeys>` bölümünde, her iki değeri güncelleştirme `StorageReferenceId` imzalama sertifikanızın (örneğin, B2C_1A_SAMLSigningCert) anahtar adını.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını karşıya yükle

Azure AD B2C Salesforce ile iletişim kurma bilebilmesi ilkenizi artık yapılandırılmıştır. Uzantı dosyası ilkenizin olmadığından herhangi bir sorun şu ana kadar doğrulamak için karşıya yüklemeyi deneyin. İlkenizin uzantı dosyasını karşıya yüklemek için:

1. Azure AD B2C kiracınıza gitmek **tüm ilkeleri** dikey penceresi.
2. Seçin **ilke varsa üzerine** onay kutusu.
3. Uzantı dosyası (TrustFrameworkExtensions.xml) karşıya yükleyin. Doğrulama başarısız olmayan emin olun.

## <a name="register-the-salesforce-saml-claims-provider-to-a-user-journey"></a>Kullanıcı yolculuğu için Salesforce SAML talep sağlayıcısını Kaydet

Ardından, Salesforce SAML kimlik sağlayıcısı, kullanıcı yolculuklarından birine ekleyin. Bu noktada, kimlik sağlayıcısı ayarlandı, ancak herhangi bir kullanıcı kaydolma veya oturum açma sayfasında kullanılabilir değil. Kimlik sağlayıcısı oturum açma sayfasına eklemek için ilk olarak mevcut bir şablonu kullanıcı yolculuğu bir kopyasını oluşturun. Ardından, ayrıca Azure AD kimlik sağlayıcısına sahip olacak şekilde şablonu değiştirin.

1. Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.
2. Bulma `<UserJourneys>` öğesini ve ardından kopyalama tüm `<UserJourney>` değer, "SignUpOrSignIn" kimliğine.
3. Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın. Bulma `<UserJourneys>` öğesi. Öğe yoksa bir tane oluşturun.
4. Kopyalanan tüm yapıştırın `<UserJourney>` bir alt öğesi olarak `<UserJourneys>` öğesi.
5. Yeni Kimliğini Yeniden Adlandır `<UserJourney>` (örneğin, SignUpOrSignUsingContoso).

### <a name="display-the-identity-provider-button"></a>Kimlik sağlayıcısının düğmesine görüntüleme

`<ClaimsProviderSelection>` Öğedir kaydolma veya oturum açma sayfasında bir kimlik sağlayıcısı düğmesi benzer. Ekleyerek bir `<ClaimsProviderSelection>` öğesi Salesforce için yeni bir düğme görünür bir kullanıcı bu sayfaya geri gittiğinde. Kimlik sağlayıcısı düğme görüntülemek için:

1. İçinde `<UserJourney>` oluşturduğunuz, Bul `<OrchestrationStep>` ile `Order="1"`.
2. Aşağıdaki XML'i ekleyin:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

3. Ayarlama `TargetClaimsExchangeId` mantıksal bir değer. Başkalarının aynı kuralı aşağıdaki öneririz (örneğin,  *\[ClaimProviderName\]Exchange*).

### <a name="link-the-identity-provider-button-to-an-action"></a>Kimlik sağlayıcısı düğme için bir eylem ile bağlantı kurun.

Yerinde bir kimlik sağlayıcısının düğmesine sahip olduğunuza göre bir eyleme bağlantı. Bu durumda, bir SAML belirteci almak için Salesforce ile iletişim kurmak Azure AD B2C için eylemdir. İçin Salesforce SAML teknik profili bağlayarak Talep sağlayıcı bunu yapabilirsiniz:

1. İçinde `<UserJourney>` düğümünü bulun `<OrchestrationStep>` ile `Order="2"`.
2. Aşağıdaki XML'i ekleyin:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

3. Güncelleştirme `Id` daha önce kullandığınız aynı değere `TargetClaimsExchangeId`.
4. Güncelleştirme `TechnicalProfileReferenceId` için `Id` , daha önce oluşturulan (örneğin, ContosoProfile) Teknik profil.

### <a name="upload-the-updated-extension-file"></a>Güncelleştirilmiş uzantı dosyasını karşıya yükle

İşiniz uzantısının değiştirme. Kaydet ve bu dosyayı karşıya yükleyin. Tüm Doğrulamalar başarıyla emin olun.

### <a name="update-the-relying-party-file"></a>Bağlı olan taraf dosyasını güncelleştirme

Ardından, oluşturduğunuz kullanıcı yolculuğu başlatan bağlı olan taraf (RP) dosyasını güncelleştirin:

1. Çalışma dizininizde SignUpOrSignIn.xml kopyalanmasına. Ardından, dosyayı (örneğin, SignUpOrSignInWithAAD.xml) yeniden adlandırın.
2. Yeni bir dosya açıp güncelleştirme `PolicyId` özniteliğini `<TrustFrameworkPolicy>` benzersiz bir değere sahip. İlkenizi (örneğin, SignUpOrSignInWithAAD) adıdır.
3. Değiştirme `ReferenceId` özniteliğini `<DefaultUserJourney>` eşleştirilecek `Id` (örneğin, SignUpOrSignUsingContoso) oluşturduğunuz yeni kullanıcı yolculuğunun.
4. Yaptığınız değişiklikleri kaydedin ve ardından dosyayı karşıya yükleyin.

## <a name="test-and-troubleshoot"></a>Test etme ve sorun giderme

Azure portalında yeni yüklediğiniz özel bir ilkeyi test etmek için ilke dikey penceresine gidin ve ardından **Şimdi Çalıştır**. Başarısız olursa bkz [özel ilke sorunlarını giderme](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Sonraki adımlar

Geri bildirim sağlamak [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
