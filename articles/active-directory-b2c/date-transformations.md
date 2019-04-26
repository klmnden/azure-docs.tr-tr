---
title: Kimlik deneyimi çerçevesi şema Azure Active Directory B2C için tarih talep dönüştürme örnekler | Microsoft Docs
description: Tarih dönüştürme örnekler kimlik deneyimi çerçevesi şema, Azure Active Directory B2C için talep.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: d36abb669490b3d3f6818c018b3844a82ecd0617
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60384247"
---
# <a name="date-claims-transformations"></a>Tarih dönüştürmeleri talep

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory (Azure AD) B2C'de kimlik deneyimi çerçevesi şema tarih talep Dönüşümleri kullanma örnekleri sağlar. Daha fazla bilgi için [ClaimsTransformations](claimstransformations.md).

## <a name="assertdatetimeisgreaterthan"></a>AssertDateTimeIsGreaterThan

Bir tarih ve saat (dize veri türü) ikinci tarihinden sonra ve saat (dize veri türü) talep talep denetler ve bir özel durum oluşturur.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | leftOperand | string | İlk talebin türü, ikinci talep daha sonra olmalıdır. |
| Inputclaim | Right | string | İlk talep önce olmalıdır talebin türü, ikinci. |
| InputParameter | AssertIfEqualTo | boole | Sol işlenen, sağ işlenenin eşitse, bu onay geçmelidir olup olmadığını belirtir. |
| InputParameter | AssertIfRightOperandIsNotPresent | boole | Sağ işlenen eksik olması durumunda, bu onay geçmelidir olup olmadığını belirtir. |
| InputParameter | TreatAsEqualIfWithinMillseconds | int | İki tür arasında izin vermek için milisaniye sayısını belirtir zamanları dikkate alınması gereken tarih saatleri eşit (örneğin, hesabı saat eğriltme için). |

**AssertDateTimeIsGreaterThan** talep dönüştürme gelen her zaman yürütülür bir [doğrulama teknik profili](validation-technical-profile.md) çağrılan bir [teknik profilSelfonaylanan](self-asserted-technical-profile.md). **DateTimeGreaterThan** otomatik olarak onaylanan teknik profil meta teknik profil kullanıcıya sunan hata iletisi denetler.

![AssertStringClaimsAreEqual execution](./media/date-transformations/assert-execution.png)

Aşağıdaki örnek `currentDateTime` ile talep `approvedDateTime` talep. Bir hata varsa durum `currentDateTime` saatinden sonra `approvedDateTime`. Dönüşüm değerlerini 5 dakika içinde (30000 milisaniye cinsinden) fark olmaları durumunda eşit olarak değerlendirir.

```XML
<ClaimsTransformation Id="AssertApprovedDateTimeLaterThanCurrentDateTime" TransformationMethod="AssertDateTimeIsGreaterThan">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="approvedDateTime" TransformationClaimType="leftOperand" />
    <InputClaim ClaimTypeReferenceId="currentDateTime" TransformationClaimType="rightOperand" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="AssertIfEqualTo" DataType="boolean" Value="false" />
    <InputParameter Id="AssertIfRightOperandIsNotPresent" DataType="boolean" Value="true" />
    <InputParameter Id="TreatAsEqualIfWithinMillseconds" DataType="int" Value="300000" />
  </InputParameters>
</ClaimsTransformation>
```

`login-NonInteractive` Doğrulama teknik profil çağrıları `AssertApprovedDateTimeLaterThanCurrentDateTime` talep dönüştürme.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertApprovedDateTimeLaterThanCurrentDateTime" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

Doğrulama otomatik olarak onaylanan teknik profil çağırır **etkileşimli olmayan oturum açma** teknik profili.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="DateTimeGreaterThan">Custom error message if the provided left operand is greater than the right operand.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **leftOperand**: 2018-10-01T15:00:00.0000000Z
    - **Right**: 2018-10-01T14:00:00.0000000Z
- Sonuç: Durum hatası

## <a name="convertdatetodatetimeclaim"></a>ConvertDateToDateTimeClaim

Dönüştürür bir **tarih** için ClaimType bir **DateTime** ClaimType. Talep dönüştürme saat biçimine dönüştürür ve 12:00:00 AM'den tarihe ekler.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | Inputclaim | date | Dönüştürülecek ClaimType. |
| outputClaim | outputClaim | Tarih/saat | Bu ClaimsTransformation çağrıldıktan sonra üreten ClaimType. |

Aşağıdaki örnek, talep dönüştürme gösterir `dateOfBirth` (date veri türü) için başka bir talep `dateOfBirthWithTime` (TarihSaat veri türü).

```XML
  <ClaimsTransformation Id="ConvertToDateTime" TransformationMethod="ConvertDateToDateTimeClaim">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="dateOfBirth" TransformationClaimType="inputClaim" />
    </InputClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="dateOfBirthWithTime" TransformationClaimType="outputClaim" />
    </OutputClaims>
  </ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **Inputclaim**: 2019-06-01
- Çıkış talep:
    - **outputClaim**: 1559347200 (1 Haziran 2019 12:00:00 AM)

## <a name="getcurrentdatetime"></a>GetCurrentDateTime

Geçerli UTC tarih ve saat alın ve değer için bir ClaimType ekleyin.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| outputClaim | currentDateTime | Tarih/saat | Bu ClaimsTransformation çağrıldıktan sonra üreten ClaimType. |

```XML
<ClaimsTransformation Id="GetSystemDateTime" TransformationMethod="GetCurrentDateTime">
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="systemDateTime" TransformationClaimType="currentDateTime" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

* Çıkış talep:
    * **currentDateTime**: 1534418820 (Ağustos 16 2018 11:27:00 AM)

## <a name="datetimecomparison"></a>DateTimeComparison

Bir dateTime daha sonra önceki ya da diğerine eşit olup olmadığını belirler. Yeni bir Boole ClaimType boolean değerini sonucudur `true` veya `false`.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | firstDateTime | Tarih/saat | Önceki veya daha sonraki bir ikinci tarih ve saat olup Karşılaştırılacak ilk tarih ve saat. Null değeri, bir özel durum oluşturur. |
| Inputclaim | secondDateTime | Tarih/saat | Önceki veya daha sonraki bir tarih ve saat ilk olup olmadığını Karşılaştırılacak ikinci tarih ve saat. Null değeri, geçerli datetTime kabul edilir. |
| InputParameter | İşleci | string | Aşağıdaki değerlerden biri: aynı, daha sonraki bir veya daha eski. |
| InputParameter | timeSpanInSeconds | int | İlk tarih ve saat için zaman aralığını ekleyin. |
| outputClaim | Sonuç | boole | Bu ClaimsTransformation çağrıldıktan sonra üreten ClaimType. |

Bu talep dönüştürme eşit, sonra veya daha önceki her iki ClaimTypes olup olmadığını belirlemek için kullanın. Örneğin, bir kullanıcı Hizmetleri (TOS) koşullarınızı kabul en son ne zaman depolayabilir. 3 ay sonra kullanıcının yeniden TOS erişmesine izin isteyebilir.
Talep dönüştürmeyi çalıştırmak için öncelikle geçerli tarih ve saat almanız ve son zamanı kullanıcı TOS de kabul eder.

```XML
<ClaimsTransformation Id="CompareLastTOSAcceptedWithCurrentDateTime" TransformationMethod="DateTimeComparison">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="currentDateTime" TransformationClaimType="firstDateTime" />
    <InputClaim ClaimTypeReferenceId="extension_LastTOSAccepted" TransformationClaimType="secondDateTime" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="operator" DataType="string" Value="later than" />
    <InputParameter Id="timeSpanInSeconds" DataType="int" Value="7776000" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isLastTOSAcceptedGreaterThanNow" TransformationClaimType="result" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **firstDateTime**: 2018-01-01T00:00:00.100000Z
    - **secondDateTime**: 2018-04-01T00:00:00.100000Z
- Giriş parametreleri:
    - **İşleç**: daha
    - **timeSpanInSeconds**: 7776000 (90 gün)
- Çıkış talep:
    - **Sonuç**: true
