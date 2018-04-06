---
title: Parola karmaşıklığını özel ilkelerinde - Azure AD B2C | Microsoft Docs
description: Özel İlkesi'nde parola karmaşıklık gereksinimlerini yapılandırma
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 08/16/2017
ms.author: davidmu
ms.openlocfilehash: 648c800da928ae230bde1ba92aea8c319a4d8fe2
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="configure-password-complexity-in-custom-policies"></a>Parola karmaşıklığını özel ilkeleri yapılandırma

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, parola karmaşıklığını nasıl çalıştığını Gelişmiş bir açıklama ve Azure AD B2C özel ilkelerini kullanma etkindir.

## <a name="azure-ad-b2c-configure-complexity-requirements-for-passwords"></a>Azure AD B2C: parolaların karmaşıklık gereksinimlerini yapılandırabilirsiniz

Azure Active Directory B2C (Azure AD B2C) destekleyen bir hesap oluşturulurken bir son kullanıcı tarafından sağlanan parola karmaşıklık gereksinimlerini değiştirme.  Varsayılan olarak, Azure AD B2C kullanır **güçlü** parolalar.  Azure AD B2C da müşterilerin kullanabileceğiniz parolaların karmaşıklık denetlemek için yapılandırma seçeneklerini destekler.  Bu makalede, parola karmaşıklığını özel ilkelerinde nasıl yapılandırılacağı hakkında alınmaktadır.  Kullanmak da mümkündür [parola karmaşıklığını yerleşik ilkeleri yapılandırma](active-directory-b2c-reference-password-complexity.md).

## <a name="prerequisites"></a>Önkoşullar

Bölümünde açıklandığı gibi bir yerel hesap oturumu-up/oturum açmayı tamamlamak için yapılandırılmış bir Azure AD B2C kiracısı [Başlarken](active-directory-b2c-get-started-custom.md).

## <a name="how-to-configure-password-complexity-in-custom-policy"></a>Parola karmaşıklığını özel ilke yapılandırma

Parola karmaşıklığını özel ilke yapılandırmak için özel ilkeniz genel yapısını içermelidir bir `ClaimsSchema`, `Predicates`, ve `InputValidations` öğesi içinde `BuildingBlocks`.

```XML
  <BuildingBlocks>
    <ClaimsSchema>...</ClaimsSchema>
    <Predicates>...</Predicates>
    <InputValidations>...</InputValidations>
  </BuildingBlocks>
```

Bu öğelerin amacını aşağıdaki gibidir:

- Her `Predicate` öğesi true veya false döndürür bir temel dize doğrulama denetimi tanımlar.
- `InputValidations` Öğeye sahip bir veya daha fazla `InputValidation` öğeleri.  Her `InputValidation` bir dizi kullanılarak oluşturulan `Predicate` öğeleri. Bu öğe boolean toplamalar işlemleri yapmanıza olanak tanır (benzer şekilde `and` ve `or`).
- `ClaimsSchema` Hangi talep doğrulandığı tanımlar.  Bunun ardından tanımlayan `InputValidation` bu talep doğrulamak için kullanılan kural.

### <a name="defining-a-predicate-element"></a>Bir koşul öğesi tanımlama

Koşulları iki yöntem türlerine sahip: IsLengthRange veya MatchesRegex. Şimdi her örneği gözden geçirin.  İlk normal bir ifade eşleştirmek için kullanılan MatchesRegex örneği gerekir.  Bu örnekte, bu sayıları içeren dizeyle eşleşir.

```XML
      <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be a pin.">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
```

Sonraki şimdi IsLengthRange örneği gözden geçirin.  Bu yöntem, minimum ve maksimum dize uzunluğu alır.

```XML
      <Predicate Id="Length" Method="IsLengthRange" HelpText="The password must be between 8 and 16 characters.">
        <Parameters>
          <Parameter Id="Minimum">8</Parameter>
          <Parameter Id="Maximum">16</Parameter>
        </Parameters>
      </Predicate>
```

Kullanım `HelpText` özniteliği denetimi başarısız olursa, son kullanıcılar için bir hata iletisi sağlayın.  Bu dize kullanarak yerelleştirilebilir [dil özelleştirme özelliği](active-directory-b2c-reference-language-customization.md).

### <a name="defining-an-inputvalidation-element"></a>Bir InputValidation öğesi tanımlama

Bir `InputValidation` toplamı olan `PredicateReferences`. Her `PredicateReferences` sırayla true olmalıdır `InputValidation` başarılı olması için.  Bununla birlikte, içinde `PredicateReferences` olarak adlandırılan bir öznitelik öğesi kullanım `MatchAtLeast` belirtmek için kaç tane `PredicateReference` denetimleri true döndürmelidir.  İsteğe bağlı olarak tanımlayan bir `HelpText` hata iletisi geçersiz kılmak için öznitelik tanımlanan `Predicate` başvurduğu öğeleri.

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

Talep türleri `newPassword` ve `reenterPassword` adları değiştirmeyin şekilde özel olarak kabul edilir.  Kullanıcı arabirimini kullanıcıyı doğrulayan doğru bunlar üzerinde temel hesabı oluşturma sırasında parolalarını reentered `ClaimType` öğeleri.  Aynı bulmak için `ClaimType` starter paketinde TrustFrameworkBase.xml öğeleri bakın.  Bu örnekte yenilikler Biz bu öğeleri tanımlamak için geçersiz kılma emin olup bir `InputValidationReference`. `ID` Bu yeni öğesinin özniteliği işaret eden `InputValidation` tanımladığımız öğesi.

```XML
    <ClaimsSchema>
      <ClaimType Id="newPassword">
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
```

### <a name="putting-it-all-together"></a>Tüm bir araya getirme

Bu örnek gösterir tüm parçaları birlikte çalışma ilkesi oluşturmak için uygun nasıl.  Bu örneği kullanmak için:

1. Önkoşul'ndaki yönergeleri izleyin [Başlarken](active-directory-b2c-get-started-custom.md) karşıdan yüklemek, yapılandırmak ve TrustFrameworkBase.xml ve TrustFrameworkExtensions.xml karşıya yükleyin
1. Bu bölümde örnek içeriği kullanarak bir SignUporSignIn.xml dosyası oluşturun.
1. SignUporSignIn.xml değiştirme güncelleştirme `yourtenant` Azure AD B2C Kiracı adınız ile.
1. Son SignUporSignIn.xml ilke dosyası yükleyin.

Bu örnek, PIN parolalar için doğrulama ve güçlü parolalar için bir tane içerir:

- Ara `PINpassword`. Bu `InputValidation` öğesi bir PIN herhangi bir uzunluktaki doğrular.  İçinde başvurulmadığı için şu anda kullanılmaz `InputValidationReference` öğesi içinde `ClaimType`. 
- Ara `PasswordValidation`. Bu `InputValidation` öğesi doğrular parola 8-16 karakter, simge veya ve büyük harf, küçük harf, rakam, 4, 3 içerir.  İçinde başvurulan `ClaimType`.  Bu nedenle, bu kural Bu ilkede zorlanan.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<TrustFrameworkPolicy
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
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
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
    <Predicates>
      <Predicate Id="Lowercase" Method="MatchesRegex" HelpText="a lowercase">
        <Parameters>
          <Parameter Id="RegularExpression">^[a-z]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Uppercase" Method="MatchesRegex" HelpText="an uppercase">
        <Parameters>
          <Parameter Id="RegularExpression">^[A-Z]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Number" Method="MatchesRegex" HelpText="a number">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Symbol" Method="MatchesRegex" HelpText="a symbol">
        <Parameters>
          <Parameter Id="RegularExpression">^[!@#$%^*()]+$</Parameter>
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
