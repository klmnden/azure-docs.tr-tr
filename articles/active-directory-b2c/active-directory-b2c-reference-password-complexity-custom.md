---
title: Parola karmaşıklığı özel ilkeleri Azure Active Directory B2C | Microsoft Docs
description: Parola karmaşıklık gereksinimlerini özel ilkede yapılandırılır.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/16/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c6b8312a08d1d92bccf70e7d3dda5f01811b4f87
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848536"
---
# <a name="configure-password-complexity-in-custom-policies"></a>Parola karmaşıklığını özel ilkeleri yapılandırma

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, parola karmaşıklığını nasıl çalıştığına ilişkin gelişmiş bir açıklama ve özel Azure AD B2C ilkeleri kullanılarak etkinleştirilir.

## <a name="azure-ad-b2c-configure-complexity-requirements-for-passwords"></a>Azure AD B2C: parola karmaşıklık gereksinimlerini yapılandırma

Azure Active Directory B2C (Azure AD B2C) destekleyen bir hesabı oluştururken bir son kullanıcı tarafından sağlanan parola karmaşıklık gereksinimleri değiştirme.  Varsayılan olarak, Azure AD B2C kullanır **güçlü** parola.  Azure AD B2C, ayrıca müşteriler parola karmaşıklığı denetlemek için yapılandırma seçeneklerini destekler.  Bu makalede, parola karmaşıklığını özel ilkeleri yapılandırma hakkında konuşuyor.  Kullanmak da mümkündür [parola karmaşıklığını yerleşik ilkeleri yapılandırma](active-directory-b2c-reference-password-complexity.md).

## <a name="prerequisites"></a>Önkoşullar

Bölümünde anlatıldığı gibi yerel bir hesap oturumu açma kaydolma/oturum açma, tamamlamak için yapılandırılmış bir Azure AD B2C kiracısı [Başlarken](active-directory-b2c-get-started-custom.md).

## <a name="how-to-configure-password-complexity-in-custom-policy"></a>Parola karmaşıklığını özel ilke yapılandırma

Parola karmaşıklığını özel ilke yapılandırmak için özel ilkeniz genel yapısını içermelidir bir `ClaimsSchema`, `Predicates`, ve `InputValidations` öğe içinde `BuildingBlocks`.

```XML
  <BuildingBlocks>
    <ClaimsSchema>...</ClaimsSchema>
    <Predicates>...</Predicates>
    <InputValidations>...</InputValidations>
  </BuildingBlocks>
```

Bu öğeleri amacı aşağıdaki gibidir:

- Her `Predicate` öğesi true veya false değeri döndüren bir temel dize doğrulama denetimi tanımlar.
- `InputValidations` Öğeye sahip bir veya daha fazla `InputValidation` öğeleri.  Her `InputValidation` bir dizi kullanarak oluşturulan `Predicate` öğeleri. Bu öğe Boole toplamalar yapmanıza olanak tanır (benzer şekilde `and` ve `or`).
- `ClaimsSchema` Hangi talep Doğrulanmakta olan tanımlar.  Bunun ardından tanımlayan `InputValidation` bu talebi doğrulamak için kullanılan kural.

### <a name="defining-a-predicate-element"></a>Bir koşul öğesi tanımlama

Koşullar sahip iki yöntem tür: IsLengthRange veya MatchesRegex. Her bir örneği gözden geçirelim.  İlk normal bir ifadeyle eşleşen için kullanılan MatchesRegex örneği sahibiz.  Bu örnekte, bu sayıları içeren dizeyle eşleşir.

```XML
      <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be a pin.">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
```

Sonraki IsLengthRange örneği gözden geçirelim.  Bu yöntem, minimum ve maksimum dize uzunluğunu alır.

```XML
      <Predicate Id="Length" Method="IsLengthRange" HelpText="The password must be between 8 and 16 characters.">
        <Parameters>
          <Parameter Id="Minimum">8</Parameter>
          <Parameter Id="Maximum">16</Parameter>
        </Parameters>
      </Predicate>
```

Kullanım `HelpText` denetimi başarısız olursa, son kullanıcılar için bir hata iletisi sağlamak için özniteliği.  Bu dize kullanarak yerelleştirilebilen [dil özelleştirme özelliği](active-directory-b2c-reference-language-customization.md).

### <a name="defining-an-inputvalidation-element"></a>InputValidation öğesi tanımlama

Bir `InputValidation` birleştirilmesinden oluşan olan `PredicateReferences`. Her `PredicateReferences` doğru sırada olmalıdır `InputValidation` başarılı olması için.  Bununla birlikte, içinde `PredicateReferences` öğesi kullanımı olarak adlandırılan bir öznitelik `MatchAtLeast` belirtmek için kaç `PredicateReference` denetimleri true döndürmelidir.  İsteğe bağlı olarak tanımlayan bir `HelpText` özniteliği hata iletisi geçersiz kılmak için tanımlanan `Predicate` ona başvuran öğeler.

```XML
      <InputValidation Id="PasswordValidation">
        <PredicateReferences Id="LengthGroup" MatchAtLeast="1">
          <PredicateReference Id="Length" />
        </PredicateReferences>
        <PredicateReferences Id="3of4" MatchAtLeast="3" HelpText="You must have at least 3 of the following character classes:">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </InputValidation>
```

### <a name="defining-a-claimsschema-element"></a>ClaimsSchema öğesi tanımlama

Talep türleri `newPassword` ve `reenterPassword` adlarını değiştirmeyin bu nedenle özel olarak kabul edilir.  Kullanıcı arabirimini kullanıcıyı doğrulayan doğru bunlar üzerinde temel hesap oluşturma sırasında parolalarını reentered `ClaimType` öğeleri.  Aynı bulmak için `ClaimType` başlangıç Pack TrustFrameworkBase.xml öğeleri bakın.  Bu örnekte yenilikler Biz bu öğeleri tanımlamak için geçersiz olan bir `InputValidationReference`. `ID` Özniteliği, bu yeni işaret eden `InputValidation` tanımladığımız öğesi.

```XML
    <ClaimsSchema>
      <ClaimType Id="newPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
```

### <a name="putting-it-all-together"></a>Hepsini bir araya getirme

Bu örnek gösterir parçaların birlikte çalışma ilkesi oluşturmak için nasıl uyumlu bir şekilde.  Bu örneği kullanmak için:

1. Önkoşul yönergeleri [Başlarken](active-directory-b2c-get-started-custom.md) indirmek için yapılandırma ve TrustFrameworkBase.xml ve TrustFrameworkExtensions.xml karşıya yükleyin
1. Bu bölümde örnek içeriği kullanarak bir SignUporSignIn.xml dosyası oluşturun.
1. SignUporSignIn.xml değiştirerek güncelleştirme `yourtenant` Azure AD B2C Kiracı adınızla.
1. SignUporSignIn.xml ilke dosyasını en son karşıya yükleyin.

Bu örnek, bir doğrulama PIN parolaları için diğeri de güçlü parolalar içerir:

- Aranacak `PINpassword`. Bu `InputValidation` öğesi herhangi bir uzunlukta bir PIN doğrular.  İçinde başvurulmadığı için şu anda kullanılmaz `InputValidationReference` öğe içinde `ClaimType`. 
- Aranacak `PasswordValidation`. Bu `InputValidation` öğeyi doğrular parola 8-16 karakter ve 3 / 4 büyük harf, küçük harf, sayı içeren veya semboller.  İçinde başvurulan `ClaimType`.  Bu nedenle, bu kural Bu ilkede zorlanmasını.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<TrustFrameworkPolicy
  xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="https://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="yourtenant.onmicrosoft.com"
  PolicyId="B2C_1A_signup_signin"
  PublicPolicyUri="http://yourtenant.onmicrosoft.com/B2C_1A_signup_signin">
 <BasePolicy>
    <TenantId>yourtenant.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkExtensions</PolicyId>
  </BasePolicy>
  <BuildingBlocks>
    <ClaimsSchema>
      <ClaimType Id="newPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
    <Predicates>
      <Predicate Id="Lowercase" Method="MatchesRegex" HelpText="a lowercase">
        <Parameters>
          <Parameter Id="RegularExpression">[a-z]+</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Uppercase" Method="MatchesRegex" HelpText="an uppercase">
        <Parameters>
          <Parameter Id="RegularExpression">[A-Z]+</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Number" Method="MatchesRegex" HelpText="a number">
        <Parameters>
          <Parameter Id="RegularExpression">[0-9]+</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Symbol" Method="MatchesRegex" HelpText="a symbol">
        <Parameters>
          <Parameter Id="RegularExpression">[!@#$%^*()]+</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Length" Method="IsLengthRange" HelpText="The password must be between 8 and 16 characters.">
        <Parameters>
          <Parameter Id="Minimum">8</Parameter>
          <Parameter Id="Maximum">16</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be a pin.">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
    </Predicates>
    <InputValidations>
      <InputValidation Id="PasswordValidation">
        <PredicateReferences Id="LengthGroup" MatchAtLeast="1">
          <PredicateReference Id="Length" />
        </PredicateReferences>
        <PredicateReferences Id="3of4" MatchAtLeast="3" HelpText="You must have at least 3 of the following character classes:">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </InputValidation>
      <InputValidation Id="PINpassword">
        <PredicateReferences Id="PINGroup">
          <PredicateReference Id="PIN" />
        </PredicateReferences>
      </InputValidation>
    </InputValidations>
  </BuildingBlocks>
  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration, DisableStrongPassword" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
</TrustFrameworkPolicy>
```
