---
title: Özel ilkeleri ayarlama oturum değiştirebilir ve kendi kendine sağlayıcısı onaylanan yapılandırma | Microsoft Docs
description: Kaydolun ve kullanıcı girişi yapılandırmak bir kılavuz ekleme talepleri
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/29/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 2989af12407bdddf6e55e8967a0a574fff690208
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55179217"
---
# <a name="azure-active-directory-b2c-modify-sign-up-to-add-new-claims-and-configure-user-input"></a>Azure Active Directory B2C: Yeni Talep ekleyin ve kullanıcı girişi yapılandırmak için yukarı oturum değiştirin.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, kayıt kullanıcı yolculuğunuza yeni bir kullanıcı tarafından sağlanan giriş (talep) ekleyeceksiniz.  Giriş bir açılan yapılandıracak ve gerekirse tanımlayın.

## <a name="prerequisites"></a>Önkoşullar

* Makaledeki adımları tamamlayabilmeniz [özel ilkeleri ile çalışmaya başlama](active-directory-b2c-get-started-custom.md).  Devam etmeden önce yeni bir yerel hesap kaydolma için kaydolma/oturum açma kullanıcı yolculuğu test edin.


Kullanıcılarınızdan gelen ilk veri toplama kaydolma/oturum açma elde edilir.  Ek talep daha sonra Profil düzenleme kullanıcı yolculuklarından toplanabilir. Etkileşimli kullanıcı doğrudan Azure AD B2C bilgi toplayan zaman kimlik deneyimi çerçevesi kullanan kendi `selfasserted provider`. Bu sağlayıcı kullanılan herhangi bir zamanda aşağıdaki adımları uygulayın.


## <a name="define-the-claim-its-display-name-and-the-user-input-type"></a>Talep, görünen adını ve kullanıcı girişi türü tanımlayın
Kullanıcıdan kendi şehri olanak tanır.  Şu öğeye eklemek `<ClaimsSchema>` TrustFrameworkBase ilkesi dosyasında öğe:

```xml
<ClaimType Id="city">
  <DisplayName>city</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```
Burada talep özelleştirmek için yapabileceğiniz ek seçenek vardır.  Tam bir şema için başvurmak **kimlik deneyimi çerçevesi Teknik Başvuru Kılavuzu**.  Bu kılavuz, başvuru bölümüne yakında yayımlanacaktır.

* `<DisplayName>` kullanıcıya yönelik tanımlayan bir dizedir *etiketi*

* `<UserHelpText>` kullanıcının gerektiğini anlamak yardımcı olur.

* `<UserInputType>` Aşağıdaki dört seçenekleri aşağıda vurgulanan:
    * `TextBox`
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

    * `RadioSingleSelectduration` -Tek bir seçim zorlar.
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>RadioSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

    * `DropdownSingleSelect` -Yalnızca geçerli değer seçimini sağlar.

![Açılan seçeneğinin ekran görüntüsü](./media/active-directory-b2c-configure-signup-self-asserted-custom/dropdown-menu-example.png)


```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```


* `CheckboxMultiSelect` İçin bir veya daha fazla değer seçimini sağlar.

![Çoklu seçim yapılabilen seçenek ekran görüntüsü](./media/active-directory-b2c-configure-signup-self-asserted-custom/multiselect-menu-example.png)


```xml
<ClaimType Id="city">
  <DisplayName>Receive updates from which cities?</DisplayName>
  <DataType>string</DataType>
  <UserInputType>CheckboxMultiSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

## <a name="add-the-claim-to-the-sign-upsign-in-user-journey"></a>Oturum açma için talep eklemek yukarı/kullanıcı yolculuğunda oturum

1. Talep olarak ekleme bir `<OutputClaim ClaimTypeReferenceId="city"/>` TechnicalProfile için `LocalAccountSignUpWithLogonEmail` (TrustFrameworkBase ilke dosyasında bulunur).  Bu TechnicalProfile SelfAssertedAttributeProvider kullandığına dikkat edin.

  ```xml
  <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
    <DisplayName>Email signup</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
      <Item Key="IpAddressClaimReferenceId">IpAddress</Item>
      <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
      <Item Key="language.button_continue">Create</Item>
    </Metadata>
    <CryptographicKeys>
      <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
    </CryptographicKeys>
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" />
    </InputClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" />
      <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
      <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
      <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
      <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />
      <OutputClaim ClaimTypeReferenceId="authenticationSource" />
      <OutputClaim ClaimTypeReferenceId="newUser" />
      <!-- Optional claims, to be collected from the user -->
      <OutputClaim ClaimTypeReferenceId="givenName" />
      <OutputClaim ClaimTypeReferenceId="surName" />
      <OutputClaim ClaimTypeReferenceId="city"/>
    </OutputClaims>
    <ValidationTechnicalProfiles>
      <ValidationTechnicalProfile ReferenceId="AAD-UserWriteUsingLogonEmail" />
    </ValidationTechnicalProfiles>
    <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
  </TechnicalProfile>
  ```

2. Talep AAD UserWriteUsingLogonEmail ekleme bir `<PersistedClaim ClaimTypeReferenceId="city" />` kullanıcıdan topladıktan sonra talep AAD dizinine yazılacak. Dizinde talep gelecekte kullanım için kalmayacak tercih ederseniz bu adımı atlayabilirsiniz.

  ```xml
  <!-- Technical profiles for local accounts -->
  <TechnicalProfile Id="AAD-UserWriteUsingLogonEmail">
    <Metadata>
      <Item Key="Operation">Write</Item>
      <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">true</Item>
    </Metadata>
    <IncludeInSso>false</IncludeInSso>
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" Required="true" />
    </InputClaims>
    <PersistedClaims>
      <!-- Required claims -->
      <PersistedClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" />
      <PersistedClaim ClaimTypeReferenceId="newPassword" PartnerClaimType="password" />
      <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />
      <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration" />
      <!-- Optional claims. -->
      <PersistedClaim ClaimTypeReferenceId="givenName" />
      <PersistedClaim ClaimTypeReferenceId="surname" />
      <PersistedClaim ClaimTypeReferenceId="city" />
    </PersistedClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" />
      <OutputClaim ClaimTypeReferenceId="newUser" PartnerClaimType="newClaimsPrincipalCreated" />
      <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
      <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
      <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
    </OutputClaims>
    <IncludeTechnicalProfile ReferenceId="AAD-Common" />
    <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
  </TechnicalProfile>
  ```

3. Talep Directory'den bir kullanıcı olarak oturum açtığında okuyan TechnicalProfile ekleme bir `<OutputClaim ClaimTypeReferenceId="city" />`

  ```xml
  <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
    <Metadata>
      <Item Key="Operation">Read</Item>
      <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">An account could not be found for the provided user ID.</Item>
    </Metadata>
    <IncludeInSso>false</IncludeInSso>
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames" Required="true" />
    </InputClaims>
    <OutputClaims>
      <!-- Required claims -->
      <OutputClaim ClaimTypeReferenceId="objectId" />
      <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
      <!-- Optional claims -->
      <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="otherMails" />
      <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
      <OutputClaim ClaimTypeReferenceId="city" />
    </OutputClaims>
    <IncludeTechnicalProfile ReferenceId="AAD-Common" />
  </TechnicalProfile>
  ```

4. Ekleme `<OutputClaim ClaimTypeReferenceId="city" />` bu talep, başarılı kullanıcı yolculuğu sonra belirteçte uygulamaya gönderilen SignUporSignIn.xml RP ilkeye dosya.

  ```xml
  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" />
        <OutputClaim ClaimTypeReferenceId="city" />
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
  ```

## <a name="test-the-custom-policy-using-run-now"></a>"Şimdi Çalıştır" kullanarak özel bir ilkeyi test etme

1. Açık **Azure AD B2C dikey** gidin **kimlik deneyimi Çerçevesi > özel ilkeleri**.
2. Yüklenmiş ve'ı tıklatın özel bir ilkeyi seçin **Şimdi Çalıştır** düğmesi.
3. Bir e-posta adresi kullanarak kaydolma olması gerekir.

Test modunda kaydolma ekran şuna benzer görünmelidir:

![Değiştirilen kayıt seçeneğinin ekran görüntüsü](./media/active-directory-b2c-configure-signup-self-asserted-custom/signup-with-city-claim-dropdown-example.png)

  Belirtece uygulamanız artık içerecektir `city` aşağıda gösterildiği gibi talep
```json
{
  "exp": 1493596822,
  "nbf": 1493593222,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "9c2a3a9e-ac65-4e46-a12d-9557b63033a9",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustf_signup_signin",
  "nonce": "defaultNonce",
  "iat": 1493593222,
  "auth_time": 1493593222,
  "email": "joe@outlook.com",
  "given_name": "Joe",
  "family_name": "Ras",
  "city": "Bellevue",
  "name": "unknown"
}
```

## <a name="optional-remove-email-verification-from-signup-journey"></a>İsteğe bağlı: E-posta doğrulama kaydolma yolculuğu kaldırın

E-posta doğrulama işlemini atlamak için ilke Yazar kaldırmayı seçebilirsiniz `PartnerClaimType="Verified.Email"`. E-posta adresi gerekiyor ancak, "Gerekmedikçe" doğrulanmadı = true kaldırılır.  Bu seçenek, kullanım durumları için doğru seçenek olup olmadığını dikkatlice!

E-posta, varsayılan olarak etkindir doğrulandı `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` başlangıç paketi TrustFrameworkBase ilkesi dosyasında:
```xml
<OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
```

## <a name="next-steps"></a>Sonraki adımlar

İlkeniz, sosyal medya hesaplarını destekliyorsa, yeni akışlar Talep'i sosyal hesap oturum açma bilgileri için aşağıdaki teknik profil değiştirerek ekleyin. Bu talep, toplamak ve kullanıcıdan veri yazmak için sosyal hesap oturum açma bilgileri tarafından kullanılır.

1. Teknik profilini bulun **SelfAsserted sosyal** ve çıkış talep ekleyin. Taleplerde sırasını **OutputClaims** Azure AD B2C ekranında talepleri işler sırasını denetler. Örneğin, `<OutputClaim ClaimTypeReferenceId="city" />`.
2. Teknik profilini bulun **AAD UserWriteUsingAlternativeSecurityId** ve kalan talebi ekler. Örneğin, `<PersistedClaim ClaimTypeReferenceId="city" />`.
3. Teknik profilini bulun **AAD UserReadUsingAlternativeSecurityId** ve çıkış talep ekleyin. Örneğin, `<OutputClaim ClaimTypeReferenceId="city" />`.
