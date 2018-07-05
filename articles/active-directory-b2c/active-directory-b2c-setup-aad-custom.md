---
title: Azure Active Directory B2C'de özel ilkeler kullanarak bir Azure AD Sağlayıcısı Ekle | Microsoft Docs
description: Azure Active Directory B2C özel ilkeleri hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 139658b57cfdce2603d4413b5bf4c9a86b6a8c14
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444336"
---
# <a name="azure-active-directory-b2c-sign-in-by-using-azure-ad-accounts"></a>Azure Active Directory B2C: Azure AD hesapları kullanarak oturum açın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, oturum açma kullanılarak belirli bir Azure Active Directory (Azure AD) kuruluştan kullanıcılar için etkinleştirme işlemini göstermektedir [özel ilkeler](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

Bu adımlar şunlardır:

1. Bir Azure Active Directory B2C oluşturma (Azure AD B2C) Kiracı.
2. Bir Azure AD B2C uygulaması oluşturuluyor.
3. İki ilke altyapısı uygulama kaydediliyor.
4. Anahtarları ayarlama.
5. Başlangıç paketi ayarlama.

## <a name="create-an-azure-ad-app"></a>Bir Azure AD uygulamanızı oluşturma

Belirli kullanıcılar için oturum açma etkinleştirmek için Azure AD kuruluş ihtiyacınız kuruluş içinde bir uygulamayı kaydetmek Azure AD kiracısı.

>[!NOTE]
> "Contoso.com" kuruluş için kullandığımız Azure AD kiracısı ve aşağıdaki yönergeler, Azure AD B2C kiracısı olarak "fabrikamb2c.onmicrosoft.com".

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda, hesabınızı seçin. Gelen **dizin** listesinde, kuruluş seçin (contoso.com) uygulamanızı kaydetmek istediğiniz Azure AD kiracısı.
3. Seçin **diğer hizmetler** sol bölme ve "Uygulama kayıtları" için arama
4. **Yeni uygulama kaydı**’nı seçin.
5. Uygulamanız için bir ad girin (örneğin, `Azure AD B2C App`).
6. Uygulama türü için **Web uygulaması / API** öğesini seçin.
7. İçin **oturum açma URL'si**, aşağıdaki URL'yi girin. burada `yourtenant` Azure AD B2C kiracınızın adı tarafından değiştirilir (`fabrikamb2c.onmicrosoft.com`):

    >[!NOTE]
    >"Yourtenant" değeri, harflerden oluşmalıdır **oturum açma URL'si**.

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

8. Uygulama kimliğini kaydedin
9. Yeni oluşturduğunuz uygulamayı seçin.
10. Altında **ayarları** dikey penceresinde **anahtarları**.
11. Anahtar açıklaması girin, bir süre seçin ve ardından **Kaydet**. Anahtarın değeri görüntülenir. Sonraki bölümde adımlarda kullanacağınız kopyalayın.

## <a name="add-the-azure-ad-key-to-azure-ad-b2c"></a>Azure AD anahtarı Azure AD B2C'ye ekleyin.

Azure AD B2C kiracınızda contoso.com uygulama anahtarını depolamak gerekir. Bunu yapmak için:
1. Azure AD B2C kiracınıza gitmek ve seçin **B2C ayarlarını** > **kimlik deneyimi çerçevesi** > **ilke anahtarları**.
1. Seçin **+ Ekle**.
1. Seçin veya bu seçenekler girin:
   * Seçin **el ile**.
   * İçin **adı**, Azure AD Kiracı adınızın eşleşen bir ad seçin (örneğin, `ContosoAppSecret`).  Önek `B2C_1A_` anahtarınızı adına otomatik olarak eklenir.
   * Uygulama anahtarınızı yapıştırın **gizli** kutusu.
   * Seçin **imza**.
1. **Oluştur**’u seçin.
1. Anahtar oluşturduğunuzu doğrulayın `B2C_1A_ContosoAppSecret`.


## <a name="add-a-claims-provider-in-your-base-policy"></a>Temel, ilkede bir talep Sağlayıcı Ekle

Azure AD kullanarak oturum açmalarını istiyorsanız, bir talep sağlayıcısı olarak Azure AD tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C ile iletişim kuracak bir uç nokta belirtmeniz gerekir. Uç nokta Azure AD B2C tarafından belirli bir kullanıcı yapıldığını doğrulamak için kullanılan bir talepler kümesi sağlar. 

Azure AD'ye ekleyerek bir talep sağlayıcısı olarak Azure AD'ye tanımlayabilirsiniz `<ClaimsProvider>` uzantısının ilkenizin düğümünde:

1. Uzantı dosyası (TrustFrameworkExtensions.xml) çalışma dizininizi açın.
1. Bulma `<ClaimsProviders>` bölümü. Yoksa, kök düğümü ekleyin.
1. Yeni bir `<ClaimsProvider>` gösterildiği gibi düğüm:

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
                    <Item Key="METADATA">https://login.windows.net/contoso.com/.well-known/openid-configuration</Item>
                    <Item Key="ProviderName">https://sts.windows.net/00000000-0000-0000-0000-000000000000/</Item>
                    <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="IdTokenAudience">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="response_types">id_token</Item>
                    <Item Key="UsePolicyInRedirectUri">false</Item>
                </Metadata>
                <CryptographicKeys>
                    <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
                </CryptographicKeys>
                <OutputClaims>
                    <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="oid"/>
                    <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
                    <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
                    <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
                    <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
                    <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="contosoAuthentication" />
                    <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="AzureADContoso" />
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

1. Altında `<ClaimsProvider>` düğümünün değerini güncelleştirin `<Domain>` diğer kimlik sağlayıcılardan ayırmak için kullanılan benzersiz bir değer.
1. Altında `<ClaimsProvider>` düğümünün değerini güncelleştirin `<DisplayName>` için talep sağlayıcısı için bir kolay ad. Bu değer şu anda kullanılmıyor.

### <a name="update-the-technical-profile"></a>Teknik profilini güncelleştir

Azure AD uç noktasından bir belirteç almak için Azure AD B2C, Azure AD ile iletişim kurmak için kullanması gereken protokolleri tanımlamanız gerekir. İçinde yapıldığını `<TechnicalProfile>` öğesinin `<ClaimsProvider>`.
1. Güncelleştirme kimliği `<TechnicalProfile>` düğümü. Bu kimlik, ilkenin diğer bölümlerden Bu teknik profile başvurmak için kullanılır.
1. Değerini güncelleştirin `<DisplayName>`. Bu değer, oturum açma ekranında oturum açın düğmesinde görüntülenir.
1. Değerini güncelleştirin `<Description>`.
1. Azure AD, Openıd Connect protokolünü kullanır, böylece değeri emin `<Protocol>` olduğu `"OpenIdConnect"`.

Güncelleştirmeye gerek duyduğunuz `<Metadata>` XML dosyasının bir bölümünde adlandırılan daha önce özel yapılandırma ayarları yansıtacak şekilde Azure AD kiracısı. Meta veri değerlerini bir XML dosyasında şu şekilde güncelleştirin:

1. Ayarlama `<Item Key="METADATA">` için `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`burada `yourAzureADtenant` , Azure AD Kiracı adı (contoso.com).
1. Tarayıcınızı açın ve gidin `METADATA` güncelleştirdiğiniz URL'si.
1. Tarayıcı için 'Issuer' nesnesi bakın ve değerini kopyalayın. Aşağıdaki gibi görünmelidir: `https://sts.windows.net/{tenantId}/`.
1. Değerini yapıştırın `<Item Key="ProviderName">` XML dosyasında.
1. Ayarlama `<Item Key="client_id">` için uygulama kaydı uygulama kimliği.
1. Ayarlama `<Item Key="IdTokenAudience">` için uygulama kaydı uygulama kimliği.
1. Emin `<Item Key="response_types">` ayarlanır `id_token`.
1. Emin `<Item Key="UsePolicyInRedirectUri">` ayarlanır `false`.

Ayrıca, Azure AD B2C kiracınızı Azure AD'ye kayıtlı Azure AD parola bağlamanız gerekir `<ClaimsProvider>` bilgileri:

* İçinde `<CryptographicKeys>` bölümünde önceki XML dosyasında, değer için güncelleştirme `StorageReferenceId` tanımladığınız gizli başvuru kimliği (örneğin, `ContosoAppSecret`).

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını karşıya yükle

Artık Azure AD B2C, Azure AD dizini ile iletişim kurma bilebilmesi ilkenizi yapılandırdınız. İlkenizin yalnızca bu sorunları şu ana kadar sahip olmadığını onaylamak için uzantı dosyasını karşıya yüklemeyi deneyin. Bunu yapmak için:

1. Açık **tüm ilkeleri** dikey pencerede Azure AD B2C kiracınızı.
1. Denetleme **ilke varsa üzerine** kutusu.
1. Uzantı dosyası (TrustFrameworkExtensions.xml) karşıya yükleme ve doğrulama başlayabildiğinden emin olun.

## <a name="register-the-azure-ad-claims-provider-to-a-user-journey"></a>Kullanıcı yolculuğu Azure AD talep sağlayıcısını Kaydet

Şimdi, Azure AD kimlik sağlayıcısı, kullanıcı yolculuklarından birini eklemeniz gerekir. Bu noktada, kimlik sağlayıcısı ayarlandı, ancak oturumu-kaydolma/oturum açma ekranları hiçbirinde kullanılabilir değil. Kullanılabilir hale getirmek için biz varolan bir şablonu kullanıcı yolculuğu bir kopyasını oluşturun ve ayrıca Azure AD kimlik sağlayıcısına sahip olacak şekilde değiştirmek:

1. Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.
1. Bulma `<UserJourneys>` öğenin ve tüm kopyalama `<UserJourney>` içeren düğüm `Id="SignUpOrSignIn"`.
1. Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve bulun `<UserJourneys>` öğesi. Öğe yoksa bir tane ekleyin.
1. Tüm yapıştırın `<UserJourney>` alt öğesi olarak kopyaladığınız düğüm `<UserJourneys>` öğesi.
1. Yeni kullanıcı yolculuğu kimliği yeniden adlandırın (örneğin, Yeniden Adlandır `SignUpOrSignUsingContoso`).

### <a name="display-the-button"></a>Görüntü "button"

`<ClaimsProviderSelection>` Öğedir oturumu-kaydolma/oturum açma ekranı bir kimlik sağlayıcısının düğmesine benzer. Eklerseniz bir `<ClaimsProviderSelection>` öğesi Azure AD için yeni bir düğme gösterilir sayfasında bir kullanıcı gölünüzdeki olduğunda. Bu öğe eklemek için:

1. Bulma `<OrchestrationStep>` içeren düğüm `Order="1"` oluşturduğunuz kullanıcı yolculuğu içinde.
1. Aşağıdakileri ekleyin:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. Ayarlama `TargetClaimsExchangeId` uygun değeri. Başkalarının aynı kuralı aşağıdaki öneririz:  *\[ClaimProviderName\]Exchange*.

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.

Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için Azure AD ile iletişim kurmak için Azure AD B2C içindir. Düğme, Azure AD talep sağlayıcısı teknik profil bağlayarak bir eyleme bağlantı:

1. Bulma `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
1. Aşağıdakileri ekleyin:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. Güncelleştirme `Id` aynı değere sahip `TargetClaimsExchangeId` önceki bölümde.
1. Güncelleştirme `TechnicalProfileReferenceId` teknik profil Kimliğine önceki (ContosoProfile) oluşturulur.

### <a name="upload-the-updated-extension-file"></a>Güncelleştirilmiş uzantı dosyasını karşıya yükle

İşiniz uzantısının değiştirme. Dosyayı kaydedin. Ardından dosyayı yükleyin ve tüm Doğrulamalar başarıyla emin olun.

### <a name="update-the-rp-file"></a>RP dosyasını güncelleştirme

Artık oluşturduğunuz kullanıcı yolculuğu başlatacak bağlı olan taraf (RP) dosyasını güncelleştirmeniz gerekir:

1. Çalışma dizininizde bir kopyasını SignUpOrSignIn.xml olun ve yeniden adlandırın (örneğin, bu SignUpOrSignInWithAAD.xml için Yeniden Adlandır).
1. Yeni bir dosya açıp güncelleştirme `PolicyId` özniteliğini `<TrustFrameworkPolicy>` benzersiz bir değerle (örneğin, SignUpOrSignInWithAAD). <br> Bu ilke adı olacaktır (örneğin, B2C\_1A\_SignUpOrSignInWithAAD).
1. Değiştirme `ReferenceId` özniteliğini `<DefaultUserJourney>` (SignUpOrSignUsingContoso) oluşturduğunuz yeni kullanıcı yolculuğu kimliği eşleştirmek için.
1. Yaptığınız değişiklikleri kaydedin ve dosyayı karşıya yükleyin.

## <a name="troubleshooting"></a>Sorun giderme

Test dikey penceresini açıp tıklatarak yüklediğiniz özel İlkesi **Şimdi Çalıştır**. Sorunları tanılamak için hakkında bilgi edinin: [sorun giderme](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Sonraki adımlar

Geri bildirim sağlamak [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
