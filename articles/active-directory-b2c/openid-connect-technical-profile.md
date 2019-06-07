---
title: Bir Openıd Connect teknik profili Azure Active Directory B2C özel bir ilke tanımlamak | Microsoft Docs
description: Bir Openıd Connect teknik profili Azure Active Directory B2C özel bir ilke tanımlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 6d16415aa5111388ec2d2a1009ff477574ae42c5
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66512908"
---
# <a name="define-an-openid-connect-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Openıd Connect teknik profili, bir Azure Active Directory B2C özel ilke tanımlayın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C için destek sağlar [Openıd Connect](https://openid.net/2015/04/17/openid-connect-certification-program/) Protokolü kimlik sağlayıcısı. Openıd Connect 1.0, OAuth 2.0 üzerinde bir kimlik katmanı tanımlar ve teknoloji, modern kimlik doğrulama protokolleri temsil eder. Bir Openıd Connect teknik profili, Openıd Connect tabanlı kimlik sağlayıcısı, Azure AD gibi ad'sini birleştirebilir. Bir kimlik sağlayıcısı ile Federasyon, kullanıcıların oturum sosyal var olan oturum veya Kurumsal kimlikleri sağlar.

## <a name="protocol"></a>Protocol

**Adı** özniteliği **Protokolü** öğesi ayarlanması gerekiyor `OpenIdConnect`. Örneğin, protokol için **MSA OIDC** teknik profil `OpenIdConnect`:

```XML
<TechnicalProfile Id="MSA-OIDC">
  <DisplayName>Microsoft Account</DisplayName>
  <Protocol Name="OpenIdConnect" />
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

**OutputClaims** öğesi talep Openıd Connect kimlik sağlayıcısı tarafından döndürülen bir listesini içerir. İlkeniz için tanımlanan kimlik sağlayıcısı adını tanımlanan talep eşleştirmek gerekebilir. Ayarladığınız sürece kimlik sağlayıcısı tarafından döndürülen olmayan talepleri de içerebilir `DefaultValue` özniteliği.

**OutputClaimsTransformations** öğe koleksiyonu içerebilir **OutputClaimsTransformation** çıkış talep değiştirmek veya yenilerini oluşturmak için kullanılan öğeleri.

Aşağıdaki örnek, Microsoft Account kimlik sağlayıcısı tarafından döndürülen talepleri gösterir:

- **Alt** eşleşen talep **issuerUserId** talep.
- **Adı** eşleşen talep **displayName** talep.
- **E-posta** Adı Eşleme olmadan.

Teknik profil de kimlik sağlayıcısı tarafından döndürülen olmayan talepleri döndürür:

- **Identityprovider** kimlik sağlayıcısının adını içeren talep.
- **AuthenticationSource** varsayılan değeri olan talep **socialIdpAuthentication**.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="sub" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" />
</OutputClaims>
```

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_id | Evet | Kimlik sağlayıcısı uygulama tanımlayıcısı. |
| IdTokenAudience | Hayır | İd_token İzleyici. Belirtilmişse, Azure AD B2C belirteç kimlik sağlayıcısı tarafından döndürülen bir talep ve belirtilen birine eşit olup olmadığını denetler. |
| METADATA | Evet | Bir JSON yapılandırma belgesine işaret eden bir URL olarak da bilinen bir iyi bilinen openıd yapılandırma uç noktası olan Openıd Connect Discovery belirtimine göre biçimlendirilmiş. |
| ProviderName | Hayır | Kimlik sağlayıcısının adı. |
| response_types | Hayır | Openıd Connect Core 1.0 belirtimine göre yanıt türü. Olası değerler: `id_token`, `code`, veya `token`. |
| response_mode | Hayır | Kimlik sağlayıcısı sonucu Azure AD B2C geri göndermek için kullandığı yöntem. Olası değerler: `query`, `form_post` (varsayılan) veya `fragment`. |
| scope | Hayır | Openıd Connect Core 1.0 belirtimine göre tanımlanan isteğinin kapsamı. Gibi `openid`, `profile`, ve `email`. |
| HttpBinding | Hayır | Erişim belirteci ve taleplerin belirteci uç beklenen HTTP bağlama. Olası değerler: `GET` veya `POST`.  |
| ValidTokenIssuerPrefixes | Hayır | Her Kiracı için Azure Active Directory gibi bir çok kiracılı kimlik sağlayıcısı kullanarak oturum açmak için kullanılan anahtar. |
| UsePolicyInRedirectUri | Hayır | Yeniden yönlendirme URI'si oluştururken bir ilke kullanılıp kullanılmayacağını belirtir. Uygulamanız kimlik sağlayıcısı yapılandırdığınızda, yeniden yönlendirme URI'si belirtmeniz gerekir. Yeniden yönlendirme URI'si, Azure AD B2C'ye işaret `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` (login.microsoftonline.com your-Kiracı-name.b2clogin.com ile değişebilir).  Belirtirseniz `false`, kullandığınız her ilke için bir yeniden yönlendirme URI'si eklemeniz gerekir. Örneğin: `https://login.microsoftonline.com/te/{tenant}/{policy}/oauth2/authresp`. |
| MarkAsFailureOnStatusCode5xx | Hayır | Http durum kodu 5xx aralığında ise, bir dış hizmet isteğine hata olarak işaretlenmiş olup olmadığını gösterir. Varsayılan değer: `false`. |
| DiscoverMetadataByTokenIssuer | Hayır | OIDC meta verileri JWT belirteci vericisi kullanarak bulunan olup olmadığını gösterir. |

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

**CryptographicKeys** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_secret | Evet | Kimlik sağlayıcısı uygulama istemci gizli bilgisi. Yalnızca şifreleme anahtarı gereklidir **response_types** meta veri kümesine `code`. Bu durumda, Azure AD B2C, bir erişim belirteci için yetki kodunu değiştirmek için başka bir çağrı yapar. Meta veriler ayarlanırsa `id_token` şifreleme anahtarını atlayabilirsiniz.  |  

## <a name="redirect-uri"></a>Yeniden yönlendirme URI'si
 
Yeniden yönlendirme URI'si kimlik sağlayıcınızın yapılandırdığınızda girin `https://login.microsoftonline.com/te/tenant/oauth2/authresp`. Değiştirdiğinizden emin olun **Kiracı** kiracınızın adı (örneğin, contosob2c.onmicrosoft.com) ya da kiracının kimliği. Yeniden yönlendirme URI'si, tüm küçük harflerle olması gerekiyor.

Kullanıyorsanız **b2clogin.com** etki alanı yerine **login.microsoftonline.com** b2clogin.com login.microsoftonline.com yerine kullandığınızdan emin olun.

Örnekler:

- [Microsoft hesabı (MSA) özel ilkeleri kullanarak bir kimlik sağlayıcısı olarak Ekle](active-directory-b2c-custom-setup-msa-idp.md)
- [Azure AD hesapları kullanarak oturum açın](active-directory-b2c-setup-aad-custom.md)
- [Özel ilkeleri kullanarak çok kiracılı Azure AD kimlik sağlayıcısı için oturum açmasına izin ver](active-directory-b2c-setup-commonaad-custom.md)

 














