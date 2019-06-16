---
title: Özel bir ilke Azure Active Directory B2C, bir Azure Active Directory teknik profili tanımlamak | Microsoft Docs
description: Azure Active Directory B2C özel bir ilke Azure Active Directory teknik profili tanımlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 8b8bbe540d9e296b0f6a0c11a62d3b861e0115d3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66507440"
---
# <a name="define-an-azure-active-directory-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Bir Azure Active Directory teknik profili, bir Azure Active Directory B2C özel ilke tanımlayın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C, Azure Active Directory kullanıcı yönetimi için destek sağlar. Bu makalede, standartlaştırılmış bu protokolü destekleyen bir talep sağlayıcı ile etkileşim kurmak için bir teknik profil ayrıntılarını açıklar.

## <a name="protocol"></a>Protocol

**Adı** özniteliği **Protokolü** öğesi ayarlanması gerekiyor `Proprietary`. **İşleyici** özniteliği, protokol işleyici bütünleştirilmiş kodun tam adı içermelidir `Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`.

Tüm Azure AD teknik profillerini içeren **AAD yaygın** teknik profili. Protokol olarak yapılandırıldığından teknik profiller Protokolü belirtmeyin **AAD yaygın** teknik profil:

- **AAD UserReadUsingAlternativeSecurityId** ve **AAD UserReadUsingAlternativeSecurityId NoError** - arama yukarı dizinindeki sosyal hesap.
- **AAD UserWriteUsingAlternativeSecurityId** -yeni bir sosyal hesap oluşturun.
- **AAD UserReadUsingEmailAddress** - arama yukarı dizine yerel bir hesap. 
- **AAD UserWriteUsingLogonEmail** -yeni bir yerel hesap oluşturun.
- **AAD UserWritePasswordUsingObjectId** -bir yerel hesap parolasını güncelleştirin.
- **AAD UserWriteProfileUsingObjectId** -yerel veya sosyal hesabının bir kullanıcı profili güncelleştirilemiyor.
- **AAD UserReadUsingObjectId** -yerel veya sosyal hesabının bir kullanıcı profilini okuyun.
- **AAD UserWritePhoneNumberUsingObjectId** -yerel veya sosyal hesap MFA telefon numarasını yazın

Aşağıdaki örnekte gösterildiği **AAD yaygın** teknik profil:

```XML
<TechnicalProfile Id="AAD-Common">
  <DisplayName>Azure Active Directory</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />

  <CryptographicKeys>
    <Key Id="issuer_secret" StorageReferenceId="B2C_1A_TokenSigningKeyContainer" />
  </CryptographicKeys>

  <!-- We need this here to suppress the SelfAsserted provider from invoking SSO on validation profiles. -->
  <IncludeInSso>false</IncludeInSso>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

## <a name="input-claims"></a>Giriş talepleri

Aşağıdaki teknik profiller dahil **InputClaims** sosyal ve yerel hesapları için:

- Sosyal hesap teknik profiller **AAD UserReadUsingAlternativeSecurityId** ve **AAD UserWriteUsingAlternativeSecurityId** içerir **AlternativeSecurityId** talep. Bu talep, sosyal hesap kullanıcı tanımlayıcısını içerir.
- Yerel hesap teknik profiller **AAD UserReadUsingEmailAddress** ve **AAD UserWriteUsingLogonEmail** içerir **e-posta** talep. Bu talep, yerel hesap oturum açma adını içerir.
- Birleştirilmiş (yerel ve sosyal) Teknik profiller **AAD UserReadUsingObjectId**, **AAD UserWritePasswordUsingObjectId**, **AAD UserWriteProfileUsingObjectId**, ve **AAD UserWritePhoneNumberUsingObjectId** içerir **objectID** talep. Hesabın benzersiz tanımlayıcısı.

**InputClaimsTransformations** öğe koleksiyonu içerebilir **InputClaimsTransformation** giriş talepleri değiştirmek veya yenilerini oluşturmak için kullanılan öğeleri.

## <a name="output-claims"></a>Çıkış talep

**OutputClaims** öğesi talep Azure AD'ye teknik profili tarafından döndürülen bir listesini içerir. İlkeniz için Azure Active Directory'de tanımlanan adını tanımlanan talep eşleştirmek gerekebilir. Ayarladığınız sürece, Azure Active Directory tarafından döndürülen olmayan talepleri de içerebilir `DefaultValue` özniteliği.

**OutputClaimsTransformations** öğe koleksiyonu içerebilir **OutputClaimsTransformation** çıkış talep değiştirmek veya yenilerini oluşturmak için kullanılan öğeleri.

Örneğin, **AAD UserWriteUsingLogonEmail** teknik profili, bir yerel hesabı oluşturur ve aşağıdaki döndürür:

- **objectID**, yeni hesap tanımlayıcı olduğu
- **newUser**, kullanıcının yeni olup olmadığını gösterir
- **authenticationSource**, hangi ayarlar kimlik doğrulaması `localAccountAuthentication`
- **userPrincipalName**, yeni hesabın kullanıcı asıl adı
- **signInNames.emailAddress**, hesap oturum açma adı, benzer **e-posta** giriş talep

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="objectId" />
  <OutputClaim ClaimTypeReferenceId="newUser" PartnerClaimType="newClaimsPrincipalCreated" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
  <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
  <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
</OutputClaims>
```

## <a name="persistedclaims"></a>PersistedClaims

**PersistedClaims** öğeyi içeren tüm ilke ve Azure AD özniteliği ClaimsSchema bölümünde önceden tanımlanmış bir talep türü arasındaki olası eşleme bilgileri Azure AD'ye tarafından kalıcı değerleri adı. 

**AAD UserWriteUsingLogonEmail** yeni yerel hesabı oluşturur, teknik profil devam ederse, talep:

```XML
  <PersistedClaims>
    <!-- Required claims -->
    <PersistedClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" />
    <PersistedClaim ClaimTypeReferenceId="newPassword" PartnerClaimType="password"/>
    <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />
    <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration" />

    <!-- Optional claims. -->
    <PersistedClaim ClaimTypeReferenceId="givenName" />
    <PersistedClaim ClaimTypeReferenceId="surname" />
  </PersistedClaims>
```

Talep adı Azure AD özniteliği sürece adıdır **PartnerClaimType** özniteliği belirtilirse, hangi Azure AD özniteliği adı içeriyor.

## <a name="requirements-of-an-operation"></a>Bir işlem gereksinimleri

- Olmalıdır tam olarak bir **Inputclaim** tüm Azure AD teknik profiller için talep paketi öğesi. 
- İşlem olursa `Write` veya `DeleteClaims`, ayrıca içinde yer almalıdır, ardından bir **PersistedClaims** öğesi.
- Değerini **userPrincipalName** talep biçiminde olmalıdır `user@tenant.onmicrosoft.com`.
- **DisplayName** talep gereklidir ve boş bir dize olamaz.

## <a name="azure-ad-technical-provider-operations"></a>Azure AD teknik sağlayıcısı işlemleri

### <a name="read"></a>Okuma

**Okuma** işlemi tek bir kullanıcı hesabıyla ilgili verileri okur. Kullanıcı verileri okumak için bir anahtar gibi bir giriş talebini belirtmeniz gerekir **objectID**, **userPrincipalName**, **signInNames** (tüm türü, kullanıcı adı ve e-posta tabanlı bir hesabı) veya **alternativeSecurityId**.  

Aşağıdaki teknik profili, kullanıcının objectID kullanarak bir kullanıcı hesabıyla ilgili verileri okur:

```XML
<TechnicalProfile Id="AAD-UserReadUsingObjectId">
  <Metadata>
    <Item Key="Operation">Read</Item>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
  </Metadata>
  <IncludeInSso>false</IncludeInSso>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
  </InputClaims>
  <OutputClaims>

    <!-- Required claims -->
    <OutputClaim ClaimTypeReferenceId="strongAuthenticationPhoneNumber" />

    <!-- Optional claims -->
    <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
    <OutputClaim ClaimTypeReferenceId="givenName" />
    <OutputClaim ClaimTypeReferenceId="surname" />
  </OutputClaims>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

### <a name="write"></a>Yazma

**Yazma** işlemi oluşturur veya tek bir kullanıcı hesabı güncelleştirir. Bir kullanıcı hesabı yazmak için bir anahtar gibi bir giriş talebini belirtmeniz gerekir **objectID**, **userPrincipalName**, **signInNames.emailAddress**, veya  **alternativeSecurityId**.  

Aşağıdaki teknik profili, yeni sosyal hesap oluşturur:

```XML
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="Operation">Write</Item>
    <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">true</Item>
    <Item Key="UserMessageIfClaimsPrincipalAlreadyExists">You are already registered, please press the back button and sign in instead.</Item>
  </Metadata>
  <IncludeInSso>false</IncludeInSso>
  <InputClaimsTransformations>
    <InputClaimsTransformation ReferenceId="CreateOtherMailsFromEmail" />
  </InputClaimsTransformations>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityId" PartnerClaimType="alternativeSecurityId" Required="true" />
  </InputClaims>
  <PersistedClaims>
    <!-- Required claims -->
    <PersistedClaim ClaimTypeReferenceId="alternativeSecurityId" />
    <PersistedClaim ClaimTypeReferenceId="userPrincipalName" />
    <PersistedClaim ClaimTypeReferenceId="mailNickName" DefaultValue="unknown" />
    <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />

    <!-- Optional claims -->
    <PersistedClaim ClaimTypeReferenceId="otherMails" />
    <PersistedClaim ClaimTypeReferenceId="givenName" />
    <PersistedClaim ClaimTypeReferenceId="surname" />
  </PersistedClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="newUser" PartnerClaimType="newClaimsPrincipalCreated" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
  </OutputClaims>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
</TechnicalProfile>
```

### <a name="deleteclaims"></a>DeleteClaims

**DeleteClaims** işlemi talepleri sağlanan listeden bilgilerini temizler. Gelen talepler bilgileri silmek için bir anahtar gibi bir giriş talebini belirtmeniz gerekir **objectID**, **userPrincipalName**, **signInNames.emailAddress** veya **alternativeSecurityId**.  

Aşağıdaki teknik profili, talep siler:

```XML
<TechnicalProfile Id="AAD-DeleteClaimsUsingObjectId">
  <Metadata>
    <Item Key="Operation">DeleteClaims</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
  </InputClaims>
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="objectId" />
    <PersistedClaim ClaimTypeReferenceId="Verified.strongAuthenticationPhoneNumber" PartnerClaimType="strongAuthenticationPhoneNumber" />
  </PersistedClaims>
  <OutputClaims />
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

### <a name="deleteclaimsprincipal"></a>DeleteClaimsPrincipal

**DeleteClaimsPrincipal** işlem dizinden tek bir kullanıcı hesabı siler. Bir kullanıcı hesabını silmek için bir anahtar gibi bir giriş talebini belirtmeniz gerekir **objectID**, **userPrincipalName**, **signInNames.emailAddress** veya  **alternativeSecurityId**.  

Aşağıdaki teknik profili bir kullanıcı hesabının kullanıcı asıl adı kullanarak dizin siler:

```XML
<TechnicalProfile Id="AAD-DeleteUserUsingObjectId">
  <Metadata>
    <Item Key="Operation">DeleteClaimsPrincipal</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
  </InputClaims>
  <OutputClaims/>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

Aşağıdaki teknik profili kullanarak bir sosyal kullanıcı hesabı siler **alternativeSecurityId**:

```XML
<TechnicalProfile Id="AAD-DeleteUserUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="Operation">DeleteClaimsPrincipal</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="alternativeSecurityId" Required="true" />
  </InputClaims>
  <OutputClaims/>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```
## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| İşlem | Evet | Gerçekleştirilecek işlem. Olası değerler: `Read`, `Write`, `DeleteClaims`, veya `DeleteClaimsPrincipal`. | 
| RaiseErrorIfClaimsPrincipalDoesNotExist | Hayır | Kullanıcı nesnesi dizinde mevcut değilse bir hata oluşturun. Olası değerler: `true` veya `false`. | 
| UserMessageIfClaimsPrincipalDoesNotExist | Hayır | Bir hata verilmesine ise (RaiseErrorIfClaimsPrincipalDoesNotExist öznitelik açıklaması bakın), kullanıcı nesnesi yoksa, kullanıcıya göstermek için iletiyi belirtirsiniz. Değer olabilir [yerelleştirilmiş](localization.md).| 
| RaiseErrorIfClaimsPrincipalAlreadyExists | Hayır | Kullanıcı nesnesi zaten varsa, hata oluşturur. Olası değerler: `true` veya `false`.| 
| UserMessageIfClaimsPrincipalAlreadyExists | Hayır | Bir hata verilmesine ise (RaiseErrorIfClaimsPrincipalAlreadyExists öznitelik açıklaması bakın), kullanıcı nesnesi zaten varsa, kullanıcıya göstermek için iletiyi belirtirsiniz. Değer olabilir [yerelleştirilmiş](localization.md).| 
| ApplicationObjectId | Hayır | Uzantı öznitelikleri uygulama nesne tanımlayıcısı. Değer: ObjectID uygulamanın. Daha fazla bilgi için [özel bir profilde özel öznitelikler kullanın ilkesini Düzenle](active-directory-b2c-create-custom-attributes-profile-edit-custom.md). | 
| ClientId | Hayır | Kiracı bir üçüncü taraf olarak erişmek için istemci tanımlayıcısı. Daha fazla bilgi için [özel bir profilde özel öznitelikler kullanın ilkesini Düzenle](active-directory-b2c-create-custom-attributes-profile-edit-custom.md) | 














