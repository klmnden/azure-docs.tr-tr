---
title: Belirteç başvurusu, Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C'de verilen belirteç türleri
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/16/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: adb78f04c0fd5ba175bb31c9a81ad58b3b03fb42
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447328"
---
# <a name="azure-ad-b2c-token-reference"></a>Azure AD B2C: Belirteç başvurusu

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), her işleme gibi çeşitli güvenlik belirteçleri yayar [kimlik doğrulama akışı](active-directory-b2c-apps.md). Bu belge, biçimi, güvenlik özellikleri ve her belirteç türünün içeriğini açıklar.

## <a name="types-of-tokens"></a>Belirteç türleri
Azure AD B2C'yi destekleyen [OAuth 2.0 Yetkilendirme Protokolü](active-directory-b2c-reference-protocols.md), her ikisi de kullanan ve erişim belirteçlerini yenileme belirteçleri. Kimlik doğrulaması ve oturum açma aracılığıyla da destekler [Openıd Connect](active-directory-b2c-reference-protocols.md), bir üçüncü Belirtecin türü sunar: kimlik belirteci. Bu belirteçlerden her biri, taşıyıcı belirteç olarak temsil edilir.

Taşıyıcı belirteç korumalı bir kaynağın "bearer" erişim veren bir basit güvenlik belirtecidir. Taşıyıcı belirteç sunabilir herhangi bir tarafa ' dir. Taşıyıcı belirteç almak için önce azure AD B2C'yi ilk kez bir taraf doğrulaması gerekir. Ancak gerekli adımları iletilmesini ve depolanmasını belirteci güvenliğini sağlamak için alınır değil, kesildi ve olması istenmeyen bir şahıs tarafından kullanılır. Bazı güvenlik belirteçleri yetkisiz taraflar bunları tüketmesini için yerleşik bir mekanizma vardır, ancak taşıyıcı belirteçleri Bu mekanizma yoktur. Bunlar, Aktarım Katmanı Güvenliği (HTTPS) gibi güvenli bir kanal taşınan gerekir.

Taşıyıcı belirteç dışında güvenli bir kanal iletilirse, kötü amaçlı bir taraf belirteç almak ve korumalı kaynağa yetkisiz erişim elde etmek için kullanmak için bir adam-de-ortadaki adam saldırısı kullanabilirsiniz. Depolanan ya da daha sonra kullanmak üzere önbelleğe taşıyıcı belirteçleri, aynı güvenlik ilkeleri uygulayın. Uygulamanızı iletir ve güvenli bir şekilde taşıyıcı belirteçleri depolar her zaman emin olmalısınız.

Taşıyıcı belirteçleri üzerinde ek güvenlik konuları için bkz. [RFC 6750 bölüm 5](http://tools.ietf.org/html/rfc6750).

Azure AD B2C sorunları belirteçleri birçoğu, JSON web belirteçleri (Jwt'ler) uygulanır. JWT'nin, iki taraf arasında bilgi aktarma compact, URL güvenli bir yoludur. Jwt'ler talepler olarak bilinen bilgileri içerir. Onaylar taşıyıcı hakkında bilgilerin ve belirtecin konu şunlardır. Jwt'ler Taleplerde kodlanmış ve aktarım için seri hale getirilmiş JSON nesneleridir. Azure AD B2C tarafından verilen Jwt'ler imzalanmış ancak şifreli olduğundan, kolayca hata ayıklamak için bir JWT içeriğini inceleyebilirsiniz. Çeşitli araçlar mevcuttur, bunu yapabilirsiniz, dahil olmak üzere [jwt.ms](https://jwt.ms). Jwt'ler hakkında daha fazla bilgi için [JWT belirtimleri](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>Kimliği belirteçleri

Bir kimlik belirteci, uygulamanızı Azure AD B2C'den aldığı güvenlik belirteci biçimindedir `/authorize` ve `/token` uç noktaları. Kimlik belirteçlerini olarak temsil edilir [Jwt'ler](#types-of-tokens), uygulamanızda kullanıcıları tanımlamak için kullanabileceğiniz talepleri içerir. Ne zaman kimlik belirteçlerini alınan gelen `/authorize` uç nokta, bunların yapılır kullanarak [örtük akış](active-directory-b2c-reference-spa.md), tabanlı web uygulamaları, genellikle oturum için javascript kullanıcıları için kullanılır. Ne zaman kimlik belirteçlerini alınan gelen `/token` uç nokta, bunların yapılır kullanarak [gizli kod akışını](active-directory-b2c-reference-oidc.md), belirteç tarayıcıdan gizli tutar. Bu, aynı uygulama veya hizmetin iki bileşenleri arasındaki iletişim için HTTP isteklerinde güvenli bir şekilde gönderilmesi için belirteci sağlar. Gördüğünüz gibi bir kimlik belirtecinde talep kullanabilirsiniz. Hesap bilgilerini görüntülemek veya erişim denetimi kararları bir uygulama için yaygın olarak kullanılır.  

Kimlik belirteçlerini imzalanmış, ancak şu anda şifrelenmez. Uygulamanızı veya API kimlik belirteci aldığında, bu gerekir [imzayı doğrulamak](#token-validation) belirteç gerçek olduğunu kanıtlamak için. Uygulamanızı veya API de geçerli olduğunu kanıtlamak için birkaç talep belirteci doğrulamanız gerekir. Bir uygulama tarafından doğrulanmış talep senaryo gereksinimlerine bağlı olarak farklılık gösterebilir, ancak uygulamanızı bazı gerçekleştirmelidir [ortak talep doğrulamaları](#token-validation) her senaryoda.

#### <a name="sample-id-token"></a>Örnek kimlik belirteci
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Erişim belirteçleri

Bir erişim belirteci de bir güvenlik belirteci, uygulamanızı Azure AD B2C'den aldığı biçimidir `/authorize` ve `/token` uç noktaları. Erişim belirteçleri olarak da gösterilir [Jwt'ler](#types-of-tokens), Apı'lerinizi için verilen izinleri belirlemek için kullanabileceğiniz talepleri içerir. Erişim belirteçleri imzalanmış, ancak şu anda şifrelenmez. Erişim belirteçleri, API'leri ve kaynak sunuculara erişim sağlamak için kullanılmalıdır. Kullanma hakkında daha fazla bilgi edinin [erişim belirteçleri kullanma](active-directory-b2c-access-tokens.md). 

API'nizi bir erişim belirteci aldığında, bu gerekir [imzayı doğrulamak](#token-validation) belirteç gerçek olduğunu kanıtlamak için. API'nizi de geçerli olduğunu kanıtlamak için birkaç talep belirteci doğrulamanız gerekir. Bir uygulama tarafından doğrulanmış talep senaryo gereksinimlerine bağlı olarak farklılık gösterebilir, ancak uygulamanızı bazı gerçekleştirmelidir [ortak talep doğrulamaları](#token-validation) her senaryoda.

### <a name="claims-in-id-and-access-tokens"></a>Talep Kimliği ve erişim belirteçlerinde

Azure AD B2C'yi kullandığınızda, içerik belirteçlerinizden birinin üzerinde ayrıntılı denetime sahiptir. Yapılandırabileceğiniz [ilkeleri](active-directory-b2c-reference-policies.md) belirli kullanıcı veri kümelerini işlemlerinde uygulamanız için gerekli talep göndermek için. Bu talepler kullanıcının gibi standart özellikleri içerebilir `displayName` ve `emailAddress`. Ayrıca içerebilir [özel kullanıcı öznitelikleri](active-directory-b2c-reference-custom-attr.md) , B2C dizininizde tanımlayabilirsiniz. Her bir kimlik ve erişim aldığınız belirteç talep ile ilgili belirli bir kümesini içerir. Uygulamalarınız, bu talepler, güvenli bir şekilde kullanıcılar ve isteklerinin kimliğini doğrulamak için kullanabilirsiniz.

Kimliği belirteçlere talep herhangi belirli bir sırada döndürülmediğini unutmayın. Ayrıca, yeni bir talep kimliği belirteçlerinde herhangi bir zamanda tanıtılabilir. Yeni Talep sunulduğu şekilde uygulamanızı kesmemesi. Azure AD B2C tarafından verilen kimlik ve erişim belirteçlerinde bulunmasını beklediğiniz talepleri aşağıda verilmiştir. Ek taleplerin ilkeleri tarafından belirlenir. Uygulama için örnek kimliği belirteçteki talepler içine yapıştırarak inceleyerek deneyin [jwt.ms](https://jwt.ms). Daha fazla ayrıntı bulunabilir [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html).

| Ad | İste | Örnek değer | Açıklama |
| --- | --- | --- | --- |
| Hedef kitle |`aud` |`90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` |Bir izleyici talep belirtecinin hedeflenen alıcı tanımlar. Azure AD B2C için hedef kitle, uygulamanızın uygulama kimliği, uygulama kayıt portalında uygulamanıza atanan aynıdır. Uygulamanız, bu değeri doğrulamak ve eşleşmiyorsa, belirteci reddetme. |
| Sertifikayı Veren |`iss` |`https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` |Bu talep, belirteci oluşturur ve güvenlik belirteci hizmeti (STS) tanımlar. Ayrıca, kullanıcı kimlik doğrulamasının yapıldığı Azure AD dizini tanımlar. Verenin talep, belirteci Azure Active Directory v2.0 uç noktasından gelen emin olmak için uygulamanızı doğrulamalıdır. |
| Çıkışı |`iat` |`1438535543` |Bu talep, belirteci, dönem saatle gösterilir düzenlendiği saattir. |
| Sona erme zamanı |`exp` |`1438539443` |Dönem zamanı temsil talep, belirteci geçersiz hale geldiği tarih sona erme saati. Uygulamanız bu talep belirteci ömrü geçerliliğini doğrulamak için kullanmanız gerekir. |
| Öncesine değil |`nbf` |`1438535543` |Bu talep, belirteci olur geçerli, dönem zamanı gösterilen zamandır. Bu genellikle belirtecin verilmiş süresiyle aynıdır. Uygulamanız bu talep belirteci ömrü geçerliliğini doğrulamak için kullanmanız gerekir. |
| Sürüm |`ver` |`1.0` |Bu kimlik belirteci, Azure AD tarafından tanımlandığı şekilde sürümüdür. |
| Kod karması |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Yalnızca belirteç ile birlikte bir OAuth 2.0 yetkilendirme kodu verildiğinde kod karma bir kimlik belirteci dahil edilir. Kod karma bir yetkilendirme kodu özgünlüğünü doğrulamak için kullanılabilir. Bu doğrulama gerçekleştirme hakkında daha fazla ayrıntı için bkz. [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html).  |
| Erişim belirteci karması |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Yalnızca belirteç OAuth 2.0 erişim belirteci ile birlikte verildiğinde bir erişim belirteci karma bir kimlik belirteci dahil edilir. Bir erişim belirteci karma bir erişim belirteci özgünlüğünü doğrulamak için kullanılabilir. Bu doğrulama gerçekleştirme hakkında daha fazla ayrıntı için bkz. [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html)  |
| nonce |`nonce` |`12345` |Nonce belirteç yeniden yürütme saldırıları azaltmak için kullanılan bir stratejidir. Uygulamanızı bir geçici öğe içinde bir yetkilendirme isteği kullanarak belirtebilirsiniz `nonce` sorgu parametresi. İstekte sağladığınız değeri içinde değiştirilmemiş yayılan `nonce` yalnızca bir kimlik belirteci talep. Bu uygulamanın oturum belirli bir kimlik belirteci ile ilişkilendirir istekte belirtilen değerle değeri doğrulamak için uygulamanıza sağlar. Uygulamanız kimlik belirteci doğrulama işlemi sırasında bu doğrulaması gerçekleştirmeniz gerekir. |
| Konu |`sub` |`884408e1-2918-4cz0-b12d-3aa027d7563b` |Sorumlu olduğu hakkında bir uygulamanın kullanıcı gibi bilgileri belirteci onaylar budur. Bu değer sabittir ve yeniden atandı yeniden veya değiştirilemez. Belirteç bir kaynağa erişmek için kullanıldığında gibi güvenli bir şekilde, yetkilendirme denetimleri gerçekleştirmek için kullanılabilir. Varsayılan olarak, konu talep, dizinde kullanıcının nesne kimliği ile doldurulur. Daha fazla bilgi için bkz. [Azure Active Directory B2C: belirteç, oturum ve çoklu oturum açma yapılandırması](active-directory-b2c-token-session-sso.md). |
| Kimlik doğrulaması bağlamı sınıf başvurusu |`acr` |Uygulanamaz |Şu anda kullanılmıyor, söz konusu olduğunda eski ilkeleri hariç. Daha fazla bilgi için bkz. [Azure Active Directory B2C: belirteç, oturum ve çoklu oturum açma yapılandırması](active-directory-b2c-token-session-sso.md). |
| Güven Framework İlkesi |`tfp` |`b2c_1_sign_in` |Bu kimlik belirteci almak için kullanılan ilke adıdır. |
| Kimlik doğrulama süresi |`auth_time` |`1438535543` |Bu talep, bir kullanıcı son girilen kimlik bilgileri içinde dönem zamanı temsil zamandır. |

### <a name="refresh-tokens"></a>Yenileme belirteçlerini
Yenileme belirteçler, uygulamanızın yeni kimlik belirteçlerini almak ve bir OAuth 2.0 akışı belirteçlerinde erişmek için kullanabileceğiniz güvenlik belirteçleri. Uygulamanız ile uzun süreli kaynaklara erişimi kullanıcılar adına bu kullanıcılarla etkileşimi gerektirmeden sağlarlar.

Bir yenileme almak için belirteci bir belirteç yanıt uygulamanızı istemelisiniz `offline_acesss` kapsam. Hakkında daha fazla bilgi edinmek için `offline_access` kapsam, başvurmak [Azure AD B2C Protokolü başvurusu](active-directory-b2c-reference-protocols.md).

Yenileme belirteçlerini olan ve her zaman, uygulamanıza tamamen opak olacaktır. Bunlar Azure AD tarafından verilen inceledi ve yalnızca Azure AD tarafından yorumlanır. Uzun süreli olduklarından, ancak uygulamanızı bir yenileme belirteci belirli bir süre için en son beklentisiyle yazılmamalıdır. Çeşitli nedenlerle için herhangi bir anda yenileme belirteçleri geçersiz olabilir. Azure AD'ye bir belirteç isteğini yaparak kullanmak uygulamanızı bir yenileme belirteci geçerli olup olmadığını öğrenmek için tek yolu denemektir.

Pass'inizi ne zaman yeni bir belirteç için bir yenileme belirteci (ve uygulamanızı verilmişse `offline_access` kapsam), belirteç yanıt olarak yeni bir yenileme belirteci alırsınız. Yeni verilen yenileme belirteci kaydetmeniz gerekir. Bu, daha önce istekte kullanılan yenileme belirteci değiştirmeniz gerekir. Bu, yenileme belirteçleri için mümkün olan en kısa sürece geçerli kalır garanti yardımcı olur.

## <a name="token-validation"></a>Belirteç doğrulama
Uygulamanızı bir belirteci doğrulamak için imza ve talep belirtecinin denetlemelisiniz.

Birçok açık kaynak kitaplıkları Jwt'ler, tercih ettiğiniz dili bağlı olarak doğrulamak için kullanılabilir. Bu seçenekleri keşfedin yerine kendi doğrulama mantığı uygulama öneririz. Bu kılavuzdaki bilgilerini düzgün şekilde bu kitaplıkları kullanmayı öğrenin yardımcı olabilir.

### <a name="validate-the-signature"></a>İmza doğrulama
JWT'nin ayrılmış üç segmentleri içeren `.` karakter. İlk parça olan *üstbilgi*, ikincisi ise *gövdesi*, ve üçüncü *imza*. İmza segment, böylece uygulamanız tarafından güvenilen belirteç özgünlüğünü doğrulamak için kullanılabilir.

Azure AD B2C belirteçleri, RSA 256 gibi endüstri standardı asimetrik şifreleme algoritmaları kullanarak imzalanmıştır. Belirteç üstbilgisi, belirteç imzalamak için kullanılan anahtar ve şifreleme yöntemi hakkında bilgi içerir:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

`alg` Talep, belirteç imzalamak için kullanılan algoritmayı belirtir. `kid` Talep, belirteç imzalamak için kullanılan belirli bir ortak anahtar gösterir.

Herhangi bir zamanda Azure AD belirli bir dizi ortak-özel anahtar çifti herhangi birini kullanarak bir belirteç oturum açabilirsiniz. Azure AD uygulamanızı anahtar bu değişiklikleri otomatik olarak işlemek üzere yazılmış olması gerekir böylece olası bir anahtar kümesini düzenli olarak döndürür. 24 saatte bir Azure AD tarafından kullanılan ortak anahtarlar için güncelleştirmeleri denetlemek için makul bir sıklığıdır.

Azure AD B2C'yi bir Openıd Connect meta veri uç noktası vardır. Bu, çalışma zamanında Azure AD B2C hakkında bilgi almak uygulamalar sağlar. Bu bilgiler, uç noktaları, belirteç içeriği ve belirteç imzalama anahtarı içerir. B2C dizininizi her ilke için bir JSON meta verileri belgesi içerir. Örneğin, meta veri belgesi için `b2c_1_sign_in` ilkesinde `fabrikamb2c.onmicrosoft.com` şu konumdadır:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com` Geçerli kullanıcının kimliğini doğrulamak için kullanılan bir B2C dizin ve `b2c_1_sign_in` belirteci almak için kullanılan ilke. İlkeyi bir belirteç imzalamak için kullanılan (ve meta verilerini getirmek için yapılması gerekenler) belirlemek için iki seçeneğiniz vardır. İlk olarak, ilke adı dahil `acr` belirtecinde talep. Gövde çözme ve sonuçları bir JSON dizesini seri durumdan çıkarılırken 64 tabanlı tarafından talep JWT gövdesinin dışında ayrıştırabilirsiniz. `acr` Talep, belirteci vermek için kullanılan ilke adı olacaktır.  Kullanabileceğiniz diğer seçenek değerini ilkesinde şifrelemektir `state` isteği ve hangi ilke kullanılan belirlemek için kod çözme parametresi. Her iki yöntem geçerli değil.

Meta veri belgesi birçok yararlı bilgi parçalarını içeren bir JSON nesnesidir. Bunlar, Openıd Connect kimlik doğrulaması gerçekleştirmek için gerekli uç noktaları konumunu içerir. Ayrıca içerirler `jwks_uri`, ortak anahtar kümesini konumunu, sağlayan Belirteçleri imzalamak için kullanılır. Konumu buraya sağlanır, ancak konum meta veri belgesi kullanarak ve kullanıma ayrıştırma dinamik olarak getirilemedi. en iyi `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

JSON belgesini bu URL'de bulunan tüm ortak anahtar bilgilerini belirli bir anda kullanımda içerir. Uygulamanız kullanabilir `kid` belirli bir belirteç imzalamak için kullanılan JSON belgesinde ortak anahtarı seçmek için JWT üst bilgisindeki talep. Doğru ortak anahtarı ve belirtilen algoritması kullanarak onu imza doğrulaması daha sonra gerçekleştirebilirsiniz.

Bu belgenin kapsamı dışında imza doğrulaması gerçekleştirme açıklamasıdır. İhtiyacınız olduğunda bu konuda yardım sağlayacak birçok açık kaynak kitaplıkları kullanılabilir.

### <a name="validate-the-claims"></a>Talepleri doğrula
Uygulamanızı veya API kimlik belirteci aldığında, bunu talepleri karşı çeşitli denetimleri kimliği belirteçteki gerçekleştirmelisiniz. Bunlar arasında ancak bunlarla sınırlı değildir:

* **İzleyici** talep: Bu kimlik belirteci uygulamanıza verilmesi amaçlanmamıştır doğrular.
* **Öncesine** ve **süre sonu** talepler: Bu kimlik belirteci sona ermediğinden emin olun.
* **Veren** talep: Bu belirteç için uygulamanızı Azure AD tarafından verildiğini onaylar.
* **Nonce**: belirteç yeniden yürütme saldırısı riskini azaltma için bir strateji budur.

Uygulamanızı gerçekleştirmesi gereken doğrulamaları tam bir listesi için başvurmak [Openıd Connect belirtimi](https://openid.net). Bu talepler için beklenen değerle ayrıntıları dahil edilir önceki [belirteç bölüm](#types-of-tokens).  

## <a name="token-lifetimes"></a>Belirteç kullanım ömrü
Daha fazla bilgi için aşağıdaki belirteç ömrünü sağlanır. Geliştirme ve uygulama hatalarını ayıklama bunlar yardımcı olabilir. Uygulamalarınızı herhangi birini bu yaşam süreleri sabit kalması beklenir yazılmamalıdır unutmayın. Bunlar, geçirebileceğiniz ve değiştirebileceğiniz. Daha fazla bilgi edinin [belirteç ömrünü özelleştirme](active-directory-b2c-token-session-sso.md) Azure AD B2C'de.

| Belirteç | Yaşam süresi | Açıklama |
| --- | --- | --- |
| Kimliği belirteçleri |Bir saat |Kimlik belirteçleri için bir saat genellikle geçerlidir. Web uygulamanız bu yaşam süresi (önerilen) kullanıcılarla kendi oturumları korumak için kullanabilirsiniz. Farklı oturum ömrünü de seçebilirsiniz. Yeni bir kimlik belirteci almak uygulamanız gerekiyorsa, yalnızca Azure AD ile yeni bir oturum açma isteği yapması gerekiyor. Bir kullanıcının Azure AD ile bir geçerli tarayıcı oturumunu varsa, bu kullanıcının kimlik bilgileri yeniden girmek için gerekli olabilir değil. |
| Yenileme belirteçlerini |En fazla 14 gün |En fazla 14 gün için geçerli bir tek bir yenileme belirtecidir. Ancak, bir yenileme belirteci için bir dizi nedenden herhangi bir zamanda geçersiz hale gelebilir. Uygulamanızı kadar istek başarısız olur ya da uygulamanız ile yeni bir yenileme belirteci değiştirir kadar bir yenileme belirteci kullanmayı denemeye devam etmelidir. Bir yenileme belirteci ayrıca kullanıcının son girilen kimlik bilgileri bu yana 90 gün geçtiğinde, geçersiz hale gelebilir. |
| Yetkilendirme kodları |Beş dakika |Yetkilendirme kodları kasıtlı olarak ömürlüdür. Alındığında bunlar hemen erişim belirteçleri, kimlik belirteçlerini veya yenileme belirteçleri kuponları. |

