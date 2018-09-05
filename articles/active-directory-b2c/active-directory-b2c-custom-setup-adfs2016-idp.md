---
title: ADFS, Azure Active Directory B2C'de özel ilkeleri kullanarak bir SAML kimlik sağlayıcısı ekleyin | Microsoft Docs
description: AD FS 2016'yı Azure Active Directory B2C'de SAML protokolü ve özel ilkeler kullanarak ayarlama
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 2c2e6861fda42a9e8c1aabcba303bfede47ac3c1
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669235"
---
# <a name="add-adfs-as-a-saml-identity-provider-using-custom-policies-in-azure-active-directory-b2c"></a>ADFS, Azure Active Directory B2C'de özel ilkeleri kullanarak SAML kimlik sağlayıcısı olarak Ekle

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede bir ADFS kullanıcı hesabı için oturum açma kullanarak etkinleştirmek gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md) Azure Active Directory (Azure AD) B2C'de.

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

## <a name="add-the-adfs-account-application-key-to-azure-ad-b2c"></a>Azure AD B2C'ye ADFS hesabı uygulama anahtarı Ekle

Bir ADFS hesapla Federasyon uygulama adına güven Azure AD B2C hesabı istemci gizli anahtarı gerektirir. Azure AD B2C kiracınızda ADFS sertifikanızın depolanacağı gerekir. 

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olmak için Azure portalın sağ üst köşesinde bu dizine geçin. Seçin **dizini Değiştir**, oluşturduğunuz Kiracı içeren dizini seçin. Bu öğreticide *contoso* dizin adlı kiracıda içeren kullanılır *contoso0522Tenant.onmicrosoft.com*.

    ![Dizinleri değiştirme](./media/active-directory-b2c-custom-setup-adfs2016-idp/switch-directories.png)

3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin. Artık kiracınız kullanıyor olması gerekir.
4. Genel bakış sayfasında **kimlik deneyimi çerçevesi**.
5. Seçin **ilke anahtarları** kiracınızda kullanılabilir anahtarlarını görüntüleyebilir ve ardından **Ekle**.
6. Seçin **karşıya** seçeneği olarak.
7. Girin `ADFSSamlCert` adı. Önek `B2C_1A_` otomatik olarak eklenebilir.
8. Göz atın ve özel anahtarla birlikte, sertifika .pfx dosyasını seçin. Bu sertifikayı özel anahtarıyla birlikte verilen ve AD FS bağlı olan taraf için kullanılan hizmet örneğiyle aynı olmalıdır.
9. Tıklayın **Oluştur** ve oluşturduğunuzu doğrulayın `B2C_1A_ADFSSamlCert` anahtarı.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Uzantı ilkenizde bir talep Sağlayıcı Ekle

ADFS hesabını kullanarak oturum açmalarını istiyorsanız, hesabın bir talep sağlayıcısı olarak tanımlamak gerekir. Azure AD B2C ile iletişim kuran bir uç noktası belirterek bunu yapabilirsiniz. Uç nokta, Azure AD B2C tarafından belirli bir kullanıcı yapıldığını doğrulamak için kullanılan bir talepler kümesi sağlar.

ADFS talep sağlayıcısı olarak ekleyerek tanımlamak **ClaimsProvider** ilke uzantısının öğesinde.

1. Açık *TrustFrameworkExtensions.xml* çalışma dizininizde dosya ilkesi. Bir XML düzenleyicisini gerekiyorsa [Visual Studio Code deneyin](https://code.visualstudio.com/download), basit bir platformlar arası Düzenleyici olduğu.
2. Altında aşağıdaki XML'i ekleyin **ClaimsProviders** öğesi ve Değiştir **ADFS etki alanınız** ADFS etki alanınız ile adını ve değerini değiştirin **Identityprovider** Çıkış talep DNS (etki alanınızı gösteren rastgele değer) ve dosyayı kaydedin. 

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
            <Item Key="PartnerEntity">https://your-ADFS-domain/federationmetadata/2007-06/federationmetadata.xml</Item>
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

## <a name="register-the-claims-provider-for-sign-up-and-sign-in"></a>Kaydolma ve oturum açma için talep sağlayıcısını Kaydet

ADFS hesabı kimlik sağlayıcısı kaydolma ve oturum açma sayfaları içinde kullanılabilir hale getirmek için kendisine eklemeniz gerekir, **SignUpOrSignIn** kullanıcı yolculuğu. 

Mevcut bir şablonu kullanıcı yolculuğu bir kopyasını oluşturun ve ardından değişiklik ADFS kimlik sağlayıcısı içerir:

>[!NOTE]
>Daha önce kopyalanmadıysa **UserJourneys** ilkenizin temel dosya öğesine uzantı dosyası (*TrustFrameworkExtensions.xml*), bu bölümü atlayabilirsiniz.

1. İlkenizin temel dosyasını açın. Örneğin, *TrustFrameworkBase.xml*.
2. Tüm içeriğini kopyalayın **UserJourneys** öğesi.
3. Uzantı dosyasını açın (*TrustFrameworkExtensions.xml*) ve tüm içeriğini yapıştırın **UserJourneys** uzantısı dosyasında kopyaladığınız öğesi.

### <a name="display-the-button"></a>Bir düğme görüntülemek

**ClaimsProviderSelections** öğe talep sağlayıcısı seçimleri ve bunların sırası listesini tanımlar.  **ClaimsProviderSelection** öğedir kaydolma ve oturum açma sayfasında bir kimlik sağlayıcısı düğmesi benzer. Eklerseniz bir **ClaimsProviderSelection** öğesi, yeni bir düğme ADFS hesap için kullanıcı sayfası gördüğünde gösterilir. Bu öğe eklemek için:

1. İçinde **UserJourney** tanımlayıcısına sahip öğe `SignUpOrSignIn` kopyaladığınız kullanıcı yolculuklarından bulun **OrchestrationStep** öğesinin `Order="1"`.
2. Ekleme aşağıdaki **ClaimsProviderSelection** öğesi altında **ClaimsProviderSelections** öğesi:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.

Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için ADFS hesabıyla iletişim kurmak için Azure AD B2C içindir. Düğme, ADFS hesabı talep sağlayıcısı teknik profil bağlayarak bir eyleme bağlantı:

1. Bulma **OrchestrationStep** , `Order="2"` altında **UserJourney** öğesi.
2. Ekleme aşağıdaki **ClaimsExchange** öğesi altında **ClaimsExchanges** öğesi:

    ```xml
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```

> [!NOTE]
> * Emin olun `Id` aynı değere sahip `TargetClaimsExchangeId` önceki bölümde.
> * Emin olun `TechnicalProfileReferenceId` ayarlanır teknik profiline önceki (Contoso-SAML2) oluşturulur.


## <a name="optional-register-the-claims-provider-for-profile-edit"></a>[İsteğe bağlı] Profil düzenleme talep sağlayıcısı kaydetme

Profil düzenleme kullanıcı yolculuğunuza ADFS hesabı kimlik sağlayıcısı eklemek isteyebilirsiniz.

### <a name="display-the-button"></a>Bir düğme görüntülemek

1. İlke uzantısı dosyasını açın. Örneğin, *TrustFrameworkExtensions.xml*.
2. İçinde **UserJourney** tanıtıcıya sahip bir öğe `ProfileEdit` kopyaladığınız kullanıcı yolculuklarından bulun **OrchestrationStep** öğesinin `Order="1"`.
3. Ekleme aşağıdaki **ClaimsProviderSelection** öğesi altında **ClaimsProviderSelections** öğesi:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.

1. Bulma **OrchestrationStep** , `Order="2"` altında **UserJourney** öğesi.
2. Ekleme aşağıdaki **ClaimsExchange** öğesi altında **ClaimsExchanges** öğesi:

    ```xml
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```

## <a name="upload-the-policy-to-your-tenant"></a>Kiracınız için ilkeyi karşıya yükle

1. Azure portalında **tüm ilkeleri**.
2. Seçin **karşıya yükleme İlkesi**.
3. Etkinleştirme **ilke varsa üzerine**.
4. Bulun ve seçin, *TrustFrameworkExtensions.xml* ilke dosyası ve ardından **karşıya**. Doğrulama başarılı olduğundan emin olun.


## <a name="configure-an-adfs-relying-party-trust"></a>Bir AD FS bağlı olan taraf güveni yapılandırın

ADFS, Azure AD B2C'de kimlik sağlayıcısı olarak kullanmak için bir AD FS bağlı olan taraf güveni oluşturma Azure AD B2C SAML meta verileriyle gerekir. Aşağıdaki örnek, bir URL adresi için bir Azure AD B2C'Teknik profili SAML meta verilerini gösterir:

```
https://login.microsoftonline.com/te/your-tenant/your-policy/samlp/metadata?idptp=your-technical-profile
```

Aşağıdaki değerleri değiştirin:

- **Kiracı Your** Kiracı adınızla tenant.onmicrosoft.com bilgisayarınızı gibi.
- **ilke Your** ilke adınızla. İlke sağlayıcısı SAML teknik profili yapılandırdığınız veya bu ilkeden devralan bir ilke kullanın.
- **Teknik profil Your** SAML kimlik sağlayıcısı teknik profilinizin adı ile.
 
Bir tarayıcı açın ve URL'ye gidin. Doğru URL'yi yazın ve XML meta veri dosyası erişiminiz olduğundan emin olun.

AD FS Yönetimi ek bileşenini kullanarak yeni bir bağlı olan taraf güveni eklemek ve ayarları el ile yapılandırmak için federasyon sunucusunda aşağıdaki yordamı gerçekleştirin. Üyelik **Yöneticiler** veya eşdeğer yerel bilgisayarda bu yordamı tamamlamak için gereken en düşük gereksinimdir. Uygun hesapların kullanmayla ilgili ayrıntıları gözden geçirin ve Grup üyeliklerinin [yerel ve etki alanı varsayılan grupları](http://go.microsoft.com/fwlink/?LinkId=83477).

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

    ![Kural özelliklerini ayarlama](./media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)

12.  Sertifika türüne bağlı olarak, KARMA algoritması ayarlamanız gerekebilir. Bağlı olan taraf güveni (B2C Demo) Özellikler penceresinde, seçin **Gelişmiş** sekmesini ve değiştirme **güvenli karma algoritması** için `SHA-1` veya `SHA-256`, tıklatıp **Tamam**.  

### <a name="update-the-relying-party-metadata"></a>Bağlı olan taraf meta verilerini güncelleştir

SAML teknik profili değiştirilmesi güncelleştirilmiş meta veri sürümü ile ADFS güncelleştirmenizi gerektirir. Bağlı taraf uygulaması oluşturduğunuzda, ancak bir değişiklik yaptığınızda meta verileri güncelleştirmek gerekmez, ADFS'de meta verileri güncelleştirin.

1. Sunucu Yöneticisi'nde **Araçları**ve ardından **ADFS Yönetim**.
2. Oluşturduğunuz bağlı taraf güvenini seçin, **Federasyon meta verilerini güncelleştirmesi**ve ardından **güncelleştirme**. 

### <a name="test-the-policy-by-using-run-now"></a>Şimdi Çalıştır kullanarak test İlkesi

1.  Açık **Azure AD B2C ayarlarını** gidin **kimlik deneyimi çerçevesi**.
2.  Açık **B2C_1A_ProfileEdit**, karşıya yüklediğiniz bağlı olan taraf (RP) özel ilke. Seçin **Şimdi Çalıştır**. ADFS hesabınızı kullanarak oturum açabilir olması gerekir.

## <a name="download-the-complete-policy-files"></a>Tüm ilke dosyalarını indirme

İsteğe bağlı: Senaryonuz adımları tamamladıktan sonra kendi özel ilkesi dosyaları kullanarak oluşturabileceğiniz [özel ilkeler ile çalışmaya başlama](active-directory-b2c-get-started-custom.md). Örneğin bkz [yalnızca başvuru için ilke örnek dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app).
