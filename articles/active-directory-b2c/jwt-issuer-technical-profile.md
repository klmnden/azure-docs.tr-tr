---
title: Bir JWT belirteci veren özel bir ilke Azure Active Directory B2C için teknik profil tanımlama | Microsoft Docs
description: Özel bir ilke Azure Active Directory B2C, bir JWT belirteci veren teknik profil tanımlarsınız.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 10/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 247ebdc8156453062eefe6738c5c281d393a9923
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58436063"
---
# <a name="define-a-technical-profile-for-a-jwt-token-issuer-in-an-azure-active-directory-b2c-custom-policy"></a>JWT belirteci veren bir Azure Active Directory B2C özel ilkesindeki için teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C, her kimlik doğrulama akışı işlediği gibi çeşitli güvenlik belirteçleri yayar. JWT belirteci veren teknik profil bağlı taraf uygulaması için döndürülen bir JWT belirteci yayar. Genellikle bu teknik profili, son kullanıcı yolculuğu düzenleme adımı içindir.

## <a name="protocol"></a>Protokol

**Adı** özniteliği **Protokolü** öğesi ayarlanması gerekiyor `None`. Ayarlama **OutputTokenFormat** öğesine `JWT`.

Aşağıdaki örnek, teknik profil gösterir `JwtIssuer`:

```XML
<TechnicalProfile Id="JwtIssuer">
  <DisplayName>JWT Issuer</DisplayName>
  <Protocol Name="None" />
  <OutputTokenFormat>JWT</OutputTokenFormat>
  ...
</TechnicalProfile>
```
 
## <a name="input-output-and-persist-claims"></a>Talep kalıcı giriş ve çıkış

**InputClaims**, **OutputClaims**, ve **PersistClaims** öğe boş olmamalıdır. **InutputClaimsTransformations** ve **OutputClaimsTransformations** öğeler de yok.

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| issuer_refresh_token_user_identity_claim_type | Evet | Kullanıcı kimliği olarak kullanılacak talep talep içinde OAuth2 yetkilendirme kodları ve yenileme belirteçlerini. Varsayılan olarak, bu ayarlamalısınız `objectId`sürece farklı SubjectNamingInfo talep türü belirtin. | 
| SendTokenResponseBodyWithJsonNumbers | Hayır | Her zaman `true`. Burada sayısal değerleri verilmiş JSON sayı yerine dize olarak eski biçimi için ayarlanmış `false`. Bu öznitelik, bir bağımlılık gibi özellikleri dizeler olarak döndürülen önceki uygulaması üzerinde gerçekleştirilen istemciler için gereklidir. | 
| token_lifetime_secs | Hayır | Belirteç ömürleri erişim. Korunan kaynaklara erişim sağlamak için kullanılan OAuth 2.0 taşıyıcı belirtecinin ömrü. 3.600 saniye (1 saat) varsayılandır. En az (sınırlar dahil), 300 saniyedir (5 dakika) olmalıdır. En fazla (sınırlar dahil) 86.400 saniyedir (24 saat) ' dir. | 
| id_token_lifetime_secs | Hayır | Belirteç ömürleri kimliği. 3.600 saniye (1 saat) varsayılandır. En az (sınırlar dahil), 300 saniyedir (5 dakika) olmalıdır. En fazla (sınırlar dahil) 86,400 (24 saat) saniyedir. | 
| refresh_token_lifetime_secs | Hayır | Belirteç ömürleri yenileyin. Uygulamanızı offline_access kapsamı verildiğini, önce bir yenileme belirteci yeni bir erişim belirteci almak için kullanılabilecek en uzun süre. 120,9600 saniye (14 gün) varsayılandır. En azından (sınırlar dahil) 86.400 saniyedir (24 saat) gereklidir. En fazla (sınırlar dahil) 7,776,000 (90 gün) saniyedir. | 
| rolling_refresh_token_lifetime_secs | Hayır | Yenileme belirteci kayan pencere ömrü. Belirtilen sürenin geçmesinden sonra kullanıcının kimliğinin yeniden kimlik doğrulaması yapın, uygulama tarafından alınan belirteç bağımsız olarak en son geçerlilik süresini yenileyin. Bir kayan pencere ömrü uygulamak istemiyorsanız allow_infinite_rolling_refresh_token için değerini `true`. 7,776,000 saniye (90 gün) varsayılandır. En azından (sınırlar dahil) 86.400 saniyedir (24 saat) gereklidir. En fazla (sınırlar dahil) 31,536,000 (365 gün) saniyedir. | 
| allow_infinite_rolling_refresh_token | Hayır | Varsa kümesine `true`, yenileme belirteci kayan pencere ömrü her zaman geçerli olsun. |
| IssuanceClaimPattern | Evet | Verici (iss) talebi denetler. Değerlerden biri:<ul><li>AuthorityAndTenantGuid - ISS talep içeren etki alanı adınızı gibi `login.microsoftonline` veya `tenant-name.b2clogin.com`ve, Kiracı tanımlayıcısı https://login.microsoftonline.com/00000000-0000-0000-0000-000000000000/v2.0/</li><li>AuthorityWithTfp - ISS talep içeren etki alanı adınızı gibi `login.microsoftonline` veya `tenant-name.b2clogin.com`, Kiracı tanımlayıcısı ve bağlı olan taraf ilke adı. https://login.microsoftonline.com/tfp/00000000-0000-0000-0000-000000000000/b2c_1a_tp_sign-up-or-sign-in/v2.0/</li></ul> | 
| AuthenticationContextReferenceClaimPattern | Hayır | Denetimleri `acr` talep değeri.<ul><li>Hiçbiri - Azure AD B2C acr talebi verme değil</li><li>Policyıd - `acr` talep içeren ilke adı</li></ul>Bu değeri ayarlamak için Seçenekler şunlardır: TFP (güven framework ilke) ve ACR (kimlik doğrulaması bağlamı Başvurusu). TFP için bu değerin belirlenmesi önerilir, bu değeri ayarlamak için olun `<Item>` ile `Key="AuthenticationContextReferenceClaimPattern"` var ve değer `None`. Bağlı olan taraf ilkenizde, ekleme `<OutputClaims>` öğesi, bu öğe ekleme `<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />`. Ayrıca ilkenizi talep türünü içeren emin olun `<ClaimType Id="trustFrameworkPolicy">   <DisplayName>trustFrameworkPolicy</DisplayName>     <DataType>string</DataType> </ClaimType>` | 

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

Aşağıdaki öznitelikleri CryptographicKeys öğe içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| issuer_secret | Evet | X509 JWT belirteç imzalamak için kullanılacak sertifikayı (RSA anahtar kümesi). Bu `B2C_1A_TokenSigningKeyContainer` içinde cofigured anahtar [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md). | 
| issuer_refresh_token_key | Evet | X509 yenileme belirtecini şifrelemek için kullanılacak sertifikayı (RSA anahtar kümesi). Yapılandırmış `B2C_1A_TokenEncryptionKeyContainer` anahtarını [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md) |














