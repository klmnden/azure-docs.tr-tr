---
title: Azure Active Directory B2C'de özel ilkeler kullanarak bir Azure AD Sağlayıcı eklemek | Microsoft Docs
description: Azure Active Directory B2C özel ilkeleri hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 9d010564aadcb6ea33312b7fb40854cfd0a97f1a
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "34709249"
---
# <a name="azure-active-directory-b2c-sign-in-by-using-azure-ad-accounts"></a>Azure Active Directory B2C: Azure AD hesapları kullanarak oturum açın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, oturum açma kullanım yoluyla belirli bir Azure Active Directory (Azure AD) kuruluştan kullanıcılar için etkinleştirme gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [özel ilkeleri ile çalışmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

Bu adımlar şunları içerir:

1. Bir Azure Active Directory B2C oluşturma (Azure AD B2C) Kiracı.
2. Bir Azure AD B2C uygulaması oluşturuluyor.
3. İki ilke altyapısı uygulama kaydediliyor.
4. Anahtarları ayarlama.
5. Başlangıç paketi ayarlama.

## <a name="create-an-azure-ad-app"></a>Bir Azure AD uygulaması oluştur

Oturum açma için belirli bir kullanıcılardan etkinleştirmek için Azure AD kuruluş ihtiyacınız kuruluş dahilinde bir uygulamayı kaydetmek Azure AD kiracısı.

>[!NOTE]
> "Contoso.com" kuruluş için kullandığımız Azure AD kiracısı ve Azure AD B2C kiracısı'nda aşağıdaki yönergeleri olarak "fabrikamb2c.onmicrosoft.com".

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda hesabınızı seçin. Gelen **Directory** listesinde, kuruluş seçin, uygulamanız (contoso.com) kaydetmek istediğiniz Azure AD kiracısı.
3. Seçin **daha fazla hizmet** sol bölmesinde ve "Uygulama kayıtlar" için arama
4. **Yeni uygulama kaydı**’nı seçin.
5. Uygulamanız için bir ad girin (örneğin, `Azure AD B2C App`).
6. Uygulama türü için **Web uygulaması / API** öğesini seçin.
7. İçin **oturum açma URL'si**, aşağıdaki URL'yi girin nerede `yourtenant` Azure AD B2C kiracınızın adıyla değiştirilen (`fabrikamb2c.onmicrosoft.com`):

    >[!NOTE]
    >"Yourtenant" değeri, tüm küçük harfli olması gerektiğini **oturum açma URL'si**.

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

8. Uygulama Kimliği kaydedilemiyor
9. Yeni oluşturulan uygulaması'nı seçin.
10. Altında **ayarları** dikey penceresinde, select **anahtarları**.
11. Anahtar açıklama girin, bir süre seçin ve ardından **kaydetmek**. Anahtar değeri görüntülenir. Sonraki bölümde yer alan adımları kullanacağınız için kopyalayın.

## <a name="add-the-azure-ad-key-to-azure-ad-b2c"></a>Azure AD B2C için Azure AD anahtarı Ekle

Azure AD B2C kiracınızda contoso.com uygulama anahtarı depolamanız gerekir. Bunu yapmak için:
1. Azure AD B2C kiracınızın gidin ve seçin **B2C ayarlarını** > **kimlik deneyimi Framework** > **İlkesi anahtarları**.
1. Seçin **+ Ekle**.
1. Bu seçenekler girin veya seçin:
   * Seçin **el ile**.
   * İçin **adı**, Azure AD Kiracı adı ile eşleşen bir ad seçin (örneğin, `ContosoAppSecret`).  Önek `B2C_1A_` anahtarınızı adına otomatik olarak eklenir.
   * Uygulama anahtarınızı Yapıştır **gizli** kutusu.
   * Seçin **imza**.
1. **Oluştur**’u seçin.
1. Anahtar oluşturduğunuz onaylayın `B2C_1A_ContosoAppSecret`.


## <a name="add-a-claims-provider-in-your-base-policy"></a>Bir talep sağlayıcı temel ilkenizde ekleme

Azure AD kullanarak oturum açmalarını istiyorsanız, bir talep sağlayıcısı olarak Azure AD tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C ile iletişim kuracak bir uç nokta belirtmeniz gerekir. Uç nokta Azure AD B2C tarafından belirli bir kullanıcı kimliği doğrulanmış olduğunu doğrulamak için kullanılan talep kümesini sağlar. 

Azure AD ile ekleyerek bir talep sağlayıcısı olarak Azure AD tanımlayabilirsiniz `<ClaimsProvider>` ilkenizin uzantısı dosyasında düğümü:

1. Uzantı dosyası (TrustFrameworkExtensions.xml) çalışma dizininizi açın.
1. Bul `<ClaimsProviders>` bölümü. Yoksa, kök düğümü ekleyin.
1. Yeni bir ekleme `<ClaimsProvider>` şekilde düğümü:

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

1. Altında `<ClaimsProvider>` düğümü için değeri güncelleştirin `<Domain>` diğer kimlik sağlayıcılardan ayırt etmek için kullanılan benzersiz bir değer.
1. Altında `<ClaimsProvider>` düğümü için değeri güncelleştirin `<DisplayName>` için talep sağlayıcısı için bir kolay ad. Bu değer şu anda kullanılmıyor.

### <a name="update-the-technical-profile"></a>Teknik profilini güncelleştir

Azure AD uç noktasından bir belirteç almak için Azure AD B2C Azure AD ile iletişim kurmak için kullanması gereken protokolleri tanımlamanız gerekir. Bu içinde yapılır `<TechnicalProfile>` öğesinin `<ClaimsProvider>`.
1. Kimliği güncelleştirme `<TechnicalProfile>` düğümü. Bu kimliği, bu teknik profili ilkesinin diğer bölümlerden başvurmak için kullanılır.
1. Değeri güncelleştirme `<DisplayName>`. Bu değer, oturum açma düğmesine oturum açma ekranında görüntülenir.
1. Değeri güncelleştirme `<Description>`.
1. Azure AD Openıd Connect protokolünü kullanır, böylece değeri emin olun `<Protocol>` olan `"OpenIdConnect"`.

Güncelleştirmek gereken `<Metadata>` XML dosyasını bölümünde başvurulan için daha önce özel yapılandırma ayarları yansıtacak şekilde Azure AD kiracısı. XML dosyasında meta veri değerleri şu şekilde güncelleştirin:

1. Ayarlama `<Item Key="METADATA">` için `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, burada `yourAzureADtenant` Azure AD Kiracı adınız (contoso.com).
1. Tarayıcınızı açın ve gidin `METADATA` güncelleştirdiğiniz URL.
1. Tarayıcıda 'dağıtımcı' nesnesi için konum ve değerini kopyalayın. Aşağıdaki gibi görünmelidir: `https://sts.windows.net/{tenantId}/`.
1. Değeri yapıştırın `<Item Key="ProviderName">` XML dosyasında.
1. Ayarlama `<Item Key="client_id">` için uygulama kaydı uygulama kimliği.
1. Ayarlama `<Item Key="IdTokenAudience">` için uygulama kaydı uygulama kimliği.
1. Emin `<Item Key="response_types">` ayarlanır `id_token`.
1. Emin `<Item Key="UsePolicyInRedirectUri">` ayarlanır `false`.

Ayrıca Azure ad, Azure AD B2C kiracınızın kayıtlı Azure AD parola bağlamanız gerekir `<ClaimsProvider>` bilgi:

* İçinde `<CryptographicKeys>` bölümünde önceki XML dosyasında, değeri güncelleştirme `StorageReferenceId` tanımladığınız gizli başvuru kimliği (örneğin, `ContosoAppSecret`).

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını karşıya yükle

Artık, böylece Azure AD B2C, Azure AD dizini ile iletişim kurmak bildiği ilkeniz yapılandırdınız. Yalnızca bu sorunları kadarki olmadığını onaylamak için ilkenizin uzantısı dosyayı karşıya yüklemeyi deneyin. Bunu yapmak için:

1. Açık **tüm ilkeler** dikey penceresinde, Azure AD B2C kiracısı.
1. Denetleme **varsa ilkesi üzerine** kutusu.
1. Uzantı dosyası (TrustFrameworkExtensions.xml) karşıya yükleyin ve doğrulama başlayabildiğinden emin olun.

## <a name="register-the-azure-ad-claims-provider-to-a-user-journey"></a>Bir kullanıcı gezisine Azure AD talep sağlayıcısını Kaydet

Artık Azure AD kimlik sağlayıcısı, kullanıcı Yolculuklar birine eklemeniz gerekir. Bu noktada, kimlik sağlayıcısı ayarlandı, ancak oturumu-up/oturum açma ekranları hiçbirinde kullanılabilir değil. Kullanılabilir hale getirmek için var olan bir şablonu kullanıcı gezisine tekrarı oluşturur ve ardından değiştirin ayrıca Azure AD kimlik sağlayıcısı sahiptir:

1. Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.
1. Bul `<UserJourneys>` öğesi ve kopyalama tüm `<UserJourney>` içeren düğüm `Id="SignUpOrSignIn"`.
1. Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve Bul `<UserJourneys>` öğesi. Öğe yoksa, ekleyin.
1. Tüm Yapıştır `<UserJourney>` bir alt öğesi olarak kopyaladığınız düğümü `<UserJourneys>` öğesi.
1. Yeni kullanıcı gezisine Kimliğini yeniden adlandırın (örneğin, olarak yeniden adlandırın `SignUpOrSignUsingContoso`).

### <a name="display-the-button"></a>Görüntü "button"

`<ClaimsProviderSelection>` Öğesidir bir oturumu-up/oturum açma ekranında bir kimlik sağlayıcısı düğmesini benzer. Eklerseniz bir `<ClaimsProviderSelection>` öğesi Azure AD için yeni bir düğme görüntülenir sayfasında bir kullanıcı adlandırıldığını olduğunda. Bu öğe eklemek için:

1. Bul `<OrchestrationStep>` içeren düğüm `Order="1"` yeni oluşturduğunuz kullanıcı gezisine içinde.
1. Aşağıdakileri ekleyin:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. Ayarlama `TargetClaimsExchangeId` uygun bir değere. Aynısına başkalarının aşağıdaki öneririz:  *\[ClaimProviderName\]Exchange*.

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem bağlantı kurun

Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için Azure AD ile iletişim kurmak Azure AD B2C içindir. Düğme, Azure AD talep sağlayıcısı teknik profili bağlayarak bir eyleme bağlantı:

1. Bul `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
1. Aşağıdakileri ekleyin:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. Güncelleştirme `Id` ile aynı değere sahip `TargetClaimsExchangeId` önceki bölümde.
1. Güncelleştirme `TechnicalProfileReferenceId` teknik profili kimliği için oluşturduğunuz önceki (ContosoProfile).

### <a name="upload-the-updated-extension-file"></a>Güncelleştirilmiş uzantı dosyasını karşıya yükle

Tamamladığınızda uzantısının değiştirme. Dosyayı kaydedin. Ardından, dosyayı karşıya yüklemeyi ve tüm doğrulamaları başarılı olduğundan emin olun.

### <a name="update-the-rp-file"></a>RP dosyasını güncelleştirme

Şimdi yeni oluşturduğunuz kullanıcı gezisine başlatacaktır bağlı olan taraf (RP) dosyasını güncelleştirmeniz gerekir:

1. Çalışma dizininizi SignUpOrSignIn.xml bir kopyasını oluşturun ve yeniden adlandırın (örneğin, SignUpOrSignInWithAAD.xml için yeniden adlandırmadan).
1. Yeni dosya ve güncelleştirme açmak `PolicyId` için öznitelik `<TrustFrameworkPolicy>` benzersiz bir değerle (örneğin, SignUpOrSignInWithAAD). <br> Bu, ilkenin adı olacaktır (örneğin, B2C\_1A\_SignUpOrSignInWithAAD).
1. Değiştirme `ReferenceId` özniteliğini `<DefaultUserJourney>` (SignUpOrSignUsingContoso) oluşturulan yeni kullanıcı gezisine Kimliğini eşleşecek şekilde.
1. Yaptığınız değişiklikleri kaydedin ve dosyayı karşıya yükleyin.

## <a name="troubleshooting"></a>Sorun giderme

Yalnızca kendi dikey penceresini açıp tıklatarak karşıya özel ilkesini test **Şimdi Çalıştır**. İlgili sorunları tanılamak için okuma [sorun giderme](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Sonraki adımlar

Geri bildirim sağlayın [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
