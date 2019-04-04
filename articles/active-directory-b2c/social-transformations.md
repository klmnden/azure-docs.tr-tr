---
title: Sosyal hesap kimlik deneyimi çerçevesi şema Azure Active Directory B2C için dönüşüm örnekleri talep | Microsoft Docs
description: Sosyal hesap kimlik deneyimi çerçevesi şema, Azure Active Directory B2C için dönüşüm örnekleri talepleri.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 53608654392d7efb73b6dadac14f01a94bb035a7
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893529"
---
# <a name="social-accounts-claims-transformations"></a>Sosyal medya hesaplarını talep dönüştürmeleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Sosyal hesap kimlikleri Azure Active Directory (Azure AD) B2C'de depolanan bir `userIdentities` özniteliği bir **alternativeSecurityIdCollection** talep türü. Her öğe **alternativeSecurityIdCollection** veren (kimlik sağlayıcı adı, facebook.com gibi) belirtir ve `issuerUserId`, dağıtımcı için benzersiz kullanıcı tanımlayıcısı olan.

```JSON
"userIdentities": [{
    "issuer": "google.com",
    "issuerUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw"
  },
  {
    "issuer": "facebook.com",
    "issuerUserId": "MTIzNDU="
  }]
```

Bu makalede, Azure AD B2C'de kimlik deneyimi çerçevesi şema sosyal hesap talep dönüştürmeleri kullanma örnekleri sağlar. Daha fazla bilgi için [ClaimsTransformations](claimstransformations.md).

## <a name="createalternativesecurityid"></a>CreateAlternativeSecurityId

Azure Active Directory çağrılarındaki kullanılabilir kullanıcının alternativeSecurityId özelliği JSON temsilini oluşturur. Daha fazla bilgi için [AlternativeSecurityId'ın şema](/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#AlternativeSecurityIdType).

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | anahtar | string | ClaimType sosyal kimlik sağlayıcısı tarafından kullanılan benzersiz kullanıcı tanımlayıcısını belirtir. |
| Inputclaim | identityProvider | string | ClaimType facebook.com gibi sosyal hesap kimlik sağlayıcısı adını belirtir. |
| outputClaim | alternativeSecurityId | string | ClaimsTransformation çağrıldıktan sonra üreten ClaimType. Sosyal hesap kullanıcının kimlik bilgilerini içerir. **Veren** değeri `identityProvider` talep. **İssuerUserId** değeri `key` base64 biçiminde talep. |

Bu talep oluşturmak için kullanmak bir `alternativeSecurityId` ClaimType. Tüm sosyal kimlik sağlayıcısı teknik profiller tarafından gibi kullanılır `Facebook-OAUTH`. Aşağıdaki talep dönüştürme kullanıcı sosyal hesap kimliği ve kimlik sağlayıcısı adını alır. Bu teknik profili çıktısını Azure AD directory hizmetlerinde kullanılabilecek bir JSON dizesi biçimidir.

```XML
<ClaimsTransformation Id="CreateAlternativeSecurityId" TransformationMethod="CreateAlternativeSecurityId">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="socialIdpUserId" TransformationClaimType="key" />
    <InputClaim ClaimTypeReferenceId="identityProvider" TransformationClaimType="identityProvider" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="alternativeSecurityId" TransformationClaimType="alternativeSecurityId" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **Anahtar**: 12334
    - **Identityprovider**: Facebook.com
- Çıkış talep:
    - **alternativeSecurityId**: {"Issuer": "facebook.com", "issuerUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw"}

## <a name="additemtoalternativesecurityidcollection"></a>AddItemToAlternativeSecurityIdCollection

Ekler bir `AlternativeSecurityId` için bir `alternativeSecurityIdCollection` talep.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | Öğesi | string | Çıkış talebi için eklenecek ClaimType. |
| Inputclaim | koleksiyon | alternativeSecurityIdCollection | Tarafından talep Dönüştürme ilkesi varsa kullanılan ClaimTypes. Sağlanırsa, talep dönüştürme ekler `item` koleksiyonun sonuna. |
| outputClaim | koleksiyon | alternativeSecurityIdCollection | Bu ClaimsTransformation çağrıldıktan sonra üretilen ClaimTypes. Her iki giriş öğeleri içeren yeni bir koleksiyon `collection` ve `item`. |

Aşağıdaki örnek yeni bir sosyal kimlik var olan bir hesapla bağlar. Yeni bir sosyal kimlik bağlamak için:
1. İçinde **AAD UserReadUsingAlternativeSecurityId** ve **AAD UserReadUsingObjectId** teknik profiller, kullanıcının çıkış **Alternativesecurityıds** talep.
1. Bu kullanıcı ile ilişkili olmayan kimlik sağlayıcılarından birini oturum için kullanıcıya sor.
1. Kullanarak **CreateAlternativeSecurityId** talep dönüştürme, yeni bir **alternativeSecurityId** talep türü adı `AlternativeSecurityId2`
1. Çağrı **AddItemToAlternativeSecurityIdCollection** eklemek için dönüştürme talep **AlternativeSecurityId2** varolan talep **Alternativesecurityıds** Talep.
1. Kalıcı **Alternativesecurityıds** kullanıcı hesabına talep

```XML
<ClaimsTransformation Id="AddAnotherAlternativeSecurityId" TransformationMethod="AddItemToAlternativeSecurityIdCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityId2" TransformationClaimType="item" />
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityIds" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="AlternativeSecurityIds" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **öğe**: {"Issuer": "facebook.com", "issuerUserId": "MTIzNDU ="}
    - **koleksiyon**: [{"Issuer": "live.com", "issuerUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw"}]
- Çıkış talep:
    - **koleksiyon**: [{"Issuer": "live.com", "issuerUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw"}, {"Issuer": "facebook.com", "issuerUserId": "MTIzNDU ="}]

## <a name="getidentityprovidersfromalternativesecurityidcollectiontransformation"></a>GetIdentityProvidersFromAlternativeSecurityIdCollectionTransformation

Verenler listesini döndürür **alternativeSecurityIdCollection** yeni bir talep **stringCollection** talep.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | alternativeSecurityIdCollection | alternativeSecurityIdCollection | Kimlik sağlayıcıları (veren) listesini almak için kullanılan ClaimType. |
| outputClaim | identityProvidersCollection | StringCollection | Bu ClaimsTransformation çağrıldıktan sonra üretilen ClaimTypes. Kimlik sağlayıcılarının listesi alternativeSecurityIdCollection giriş talep ile ilişkilendirme |

Aşağıdaki talep dönüştürme kullanıcı okur **Alternativesecurityıds** talebi ve kimlik sağlayıcısı adlarının listesi, ilgili hesapla ilişkili ayıklar. Çıkış kullan **identityProvidersCollection** kullanıcı hesabıyla ilişkili kimlik sağlayıcılarının listesi göstermek için. Veya, çıktıya dayalı kimlik sağlayıcılarının listesi kimlik sağlayıcısı seçim sayfası üzerinde filtre **identityProvidersCollection** talep. Bu nedenle, kullanıcı hesabıyla ilişkilendirilmemiş yeni sosyal kimlik bağlantısını seçin.

```XML
<ClaimsTransformation Id="ExtractIdentityProviders" TransformationMethod="GetIdentityProvidersFromAlternativeSecurityIdCollectionTransformation">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="alternativeSecurityIds" TransformationClaimType="alternativeSecurityIdCollection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="identityProviders" TransformationClaimType="identityProvidersCollection" />
  </OutputClaims>
</ClaimsTransformation>
```

- Giriş talepleri:
    - **alternativeSecurityIdCollection**: [ { "issuer": "google.com", "issuerUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw"}, {"Issuer": "facebook.com", "issuerUserId": "MTIzNDU ="}]
- Çıkış talep:
    - **identityProvidersCollection**: [ "facebook.com", "google.com" ]

## <a name="removealternativesecurityidbyidentityprovider"></a>RemoveAlternativeSecurityIdByIdentityProvider

Kaldırır bir **AlternativeSecurityId** gelen bir **alternativeSecurityIdCollection** talep.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | identityProvider | string | Koleksiyondan kaldırılacak kimlik sağlayıcısı adını içeren ClaimType. |
| Inputclaim | koleksiyon | alternativeSecurityIdCollection | Talep dönüştürme tarafından kullanılan ClaimTypes. Talep dönüştürme Identityprovider koleksiyondan kaldırır. |
| outputClaim | koleksiyon | alternativeSecurityIdCollection | Bu ClaimsTransformation çağrıldıktan sonra üretilen ClaimTypes. Identityprovider koleksiyonundan kaldırdıktan sonra yeni toplama. |

Aşağıdaki örnek bir sosyal kimlik var olan bir hesapla bağlantısını kaldırır. Sosyal kimlik bağlantısını kaldırmak için:
1. İçinde **AAD UserReadUsingAlternativeSecurityId** ve **AAD UserReadUsingObjectId** teknik profiller, kullanıcının çıkış **Alternativesecurityıds** talep.
2. Bu kullanıcıyla ilişkilendirilmiş listesi kimlik sağlayıcıları kaldırmak için hangi sosyal hesap seçmesini isteyin.
3. Çağıran bir talep dönüştürme teknik profili çağrı **RemoveAlternativeSecurityIdByIdentityProvider** kimlik sağlayıcısı adını kullanarak seçili bir sosyal kimlik kaldırıldı, dönüştürme talep.
4. Kalıcı **Alternativesecurityıds** kullanıcı hesabına talep.

```XML
<ClaimsTransformation Id="RemoveAlternativeSecurityIdByIdentityProvider" TransformationMethod="RemoveAlternativeSecurityIdByIdentityProvider">
    <InputClaims>
        <InputClaim ClaimTypeReferenceId="secondIdentityProvider" TransformationClaimType="identityProvider" />
        <InputClaim ClaimTypeReferenceId="AlternativeSecurityIds" TransformationClaimType="collection" />
    </InputClaims>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="AlternativeSecurityIds" TransformationClaimType="collection" />
    </OutputClaims>
</ClaimsTransformation>
</ClaimsTransformations>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **Identityprovider**: facebook.com
    - **koleksiyon**: [{"Issuer": "live.com", "issuerUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw"}, {"Issuer": "facebook.com", "issuerUserId": "MTIzNDU ="}]
- Çıkış talep:
    - **koleksiyon**: [{"Issuer": "live.com", "issuerUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw"}]
