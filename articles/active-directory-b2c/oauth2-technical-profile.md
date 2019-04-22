---
title: Özel bir ilke Azure Active Directory B2C, bir OAuth2 teknik profili tanımlamak | Microsoft Docs
description: Azure Active Directory B2C, özel bir ilkede bir OAuth2 teknik profili tanımlayın.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: fde556c60f823f4bd287ca5672503158c7292f51
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58918935"
---
# <a name="define-an-oauth2-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Bir Azure Active Directory B2C özel İlkesi'nde bir OAuth2 teknik profili tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C, OAuth2 protokolünü kimlik sağlayıcısı için destek sağlar. Bu yetkilendirme ve temsilci seçilen kimlik doğrulaması için birincil protokolüdür. Daha fazla bilgi için [RFC 6749 OAuth 2.0 yetkilendirme Framework](https://tools.ietf.org/html/rfc6749). İle OAuth2 kimlik sağlayıcısı, sosyal var olan oturum izin vererek, Facebook ve Live.com gibi veya Kurumsal kimlik ile bir OAuth2 devredebilir teknik profili temel.

## <a name="protocol"></a>Protokol

**Adı** özniteliği **Protokolü** öğesi ayarlanması gerekiyor `OAuth2`. Örneğin, protokol için **Facebook OAUTH** teknik profil `OAuth2`:

```XML
<TechnicalProfile Id="Facebook-OAUTH">
  <DisplayName>Facebook</DisplayName>
  <Protocol Name="OAuth2" />
  ...    
```

## <a name="input-claims"></a>Giriş talepleri

**InputClaims** ve **InputClaimsTransformations** öğeleri gerekli değildir. Ancak ek parametreler kimlik sağlayıcınız göndermek isteyebilirsiniz. Aşağıdaki örnek ekler **domain_hint** sorgu dizesi parametresi değeri ile `contoso.com` yetkilendirme isteği için.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="domain_hint" DefaultValue="contoso.com" />
</InputClaims>
```

## <a name="output-claims"></a>Çıkış talep

**OutputClaims** öğesi talep OAuth2 kimlik sağlayıcısı tarafından döndürülen bir listesini içerir. İlkeniz için tanımlanan kimlik sağlayıcısı adını tanımlanan talep eşleştirmek gerekebilir. Ayarladığınız sürece kimlik sağlayıcısı tarafından döndürülen olmayan talepleri de içerebilir `DefaultValue` özniteliği.

**OutputClaimsTransformations** öğe koleksiyonu içerebilir **OutputClaimsTransformation** çıkış talep değiştirmek veya yenilerini oluşturmak için kullanılan öğeleri.

Aşağıdaki örnek, Facebook kimlik sağlayıcısı tarafından döndürülen talepleri gösterir:

- **First_name** talep eşlendi **givenName** talep.
- **Soyadı** talep eşlendi **Soyadı** talep.
- **DisplayName** Adı Eşleme olmadan talep...
- **E-posta** Adı Eşleme olmadan talep.

Teknik profil de kimlik sağlayıcısı tarafından döndürülen olmayan talepleri döndürür: 

- **Identityprovider** kimlik sağlayıcısının adını içeren talep.
- **AuthenticationSource** varsayılan değeri olan talep **socialIdpAuthentication**.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
  <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="first_name" />
  <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="last_name" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="facebook.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_id | Evet | Kimlik sağlayıcısı uygulama tanımlayıcısı. |
| IdTokenAudience | Hayır | İd_token İzleyici. Belirtilmişse, Azure AD B2C belirteç kimlik sağlayıcısı tarafından döndürülen bir talep ve belirtilen birine eşit olup olmadığını denetler. |
| authorization_endpoint | Evet | RFC 6749 göre yetkilendirme uç noktasının URL'si. |
| AccessTokenEndpoint | Evet | RFC 6749 göre belirteç uç noktası URL'si. |  
| ClaimsEndpoint | Evet | RFC 6749 göre kullanıcı bilgileri uç noktasının URL'si. | 
| AccessTokenResponseFormat | Hayır | Erişim belirteci uç noktası çağrısı biçimi. Örneğin, bir HTTP GET yöntemi Facebook gerektiriyor, ancak erişim belirteci yanıtı JSON biçimindedir. |
| AdditionalRequestQueryParameters | Hayır | Ek istek sorgu parametreleri. Örneğin, ek parametreler kimlik sağlayıcınız göndermek isteyebilirsiniz. Virgül olan sınırlayıcıyı kullanarak birden çok parametre ekleyebilirsiniz. | 
| ClaimsEndpointAccessTokenName | Hayır | Erişim belirteci sorgu dizesi parametresinin adı. Bazı kimlik sağlayıcıları talep uç noktaları destek GET HTTP isteği. Bu durumda, taşıyıcı belirteç yetkilendirme üst bilgisi yerine bir sorgu dizesi parametresi aracılığıyla gönderilir. |
| ClaimsEndpointFormatName | Hayır | Biçim sorgu dizesi parametresinin adı. Örneğin, adı olarak ayarlayabilirsiniz `format` uç nokta bu LinkedIn talep `https://api.linkedin.com/v1/people/~?format=json`. | 
| ClaimsEndpointFormat | Hayır | Biçim sorgu dizesi parametresinin değeri. Örneğin, değer olarak ayarlayabilirsiniz `json` uç nokta bu LinkedIn talep `https://api.linkedin.com/v1/people/~?format=json`. | 
| ProviderName | Hayır | Kimlik sağlayıcısının adı. |
| response_mode | Hayır | Kimlik sağlayıcısı sonucu Azure AD B2C geri göndermek için kullandığı yöntem. Olası değerler: `query`, `form_post` (varsayılan) veya `fragment`. |
| scope | Hayır | OAuth2 kimlik sağlayıcısı belirtimine göre tanımlanan erişim isteğinin kapsamı. Gibi `openid`, `profile`, ve `email`. |
| HttpBinding | Hayır | Erişim belirteci ve taleplerin belirteci uç beklenen HTTP bağlama. Olası değerler: `GET` veya `POST`.  |
| ResponseErrorCodeParamName | Hayır | HTTP 200 (Tamam) döndürülen hata iletisi içeren parametrenin adı. |
| ExtraParamsInAccessTokenEndpointResponse | Hayır | Alınan yanıtta döndürülebilecek ek parametreleri içeren **AccessTokenEndpoint** bazı kimlik sağlayıcıları. Örneğin, gelen yanıt **AccessTokenEndpoint** gibi ek bir parametre içeren `openid`, zorunlu bir parametre access_token yanı sıra olduğu bir **ClaimsEndpoint** istek sorgu dize. Birden çok parametre adları kaçış verilecek ve virgülle ayrılmış ',' sınırlayıcısı. |
| ExtraParamsInClaimsEndpointRequest | Hayır | İçinde döndürülen ek parametreleri içeren **ClaimsEndpoint** bazı kimlik sağlayıcıları tarafından istek. Birden çok parametre adları kaçış verilecek ve virgülle ayrılmış ',' sınırlayıcısı. |

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

**CryptographicKeys** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_secret | Evet | Kimlik sağlayıcısı uygulama istemci gizli bilgisi. Yalnızca şifreleme anahtarı gereklidir **response_types** meta veri kümesine `code`. Bu durumda, Azure AD B2C, bir erişim belirteci için yetki kodunu değiştirmek için başka bir çağrı yapar. Meta veriler ayarlanırsa `id_token` şifreleme anahtarını atlayabilirsiniz.  |  

## <a name="redirect-uri"></a>Yönlendirme URI'si

Kimlik sağlayıcınızın yeniden yönlendirme URL'sini yapılandırırken girin `https://login.microsoftonline.com/te/tenant/policyId/oauth2/authresp`. Değiştirdiğinizden emin olun **Kiracı** kiracınızın adı (örneğin, contosob2c.onmicrosoft.com) ile ve **Policyıd** ilkenizin (örneğin, b2c_1a_policy) tanımlayıcısına sahip. Yeniden yönlendirme URI'si, tüm küçük harflerle olması gerekiyor.

Kullanıyorsanız **b2clogin.com** etki alanı yerine **login.microsoftonline.com** b2clogin.com login.microsoftonline.com yerine kullandığınızdan emin olun.

Örnekler:

- [Google + özel ilkeleri kullanarak OAuth2 kimlik sağlayıcısı olarak Ekle](active-directory-b2c-custom-setup-goog-idp.md)













