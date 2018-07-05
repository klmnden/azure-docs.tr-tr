---
title: Azure Active Directory B2C'de özel ilkeleri kullanarak çok kiracılı Azure AD kimlik sağlayıcısı Ekle | Microsoft Docs
description: Özel ilkeleri - Azure Active Directory B2C kullanarak bir çok kiracılı Azure AD kimlik sağlayıcısı ekleyin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/14/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 2a8a23245a17c9a80c70860588a8312dbbb5e926
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446102"
---
# <a name="azure-active-directory-b2c-allow-users-to-sign-in-to-a-multi-tenant-azure-ad-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: özel ilkeleri kullanarak çok kiracılı Azure AD kimlik sağlayıcısı için oturum açmasına izin ver

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, oturum açma kullanılarak Azure Active Directory (Azure AD) çok Kiracı uç noktası kullanan kullanıcılar için etkinleştirme işlemini göstermektedir [özel ilkeler](active-directory-b2c-overview-custom.md). Bu, kullanıcıların Azure AD B2C'ye her Kiracı için teknik bir sağlayıcı yapılandırması olmadan oturum açmak için birden çok Azure AD kiracısından sağlar. Ancak, bu kiracının tüm üyeleri Konuk **yapmamayı** oturum açmak kullanabilirsiniz. Bunun için gerekecek [tek tek her Kiracı yapılandırma](active-directory-b2c-setup-aad-custom.md).

>[!NOTE]
> "Contoso.com" kuruluş için kullandığımız Azure AD kiracısı ve aşağıdaki yönergeler, Azure AD B2C kiracısı olarak "fabrikamb2c.onmicrosoft.com".

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

Bu adımlar şunlardır:
     
1. Bir Azure Active Directory B2C oluşturma (Azure AD B2C) Kiracı.
1. Bir Azure AD B2C uygulaması oluşturuluyor.    
1. İki ilke altyapısı uygulama kaydediliyor.  
1. Anahtarları ayarlama. 
1. Başlangıç paketi ayarlama.

## <a name="step-1-create-a-multi-tenant-azure-ad-app"></a>1. Adım Çok kiracılı Azure AD uygulaması oluşturma

Çok kiracılı Azure AD uç noktası kullanan kullanıcılar için oturum açma etkinleştirmek için çok kiracılı bir uygulama, Azure AD kiracılarıyla birinde kayıtlı olması gerekir. Bu makalede, Azure AD B2C kiracınızda Azure AD çok kiracılı bir uygulama oluşturulacağını göstereceğiz. Oturum açma, çok kiracılı bir Azure AD uygulaması genelindeki kullanıcılar için daha sonra etkinleştirin.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst çubuğunda, hesabınızı seçin. Gelen **dizin** listesinde, Azure AD uygulaması (fabrikamb2c.onmicrosoft.com) kaydetmek için Azure AD B2C kiracınızı seçin.
1. Seçin **diğer hizmetler** sol bölme ve "Uygulama kayıtları" için arama
1. **Yeni uygulama kaydı**’nı seçin.
1. Uygulamanız için bir ad girin (örneğin, `Azure AD B2C App`).
1. Uygulama türü için **Web uygulaması / API** öğesini seçin.
1. İçin **oturum açma URL'si**, aşağıdaki URL'yi girin. burada `yourtenant` Azure AD B2C kiracınızın adı tarafından değiştirilir (`fabrikamb2c.onmicrosoft.com`):

    >[!NOTE]
    >"Yourtenant" değeri, harflerden oluşmalıdır **oturum açma URL'si**.

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Uygulama kimliğini kaydedin
1. Yeni oluşturduğunuz uygulamayı seçin.
1. Altında **ayarları** dikey penceresinde **özellikleri**.
1. Ayarlama **çok kiracılı** için **Evet**.
1. Altında **ayarları** dikey penceresinde **anahtarları**.
1. Yeni bir anahtar oluşturun ve kaydedin. Sonraki bölümde yer alan adımları olarak kullanır.

## <a name="step-2-add-the-azure-ad-key-to-azure-ad-b2c"></a>2. Adım Azure AD anahtarı Azure AD B2C'ye ekleyin.

Azure AD B2C ayarlarında uygulama anahtarı kayıt olmanız gerekir. Bunu yapmak için:

1. Azure AD B2C ayarları menüsüne gidin
1. Tıklayarak **kimlik deneyimi çerçevesi** > **ilke anahtarları**.
1. Seçin **+ Ekle**.
1. Seçin veya bu seçenekler girin:
   * Seçin **el ile**.
   * İçin **adı**, Azure AD Kiracı adınızın eşleşen bir ad seçin (örneğin, `AADAppSecret`).  Önek `B2C_1A_` anahtarınızı adına otomatik olarak eklenir.
   * Uygulama anahtarınızı yapıştırın **gizli** kutusu.
   * Seçin **imza**.
1. **Oluştur**’u seçin.
1. Anahtar oluşturduğunuzu doğrulayın `B2C_1A_AADAppSecret`.

## <a name="step-3-add-a-claims-provider-in-your-base-policy"></a>3. Adım Temel, ilkede bir talep Sağlayıcı Ekle

Azure AD kullanarak oturum açmalarını istiyorsanız, bir talep sağlayıcısı olarak Azure AD tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C ile iletişim kuracak bir uç nokta belirtmeniz gerekir. Uç nokta Azure AD B2C tarafından belirli bir kullanıcı yapıldığını doğrulamak için kullanılan bir talepler kümesi sağlar. 

Azure AD'ye ekleyerek bir talep sağlayıcısı olarak Azure AD'ye tanımlayabilirsiniz `<ClaimsProvider>` uzantısının ilkenizin düğümünde:

1. Uzantı dosyası (TrustFrameworkExtensions.xml) çalışma dizininizi açın.
1. Bulma `<ClaimsProviders>` bölümü. Yoksa, kök düğümü ekleyin.
1. Yeni bir `<ClaimsProvider>` gösterildiği gibi düğüm:

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

1. Altında `<ClaimsProvider>` düğümünün değerini güncelleştirin `<Domain>` diğer kimlik sağlayıcılardan ayırmak için kullanılan benzersiz bir değer.
1. Altında `<TechnicalProfile>` düğümünün değerini güncelleştirin `<DisplayName>`. Bu değer, oturum açma ekranında oturum açın düğmesinde görüntülenir.
1. Değerini güncelleştirin `<Description>`.
1. Ayarlama `<Item Key="client_id">` için Azure AD Kiracı mulity uygulama kaydı uygulama kimliği.

### <a name="step-31-restrict-access-to-a-specific-list-of-azure-ad-tenants"></a>Adım 3.1 belirli bir Azure AD kiracılarıyla listesi erişimi kısıtlama

> [!NOTE]
> Kullanarak `https://sts.windows.net` değeri olarak **ValidTokenIssuerPrefixes** tüm Azure AD kullanıcılarının uygulamanızda oturum açmak izin verir.

Geçerli bir belirteç verenler listesini güncelleştirmek ve belirli kullanıcıların oturum açma Azure AD kiracılarıyla listesine erişimi gerekir. Değerlerini almak için meta veriler her özel bir ara gerekecektir gelen kullanıcıların istediğiniz Azure AD kiracılarıyla oturum açın. Veri biçimi aşağıdaki gibi görünür: `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`burada `yourAzureADtenant` , Azure AD Kiracı adı (contoso.com veya diğer Azure AD kiracısı).
1. Tarayıcınızı açın ve meta verileri URL'sine gidin.
1. Tarayıcı için 'Issuer' nesnesi bakın ve değerini kopyalayın. Aşağıdaki gibi görünmelidir: `https://sts.windows.net/{tenantId}/`.
1. Değerini yapıştırın `ValidTokenIssuerPrefixes` anahtarı. Bunları virgülle ayırarak birden fazla ekleyebilirsiniz. Bunun bir örneği XML yukarıdaki örnekte bırakılmıştır.

## <a name="step-4-register-the-azure-ad-account-claims-provider"></a>4. Adım. Azure AD hesabı talep sağlayıcısını Kaydet

### <a name="step-41-make-a-copy-of-the-user-journey"></a>Kullanıcı yolculuğu bir kopyasını adım 4.1 olun

Şimdi, Azure AD kimlik sağlayıcısı, kullanıcı yolculuklarından birini eklemeniz gerekir. Bu noktada, kimlik sağlayıcısı ayarlandı, ancak oturumu-kaydolma/oturum açma ekranları hiçbirinde kullanılabilir değil.

Kullanılabilir hale getirmek için biz varolan bir şablonu kullanıcı yolculuğu bir kopyasını oluşturun ve ayrıca Azure AD kimlik sağlayıcısına sahip olacak şekilde değiştirmek:

1. Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.
1. Bulma `<UserJourneys>` öğenin ve tüm kopyalama `<UserJourney>` içeren düğüm `Id="SignUpOrSignIn"`.
1. Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve bulun `<UserJourneys>` öğesi. Öğe yoksa bir tane ekleyin.
1. Tüm yapıştırın `<UserJourney>` alt öğesi olarak kopyaladığınız düğüm `<UserJourneys>` öğesi.
1. Yeni kullanıcı yolculuğu kimliği yeniden adlandırın (örneğin, Yeniden Adlandır `SignUpOrSignUsingAzureAD`). 

### <a name="step-42-display-the-button"></a>"Button" Adım 4.2 görüntüleme

`<ClaimsProviderSelection>` Öğedir oturumu-kaydolma/oturum açma ekranı bir kimlik sağlayıcısının düğmesine benzer. Eklerseniz bir `<ClaimsProviderSelection>` öğesi Azure AD için yeni bir düğme gösterilir sayfasında bir kullanıcı gölünüzdeki olduğunda. Bu öğe eklemek için:

1. Bulma `<OrchestrationStep>` içeren düğüm `Order="1"` , oluşturduğunuz kullanıcı giden.
1. Aşağıdakileri ekleyin:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="AzureADExchange" />
    ```

1. Ayarlama `TargetClaimsExchangeId` uygun değeri. Başkalarının aynı kuralı aşağıdaki öneririz:  *\[ClaimProviderName\]Exchange*.

### <a name="step-43-link-the-button-to-an-action"></a>Adım 4.3 bağlantı bir eyleme düğme

Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için Azure AD ile iletişim kurmak için Azure AD B2C içindir. Düğme, Azure AD talep sağlayıcısı teknik profil bağlayarak bir eyleme bağlantı:

1. Bulma `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
1. Aşağıdakileri ekleyin:

    ```XML
    <ClaimsExchange Id="AzureADExchange" TechnicalProfileReferenceId="Common-AAD" />
    ```

1. Güncelleştirme `Id` aynı değere sahip `TargetClaimsExchangeId` önceki bölümde.
1. Güncelleştirme `TechnicalProfileReferenceId` teknik profil Kimliğine önceki (ortak-AAD) oluşturulur.

## <a name="step-5-create-a-new-rp-policy"></a>5. adım: yeni bir RP ilkesi oluşturma

Artık oluşturduğunuz kullanıcı yolculuğu başlatacak bağlı olan taraf (RP) dosyasını güncelleştirmeniz gerekir:
 
1. Çalışma dizininizde bir kopyasını SignUpOrSignIn.xml olun ve yeniden adlandırın (örneğin, bu SignUpOrSignInWithAAD.xml için Yeniden Adlandır).  
1. Yeni bir dosya açıp güncelleştirme `PolicyId` özniteliğini `<TrustFrameworkPolicy>` benzersiz bir değerle (örneğin, SignUpOrSignInWithAAD). Bu ilke adı olacaktır (örneğin, B2C\_1A\_SignUpOrSignInWithAAD). 
1. Değiştirme `ReferenceId` özniteliğini `<DefaultUserJourney>` (SignUpOrSignUsingAzureAD) oluşturduğunuz yeni kullanıcı yolculuğu kimliği eşleştirmek için. 
1. Yaptığınız değişiklikleri kaydedin ve dosyayı karşıya yükleyin. 

## <a name="step-6-upload-the-policy-to-your-tenant"></a>6. adım: ilke kiracınıza karşıya yükleyin.

1. İçinde [Azure portalında](https://portal.azure.com), geçiş [Azure AD B2C kiracınızın bağlamında](active-directory-b2c-navigate-to-b2c-context.md)ve ardından **Azure AD B2C**.
1. Seçin **kimlik deneyimi çerçevesi**.
1. Seçin **tüm ilkeleri**.
1. Seçin **karşıya yükleme İlkesi**.
1. Seçin **ilke varsa üzerine** onay kutusu.
1. Karşıya yükleme `TrustFrameworkExtensions.xml` dosyasını ve RP dosyasını (örneğin `SignUpOrSignInWithAAD.xml`) ve bunların doğrulama geçirmek emin olun.

## <a name="step-7-test-the-custom-policy-by-using-run-now"></a>7. adım: Test Şimdi Çalıştır kullanarak özel ilke

1. Seçin **Azure AD B2C ayarlarını**ve ardından **kimlik deneyimi çerçevesi**.
    > [!NOTE]
    > Çalıştırma artık Kiracı'da önceden kayıtlı için en az bir uygulama gerektirir. Azure AD B2C uygulamaları kaydetme hakkında bilgi için bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makalesi.

1. Karşıya yüklediğiniz bağlı olan taraf (RP) özel bir ilkeyi açın (*B2C\_1A\_SignUpOrSignInWithAAD*) ve ardından **Şimdi Çalıştır**.
1. Şimdi Azure AD hesabını kullanarak oturum açabilir olmalıdır.

## <a name="optional-step-8-register-the-azure-ad-account-claims-provider-to-the-profile-edit-user-journey"></a>(İsteğe bağlı) 8. adım: Azure AD hesabı talep sağlayıcısı profil düzenleme kullanıcı yolculuğu için kaydolun

Azure AD hesabı kimlik sağlayıcısına eklemek isteyebilirsiniz, `ProfileEdit` kullanıcı yolculuğu. Kullanıcı yolculuğu kullanılabilir hale getirmek için 6. adımları 4 yineleyin. Bu kez, select `<UserJourney>` içeren düğüm `Id="ProfileEdit"`. Kaydetme, karşıya yükleme ve ilkeniz test.

## <a name="troubleshooting"></a>Sorun giderme

Sorunları tanılamak için hakkında bilgi edinin: [sorun giderme](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Sonraki adımlar

Geri bildirim sağlamak [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
