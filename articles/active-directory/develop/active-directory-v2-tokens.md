---
title: Azure Active Directory v2.0 belirteçler başvurusu | Microsoft Docs
description: Azure AD v2.0 uç noktası tarafından gösterilen talep ve belirteç türleri
services: active-directory
documentationcenter: ''
author: hpsin
manager: mtillman
editor: ''
ms.assetid: dc58c282-9684-4b38-b151-f3e079f034fd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2018
ms.author: hirsin
ms.custom: aaddev
ms.openlocfilehash: 4a408fb40c976c6e06f00d074504de6a3ec29bd1
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-active-directory-v20-tokens-reference"></a>Azure Active Directory v2.0 belirteç başvurusu
Azure Active Directory (Azure AD) v2.0 uç her güvenlik belirteçleri çeşitli türlerde yayar [kimlik doğrulaması akışı](active-directory-v2-flows.md). Bu başvuru biçimi, güvenlik özellikleri ve her tür bir belirteç içeriği açıklar.

> [!NOTE]
> V2.0 uç noktası, tüm Azure Active Directory senaryolarını ve özelliklerini desteklemez. V2.0 uç kullanması gerekip gerekmediğini belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
>
>

## <a name="types-of-tokens"></a>Belirteç türleri
V2.0 uç destekleyen [OAuth 2.0 yetkilendirme protokolünü](active-directory-v2-protocols.md), kullanan erişim belirteçleri ve yenileme belirteçleri. V2.0 uç noktası kimlik doğrulama ve oturum açma aracılığıyla da destekler [Openıd Connect](active-directory-v2-protocols.md). Openıd Connect kimliği belirteci bir belirteç üçüncü türü sunar. Bu belirteçler her olarak temsil edilen bir *taşıyıcı* belirteci.

Bir taşıyıcı belirteci korunan bir kaynağa taşıyıcı erişim veren bir basit güvenlik belirtecidir. Taşıyıcı belirteci sunabilir herhangi bir tarafa ' dir. Adımları belirteç iletim ve depolama sırasında güvenli hale getirmek için katılmaz varsa bir taraf taşıyıcı belirteci almak için Azure AD ile kimlik doğrulaması yapmalıdır olsa da ele ve istenmeyen bir şahıs tarafından kullanılır. Bazı güvenlik belirteçleri yetkisiz tarafların onları kullanmasına engel olmak için yerleşik bir mekanizma sahip ancak taşıyıcı belirteçlerini yapın. Aktarım Katmanı Güvenliği (HTTPS) gibi güvenli bir kanal taşıyıcı belirteçlerini taşınan gerekir. Bu tür güvenlik bir taşıyıcı belirteci iletilirse, kötü amaçlı bir taraf "man-in--middle saldırı" kullanabilirsiniz belirtecini almak ve yetkisiz erişim korunan bir kaynağa için kullanın. Depolama veya taşıyıcı belirteçlerini daha sonra kullanmak için önbelleğe alma aynı güvenlik ilkeleri uygulayın. Uygulamanızı güvenli bir şekilde iletir ve taşıyıcı belirteçleri depolar her zaman emin olun. Taşıyıcı belirteçler için daha fazla güvenlik konuları için bkz: [RFC 6750 bölüm 5](http://tools.ietf.org/html/rfc6750).

Birçok v2.0 uç noktası tarafından yayınlanan belirteçleri JSON Web belirteçleri (Jwt'ler) uygulanır. JWT, iki taraf arasında bilgi aktarımı yapmak için compact, URL için güvenli bir yoludur. JWT'nin bilgileri adlı bir *talep*. Bu konu belirtecin ve taşıyıcı hakkındaki bilgilerin onayı ifade eder. JWT'nin talepleri, kodlanmış ve aktarım için seri JavaScript nesne gösterimi (JSON) nesneleridir. V2.0 uç noktası tarafından verilen Jwt'ler imzalanmış ancak şifreli olduğundan, hata ayıklama amacıyla JWT içeriğini kolayca inceleyebilirsiniz. Jwt'ler hakkında daha fazla bilgi için bkz: [JWT belirtimi](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>Kimliği belirteçleri
Bir kimliği belirteci kullanarak kimlik doğrulaması gerçekleştirdiğinde, uygulamanızı alan oturum açma güvenlik belirteci biçimidir [Openıd Connect](active-directory-v2-protocols.md). Kimlik belirteçlerini olarak temsil [Jwt'ler](#types-of-tokens), ve kullanıcının uygulamanıza oturum açmak için kullanabileceğiniz talepleri içerir. Talep Kimliği belirteci çeşitli şekillerde kullanabilirsiniz. Genellikle, admins hesabı bilgilerini görüntülemek veya erişim denetimi kararlarını bir uygulama için kimliği belirteçleri kullanın. V2.0 uç nokta açmış kullanıcı türünü bağımsız olarak talepler tutarlı bir dizi vardır kimliği belirteci yalnızca bir tür yayınlar. Biçim ve kimlik belirteçlerini içeriğini iş veya Okul hesapları ve kişisel Microsoft hesabı kullanıcıları için aynıdır.

Şu anda kimlik belirteçlerini imzalanmış fakat şifrelenmez. Uygulamanızı bir kimliği belirteci aldığında, gerekir [imzayı doğrulamak](#validating-tokens) belirtecin Orijinallik kanıtlamak ve geçerliliğini kanıtlamak için birkaç talep belirteci doğrulamak için. Bir uygulama tarafından doğrulanmış talep senaryonun gereksinimlerine bağlı olarak farklılık gösterir, ancak uygulamanızı bazı gerçekleştirmelisiniz [ortak talep doğrulamaları](#validating-tokens) her senaryoda.

Talep ilgili tam Ayrıntılar aşağıdaki bölümlerde, bir örnek kimliği belirteci yanı sıra kimliği belirteçlerinde sunuyoruz. Talep Kimliği belirteçlerinde belirli bir sırada döndürülmez unutmayın. Ayrıca, yeni talep kimliği belirteçlere herhangi bir zamanda tanıtılabilir. Yeni Talep sunulduğunda, uygulamanızın çalışmamasına neden değil. Aşağıdaki listede, uygulamanız şu anda güvenilir bir şekilde çevirebilir talepleri içerir. Daha ayrıntılı bilgi bulabilirsiniz [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-id-token"></a>Örnek kimliği belirteci
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [!TIP]
> Uygulama için örnek kimliği belirtecinizdeki talepleri incelemek için örnek kimliği belirtece Yapıştır [jwt.ms](http://jwt.ms/).
>
>

#### <a name="claims-in-id-tokens"></a>Talep Kimliği belirteçlerinde
| Ad | İste | Örnek değer | Açıklama |
| --- | --- | --- | --- |
| Hedef kitle |`aud` |`6731de76-14a6-49ae-97bc-6eba6914391e` |Belirtecin hedeflenen alıcı tanımlar. Kimliği belirteçlerinde İzleyici uygulamanızın uygulama Microsoft uygulaması kayıt portalında uygulamanıza atanan kimliğidir. Uygulamanız bu değeri doğrulamak ve değer eşleşmiyorsa belirteci reddetme gerekir. |
| Veren |`iss` |`https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` |Oluşturur ve belirteci ve kullanıcı kimlik doğrulamasının yapıldığı Azure AD kiracısı döndüren güvenlik belirteci hizmeti (STS) tanımlar. Uygulamanızı belirteç v2.0 uç noktasından gelen emin olmak için verenin talep doğrulamalıdır. Bu aynı zamanda talep GUID bölümünü uygulamaya oturum açarak kiracılar sınırlamak için kullanmanız gerekir. Kullanıcı bir Microsoft hesabı tüketici kullanıcıdan geldiğini belirten GUID'dir `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| çıkışı |`iat` |`1452285331` |Belirteç düzenlendiği zaman dönem saatle gösterilir. |
| süre sonu |`exp` |`1452289231` |Hangi belirteci geçersiz hale geldiği tarih dönem saatle gösterilir. Uygulamanızı belirteç ömrü geçerliliğini doğrulamak için bu talep kullanmanız gerekir. |
| önce değil |`nbf` |`1452285331` |Hangi belirtecin geçerli olduğu zaman dönem saatle gösterilir. Genellikle verme süresi ile aynı değil. Uygulamanızı belirteç ömrü geçerliliğini doğrulamak için bu talep kullanmanız gerekir. |
| sürüm |`ver` |`2.0` |Azure AD tarafından tanımlandığı şekilde kimliği belirteci sürümü. V2.0 uç noktası için değerdir `2.0`. |
| kiracı kimliği |`tid` |`b9419818-09af-49c2-b0c3-653adc1f376e` |Kullanıcı bulunan Azure AD kiracısı temsil eden bir GUID. İş ve Okul hesapları için, kullanıcının ait olduğu kuruluşun değişmez Kiracı kimliği GUID'dir. Kişisel hesaplar için değerdir `9188040d-6c67-4c5b-b112-36a304b66dad`. `profile` Kapsam bu talebi almak için gereklidir. |
| kod karma |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Yalnızca bir OAuth 2.0 yetkilendirme koduyla kimliği belirteç kodu karma kimliği belirteçleri dahil edilir. Bir yetkilendirme kodu özgünlüğünü doğrulamak için kullanılabilir. Bu doğrulama gerçekleştirme hakkında daha fazla bilgi için bkz: [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html). |
| erişim belirteci karma |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Yalnızca zaman kimliği belirteci bir OAuth 2.0 erişim belirteci ile verilen belirteç karma Kodunda yer alan erişim belirteçleri. Bir erişim belirteci özgünlüğünü doğrulamak için kullanılabilir. Bu doğrulama gerçekleştirme hakkında daha fazla bilgi için bkz: [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html). |
| nonce |`nonce` |`12345` |Nonce belirteç yeniden yürütme saldırılarını Azaltıcı stratejidir. Uygulamanızı bir nonce bir yetkilendirme isteği kullanarak belirtebilirsiniz `nonce` sorgu parametresi. İstekte sağladığınız değerin kimliği belirtecinin yayılan `nonce` değiştirilmemiş talep. Uygulamanızı değeri uygulamanın oturumunu belirli bir kimliği belirteciyle ilişkilendirir istekte belirtilen değerle karşılaştırarak doğrulayın. Uygulamanız bu doğrulama kimliği belirteci doğrulama işlemi sırasında gerçekleştirmeniz gerekir. |
| ad |`name` |`Babe Ruth` |Ad talep belirteci konu tanımlayan okunabilir bir değer sağlar. Değerin benzersiz olması garanti edilmemiştir, değişebilir ve yalnızca görüntüleme amacıyla kullanılmak üzere tasarlanmıştır. `profile` Kapsam bu talebi almak için gereklidir. |
| e-posta |`email` |`thegreatbambino@nyy.onmicrosoft.com` |Varsa, kullanıcı hesabıyla ilişkili birincil e-posta adresi. Değerini değişebilir ve zaman içinde değişebilir. `email` Kapsam bu talebi almak için gereklidir. |
| tercih edilen kullanıcı adı |`preferred_username` |`thegreatbambino@nyy.onmicrosoft.com` |V2.0 uç kullanıcıyı temsil eden birincil kullanıcı adı. Bir e-posta adresi, telefon numarası ya da belirtilen biçim olmadan genel bir kullanıcı adı olabilir. Değerini değişebilir ve zaman içinde değişebilir. Değişebilir olduğundan, bu değer yetkilendirme kararları için kullanılmamalıdır. `profile` Kapsam bu talebi almak için gereklidir. |
| Konu |`sub` |`MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | Hakkında bilgi, bir uygulamanın kullanıcı gibi belirteci onaylar sorumlu. Bu değer sabittir ve yeniden atandığında yeniden ya da silinemez. Belirtecin bir kaynağa erişmek için kullanıldığında gibi güvenli bir şekilde, yetkilendirme denetimleri gerçekleştirmek için kullanılabilir ve, veritabanı tablolarındaki anahtar olarak kullanılan. Konu her zaman olduğu için Azure AD sorunları, bu değer bir genel amaçlı yetkilendirme sisteminde kullanmanızı öneririz, belirteçleri sunar. Konu, ancak, ikili bir tanımlayıcıdır. - belirli bir uygulama kimliği için benzersizdir  Bu nedenle, iki farklı istemci kimliği kullanarak iki farklı uygulamalarda tek bir kullanıcı oturum açtığında, bu uygulamaları konu talep için iki farklı değerler alır.  Bu olabilir veya mimarisi ve gizlilik gereksinimlerinize bağlı olarak gerekli değildir. |
| Nesne Kimliği |`oid` |`a1dbdde8-e4f9-4571-ad93-3059e3750d23` | Microsoft kimlik sistemi, bu durumda, bir kullanıcı hesabı nesnesi için değişmez tanımlayıcısı.  Ayrıca, veritabanı tablolarında güvenle ve bir anahtar olarak yetkilendirme denetimleri gerçekleştirmek için de kullanılabilir. Bu kimliği kullanıcı uygulamalar arasında benzersiz şekilde tanımlar.-aynı kullanıcı imzalama iki farklı uygulamaları, aynı değeri alacak `oid` talep.  Başka bir deyişle, bu sorguları Microsoft Graph gibi Microsoft online Services yaparken kullanılabilir.  Microsoft Graph bu kimliği olarak döndürülecek `id` özelliği için belirtilen kullanıcı hesabı.  Çünkü `oid` kullanıcılar ilişkilendirmek birden fazla uygulama verir `profile` kapsamı bu talebi almak için gereklidir. Tek bir kullanıcı birden fazla kiracılar varsa, kullanıcının her bir kiracı farklı nesne Kimliğinde içerecek Not - kullanıcı kimlik bilgileriyle her bir hesaba oturum olsa bile farklı hesaplar kabul edilir. |

### <a name="access-tokens"></a>Erişim belirteçleri

V2.0 uç noktası için Web API'leri gibi güvenli kaynaklara erişim belirteçlerini vermek için Azure AD ile kayıtlı üçüncü taraf uygulamalar sağlar. Uygulamaya erişim belirteçlerini vermek için ayarlama hakkında daha fazla bilgi için lütfen bkz [v2.0 uç noktası ile bir uygulama nasıl](active-directory-v2-app-registration.md). Uygulamanın v2.0 uç noktası ile kayıt sırasında geliştirici olarak adlandırılan erişim düzeyleri belirleyebilir **kapsamları**, erişim belirteçleri verilebilir. Örneğin, **calendars.read** Microsoft Graph API içinde tanımlanan kapsamı kullanıcının Takvim okuma izni verir. Uygulamanız v2.0 uç noktasından bir erişim belirteci aldığında, senaryonuza bağlı olarak belirtecin imza, veren, İzleyici, sona erme zamanı ve herhangi bir talep doğrulamanız gerekir. 

Bir erişim belirteci v2.0 uç noktasından istediğinde v2.0 uç de kullanmak, uygulamanız için erişim belirtecini hakkındaki meta verileri döndürür. Bu bilgiler erişim belirteci kapsamlar için geçerlidir ve sona erme saati içerir. Uygulamanız bu meta veriler akıllı erişim belirteçleri açık ayrıştırmak zorunda kalmadan erişim belirteci önbelleğe alma işlemi için kullanır.

### <a name="refresh-tokens"></a>Yenileme belirteçlerini
Yenileme belirteçleri olan bir OAuth 2.0 akışı yeni erişim belirteçleri almak için uygulamanızı kullanabilirsiniz güvenlik belirteçleri. Uygulamanız, kullanıcı etkileşimi gerektirmeden uzun vadeli kaynaklara erişimi bir kullanıcı adına elde etmek için yenileme belirteçleri kullanabilirsiniz.

Yenileme belirteçleri çok kaynak. Bir kaynak için bir belirteç isteğini sırasında alınan bir yenileme belirteci erişim belirteçleri tamamen farklı bir kaynak için kullanılan.

Bir yenileme belirteci yanıt olarak almak için uygulamanızı istemelisiniz ve verilmesi `offline_acesss` kapsam. Daha fazla bilgi edinmek için `offline_access` kapsam için bkz: [onay ve kapsamları](active-directory-v2-scopes.md) makale.

Yenileme belirteçlerini olan ve her zaman, uygulamanıza tamamen opak olacaktır. Bunlar Azure AD v2.0 uç noktası tarafından verilir ve yalnızca Denetlenmekte ve v2.0 uç noktası tarafından yorumlanır. Uzun süreli, ancak bir yenileme belirteci süre boyunca sürer beklenir uygulamanızı yazılmamalıysa. Yenileme belirteçleri geçersiz kılınan olabilir - Ayrıntılar için çeşitli nedenlerle herhangi bir anda bkz [belirteci iptal](active-directory-token-and-claims.md#token-revocation). Tek bir yenileme belirteci geçerli olup olmadığını bilmek, uygulamanız için bir belirteç isteğini v2.0 uç noktasına yaparak kullanma girişiminde yoludur.

Yeni bir erişim belirteci için bir yenileme belirteci almak zaman (ve uygulamanızı verilen `offline_access` kapsam), belirteç yanıt olarak yeni bir yenileme belirteci alma. İstekte kullanılan bir değiştirmek için yeni verilen yenileme belirteci kaydedin. Bu, yenileme belirteçleri mümkün olduğunca uzun bir süre geçerli kalacağını güvence altına alır.

## <a name="validating-tokens"></a>Belirteçleri doğrulanıyor
Şu anda, uygulamalarınızı yapmanız yalnızca belirteç doğrulama kimliği belirteçleri doğruluyor. Bir kimliği belirteci doğrulamak için uygulamanızın kimliği belirtecinin imzası ve kimliği belirtecinizdeki talepleri doğrulamalıdır.

<!-- TODO: Link -->
Microsoft, kitaplıklar ve kolayca belirteci doğrulama nasıl ele alınacağını gösteren kod örnekleri sağlar. Sonraki bölümlerde, temel alınan işlem açıklanmaktadır. Bazı üçüncü taraf açık kaynak kitaplıkları da JWT doğrulama için kullanılabilir. Neredeyse her platform ve dil için en az bir kitaplığın seçeneği yoktur.

### <a name="validate-the-signature"></a>İmza doğrulama
JWT'nin tarafından ayrılmış üç segmentleri içerir `.` karakter. İlk kesim olarak bilinen *üstbilgi*, ikinci kesim *gövde*, ve üçüncü kesim *imza*. İmza kesimi, böylece uygulamanız tarafından güvenilen kimliği belirteci özgünlüğünü doğrulamak için kullanılabilir.

Kimlik belirteçlerini RSA 256 gibi endüstri standardı asimetrik şifreleme algoritmaları kullanılarak imzalanmıştır. Kimliği belirteci üstbilgisi belirteç imzalamak için kullanılan anahtar ve şifreleme yöntemi hakkında bilgi sahiptir. Örneğin:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

`alg` Talep belirteç imzalamak için kullanılan algoritmayı belirtir. `kid` Talep belirteç imzalamak için kullanılan ortak anahtar gösterir.

Herhangi bir zamanda, belirli bir genel-özel anahtar çiftleri kümesini herhangi birini kullanarak v2.0 uç noktası kimliği belirteci imzalayabilir. Bu anahtar değişiklikleri otomatik olarak işlemek için uygulamanızı yazılması gereken şekilde v2.0 uç düzenli aralıklarla anahtarları, olası kümesini döndürür. 24 saatte v2.0 uç noktası tarafından kullanılan ortak anahtarlar için güncelleştirmeleri denetlemek için makul bir sıklığıdır.

Konumunda bulunan Openıd Connect meta veri belgesi kullanarak imzayı doğrulamak için gereken imzalama anahtar verileri elde edebilirsiniz:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [!TIP]
> URL'yi bir tarayıcıda deneyin!
>
>

Bu meta veri belgesi Openıd Connect kimlik doğrulaması için gereken çeşitli uç noktaları konumu gibi bilgileri, çeşitli yararlı parçalarını içeren bir JSON nesnesidir.  Belge de içeren bir *jwks_uri*, Belirteçleri imzalamak için kullanılan ortak anahtarlar konumunu sağlar. Jwks_uri bulunan JSON belgesi şu anda kullanımda olan tüm ortak anahtar bilgileri yok. Uygulamanızı kullanabilirsiniz `kid` bu belgede hangi ortak anahtarı bir belirteç imzalamak için kullanılan seçmek için JWT üstbilgisinde talep. Ardından, imza doğrulaması'nin doğru ortak anahtar ve belirtilen algoritmasını kullanarak gerçekleştirir.

İmza doğrulaması gerçekleştirme, bu belgenin kapsamı dışında olur. Birçok açık kaynak kitaplıkları, bu konuda yardımcı olmak kullanılabilir.

### <a name="validate-the-claims"></a>Talep doğrula
Uygulamanızı bir kimliği belirteci kullanıcı oturum açma sırasında aldığında bunu talep karşı birkaç denetimleri kimliği belirteçte gerçekleştirmelisiniz. Bunlar dahil ancak bunlarla sınırlı değildir:

* **Hedef kitle** talep kimliği belirteci uygulamanıza verilecek tasarlanmıştır doğrulamak için
* **önce değil** ve **süre sonu** talep kimliği belirtecin süresi geçmemiş doğrulamak için
* **veren** talep belirteci uygulamanıza v2.0 uç noktası tarafından verilmiş olduğunu doğrulamak için
* **nonce**, bir belirteç yeniden yürütme saldırı azaltma

Uygulamanızı gerçekleştirmesi gereken talep doğrulamaları tam bir listesi için bkz: [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Bu talep beklenen değerler ayrıntılarını dahil edilmiştir [kimlik belirteçlerini](# ID tokens) bölümü.

## <a name="token-lifetimes"></a>Belirteç ömürleri
Aşağıdaki belirteci yaşam süreleri bilgilerinizi yalnızca sunuyoruz. Geliştirmek ve uygulama hata ayıklama bilgileri size yardımcı olabilir. Uygulamalarınızı sabit kalması için bu yaşam süreleri hiçbirini beklenir yazılmamalıysa. Yaşam süresi can belirteci ve herhangi bir zamanda değiştirir.

| Belirteç | Yaşam süresi | Açıklama |
| --- | --- | --- |
| Kimlik belirteçlerini (iş veya Okul hesapları) |1 saat |Kimlik belirteçlerini, genellikle 1 saat boyunca geçerlidir. Web uygulamanız kendi oturumunda (önerilen) kullanıcı ile korumak için bu aynı yaşam kullanabilirsiniz veya tamamen farklı oturum yaşam süresi seçebilirsiniz. Uygulamanızı yeni bir kimliği belirtecini almak gerekirse, son noktanın yetkilendirilmesi yeni bir oturum açma isteği v2.0 için yapmanız gerekir. Kullanıcının geçerli tarayıcı oturumu v2.0 uç noktası ile varsa, kullanıcı kimlik bilgilerini yeniden girmek için gerekli olabilir değil. |
| Kimlik belirteçlerini (Kişisel hesapları) |24 saat |Kişisel hesaplar için kimlik belirteçlerini, genellikle 24 saat boyunca geçerlidir. Web uygulamanız kendi oturumunda (önerilen) kullanıcı ile korumak için bu aynı yaşam kullanabilirsiniz veya tamamen farklı oturum yaşam süresi seçebilirsiniz. Uygulamanızı yeni bir kimliği belirtecini almak gerekirse, son noktanın yetkilendirilmesi yeni bir oturum açma isteği v2.0 için yapmanız gerekir. Kullanıcının geçerli tarayıcı oturumu v2.0 uç noktası ile varsa, kullanıcı kimlik bilgilerini yeniden girmek için gerekli olabilir değil. |
| Erişim belirteçleri (iş veya Okul hesapları) |1 saat |Belirteç yanıtları belirteci meta verilerinin bir parçası belirtilir. |
| Erişim belirteçleri (Kişisel hesapları) |1 saat |Belirteç yanıtları belirteci meta verilerinin bir parçası belirtilir. Kişisel hesapları adına verilen erişim belirteçleri için farklı bir yaşam süresi yapılandırılabilir, ancak 1 saat normaldir. |
| Yenileme belirteçlerini (iş veya Okul hesabı) |14 gün |Bir tek yenileme belirteci 14 gün sayısı için geçerli değil. Ancak, uygulamanızı bir yenileme belirteci başarısız kadar veya isteğe bağlı olarak, uygulamanızın yeni bir yenileme belirteciyle değiştirir kadar kullanmayı denemeye devam şekilde yenileme belirtecini çeşitli nedenlerle herhangi bir zamanda geçersiz hale gelebilir. Bir yenileme belirteci ayrıca kullanıcının kimlik bilgilerini girdiği bu yana 90 gün olması durumunda geçersiz hale gelir. |
| Yenileme belirteçlerini (Kişisel hesapları) |En fazla 1 yıl |Bir tek yenileme belirteci en fazla 1 yıl için geçerlidir. Ancak, uygulamanızı kadar başarısız bir yenileme belirteci kullanmayı denemeye devam şekilde yenileme belirtecini çeşitli nedenlerle herhangi bir zamanda geçersiz hale gelebilir. |
| Yetkilendirme kodları (iş veya Okul hesapları) |10 dakika |Yetkilendirme kodları bilerek kısa süreli ve belirteçleri alındığında hemen erişim belirteçleri ve yenileme belirteçleri için kullanılan. |
| Yetkilendirme kodları (Kişisel hesapları) |5 dakika |Yetkilendirme kodları bilerek kısa süreli ve belirteçleri alındığında hemen erişim belirteçleri ve yenileme belirteçleri için kullanılan. Kişisel hesapları adına verilen yetkilendirme kodları tek seferlik kullanım içindir. |
