---
title: Azure Active Directory erişim belirteçlerini başvurusu | Microsoft Docs
description: Azure AD tarafından v1.0 ve v2.0 uç yayılan erişim belirteçleri hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/15/2019
ms.author: celested
ms.reviewer: hirsin
ms.custom: fasttrack-edit
ms.collection: M365-identity-device-management
ms.openlocfilehash: 17c9ef471ca1536f928ca5ae2fe4f55e8e2b3424
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58878426"
---
# <a name="azure-active-directory-access-tokens"></a>Azure Active Directory erişim belirteçleri

Erişim belirteçleri, güvenli bir şekilde Azure tarafından korunan API'leri çağırmak istemcileri etkinleştirir. Azure Active Directory (Azure AD) erişim belirteçleri [Jwt'ler](https://tools.ietf.org/html/rfc7519), Base64 kodlu Azure tarafından imzalanmış bir JSON nesnesi. İstemciler, yalnızca kaynak erişim belirteçleri belirtecin içeriği olarak donuk dizeler olarak hedeflenen değerlendirmeniz gerekir. Doğrulama ve hata ayıklama amacıyla, geliştiricilerin bir site gibi Jwt'ler çözebilen [jwt.ms](https://jwt.ms). İstemci erişim ya da uç noktasından (v1.0 veya v2.0) belirteci almak çeşitli protokoller kullanarak.

Azure AD, ayrıca bir erişim belirteci isteğinde bulunduğunuzda uygulamanızın tüketim için erişim belirteci hakkında bazı meta verileri döndürür. Bu bilgiler, kapsamlar için geçerlidir ve erişim belirteci süre sonu içerir. Bu veriler, akıllı erişim belirteçleri erişim belirteci ayrıştırılamadı gerek kalmadan önbelleğe alma işlemi için uygulamanızı sağlar.

Uygulamanızın istemciler için erişim talep edebilir bir kaynak (web API'si) ise, erişim belirteçleri kullanılmak üzere kimlik doğrulaması ve yetkilendirme, kullanıcı, istemci, veren, izinleri ve diğer yararlı bilgiler sağlar. 

Nasıl kaynak doğrulamak ve bir erişim belirteci içindeki taleplerin kullanılmasını öğrenmek için aşağıdaki bölümlere bakın.

> [!NOTE]
> Kişisel hesap (örneğin, hotmail.com veya outlook.com) ile istemci uygulamanızı test ederken, istemci tarafından alınan erişim belirteci donuk bir dize olduğunu fark edebilirsiniz. Erişilen kaynak şifrelenir ve istemci tarafından anlaşılan eski MSA (Microsoft hesabı) biletlerini istedi olmasıdır.

## <a name="sample-tokens"></a>Örnek belirteçleri

V1.0 ve v2.0 belirteçleri, aşağıdakine benzer ve aynı talep çoğunu içerir. Her bir örnek burada verilmiştir.

### <a name="v10"></a>v1.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9.eyJhdWQiOiJlZjFkYTlkNC1mZjc3LTRjM2UtYTAwNS04NDBjM2Y4MzA3NDUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTUyMjIyOS8iLCJpYXQiOjE1MzcyMzMxMDYsIm5iZiI6MTUzNzIzMzEwNiwiZXhwIjoxNTM3MjM3MDA2LCJhY3IiOiIxIiwiYWlvIjoiQVhRQWkvOElBQUFBRm0rRS9RVEcrZ0ZuVnhMaldkdzhLKzYxQUdyU091TU1GNmViYU1qN1hPM0libUQzZkdtck95RCtOdlp5R24yVmFUL2tES1h3NE1JaHJnR1ZxNkJuOHdMWG9UMUxrSVorRnpRVmtKUFBMUU9WNEtjWHFTbENWUERTL0RpQ0RnRTIyMlRJbU12V05hRU1hVU9Uc0lHdlRRPT0iLCJhbXIiOlsid2lhIl0sImFwcGlkIjoiNzVkYmU3N2YtMTBhMy00ZTU5LTg1ZmQtOGMxMjc1NDRmMTdjIiwiYXBwaWRhY3IiOiIwIiwiZW1haWwiOiJBYmVMaUBtaWNyb3NvZnQuY29tIiwiZmFtaWx5X25hbWUiOiJMaW5jb2xuIiwiZ2l2ZW5fbmFtZSI6IkFiZSAoTVNGVCkiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMjIyNDcvIiwiaXBhZGRyIjoiMjIyLjIyMi4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJvaWQiOiIwMjIyM2I2Yi1hYTFkLTQyZDQtOWVjMC0xYjJiYjkxOTQ0MzgiLCJyaCI6IkkiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJsM19yb0lTUVUyMjJiVUxTOXlpMmswWHBxcE9pTXo1SDNaQUNvMUdlWEEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6ImFiZWxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJGVnNHeFlYSTMwLVR1aWt1dVVvRkFBIiwidmVyIjoiMS4wIn0=.D3H6pMUtQnoJAGq6AHd
```

Bu v1.0 belirtecinde görüntülemek [JWT.ms](https://jwt.ms/#access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9.eyJhdWQiOiJlZjFkYTlkNC1mZjc3LTRjM2UtYTAwNS04NDBjM2Y4MzA3NDUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTUyMjIyOS8iLCJpYXQiOjE1MzcyMzMxMDYsIm5iZiI6MTUzNzIzMzEwNiwiZXhwIjoxNTM3MjM3MDA2LCJhY3IiOiIxIiwiYWlvIjoiQVhRQWkvOElBQUFBRm0rRS9RVEcrZ0ZuVnhMaldkdzhLKzYxQUdyU091TU1GNmViYU1qN1hPM0libUQzZkdtck95RCtOdlp5R24yVmFUL2tES1h3NE1JaHJnR1ZxNkJuOHdMWG9UMUxrSVorRnpRVmtKUFBMUU9WNEtjWHFTbENWUERTL0RpQ0RnRTIyMlRJbU12V05hRU1hVU9Uc0lHdlRRPT0iLCJhbXIiOlsid2lhIl0sImFwcGlkIjoiNzVkYmU3N2YtMTBhMy00ZTU5LTg1ZmQtOGMxMjc1NDRmMTdjIiwiYXBwaWRhY3IiOiIwIiwiZW1haWwiOiJBYmVMaUBtaWNyb3NvZnQuY29tIiwiZmFtaWx5X25hbWUiOiJMaW5jb2xuIiwiZ2l2ZW5fbmFtZSI6IkFiZSAoTVNGVCkiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMjIyNDcvIiwiaXBhZGRyIjoiMjIyLjIyMi4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJvaWQiOiIwMjIyM2I2Yi1hYTFkLTQyZDQtOWVjMC0xYjJiYjkxOTQ0MzgiLCJyaCI6IkkiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJsM19yb0lTUVUyMjJiVUxTOXlpMmswWHBxcE9pTXo1SDNaQUNvMUdlWEEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6ImFiZWxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJGVnNHeFlYSTMwLVR1aWt1dVVvRkFBIiwidmVyIjoiMS4wIn0=.D3H6pMUtQnoJAGq6AHd).

### <a name="v20"></a>v2.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9.eyJhdWQiOiI2ZTc0MTcyYi1iZTU2LTQ4NDMtOWZmNC1lNjZhMzliYjEyZTMiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3L3YyLjAiLCJpYXQiOjE1MzcyMzEwNDgsIm5iZiI6MTUzNzIzMTA0OCwiZXhwIjoxNTM3MjM0OTQ4LCJhaW8iOiJBWFFBaS84SUFBQUF0QWFaTG8zQ2hNaWY2S09udHRSQjdlQnE0L0RjY1F6amNKR3hQWXkvQzNqRGFOR3hYZDZ3TklJVkdSZ2hOUm53SjFsT2NBbk5aY2p2a295ckZ4Q3R0djMzMTQwUmlvT0ZKNGJDQ0dWdW9DYWcxdU9UVDIyMjIyZ0h3TFBZUS91Zjc5UVgrMEtJaWpkcm1wNjlSY3R6bVE9PSIsImF6cCI6IjZlNzQxNzJiLWJlNTYtNDg0My05ZmY0LWU2NmEzOWJiMTJlMyIsImF6cGFjciI6IjAiLCJuYW1lIjoiQWJlIExpbmNvbG4iLCJvaWQiOiI2OTAyMjJiZS1mZjFhLTRkNTYtYWJkMS03ZTRmN2QzOGU0NzQiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJhYmVsaUBtaWNyb3NvZnQuY29tIiwicmgiOiJJIiwic2NwIjoiYWNjZXNzX2FzX3VzZXIiLCJzdWIiOiJIS1pwZmFIeVdhZGVPb3VZbGl0anJJLUtmZlRtMjIyWDVyclYzeERxZktRIiwidGlkIjoiNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3IiwidXRpIjoiZnFpQnFYTFBqMGVRYTgyUy1JWUZBQSIsInZlciI6IjIuMCJ9.pj4N-w_3Us9DrBLfpCt
```

Bu v2.0 belirteç görüntülemek [JWT.ms](https://jwt.ms/#access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9.eyJhdWQiOiI2ZTc0MTcyYi1iZTU2LTQ4NDMtOWZmNC1lNjZhMzliYjEyZTMiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3L3YyLjAiLCJpYXQiOjE1MzcyMzEwNDgsIm5iZiI6MTUzNzIzMTA0OCwiZXhwIjoxNTM3MjM0OTQ4LCJhaW8iOiJBWFFBaS84SUFBQUF0QWFaTG8zQ2hNaWY2S09udHRSQjdlQnE0L0RjY1F6amNKR3hQWXkvQzNqRGFOR3hYZDZ3TklJVkdSZ2hOUm53SjFsT2NBbk5aY2p2a295ckZ4Q3R0djMzMTQwUmlvT0ZKNGJDQ0dWdW9DYWcxdU9UVDIyMjIyZ0h3TFBZUS91Zjc5UVgrMEtJaWpkcm1wNjlSY3R6bVE9PSIsImF6cCI6IjZlNzQxNzJiLWJlNTYtNDg0My05ZmY0LWU2NmEzOWJiMTJlMyIsImF6cGFjciI6IjAiLCJuYW1lIjoiQWJlIExpbmNvbG4iLCJvaWQiOiI2OTAyMjJiZS1mZjFhLTRkNTYtYWJkMS03ZTRmN2QzOGU0NzQiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJhYmVsaUBtaWNyb3NvZnQuY29tIiwicmgiOiJJIiwic2NwIjoiYWNjZXNzX2FzX3VzZXIiLCJzdWIiOiJIS1pwZmFIeVdhZGVPb3VZbGl0anJJLUtmZlRtMjIyWDVyclYzeERxZktRIiwidGlkIjoiNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3IiwidXRpIjoiZnFpQnFYTFBqMGVRYTgyUy1JWUZBQSIsInZlciI6IjIuMCJ9.pj4N-w_3Us9DrBLfpCt).

## <a name="claims-in-access-tokens"></a>Taleplerde erişim belirteçleri

Jwt'ler üç parçalara bölünür:

* **Üst bilgi** -hakkında bilgi sağlar [belirteci doğrulamak](#validating-tokens) belirtecini ve nasıl imzalandığı türüyle ilgili bilgiler dahil olmak üzere.
* **Yükü** -tüm kullanıcı veya arama hizmetiniz çalışıyor, uygulama ile ilgili önemli veriler içerir.
* **İmza** -belirteci doğrulamak için kullanılan ham madde olduğu.

Her parça birbirinden noktayla ayrılmış (`.`) ve ayrı olarak Base64 kodlu.

Yalnızca doldurmak için bir değer varsa, mevcut taleplerdir. Bu nedenle, uygulamanızı bir bağımlılık mevcut olan bir talebi temel almamalıdır. Örnekler `pwd_exp` (süresi dolacak şekilde parolaları her Kiracı gerektirir) veya `family_name` ([istemci kimlik bilgileri](v1-oauth2-client-creds-grant-flow.md) akışlardır adlara sahip olmayan uygulamaları adına). Erişim belirteci doğrulama için kullanılan talepleri her zaman mevcut olacak.

> [!NOTE]
> Bazı talep, Azure AD belirteçleri yeniden durumunda güvenli yardımcı olmak için kullanılır. Bunlar, açıklamada "Donuk" olarak bulunan ortak tüketim için olmaması olarak işaretlenir. Bu talep bir belirteç içinde yer alamaz veya olabilir ve bir bildirim olmadan yeni vm'lere eklenebilir.

### <a name="header-claims"></a>Üst bilgi talep

|İste | Biçimlendir | Açıklama |
|--------|--------|-------------|
| `typ` | Dize - her zaman "JWT" | JWT belirteci olduğunu gösterir.|
| `nonce` | String | Belirteç yeniden yürütme saldırılarına karşı korumak için kullanılan benzersiz tanımlayıcısı. Kaynağınızı olumsuzlukları karşı korumak için bu değer kaydedebilirsiniz. |
| `alg` | String | Örneğin, "RS256" belirteç imzalamak için kullanılan algoritmayı belirtir |
| `kid` | String | Bu belirteç imzalamak için kullanılan ortak anahtar için parmak izini belirtir. Erişim belirteçleri, hem v1.0 hem de v2.0 yayılır. |
| `x5t` | String | Aynı (kullanın ve değerini) olarak işlev `kid`. `x5t` eski bir talep v1.0 erişim belirteçleri uyumluluk amacıyla, yalnızca yayılır. |

### <a name="payload-claims"></a>Yükü talepleri

| İste | Biçimlendir | Açıklama |
|-----|--------|-------------|
| `aud` | Dize, bir uygulama kimliği URI'si | Amaçladığınız alıcının belirtecin tanımlar. Erişim belirteçlerinde, hedef kitlesi, uygulamanızın uygulama kimliği, Azure portalında uygulamanıza atanan ' dir. Uygulamanız, bu değeri doğrulamak ve belirteç değeri eşleşmiyorsa reddet. |
| `iss` | Dize, bir STS URI | Oluşturur ve belirteci ve kullanıcı kimlik doğrulamasının yapıldığı Azure AD kiracısı döndürür güvenlik belirteci hizmeti (STS) tanımlar. Verilen belirtecin bir v2.0 belirteç ise (bkz `ver` talep), URI içinde sona erecek `/v2.0`. Kullanıcının bir Microsoft hesabı tüketici kullanıcıdan olduğunu gösteren GUID `9188040d-6c67-4c5b-b112-36a304b66dad`. Uygulamanız varsa, uygulamaya oturum açabilirsiniz kiracılar kümesini sınırlandırmak için talep GUID kısmını kullanmanız gerekir. |
|`idp`| Dize, genellikle bir STS URI | Belirtecin öznesinin kimliğini doğrulayan kimlik sağlayıcısını kaydeder. -Veren olarak aynı kiracıda değil kullanıcı hesabının konukların sürece örneği için bu değer veren talep değeri için aynıdır. Talep mevcut değilse, değeri anlamına `iss` bunun yerine kullanılabilir.  Bir kuruluş (Azure AD kiracısına davet örneği için bir kişisel hesap) bağlamında kullanılan kişisel hesapları için `idp` talep 'live.com' veya Microsoft hesabı kiracısının içeren bir STS URI olabilir `9188040d-6c67-4c5b-b112-36a304b66dad`. |  
| `iat` | int, UNIX zaman damgası | "Konumunda verilen" kimlik doğrulaması için bu belirteci gerçekleştiği gösterir. |
| `nbf` | int, UNIX zaman damgası | "Nbf" (önce değil) talep işlem önüne JWT değil kabul edilmesi gereken zamanı tanımlar. |
| `exp` | int, UNIX zaman damgası | "Exp" (süre) talep veya daha sonra JWT işleme için kabul edilmesi gereken değil sona erme zamanı tanımlar. Kaynak belirteci de, bu süreden önce bir değişiklik kimlik doğrulaması gerekli olduğunda gibi reddedebilir veya belirteç iptali algılandı dikkat edin önemlidir. |
| `acr` | Dize, "0" veya "1" | "Kimlik doğrulaması bağlamı sınıf" talebi. "0" değeri, son kullanıcı kimlik doğrulaması, ISO/IEC 29115 gereksinimlerini karşılamayan gösterir. |
| `aio` | Donuk dize | Belirteci yeniden kullanım için verileri kaydetmek üzere Azure AD tarafından kullanılan bir iç talep. Kaynakları bu talep kullanmamanız gerekir. |
| `amr` | Dize JSON dizisi | Yalnızca v1.0 belirteçlerinde sunar. Belirtecin konu kimliğinin nasıl tanımlar. Bkz: [amr talep bölüm](#the-amr-claim) daha fazla ayrıntı için. |
| `appid` | Dize, bir GUID | Yalnızca v1.0 belirteçlerinde sunar. Belirteci kullanarak istemci uygulama kimliği. Uygulamanın kendisini olarak ya da bir kullanıcı adına davranabilir. Uygulama kimliği, genellikle Uygulama nesnesini temsil eder, ancak aynı zamanda Azure AD'de hizmet sorumlusu nesnesi gösterebilir. |
| `appidacr` | "0", "1" veya "2" | Yalnızca v1.0 belirteçlerinde sunar. İstemcinin kimlik doğrulamasının nasıl yapıldığı gösterir. Genel bir istemci için "0" değeridir. İstemci Kimliğini ve istemci gizli anahtarı kullanılırsa, "1" bir değerdir. Bir istemci sertifikası kimlik doğrulaması için kullanıldığında, "2" bir değerdir. |
| `azp` | Dize, bir GUID | Yalnızca v2.0 belirteçlerinde sunar. Belirteci kullanarak istemci uygulama kimliği. Uygulamanın kendisini olarak ya da bir kullanıcı adına davranabilir. Uygulama kimliği, genellikle Uygulama nesnesini temsil eder, ancak aynı zamanda Azure AD'de hizmet sorumlusu nesnesi gösterebilir. |
| `azpacr` | "0", "1" veya "2" | Yalnızca v2.0 belirteçlerinde sunar. İstemcinin kimlik doğrulamasının nasıl yapıldığı gösterir. Genel bir istemci için "0" değeridir. İstemci Kimliğini ve istemci gizli anahtarı kullanılırsa, "1" bir değerdir. Bir istemci sertifikası kimlik doğrulaması için kullanıldığında, "2" bir değerdir. |
| `groups` | JSON dizisi GUID'leri | Öznenin grup üyeliklerini temsil eden nesne kimlikleri sağlar. Bu değerler benzersiz (nesne kimliği bakın) ve güvenli bir şekilde bir kaynağa erişim yetkisi zorlama gibi erişimi yönetmek için kullanılabilir. Grupları talepte bulunan gruplarını aracılığıyla uygulama başına temelinde, yapılandırılan `groupMembershipClaims` özelliği [uygulama bildirimini](reference-app-manifest.md). Bir null değeri tüm grupları hariç, yalnızca güvenlik grupları ve Office 365 dağıtım listeleri Active Directory güvenlik grubu üyelikleri ve bir değeri "Tüm" dahil "IDAP" değerini içerecektir. <br><br>Bkz: `hasgroups` kullanımıyla ilgili ayrıntılar için aşağıda talep `groups` örtük vermeyi ile talep. <br>Kullanıcının grup sayısı (SAML 150, JWT için 200) bir limitini aşması durumunda diğer akışlar için daha sonra bir fazla kullanım talep kullanıcı gruplarının listesini içeren Graph uç noktası işaret talep kaynaklarına eklenir. |
| `hasgroups` | Boole | Varsa, her zaman `true`, olan en az bir gruba kullanıcı belirten. Öğesinin yerine kullanılmalıdırlar `groups` Jwt'ler için bir talep tam grupları talep varsa örtük vermeyi akışlardaki URI parçası (şu anda, 6 veya daha fazla grup) URL uzunluk sınırları ötesinde genişletin. Kullanıcının grupları belirlemek için istemci Graph kullanmanız gerektiğini belirtir (`https://graph.windows.net/{tenantID}/users/{userID}/getMemberObjects`). |
| `groups:src1` | JSON nesnesi | Uzunluğu sınırlı olmayan belirteç istekleri için (bkz `hasgroups` yukarıda), ancak yine de çok büyük belirteç için bir bağlantı kullanıcı için tam grupları listesine eklenir. SAML yerine yeni bir talep olarak için Dağıtılmış bir talep olarak jwt'lerinin `groups` talep. <br><br>**Örnek JWT değer**: <br> `"groups":"src1"` <br> `"_claim_sources`: `"src1" : { "endpoint" : "https://graph.windows.net/{tenantID}/users/{userID}/getMemberObjects" }` |
| `preferred_username` | String | Kullanıcıyı temsil eden birincil kullanıcı adı. Bir e-posta adresi, telefon numarası veya belirli bir biçimdeki olmadan genel bir kullanıcı adı olabilir. Değerini değişebilir ve zaman içinde değişebilir. Değişebilir olduğundan, bu değer yetkilendirme kararları vermek için kullanılmamalıdır.  Bu ancak kullanıcı adı ipuçları için kullanılabilir. `profile` Kapsamı, bu talebi için gereklidir. |
| `name` | String | Belirtecin konu tanımlayan insanlar tarafından okunabilen bir değer sağlar. Değerin benzersiz olması garanti edilmez, değişebilir ve yalnızca görüntüleme amaçları için kullanılmak üzere tasarlanmıştır. `profile` Kapsamı, bu talebi için gereklidir. |
| `oid` | Dize, bir GUID | Microsoft kimlik platformu, bu durumda, bir kullanıcı hesabı, bir nesne değişmez tanımlayıcısı. Ayrıca, veritabanı tablolarında güvenli bir şekilde ve bir anahtar olarak yetkilendirme denetimleri gerçekleştirmek için de kullanılabilir. Bu kimliği kullanıcı uygulamalar arasında benzersiz olarak tanımlayan - aynı kullanıcı imzalama iki farklı uygulama aynı değeri alacak `oid` talep. Bu nedenle, `oid` yapma gibi çevrimiçi Microsoft hizmetlerine, Microsoft Graph sorguladığında kullanılabilir. Microsoft Graph, bu kimliği olarak döndüreceği `id` özelliği için belirtilen kullanıcı hesabı. Çünkü `oid` kullanıcılar ilişkilendirmek birden fazla uygulama sağlayan `profile` kapsamı, bu talebi için gereklidir. Tek bir kullanıcı birden fazla Kiracı varsa, kullanıcının her Kiracı farklı nesne Kimliğinde içereceğini unutmayın - kullanıcının her hesap aynı kimlik bilgileriyle oturum açtığı olsa bile farklı hesaplar kabul edilir. |
| `rh` | Donuk dize | Belirteçleri düzeltin için Azure tarafından kullanılan bir iç talep. Kaynakları bu talep kullanmamanız gerekir. |
| `scp` | Dize, boşlukla ayrılmış kapsam listesi | Kendisi için istemci uygulaması istenen (ve alınan), uygulamanız tarafından kullanıma sunulan kapsamları kümesini onayı. Uygulamanız bu kapsamları, uygulamanız tarafından kullanıma sunulan geçerli olanlardır ve bu kapsamları değerine göre yetkilendirme kararları doğrulamanız gerekir. İçin yalnızca dahil edilen [kullanıcı belirteçleri](#user-and-application-tokens). |
| `roles` | Dize dizesi izinlerin bir listesi | İstekte bulunan uygulamayla çağırmak için izin verilen, uygulamanız tarafından kullanıma sunulan izinleri kümesi. İçin [uygulama belirteçleri](#user-and-application-tokens), bu sırasında kullanılan [istemci kimlik bilgileri](v1-oauth2-client-creds-grant-flow.md) kullanıcı kapsamları yerine akış.  İçin [kullanıcı belirteçleri](#user-and-application-tokens) bu kullanıcıya atandı şekilde hedef uygulamayı rolleri ile doldurulur. |
| `sub` | Dize, bir GUID | Sorumlu olduğu hakkında bir uygulamanın kullanıcı gibi bilgileri belirteci onaylar. Bu değer sabittir ve yeniden atandı yeniden veya değiştirilemez. Bu belirteci bir kaynağa erişmek için kullanıldığında gibi güvenli bir şekilde, yetkilendirme denetimleri gerçekleştirmek için kullanılabilir ve veritabanı tablolarındaki bir anahtar olarak kullanılabilir. Her zaman konudur çünkü Azure AD sorunları, bu değer bir genel amaçlı yetkilendirme sistemde kullanmanızı öneririz, belirteçleri sunar. Benzersiz bir uygulama belirli kimliğe - konu, ancak ikili bir tanımlayıcıdır. Bu nedenle, tek bir kullanıcı iki farklı uygulamalara iki farklı istemci kimliklerini kullanarak oturum açtığında, bu uygulamaların konu talebi için iki farklı değerler alır. Bu olabilir veya mimari ve gizlilik gereksinimlerinize bağlı olarak gerekli değildir. |
| `tid` | Dize, bir GUID | Kullanıcı dandır Azure AD kiracısı temsil eder. İş ve Okul hesapları için kullanıcının ait olduğu kuruluş sabit Kiracı kimliği bir GUID'dir. Kişisel hesapları için değerdir `9188040d-6c67-4c5b-b112-36a304b66dad`. `profile` Kapsamı, bu talebi için gereklidir. |
| `unique_name` | String | Yalnızca v1.0 belirteçlerinde sunar. Belirtecin konusunu tanımlayan ve okunabilir bir değer sunar. Bu değer, bir kiracıda benzersiz olması garanti edilmez ve yalnızca görüntüleme amaçları için kullanılmalıdır. |
| `uti` | Donuk dize | Belirteçleri düzeltin için Azure tarafından kullanılan bir iç talep. Kaynakları bu talep kullanmamanız gerekir. |
| `ver` | Dize, ya da `1.0` veya `2.0` | Erişim belirteci sürümünü gösterir. |

#### <a name="v10-basic-claims"></a>temel talep V1.0

Aşağıdaki talep v1.0 belirteçleri varsa dahil edilir, ancak v2.0 belirteçlerinde varsayılan olarak dahil edilmez. V2.0 ve bu talepler gerek kullanıyorsanız, bunları isteyen kullanarak [isteğe bağlı bir talep](active-directory-optional-claims.md).

| İste | Biçimlendir | Açıklama |
|-----|--------|-------------|
| `ipaddr`| String | IP adresi gelen kimliği doğrulanmış kullanıcı. |
| `onprem_sid`| Dize, içinde [SID biçimi](https://docs.microsoft.com/windows/desktop/SecAuthZ/sid-components) | Bir şirket içi kimlik doğrulamasıyla kullanıcının sahip olduğu durumlarda bu talep, SID sağlar. Kullanabileceğiniz `onprem_sid` yetkilendirme eski uygulamalar için.|
| `pwd_exp`| int, UNIX zaman damgası | Kullanıcı parolasının süresi dolduğunda gösterir. |
| `pwd_url`| String | Kullanıcının parolasını sıfırlamak için kullanıcılar'ın nerede gönderilebilecek bir URL. |
| `in_corp`|boole | Sinyaller istemci ve şirket ağından oturum açılıyor. Değilse, talep dahil değildir. |
| `nickname`| String | İlk veya son adından ayrı kullanıcı için ek bir ad.|
| `family_name` | String | Kullanıcı nesnesindeki tanımlandığı şekilde, son adını, soyadını veya kullanıcının aile adı sağlar. |
| `given_name` | String | İlk sağlar veya kullanıcı nesnesindeki belirlenen kullanıcının verilen adı. |
| `upn` | String | Kullanıcının kullanıcı adı. Bir telefon numarası, e-posta adresi veya biçimlendirilmemiş dize olabilir. Yalnızca görüntüleme amaçları ve yeniden kimlik doğrulaması senaryolarda sağlayarak kullanıcı adı ipuçları için kullanılmalıdır. |

#### <a name="the-amr-claim"></a>`amr` Talep

Microsoft kimlik, uygulamanızla ilgili yolları, çeşitli kimlik doğrulaması yapabilir. `amr` Talep olduğu gibi birden çok öğe içeren bir dizi `["mfa", "rsa", "pwd"]`, hem parola hem de kimlik doğrulayıcı uygulamasını kullanılan bir kimlik doğrulaması için.

| Değer | Açıklama |
|-----|-------------|
| `pwd` | Parola kimlik doğrulaması, bir kullanıcının Microsoft parolası veya bir uygulamanın istemci gizli anahtarı. |
| `rsa` | Kimlik doğrulaması temel bir RSA anahtar kanıtı üzerinde örneğin ile [Microsoft Authenticator uygulamasını](https://aka.ms/AA2kvvu). Kimlik doğrulaması X509 ait olan bir hizmeti tarafından otomatik olarak imzalanan bir JWT yapılmışsa bu içeren sertifika. |
| `otp` | E-posta veya kısa mesaj kullanarak bir kerelik geçiş kodu. |
| `fed` | Bir Federasyon kimlik doğrulamasını onaylama (örneğin, JWT veya SAML) kullanıldı. |
| `wia` | Tümleşik Windows Kimlik Doğrulaması |
| `mfa` | Çok faktörlü kimlik doğrulaması kullanıldı. Bu mevcut olduğunda bir kimlik doğrulama yöntemleri de dahil edilir. |
| `ngcmfa` | Eşdeğer `mfa`belirli Gelişmiş kimlik bilgisi türlerini sağlamak için kullanılır. |
| `wiaormfa`| Kullanıcının kimliğini doğrulamak için Windows ya da bir MFA kimlik bilgisi kullanılır. |
| `none` | Kimlik doğrulaması gerçekleştirildi. |

## <a name="validating-tokens"></a>Belirteçleri doğrulama

Bir id_token veya bir access_token doğrulamak için uygulamanızı hem belirtecinin imzası hem de talep doğrulamalıdır. Erişim belirteçleri doğrulamak için uygulamanızı de veren, İzleyici ve imzalama belirteçleri doğrulamalıdır. Bu değerler Openıd keşif belgesi karşı doğrulanması gerekir. Örneğin, belgenin Kiracı bağımsız sürümünü şu konumdadır [ https://login.microsoftonline.com/common/.well-known/openid-configuration ](https://login.microsoftonline.com/common/.well-known/openid-configuration). 

Azure AD ara yazılım erişim belirteçleri doğrulamak için yerleşik özellikler vardır ve aracılığıyla göz atabilirsiniz bizim [örnekleri](https://docs.microsoft.com/azure/active-directory/active-directory-code-samples) birini tercih ettiğiniz dilde bulunacak. Açıkça bir JWT belirteci doğrulama hakkında daha fazla bilgi için bkz. [el ile JWT doğrulama örnek](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation). 

Kitaplıkları ve belirteç doğrulama kolayca nasıl ele alınacağını gösteren kod örnekleri sunuyoruz. Temel alınan işlem anlamak istiyorsanız olanlar için aşağıdaki bilgileri sağlanır. Kullanılabilir ayrıca çeşitli üçüncü taraf açık kaynak kitaplıkları için JWT doğrulama - neredeyse her platform ve dil burada için en az bir seçenek yoktur. Azure AD kimlik doğrulama kitaplıkları ve kod örnekleri hakkında daha fazla bilgi için bkz. [v1.0 kimlik doğrulama kitaplıkları](active-directory-authentication-libraries.md) ve [v2.0 kimlik doğrulama kitaplıkları](reference-v2-libraries.md).

### <a name="validating-the-signature"></a>İmza doğrulama

JWT'nin tarafından ayrılmış üç segmentleri içeren `.` karakter. İlk segment olarak bilinen **üstbilgi**olarak ikinci **gövdesi**ve üçüncü olarak **imza**. İmza segment, böylece uygulamanız tarafından güvenilen belirteç özgünlüğünü doğrulamak için kullanılabilir.

Azure AD tarafından verilen belirteçlere RSA 256 gibi sektörde standart asimetrik şifreleme algoritmaları kullanılarak imzalanmış. JWT üstbilgisi, belirteç imzalamak için kullanılan anahtar ve şifreleme yöntemiyle ilgili bilgiler içerir:

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "iBjL1Rcqzhiy4fpxIxdZqohM2Yk",
  "kid": "iBjL1Rcqzhiy4fpxIxdZqohM2Yk"
}
```

`alg` Talep belirteci, while imzalamak için kullanılan algoritmayı belirtir `kid` talep, belirteç imzalamak için kullanılan belirli bir ortak anahtar gösterir.

Herhangi belirli bir noktada, Azure AD, belirli bir dizi ortak-özel anahtar çifti herhangi birini kullanarak bir id_token oturum açabilirsiniz. Uygulamanızın bu anahtar değişiklikleri otomatik olarak işlemek için yazılmış olması gerekir böylece azure AD'ye anahtarlarını düzenli aralıklarla, olası kümesini döndürür. 24 saatte bir Azure AD tarafından kullanılan ortak anahtarlar için güncelleştirmeleri denetlemek için makul bir sıklığıdır.

Konumundaki Openıd Connect meta veri belgesi kullanarak imzayı doğrulamak gereken imzalama anahtar verileri elde edebilirsiniz:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [!TIP]
> Bu deneyin [URL](https://login.microsoftonline.com/common/.well-known/openid-configuration) bir tarayıcıda!

Bu meta veri belgesi:

* Openıd Connect kimlik doğrulaması gerçekleştirmek için gereken çeşitli uç nokta konumu gibi bilgiler birkaç faydalı parçalarını içeren bir JSON nesnesi. 
* İçeren bir `jwks_uri`, Belirteçleri imzalamak için kullanılan ortak anahtar kümesini konumunu sağlar. JSON belgesini konumundaki `jwks_uri` tüm zaman içinde belirli o anda kullanımda ortak anahtar bilgilerini içerir. Uygulamanız kullanabilir `kid` hangi ortak anahtarı bu belgedeki belirli bir belirteç imzalamak için kullanılan seçmek için JWT üstbilgisinde talep. Ayrıca, doğru ortak anahtarı ve belirtilen algoritmasını kullanarak imza doğrulaması daha sonra gerçekleştirebilirsiniz.

> [!NOTE]
> Her ikisi de v1.0 uç noktanın döndürdüğü `x5t` ve `kid` talepleri yalnızca v2.0 uç noktası ile yanıtlar sırada `kid` talep. Bundan sonra kullanılması önerilir `kid` belirtecinizi doğrulamak talep.

Bu belgenin kapsamı dışında olan imzası doğrulama gerçekleştiriliyor - birçok açık kaynak kitaplıkları, gerekirse, bunu yapmanıza yardımcı olmak için kullanılabilir.

### <a name="claims-based-authorization"></a>Talep tabanlı yetkilendirme

Bu adım, uygulamanızın iş mantığı gösterecektir, bazı yaygın yetkilendirme yöntemleri aşağıda açıklanmıştır.

* Denetleme `scp` veya `roles` mevcut tüm kapsamlar API'niz tarafından kullanıma sunulan bu eşleştiğini doğrulamak için talep ve istenen eylemi gerçekleştirmek istemcinin sağlamak.
* Çağıran istemci kullanarak API'nize çağrı izin sağlamak `appid` talep.
* Çağıran istemciyi kullanarak kimlik doğrulama durumunu doğrulamak `appidacr` -API çağırmak için genel istemcilere izin verilmiyorsa 0 olmamalıdır.
* Kontrol listesini karşı geçmiş `nonce` belirteç değil yeniden doğrulamak talep.
* Bu maddeyi `tid` API çağırmak için izin verilen bir kiracı ile eşleşir.
* Kullanım `acr` kullanıcı MFA gerçekleştirilen doğrulamak talep. Bu kullanılarak zorlanıp zorlanmayacağını [koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/overview).
* Siz istediniz, `roles` veya `groups` erişim belirtecinde talep, kullanıcının bu eylemi gerçekleştirmek için izin verilen grupta olduğunu doğrulayın.
  * Örtük akışını kullanarak belirteçleri için büyük olasılıkla sorgulamak ihtiyacınız olacak [Microsoft Graph](https://developer.microsoft.com/graph/) bu için olarak verilerdir genellikle belirteç sığmayacak kadar büyük.

## <a name="user-and-application-tokens"></a>Kullanıcı ve uygulama belirteçleri

Uygulama belirteçleri (normal akışı) kullanıcı adına veya doğrudan bir uygulamadan alabilirsiniz (aracılığıyla [istemci kimlik bilgileri akışı](v1-oauth2-client-creds-grant-flow.md)). Bu çağrı bir uygulamadan gelen ve onu destekleyen bir kullanıcı yok. Bu yalnızca uygulama belirteçlerini gösterir. Bu belirteçlerin büyük ölçüde aynı, bazı farklar ele alınmıştır:

* Yalnızca uygulama belirteçleri sahip olmaz bir `scp` talep ve bunun yerine olacak bir `roles` talep. Uygulama izni (aksine, temsilci izinleri) kaydedildiği yerdir budur. Temsilcili ve uygulama izinleri hakkında daha fazla bilgi için bkz ve vermiş olursunuz [v1.0](v1-permissions-and-consent.md) ve [v2.0](v2-permissions-and-consent.md).
* Birçok insan özgü talep eksilir gibi `name`.

## <a name="token-revocation"></a>Belirteç iptali

Yenileme belirteçlerini geçersiz veya çeşitli nedenlerle için herhangi bir zamanda iptal edildi. Bu iki ana kategoriye ayrılır: zaman aşımları ve geri alma işlemleri.

### <a name="token-timeouts"></a>Belirteci zaman aşımları

* MaxInactiveTime: Yenileme belirteci MaxInactiveTime tarafından dikte zaman içinde kullanılmamış, belirteci yenilemek artık geçerli olmayacak. 
* MaxSessionAge: Ardından MaxAgeSessionMultiFactor veya MaxAgeSessionSingleFactor (kadar iptal edilen) varsayılan dışında bir şey atanmışsa, MaxAgeSession * ayarlanmış bir süre geçtikten sonra yeniden kimlik doğrulamanın gerekli olarak olacaktır. 
* Örnekler:
  * 5 günlük bir MaxInactiveTime kiracısına sahip kullanıcı için bir hafta tatile oluştu ve bu nedenle AAD kullanıcıdan yeni bir belirteç isteğini 7 gündür görülmeyen değil. Kullanıcı yeni bir belirteç istediğinde, kendi yenileme belirteci iptal edildi ve kendi kimlik bilgileri yeniden girmelisiniz bulabilirsiniz.
  * 1 günlük bir MaxAgeSessionSingleFactor duyarlı bir uygulamaya sahiptir. Pazartesi ve Salı (25 saat geçtikten sonra) bir kullanıcı oturum açarsa, yeniden kimlik doğrulamaya zorlayabilir gerekir.

### <a name="revocation"></a>İptal etme

|   | Parola tabanlı tanımlama bilgisi | Parola tabanlı belirteci | Parola tabanlı olmayan tanımlama bilgisi | Parola tabanlı olmayan belirteci | Gizli istemci belirteci |
|---|-----------------------|----------------------|---------------------------|--------------------------|---------------------------|
| Parola süre sonu | Canlı kalır | Canlı kalır | Canlı kalır | Canlı kalır | Canlı kalır |
| Parola kullanıcı tarafından değiştirildi | İptal edildi | İptal edildi | Canlı kalır | Canlı kalır | Canlı kalır |
| SSPR kullanıcı yok | İptal edildi | İptal edildi | Canlı kalır | Canlı kalır | Canlı kalır |
| Yönetici parolasını sıfırlar. | İptal edildi | İptal edildi | Canlı kalır | Canlı kalır | Canlı kalır |
| Kullanıcı iptal, yenileme belirteçleri [PowerShell aracılığıyla](https://docs.microsoft.com/powershell/module/azuread/revoke-azureadsignedinuserallrefreshtoken) | İptal edildi | İptal edildi | İptal edildi | İptal edildi | İptal edildi |
| Yönetici, Kiracı için tüm yenileme belirteçleri iptal [PowerShell aracılığıyla](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken) | İptal edildi | İptal edildi |İptal edildi | İptal edildi | İptal edildi |
| [Çoklu oturum kapatmak](v1-protocols-openid-connect-code.md#single-sign-out) Web'de | İptal edildi | Canlı kalır | İptal edildi | Canlı kalır | Canlı kalır |

> [!NOTE]
> "Olmayan parola tabanlı" oturum açma kullanıcı almak için bir parola girildiği yaramadı biridir. Örneğin, yüz tanıma Windows Hello, bir FIDO anahtar veya bir PIN ile kullanarak. 
>
> Windows birincil yenileme belirteci ile bilinen bir sorun var. PRT parola alınır ve ardından kullanıcı oturumu Merhaba, bu PRT, oluşturulma değiştirmez ve kullanıcı parolasını değiştirirse iptal edilir.
>
> Yenileme belirteçleri değil geçersiz veya yeni bir erişim belirteci getirme ve belirteç yenileme için kullanılan iptal edildi.  

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [ `id_tokens` Azure AD'de](id-tokens.md).
* İzin hakkında bilgi edinin ve vermiş olursunuz [v1.0](v1-permissions-and-consent.md) ve [v2.0](v2-permissions-and-consent.md).
