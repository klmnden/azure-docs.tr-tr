---
title: Azure Active Directory B2C'de özel ilkeler kullanarak Salesforce SAML sağlayıcı ekleme | Microsoft Docs
description: Azure Active Directory B2C özel ilkeleri oluşturun ve yönetin hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 06/11/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f4399730755c15fe5e171bf7fd5826c2b7ffea0a
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34709657"
---
# <a name="azure-active-directory-b2c-sign-in-by-using-salesforce-accounts-via-saml"></a>Azure Active Directory B2C: SAML aracılığıyla Salesforce hesaplarını kullanarak oturum açın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede nasıl kullanılacağı gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md) oturum açma kullanıcıların belirli bir Salesforce kuruluştan ayarlamak için.

## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-ad-b2c-setup"></a>Azure AD B2C Kurulumu

Nasıl yapacağınızı tüm adımları tamamladığınızdan emin olmak için [özel ilkelerini kullanmaya başlama](active-directory-b2c-get-started-custom.md) Azure Active Directory B2C içinde (Azure AD B2C).

Bunlar:

* Azure AD B2C kiracısı oluşturun.
* Bir Azure AD B2C uygulaması oluşturun.
* İki ilke altyapısı uygulama kaydedin.
* Anahtarları ayarlayın.
* Başlangıç paketi ayarlayın.

### <a name="salesforce-setup"></a>Salesforce Kurulum

Bu makalede, aşağıdaki yapmış olduğunuz olduğunu varsayın:

* İmzalanmış Salesforce hesabınız için. Oturum açabileceğiniz bir [ücretsiz bir geliştirici sürümü hesap](https://developer.salesforce.com/signup).
* [My etki alanını ayarlama](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) Salesforce kuruluşunuz için.

## <a name="set-up-salesforce-so-users-can-federate"></a>Kullanıcıların devredebilir Salesforce ayarlandı

Azure AD B2C ile Salesforce iletişim yardımcı olmak için Salesforce meta veri URL'sini almanız gerekir.

### <a name="set-up-salesforce-as-an-identity-provider"></a>Salesforce kimlik sağlayıcısı olarak ayarlayın

> [!NOTE]
> Bu makalede, kullanmakta olan varsayıyoruz [Salesforce Şimşek deneyimi](https://developer.salesforce.com/page/Lightning_Experience_FAQ).

1. [Salesforce oturum](https://login.salesforce.com/). 
2. Sol menüde altında **ayarları**, genişletin **kimlik**ve ardından **kimlik sağlayıcısı**.
3. Tıklatın **etkinleştirme kimlik sağlayıcısı**.
4. Altında **sertifikayı seçin**, Salesforce Azure AD B2C ile iletişim kurmak için kullanmak istediğiniz sertifikayı seçin. (Varsayılan sertifikayı kullanabilirsiniz.) **Kaydet**’e tıklayın. 

### <a name="create-a-connected-app-in-salesforce"></a>Salesforce'ta bağlı bir uygulama oluşturma

1. Üzerinde **kimlik sağlayıcısı** sayfasında, Git **hizmet sağlayıcıları**.
2. Tıklatın **hizmet sağlayıcıları bağlı uygulamalar şimdi oluşturulur. Burayı tıklatın.**
3. Altında **temel bilgileri**, bağlı uygulamanız için gerekli değerleri girin.
4. Altında **Web uygulaması ayarları**seçin **etkinleştirmek SAML** onay kutusu.
5. İçinde **varlık kimliği** alanında, aşağıdaki URL'yi girin. Değeri yerini olmak `tenantName`.
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```
6. İçinde **ACS URL** alanında, aşağıdaki URL'yi girin. Değeri yerini olmak `tenantName`.
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. Tüm diğer ayarlar için varsayılan değerleri bırakın.
8. Listenin sonuna kaydırın ve ardından **kaydetmek**.

### <a name="get-the-metadata-url"></a>Meta veri URL'sini alın

1. Bağlı uygulamanızı genel bakış sayfasında **Yönet**.
2. Değeri kopyalama **meta veri bulma uç noktası**ve kaydedin. Bu makalenin sonraki bölümlerinde kullanacaksınız.

### <a name="set-up-salesforce-users-to-federate"></a>Salesforce kullanıcıları birleştirmek için ayarlama

1. Üzerinde **Yönet** sayfasında, bağlı uygulama, Git **profilleri**.
2. Tıklatın **profillerini yönetmek**.
3. Profilleri (veya kullanıcı grupları) seçin, Azure AD B2C ile birleştirmek istediğiniz. Bir sistem yöneticisi olarak seçin **Sistem Yöneticisi** Salesforce hesabınıza kullanarak birleştirmek için onay kutusunu.

## <a name="generate-a-signing-certificate-for-azure-ad-b2c"></a>Azure AD B2C için imzalama sertifikası oluştur

Salesforce gönderilen istekleri Azure AD B2C tarafından imzalanması gerekir. Bir imzalama sertifikası oluşturmak için Azure PowerShell'i açın ve aşağıdaki komutları çalıştırın.

> [!NOTE]
> Kiracı adı ve parola üstteki iki satırlarındaki güncelleştirdiğinizden emin olun.

```PowerShell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="add-the-saml-signing-certificate-to-azure-ad-b2c"></a>Azure AD B2C'ye SAML imzalama sertifikası ekleme

İmzalama sertifikasının, Azure AD B2C kiracınızın karşıya yükle: 

1. Azure AD B2C kiracınızın gidin. Tıklatın **ayarları** > **kimlik deneyimi Framework** > **İlkesi anahtarları**.
2. Tıklatın **+ Ekle**ve ardından:
    1. Tıklatın **seçenekleri** > **karşıya**.
    2. Girin bir **adı** (örneğin, SAMLSigningCert). Önek *B2C_1A_* anahtarınızı adına otomatik olarak eklenir.
    3. Sertifikanızı seçmek için **dosya denetimi karşıya**. 
    4. PowerShell komut dosyasında ayarlanan sertifikanın parolasını girin.
3. **Oluştur**’a tıklayın.
4. Bir anahtar (örneğin, B2C_1A_SAMLSigningCert) oluşturduğunuzdan emin olun. Tam adını not edin (de dahil olmak üzere *B2C_1A_*). Daha sonra ilkesinde bu anahtara başvurur.

## <a name="create-the-salesforce-saml-claims-provider-in-your-base-policy"></a>Temel ilkenizde Salesforce SAML Talep sağlayıcı oluşturma

Kullanıcıların Salesforce kullanarak oturum açabilmeniz için bir talep sağlayıcısı olarak Salesforce tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C ile iletişim kuracak uç noktası belirtmeniz gerekir. Uç nokta olacak *sağlamak* bir dizi *talep* , belirli bir kullanıcı kimliği doğrulanmış olduğunu doğrulamak için Azure AD B2C kullanır. Bunu yapmak için ekleyin bir `<ClaimsProvider>` ilkenizin uzantısı dosyasında Salesforce için:

1. Uzantı dosyası (TrustFrameworkExtensions.xml) çalışma dizininizi açın.
2. Bul `<ClaimsProviders>` bölümü. Yoksa, kök düğümü altında oluşturun.
3. Yeni bir ekleme `<ClaimsProvider>`:

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
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="externalIdp"/>
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

Altında `<ClaimsProvider>` düğümü:

1. Değeri değiştirme `<Domain>` ayırt eden benzersiz bir değere `<ClaimsProvider>` diğer kimlik sağlayıcılardan gelen.
2. Değeri güncelleştirme `<DisplayName>` talep sağlayıcısı için bir görünen ad için. Şu anda, bu değer kullanılmıyor.

### <a name="update-the-technical-profile"></a>Teknik profilini güncelleştir

Salesforce bir SAML belirtecini almak için Azure AD B2C Azure Active Directory (Azure AD) ile iletişim kurmak için kullanacağınız protokolleri tanımlayın. Bunu yapmak `<TechnicalProfile>` öğesinin `<ClaimsProvider>`:

1. Kimliği güncelleştirme `<TechnicalProfile>` düğümü. Bu kimliği, bu teknik profili ilkesinin diğer bölümlerden başvurmak için kullanılır.
2. Değeri güncelleştirme `<DisplayName>`. Bu değer, oturum açma sayfasında oturum açma düğmesi görüntülenir.
3. Değeri güncelleştirme `<Description>`.
4. Salesforce SAML 2.0 protokolü kullanılır. Değeri emin `<Protocol>` olan **SAML2**.

Güncelleştirme `<Metadata>` belirli Salesforce hesabınıza ilişkin ayarları yansıtacak şekilde önceki XML bölümünde. XML'de, meta veri değerlerini güncelleştirin:

1. Değerini güncelleştirmek `<Item Key="PartnerEntity">` daha önce kopyaladığınız Salesforce meta veri URL'si. Aşağıdaki biçime sahiptir: 

    `https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp/connectedapp.xml`

2. İçinde `<CryptographicKeys>` bölümünde, her iki örnekleri için değeri güncelleştirin `StorageReferenceId` imzalama sertifikanızın (örneğin, B2C_1A_SAMLSigningCert) anahtar adını.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını karşıya yükle

Böylece Azure AD B2C Salesforce ile iletişim kurmak bildiği ilkeniz artık yapılandırılmıştır. Uzantı dosyası ilkenizin olmadığından sorunları kadar doğrulamak için yüklemeyi deneyin. Uzantı dosyası ilkenizin karşıya yüklemek için:

1. Azure AD B2C kiracınızda Git **tüm ilkeler** dikey.
2. Seçin **varsa ilkesi üzerine** onay kutusu.
3. Uzantı dosyası (TrustFrameworkExtensions.xml) karşıya yükleyin. Doğrulama başarısız olmayan emin olun.

## <a name="register-the-salesforce-saml-claims-provider-to-a-user-journey"></a>Bir kullanıcı gezisine Salesforce SAML talep sağlayıcısını Kaydet

Ardından, Salesforce SAML kimlik sağlayıcısı, kullanıcı Yolculuklar birine ekleyin. Bu noktada, kimlik sağlayıcısı ayarlandı, ancak herhangi bir kullanıcı oturum açma veya kaydolma sayfalarını üzerinde kullanılabilir değil. Bir oturum açma sayfasına kimlik sağlayıcısı eklemek için ilk olarak, var olan bir şablonu kullanıcı gezisine bir kopyasını oluşturun. Ardından, ayrıca Azure AD kimlik sağlayıcısı sahip olması şablon değiştirin.

1. Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.
2. Bul `<UserJourneys>` öğesini ve ardından kopyalama tüm `<UserJourney>` değeri, "SignUpOrSignIn" kimliğine dahil olmak üzere.
3. Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın. Bul `<UserJourneys>` öğesi. Öğe yoksa, bir tane oluşturun.
4. Kopyalanan tüm Yapıştır `<UserJourney>` bir alt öğesi olarak `<UserJourneys>` öğesi.
5. Yeni kimliği yeniden adlandırma `<UserJourney>` (örneğin, SignUpOrSignUsingContoso).

### <a name="display-the-identity-provider-button"></a>Görüntü kimlik sağlayıcısı düğmesi

`<ClaimsProviderSelection>` Öğesidir kaydolma veya oturum açma sayfasında bir kimlik sağlayıcısı düğmesini benzer. Ekleyerek bir `<ClaimsProviderSelection>` öğesi Salesforce için yeni bir düğme görüntülenir bir kullanıcı bu sayfaya geçtiğinde. Kimlik sağlayıcısı düğmesini görüntülemek için:

1. İçinde `<UserJourney>` oluşturduğunuz, Bul `<OrchestrationStep>` ile `Order="1"`.
2. Aşağıdaki XML ekleyin:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

3. Ayarlama `TargetClaimsExchangeId` için mantıksal bir değer. Aynısına başkalarının aşağıdaki öneririz (örneğin,  *\[ClaimProviderName\]Exchange*).

### <a name="link-the-identity-provider-button-to-an-action"></a>Kimlik sağlayıcısı düğmesine bir eyleme bağlantı

Bir kimlik sağlayıcısı düğmesi yerinde sahip olduğunuza göre bir eyleme bağlantı. Bu durumda, bir SAML belirteci almak için Salesforce ile iletişim kurmak Azure AD B2C için eylemdir. Teknik profil için Salesforce SAML bağlayarak Talep sağlayıcı bunu yapabilirsiniz:

1. İçinde `<UserJourney>` düğümünde, bulmak `<OrchestrationStep>` ile `Order="2"`.
2. Aşağıdaki XML ekleyin:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

3. Güncelleştirme `Id` daha önce kullandığınız aynı değere `TargetClaimsExchangeId`.
4. Güncelleştirme `TechnicalProfileReferenceId` için `Id` teknik daha önce oluşturduğunuz (örneğin, ContosoProfile) profil.

### <a name="upload-the-updated-extension-file"></a>Güncelleştirilmiş uzantı dosyasını karşıya yükle

Tamamladığınızda uzantısının değiştirme. Kaydet ve bu dosyayı karşıya yükleyin. Tüm Doğrulamalar başarıyla emin olun.

### <a name="update-the-relying-party-file"></a>Bağlı olan taraf dosyasını güncelleştirme

Ardından, oluşturduğunuz kullanıcı gezisine başlatır bağlı olan taraf (RP) dosyasını güncelleştirin:

1. Çalışma dizininizi SignUpOrSignIn.xml kopyası. Sonra (örneğin, SignUpOrSignInWithAAD.xml) yeniden adlandırın.
2. Yeni dosya ve güncelleştirme açmak `PolicyId` için öznitelik `<TrustFrameworkPolicy>` benzersiz bir değere sahip. İlkeniz (örneğin, SignUpOrSignInWithAAD) adıdır.
3. Değiştirme `ReferenceId` özniteliğini `<DefaultUserJourney>` eşleşecek şekilde `Id` (örneğin, SignUpOrSignUsingContoso) oluşturulan yeni kullanıcı gezisine biri.
4. Yaptığınız değişiklikleri kaydedin ve dosyayı karşıya yükleyin.

## <a name="test-and-troubleshoot"></a>Test etme ve sorun giderme

Azure Portalı'nda, yeni karşıya yüklediğiniz, özel ilke sınamak için ilke dikey penceresine gidin ve ardından **Şimdi Çalıştır**. Başarısız olursa bkz [özel ilke sorunlarını giderme](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Sonraki adımlar

Geri bildirim sağlayın [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
