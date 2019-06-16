---
title: Kimlik deneyimi çerçevesi şema Azure Active Directory B2C için JSON talep dönüştürme örnekler | Microsoft Docs
description: JSON dönüşüm örnekleri kimlik deneyimi çerçevesi şema, Azure Active Directory B2C için talep.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 9a026d205d3ab855ecbb51048e7464df6fb4a094
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66510760"
---
# <a name="json-claims-transformations"></a>JSON dönüştürmeleri talep

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory (Azure AD) B2C'de kimlik deneyimi çerçevesi şema JSON talep dönüştürmeleri kullanma örnekleri sağlar. Daha fazla bilgi için [ClaimsTransformations](claimstransformations.md).

## <a name="getclaimfromjson"></a>GetClaimFromJson

Belirtilen öğeyi bir JSON veri alın.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | inputJson | string | Talep dönüştürme tarafından öğesini almak için kullanılan ClaimTypes. |
| InputParameter | claimToExtract | string | Ayıklanacak JSON öğesi adı. |
| outputClaim | extractedClaim | string | Bu dönüşüm talep sonra üreten ClaimType çağırıldı, öğe değeri belirtilen _claimToExtract_ giriş parametresi. |

Aşağıdaki örnekte, talep dönüştürme ayıklanan `emailAddress` JSON verilerini öğesinden: `{"emailAddress": "someone@example.com", "displayName": "Someone"}`

```XML
<ClaimsTransformation Id="GetEmailClaimFromJson" TransformationMethod="GetClaimFromJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="customUserData" TransformationClaimType="inputJson" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="claimToExtract" DataType="string" Value="emailAddress" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="extractedEmail" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **inputJson**: {"emailAddress": "someone@example.com", "displayName": "Kişi"}
- Giriş parametresi:
    - **claimToExtract**: emailAddress
- Çıkış talep: 
  - **extractedClaim**: someone@example.com


## <a name="getclaimsfromjsonarray"></a>GetClaimsFromJsonArray

Belirtilen öğelerin listesini Json verilerinizden yararlanın.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | jsonSourceClaim | string | Talep dönüştürme tarafından olan talepleri almak için kullanılan ClaimTypes. |
| InputParameter | errorOnMissingClaims | boole | Taleplerinden biri eksikse bir hata harekete geçirileceğini belirtir. |
| InputParameter | includeEmptyClaims | string | Boş talep eklenip eklenmeyeceğini belirtin. |
| InputParameter | jsonSourceKeyName | string | Öğe anahtarı adı |
| InputParameter | jsonSourceValueName | string | Öğe değer adı |
| outputClaim | Koleksiyon | dize, int, boolean ve datetime |Ayıklanacak talep listesi. Talep adı belirtilen birine eşit olmalıdır _jsonSourceClaim_ giriş talep. |

Aşağıdaki örnekte, talep dönüştürme aşağıdaki talep ayıklar: e-posta (dize), displayName (dize), membershipNum (int), etkin (boolean) ve doğum tarihi (TarihSaat) JSON verilerinden.

```JSON
[{"key":"email","value":"someone@example.com"}, {"key":"displayName","value":"Someone"}, {"key":"membershipNum","value":6353399}, {"key":"active","value":true}, {"key":"birthdate","value":"1980-09-23T00:00:00Z"}]
```

```XML
<ClaimsTransformation Id="GetClaimsFromJson" TransformationMethod="GetClaimsFromJsonArray">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="jsonSourceClaim" TransformationClaimType="jsonSource" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="errorOnMissingClaims" DataType="boolean" Value="false" />
    <InputParameter Id="includeEmptyClaims" DataType="boolean" Value="false" />
    <InputParameter Id="jsonSourceKeyName" DataType="string" Value="key" />
    <InputParameter Id="jsonSourceValueName" DataType="string" Value="value" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="membershipNum" />
    <OutputClaim ClaimTypeReferenceId="active" />
    <OutputClaim ClaimTypeReferenceId="birthdate" />
  </OutputClaims>
</ClaimsTransformation>
```    

- Giriş talepleri:
  - **jsonSourceClaim**: [{"key": "email", "value": "someone@example.com"}, {"anahtarını": "displayName", "value": "Kişi"}, {"anahtarını": "membershipNum", "value": 6353399}, {"anahtarını": "etkin", "value": true}, {"anahtarını": "Doğum", "value": "1980-09-23T00:0 0:00Z"}]
- Giriş parametreleri:
    - **errorOnMissingClaims**: false
    - **includeEmptyClaims**: false
    - **jsonSourceKeyName**: anahtar
    - **jsonSourceValueName**: değer
- Çıkış talep:
  - **e-posta**: "someone@example.com"
  - **displayName**: "Kişi"
  - **membershipNum**: 6353399
  - **Etkin**: true
  - **Doğum tarihi**: 1980-09-23T00:00:00Z

## <a name="getnumericclaimfromjson"></a>GetNumericClaimFromJson

Belirtilen bir sayısal (uzun) öğesi bir JSON verilerini alır.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | inputJson | string | Talep dönüştürme tarafından talep almak için kullanılan ClaimTypes. |
| InputParameter | claimToExtract | string | Ayıklanacak JSON öğesi adı. |
| outputClaim | extractedClaim | long | Öğesinin değeri belirtilen bu ClaimsTransformation çağrıldıktan sonra üreten ClaimType _claimToExtract_ giriş parametreleri. |

Aşağıdaki örnekte, talep dönüştürme ayıklar `id` JSON verilerini öğesinden.

```JSON
{
    "emailAddress": "someone@example.com", 
    "displayName": "Someone", 
    "id" : 6353399
}
```

```XML
<ClaimsTransformation Id="GetIdFromResponse" TransformationMethod="GetNumericClaimFromJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="exampleInputClaim" TransformationClaimType="inputJson" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="claimToExtract" DataType="string" Value="id" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="membershipId" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **inputJson**: {"emailAddress": "someone@example.com", "displayName": "Kişi", "id": 6353399}
- Giriş parametreleri
    - **claimToExtract**: kimliği
- Çıkış talep: 
    - **extractedClaim**: 6353399

## <a name="getsinglevaluefromjsonarray"></a>GetSingleValueFromJsonArray

Bir JSON veri diziden ilk öğeyi alır.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | inputJsonClaim | string | Talep dönüştürme tarafından JSON diziden öğesini almak için kullanılan ClaimTypes. |
| outputClaim | extractedClaim | string | Bu ClaimsTransformation çağrıldıktan sonra üreten ClaimType, JSON dizideki ilk öğe. |

Aşağıdaki örnekte, talep dönüştürme JSON diziden ilk öğeyi (e-posta adresi) ayıklar `["someone@example.com", "Someone", 6353399]`.

```XML
<ClaimsTransformation Id="GetEmailFromJson" TransformationMethod="GetSingleValueFromJsonArray">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userData" TransformationClaimType="inputJsonClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **inputJsonClaim**: ["someone@example.com", "Kişi", 6353399]
- Çıkış talep: 
  - **extractedClaim**: someone@example.com

## <a name="xmlstringtojsonstring"></a>XmlStringToJsonString

XML verilerini JSON biçimine dönüştürür.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | xml | string | Talep dönüştürme tarafından verileri XML'den JSON biçimine dönüştürmek için kullanılan ClaimTypes. |
| outputClaim | json | string | Bu ClaimsTransformation çağrıldıktan sonra üreten ClaimType, verileri JSON biçiminde. |

```XML
<ClaimsTransformation Id="ConvertXmlToJson" TransformationMethod="XmlStringToJsonString">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="intpuXML" TransformationClaimType="xml" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="outputJson" TransformationClaimType="json" />
  </OutputClaims>
</ClaimsTransformation>
```

Aşağıdaki örnekte, talep dönüştürme, aşağıdaki XML verilerini JSON biçimine dönüştürür.

#### <a name="example"></a>Örnek
Giriş talep:

```XML
<user>
  <name>Someone</name>
  <email>someone@example.com</email>
</user>
```

Çıkış talep:

```JSON
{
  "user": {
    "name":"Someone",
    "email":"someone@example.com"
  }
}
```

