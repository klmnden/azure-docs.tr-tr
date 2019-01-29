---
title: Openıd Connect, Azure Active Directory B2C ile oturum açma Web | Microsoft Docs
description: Azure Active Directory uygulaması Openıd Connect kimlik doğrulama protokolü kullanarak Web uygulamaları oluşturma.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 39a3164c27fa30250fe08e864db889eac844f646
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173012"
---
# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: OpenID Connect ile web oturumu açma
Openıd Connect, kullanıcıların web uygulamalarına güvenli bir şekilde oturum açmak için kullanılan OAuth 2.0 üzerinde yerleşik bir kimlik doğrulama protokolü olan. Azure Active Directory B2C kullanarak Openıd Connect (Azure AD B2C) uygulaması, devredebilir kaydolma, oturum açma ve web uygulamalarınızı Azure Active Directory (Azure AD) içinde diğer kimlik yönetimi deneyimleri. Bu kılavuz dilden bağımsız bir şekilde bunu nasıl gösterir. Bu, HTTP iletileri gönderip bizim açık kaynak kitaplıkları hiçbirini kullanmadan nasıl açıklar.

[Openıd Connect](https://openid.net/specs/openid-connect-core-1_0.html) OAuth 2.0 genişletir *yetkilendirme* protokolü olarak kullanılmak üzere bir *kimlik doğrulaması* protokolü. OAuth kullanarak, çoklu oturum açma gerçekleştirmenizi sağlar. Kavramını sunar bir *kimlik belirteci*, kullanıcının kimliğini doğrulamak ve kullanıcının temel profil bilgilerini almak istemci izin veren bir güvenlik belirteci olduğu.

Ayrıca, OAuth 2.0 genişlettiğinden, güvenli bir şekilde almak uygulamalar sağlar *erişim belirteçlerini*. Access_tokens tarafından güvenliği sağlanan kaynaklara erişmek için kullanabileceğiniz bir [yetkilendirme sunucusu](active-directory-b2c-reference-protocols.md#the-basics). Bir sunucuda barındırılan ve tarayıcı aracılığıyla erişilen bir web uygulaması derliyorsanız, Openıd Connect öneririz. Azure AD B2C'yi kullanarak, mobil veya Masaüstü uygulamalarınız için Kimlik Yönetimi eklemek istiyorsanız, kullanması gereken [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) Openıd Connect yerine.

Basit kimlik doğrulaması ve yetkilendirme birden yapmak için standart olan Openıd Connect protokolü, Azure AD B2C'yi genişletir. Tanıttığı [kullanıcı akışı parametre](active-directory-b2c-reference-policies.md), Openıd Connect gibi kullanıcı deneyimlerini--eklemek için kullanmanıza olanak sağlayan kaydolma, oturum açma ve profil yönetimi--uygulamanıza. Burada, Openıd Connect ve kullanıcı akışları her biri bu deneyimler, web uygulamalarınızda uygulamak için nasıl kullanılacağını göstereceğiz. Ayrıca web API'leri erişmek için erişim belirteçlerini almak nasıl göstereceğiz.

Bizim örnek B2C dizini, fabrikamb2c.onmicrosoft.com yanı sıra örnek uygulamamız örnek HTTP istekleri sonraki bölümde kullanmak https://aadb2cplayground.azurewebsites.netve kullanıcı akışları. İstekleri bu değerleri kullanarak kendiniz denemek ücretsiz veya bunları kendi değerlerinizle değiştirebilirsiniz.
Bilgi edinmek için nasıl [kendi B2C Kiracı, uygulama ve kullanıcı Akışları Al](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Kimlik doğrulama isteği gönder
Web uygulamanız, kullanıcının kimliğini doğrulamak ve kullanıcı akışı yürütmek gerektiğinde kullanıcıya yönlendirebilir `/authorize` uç noktası. Bu etkileşimli akış burada kullanıcı, kullanıcı akışı bağlı olarak bir eylemde bölümüdür.

Bu istekte istemci, kullanıcıdan almak için gereken izinleri belirtir. `scope` parametresi ve yürütüleceği kullanıcı akışı `p` parametresi. Üç örnek, her farklı kullanıcı akışı kullanarak (ile okunabilirlik için satır sonları), aşağıdaki bölümlerde verilmiştir. Nasıl çalıştığını her istek için bir genel görünüm almak için isteği bir tarayıcıya yapıştırarak bunu çalıştırmayı deneyin.

#### <a name="use-a-sign-in-user-flow"></a>Bir kullanıcı oturum açma akışını kullanın
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-user-flow"></a>Kaydolma kullanıcı akışı kullanın
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-user-flow"></a>Bir profili Düzenle kullanıcı akışını kullanın
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
| client_id |Gereklidir |Uygulama Kimliği [Azure portalında](https://portal.azure.com/) uygulamanıza atanan. |
| response_type |Gereklidir |Yanıt türü için Openıd Connect kimlik belirteci içermelidir. Ayrıca web uygulamanızı bir web API'sini çağırmak için belirteçleri erişmesi gerekiyorsa, kullanabileceğiniz `code+id_token`burada yaptığımız gibi. |
| redirect_uri |Önerilen |`redirect_uri` Uygulamanızın, burada kimlik doğrulama yanıtlarının gönderilebilen veya uygulamanız tarafından alınan parametresi. Bu tam olarak biri eşleşmelidir `redirect_uri` URL olarak kodlanmış olması dışında Portalı'nda kayıtlı parametreleri. |
| scope |Gereklidir |Kapsamları boşlukla ayrılmış listesi. Tek bir kapsam değeri, talep edilen hem izinleri Azure AD'ye gösterir. `openid` Kapsamı kullanıcının oturum açmasını ve kullanıcı kimliği belirteçleri (Bu makalenin sonraki bölümlerinde gelmesini daha) biçiminde hakkında veri alma izni gösterir. `offline_access` Kapsamı, web uygulamaları için isteğe bağlıdır. Uygulamanıza gerekecek gösteren bir *yenileme belirteci* kaynaklarına uzun süreli erişim için. |
| response_mode |Önerilen |Ortaya çıkan bir yetkilendirme kodu uygulamanıza geri göndermek için kullanılması gereken yöntemini. Ya da olabilir `query`, `form_post`, veya `fragment`.  `form_post` Yanıt modu, en iyi güvenlik için önerilir. |
| durum |Önerilen |Ayrıca belirteci yanıtta döndürülen isteğinde bulunan bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Rastgele oluşturulmuş bir benzersiz değeri, genellikle siteler arası istek sahteciliği saldırılarına önlemek için kullanılır. Durumu, uygulama kullanıcının durumu hakkında bilgi, bulunduğunuz sayfaya gibi kimlik doğrulama isteği oluşmadan önce kodlamak için de kullanılır. |
| nonce |Gereklidir |Sonuçta elde edilen kimlik belirtecinde talep olarak dahil edilecek (uygulama tarafından oluşturulan) istek içindeki bir değer. Uygulama, belirteç yeniden yürütme saldırıları azaltmak için bu değer daha sonra doğrulayabilirsiniz. Genellikle istek kaynağı tanımlamak için kullanılan benzersiz bir rastgele dize değeridir. |
| p |Gereklidir |Yürütülecek kullanıcı akışı. B2C kiracınızda oluşturulan kullanıcı akışı adıdır. Kullanıcı akışı ad değer ile başlaması gereken `b2c\_1\_`. İlkeleri hakkında daha fazla bilgi edinin ve [Genişletilebilir kullanıcı akışı çerçevesi](active-directory-b2c-reference-policies.md). |
| istemi |İsteğe bağlı |Gerekli olan kullanıcı etkileşimi türü. Şu anda geçerli olan `login`, bu isteği kimlik bilgilerini girmesini zorlar. Çoklu oturum açma etkili olmaz. |

Bu noktada, kullanıcı kullanıcı Akış iş akışı tamamlamanız istenir. Bu dizinin veya herhangi bir kullanıcı akışı nasıl tanımlandığını bağlı olarak, adım sayısı kaydolma bir sosyal kimlik bilgilerinizle oturum, kullanıcı adını ve parolasını girerek kullanıcı gerektirebilir.

Kullanıcının kullanıcı akışı tamamlandıktan sonra Azure AD uygulamanızı belirtilen bir yanıt döndürür `redirect_uri` parametresi, belirtilen yöntemi kullanarak `response_mode` parametresi. Yanıt, yürütülen kullanıcı akışını bağımsız önceki örneklerin her biri için aynıdır.

Başarılı yanıt kullanarak bir `response_mode=fragment` şöyle görünmelidir:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --- | --- |
| id_token |Uygulama istenen kimlik belirteci. Kimlik belirteci, kullanıcının kimliğini doğrulamak ve kullanıcıyı bir oturum başlatmak için kullanabilirsiniz. Kimlik belirteçlerini ve içerikleri daha fazla ayrıntı dahil [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). |
| kod |Yetkilendirme kodu kullandıysanız, uygulamayı, istenen `response_type=code+id_token`. Uygulama, bir hedef kaynak için bir erişim belirteci istemek için yetkilendirme kodu kullanabilirsiniz. Yetkilendirme kodları çok kısa ömürlüdür. Genellikle, yaklaşık 10 dakika sonra süresi. |
| durum |Varsa bir `state` aynı değeri yanıt olarak görünmesi gereken parametresi istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerleri aynıdır. |

Hata yanıtları da gönderilebilir `redirect_uri` parametresi uygulama bunları uygun şekilde işleyebilmeniz:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --- | --- |
| error |Hataları tepki vermek için kullanılabilir ve oluşan hataların türlerini sınıflandırmak için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |
| durum |Bu bölümdeki ilk tablonun tam açıklamasına bakın. Varsa bir `state` aynı değeri yanıt olarak görünmesi gereken parametresi istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerleri aynıdır. |

## <a name="validate-the-id-token"></a>Kimlik belirteci doğrulama
Yalnızca bir kimlik belirteci alma kullanıcının kimliğini doğrulamak yeterli değil. Kimlik belirtecinin imzası doğrulama ve uygulamanızın gereksinimlerini başına belirteçteki talepleri doğrulamak gerekir. Azure AD B2C'yi kullanan [JSON Web belirteçleri (Jwt'ler)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ve Belirteçleri imzalamak ve geçerli olduğunu doğrulamak için ortak anahtar şifrelemesi.

Jwt'ler, tercih dilinizi bağlı olarak doğrulamak için kullanılabilen birçok açık kaynak kitaplıkları vardır. Kendi doğrulama mantığını uygulama yerine bu seçenekleri keşfetmeye öneririz. Buradaki bilgileri düzgün bir şekilde bu kitaplıkları kullanmayı ayırabiliriz yararlı olacaktır.

Azure AD B2C, çalışma zamanında Azure AD B2C hakkında bilgi almak uygulama izin veren bir Openıd Connect meta veri uç noktası, sahiptir. Bu bilgiler, uç noktaları, belirteç içeriği ve belirteç imzalama anahtarı içerir. B2C kiracınızda bulunan her bir kullanıcı akışı için bir JSON meta verileri belgesi yoktur. Örneğin, meta veri belgesi için `b2c_1_sign_in` kullanıcı akışını `fabrikamb2c.onmicrosoft.com` şu konumdadır:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Bu yapılandırma belgenin özelliklerinden biri `jwks_uri`, değeri aynı kullanıcı akışı olacaktır:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Hangi kullanıcı akışı Kimliğiniz oturum açarken kullanılan belirlemek için belirteci (ve nereden meta verilerini getirmek), iki seçeneğiniz vardır. İlk olarak, kullanıcı Akış adının yer aldığı `acr` kimliği belirtecinde talep. Bir kimlik belirteci gelen talepleri ayrıştırma hakkında daha fazla bilgi için bkz: [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). Kullanıcı akışını değerini kodlamak için kullanabileceğiniz diğer seçenek olan `state` isteği ve hangi kullanıcı akışı kullanılan belirlemek için kod çözme parametresi. Her iki yöntem geçerli değil.

Openıd Connect meta veri uç noktasından meta veri belgesi edindiğiniz sonra (Bu, bu uç noktada bulunur) 256 RSA ortak anahtarları kullanabilirsiniz kimlik belirteci imzasını doğrulamak için. Zaman içinde herhangi bir anda bu uç noktada listelenen birden çok anahtar olabilir, her tarafından tanımlanan bir `kid` talep. Ayrıca kimlik belirteci başlığını içeren bir `kid` talep, bu anahtarların hangisinin kimlik belirteci imzalamak için kullanılan gösterir. Daha fazla bilgi için [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md) (bölüm [belirteçleri doğrulama](active-directory-b2c-reference-tokens.md#token-validation), özellikle).
<!--TODO: Improve the information on this-->

Kimlik belirteci imzası doğruladıktan sonra doğrulamak için gereken çeşitli talep vardır. Örneğin:

* Doğrulanması `nonce` belirteç yeniden yürütme saldırıları önlemek talep. Oturum açma isteği belirtilen değeri olmalıdır.
* Doğrulanması `aud` kimlik belirteci uygulamanız için verilmiş emin olmak talep. Değeri, uygulamanızın uygulama kimliği olmalıdır.
* Doğrulanması `iat` ve `exp` kimlik belirteci sona ermediğinden emin olmak talep.

Gerçekleştirmeniz gereken birkaç daha fazla doğrulamaları vardır. Bu ayrıntılı olarak açıklanan [Openıd Connect çekirdek Spec](https://openid.net/specs/openid-connect-core-1_0.html).  Senaryonuza bağlı olarak ek istekleri doğrulamak isteyebilirsiniz. Bazı ortak doğrulamaları şunlardır:

* Kullanıcı/kuruluş uygulama için kaydolmuş emin olma.
* Kullanıcı uygun yetkilendirme/ayrıcalıklara sahip olduğundan emin olmak.
* Belirli bir kimlik doğrulama gücü, Azure multi-Factor Authentication gibi oluştuğunu sağlama.

Bir kimliği belirteçteki talepleri hakkında daha fazla bilgi için bkz. [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md).

Kimlik belirteci doğrulandıktan sonra kullanıcı ile oturum başlayabilirsiniz. Uygulamanızda kullanıcı hakkında bilgi almak için kimliği belirteçteki talepleri kullanabilirsiniz. Bu bilgiler kullanımları görüntüle ve kayıtları yetkilendirme içerir.

## <a name="get-a-token"></a>Bir belirteç Al
Kullanıcı akışları yalnızca yürütmek için web uygulamanız gerekiyorsa, birkaç sonraki bölümlere atlayabilirsiniz. Bu bölümlerde, yapmanız gereken uygulamaları çağrıları bir Web API'sine kimliği doğrulanmış ve aynı zamanda Azure AD B2C tarafından korunan web geçerlidir.

Aldığınız yetkilendirme kodunu almak (kullanarak `response_type=code+id_token`) için bir belirteç göndererek istenen kaynağa bir `POST` isteği `/token` uç noktası. Şu anda bir belirteci isteyebileceği yalnızca uygulamanın kendi arka uç web API'nizi kaynaktır. Kendiniz için bir belirteç isteyen Kural kapsamı olarak uygulamanızın istemci kimliği kullanmaktır:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
| p |Gereklidir |Yetkilendirme kodunu almak için kullanılan kullanıcı akışı. Bu istekte farklı kullanıcı akışı kullanamazsınız. Bu parametre sorgu dizesi için eklediğiniz Not `POST` gövdesi. |
| client_id |Gereklidir |Uygulama Kimliği [Azure portalında](https://portal.azure.com/) uygulamanıza atanan. |
| grant_type değeri |Gereklidir |Olmalıdır verme türünü `authorization_code` yetkilendirme kod akışı için. |
| scope |Önerilen |Kapsamları boşlukla ayrılmış listesi. Tek bir kapsam değeri, talep edilen hem izinleri Azure AD'ye gösterir. `openid` Kapsamı kullanıcının oturum açmasını ve id_token parametreleri biçiminde bir kullanıcı hakkında veri alma izni gösterir. Aynı uygulama kimliği istemci tarafından temsil edilen uygulamanın kendi arka uç Web API'si, belirteçlerini almak için kullanılabilir. `offline_access` Kapsamını belirtir uygulamanızı kaynaklarına uzun süreli erişim için bir yenileme belirteci gerekir. |
| kod |Gereklidir |Akışın ilk oluşturan içinde alınan yetkilendirme kodu. |
| redirect_uri |Gereklidir |`redirect_uri` Yetkilendirme kodu aldığınız uygulama parametresi. |
| client_secret |Gereklidir |İçinde oluşturulan uygulama gizli anahtarı [Azure portalında](https://portal.azure.com/). Bu uygulama gizli anahtarı önemli güvenlik bir yapıdır. Onu güvenli bir şekilde sunucunuzda depolamanız gerekir. Ayrıca, düzenli aralıklarla bu gizli döndürme. |

Başarılı bir token yanıt şu şekilde görünür:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametre | Açıklama |
| --- | --- |
| not_before |Saat, belirteç geçerli dönem zamanı kabul edilir. |
| token_type |Belirteç türü değeri. Azure AD destekleyen tek tür `Bearer`. |
| access_token |İstediğiniz imzalı JWT belirteci. |
| scope |Belirtecin geçerli olduğu kapsamları. Bu, daha sonra kullanmak için belirteç önbelleğe almak için kullanılabilir. |
| expires_in |Erişim belirteci (saniye olarak) geçerli olduğu süre uzunluğu. |
| refresh_token |OAuth 2.0 yenileme belirteci. Uygulama geçerli belirtecin süresi dolduktan sonra ek belirteçlerini almak için bu belirteci kullanabilirsiniz. Yenileme belirteçleri uzun süreli ve uzun süre için kaynaklarına erişimi korumak için kullanılabilir. Daha fazla ayrıntı için başvurmak [B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). Kapsam kullanılan gerekir Not `offline_access` yetkilendirme ve belirteç isteklerinin bir yenileme belirteci almak için. |

Hata yanıtları şöyle görünür:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametre | Açıklama |
| --- | --- |
| error |Hataları tepki vermek için kullanılabilir ve oluşan hataların türlerini sınıflandırmak için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

## <a name="use-the-token"></a>Belirteci kullanın
Bir erişim belirteci başarıyla edindiğiniz, belirteç isteklerini kullanabilirsiniz arka ucunuz için web API'leri dahil ederek `Authorization` üst bilgi:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Belirteci Yenile
Kimlik belirteçlerini kısa ömürlüdür. Kaynaklara erişmeye devam edebilmek için süresi dolduktan sonra bunları yenilemeniz gerekir. Başka bir göndererek yapabilirsiniz `POST` isteği `/token` uç noktası. Bu süre `refresh_token` yerine parametre `code` parametresi:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parametre | Gereklidir | Açıklama |
| --- | --- | --- |
| p |Gereklidir |Özgün yenileme belirteci almak için kullanılan kullanıcı akışı. Bu istekte farklı kullanıcı akışı kullanamazsınız. Bu parametre eklediğiniz sorgu dizesi için değil, POST gövdesini not edin. |
| client_id |Gereklidir |Uygulama Kimliği [Azure portalında](https://portal.azure.com/) uygulamanıza atanan. |
| grant_type değeri |Gereklidir |Bu oluşturan yetkilendirme kod akışı için bir yenileme belirteci olmalıdır verme türü. |
| scope |Önerilen |Kapsamları boşlukla ayrılmış listesi. Tek bir kapsam değeri, talep edilen hem izinleri Azure AD'ye gösterir. `openid` Kapsamı kullanıcının oturum açmasını ve kullanıcı kimliği belirteçleri şeklinde hakkında veri alma izni gösterir. Aynı uygulama kimliği istemci tarafından temsil edilen uygulamanın kendi arka uç Web API'si, belirteçlerini almak için kullanılabilir. `offline_access` Kapsamını belirtir uygulamanızı kaynaklarına uzun süreli erişim için bir yenileme belirteci gerekir. |
| redirect_uri |Önerilen |`redirect_uri` Yetkilendirme kodu aldığınız uygulama parametresi. |
| refresh_token |Gereklidir |Akışın ikinci oluşturan içinde alınan özgün bir yenileme belirteci. Kapsam kullanılan gerekir Not `offline_access` yetkilendirme ve belirteç isteklerinin bir yenileme belirteci almak için. |
| client_secret |Gereklidir |İçinde oluşturulan uygulama gizli anahtarı [Azure portalında](https://portal.azure.com/). Bu uygulama gizli anahtarı önemli güvenlik bir yapıdır. Onu güvenli bir şekilde sunucunuzda depolamanız gerekir. Ayrıca, düzenli aralıklarla bu gizli döndürme. |

Başarılı bir token yanıt şu şekilde görünür:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametre | Açıklama |
| --- | --- |
| not_before |Saat, belirteç geçerli dönem zamanı kabul edilir. |
| token_type |Belirteç türü değeri. Azure AD destekleyen tek tür `Bearer`. |
| access_token |İstediğiniz imzalı JWT belirteci. |
| scope |Daha sonra kullanmak için belirteçlerini önbelleğe alma işlemi için kullanılan belirteci için geçerli olan kapsamı. |
| expires_in |Erişim belirteci (saniye olarak) geçerli olduğu süre uzunluğu. |
| refresh_token |OAuth 2.0 yenileme belirteci. Uygulama geçerli belirtecin süresi dolduktan sonra ek belirteçlerini almak için bu belirteci kullanabilirsiniz.  Yenileme belirteçleri uzun süreli ve uzun süre için kaynaklarına erişimi korumak için kullanılabilir. Daha fazla ayrıntı için başvurmak [B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). |

Hata yanıtları şöyle görünür:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametre | Açıklama |
| --- | --- |
| error |Hataları tepki vermek için kullanılabilir ve oluşan hataların türlerini sınıflandırmak için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

## <a name="send-a-sign-out-request"></a>Oturum kapatma isteği gönder
Uygulama dışında kullanıcı oturum açmak istediğinizde, bunu uygulamanızın tanımlama ya da aksi takdirde son kullanıcı ile oturum temizlemek yeterli değil. Ayrıca, oturumu kapatmak için Azure AD'ye kullanıcı yönlendirmesi gerekir. Bunu yapmak başarısız olursa, kullanıcı kimlik bilgilerini yeniden girmeye gerek kalmadan uygulamanızı yeniden kimlik doğrulaması yapmasını mümkün olabilir. Geçerli tek bir oturum açma oturumu Azure AD ile olacağından budur.

Yalnızca kullanıcıya yönlendirebilirsiniz `end_session` Openıd Connect meta veri belgesinde listelenen uç nokta içinde daha önce "doğrulama kimlik belirteci" bölümünde açıklanan:

```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
| p |Gereklidir |Kullanıcının uygulamanızı imzalamak için kullanmak istediğiniz kullanıcı akışı. |
| post_logout_redirect_uri |Önerilen |Kullanıcı için sonra yeniden yönlendirilmesi gereken URL'yi başarılı oturum kapatma. Dahil edilmezse, Azure AD B2C kullanıcı genel bir ileti gösterilir. |

> [!NOTE]
> Kullanıcıya yönlendiren rağmen `end_session` uç nokta bazı Azure AD B2C ile kullanıcının çoklu oturum açma durumu Temizle, sosyal kimlik sağlayıcısı (IDP) oturumuna tarafından kullanıcının oturum yok. Kullanıcı bir sonraki oturum açma sırasında aynı IDP seçerse, bunlar, kullanıcıların kimlik bilgilerini girmeden kimliğinin yeniden doğrulanması. B2C uygulamanızdan imzalamak bir kullanıcı isterse, bunu mutlaka kendi Facebook hesabın oturumu kapatılsın istedikleri anlamına gelmez. Ancak, yerel hesaplar olması durumunda, kullanıcının oturumunu düzgün sonlandırılır.
> 
> 

## <a name="use-your-own-b2c-tenant"></a>Kendi B2C kiracınızı kullanın
Bu istekler kendiniz için denemek istiyorsanız, önce bu üç adımı gerçekleştirmeniz ve ardından daha önce kendi açıklanan örnek değerleri değiştirin:

1. [B2C kiracısı oluşturma](active-directory-b2c-get-started.md)ve isteklerindeki kiracınızın adını kullanın.
2. [Uygulama oluşturma](active-directory-b2c-app-registration.md) uygulama kimliğini edinme Bir web uygulaması/web API uygulamanıza dahil edin. İsteğe bağlı olarak, bir uygulama gizli anahtarı oluşturun.
3. [Kullanıcı akışlarınızı oluşturun](active-directory-b2c-reference-policies.md) akışı adları, kullanıcı elde edilir.

