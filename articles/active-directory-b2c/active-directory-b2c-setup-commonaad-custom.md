---
title: Özel ilkeler - Azure Active Directory B2C kullanarak çok kiracılı Azure AD kimlik sağlayıcısı ekleyin | Microsoft Docs
description: Özel ilkeler - Azure Active Directory B2C kullanarak çok kiracılı Azure AD kimlik sağlayıcısı ekleyin
services: active-directory-b2c
documentationcenter: ''
author: parakhj
manager: alexsi
editor: parakhj
ms.assetid: 33c64001-5261-4ed9-8f46-b09839165250
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/14/2018
ms.author: parakhj
ms.openlocfilehash: cff5c1eed374683ad3e2c1f1a69f6f172f36c536
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="azure-active-directory-b2c-allow-users-to-sign-in-to-a-multi-tenant-azure-ad-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: özel ilkelerini kullanarak çok kiracılı Azure AD kimlik sağlayıcısı için oturum açmak kullanıcılara izin ver

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, oturum açma kullanılarak Azure Active Directory (Azure AD) için ortak bir uç kullanan kullanıcılar etkinleştirme gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md).

>[!NOTE]
> "Contoso.com" kuruluş için kullandığımız Azure AD kiracısı ve Azure AD B2C kiracısı'nda aşağıdaki yönergeleri olarak "fabrikamb2c.onmicrosoft.com".

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [özel ilkeleri ile çalışmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

Bu adımlar şunları içerir:
     
1. Bir Azure Active Directory B2C oluşturma (Azure AD B2C) Kiracı.
2. Bir Azure AD B2C uygulaması oluşturuluyor.    
3. İki ilke altyapısı uygulama kaydediliyor.  
4. Anahtarları ayarlama. 
5. Başlangıç paketi ayarlama.

## <a name="step-1-create-a-multi-tenant-azure-ad-app"></a>1. Adım Çok kiracılı Azure AD uygulaması oluştur

Oturum açma çok kiracılı Azure AD uç noktası kullanan kullanıcılar etkinleştirmek için Azure AD kiracılarıyla hiçbirinde kayıtlı bir çok kiracılı uygulamanızın olması gerekir. Bu makalede, Azure AD B2C kiracınızda bir çok kiracılı Azure AD uygulaması oluşturmak nasıl göstereceğiz. Oturum açma, çok kiracılı Azure AD uygulaması kullanımı ile kullanıcılar için daha sonra etkinleştirin.

>[!NOTE]
> Azure AD kullanıcılarının isterseniz **ve Microsoft hesabı olan kullanıcılar** oturum açmak için bu bölüm atlayın ve bunun yerine bir uygulamada kaydetmek [Microsoft Geliştirici Portalı](https://apps.dev.microsoft.com).

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst çubuğunda hesabınızı seçin. Gelen **Directory** listesinde, Azure AD uygulaması (fabrikamb2c.onmicrosoft.com) kaydetmek için Azure AD B2C kiracısı seçin.
2. Seçin **daha fazla hizmet** sol bölmesinde ve "Uygulama kayıtlar" için arama
3. **Yeni uygulama kaydı**’nı seçin.
4. Uygulamanız için bir ad girin (örneğin, `Azure AD B2C App`).
5. Uygulama türü için **Web uygulaması / API** öğesini seçin.
6. İçin **oturum açma URL'si**, aşağıdaki URL'yi girin nerede `yourtenant` Azure AD B2C kiracınızın adıyla değiştirilen (`fabrikamb2c.onmicrosoft.com`):

    >[!NOTE]
    >"Yourtenant" değeri, tüm küçük harfli olması gerektiğini **oturum açma URL'si**.

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Uygulama Kimliği kaydedilemiyor
1. Yeni oluşturulan uygulaması'nı seçin.
1. Altında **ayarları** dikey penceresinde, select **özellikleri**.
1. Ayarlama **çoklu kiralanan** için **Evet**.
1. Altında **ayarları** dikey penceresinde, select **anahtarları**.
1. Yeni bir anahtar oluşturun ve kaydedin. Sonraki bölümde yer alan adımları de kullanır.

## <a name="step-2-add-the-azure-ad-key-to-azure-ad-b2c"></a>2. Adım Azure AD B2C için Azure AD anahtarı Ekle

Azure AD B2C ayarlarında uygulama anahtarı kaydetmeniz gerekir. Bunu yapmak için:

1. Azure AD B2C için ayarları menüsüne gidin
1. Tıklayın **kimlik deneyimi Framework** > **İlkesi anahtarları**.
1. Seçin **+ Ekle**.
1. Bu seçenekler girin veya seçin:
   * Seçin **el ile**.
   * İçin **adı**, Azure AD Kiracı adı ile eşleşen bir ad seçin (örneğin, `AADAppSecret`).  Önek `B2C_1A_` anahtarınızı adına otomatik olarak eklenir.
   * Uygulama anahtarınızı Yapıştır **gizli** kutusu.
   * Seçin **imza**.
5. **Oluştur**’u seçin.
6. Anahtar oluşturduğunuz onaylayın `B2C_1A_AADAppSecret`.

## <a name="step-3-add-a-claims-provider-in-your-base-policy"></a>3. Adım Bir talep sağlayıcı temel ilkenizde ekleme

Azure AD kullanarak oturum açmalarını istiyorsanız, bir talep sağlayıcısı olarak Azure AD tanımlamanız gerekir. Diğer bir deyişle, Azure AD B2C ile iletişim kuracak bir uç nokta belirtmeniz gerekir. Uç nokta Azure AD B2C tarafından belirli bir kullanıcı kimliği doğrulanmış olduğunu doğrulamak için kullanılan talep kümesini sağlar. 

Azure AD ile ekleyerek bir talep sağlayıcısı olarak Azure AD tanımlayabilirsiniz `<ClaimsProvider>` ilkenizin uzantısı dosyasında düğümü:

1. Uzantı dosyası (TrustFrameworkExtensions.xml) çalışma dizininizi açın.
1. Bul `<ClaimsProviders>` bölümü. Yoksa, kök düğümü ekleyin.
1. Yeni bir ekleme `<ClaimsProvider>` şekilde düğümü:

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
        
        <!-- The key below allows you to specify each of the Azure AD tenants that can be used to sign in. If you would like only specific tenants to be able to sign in, uncomment the line below and update the GUIDs. -->
        <!-- <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/00000000-0000-0000-0000-000000000000,https://sts.windows.net/11111111-1111-1111-1111-111111111111</Item> -->

        <!-- The commented key below specifies that users from any tenant can sign-in. Comment or remove the line below if using the line above. -->
        <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/</Item>
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

1. Altında `<ClaimsProvider>` düğümü için değeri güncelleştirin `<Domain>` diğer kimlik sağlayıcılardan ayırt etmek için kullanılan benzersiz bir değer.
1. Altında `<TechnicalProfile>` düğümü için değeri güncelleştirin `<DisplayName>`. Bu değer, oturum açma düğmesine oturum açma ekranında görüntülenir.
1. Değeri güncelleştirme `<Description>`.
1. Ayarlama `<Item Key="client_id">` için Azure AD mulity Kiracı uygulama kaydı uygulama kimliği.

### <a name="step-31-optional-restrict-access-to-specific-list-of-azure-ad-tenants"></a>Azure AD kiracılarıyla belirli listesi 3.1 erişimi [isteğe bağlı] kısıtlama adım
Geçerli belirteç verenler listesini güncelleştirmek ve belirli kullanıcılar oturum açma Azure AD kiracılarıyla listesine erişimi sınırlamak isteyebilirsiniz. Değerleri almak için her özel meta verileri oluşturmayı inceler gerekecektir gelen kullanıcınız istediğiniz Azure AD kiracılarıyla oturum açın. Veri biçimi şu şekilde görünür: `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, burada `yourAzureADtenant` , Azure AD Kiracı adınız (contoso.com veya diğer Azure AD kiracısı).
1. Tarayıcınızı açın ve meta veri URL'sine gidin.
1. Tarayıcıda 'dağıtımcı' nesnesi için konum ve değerini kopyalayın. Aşağıdaki gibi görünmelidir: `https://sts.windows.net/{tenantId}/`.
1. Değeri yapıştırın `ValidTokenIssuerPrefixes` anahtarı. Virgül kullanarak aralarına tarafından birden çok ekleyebilirsiniz. Bunun bir örneğini XML yukarıdaki örnekte bırakılmıştır.

> [!NOTE]
> Kullanarak `https://sts.windows.net` bir önek değeri uygulamanıza oturum tüm Azure AD kullanıcılarının izin.

## <a name="step-4-register-the-azure-ad-account-claims-provider"></a>4. Adım. Azure AD hesabı talep sağlayıcısını Kaydet

### <a name="step-41-make-a-copy-of-the-user-journey"></a>Adım 4.1 kullanıcı gezisine bir kopyasını olun

Artık Azure AD kimlik sağlayıcısı, kullanıcı Yolculuklar birine eklemeniz gerekir. Bu noktada, kimlik sağlayıcısı ayarlandı, ancak oturumu-up/oturum açma ekranları hiçbirinde kullanılabilir değil.

Kullanılabilir hale getirmek için var olan bir şablonu kullanıcı gezisine tekrarı oluşturur ve ardından değiştirin ayrıca Azure AD kimlik sağlayıcısı sahiptir:

1. Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.
1. Bul `<UserJourneys>` öğesi ve kopyalama tüm `<UserJourney>` içeren düğüm `Id="SignUpOrSignIn"`.
1. Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve Bul `<UserJourneys>` öğesi. Öğe yoksa, ekleyin.
1. Tüm Yapıştır `<UserJourney>` bir alt öğesi olarak kopyaladığınız düğümü `<UserJourneys>` öğesi.
1. Yeni kullanıcı gezisine Kimliğini yeniden adlandırın (örneğin, olarak yeniden adlandırın `SignUpOrSignUsingAzureAD`). 

### <a name="step-42-display-the-button"></a>Adım 4.2 görünen "button"

`<ClaimsProviderSelection>` Öğesidir bir oturumu-up/oturum açma ekranında bir kimlik sağlayıcısı düğmesini benzer. Eklerseniz bir `<ClaimsProviderSelection>` öğesi Azure AD için yeni bir düğme görüntülenir sayfasında bir kullanıcı adlandırıldığını olduğunda. Bu öğe eklemek için:

1. Bul `<OrchestrationStep>` içeren düğüm `Order="1"` , oluşturduğunuz kullanıcı gezisine içinde.
1. Aşağıdakileri ekleyin:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="AzureADExchange" />
    ```

1. Ayarlama `TargetClaimsExchangeId` uygun bir değere. Aynısına başkalarının aşağıdaki öneririz:  *\[ClaimProviderName\]Exchange*.

### <a name="step-43-link-the-button-to-an-action"></a>Adım 4.3 bağlantı için bir eylem düğmesi

Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için Azure AD ile iletişim kurmak Azure AD B2C içindir. Düğme, Azure AD talep sağlayıcısı teknik profili bağlayarak bir eyleme bağlantı:

1. Bul `<OrchestrationStep>` içeren `Order="2"` içinde `<UserJourney>` düğümü.
1. Aşağıdakileri ekleyin:

    ```XML
    <ClaimsExchange Id="AzureADExchange" TechnicalProfileReferenceId="Common-AAD" />
    ```

1. Güncelleştirme `Id` ile aynı değere sahip `TargetClaimsExchangeId` önceki bölümde.
1. Güncelleştirme `TechnicalProfileReferenceId` teknik profili kimliği için oluşturduğunuz önceki (ortak-AAD).

## <a name="step-5-create-a-new-rp-policy"></a>5. adım: yeni bir RP ilkesi oluşturma

Şimdi yeni oluşturduğunuz kullanıcı gezisine başlatacaktır bağlı olan taraf (RP) dosyasını güncelleştirmeniz gerekir:
 
1. Çalışma dizininizi SignUpOrSignIn.xml bir kopyasını oluşturun ve yeniden adlandırın (örneğin, SignUpOrSignInWithAAD.xml için yeniden adlandırmadan).  
1. Yeni dosya ve güncelleştirme açmak `PolicyId` için öznitelik `<TrustFrameworkPolicy>` benzersiz bir değerle (örneğin, SignUpOrSignInWithAAD). Bu, ilkenin adı olacaktır (örneğin, B2C\_1A\_SignUpOrSignInWithAAD). 
1. Değiştirme `ReferenceId` özniteliğini `<DefaultUserJourney>` (SignUpOrSignUsingAzureAD) oluşturulan yeni kullanıcı gezisine Kimliğini eşleşecek şekilde. 
1. Yaptığınız değişiklikleri kaydedin ve dosyayı karşıya yükleyin. 

## <a name="step-6-upload-the-policy-to-your-tenant"></a>6. adım: ilke kiracınız için karşıya yükleme

1. İçinde [Azure portal](https://portal.azure.com), geçiş [bağlamı Azure AD B2C kiracınızın](active-directory-b2c-navigate-to-b2c-context.md)ve ardından **Azure AD B2C**.
2. Seçin **kimlik deneyimi Framework**.
3. Seçin **tüm ilkeleri**.
4. Seçin **karşıya İlkesi**.
5. Seçin **varsa ilkesi üzerine** onay kutusu.
6. Karşıya yükleme `TrustFrameworkExtensions.xml` dosyasını ve RP dosyasını (örneğin `SignUpOrSignInWithAAD.xml`) ve doğrulama geçirirler emin olun.

## <a name="step-7-test-the-custom-policy-by-using-run-now"></a>7. adım: Test Şimdi Çalıştır kullanarak özel İlkesi

1. Seçin **Azure AD B2C ayarlarını**ve ardından **kimlik deneyimi Framework**.
    > [!NOTE]
    > Çalışma şimdi Kiracı'preregistered için en az bir uygulama için gereklidir. Uygulamaları kaydetmek öğrenmek için Azure AD B2C bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makale.

1. Karşıya yüklediğiniz bağlı olan taraf (RP) özel İlkesi'ni açın (*B2C\_1A\_SignUpOrSignInWithAAD*) ve ardından **Şimdi Çalıştır**.
1. Artık Azure AD hesabı kullanarak oturum açabilir olması gerekir.

## <a name="optional-step-8-register-the-azure-ad-account-claims-provider-to-the-profile-edit-user-journey"></a>(İsteğe bağlı) 8. adım: Profil düzenleme kullanıcı gezisine Azure AD hesabı talep sağlayıcısını Kaydet

Azure AD hesabı kimlik sağlayıcısı eklemek isteyebilirsiniz, `ProfileEdit` kullanıcı gezisine. Kullanıcı gezisine kullanılabilmesi için 6. adım 4 yineleyin. Bu süre, select `<UserJourney>` içeren düğümü `Id="ProfileEdit"`. Kaydetme, karşıya yükleme ve ilkeniz test.

## <a name="troubleshooting"></a>Sorun giderme

İlgili sorunları tanılamak için okuma [sorun giderme](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Sonraki adımlar

Geri bildirim sağlayın [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
