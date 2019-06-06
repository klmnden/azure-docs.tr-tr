---
title: Kimlik deneyimi çerçevesi şema Azure Active Directory B2C için StringCollection talep dönüştürme örnekler | Microsoft Docs
description: StringCollection dönüşüm örnekleri kimlik deneyimi çerçevesi şema, Azure Active Directory B2C için talep.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 98453daeb34d093b49cdcc636f68c3d7ae017126
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66512430"
---
# <a name="stringcollection-claims-transformations"></a>StringCollection talep dönüşümleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory (Azure AD) B2C'de kimlik deneyimi çerçevesi şema dize koleksiyonu talep Dönüşümleri kullanma örnekleri sağlar. Daha fazla bilgi için [ClaimsTransformations](claimstransformations.md).

## <a name="additemtostringcollection"></a>AddItemToStringCollection

Yeni bir stringCollection talep dize talep ekler. 

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | Öğesi | string | Çıkış talebi için eklenecek ClaimType. |
| Inputclaim | Koleksiyon | StringCollection | [İsteğe bağlı] Belirtilmişse, talep dönüştürme bu koleksiyondan öğelerin kopyalar ve çıkış koleksiyon talep sonuna öğe ekler. |
| outputClaim | Koleksiyon | StringCollection | Bu ClaimsTransformation çağrıldıktan sonra üretilen ClaimTypes. |

Kullanım bu dönüştürme için yeni veya mevcut bir stringCollection bir dize eklemek için talep. İçinde yaygın olarak kullanılan bir **AAD UserWriteUsingAlternativeSecurityId** teknik profili. Yeni bir sosyal hesap oluşturulmadan önce **CreateOtherMailsFromEmail** talep dönüştürme ClaimType okur ve değerine ekler **otherMails** ClaimType. 

Aşağıdaki talep dönüştürme ekler **e-posta** için ClaimType **otherMails** ClaimType.

```XML
<ClaimsTransformation Id="CreateOtherMailsFromEmail" TransformationMethod="AddItemToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="item" />
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **koleksiyon**: ["someone@outlook.com"]
  - **öğe**: "admin@contoso.com"
- Çıkış talep: 
  - **koleksiyon**: ["someone@outlook.com","admin@contoso.com"]

## <a name="addparametertostringcollection"></a>AddParameterToStringCollection

Bir dize parametresi için yeni bir stringCollection talep ekler. 

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | Koleksiyon | StringCollection | [İsteğe bağlı] Belirtilmişse, talep dönüştürme bu koleksiyondan öğelerin kopyalar ve çıkış koleksiyon talep sonuna öğe ekler. |
| InputParameter | Öğesi | string | Çıkış talebi için eklenecek değer. |
| outputClaim | Koleksiyon | StringCollection | Bu ClaimsTransformation çağrıldıktan sonra üretilecek ClaimTypes. |

Kullanım bu talep dönüştürmeyi için yeni veya mevcut bir stringCollection bir dize değeri ekleyin. Aşağıdaki örnek, bir sabit bir e-posta adresini ekler (admin@contoso.com) için **otherMails** talep. 

```XML
<ClaimsTransformation Id="SetCompanyEmail" TransformationMethod="AddParameterToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="item" DataType="string" Value="admin@contoso.com" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **koleksiyon**: ["someone@outlook.com"]
- Giriş parametreleri 
  - **öğe**: "admin@contoso.com"
- Çıkış talep:
  - **koleksiyon**: ["someone@outlook.com","admin@contoso.com"]

## <a name="getsingleitemfromstringcollection"></a>GetSingleItemFromStringCollection

İlk öğe, sağlanan dizenin koleksiyondan alır. 

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | Koleksiyon | StringCollection | Talep dönüştürme tarafından öğesini almak için kullanılan ClaimTypes. |
| outputClaim | extractedItem | string | Bu ClaimsTransformation çağrıldıktan sonra üretilen ClaimTypes. Koleksiyondaki ilk öğe. |

Aşağıdaki örnek okur **otherMails** talep ve ilk öğesine dönüş **e-posta** talep. 

```XML
<ClaimsTransformation Id="CreateEmailFromOtherMails" TransformationMethod="GetSingleItemFromStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="extractedItem" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **koleksiyon**: ["someone@outlook.com","someone@contoso.com"]
- Çıkış talep: 
  - **extractedItem**: "someone@outlook.com"

