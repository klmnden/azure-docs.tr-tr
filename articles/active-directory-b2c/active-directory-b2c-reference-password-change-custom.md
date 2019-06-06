---
title: Azure Active Directory B2C'de özel ilkeleri kullanarak parola değişikliği yapılandırma | Microsoft Docs
description: Azure Active Directory B2C'de özel ilkeleri kullanarak parolalarını değiştirme olanağı hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: a49f62b6fc1ea00084266d4c5405f8bf96d034cb
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66509251"
---
# <a name="configure-password-change-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeleri kullanarak parola değişikliği yapılandırın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C'de, yerel bir hesapla oturum olmadıklarına kanıtlamak gerek kalmadan parolalarını değiştirmek için e-posta doğrulaması ile oturum açan kullanıcılar etkinleştirebilirsiniz. Kullanıcı zaman oturumu sona ererse akışı parolasını değiştirmek, yeniden oturum açmanız istenir. Bu makalede parola değişikliği yapılandırma işlemi gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md). Yapılandırmak da mümkündür [Self Servis parola sıfırlama](active-directory-b2c-reference-sspr.md) kullanıcı akışları için.

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [Active Directory B2C özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md).

## <a name="add-the-elements"></a>Öğeleri Ekle 

1. Açık, *TrustframeworkExtensions.xml* dosyasını açıp aşağıdaki **ClaimType** tanımlayıcısına sahip öğe `oldPassword` için [ClaimsSchema](claimsschema.md) öğesi: 

    ```XML
    <BuildingBlocks>
      <ClaimsSchema>
        <ClaimType Id="oldPassword">
          <DisplayName>Old Password</DisplayName>
          <DataType>string</DataType>
          <UserHelpText>Enter password</UserHelpText>
          <UserInputType>Password</UserInputType>
        </ClaimType>
      </ClaimsSchema>
    </BuildingBlocks>
    ```

2. A [ClaimsProvider](claimsproviders.md) öğesi, kullanıcının kimliğini doğrulayan teknik profili içerir. Aşağıdaki Talep Sağlayıcıları Ekle **ClaimsProviders** öğesi:

    ```XML
    <ClaimsProviders>
      <ClaimsProvider>
        <DisplayName>Local Account SignIn</DisplayName>
        <TechnicalProfiles>
          <TechnicalProfile Id="login-NonInteractive-PasswordChange">
            <DisplayName>Local Account SignIn</DisplayName>
            <Protocol Name="OpenIdConnect" />
            <Metadata>
              <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">We can't seem to find your account</Item>
              <Item Key="UserMessageIfInvalidPassword">Your password is incorrect</Item>
              <Item Key="UserMessageIfOldPasswordUsed">Looks like you used an old password</Item>
              <Item Key="ProviderName">https://sts.windows.net/</Item>
              <Item Key="METADATA">https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration</Item>
              <Item Key="authorization_endpoint">https://login.microsoftonline.com/{tenant}/oauth2/token</Item>
              <Item Key="response_types">id_token</Item>
              <Item Key="response_mode">query</Item>
              <Item Key="scope">email openid</Item>
              <Item Key="UsePolicyInRedirectUri">false</Item>
              <Item Key="HttpBinding">POST</Item>
              <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
              <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
            </Metadata>
            <InputClaims>
              <InputClaim ClaimTypeReferenceId="signInName" PartnerClaimType="username" Required="true" />
              <InputClaim ClaimTypeReferenceId="oldPassword" PartnerClaimType="password" Required="true" />
              <InputClaim ClaimTypeReferenceId="grant_type" DefaultValue="password" />
              <InputClaim ClaimTypeReferenceId="scope" DefaultValue="openid" />
              <InputClaim ClaimTypeReferenceId="nca" PartnerClaimType="nca" DefaultValue="1" />
              <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
              <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
            </InputClaims>
            <OutputClaims>
              <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="oid" />
              <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid" />
              <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
              <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
              <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
              <OutputClaim ClaimTypeReferenceId="userPrincipalName" PartnerClaimType="upn" />
              <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
            </OutputClaims>
          </TechnicalProfile>
        </TechnicalProfiles>
      </ClaimsProvider>
      <ClaimsProvider>
        <DisplayName>Local Account Password Change</DisplayName>
        <TechnicalProfiles>
          <TechnicalProfile Id="LocalAccountWritePasswordChangeUsingObjectId">
            <DisplayName>Change password (username)</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
              <Item Key="ContentDefinitionReferenceId">api.selfasserted</Item>
            </Metadata>
            <CryptographicKeys>
              <Key Id="issuer_secret" StorageReferenceId="B2C_1A_TokenSigningKeyContainer" />
            </CryptographicKeys>
            <InputClaims>
              <InputClaim ClaimTypeReferenceId="objectId" />
            </InputClaims>
            <OutputClaims>
              <OutputClaim ClaimTypeReferenceId="oldPassword" Required="true" />
              <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
              <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
            </OutputClaims>
            <ValidationTechnicalProfiles>
              <ValidationTechnicalProfile ReferenceId="login-NonInteractive-PasswordChange" />
              <ValidationTechnicalProfile ReferenceId="AAD-UserWritePasswordUsingObjectId" />
            </ValidationTechnicalProfiles>
          </TechnicalProfile>
        </TechnicalProfiles>
      </ClaimsProvider>
    </ClaimsProviders>
    ```

    Değiştirin `IdentityExperienceFrameworkAppId` önkoşul öğreticisinde oluşturulan IdentityExperienceFramework uygulamanın uygulama kimliği. Değiştirin `ProxyIdentityExperienceFrameworkAppId` ayrıca daha önce oluşturduğunuz ProxyIdentityExperienceFramework uygulamanın uygulama kimliği.

3. [UserJourney](userjourneys.md) öğesi uygulamanızla etkileşim kurulurken, kullanıcının gerçekleştirdiği yolunu tanımlar. Ekleme **UserJourneys** ile yoksa öğe **UserJourney** olarak tanımlanan `PasswordChange`:

    ```XML
    <UserJourneys>
      <UserJourney Id="PasswordChange">
        <OrchestrationSteps>
          <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
            <ClaimsProviderSelections>
              <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
            </ClaimsProviderSelections>
          </OrchestrationStep>
          <OrchestrationStep Order="2" Type="ClaimsExchange">
            <ClaimsExchanges>
              <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
            </ClaimsExchanges>
          </OrchestrationStep>
          <OrchestrationStep Order="3" Type="ClaimsExchange">
            <ClaimsExchanges>
              <ClaimsExchange Id="NewCredentials" TechnicalProfileReferenceId="LocalAccountWritePasswordChangeUsingObjectId" />
            </ClaimsExchanges>
          </OrchestrationStep>
          <OrchestrationStep Order="4" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
        </OrchestrationSteps>
        <ClientDefinition ReferenceId="DefaultWeb" />
      </UserJourney>
    </UserJourneys>
    ```

4. Kaydet *TrustFrameworkExtensions.xml* ilke dosyası.
5. Kopyalama *ProfileEdit.xml* ile başlangıç paketi indirilir ve adlandırın dosya *ProfileEditPasswordChange.xml*.
6. Yeni bir dosya açıp güncelleştirme **Policyıd** benzersiz bir değere sahip bir öznitelik. İlkenizin adını değerdir. Örneğin, *B2C_1A_profile_edit_password_change*.
7. Değiştirme **Referenceıd** özniteliğini `<DefaultUserJourney>` oluşturduğunuz yeni kullanıcı yolculuğu kimliği eşleştirmek için. Örneğin, *PasswordChange*.
8. Yaptığınız değişiklikleri kaydedin.

Örnek ilke bulabilirsiniz [burada](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/password-change). 

## <a name="test-your-policy"></a>İlkenizi test

Uygulamalarınızı Azure AD B2C'de test etme, döndürülen Azure AD B2C belirteci sahip olmak yararlı olabilir `https://jwt.ms` Taleplerde gözden geçirebilmek için.

### <a name="upload-the-files"></a>Dosyaları karşıya yükleme

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **kimlik deneyimi çerçevesi**.
5. Özel ilkeler sayfasında tıklayın **karşıya yükleme İlkesi**.
6. Seçin **ilke varsa üzerine**, arayın ve seçin *TrustframeworkExtensions.xml* dosya.
7. **Karşıya Yükle**'ye tıklayın.
8. 5. adım-7 gibi bağlı olan taraf dosyası için yineleyin *ProfileEditPasswordChange.xml*.

### <a name="run-the-policy"></a>İlke çalıştırın

1. Değiştirdiğiniz İlkesi'ni açın. Örneğin, *B2C_1A_profile_edit_password_change*.
2. İçin **uygulama**, daha önce kaydettiğiniz uygulamanızı seçin. Belirteç görmek için **yanıt URL'si** göstermelidir `https://jwt.ms`.
3. **Şimdi çalıştır**’a tıklayın. Daha önce oluşturduğunuz acouunt ile oturum açın. Şimdi parola değiştirme fırsatını da olmalıdır. 

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında öğrenin [parola karmaşıklığını Azure Active Directory B2C'de özel ilkeleri kullanarak yapılandırma](active-directory-b2c-reference-password-complexity-custom.md). 
