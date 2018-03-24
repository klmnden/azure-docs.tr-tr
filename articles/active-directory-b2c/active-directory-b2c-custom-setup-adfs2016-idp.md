---
title: 'Azure Active Directory B2C: ADFS özel ilkelerini kullanma SAML kimlik sağlayıcısı ekleyin.'
description: ADFS SAML protokolü ve özel ilkeler kullanılarak 2016 ayarlama nasıl yapılır makalesi
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 08/04/2017
ms.author: davidmu
ms.openlocfilehash: af102bbc3bc7608fe641db19f4af8c760907a564
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-add-adfs-as-a-saml-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: ADFS özel ilkelerini kullanma SAML kimlik sağlayıcısı ekleyin.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, oturum açma kullanılarak ADFS hesabından kullanıcılar için etkinleştirme gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [özel ilkeleri ile çalışmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

Bu adımlar şunları içerir:

1.  Bağlı olan taraf güveni ADFS oluşturuluyor.
2.  ADFS bağlı olan taraf güveni sertifika Azure AD B2C'ye ekleniyor.
3.  Talep sağlayıcı için bir ilke ekleniyor.
4.  ADFS kaydetme hesabı kullanıcı gezisine sağlayıcısına talepleri.
5.  İlke için bir Azure AD B2C karşıya yükleme, Kiracı ve test.

## <a name="to-create-a-claims-aware-relying-party-trust"></a>Bir talep kullanan bağlı olan taraf güveni oluşturmak için

ADFS Azure Active Directory (Azure AD) B2C bir kimlik sağlayıcısı olarak kullanmak için bir ADFS bağlı olan taraf güveni oluşturmak ve doğru parametrelerle sağlamanız gerekir.

AD FS Yönetimi ek bileşenini kullanarak yeni bir bağlı olan taraf güveni eklemek ve ayarları el ile yapılandırmak için bir federasyon sunucusu üzerinde aşağıdaki yordamı gerçekleştirin.

Üyelik **Yöneticiler**, ya da eşdeğer yerel bilgisayarda bu yordamı tamamlamak için gereken en düşük gereksinimdir. Uygun hesapları kullanmayla ilgili ayrıntıları gözden geçirin ve grup üyeliklerini [yerel ve etki alanı varsayılan grupları](http://go.microsoft.com/fwlink/?LinkId=83477)

1.  Sunucu Yöneticisi'nde **Araçları**ve ardından **ADFS Yönetim**.

2.  Tıklayın **bağlı olan taraf güveni ekleme**.
    ![Bağlı olan taraf güveni ekleme](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)

3.  Üzerinde **Hoş Geldiniz** sayfasında, **talep kullanan** tıklatıp **Başlat**.
    ![Hoş Geldiniz sayfasında, talep kullanan seçin](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)
4.  Üzerinde **veri kaynağı Seç** sayfasında, **bağlı olan taraf verilerini elle girin**ve ardından **sonraki**.
    ![Bağlı olan taraf verilerini girin](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)

5.  Üzerinde **görünen adı belirt** sayfasında, bir ad yazın **görünen adı**altında **notları** bu bağlı olan taraf güveni için bir açıklama yazın ve ardından **sonraki** .
    ![Görünen ad ve notlar belirtin](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)
6.  İsteğe bağlı. Bir isteğe bağlı belirteç şifreleme sertifikası sonra varsa **sertifika Yapılandır** sayfasında, **Gözat** , sertifika dosyasını bulun ve ardından **sonraki**.
    ![Sertifika yapılandırma](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)
7.  Üzerinde **URL Yapılandır** sayfasında, **SAML 2.0 WebSSO protokolü için desteği etkinleştir** onay kutusu. Altında **bağlı olan taraf SAML 2.0 SSO hizmet URL'si**, bu bağlı olan taraf güveni için güvenlik onaylama işlemi biçimlendirme dili (SAML) Hizmeti uç nokta URL'sini yazın ve ardından **sonraki**.  İçin **bağlı olan taraf SAML 2.0 SSO hizmet URL'si**, yapıştırma `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}`. {Tenant} (örneğin, contosob2c.onmicrosoft.com), kiracının adıyla değiştirin ve {İlkesi} uzantıları ilke adı (örneğin, B2C_1A_TrustFrameworkExtensions) ile değiştirin.
    > [!IMPORTANT]
    >İlke adı signup_or_signin ilke, çünkü bu durumda devraldığı bilgisayardır: `B2C_1A_TrustFrameworkExtensions`.
    >Örneğin bir URL olabilir: https://login.microsoftonline.com/te/ **contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.

    ![Bağlı olan taraf SAML 2.0 SSO hizmet URL'si](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-6.png)
8. Üzerinde **tanımlayıcıları yapılandırma** sayfasında, önceki adımı olarak aynı URL'yi belirtin, **Ekle** bunları listeye ekleyin ve ardından **sonraki**.
    ![Bağlı olan taraf güveni tanımlayıcıları](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)
9.  Üzerinde **erişim denetimi ilkesini seçin** bir ilke seçin ve tıklatın **sonraki**.
    ![Erişim denetimi ilkesini seçin](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)
10.  Üzerinde **güven eklemeye hazır** sayfasında, ayarları gözden geçirin ve ardından **sonraki** bağlı olan taraf kaydetmek için güven bilgileri.
    ![Bağlı olan taraf güven bilgilerinizi Kaydet](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)
11.  Üzerinde **son** sayfasında, **Kapat**, bu eylem otomatik olarak görüntüler **talep kurallarını Düzenle** iletişim kutusu.
    ![Talep kurallarını Düzenle](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)
12. Tıklatın **Kuralı Ekle**.  
      ![Yeni Kural Ekle](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)
13.  İçinde **talep kuralı şablonu**seçin **Gönder LDAP özniteliklerini talep olarak**.
    ![Gönderme LDAP özniteliklerini talep şablonu kural olarak seçin](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)
14.  Sağlamak **talep kuralı adı**. İçin **öznitelik deposu** seçin **seçin Active Directory** aşağıdaki talep ekleyin ve ardından **son** ve **Tamam**.
    ![Kural özelliklerini ayarlama](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)
15.  Sunucu Yöneticisi'nde seçin **bağlı olan taraf güvenleri** sonra seçin bağlı olan taraf oluşturduğunuz güven ve ' **özellikleri**.
    ![Bağlı olan taraf Özellikleri Düzenle](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)
16.  Bir bağlı olan taraf güveni (B2C Demo) Özellikler penceresini tıklatın **imza** sekmesinde **Ekle**.  
    ![Set imza](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)
17.  İmza sertifikanızı (özel anahtarı olmayan .cert dosyası) ekleyin.  
    ![İmza sertifikası ekleme](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-3.png)
18.  Bağlı olan taraf güveni (B2C Demo) Özellikleri penceresinde tıklatın **Gelişmiş** sekmesinde ve değiştirme **güvenli karma algoritması** için **SHA-1**, tıklatın **Tamam**.  
    ![Güvenli Karma algoritması SHA-1 olarak ayarlayın](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)

## <a name="add-the-adfs-account-application-key-to-azure-ad-b2c"></a>Azure AD B2C'ye ADFS hesap uygulama anahtarı Ekle
ADFS hesaplarıyla federasyon güven Azure AD B2C uygulama adına ADFS hesabına istemci gizli anahtarı gerektirir. Azure AD B2C kiracınızda ADFS sertifikanızı depolamanız gerekir. 

1.  Azure AD B2C kiracınızın gidin ve seçin **B2C ayarlarını** > **kimlik deneyimi Framework**
2.  Seçin **İlkesi anahtarları** kiracınızda kullanılabilir tuşlarını görmek için.
3.  Tıklatın **+ Ekle**.
4.  İçin **seçenekleri**, kullanın **karşıya**.
5.  İçin **adı**, kullanmak `ADFSSamlCert`.  
    Önek `B2C_1A_` otomatik olarak eklenebilir.
6.  Dosya karşıya yükleme içinde ** özel anahtarla sertifika .pfx dosyasını seçin. Not: Bu sertifikayı (özel anahtarla) verilen ve ADFS bağlı olan taraf için kullanılan adla aynı olmalıdır.
![İlke anahtarını karşıya yükleyin](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)
7.  **Oluştur**'a tıklayın
8.  Anahtar oluşturduğunuz onaylayın `B2C_1A_ADFSSamlCert`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Bir talep sağlayıcı uzantısı ilkenizde ekleme
ADFS hesabını kullanarak oturum açmalarını istiyorsanız, bir talep sağlayıcısı olarak ADFS hesap tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C ile iletişim kuran bir uç nokta belirtmeniz gerekir. Uç nokta Azure AD B2C tarafından belirli bir kullanıcı kimliği doğrulanmış olduğunu doğrulamak için kullanılan talep kümesini sağlar.

ADFS ekleyerek bir talep sağlayıcısı olarak tanımlamak `<ClaimsProvider>` İlkesi uzantısının düğümünde:

1. Uzantı ilke dosyası (TrustFrameworkExtensions.xml) çalışma dizininizi açın. Bir XML Düzenleyici, gerekirse [Visual Studio Code deneyin](https://code.visualstudio.com/download), basit bir platformlar arası Düzenleyici.
2. Bul `<ClaimsProviders>` bölümü
3. Altında aşağıdaki XML parçacığını ekleyin `ClaimsProviders` öğesi ve Değiştir `identityProvider` DNS sunucunuzun (etki alanınızdaki gösterir rastgele değer) ile ve dosyayı kaydedin. 

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

## <a name="register-the-adfs-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Yukarı imzalamak için ADFS hesap talep sağlayıcısı kaydedilemedi veya kullanıcı gezisine oturum
Bu noktada, kimlik sağlayıcısı ayarlandığına.  Ancak, oturumu-up/oturum açma ekranları hiçbirinde kullanılabilir değil. ADFS hesabı kimlik sağlayıcısı, kullanıcı eklemek gereken artık `SignUpOrSignIn` kullanıcı gezisine. Kullanılabilir hale getirmek için var olan bir şablonu kullanıcı gezisine tekrarı oluşturuyoruz.  ADFS kimlik sağlayıcısı içerecek şekilde sonra biz değiştirebilirsiniz:

>[!NOTE]
>Daha önce kopyaladıysanız `<UserJourneys>` ilkenizin temel dosyanın öğesine uzantısının (TrustFrameworkExtensions.xml bu bölümü atlayın).

1.  Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.
2.  Bul `<UserJourneys>` öğesi ve tüm içeriğini kopyalayın `<UserJourneys>` düğümü.
3.  Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve Bul `<UserJourneys>` öğesi. Öğe yoksa, ekleyin.
4.  Tüm içeriğini yapıştırın `<UserJournesy>` bir alt öğesi olarak kopyaladığınız düğümü `<UserJourneys>` öğesi.

### <a name="display-the-button"></a>Görüntü düğmesi
`<ClaimsProviderSelections>` Öğesi talep sağlayıcısı seçme seçenekleri ve bunların sırası listesini tanımlar.  `<ClaimsProviderSelection>` öğesi, bir oturumu-up/oturum açma sayfasında bir kimlik sağlayıcısı düğmesini benzerdir. Eklerseniz bir `<ClaimsProviderSelection>` öğesi ADFS hesap için yeni bir düğme görüntülenir sayfasında bir kullanıcı adlandırıldığını olduğunda. Bu öğe eklemek için:

1.  Bul `<UserJourney>` içeren düğüm `Id="SignUpOrSignIn"` kopyaladığınız kullanıcı gezisine içinde.
2.  Bulun `<OrchestrationStep>` içeren düğümü `Order="1"`
3.  Aşağıdaki XML parçacığını altında ekleyin `<ClaimsProviderSelections>` düğümü:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```
### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem bağlantı kurun

Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için ADFS hesabı ile iletişim kurmak Azure AD B2C içindir. Düğme, ADFS hesap talep sağlayıcısı teknik profili bağlayarak bir eyleme bağlantı:

1.  Bul `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
2.  Aşağıdaki XML parçacığını altında ekleyin `<ClaimsExchanges>` düğümü:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

> [!NOTE]
> * Olun `Id` , aynı değere sahip `TargetClaimsExchangeId` önceki bölümde.
> * Olun `TechnicalProfileReferenceId` ayarlanmış teknik profiline oluşturduğunuz önceki (Contoso-SAML2).

## <a name="upload-the-policy-to-your-tenant"></a>İlke kiracınız için karşıya yükleme
1.  İçinde [Azure portal](https://portal.azure.com), içine geçiş [bağlam Azure AD B2C kiracınızın](active-directory-b2c-navigate-to-b2c-context.md), açarak **Azure AD B2C** dikey.
2.  Seçin **kimlik deneyimi Framework**.
3.  Açık **tüm ilkeler** dikey.
4.  Seçin **karşıya İlkesi**.
5.  Denetleme **varsa ilkesi üzerine** kutusu.
6.  **Karşıya yükleme** TrustFrameworkExtensions.xml ve doğrulama başlayabildiğinden emin olun

## <a name="test-the-custom-policy-by-using-run-now"></a>Özel ilke Şimdi Çalıştır kullanarak test
1.  Açık **Azure AD B2C ayarlarını** ve Git **kimlik deneyimi Framework**.
2.  Açık **B2C_1A_signup_signin**, karşıya yüklediğiniz bağlı olan taraf (RP) özel ilke. Seçin **Şimdi Çalıştır**.
3.  ADFS hesabı kullanarak oturum olması gerekir.

## <a name="optional-register-the-adfs-account-claims-provider-to-profile-edit-user-journey"></a>[İsteğe bağlı] Profil düzenleme kullanıcı gezisine ADFS hesap talep sağlayıcısını Kaydet
ADFS hesabı kimlik sağlayıcısı Ayrıca, kullanıcı eklemek isteyebilirsiniz `ProfileEdit` kullanıcı gezisine. Kullanılabilir hale getirmek için biz son iki adımı yineleyin:

### <a name="display-the-button"></a>Görüntü düğmesi
1.  Uzantı dosyası ilkenizin (örneğin, TrustFrameworkExtensions.xml) açın.
2.  Bul `<UserJourney>` içeren düğüm `Id="ProfileEdit"` kopyaladığınız kullanıcı gezisine içinde.
3.  Bulun `<OrchestrationStep>` içeren düğümü `Order="1"`
4.  Aşağıdaki XML parçacığını altında ekleyin `<ClaimsProviderSelections>` düğümü:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem bağlantı kurun
1.  Bul `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
2.  Aşağıdaki XML parçacığını altında ekleyin `<ClaimsExchanges>` düğümü:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Özel Profil Düzenleme İlkesi Şimdi Çalıştır kullanarak test
1.  Açık **Azure AD B2C ayarlarını** ve Git **kimlik deneyimi Framework**.
2.  Açık **B2C_1A_ProfileEdit**, karşıya yüklediğiniz bağlı olan taraf (RP) özel ilke. Seçin **Şimdi Çalıştır**.
3.  ADFS hesabı kullanarak oturum olması gerekir.

## <a name="download-the-complete-policy-files"></a>Tam ilke dosyaları indirme
İsteğe bağlı: Dosyaları ile çalışmaya başlama özel ilkeler tamamladıktan sonra size yol kendi özel İlkesi kullanarak senaryonuz yapı öneririz. [İlke örnek dosyaları yalnızca başvuru için](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app)
