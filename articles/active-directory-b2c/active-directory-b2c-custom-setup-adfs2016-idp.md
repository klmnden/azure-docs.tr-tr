---
title: ADFS, Azure Active Directory B2C'de özel ilkeleri kullanarak bir SAML kimlik sağlayıcısı ekleyin | Microsoft Docs
description: AD FS 2016 SAML protokolü ve özel ilkeler kullanarak ayarı bir nasıl yapılır makalesi
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 4380d21eded3f97e3b3107e78c9544a09d1b0bb2
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338539"
---
# <a name="azure-active-directory-b2c-add-adfs-as-a-saml-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Özel ilkeleri kullanarak bir SAML kimlik sağlayıcısı olarak ADFS Ekle

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Oturum açma için kullanıcı kullanımının ADFS hesaptan etkinleştirme Bu makale [özel ilkeler](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

Bu adımlar şunlardır:

1.  Bir AD FS bağlı olan taraf güveni oluşturma.
2.  Azure AD B2C'ye AD FS bağlı olan taraf güveni sertifikası ekleniyor.
3.  Talep sağlayıcı için bir ilke ekleniyor.
4.  ADFS kaydetme hesabı bir kullanıcı yolculuğu sağlayıcısına talepleri.
5.  Bir Azure AD B2C İlkesi karşıya yükleme, Kiracı ve test.

## <a name="to-create-a-claims-aware-relying-party-trust"></a>Bir talep kullanan bağlı olan taraf güveni oluşturmak için

ADFS, Azure Active Directory (Azure AD) B2C'de kimlik sağlayıcısı olarak kullanmak için bir AD FS bağlı olan taraf güveni oluşturma ve doğru parametreleri sağlamanız gerekir.

AD FS Yönetimi ek bileşenini kullanarak yeni bir bağlı olan taraf güveni eklemek ve ayarları el ile yapılandırmak için federasyon sunucusunda aşağıdaki yordamı gerçekleştirin.

Üyelik **Yöneticiler**, veya eşdeğeri, yerel bilgisayarda bu yordamı tamamlamak için gereken en düşük gereksinimdir. Uygun hesapların kullanmayla ilgili ayrıntıları gözden geçirin ve Grup üyeliklerinin [yerel ve etki alanı varsayılan grupları](http://go.microsoft.com/fwlink/?LinkId=83477)

1.  Sunucu Yöneticisi'nde **Araçları**ve ardından **ADFS Yönetim**.

2.  Tıklayarak **bağlı olan taraf güveni ekleme**.
    ![Bağlı olan taraf güveni ekleme](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)

3.  Üzerinde **Hoş Geldiniz** sayfasında **talep kullanan** tıklatıp **Başlat**.
    ![Hoş Geldiniz sayfasında, talep kullanan seçin](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)
4.  Üzerinde **veri kaynağı Seç** sayfasında **bağlı olan taraf verilerini elle girin**ve ardından **sonraki**.
    ![Bağlı olan taraf hakkındaki verileri girin](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)

5.  Üzerinde **görünen adı belirt** sayfasında, bir ad yazın **görünen ad**altında **notları** bu bağlı olan taraf güveni için bir açıklama yazın ve ardından **İleri** .
    ![Görünen ad ve notları belirtin](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)
6.  İsteğe bağlı. Bir isteğe bağlı bir belirteç şifreleme sertifikası sonra varsa **sertifika Yapılandır** sayfasında **Gözat** , sertifika dosyasını bulun ve ardından **sonraki**.
    ![Sertifika yapılandırma](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)
7.  Üzerinde **URL Yapılandır** sayfasında **SAML 2.0 WebSSO protokolü için desteği etkinleştir** onay kutusu. Altında **bağlı olan taraf SAML 2.0 SSO hizmet URL'si**bu bağlı olan taraf güveni için güvenlik onaylama işlemi biçimlendirme dili (SAML) Hizmeti uç nokta URL'sini yazın ve ardından **sonraki**.  İçin **bağlı olan taraf SAML 2.0 SSO hizmet URL'si**, Yapıştır `https://(tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/{policy}`. {Tenant} (örneğin, contosob2c) kiracınızın adıyla değiştirin ve {} İlkesi uzantıları ilke adı (örneğin, B2C_1A_TrustFrameworkExtensions) ile değiştirin.
    > [!IMPORTANT]
    >İlke adı signup_or_signin İlkesi, bu durumda bu devralınan paroladır: `B2C_1A_TrustFrameworkExtensions`.
    >Örneğin bir URL olabilir: https://**contosob2c**.b2clogin.com/te/**contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.

    ![Bağlı olan taraf SAML 2.0 SSO hizmet URL'si](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-6.png)
8. Üzerinde **tanımlayıcı Yapılandır** sayfasında, önceki adım olarak aynı URL'yi belirtin, **Ekle** bunları listeye ekleyin ve ardından **sonraki**.
    ![Bağlı olan taraf güveni tanımlayıcıları](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)
9.  Üzerinde **erişim denetimi ilkesini seçin** bir ilke seçin ve tıklayın **sonraki**.
    ![Erişim denetimi ilkesini seçin](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)
10.  Üzerinde **güven eklemeye hazır** sayfasında, ayarları gözden geçirin ve ardından **sonraki** kaydetmek bağlı olan taraf güven bilgilerinizi.
    ![Bağlı olan taraf güven bilgilerinizi kaydedin](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)
11.  Üzerinde **son** sayfasında **Kapat**, bu eylem otomatik olarak görüntüler **talep kurallarını Düzenle** iletişim kutusu.
    ![Talep kurallarını Düzenle](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)
12. Tıklayın **Kuralı Ekle**.  
      ![Yeni Kural Ekle](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)
13.  İçinde **talep kuralı şablonu**seçin **Gönder LDAP özniteliklerini talep olarak**.
    ![Gönderme LDAP özniteliklerini talep şablon kuralı seçin](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)
14.  Sağlamak **talep kuralı adı**. İçin **öznitelik deposu** seçin **seçin Active Directory** aşağıdaki talep ekleyin, ardından tıklatın **son** ve **Tamam**.
    ![Kural özelliklerini ayarlama](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)
15.  Sunucu Yöneticisi'nde **bağlı olan taraf güvenleri** sonra seçin bağlı olan taraf güveni oluşturduğunuz ve **özellikleri**.
    ![Bağlı olan taraf Özellikleri Düzenle](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)
16.  Bir bağlı olan taraf güveni (B2C Demo) Özellikler penceresini tıklatın **imza** sekmesine **Ekle**.  
    ![Kümesi imzası](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)
17.  (Özel anahtarı olmayan .cert dosyası), imza sertifikası ekleyin.  
    ![İmza sertifikası Ekle](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-3.png)
18.  Bağlı olan taraf güveni (B2C Demo) Özellikleri penceresinde tıklayın **Gelişmiş** sekmesini ve değiştirme **güvenli karma algoritması** için **SHA-1**, tıklayın **Tamam**.  
    ![Güvenli Karma algoritması SHA-1 olarak ayarlayın.](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)

## <a name="add-the-adfs-account-application-key-to-azure-ad-b2c"></a>Azure AD B2C'ye ADFS hesabı uygulama anahtarı Ekle
ADFS hesapları ile federasyon güven Azure AD B2C'yi uygulama adına ADFS hesabına bir istemci gizli anahtarı gerektirir. Azure AD B2C kiracınızda ADFS sertifikanızın depolanacağı gerekir. 

1.  Azure AD B2C kiracınıza gitmek ve seçin **B2C ayarlarını** > **kimlik deneyimi çerçevesi**
2.  Seçin **ilke anahtarları** kiracınızda kullanılabilir tuşlarını görmek için.
3.  Tıklayın **+ Ekle**.
4.  İçin **seçenekleri**, kullanın **karşıya**.
5.  İçin **adı**, kullanın `ADFSSamlCert`.  
    Önek `B2C_1A_` otomatik olarak eklenebilir.
6.  Dosya karşıya yükleme, ** özel anahtara sahip sertifika .pfx dosyanızı seçin. Not: Bu sertifika (özel anahtarı ile) yayımlanan ve AD FS bağlı olan taraf için kullanılan hizmet örneğiyle aynı olmalıdır.
![İlke anahtarı karşıya yükle](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)
7.  **Oluştur**'a tıklayın
8.  Anahtar oluşturduğunuzu doğrulayın `B2C_1A_ADFSSamlCert`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Uzantı ilkenizde bir talep Sağlayıcı Ekle
ADFS hesabını kullanarak oturum açmalarını istiyorsanız, bir talep sağlayıcısı olarak ADFS hesabı tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C ile iletişim kuran bir uç nokta belirtmeniz gerekir. Uç nokta, Azure AD B2C tarafından belirli bir kullanıcı yapıldığını doğrulamak için kullanılan bir talepler kümesi sağlar.

ADFS talep sağlayıcısı olarak ekleyerek tanımlamak `<ClaimsProvider>` ilke uzantısının düğümünde:

1. Uzantı ilke dosyası (TrustFrameworkExtensions.xml) çalışma dizininizi açın. Bir XML düzenleyicisini gerekiyorsa [Visual Studio Code deneyin](https://code.visualstudio.com/download), basit bir platformlar arası Düzenleyici.
2. Bulma `<ClaimsProviders>` bölümü
3. Aşağıdaki XML parçacığını altında ekleyin `ClaimsProviders` öğesi ve Değiştir `identityProvider` DNS (etki alanınızı gösteren rastgele değer) ve dosyayı kaydedin. 

```xml
<ClaimsProvider>
    <Domain>contoso.com</Domain>
    <DisplayName>Contoso ADFS</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Contoso-SAML2">
        <DisplayName>Contoso ADFS</DisplayName>
        <Description>Login with your Contoso account</Description>
        <Protocol Name="SAML2"/>
        <Metadata>
        <Item Key="RequestsSigned">false</Item>
        <Item Key="WantsEncryptedAssertions">false</Item>
        <Item Key="PartnerEntity">https://{your_ADFS_domain}/federationmetadata/2007-06/federationmetadata.xml</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userPrincipalName" />
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

## <a name="register-the-adfs-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Yukarı imzalamak için ADFS hesabı talep sağlayıcısı kaydetme veya kullanıcı yolculuğunda oturum
Bu noktada, kimlik sağlayıcısı ayarlandığını gösterdiğinde.  Ancak, oturumu-kaydolma/oturum açma ekranları hiçbirinde kullanılabilir değil. Artık ADFS hesabı kimlik sağlayıcısı için kullanıcı eklemek gerekiyor `SignUpOrSignIn` kullanıcı yolculuğu. Kullanılabilir hale getirmek için mevcut bir şablonu kullanıcı yolculuğunun yinelenen oluştururuz.  ADFS kimlik sağlayıcısı içerecek sonra biz değiştirebilirsiniz:

>[!NOTE]
>Daha önce kopyalanmadıysa `<UserJourneys>` ilkenizin temel dosya öğesine uzantı dosyası (TrustFrameworkExtensions.xml bu bölümü atlayın).

1.  Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.
2.  Bulma `<UserJourneys>` öğenin ve tüm içeriğini kopyalayın `<UserJourneys>` düğümü.
3.  Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve bulun `<UserJourneys>` öğesi. Öğe yoksa bir tane ekleyin.
4.  Tüm içeriğini yapıştırın `<UserJournesy>` alt öğesi olarak kopyaladığınız düğüm `<UserJourneys>` öğesi.

### <a name="display-the-button"></a>Bir düğme görüntülemek
`<ClaimsProviderSelections>` Öğe talep sağlayıcısı seçme seçenekleri ve bunların sırası listesini tanımlar.  `<ClaimsProviderSelection>` öğesi, bir oturumu-kaydolma/oturum açma sayfasında bir kimlik sağlayıcısı düğmesi benzerdir. Eklerseniz bir `<ClaimsProviderSelection>` öğesi ADFS hesabı için yeni bir düğme gösterilir sayfasında bir kullanıcı gölünüzdeki olduğunda. Bu öğe eklemek için:

1.  Bulma `<UserJourney>` içeren düğüm `Id="SignUpOrSignIn"` kopyaladığınız kullanıcı giden.
2.  Bulun `<OrchestrationStep>` içeren düğüm `Order="1"`
3.  XML kod parçacığı altında aşağıdaki ekleyin `<ClaimsProviderSelections>` düğüm:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```
### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.

Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için ADFS hesabıyla iletişim kurmak için Azure AD B2C içindir. Düğme, ADFS hesabı talep sağlayıcısı teknik profil bağlayarak bir eyleme bağlantı:

1.  Bulma `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
2.  XML kod parçacığı altında aşağıdaki ekleyin `<ClaimsExchanges>` düğüm:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

> [!NOTE]
> * Olun `Id` , aynı değere sahip `TargetClaimsExchangeId` önceki bölümde.
> * Olun `TechnicalProfileReferenceId` ayarlanır teknik profiline önceki (Contoso-SAML2) oluşturulur.

## <a name="upload-the-policy-to-your-tenant"></a>Kiracınız için ilkeyi karşıya yükle
1.  İçinde [Azure portalında](https://portal.azure.com), içine geçiş [Azure AD B2C kiracınızın bağlamında](active-directory-b2c-navigate-to-b2c-context.md)açın **Azure AD B2C** dikey penceresi.
2.  Seçin **kimlik deneyimi çerçevesi**.
3.  Açık **tüm ilkeleri** dikey penceresi.
4.  Seçin **karşıya yükleme İlkesi**.
5.  Denetleme **ilke varsa üzerine** kutusu.
6.  **Karşıya yükleme** TrustFrameworkExtensions.xml ve doğrulama başlayabildiğinden emin olun.

## <a name="test-the-custom-policy-by-using-run-now"></a>Şimdi Çalıştır'i kullanarak özel bir ilkeyi test
1.  Açık **Azure AD B2C ayarlarını** gidin **kimlik deneyimi çerçevesi**.
2.  Açık **B2C_1A_signup_signin**, karşıya yüklediğiniz bağlı olan taraf (RP) özel ilke. Seçin **Şimdi Çalıştır**.
3.  ADFS hesabınızı kullanarak oturum açabilir olması gerekir.

## <a name="optional-register-the-adfs-account-claims-provider-to-profile-edit-user-journey"></a>[İsteğe bağlı] Profil düzenleme kullanıcı yolculuğu için ADFS hesabı talep sağlayıcısını Kaydet
ADFS hesabı kimlik sağlayıcısı Ayrıca, kullanıcı eklemek isteyebileceğiniz `ProfileEdit` kullanıcı yolculuğu. Kullanılabilir hale getirmek için biz son iki adımı yineleyin:

### <a name="display-the-button"></a>Bir düğme görüntülemek
1.  Uzantı dosyası ilkenizin (örneğin, TrustFrameworkExtensions.xml) açın.
2.  Bulma `<UserJourney>` içeren düğüm `Id="ProfileEdit"` kopyaladığınız kullanıcı giden.
3.  Bulun `<OrchestrationStep>` içeren düğüm `Order="1"`
4.  XML kod parçacığı altında aşağıdaki ekleyin `<ClaimsProviderSelections>` düğüm:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.
1.  Bulma `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
2.  XML kod parçacığı altında aşağıdaki ekleyin `<ClaimsExchanges>` düğüm:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Şimdi Çalıştır kullanarak test özel profil Düzenleme İlkesi
1.  Açık **Azure AD B2C ayarlarını** gidin **kimlik deneyimi çerçevesi**.
2.  Açık **B2C_1A_ProfileEdit**, karşıya yüklediğiniz bağlı olan taraf (RP) özel ilke. Seçin **Şimdi Çalıştır**.
3.  ADFS hesabınızı kullanarak oturum açabilir olması gerekir.

## <a name="download-the-complete-policy-files"></a>Tüm ilke dosyalarını indirme
İsteğe bağlı: Senaryonuz dosyaları ile çalışmaya başlama özel ilkeleri tamamladıktan sonra size yol kendi özel İlkesi kullanarak derleme öneririz. [İlke örnek dosyaları yalnızca başvuru için](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app)
