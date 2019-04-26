---
title: Azure Active Directory B2C, özel bir ilkede talep dönüştürme teknik profil tanımlama | Microsoft Docs
description: Talep dönüştürme teknik profili Azure Active Directory B2C özel bir ilke tanımlayın.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: a204e8cdc20a6897c40d4d5f68217a2922371737
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60386712"
---
# <a name="define-a-claims-transformation-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Talep dönüştürme teknik profil bir Azure Active Directory B2C özel ilke tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Talep dönüştürme teknik profili, çıkış talep dönüşümleri, taleplerin değerlerini değiştirmek, talepleri doğrulamak veya çıkış talep kümesi için varsayılan değerleri ayarlamak için çağrılacak sağlar.

## <a name="protocol"></a>Protokol

**Adı** özniteliği **Protokolü** öğesi ayarlanması gerekiyor `Proprietary`. **İşleyici** özniteliği Azure AD B2C tarafından kullanılan protokol işleyicisi bütünleştirilmiş kodun tam adı içermesi gerekir: `Web.TPEngine.Providers.ClaimsTransformationProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`.

Aşağıdaki örnek, bir talep dönüştürme teknik profili gösterir:

```XML
<TechnicalProfile Id="Facebook-OAUTH-UnLink">
    <DisplayName>Unlink Facebook</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.ClaimsTransformationProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="output-claims"></a>Çıkış talep

**OutputClaims** öğe zorunludur. En az bir talep teknik profili tarafından döndürülen çıkış sağlamanız gerekir. Aşağıdaki örnek, varsayılan değerleri çıkış talep kümesi gösterilmektedir:

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="ageGroup" DefaultValue="Undefined" />
  <OutputClaim ClaimTypeReferenceId="ageGroupValueChanged" DefaultValue="false" />
</OutputClaims>
```

## <a name="output-claims-transformations"></a>Çıkış talep dönüşümleri

**OutputClaimsTransformations** öğe koleksiyonu içerebilir **OutputClaimsTransformation** talep değiştirmek veya yenilerini oluşturmak için kullanılan öğeleri. Aşağıdaki teknik profil çağrıları **RemoveAlternativeSecurityIdByIdentityProvider** talep dönüştürme. Bu talep dönüştürme kaldırır bir sosyal tanımlamak koleksiyonundan **Alternativesecurityıds**. Bu teknik profilinin çıkış talepler **identityProvider2**, Hosted `facebook.com`, ve **Alternativesecurityıds**, bununla ilişkili sosyal kimlikleri listesini içerir facebook.com kimlik kaldırıldıktan sonra kullanıcı.

```XML
<ClaimsTransformations>
  <ClaimsTransformation Id="RemoveAlternativeSecurityIdByIdentityProvider"
TransformationMethod="RemoveAlternativeSecurityIdByIdentityProvider">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="IdentityProvider2"
TransformationClaimType="identityProvider" />
      <InputClaim ClaimTypeReferenceId="AlternativeSecurityIds"
TransformationClaimType="collection" />
    </InputClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="AlternativeSecurityIds"
TransformationClaimType="collection" />
    </OutputClaims>
  </ClaimsTransformation>
</ClaimsTransformations>
...
<TechnicalProfile Id="Facebook-OAUTH-UnLink">
    <DisplayName>Unlink Facebook</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.ClaimsTransformationProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="identityProvider2" DefaultValue="facebook.com" AlwaysUseDefaultValue="true" />
    </OutputClaims>
    <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="RemoveAlternativeSecurityIdByIdentityProvider" />
    </OutputClaimsTransformations>
    <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

Talep dönüştürme teknik profili, bir kullanıcı yolculuğu'nın düzenleme adımı bir talep dönüştürme yürütmek sağlar. Aşağıdaki örnekte, düzenleme adımı çağırır Temel'e değiştirilemedi teknik profillerinden birini gibi **Temel'e değiştirilemedi Facebook OAUTH**. Bu teknik profili talepleri dönüştürme teknik profil çağırır **RemoveAlternativeSecurityIdByIdentityProvider**, yeni oluşturduğu **AlternativeSecurityIds2** içeren talep Facebook kimlik koleksiyonlardan kaldırılırken kullanıcı sosyal kimlikleri listesi.

```XML
<UserJourney Id="AccountUnLink">
  <OrchestrationSteps>
    ...
    <OrchestrationStep Order="8" Type="ClaimsExchange">
      <ClaimsExchanges>
        <ClaimsExchange Id="UnLinkFacebookExchange" TechnicalProfileReferenceId="UnLink-Facebook-OAUTH" />
        <ClaimsExchange Id="UnLinkMicrosoftExchange" TechnicalProfileReferenceId="UnLink-Microsoft-OAUTH" />
        <ClaimsExchange Id="UnLinkGitHubExchange" TechnicalProfileReferenceId="UnLink-GitHub-OAUTH" />
      </ClaimsExchanges>
    </OrchestrationStep>
    ...
  </OrchestrationSteps>
</UserJourney>
```

## <a name="use-a-validation-technical-profile"></a>Doğrulama teknik profilini kullanmak

Talep dönüştürme teknik profil bilgileri doğrulamak için kullanılabilir. Aşağıdaki örnekte, [kendi kendine teknik profil onaylanan](self-asserted-technical-profile.md) adlı **LocalAccountSignUpWithLogonEmail** kullanıcıdan iki kez e-posta girin, sonra çağıran [teknik doğrulama profili](validation-technical-profile.md) adlı **doğrulama e-posta** e-postaları doğrulamak için. **Doğrulama e-posta** teknik profili, talep dönüştürme çağırır **AssertEmailAreEqual** iki talep Karşılaştırılacak **e-posta** ve **emailRepeat** ve bunlar belirtilen karşılaştırma göre eşit değilse bir özel durum.

```XML
<ClaimsTransformations>
  <ClaimsTransformation Id="AssertEmailAreEqual" TransformationMethod="AssertStringClaimsAreEqual">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim1" />
      <InputClaim ClaimTypeReferenceId="emailRepeat" TransformationClaimType="inputClaim2" />
    </InputClaims>
    <InputParameters>
      <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
    </InputParameters>
  </ClaimsTransformation>
</ClaimsTransformations>
```

Talep dönüştürme teknik profil çağrıları **AssertEmailAreEqual** kullanıcı tarafından sağlanan e-postaları aynı değerler olduğunu onaylar, dönüştürme talep.

```XML
<TechnicalProfile Id="Validate-Email">
  <DisplayName>Unlink Facebook</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.ClaimsTransformationProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="emailRepeat" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" />
  </OutputClaims>
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertEmailAreEqual" />
  </OutputClaimsTransformations>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

Bir kendi kendine onaylanan teknik profili doğrulama teknik profili çağırın ve belirtilen hata mesajını göstermeye **UserMessageIfClaimsTransformationStringsAreNotEqual** meta verileri.

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>User ID signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    ...
    <Item Key="UserMessageIfClaimsTransformationStringsAreNotEqual">The email addresses you provided are not the same</Item>
  </Metadata>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" />
    <OutputClaim ClaimTypeReferenceId="emailRepeat" />
    ...
  </OutputClaims>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="Validate-Email" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```
