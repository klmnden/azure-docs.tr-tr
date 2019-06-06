---
title: Kimlik deneyimi çerçevesi şema, Azure Active Directory B2C için genel talep dönüştürme örnekler | Microsoft Docs
description: Kimlik deneyimi çerçevesi şema, Azure Active Directory B2C için genel talep dönüştürme örnekleri.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: a5f8068ea7e97343749c719d2d0800e20701079c
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66510996"
---
# <a name="general-claims-transformations"></a>Genel talep dönüştürmeleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory (Azure AD) B2C'de kimlik deneyimi çerçevesi şema genel talep dönüştürmeleri kullanma örnekleri sağlar. Daha fazla bilgi için [ClaimsTransformations](claimstransformations.md).

## <a name="doesclaimexist"></a>DoesClaimExist

Denetler **Inputclaim** veya mevcut olduğundan ve ayarlar **outputClaim** true veya false uygun şekilde.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | Inputclaim |Tüm | Giriş talep, varlığı doğrulanması gerekiyor. |
| outputClaim | outputClaim | boole | Bu ClaimsTransformation çağrıldıktan sonra üreten ClaimType. |

Bu dönüşüm talebi yok veya herhangi bir değer içeren denetlemek için talep kullanın. Talep var olup olmadığını belirten Boolean bir değer dönüş değeridir. Aşağıdaki örnek, e-posta adresi var olup olmadığını denetler.

```XML
<ClaimsTransformation Id="CheckIfEmailPresent" TransformationMethod="DoesClaimExist">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim" />
  </InputClaims>                    
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isEmailPresent" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **Inputclaim**: someone@contoso.com
- Çıkış talep: 
    - **outputClaim**: true

## <a name="hash"></a>Karma

Güvenlik değeri ve bir gizli dizi kullanarak sağlanan düz metin karma.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | düz metin | string | Şifrelenecek giriş talep |
| Inputclaim | güvenlik değeri | string | Anahtar güvenlik değerinin parametre. Oluşturabileceğiniz rastgele bir sıra değerini kullanarak `CreateRandomString` talep dönüştürme. |
| InputParameter | randomizerSecret | string | Mevcut bir Azure AD B2C işaret **ilke anahtarları**. Yeni bir tane oluşturmak için: Azure AD B2C kiracınızı seçin **B2C Ayarları > kimlik deneyimi çerçevesi**. Seçin **ilke anahtarları** kiracınızda kullanılabilir tuşlarını görmek için. **Add (Ekle)** seçeneğini belirleyin. İçin **seçenekleri**seçin **el ile**. (Önek B2C_1A_ otomatik olarak eklenebilir.) bir ad sağlayın. Gizli kutusuna, gibi 1234567890 kullanmak istediğiniz herhangi bir gizli dizi girin. Anahtar kullanımı için seçin **gizli**. **Oluştur**’u seçin. |
| outputClaim | Karma | string | Bu dönüşüm talep sonra üreten ClaimType çağrılmış. Yapılandırılmış talep `plaintext` Inputclaim. |

```XML
<ClaimsTransformation Id="HashPasswordWithEmail" TransformationMethod="Hash">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="password" TransformationClaimType="plaintext" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="salt" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="randomizerSecret" DataType="string" Value="B2C_1A_AccountTransformSecret" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="hashedPassword" TransformationClaimType="hash" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **düz metin**: MyPass@word1
    - **Salt**: 487624568
    - **randomizerSecret**: B2C_1A_AccountTransformSecret
- Çıkış talep: 
    - **outputClaim**: CdMNb/KTEfsWzh9MR1kQGRZCKjuxGMWhA5YQNihzV6U=



