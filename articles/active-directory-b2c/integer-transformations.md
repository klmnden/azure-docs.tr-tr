---
title: Kimlik deneyimi çerçevesi şema Azure Active Directory B2C için tamsayı talep dönüştürme örnekler | Microsoft Docs
description: Dönüşüm örnekleri, tamsayı kimlik deneyimi çerçevesi şema, Azure Active Directory B2C için talep.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 976cf55dd8f1c11d1ea605b25086fa644afec980
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66510813"
---
# <a name="integer-claims-transformations"></a>Tamsayı dönüştürmeleri talep

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory (Azure AD) B2C'de kimlik deneyimi çerçevesi şema tamsayı talep dönüştürmeleri kullanma örnekleri sağlar. Daha fazla bilgi için [ClaimsTransformations](claimstransformations.md).

## <a name="convertnumbertostringclaim"></a>ConvertNumberToStringClaim 

Long veri türü bir dize veri türüne dönüştürür.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | Inputclaim | long | Bir dizeye dönüştürmek için ClaimType. |
| outputClaim | outputClaim | string | Bu ClaimsTransformation çağrıldıktan sonra üreten ClaimType. |

Bu örnekte, `numericUserId` uzun bir değer türü olan talep dönüştürülen bir `UserId` talep dize ile bir değer türü.

```XML
<ClaimsTransformation Id="CreateUserId" TransformationMethod="ConvertNumberToStringClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="numericUserId" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="UserId" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **Inputclaim**: 12334 (uzun)
- Çıkış talep: 
    - **outputClaim**: "12334" (dize)

