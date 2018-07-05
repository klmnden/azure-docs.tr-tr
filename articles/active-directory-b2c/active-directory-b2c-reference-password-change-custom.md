---
title: Azure Active Directory B2C'de Self Servis parola değişimi | Microsoft Docs
description: Azure Active Directory B2C'de tüketicileriniz için Self Servis parola değişimi ayarlamak nasıl yazılacağını gösteren bir konu.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 028d10b5c005be2db7cfd9c5ca5210ab55f0592a
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448144"
---
# <a name="azure-active-directory-b2c-configure-password-change-in-custom-policies"></a>Azure Active Directory B2C: parola değiştirme özel ilkeleri yapılandırın.  
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Parola değişikliği özelliğiyle tüketici oturum açma (yerel hesaplar kullanarak) parolalarını açıklandığı gibi e-posta doğrulama tarafından olmadıklarına kanıtlamak gerek kalmadan değiştirebilirsiniz [Self Servis parola sıfırlama akış.](active-directory-b2c-reference-sspr.md) Tüketici alır zaman oturumu sona ererse akış için parola değiştirme, kullanıcı yeniden oturum açmanız istenir. 

## <a name="prerequisites"></a>Önkoşullar

Bölümünde anlatıldığı gibi yerel bir hesap oturumu açma kaydolma/oturum açma, tamamlamak için yapılandırılmış bir Azure AD B2C kiracısı [Başlarken](active-directory-b2c-get-started-custom.md).

## <a name="how-to-configure-password-change-in-custom-policy"></a>Parola değiştirme özel ilke yapılandırma

Parola değiştirme özel ilkesindeki yapılandırmak için güven framework uzantıları ilkenizde aşağıdaki değişiklikleri yapın, 

## <a name="define-a-claimtype-oldpassword"></a>Bir ClaimType 'oldPassword' tanımlayın

Genel, özel ilkeniz yapısını içermelidir bir `ClaimsSchema`ve yeni bir tanımlama `ClaimType` olarak aşağıdaki ' oldPassword' 

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

Bu öğeleri amacı aşağıdaki gibidir:

- `ClaimsSchema` Hangi talep Doğrulanmakta olan tanımlar.  Bu durumda, 'eski parola' doğrulanır. 

## <a name="add-a-password-change-claims-provider-with-its-supporting-elements"></a>Destekleyici öğeleri ile bir parola değişikliği talep sağlayıcısı Ekle

Parola değiştirme talep sağlayıcısı olur

1. Kullanıcı eski parolayı karşı kimlik doğrulaması
2. Ve 'yeni parola', 'Yeni Parolayı Onayla' eşleşiyorsa, bu değer, B2C veri deposunda depolanır ve bu nedenle parolası başarıyla değiştirildi. 

![görüntü](images/passwordchange.jpg)

Aşağıdaki talep sağlayıcısı uzantıları ilkenizi ekleyin. 

```XML
<ClaimsProviders>
    <ClaimsProvider>
      <DisplayName>Local Account SignIn</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="login-NonInteractive">
          <Metadata>
           <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
           <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
            <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
          </InputClaims>
        </TechnicalProfile>
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



### <a name="add-the-application-ids-to-your-custom-policy"></a>Özel ilkenizi uygulama kimlikleri ekleme

Uygulama kimlikleri uzantıları dosyaya ekleyin (`TrustFrameworkExtensions.xml`):

1. Öğesi (TrustFrameworkExtensions.xml) uzantıları dosyasında bulma `<TechnicalProfile Id="login-NonInteractive">` ve `<TechnicalProfile Id="login-NonInteractive-PasswordChange">`

2. Tüm örneklerinin yerine `IdentityExperienceFrameworkAppId` açıklandığı gibi kimlik deneyimi çerçevesi uygulamasının uygulama kimliği ile [Başlarken](active-directory-b2c-get-started-custom.md). Örnek aşağıda verilmiştir:

   ```
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```

3. Tüm örneklerinin yerine `ProxyIdentityExperienceFrameworkAppId` açıklandığı Proxy kimlik deneyimi çerçevesi uygulamasının uygulama kimliği ile [Başlarken](active-directory-b2c-get-started-custom.md).

4. Uzantıları dosyanızı kaydedin.



## <a name="create-a-password-change-user-journey"></a>Parola değişikliği kullanıcı yolculuğu oluşturma

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

İşiniz uzantısının değiştirme. Kaydet ve bu dosyayı karşıya yükleyin. Tüm Doğrulamalar başarıyla emin olun.



## <a name="create-a-relying-party-rp-file"></a>Bir bağlı olan taraf (RP) dosyası oluşturun

Ardından, oluşturduğunuz kullanıcı yolculuğu başlatan bağlı olan taraf (RP) dosyasını güncelleştirin:

1. Çalışma dizininizde ProfileEdit.xml kopyalanmasına. Ardından, dosyayı (örneğin, PasswordChange.xml) yeniden adlandırın.
2. Yeni bir dosya açıp güncelleştirme `PolicyId` özniteliğini `<TrustFrameworkPolicy>` benzersiz bir değere sahip. İlkenizi (örneğin, PasswordChange) adıdır.
3. Değiştirme `ReferenceId` özniteliğini `<DefaultUserJourney>` eşleştirilecek `Id` (örneğin, PasswordChange) oluşturduğunuz yeni kullanıcı yolculuğunun.
4. Yaptığınız değişiklikleri kaydedin ve ardından dosyayı karşıya yükleyin.
5. Azure portalında, karşıya yüklediğiniz özel bir ilkeyi test etmek için ilke dikey penceresine gidin ve ardından **Şimdi Çalıştır**.




## <a name="link-to-password-change-sample-policy"></a>Parola değişikliği örnek ilkesine yönelik bağlantı

Örnek ilke bulabilirsiniz [burada](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/password-change). 










