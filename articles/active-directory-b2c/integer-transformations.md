---
title: Kimlik deneyimi çerçevesi şema Azure Active Directory B2C için tamsayı talep dönüştürme örnekler | Microsoft Docs
description: Dönüşüm örnekleri, tamsayı kimlik deneyimi çerçevesi şema, Azure Active Directory B2C için talep.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 358ee07b8fd32edded084d406e490cae9f557fdd
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55159531"
---
# <a name="integer-claims-transformations"></a>Tamsayı dönüştürmeleri talep

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory (Azure AD) B2C'de kimlik deneyimi çerçevesi şema tamsayı talep dönüştürmeleri kullanma örnekleri sağlar. Daha fazla bilgi için [ClaimsTransformations](claimstransformations.md).

## <a name="convertnumbertostringclaim"></a>ConvertNumberToStringClaim 

Long veri türü bir dize veri türüne dönüştürür.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | Inputclaim | uzun | Bir dizeye dönüştürmek için ClaimType. |
| outputClaim | outputClaim | dize | Bu ClaimsTransformation çağrıldıktan sonra üreten ClaimType. |

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

