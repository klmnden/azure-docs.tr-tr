---
title: Azure Active Directory v2.0 belirteç başvurusu | Microsoft Docs
description: Azure AD v2.0 uç noktası tarafından yayılan talepler ve belirteç türleri
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: dc58c282-9684-4b38-b151-f3e079f034fd
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 7fcc39aed1599c1fa928736453082cbce6d7e27e
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39581969"
---
# <a name="azure-active-directory-v20-tokens-reference"></a>Azure Active Directory v2.0 belirteç başvurusu
Azure Active Directory (Azure AD) v2.0 uç noktası her güvenlik belirteçleri birkaç tür yayar [kimlik doğrulama akışı](active-directory-v2-flows.md). Bu başvuru biçimini, güvenlik özellikleri ve her belirteç türünün içeriğini açıklar.

> [!NOTE]
> V2.0 uç noktası, tüm Azure Active Directory senaryolarını ve özelliklerini desteklemez. V2.0 uç noktası kullanması gerekip gerekmediğini belirlemek için aşağıdaki hakkında bilgi edinin: [v2.0 sınırlamaları](active-directory-v2-limitations.md).
>
>

## <a name="types-of-tokens"></a>Belirteç türleri
V2.0 uç noktası destekleyen [OAuth 2.0 Yetkilendirme Protokolü](active-directory-v2-protocols.md), kullanan erişim belirteçlerini ve yenileme belirteçleri. V2.0 uç noktası kimlik doğrulaması ve oturum açma aracılığıyla destekler [Openıd Connect](active-directory-v2-protocols.md). Openıd Connect kimlik belirteci belirteci üçüncü türü sunar. Bu belirteçlerden her biri olarak temsil edilen bir *taşıyıcı* belirteci.

Taşıyıcı belirteç korumalı bir kaynağın taşıyıcı erişim veren bir basit güvenlik belirtecidir. Taşıyıcı belirteç sunabilir herhangi bir tarafa ' dir. Adımları belirteç iletilmesini ve depolanmasını sırasında güvenliğini sağlamak için alınır değil, bir tarafın taşıyıcı belirteç almak için Azure AD kimlik doğrulaması gerekir ancak kesildi ve istenmeyen bir şahıs tarafından kullanılır. Bazı güvenlik belirteçleri yetkisiz taraflar, onları kullanmasına engel olmak için yerleşik bir mekanizma vardır, ancak taşıyıcı belirteçleri kullanmayın. Taşıyıcı belirteçleri Aktarım Katmanı Güvenliği (HTTPS) gibi güvenli bir kanal taşınmasını gerekir. Kötü amaçlı bir taraf, bu tür bir güvenlik bir taşıyıcı belirteç iletilirse, bir "ortadaki-de-ortadaki adam saldırısı" kullanabilir belirteç almak ve korunan bir kaynağa yetkisiz erişim için kullanın. Depolama veya daha sonra kullanmak için taşıyıcı belirteçlerini önbelleğe alma aynı güvenlik ilkeleri uygulayın. Uygulamanız güvenli bir şekilde aktarır ve taşıyıcı belirteçleri depolar her zaman emin olmalısınız. Taşıyıcı belirteçler için daha fazla güvenlik konuları için bkz. [RFC 6750 bölüm 5](http://tools.ietf.org/html/rfc6750).

V2.0 uç noktası tarafından verilen belirteçleri birçoğu, JSON Web belirteçleri (Jwt'ler) uygulanır. JWT'nin, iki taraf arasında bilgi aktarımı yapmak compact, URL güvenli bir yoludur. JWT'nin bilgileri adlı bir *talep*. Taşıyıcı ve belirtecin konu hakkındaki bilgilerin bir olaydır. JWT'nin Taleplerde kodlanmış ve aktarım için seri hale getirilmiş JavaScript nesne gösterimi (JSON) nesneleridir. V2.0 uç noktası tarafından verilen Jwt'ler imzalanmış ancak şifreli olduğundan, kolayca hata ayıklama amacıyla bir JWT içeriğini inceleyebilirsiniz. Jwt'ler hakkında daha fazla bilgi için bkz: [JWT belirtimi](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>Kimliği belirteçleri
Bir kimlik belirteci kullanarak kimlik doğrulaması gerçekleştirdiğinde, uygulamanızı aldığı oturum güvenlik belirteci biçimindedir [Openıd Connect](active-directory-v2-protocols.md). Kimlik belirteçlerini olarak temsil edilir [Jwt'ler](#types-of-tokens), kullanıcı uygulamanızda oturum açmak için kullanabileceğiniz talepleri içerir. Talepler bir kimlik belirteci çeşitli şekillerde kullanabilirsiniz. Genellikle, Yöneticiler, hesap bilgilerini görüntülemek veya erişim denetimi kararları bir uygulama için kimliği belirteçleri kullanın. V2.0 uç noktası yalnızca bir tür talepler, oturum açmış kullanıcı türünden bağımsız olarak tutarlı özellik kümesi olan kimlik belirteci verir. Kimlik belirteçlerini içeriğini ve biçimini iş veya Okul hesapları ve kişisel Microsoft hesabı kullanıcıları için aynıdır.

Şu anda kimlik belirteçlerini imzalı fakat şifrelenmez. Uygulamanıza bir kimlik belirteci aldığında, bu gerekir [imzayı doğrulamak](#validating-tokens) belirtecinin kimliğinin doğruluğunu kanıtlamak ve belirtecin geçerliliğini kanıtlamak için birkaç talepleri doğrulamak için. Bir uygulama tarafından doğrulanmış talep senaryo gereksinimlerine bağlı olarak farklılık gösterir, ancak uygulamanızı bazı gerçekleştirmelidir [ortak talep doğrulamaları](#validating-tokens) her senaryoda.

Talepler hakkında tam Ayrıntılar aşağıdaki bölümlerde, örnek kimlik belirteci yanı sıra kimliği belirteçlerinde sunuyoruz. Kimliği belirteçlere talep belirli bir sırayla döndürülmediğini unutmayın. Ayrıca, yeni bir talep kimliği belirteçlere herhangi bir zamanda tanıtılabilir. Yeni Talep sunulduğunda uygulamanızı kesmemesi. Aşağıdaki liste, uygulamanız şu anda güvenilir bir şekilde yorumlayabilir talepleri içerir. Daha fazla bilgi bulabilirsiniz [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-id-token"></a>Örnek kimlik belirteci
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [!TIP]
> Uygulama için örnek kimliği belirteçteki talepleri incelemek için örnek kimliği belirtece yapıştırın [jwt.ms](http://jwt.ms/).
>
>

#### <a name="claims-in-id-tokens"></a>Kimliği belirteçlere talep
| Ad | İste | Örnek değer | Açıklama |
| --- | --- | --- | --- |
| Hedef kitle |`aud` |`6731de76-14a6-49ae-97bc-6eba6914391e` |Amaçladığınız alıcının belirtecin tanımlar. Kimliği belirteçlerinde, hedef kitlesi, uygulamanızın uygulama kimliği, Microsoft uygulama kayıt portalında uygulamanıza atanan ' dir. Uygulamanız bu değeri doğrulamak ve değer eşleşmiyorsa belirteci reddetme. |
| Veren |`iss` |`https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` |Oluşturur ve belirteci ve kullanıcı kimlik doğrulamasının yapıldığı Azure AD kiracısı döndürür güvenlik belirteci hizmeti (STS) tanımlar. Verenin talep belirteci v2.0 uç noktasından gelen emin olmak için uygulamanızı doğrulamalıdır. Uygulamaya oturum kiracılar sınırlamak için talep GUID kısmını de kullanmalısınız. Kullanıcının bir Microsoft hesabı tüketici kullanıcıdan olduğunu gösteren GUID `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| Çıkışı |`iat` |`1452285331` |Belirteç düzenlendiği zaman dönem saatle gösterilir. |
| Süre sonu |`exp` |`1452289231` |Başlangıçtan belirteci geçersiz hale geldiği tarih dönem saatle gösterilir. Uygulamanız bu talep belirteci ömrü geçerliliğini doğrulamak için kullanmanız gerekir. |
| Öncesine değil |`nbf` |`1452285331` |Başlangıçtan belirtecin geçerli hale geldiği tarih dönem saatle gösterilir. Genellikle verme zamanı ile aynı olur. Uygulamanız bu talep belirteci ömrü geçerliliğini doğrulamak için kullanmanız gerekir. |
| sürüm |`ver` |`2.0` |Azure AD tarafından tanımlanan kimlik belirteci sürümü. V2.0 uç noktası için değerdir `2.0`. |
| kiracı kimliği |`tid` |`b9419818-09af-49c2-b0c3-653adc1f376e` |Kullanıcı dandır Azure AD kiracısı temsil eden bir GUID. İş ve Okul hesapları için kullanıcının ait olduğu kuruluş sabit Kiracı kimliği bir GUID'dir. Kişisel hesapları için değerdir `9188040d-6c67-4c5b-b112-36a304b66dad`. `profile` Kapsamı, bu talebi için gereklidir. |
| Kod karması |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Yalnızca bir OAuth 2.0 yetkilendirme kodu ile kimlik belirteci verildiğinde kod karma kimlik belirteçleri bulunur. Bir yetkilendirme kodu özgünlüğünü doğrulamak için kullanılabilir. Bu doğrulama gerçekleştirme hakkında daha fazla ayrıntı için bkz. [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html). |
| Erişim belirteci karması |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Belirteç karması Kimliğinde bulunan erişim yalnızca zaman kimlik belirteci bir OAuth 2.0 erişim belirteci ile verilen belirteçler. Bir erişim belirteci özgünlüğünü doğrulamak için kullanılabilir. Bu doğrulama gerçekleştirme hakkında daha fazla ayrıntı için bkz. [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html). |
| nonce |`nonce` |`12345` |Nonce belirteç yeniden yürütme saldırılarını azaltma stratejisidir. Uygulamanızı bir geçici öğe içinde bir yetkilendirme isteği kullanarak belirtebilirsiniz `nonce` sorgu parametresi. İstekte sağladığınız değerin kimliği belirtecinin yayıldığını `nonce` talep, değiştirilmemiş. Uygulamanızı belirli bir kimlik belirteci ile uygulamanın oturum ilişkilendirir istekte belirtilen değerle değeri doğrulayabilirsiniz. Uygulamanız kimlik belirteci doğrulama işlemi sırasında bu doğrulaması gerçekleştirmeniz gerekir. |
| ad |`name` |`Babe Ruth` |Ad talebini belirtecin konu tanımlayan insanlar tarafından okunabilen bir değer sağlar. Değerin benzersiz olması garanti edilmez, değişebilir ve yalnızca görüntüleme amaçları için kullanılmak üzere tasarlanmıştır. `profile` Kapsamı, bu talebi için gereklidir. |
| e-posta |`email` |`thegreatbambino@nyy.onmicrosoft.com` |Varsa, kullanıcı hesabıyla ilişkili birincil e-posta adresi. Değerini değişebilir ve zaman içinde değişebilir. `email` Kapsamı, bu talebi için gereklidir. |
| tercih edilen kullanıcı adı |`preferred_username` |`thegreatbambino@nyy.onmicrosoft.com` |Kullanıcıyı v2.0 uç noktasını temsil eden birincil kullanıcı adı. Bir e-posta adresi, telefon numarası veya belirli bir biçimdeki olmadan genel bir kullanıcı adı olabilir. Değerini değişebilir ve zaman içinde değişebilir. Değişebilir olduğundan, bu değer yetkilendirme kararları vermek için kullanılmamalıdır. `profile` Kapsamı, bu talebi için gereklidir. |
| Konu |`sub` |`MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | Sorumlu olduğu hakkında bir uygulamanın kullanıcı gibi bilgileri belirteci onaylar. Bu değer sabittir ve yeniden atandı yeniden veya değiştirilemez. Bu belirteci bir kaynağa erişmek için kullanıldığında gibi güvenli bir şekilde, yetkilendirme denetimleri gerçekleştirmek için kullanılabilir ve veritabanı tablolarındaki bir anahtar olarak kullanılabilir. Her zaman konudur çünkü Azure AD sorunları, bu değer bir genel amaçlı yetkilendirme sistemde kullanmanızı öneririz, belirteçleri sunar. Benzersiz bir uygulama belirli kimliğe - konu, ancak ikili bir tanımlayıcıdır. Bu nedenle, tek bir kullanıcı iki farklı uygulamalara iki farklı istemci kimliklerini kullanarak oturum açtığında, bu uygulamaların konu talebi için iki farklı değerler alır. Bu olabilir veya mimari ve gizlilik gereksinimlerinize bağlı olarak gerekli değildir. |
| Nesne Kimliği |`oid` |`a1dbdde8-e4f9-4571-ad93-3059e3750d23` | Microsoft kimlik sistemi, bu durumda, bir kullanıcı hesabı, bir nesne değişmez tanımlayıcısı. Ayrıca, veritabanı tablolarında güvenli bir şekilde ve bir anahtar olarak yetkilendirme denetimleri gerçekleştirmek için de kullanılabilir. Bu kimliği kullanıcı uygulamalar arasında benzersiz olarak tanımlayan - aynı kullanıcı imzalama iki farklı uygulama aynı değeri alacak `oid` talep. Başka bir deyişle, bu sorgular, Microsoft Graph gibi Microsoft çevrimiçi hizmetlerine yaparken kullanılabilir. Microsoft Graph, bu kimliği olarak döndüreceği `id` özelliği için belirtilen kullanıcı hesabı. Çünkü `oid` kullanıcılar ilişkilendirmek birden fazla uygulama sağlayan `profile` kapsamı, bu talebi için gereklidir. Tek bir kullanıcı birden fazla Kiracı varsa, kullanıcının her Kiracı farklı nesne Kimliğinde içereceğini unutmayın - kullanıcının her hesap aynı kimlik bilgileriyle oturum açtığı olsa bile farklı hesaplar kabul edilir. |

### <a name="access-tokens"></a>Erişim belirteçleri

V2.0 uç noktası için Web API'leri gibi güvenli kaynaklara erişim belirteçlerini vermek için Azure AD ile kayıtlı bir üçüncü taraf uygulamalar sağlar. Uygulamaya erişim belirteçlerini vermek için ayarlama hakkında daha fazla bilgi için lütfen bkz [v2.0 uç noktası ile bir uygulamayı kaydetme](quickstart-v2-register-an-app.md). Uygulamanın v2.0 uç noktası ile kayıt sırasında geliştirici olarak adlandırılan, erişim düzeyleri belirleyebilir **kapsamları**, erişim belirteçleri verilebilir. Örneğin, **calendars.read** Microsoft Graph API'de tanımlanan kapsamı kullanıcının Takvim okuma izni verir. Uygulama v2.0 uç noktasından bir erişim belirteci aldığında, senaryonuza bağlı olarak belirtecinin imzası, veren, İzleyici, sona erme zamanı ve herhangi bir talep doğrulamanız gerekir. 

V2.0 uç noktasından bir erişim belirteci isteğinde bulunduğunuzda v2.0 uç noktası de kullanmak için uygulamanızı için erişim belirteci hakkındaki meta verileri döndürür. Bu bilgiler, kapsamlar için geçerlidir ve erişim belirteci süre sonu içerir. Uygulamanıza akıllı erişim belirteçlerinin açık ayrıştırma zorunda kalmadan erişim belirteci önbelleğe alma işlemi bu metaveriyi kullanır.

### <a name="refresh-tokens"></a>Yenileme belirteçlerini
Yenileme belirteçler, uygulamanızı bir OAuth 2.0 akışı yeni erişim belirteçlerini almak için kullanabileceğiniz güvenlik belirteçleri. Uygulamanızı, yenileme belirteçleri, kullanıcı etkileşimi gerektirmeden bir kullanıcı adına kaynakları için uzun süreli erişim elde etmek için kullanabilirsiniz.

Çok kaynaklı yenileme belirteçleri. Bir kaynak için bir belirteç isteği sırasında alınan bir yenileme belirteci, tamamen farklı bir kaynağa erişim belirteçleri için ödenebilecek.

Bir yenileme belirteci bir yanıt almak için uygulamanızı istemelisiniz ve verilmesi `offline_access` kapsam. Hakkında daha fazla bilgi edinmek için `offline_access` kapsam için bkz: [rızası ve kapsamları](v2-permissions-and-consent.md) makalesi.

Yenileme belirteçlerini olan ve her zaman, uygulamanıza tamamen opak olacaktır. Bunlar Azure AD v2.0 uç noktası tarafından verilen yalnızca inceledi ve v2.0 uç noktası tarafından yorumlanır. Uzun süreli olduklarından, ancak uygulamanızı bir yenileme belirteci süre boyunca en son, bekleyebileceğiniz yazılmamalıdır. Yenileme belirteçleri geçersiz olabilir - Ayrıntılar için çeşitli nedenlerle herhangi bir anda bkz [iptal belirteci](v1-id-and-access-tokens.md#token-revocation). V2.0 uç noktası için bir belirteç isteğini yaparak kullanmak uygulamanızı bir yenileme belirteci geçerli olup olmadığını öğrenmek için tek yolu denemektir.

Pass'inizi ne zaman yeni bir erişim belirteci için bir yenileme belirteci (ve uygulamanızı verilen `offline_access` kapsam), belirteci yanıt olarak yeni bir yenileme belirteci alırsınız. İstekte kullanılan değiştirmek için yeni verilen yenileme belirteci kaydedin. Bu, yenileme belirteçleri mümkün olduğunca uzun bir süre için geçerli kalmasını güvence altına alır.

## <a name="validating-tokens"></a>Belirteçleri doğrulama
Şu anda, uygulamalarınızı yapmanız yalnızca belirteç doğrulama kimliği belirteçleri doğruluyor. Bir kimlik belirteci doğrulamak için uygulamanızın kimliği belirtecinin imzası hem kimliği belirteçteki talepleri doğrulamalıdır.

<!-- TODO: Link --> Microsoft, kitaplıkları ve belirteç doğrulama kolayca nasıl ele alınacağını gösteren kod örnekleri sağlar. Sonraki bölümlerde, şu temel işlemi açıklar. Çeşitli üçüncü taraf açık kaynak kitaplıkları da JWT doğrulama için kullanılabilir. Neredeyse tüm platform ve dil için en az bir kitaplık seçeneği yoktur.

### <a name="validate-the-signature"></a>İmza doğrulama
JWT'nin tarafından ayrılmış üç segmentleri içeren `.` karakter. İlk segment olarak bilinen *üstbilgi*, ikinci bir segmenttir *gövdesi*, ve üçüncü kesim *imza*. İmza segment, böylece uygulamanız tarafından güvenilen kimlik belirteci özgünlüğünü doğrulamak için kullanılabilir.

RSA 256 gibi endüstri standardı asimetrik şifreleme algoritmaları kullanarak kimlik belirteçlerini imzalanmıştır. Kimlik belirteci üstbilgisi, belirteç imzalamak için kullanılan anahtar ve şifreleme yöntemi ilgili bilgiler bulunur. Örneğin:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

`alg` Talep, belirteç imzalamak için kullanılan algoritmayı belirtir. `kid` Talep, belirteç imzalamak için kullanılan ortak anahtar gösterir.

V2.0 uç noktası kimliği ve erişim belirteçleri, belirli bir ortak-özel anahtar çiftleri kümesini herhangi birini kullanarak imzalar. Anahtar bu değişiklikleri otomatik olarak işlemek için uygulamanızı yazılmış şekilde v2.0 uç noktasına düzenli aralıklarla anahtarları, olası kümesini döndürür. 24 saatte v2.0 uç noktası tarafından kullanılan ortak anahtarlar için güncelleştirmeleri denetlemek için makul bir sıklığıdır.

Openıd Connect meta verileri konumundaki belge kullanarak imzayı doğrulamak için gereken imzalama anahtar verileri elde edebilirsiniz:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [!TIP]
> Bir tarayıcıda URL'yi deneyin!

Bu meta veri belgesi Openıd Connect kimlik doğrulaması için gereken çeşitli uç nokta konumu gibi birçok yararlı bilgi, parçalarını içeren bir JSON nesnesidir. Belge de içeren bir *jwks_uri*, Belirteçleri imzalamak için kullanılan ortak anahtar kümesini konumunu sağlar. Şu anda kullanılmakta olan tüm ortak anahtar bilgilerini jwks_uri bulunan JSON belgesini sahiptir. Uygulamanız kullanabilir `kid` hangi ortak anahtarı bu belgedeki bir belirteç imzalamak için kullanılan seçmek için JWT üstbilgisinde talep. Ardından, doğru ortak anahtarı ve belirtilen algoritma kullanarak imza doğrulaması da gerçekleştirir.

> [!NOTE]
> `x5t` Talep v2.0 uç noktası kullanım dışı bırakılmıştır. Kullanmanızı öneririz `kid` belirtecinizi doğrulamak talep.

İmza doğrulama gerçekleştirme, bu belgenin kapsamı dışında olan. Bu konuda yardım sağlayacak birçok açık kaynak kitaplıkları kullanılabilir.

### <a name="validate-the-claims"></a>Talepleri doğrula
Uygulamanızı kullanıcı oturum açma sırasında bir kimlik belirteci aldığında, bunu birkaç denetimleri talepleri karşı kimlik belirteci gerçekleştirmelisiniz. Bunlar dahil ancak bunlarla sınırlı değildir:

* **Hedef kitle** talep kimlik belirteci uygulamanıza verilmesi amaçlanmamıştır doğrulamak için
* **öncesine** ve **süre sonu** talep kimliği belirtecin süresinin sona ermediğini doğrulamak için
* **veren** talep belirteci uygulamanıza v2.0 uç noktası tarafından verildiğini doğrulamak için
* **nonce**, bir belirteç yeniden yürütme saldırısı riskini azaltma

Uygulamanızı gerçekleştirmesi gereken talep doğrulamaları tam bir listesi için bkz. [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Bu talepler için beklenen değerle ayrıntıları dahil edilecek [kimlik belirteçlerini](# ID tokens) bölümü.

## <a name="token-lifetimes"></a>Belirteç kullanım ömrü
Yalnızca bilgi için aşağıdaki belirteç ömrünü sunuyoruz. Geliştirme ve uygulamalarında hata ayıklama bilgileri, yardımcı olabilir. Uygulamalarınızı herhangi birini bu yaşam süreleri sabit kalması beklenir yazılmamalıdır. Belirteç ömürleri olabilir ve herhangi bir zamanda değişir.

| Belirteç | Yaşam süresi | Açıklama |
| --- | --- | --- |
| Kimlik belirteçlerini (iş veya Okul hesapları) |1 saat |Kimlik belirteçlerini, genellikle 1 saat boyunca geçerlidir. Web uygulamanız bu aynı yaşam süresi (önerilen) kullanıcıya kendi oturumu korumak için kullanabilirsiniz veya tamamen farklı oturum ömrünü seçebilirsiniz. Yeni bir kimlik belirteci almak uygulamanız gerekiyorsa, son noktanın yetkilendirilmesi yeni bir oturum açma isteği v2.0 için yapmanız gerekir. Kullanıcının geçerli tarayıcı oturumu v2.0 uç noktası ile varsa, kullanıcı kimlik bilgilerini yeniden girmeniz gerekebilir değil. |
| Kimlik belirteçlerini (Kişisel hesapları) |24 saat |Kişisel hesaplar için kimlik belirteçlerini, genellikle 24 saat boyunca geçerlidir. Web uygulamanız bu aynı yaşam süresi (önerilen) kullanıcıya kendi oturumu korumak için kullanabilirsiniz veya tamamen farklı oturum ömrünü seçebilirsiniz. Yeni bir kimlik belirteci almak uygulamanız gerekiyorsa, son noktanın yetkilendirilmesi yeni bir oturum açma isteği v2.0 için yapmanız gerekir. Kullanıcının geçerli tarayıcı oturumu v2.0 uç noktası ile varsa, kullanıcı kimlik bilgilerini yeniden girmeniz gerekebilir değil. |
| Erişim belirteçleri (iş veya Okul hesapları) |1 saat |Belirteç yanıtlarını belirteci meta verilerini bir parçası olarak gösterilir. |
| Erişim belirteçleri (Kişisel hesapları) |1 saat |Belirteç yanıtlarını belirteci meta verilerini bir parçası olarak gösterilir. Verilen kişisel hesaplar adına erişim belirteçlerini farklı bir yaşam süresi için yapılandırılabilir, ancak 1 saat normaldir. |
| Yenileme belirteçlerini (iş veya Okul hesabı) |En fazla 14 gün |En fazla 14 gün için geçerli bir tek bir yenileme belirtecidir. Ancak, uygulamanızın başarısız kadar ya da isteğe bağlı olarak uygulamanızı yeni bir yenileme belirteciyle değiştirir kadar bir yenileme belirteci kullanmayı deneyin sürmelidir için yenileme belirtecini çeşitli nedenlerden dolayı herhangi bir zamanda geçersiz hale gelebilir. Bir yenileme belirteci, ayrıca, kullanıcının kimlik bilgilerini girdiği 90 gün oluşturmanızın geçersiz olur. |
| Yenileme belirteçlerini (Kişisel hesapları) |En fazla 1 yıl |Bir tek bir yenileme belirteci en fazla 1 yıl için geçerlidir. Ancak, uygulamanızın başarısız kadar bir yenileme belirteci kullanmayı deneyin sürmelidir için yenileme belirtecini çeşitli nedenlerden dolayı herhangi bir zamanda geçersiz hale gelebilir. |
| Yetkilendirme kodları (iş veya Okul hesapları) |10 dakika |Yetkilendirme kodları bilerek kısa ömürlüdür ve belirteçleri alındığında hemen erişim belirteci ve yenileme belirteçleri için kullanılamadı. |
| Yetkilendirme kodları (Kişisel hesapları) |5 dakika |Yetkilendirme kodları bilerek kısa ömürlüdür ve belirteçleri alındığında hemen erişim belirteci ve yenileme belirteçleri için kullanılamadı. Kişisel hesaplar adına veren yetkilendirme kodları tek seferlik kullanım içindir. |
