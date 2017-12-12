---
title: "Azure Active Directory B2C: Self Servis parola değiştirme | Microsoft Docs"
description: "Azure Active Directory B2C içinde tüketicileriniz için Self Servis parola değişikliği kurma gösteren bir konu"
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: mtillman
ms.assetid: 712a7128-5788-4914-8a52-24e200aa4de1
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2016
ms.author: vigunase
ms.openlocfilehash: 76e7ed328716d09dc57e25f15c411f07fda77bb9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-configure-password-change-in-custom-policies"></a>Azure Active Directory B2C: parola değiştirme özel ilkelerini yapılandırın.  
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Parola değişikliği özelliğiyle (yerel hesaplar kullanarak) oturum açmış tüketicilere parolalarını açıklandığı gibi e-posta doğrulama ile bunların kimlik doğrulamasını kanıtlamak gerek kalmadan değiştirebilirsiniz [Self Servis parola sıfırlama akış.](active-directory-b2c-reference-sspr.md) Tüketici alır zamanına göre oturumun süresi dolarsa akış için parola değiştirme, kullanıcı yeniden oturum açmanız istenir. 

## <a name="prerequisites"></a>Ön koşullar

Bölümünde açıklandığı gibi bir yerel hesap oturumu-up/oturum açmayı tamamlamak için yapılandırılmış bir Azure AD B2C kiracısı [Başlarken](active-directory-b2c-get-started-custom.md).

## <a name="how-to-configure-password-change-in-custom-policy"></a>Parola değiştirme özel ilke yapılandırma

Parola değiştirme özel ilkesindeki yapılandırmak için güven framework uzantıları ilkenizde aşağıdaki değişiklikleri yapın, 

## <a name="define-a-claimtype-oldpassword"></a>ClaimType 'EskiParola' tanımlayın

Genel yapısı, özel ilkeniz içermelidir bir `ClaimsSchema`ve yeni bir tanımlama `ClaimType` aşağıdaki olarak ' EskiParola' 

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

Bu öğelerin amacını aşağıdaki gibidir:

- `ClaimsSchema` Hangi talep doğrulandığı tanımlar.  Bu durumda, 'eski parola' doğrulanacak. 

## <a name="add-a-password-change-claims-provider-with-its-supporting-elements"></a>Parola değişikliği Talep sağlayıcı destekleyen öğeleri ile ekleme

Parola değiştirme Talep sağlayıcı

1. Eski parola karşı kullanıcı kimlik doğrulaması
2. Ve 'yeni parola' 'Yeni Parolayı Onayla' eşleşiyorsa, bu değer B2C veri deposunda depolanır ve bu nedenle parolası başarıyla değiştirildi. 

![görüntü](images/passwordchange.jpg)

Aşağıdaki Talep sağlayıcı uzantıları ilkeniz ekleyin. 

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



### <a name="add-the-application-ids-to-your-custom-policy"></a>Özel ilkeniz uygulama kimlikleri Ekle

Uygulama kimlikleri uzantıları dosyasına ekleyin (`TrustFrameworkExtensions.xml`):

1. Uzantıları dosyasında (TrustFrameworkExtensions.xml) öğesi bulunamıyor `<TechnicalProfile Id="login-NonInteractive">` ve`<TechnicalProfile Id="login-NonInteractive-PasswordChange">`

2. Tüm örneklerinin yerine `IdentityExperienceFrameworkAppId` açıklandığı gibi kimlik deneyimi Framework uygulamanın uygulama Kimliğine sahip [Başlarken](active-directory-b2c-get-started-custom.md). Örnek aşağıda verilmiştir:

   ```
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```

3. Tüm örneklerinin yerine `ProxyIdentityExperienceFrameworkAppId` açıklandığı gibi Proxy kimlik deneyimi Framework uygulamanın uygulama Kimliğine sahip [Başlarken](active-directory-b2c-get-started-custom.md).

4. Uzantıları dosyanızı kaydedin.



## <a name="create-a-password-change-user-journey"></a>Parola değişikliği kullanıcı gezisine oluşturma

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

Tamamladığınızda uzantısının değiştirme. Kaydet ve bu dosyayı karşıya yükleyin. Tüm Doğrulamalar başarıyla emin olun.



## <a name="create-a-relying-party-rp-file"></a>Bir bağlı olan taraf (RP) dosyası oluşturun

Ardından, oluşturduğunuz kullanıcı gezisine başlatır bağlı olan taraf (RP) dosyasını güncelleştirin:

1. Çalışma dizininizi ProfileEdit.xml kopyası. Sonra (örneğin, PasswordChange.xml) yeniden adlandırın.
2. Yeni dosya ve güncelleştirme açmak `PolicyId` için öznitelik `<TrustFrameworkPolicy>` benzersiz bir değere sahip. İlkeniz (örneğin, PasswordChange) adıdır.
3. Değiştirme `ReferenceId` özniteliğini `<DefaultUserJourney>` eşleşecek şekilde `Id` (örneğin, PasswordChange) oluşturulan yeni kullanıcı gezisine biri.
4. Yaptığınız değişiklikleri kaydedin ve dosyayı karşıya yükleyin.
5. Azure portalında, yüklediğiniz, özel ilke sınamak için ilke dikey penceresine gidin ve ardından **Şimdi Çalıştır**.




## <a name="link-to-password-change-sample-policy"></a>Parola değişikliği örnek İlkesi'ne bağlama

Örnek ilke bulabilirsiniz [burada](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/password-change). 










