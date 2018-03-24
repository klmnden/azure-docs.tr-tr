---
title: Başvuru - Azure AD B2C belirteç | Microsoft Docs
description: Azure Active Directory B2C'de yayınlanan belirteçleri türleri
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 08/16/2017
ms.author: davidmu
ms.openlocfilehash: e5cc6a0974f9481491518779209ec5256870921f
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-ad-b2c-token-reference"></a>Azure AD B2C: Belirteç başvurusu

Azure Active Directory B2C (Azure AD B2C) her işlediği gibi çeşitli güvenlik belirteçleri yayar [kimlik doğrulaması akışı](active-directory-b2c-apps.md). Bu belgede biçimi, güvenlik özellikleri ve her tür bir belirteç içeriği açıklanmaktadır.

## <a name="types-of-tokens"></a>Belirteç türleri
Azure AD B2C destekleyen [OAuth 2.0 yetkilendirme protokolünü](active-directory-b2c-reference-protocols.md), erişim belirteçleri ve yenileme belirteçleri hangi her ikisini de kullanır. Kimlik doğrulama ve oturum açma aracılığıyla da destekler [Openıd Connect](active-directory-b2c-reference-protocols.md), bir üçüncü Belirtecin türü sunar: kimliği belirteci. Her Bu belirteçler, taşıyıcı belirteç olarak temsil edilir.

Bir taşıyıcı belirteci korunan bir kaynağa "bearer" erişim veren bir basit güvenlik belirtecidir. Taşıyıcı belirteci sunabilir herhangi bir tarafa ' dir. Bir taşıyıcı belirteci almak için önce azure AD B2C önce bir taraf kimlik doğrulaması gerekir. Ancak belirteç iletim ve depolama güvenli hale getirmek için gerekli adımları katılmaz varsa, onu kullanılabilir ele ve istenmeyen bir şirket tarafından kullanılan. Bazı güvenlik belirteçleri kullanarak gelen yetkisiz tarafların engellemek için yerleşik bir mekanizma sahip, ancak taşıyıcı belirteçlerini bu düzenek sahip değil. Bunlar Aktarım Katmanı Güvenliği (HTTPS) gibi güvenli bir kanal taşınan gerekir.

Bir taşıyıcı belirteci dışında güvenli bir kanal iletilirse, kötü amaçlı bir taraf belirtecini almak ve korunan bir kaynağa yetkisiz erişim kazanmak için kullanmak üzere bir adam-in--middle saldırı kullanabilirsiniz. Taşıyıcı belirteçlerini depolanan veya daha sonra kullanmak için önbelleğe alınmış, aynı güvenlik ilkelerini uygular. Her zaman uygulamanızı aktarır ve güvenli bir şekilde taşıyıcı belirteçleri depolar emin olun.

Taşıyıcı belirteçlerini üzerinde ek güvenlik konuları için bkz: [RFC 6750 bölüm 5](http://tools.ietf.org/html/rfc6750).

Azure AD B2C sorunları belirteçleri çoğunu JSON web belirteçleri (Jwt'ler) uygulanır. JWT, iki taraf arasında bilgi aktarma compact, URL için güvenli bir yoludur. Jwt'ler talepler olarak bilinen bilgiler içerir. Bunlar, taşıyıcı hakkındaki bilgilerin onaylar ve belirteç konu olan. Jwt'ler talepleri, kodlanmış ve aktarım için seri JSON nesneleridir. Azure AD B2C tarafından verilen Jwt'ler imzalanmış ancak şifreli olduğundan, hata ayıklamak için JWT içeriğini kolayca inceleyebilirsiniz. Birkaç araç bulunmaktadır, yapabilir dahil olmak üzere bu [jwt.ms](https://jwt.ms). Jwt'ler hakkında daha fazla bilgi için bkz [JWT belirtimleri](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>Kimliği belirteçleri

Bir kimliği belirteci uygulamanızı Azure AD B2C ' aldığı güvenlik belirteci biçimidir `/authorize` ve `/token` uç noktaları. Kimlik belirteçlerini olarak temsil [Jwt'ler](#types-of-tokens), uygulamanızda kullanıcıları tanımlamak için kullanabileceğiniz talepleri içerir. Ne zaman kimlik belirteçlerini edinilen gelen `/authorize` uç noktası, bunlar yapılır bunu kullanarak [örtük akış](active-directory-b2c-reference-spa.md), temel web uygulamaları, genellikle kullanıcılar için javascript oturumu açmak için kullanılır. Ne zaman kimlik belirteçlerini edinilen gelen `/token` uç noktası, bunlar yapılır bunu kullanarak [gizli kod akış](active-directory-b2c-reference-oidc.md), tarayıcıdan gizli belirteci tutar. Bu, aynı uygulama veya hizmet iki bileşenleri arasındaki iletişim için HTTP isteklerinde güvenli bir şekilde gönderilmesi için belirteci sağlar. Uygun gördüğünüz şekilde kimliği belirteçte talep kullanabilirsiniz. Hesap bilgilerini görüntülemek veya erişim denetimi kararlarını bir uygulama için yaygın olarak kullanılır.  

Kimlik belirteçlerini imzalanmış, ancak şu anda şifrelenmez. Uygulamanızı veya API kimliği belirteci aldığında, gerekir [imzayı doğrulamak](#token-validation) belirteç gerçek olduğunu kanıtlamak için. Ayrıca, uygulama veya API geçerli olduğunu kanıtlamak için birkaç talep belirteci doğrulamanız gerekir. Senaryonun gereksinimlerine bağlı olarak, bir uygulama tarafından doğrulanmış talep farklılık gösterebilir, ancak uygulamanızı bazı gerçekleştirmelisiniz [ortak talep doğrulamaları](#token-validation) her senaryoda.

#### <a name="sample-id-token"></a>Örnek kimliği belirteci
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

Bir erişim belirteci Ayrıca uygulamanızı Azure AD B2C ' aldığı güvenlik belirteci biçimidir `/authorize` ve `/token` uç noktaları. Erişim belirteçleri ayrıca olarak temsil [Jwt'ler](#types-of-tokens), Apı'lerinizi için verilen izinleri tanımlamak için kullanabileceğiniz talepleri içerir. Erişim belirteçleri imzalanmış, ancak şu anda şifrelenmez. Erişim belirteçleri API'ler ve kaynak sunuculara erişmesini sağlamak için kullanılmalıdır. Nasıl yapılır hakkında daha fazla bilgi [erişim belirteçleri kullanmak](active-directory-b2c-access-tokens.md). 

API'nizi bir erişim belirteci aldığında, gerekir [imzayı doğrulamak](#token-validation) belirteç gerçek olduğunu kanıtlamak için. API'nizi da geçerli olduğunu kanıtlamak için birkaç talep belirteci doğrulamanız gerekir. Senaryonun gereksinimlerine bağlı olarak, bir uygulama tarafından doğrulanmış talep farklılık gösterebilir, ancak uygulamanızı bazı gerçekleştirmelisiniz [ortak talep doğrulamaları](#token-validation) her senaryoda.

### <a name="claims-in-id-and-access-tokens"></a>Kimlik ve erişim belirteçleri talepleri

Azure AD B2C kullandığınızda, belirteçlerinizi içeriğini hassas denetime sahip olursunuz. Yapılandırabileceğiniz [ilkeleri](active-directory-b2c-reference-policies.md) belirli kullanıcı veri kümelerinin kendi işlemleri için uygulamanızı gerektiren talepleri göndermek için. Bu talep kullanıcının gibi standart özellikler içerebilir `displayName` ve `emailAddress`. Ayrıca içerebilir [özel kullanıcı öznitelikleri](active-directory-b2c-reference-custom-attr.md) , B2C dizininizde tanımlayabilirsiniz. Her kimlik ve erişim aldığınız belirteç talep güvenlikle ilgili belirli bir kümesini içerir. Uygulamalarınızı, bu talepler güvenli bir şekilde kullanıcılar ve istekleri kimlik doğrulaması yapmak için kullanabilirsiniz.

Talep Kimliği belirteçlerinde belirli bir sırada döndürülmez unutmayın. Ayrıca, yeni talep kimliği belirteçleri herhangi bir zamanda tanıtılabilir. Yeni Talep sunulduğu şekilde uygulamanızı parçalara değil. Burada, Azure AD B2C tarafından yayınlanan kimliği ve erişim belirteçleri mevcut beklediğiniz talep bulunmaktadır. Herhangi bir ek talep ilkeleri tarafından belirlenir. Uygulama için örnek kimliği belirtecinizdeki talepleri içine yapıştırarak inceleniyor deneyin [jwt.ms](https://jwt.ms). Daha fazla ayrıntı bulunabilir [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html).

| Ad | İste | Örnek değer | Açıklama |
| --- | --- | --- | --- |
| Hedef kitle |`aud` |`90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` |Bir izleyici talep belirtecinin hedeflenen alıcı tanımlar. Azure AD B2C için hedef kitle, uygulamanızın uygulama kimliği, uygulama kayıt portalında uygulamanıza atanan aynıdır. Uygulamanız bu değeri doğrulamak ve onu eşleşmiyorsa belirteci reddetme gerekir. |
| Veren: |`iss` |`https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` |Bu talep oluşturur ve belirteci döndüren güvenlik belirteci hizmeti (STS) tanımlar. Ayrıca, kullanıcı kimlik doğrulamasının yapıldığı Azure AD dizini tanımlar. Uygulamanız Azure Active Directory v2.0 uç noktasından belirteç geldiğini emin olmak için verenin talep doğrulamalıdır. |
| çıkışı |`iat` |`1438535543` |Bu talep belirteci, dönem saatle gösterilir düzenlendiği zamandır. |
| Sona erme zamanı |`exp` |`1438539443` |Dönem zaman temsil hangi belirteci geçersiz hale geldiği tarih talep sona erme saati. Uygulamanızı belirteç ömrü geçerliliğini doğrulamak için bu talep kullanmanız gerekir. |
| önce değil |`nbf` |`1438535543` |Bu talep aktarılma belirteç olur geçerli, dönem zaman gösterilen saattir. Bu genellikle belirtecin verilmiş süresiyle aynıdır. Uygulamanızı belirteç ömrü geçerliliğini doğrulamak için bu talep kullanmanız gerekir. |
| Sürüm |`ver` |`1.0` |Bu kimliği belirteci Azure AD tarafından tanımlandığı şekilde sürümüdür. |
| kod karma |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Yalnızca bir OAuth 2.0 yetkilendirme kodu ile birlikte belirtecin verildiğinde bir kod karma bir kimlik belirtecinde dahil edilir. Kod karma bir kimlik doğrulama kodu özgünlüğünü doğrulamak için kullanılabilir. Bu doğrulama gerçekleştirme konusunda daha fazla ayrıntı için bkz: [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html).  |
| erişim belirteci karma |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Yalnızca bir OAuth 2.0 erişim belirteci ile birlikte belirtecin verildiğinde bir erişim belirteci karma bir kimlik belirtecinde dahil edilir. Bir erişim belirteci karma bir erişim belirteci özgünlüğünü doğrulamak için kullanılabilir. Bu doğrulama gerçekleştirme konusunda daha fazla ayrıntı için bkz: [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html)  |
| nonce |`nonce` |`12345` |Nonce belirteç yeniden yürütme saldırıları azaltmak için kullanılan bir stratejidir. Uygulamanızı bir nonce bir yetkilendirme isteği kullanarak belirtebilirsiniz `nonce` sorgu parametresi. İstekte sağladığınız değerin içinde değiştirilmemiş yayılan `nonce` yalnızca bir kimliği belirtecini talep. Bu uygulamanın oturumunu belirli bir kimliği belirteciyle ilişkilendirir istekte belirtilen değerle değerini doğrulamak uygulamanızı sağlar. Uygulamanız bu doğrulama kimliği belirteci doğrulama işlemi sırasında gerçekleştirmeniz gerekir. |
| Konu |`sub` |`884408e1-2918-4cz0-b12d-3aa027d7563b` |Hakkında bilgi, bir uygulamanın kullanıcı gibi belirteci onaylar asıl budur. Bu değer sabittir ve yeniden atandığında yeniden ya da silinemez. Belirtecin bir kaynağa erişmek için kullanıldığında gibi güvenli bir şekilde, yetkilendirme denetimleri gerçekleştirmek için kullanılabilir. Varsayılan olarak, konu talep dizindeki kullanıcı nesne kimliği ile doldurulur. Daha fazla bilgi için bkz: [Azure Active Directory B2C: belirteci, oturum ve tek oturum açma yapılandırması](active-directory-b2c-token-session-sso.md). |
| Kimlik doğrulama bağlamı sınıf başvurusu |`acr` |Uygulanamaz |Şu anda kullanılmayan, söz konusu olduğunda eski ilkeleri dışındaki. Daha fazla bilgi için bkz: [Azure Active Directory B2C: belirteci, oturum ve tek oturum açma yapılandırması](active-directory-b2c-token-session-sso.md). |
| Güven Framework İlkesi |`tfp` |`b2c_1_sign_in` |Kimliği belirtecini almak için kullanılan ilkeyi adıdır. |
| Kimlik doğrulama süresi |`auth_time` |`1438535543` |Bu talep bir kullanıcı son girilen kimlik bilgileri, dönem zamanında temsil saattir. |

### <a name="refresh-tokens"></a>Yenileme belirteçlerini
Yenileme belirteçleri olan uygulamanızı yeni kimlik belirteçlerini edinmeli ve bir OAuth 2.0 akışı belirteçleri erişmek için kullanabileceğiniz güvenlik belirteçleri. Uygulamanızı uzun vadeli kaynaklara erişimi kullanıcılar adına sahip olan kullanıcılar ile etkileşimi gerektirmeden sağlarlar.

Bir yenileme almak için bir belirteç yanıt belirteç uygulamanızı istemelisiniz `offline_acesss` kapsam. Daha fazla bilgi edinmek için `offline_access` kapsam, başvurmak [Azure AD B2C Protokolü başvurusu](active-directory-b2c-reference-protocols.md).

Yenileme belirteçlerini olan ve her zaman, uygulamanıza tamamen opak olacaktır. Bunlar Azure AD tarafından verilen ve Denetlenmekte ve yalnızca Azure AD tarafından yorumlanır. Uzun süreli, ancak bir yenileme belirteci belirli bir döneme ait en son Beklenti ile uygulamanızı yazılmamalıysa. Çeşitli nedenlerle için herhangi bir anda yenileme belirteçleri geçersiz olabilir. Tek bir yenileme belirteci geçerli olup olmadığını bilmek, uygulamanız için Azure AD için bir belirteç isteğini yaparak kullanma girişiminde yoludur.

Bir yenileme belirteci için yeni bir belirteç almak zaman (ve uygulamanızı verilirse `offline_access` kapsam), belirteç yanıt olarak yeni bir yenileme belirteci alırsınız. Yeni verilen yenileme belirteci kaydetmeniz gerekir. Daha önce istekte kullanılan yenileme belirteci değiştirmeniz gerekir. Bu, yenileme belirteçleri mümkün olduğunca uzun bir süre geçerli kalacağını garanti yardımcı olur.

## <a name="token-validation"></a>Belirteç doğrulama
Bir belirteci doğrulamak için uygulamanızı imza ve talep belirtecinin denetlemelisiniz.

Birçok açık kaynak kitaplıkları Jwt'ler, tercih ettiğiniz dili bağlı olarak doğrulamak için kullanılabilir. Bu seçenek keşfetmek yerine kendi doğrulama mantığını uygulamak öneririz. Bu kılavuzda yer alan bilgiler, bu kitaplıkları düzgün bir şekilde kullanmayı öğrenmek yardımcı olabilir.

### <a name="validate-the-signature"></a>İmza doğrulama
JWT ayrılmış üç parçaları içeren `.` karakter. İlk kesim *üstbilgi*, ikincisi ise *gövde*, ve üçüncü *imza*. İmza kesimi, böylece uygulamanız tarafından güvenilen belirteç özgünlüğünü doğrulamak için kullanılabilir.

Azure AD B2C belirteç RSA 256 gibi endüstri standardı asimetrik şifreleme algoritmaları kullanılarak imzalanmıştır. Belirteç üstbilgisi belirteç imzalamak için kullanılan anahtar ve şifreleme yöntemi hakkında bilgi içerir:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

`alg` Talep belirteç imzalamak için kullanılan algoritmayı belirtir. `kid` Talep belirteç imzalamak için kullanılan belirli ortak anahtarı gösterir.

Belirli bir zamanda, Azure AD belirli bir genel-özel anahtar çiftleri kümesini herhangi birini kullanarak bir belirteç oturum açabilirsiniz. Bu anahtar değişiklikleri otomatik olarak işlemek için uygulamanızı yazılması gereken şekilde azure AD anahtarları olası kümesini düzenli olarak döndürür. 24 saatte bir Azure AD tarafından kullanılan ortak anahtarlar için güncelleştirmeleri denetlemek için makul bir sıklığıdır.

Azure AD B2C Openıd Connect meta veri son nokta vardır. Bu, çalışma zamanında Azure AD B2C hakkında bilgi almak uygulamalarının sağlar. Bu bilgiler, uç noktaları, belirteç içerikleri ve belirteç imzalama anahtarları içerir. Her ilke için bir JSON meta veri belgesi, B2C dizini içerir. Örneğin, meta veri belgesi için `b2c_1_sign_in` ilkesinde `fabrikamb2c.onmicrosoft.com` bulunur:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com` kullanıcının kimliğini doğrulamak için kullanılan B2C dizini ve `b2c_1_sign_in` belirtecini almak için kullanılan ilkesi. İlkeyi bir belirteç imzalamak için kullanılan (ve meta verileri almak için nereye) belirlemek için iki seçeneğiniz vardır. İlk olarak, ilke adını dahil `acr` belirteç talep. Gövde kod çözme ve sonuçları JSON dizesinde seri 64 tabanlı tarafından talep JWT gövdesi dışında ayrıştıramıyor. `acr` Talep belirteci vermek için kullanılan ilkenin adı olacaktır.  Değerini ilkesinde kodlamak için diğer seçeneğiniz olduğunu `state` isteği gönderin ve ardından ilkeyi kullanılan belirlemek için kod çözme parametresi. Her iki yöntem geçerli değil.

Meta veri belgesi birkaç yararlı bilgi parçalarını içeren bir JSON nesnesidir. Bunlar, Openıd Connect kimlik doğrulama gerçekleştirmek için gerekli uç noktaları konumunu içerir. Ayrıca içerirler `jwks_uri`, hangi ortak anahtarlar konumunu gösteren Belirteçleri imzalamak için kullanılır. Konum burada sağlanır, ancak konum meta veri belgesi kullanarak ve çıkışı ayrıştırma dinamik olarak getirilemedi en iyisidir `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

Bu URL'de bulunan JSON belgesi tüm ortak anahtar bilgileri belirli bir anda kullanımda içerir. Uygulamanızı kullanabilirsiniz `kid` JSON belgenin belirli bir belirteç imzalamak için kullanılan ortak anahtar seçmek için JWT üstbilgisinde talep. Doğru ortak anahtar ve belirtilen algoritma kullanarak bunu imza doğrulaması daha sonra gerçekleştirebilirsiniz.

Bu belgenin kapsamı dışında imza doğrulaması gerçekleştirme açıklamasıdır. Birçok açık kaynak kitaplıkları ihtiyacınız olursa, bu konuda yardımcı olmak kullanılabilir.

### <a name="validate-the-claims"></a>Talep doğrula
Uygulamanızı veya API kimliği belirteci aldığında bunu talep karşı bazı denetimleri kimliği belirteçte gerçekleştirmelisiniz. Bunlar arasında ancak bunlarla sınırlı değildir:

* **İzleyici** talep: Bu kimliği belirteci uygulamanıza verilecek tasarlanmıştır doğrular.
* **Önce değil** ve **süre sonu** talep: Bu kimliği belirtecin süresi geçmemiş doğrulayın.
* **Veren** talep: Bu belirteç, uygulamanızın Azure AD tarafından verildiğini doğrular.
* **Nonce**: belirteç yeniden yürütme saldırı azaltma için bir strateji budur.

Uygulamanızı gerçekleştirmesi gerektiğini doğrulamaları tam bir listesi için başvurmak [Openıd Connect belirtimi](https://openid.net). Bu talep beklenen değerler ayrıntılarını yer alan önceki [belirteci bölüm](#types-of-tokens).  

## <a name="token-lifetimes"></a>Belirteç ömürleri
Aşağıdaki belirteci yaşam süreleri bilginiz ilerletmek için sağlanır. Geliştirme ve hata ayıklama uygulamaları bunlar yardımcı olabilir. Uygulamalarınızı sabit kalması için bu yaşam süreleri hiçbirini beklenir yazılmamalıysa unutmayın. Bunlar olabilir ve değiştirir. Daha fazla bilgi edinin [belirteci yaşam süreleri özelleştirmesini](active-directory-b2c-token-session-sso.md) Azure AD B2C'de.

| Belirteç | Yaşam süresi | Açıklama |
| --- | --- | --- |
| Kimliği belirteçleri |Bir saat |Kimlik belirteçlerini bir saat için genellikle geçerlidir. Web uygulamanızın bu yaşam süresi (önerilen) kullanıcılarla kendi oturumları korumak için kullanabilirsiniz. Ayrıca, farklı oturum yaşam süresi seçebilirsiniz. Uygulamanızı yeni bir kimliği belirtecini almak gerekirse, yalnızca Azure AD ile yeni bir oturum açma isteği yapmak gerekir. Geçerli tarayıcı oturumu Azure AD ile bir kullanıcı varsa, bu kullanıcının kimlik bilgilerini yeniden girmek için gerekli olabilir değil. |
| Yenileme belirteçlerini |14 gün |Bir tek yenileme belirteci 14 gün sayısı için geçerli değil. Ancak, bir yenileme belirteci pek çok zaman geçersiz hale gelebilir. Uygulamanızı bir yenileme belirteci isteği başarısız kadar veya uygulamanız ile yeni bir yenileme belirteci değiştirir kadar kullanmayı denemeye devam etmelidir. Bir yenileme belirteci Ayrıca kullanıcı son girilen kimlik bilgileri üzerinden 90 gün geçtiğinde, geçersiz hale gelebilir. |
| Yetkilendirme kodları |Beş dakika |Yetkilendirme kodları bilerek ömürlüdür. Bunlar alındığında bunlar hemen erişim belirteçleri, kimlik belirteçlerini veya yenileme belirteçleri için kullanılan. |

