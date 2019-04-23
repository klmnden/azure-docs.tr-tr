---
title: Azure Active Directory B2C, özel bir ilkede OAuth1 teknik profili tanımlama | Microsoft Docs
description: Azure Active Directory B2C, özel bir ilkede OAuth1 teknik profili tanımlayın.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 3fe839de8cbaa0b321b0b0602b000b7575224dde
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60010067"
---
# <a name="define-an-oauth1-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>OAuth1 teknik profili, bir Azure Active Directory B2C özel ilke tanımlayın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C için destek sağlar [OAuth 1.0 protokolünü](https://tools.ietf.org/html/rfc5849) kimlik sağlayıcısı. Bu makalede, standartlaştırılmış bu protokolü destekleyen bir talep sağlayıcı ile etkileşim kurmak için bir teknik profil ayrıntılarını açıklar. OAuth1 teknik profili ile Twitter gibi bir OAuth1 tabanlı kimlik sağlayıcısı ile ad'sini birleştirebilir. Kimlik sağlayıcısı ile Federasyon, kullanıcıların oturum sosyal var olan oturum veya Kurumsal kimlikleri sağlar.

## <a name="protocol"></a>Protokol

**Adı** özniteliği **Protokolü** öğesi ayarlanması gerekiyor `OAuth1`. Örneğin, protokol için **Twitter OAUTH1** teknik profil `OAuth1`.

```XML
<TechnicalProfile Id="Twitter-OAUTH1">
  <DisplayName>Twitter</DisplayName>
  <Protocol Name="OAuth1" />
  ...    
```

## <a name="input-claims"></a>Giriş talepleri

**InputClaims** ve **InputClaimsTransformations** öğe boş olmamalıdır.

## <a name="output-claims"></a>Çıkış talep

**OutputClaims** öğesi talep OAuth1 kimlik sağlayıcısı tarafından döndürülen bir listesini içerir. İlkeniz için tanımlanan kimlik sağlayıcısı adını tanımlanan talep eşleştirmek gerekebilir. Ayarladığınız sürece kimlik sağlayıcısı tarafından döndürülen olmayan talepleri de içerebilir **DefaultValue** özniteliği.

**OutputClaimsTransformations** öğe koleksiyonu içerebilir **OutputClaimsTransformation** çıkış talep değiştirmek veya yenilerini oluşturmak için kullanılan öğeleri.

Aşağıdaki örnek, Twitter kimlik sağlayıcısı tarafından döndürülen talepleri gösterir:

- **USER_ID** eşleşen talep **issuerUserId** talep.
- **Screen_name** eşleşen talep **displayName** talep.
- **E-posta** Adı Eşleme olmadan talep.

Teknik profil de kimlik sağlayıcısı tarafından döndürülen olmayan talepleri döndürür: 

- **Identityprovider** kimlik sağlayıcısının adını içeren talep.
- **AuthenticationSource** varsayılan değeri olan talep `socialIdpAuthentication`.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="user_id" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="screen_name" />
  <OutputClaim ClaimTypeReferenceId="email" />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="twitter.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_id | Evet | Kimlik sağlayıcısı uygulama tanımlayıcısı. |
| ProviderName | Hayır | Kimlik sağlayıcısının adı. |
| request_token_endpoint | Evet | İstek, RFC 5849 göre belirteç uç noktası URL'si. |
| authorization_endpoint | Evet | RFC 5849 göre yetkilendirme uç noktasının URL'si. |
| access_token_endpoint | Evet | RFC 5849 göre belirteç uç noktası URL'si. |
| ClaimsEndpoint | Hayır | Kullanıcı bilgileri uç noktasının URL'si. | 
| ClaimsResponseFormat | Hayır | Talep yanıt biçimi.|

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

**CryptographicKeys** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_secret | Evet | Kimlik sağlayıcısı uygulama istemci gizli bilgisi.   | 

## <a name="redirect-uri"></a>Yönlendirme URI'si

Kimlik sağlayıcınızın yeniden yönlendirme URL'sini yapılandırırken girin `https://login.microsoftonline.com/te/tenant/policyId/oauth1/authresp`. Değiştirdiğinizden emin olun **Kiracı** Kiracı adınız (örneğin, contosob2c.onmicrosoft.com) ile ve **Policyıd** ilkenizin (örneğin, b2c_1a_policy) tanımlayıcısına sahip. Yeniden yönlendirme URI'si, tüm küçük harflerle olması gerekiyor. Kimlik sağlayıcısı oturum açma kullanan tüm ilkeleri için bir yönlendirme URL'si ekleyin. 

Kullanıyorsanız **b2clogin.com** etki alanı yerine **login.microsoftonline.com** b2clogin.com login.microsoftonline.com yerine kullandığınızdan emin olun.

Örnekler:

- [Özel ilkeler kullanarak OAuth1 kimlik sağlayıcısı olarak twitter Ekle](active-directory-b2c-custom-setup-twitter-idp.md)













