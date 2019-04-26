---
title: ClaimsTransformations - Azure Active Directory B2C | Microsoft Docs
description: Kimlik deneyimi çerçevesi şema, Azure Active Directory B2C ClaimsTransformations öğe tanımı.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: bc6cc7b07d3dce43a666b3e5b0a958b41cdd3131
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60313826"
---
# <a name="claimstransformations"></a>ClaimsTransformations

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**ClaimsTransformations** öğesi içinde kullanıcı yolculuklarından parçası olarak kullanılabilecek talep dönüştürme işlevleri listesini içeren bir [özel ilke](active-directory-b2c-overview-custom.md). Talep dönüştürme içinde başka bir belirtilen talep dönüştürür. Talep dönüştürme örneğin bir dize koleksiyona bir öğe eklemeye veya bir dizenin biçimi değiştirme dönüştürme yöntemini belirtin.

Kullanıcı yolculuklarından içinde kullanılabilecek talep dönüştürme işlevlerin listesi eklemek için ilke BuildingBlocks bölümü altında ClaimsTransformations XML öğesi bildirilmelidir.

```xml
<ClaimsTransformations>
  <ClaimsTransformation Id="<identifier>" TransformationMethod="<method>">
    ...
  </ClaimsTransformation>
</ClaimsTransformations>
```

**ClaimsTransformation** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik |Gerekli | Açıklama |
| --------- |-------- | ----------- |
| Kimlik |Evet | Talep dönüştürme benzersiz şekilde tanımlamak için kullanılan tanımlayıcıdır. Tanımlayıcı, ilke içinde diğer XML öğelerden başvuruluyor. |
| TransformationMethod | Evet | Talep dönüştürme içinde kullanılacak dönüştürme yöntemi. Her talep dönüştürmeyi kendi değerlerine sahip. Bkz: [talep dönüştürme başvuru](#claims-transformations-reference) kullanılabilir değerlerin tam listesi için. |

## <a name="claimstransformation"></a>ClaimsTransformation

**ClaimsTransformation** öğesi aşağıdaki öğeleri içerir:

```xml
<ClaimsTransformation Id="<identifier>" TransformationMethod="<method>">
  <InputClaims>
    ...
  </InputClaims>
  <InputParameters>
    ...
  </InputParameters>                
  <OutputClaims>
    ...
  </OutputClaims>
</ClaimsTransformation>
```


| Öğe | Oluşumlar | Açıklama |
| ------- | -------- | ----------- |
| InputClaims | 0:1 | Listesini **Inputclaim** öğesi olarak gerçekleştirilen talep türlerini belirten talep dönüştürme için giriş. Bu öğelerin her biri, ilkeyi ClaimsSchema bölümünde önceden tanımlanmış bir ClaimType başvuru içeriyor. |
| InputParameters | 0:1 | Listesini **InputParameter** talep dönüştürme için giriş olarak sağlanan öğeleri.  
| OutputClaims | 0:1 | Listesini **OutputClaim** talep türlerini belirtme öğelerini ClaimsTransformation çağrıldıktan sonra oluşturulur. Bu öğelerin her biri bir ClaimType ClaimsSchema bölümünde önceden tanımlı başvuru içeriyor. |

### <a name="inputclaims"></a>InputClaims

**InputClaims** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| Inputclaim | 1:n | Beklenen bir giriş talep türü. |

#### <a name="inputclaim"></a>Inputclaim

**Inputclaim** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik |Gerekli | Açıklama |
| --------- | ----------- | ----------- |
| ClaimTypeReferenceId |Evet | İlke ClaimsSchema bölümünde önceden tanımlanmış bir ClaimType başvuru. |
| TransformationClaimType |Evet | Bir dönüştürme başvurmak için bir tanımlayıcı talep türü. Her talep dönüştürmeyi kendi değerlerine sahip. Bkz: [talep dönüştürme başvuru](#claims-transformations-reference) kullanılabilir değerlerin tam listesi için. |

### <a name="inputparameters"></a>InputParameters

**InputParameters** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| InputParameter | 1:n | Beklenen bir giriş parametresi. |

#### <a name="inputparameter"></a>InputParameter

| Öznitelik | Gerekli |Açıklama |
| --------- | ----------- |----------- |
| Kimlik | Evet | Talep dönüştürme yönteminin bir parametresi için bir başvuru bir tanımlayıcı. Her talep dönüştürme yöntemi kendi değerlerine sahip. Talep dönüştürme kullanılabilir değerlerin tam listesi için bkz. |
| DataType | Evet | Parametresinin dize, Boolean, tamsayı veya tarih/saat gibi özel ilke XML şema veri türü numaralandırması kabul veri türü. Bu türü aritmetik işlemler gerçekleştirmek için kullanılır. Her talep dönüştürmeyi kendi değerlerine sahip. Bkz: [talep dönüştürme başvuru](#claims-transformations-reference) kullanılabilir değerlerin tam listesi için. |
| Değer | Evet | Dönüştürme için verbatim geçirilen değer. Bazı değerler rastgele, bunlardan bazıları talep dönüştürme yöntemi seçin. |

### <a name="outputclaims"></a>OutputClaims

**OutputClaims** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| outputClaim | 0: n | Beklenen bir çıkış talep türü. |

#### <a name="outputclaim"></a>outputClaim 

**OutputClaim** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik |Gerekli | Açıklama |
| --------- | ----------- |----------- |
| ClaimTypeReferenceId | Evet | İlke ClaimsSchema bölümünde önceden tanımlanmış bir ClaimType başvuru.
| TransformationClaimType | Evet | Bir dönüştürme başvurmak için bir tanımlayıcı talep türü. Her talep dönüştürmeyi kendi değerlerine sahip. Bkz: [talep dönüştürme başvuru](#claims-transformations-reference) kullanılabilir değerlerin tam listesi için. |
 
Giriş talep ve çıkış talep türdeki (dize veya boolean) ise, aynı giriş talep çıkış talep kullanabilirsiniz. Bu durumda, talep dönüştürme giriş talep çıkış değeri ile değiştirir.

## <a name="example"></a>Örnek

Örneğin, son kullanıcı kabul Hizmetleri koşullarınızın sürümünü depolayabilir. Hizmetler koşulları güncelleştirdiğinizde, yeni sürümü kabul etmesi sorabilirsiniz. Aşağıdaki örnekte, **HasTOSVersionChanged** talep dönüştürme değerini karşılaştırır **TOSVersion** talep değeriyle **LastTOSAcceptedVersion**talep ve boolean döndürür **TOSVersionChanged** talep.

```XML
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="TOSVersionChanged">
      <DisplayName>Indicates if the TOS version accepted by the end user is equal to the current version</DisplayName>
      <DataType>boolean</DataType>
    </ClaimType>
    <ClaimType Id="TOSVersion">
      <DisplayName>TOS version</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
    <ClaimType Id="LastTOSAcceptedVersion">
      <DisplayName>TOS version accepted by the end user</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
  </ClaimsSchema>

  <ClaimsTransformations>
    <ClaimsTransformation Id="HasTOSVersionChanged" TransformationMethod="CompareClaims">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="TOSVersion" TransformationClaimType="inputClaim1" />
        <InputClaim ClaimTypeReferenceId="LastTOSAcceptedVersion" TransformationClaimType="inputClaim2" />
      </InputClaims>
      <InputParameters>
        <InputParameter Id="operator" DataType="string" Value="NOT EQUAL" />
      </InputParameters>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="TOSVersionChanged" TransformationClaimType="outputClaim" />
      </OutputClaims>
    </ClaimsTransformation>
  </ClaimsTransformations>
</BuildingBlocks>
```

## <a name="claims-transformations-reference"></a>Talep dönüştürmeleri referans

Talep dönüştürmeleri örnekleri için aşağıdaki başvuru sayfalarına bakın:

- [Boole değeri](boolean-transformations.md)
- [Tarih](date-transformations.md)
- [tamsayı](integer-transformations.md)
- [JSON](json-transformations.md)
- [Genel](general-transformations.md)
- [Sosyal hesap](social-transformations.md)
- [dize](string-transformations.md)
- [StringCollection](stringcollection-transformations.md)

