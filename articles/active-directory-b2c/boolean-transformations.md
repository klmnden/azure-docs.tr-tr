---
title: Kimlik deneyimi çerçevesi şema Azure Active Directory B2C için Boolean talep dönüştürme örnekler | Microsoft Docs
description: Boolean dönüştürme örnekler kimlik deneyimi çerçevesi şema, Azure Active Directory B2C için talep.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 0a08849340d19055a03f85ca401757a81cd2c95d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66511734"
---
# <a name="boolean-claims-transformations"></a>Boole talep dönüştürmeleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory (Azure AD) B2C'de kimlik deneyimi çerçevesi şema Boole talep dönüştürmeleri kullanma örnekleri sağlar. Daha fazla bilgi için [ClaimsTransformations](claimstransformations.md).

## <a name="andclaims"></a>Konular Veilgili

İki boolean inputClaims ve işlemini gerçekleştirir ve işlemin sonucu ile outputClaim ayarlar.

| Öğe  | TransformationClaimType  | Veri Türü  | Notlar |
|-------| ------------------------ | ---------- | ----- |
| Inputclaim | inputClaim1 | boole | Değerlendirilecek ilk ClaimType. |
| Inputclaim | inputClaim2  | boole | Değerlendirilecek ikinci ClaimType. |
|outputClaim | outputClaim | boole | Bu dönüşüm talep sonra üretilecek ClaimTypes (true veya false) çağrılmış. |

Aşağıdaki talep dönüştürme gösterir nasıl yapılır ve iki boolean ClaimTypes: `isEmailNotExist`, ve `isSocialAccount`. Çıkış talep `presentEmailSelfAsserted` ayarlanır `true` hem giriş talep değerini ise `true`. Sosyal hesap e-posta boş ise bir düzenleme adımı, önkoşul otomatik olarak onaylanan bir sayfa önceden yüklenmiş olarak kullanabilirsiniz.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="AndClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isEmailNotExist" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="isSocialAccount" TransformationClaimType="inputClaim2" />
  </InputClaims>                    
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="presentEmailSelfAsserted" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputClaim1**: true
    - **inputClaim2**: false
- Çıkış talep:
    - **outputClaim**: false


## <a name="assertbooleanclaimisequaltovalue"></a>AssertBooleanClaimIsEqualToValue

Boole değerleri iki talep eşitse ve değilse bir özel durum oluşturur denetler.

| Öğe | TransformationClaimType  | Veri Türü  | Notlar |
| ---- | ------------------------ | ---------- | ----- |
| Inputclaim | Inputclaim | boole | Değerlendirilecek ClaimType. |
| InputParameter |valueToCompareTo | boole | (True veya false) Karşılaştırılacak değer. |

**AssertBooleanClaimIsEqualToValue** talep dönüştürme gelen her zaman yürütülür bir [doğrulama teknik profili](validation-technical-profile.md) çağrılan bir [teknik profilSelfonaylanan](self-asserted-technical-profile.md). **UserMessageIfClaimsTransformationBooleanValueIsNotEqual** otomatik olarak onaylanan teknik profil meta teknik profil kullanıcıya sunan hata iletisi denetler.

![AssertStringClaimsAreEqual execution](./media/boolean-transformations/assert-execution.png)

Aşağıdaki talep dönüştürme ile bir boolean ClaimType değerini denetlemek anlatan bir `true` değeri. Varsa değerini `accountEnabled` ClaimType false ise, bir hata iletisi oluşturulur.

```XML
<ClaimsTransformation Id="AssertAccountEnabledIsTrue" TransformationMethod="AssertBooleanClaimIsEqualToValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="accountEnabled" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="valueToCompareTo" DataType="boolean" Value="true" />
  </InputParameters>
</ClaimsTransformation>
```


`login-NonInteractive` Doğrulama teknik profil çağrıları `AssertAccountEnabledIsTrue` talep dönüştürme.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertAccountEnabledIsTrue" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

Doğrulama otomatik olarak onaylanan teknik profil çağırır **etkileşimli olmayan oturum açma** teknik profili.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationBooleanValueIsNotEqual">Custom error message if account is disabled.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **Inputclaim**: false
    - **valueToCompareTo**: true
- Sonuç: Durum hatası

## <a name="notclaims"></a>NotClaims

Bir boolean Inputclaim değil işlemi gerçekleştirir ve işlemin sonucu ile outputClaim ayarlar.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | Inputclaim | boole | Yapılacak talep. |
| outputClaim | outputClaim | boole | Bu ClaimsTransformation (true veya false) çağrıldıktan sonra üretilen ClaimTypes. |

Mantıksal olumsuzlaştırma üzerinde bir talep için talep dönüştürmeyi bu kullanın.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="NotClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="inputClaim" />
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **Inputclaim**: false
- Çıkış talep:
    - **outputClaim**: true

## <a name="orclaims"></a>OrClaims 

Bir veya iki boolean inputClaims hesaplar ve işlemin sonucu ile outputClaim ayarlar.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | inputClaim1 | boole | Değerlendirilecek ilk ClaimType. |
| Inputclaim | inputClaim2 | boole | Değerlendirilecek ikinci ClaimType. |
| outputClaim | outputClaim | boole | Bu ClaimsTransformation (true veya false) çağrıldıktan sonra üretilecek ClaimTypes. |

Aşağıdaki talep dönüştürme gösterir nasıl `Or` iki boolean ClaimTypes. Bir talep değerini ise düzenleme adımda önkoşul otomatik olarak onaylanan bir sayfa önceden için kullanabileceğiniz `true`.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="OrClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isLastTOSAcceptedNotExists" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="isLastTOSAcceptedGreaterThanNow" TransformationClaimType="inputClaim2" />
  </InputClaims>                    
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="presentTOSSelfAsserted" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputClaim1**: true
    - **inputClaim2**: false
- Çıkış talep:
    - **outputClaim**: true

