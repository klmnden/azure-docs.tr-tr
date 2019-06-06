---
title: Openıd Connect - Azure Active Directory B2C ile oturum açma Web | Microsoft Docs
description: Azure Active Directory B2C'de Openıd Connect kimlik doğrulama protokolü kullanarak web uygulamaları oluşturun.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 85639e2648131f9475ad2ae77f31d43e64bf82e7
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66509215"
---
# <a name="web-sign-in-with-openid-connect-in-azure-active-directory-b2c"></a>Web oturumu açma Openıd Connect, Azure Active Directory B2C ile

Openıd Connect, kullanıcıların web uygulamalarına güvenli bir şekilde oturum açmak için kullanılan OAuth 2.0 üzerinde yerleşik bir kimlik doğrulama protokolü olan. Azure Active Directory B2C kullanarak Openıd Connect (Azure AD B2C) uygulaması, devredebilir kaydolma, oturum açma ve web uygulamalarınızı Azure Active Directory (Azure AD) içinde diğer kimlik yönetimi deneyimleri. Bu kılavuz dilden bağımsız bir şekilde bunu nasıl gösterir. Bu, HTTP iletileri gönderip bizim açık kaynak kitaplıkları hiçbirini kullanmadan nasıl açıklar.

[Openıd Connect](https://openid.net/specs/openid-connect-core-1_0.html) OAuth 2.0 genişletir *yetkilendirme* protokolü olarak kullanılmak üzere bir *kimlik doğrulaması* protokolü. Bu kimlik doğrulama protokolü çoklu oturum açma gerçekleştirmenizi sağlar. Kavramını sunar bir *kimlik belirteci*, kullanıcının kimliğini doğrulamak ve kullanıcının temel profil bilgilerini almak istemci sağlar.

Ayrıca, OAuth 2.0 genişlettiğinden, güvenli bir şekilde almak için uygulamalara sağlar *erişim belirteçlerini*. Erişim belirteçleri tarafından güvenliği sağlanan kaynaklara erişmek için kullanabileceğiniz bir [yetkilendirme sunucusu](active-directory-b2c-reference-protocols.md). Openıd Connect önerilir bir sunucuda barındırılan ve tarayıcı aracılığıyla erişilen bir web uygulaması oluştururken. Kimlik Yönetimi mobil uygulamanıza eklemek istediğiniz veya Masaüstü uygulamaları, Azure AD B2C kullanarak kullanmalısınız [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) Openıd Connect yerine. Belirteçleri hakkında daha fazla bilgi için bkz: [belirteçler Azure Active Directory B2C genel bakış](active-directory-b2c-reference-tokens.md)

Basit kimlik doğrulaması ve yetkilendirme birden yapmak için standart olan Openıd Connect protokolü, Azure AD B2C'yi genişletir. Tanıttığı [kullanıcı akışı parametre](active-directory-b2c-reference-policies.md), uygulamanız için kullanıcı eklemek için Openıd Connect kullanmanıza olanak sağlayan gibi deneyimleri kaydolma, oturum açma ve profil yönetimi.

## <a name="send-authentication-requests"></a>Kimlik doğrulama isteği gönder

Kullanıcının kimliğini doğrulamak ve kullanıcı akışı çalıştırmak, web uygulamanız gerektiğinde kullanıcıya yönlendirebilir `/authorize` uç noktası. Kullanıcı kullanıcı bir akışa bağlı olarak eylemi alır.

Bu istekte istemci, kullanıcıdan almak için gereken izinleri belirtir. `scope` parametresi ve çalıştırmak için kullanıcı akışı `p` parametresi. Üç örnek, her farklı kullanıcı akışı kullanarak (ile okunabilirlik için satır sonları), aşağıdaki bölümlerde verilmiştir. Nasıl çalıştığını her istek için bir genel görünüm almak için isteği bir tarayıcıya yapıştırarak bunu çalıştırmayı deneyin. Değiştirebilirsiniz `fabrikamb2c` kiracınız yoksa ve bir kullanıcı akışı oluşturduysanız adı.

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

| Parametre | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_id | Evet | Uygulama Kimliği [Azure portalında](https://portal.azure.com/) uygulamanıza atanmış. |
| response_type | Evet | İçin Openıd Connect kimlik belirteci içermelidir. Ayrıca web uygulamanızı bir web API'sini çağırmak için belirteçleri erişmesi gerekiyorsa, kullanabileceğiniz `code+id_token`. |
| redirect_uri | Hayır | `redirect_uri` Burada kimlik doğrulama yanıtlarının gönderilebilen veya uygulamanız tarafından alınan, uygulamanızın parametresi. Bu tam olarak biri eşleşmelidir `redirect_uri` URL olarak kodlanmış olması dışında Azure portalında kaydettiğiniz parametreleri. |
| scope | Evet | Kapsamları boşlukla ayrılmış listesi. `openid` Kapsamı kullanıcının oturum açmasını ve kullanıcı kimliği belirteçleri şeklinde hakkında veri alma izni gösterir. `offline_access` Kapsamı, web uygulamaları için isteğe bağlıdır. Uygulamanızı gerektiğini belirten bir *yenileme belirteci* genişletilmiş kaynaklara erişim için. |
| response_mode | Hayır | Ortaya çıkan bir yetkilendirme kodu uygulamanıza geri göndermek için kullanılan yöntem. Ya da olabilir `query`, `form_post`, veya `fragment`.  `form_post` Yanıt modu, en iyi güvenlik için önerilir. |
| durum | Hayır | Ayrıca belirteci yanıtta döndürülen isteğinde bulunan bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Rastgele oluşturulmuş bir benzersiz değeri, genellikle siteler arası istek sahteciliği saldırılarına önlemek için kullanılır. Durumu, uygulama kullanıcının durumu hakkında bilgi, bulunduğunuz sayfaya gibi kimlik doğrulama isteği oluşmadan önce kodlamak için de kullanılır. |
| nonce | Evet | Sonuçta elde edilen kimlik belirtecinde talep olarak dahil edilen (uygulama tarafından oluşturulan) istek içindeki bir değer. Uygulamanın belirteç yeniden yürütme saldırıları azaltmak için bu değer daha sonra doğrulayabilirsiniz. Genellikle istek kaynağı tanımlamak için kullanılan benzersiz bir rastgele dize değeridir. |
| p | Evet | Kullanıcı akışı çalıştırılır. Azure AD B2C kiracınızda oluşturulan kullanıcı akışı adıdır. Kullanıcı akışını adı ile başlamalıdır `b2c\_1\_`. |
| istemi | Hayır | Gerekli bir kullanıcı etkileşimi türü. Şu anda geçerli olan `login`, bu isteği kimlik bilgilerini girmesini zorlar. |

Bu noktada, kullanıcı iş akışını tamamlamak istenir. Kullanıcı sahip olabilir, kullanıcı adı ve parolasını girin, bir sosyal kimlik ya da oturum açın, dizin için kaydolun. Kullanıcı akışı nasıl tanımlandığını bağlı olarak adımlar herhangi bir sayı olabilir.

Kullanıcının kullanıcı akışı tamamlandıktan sonra belirtilen uygulamanız için bir yanıt döndürülür `redirect_uri` parametresi, belirtilen yöntemi kullanarak `response_mode` parametresi. Yanıt her bağımsız kullanıcı akışını önceki örneklerin aynıdır.

Başarılı yanıt kullanarak bir `response_mode=fragment` şöyle görünmelidir:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --------- | ----------- |
| id_token | Bu uygulama istendiğinde kimlik belirteci. Kimlik belirteci, kullanıcının kimliğini doğrulamak ve kullanıcıyı bir oturum başlatmak için kullanabilirsiniz. |
| code | Yetkilendirme kodu kullandıysanız, uygulamayı, istenen `response_type=code+id_token`. Uygulama, bir hedef kaynak için bir erişim belirteci istemek için yetkilendirme kodu kullanabilirsiniz. Yetkilendirme kodları, genellikle yaklaşık 10 dakika sonra süresi dolar. |
| durum | Varsa bir `state` aynı değeri yanıt olarak görünmesi gereken parametresi istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerleri aynıdır. |

Hata yanıtları da gönderilebilir `redirect_uri` parametresi uygulama bunları uygun şekilde işleyebilmeniz:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --------- | ----------- |
| error | Oluşan hataları türlerini sınıflandırmak için kullanılan kod. |
| error_description | Bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |
| durum | Varsa bir `state` aynı değeri yanıt olarak görünmesi gereken parametresi istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerleri aynıdır. |

## <a name="validate-the-id-token"></a>Kimlik belirteci doğrulama

Yalnızca bir kimlik belirteci alma kullanıcının kimliğini doğrulamak yeterli değil. Kimlik belirtecinin imzası doğrulayın ve uygulamanızın gereksinimlerini başına belirteçteki talepleri doğrulayın. Azure AD B2C'yi kullanan [JSON Web belirteçleri (Jwt'ler)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ve Belirteçleri imzalamak ve geçerli olduğunu doğrulamak için ortak anahtar şifrelemesi. Jwt'ler, tercih dilinizi bağlı olarak doğrulamak için kullanılabilen birçok açık kaynak kitaplıkları vardır. Kendi doğrulama mantığını uygulama yerine bu seçenekleri keşfetmeye öneririz. 

Azure AD B2C, çalışma zamanında Azure AD B2C hakkında bilgi almak uygulama izin veren bir Openıd Connect meta veri uç noktası, sahiptir. Bu bilgiler, uç noktaları, belirteç içeriği ve belirteç imzalama anahtarı içerir. B2C kiracınızda bulunan her bir kullanıcı akışı için bir JSON meta verileri belgesi yoktur. Örneğin, meta veri belgesi için `b2c_1_sign_in` kullanıcı akışını `fabrikamb2c.onmicrosoft.com` şu konumdadır:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Bu yapılandırma belgenin özelliklerinden biri `jwks_uri`, değeri aynı kullanıcı akışı olacaktır:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Hangi kullanıcı akışı Kimliğiniz oturum açarken kullanılan belirlemek için belirteci (ve meta verilerin alınacağı), iki seçeneğiniz vardır. İlk olarak, kullanıcı Akış adının yer aldığı `acr` kimliği belirtecinde talep. Kullanıcı akışını değerini kodlamak için kullanabileceğiniz diğer seçenek olan `state` isteği ve hangi kullanıcı akışı kullanılan belirlemek için kod çözme parametresi. Her iki yöntem geçerli değil.

Openıd Connect meta veri uç noktasından meta veri belgesi edindiğiniz sonra kimlik belirteci imzasını doğrulamak için 256 RSA ortak anahtarları kullanabilirsiniz. Bu uç noktada listelenen birden çok anahtar olabilir, her tarafından tanımlanan bir `kid` talep. Ayrıca kimlik belirteci başlığını içeren bir `kid` talep, bu anahtarların hangisinin kimlik belirteci imzalamak için kullanılan gösterir.

Kimlik belirteci imzası doğruladıktan sonra doğrulamak için gereken çeşitli talep vardır. Örneğin:

- Doğrulama `nonce` belirteç yeniden yürütme saldırıları önlemek talep. Oturum açma isteği belirtilen değeri olmalıdır.
- Doğrulama `aud` kimlik belirteci uygulamanız için verilmiş emin olmak talep. Değeri, uygulamanızın uygulama kimliği olmalıdır.
- Doğrulama `iat` ve `exp` iddia kimlik belirteci dolmadığından emin olun.

Gerçekleştirmeniz gereken birkaç daha fazla doğrulamaları vardır. Doğrulamaları ayrıntılı olarak açıklanan [Openıd Connect çekirdek Spec](https://openid.net/specs/openid-connect-core-1_0.html). Senaryonuza bağlı olarak ek istekleri doğrulamak isteyebilirsiniz. Bazı ortak doğrulamaları şunlardır:

- Kullanıcı/kuruluş uygulama için kaydolmuş emin olma.
- Kullanıcı uygun yetkilendirme/ayrıcalıklara sahip olduğundan emin olmak.
- Belirli bir kimlik doğrulama gücü, Azure multi-Factor Authentication gibi oluştuğunu sağlama.

Kimlik belirteci doğruladıktan sonra kullanıcı ile oturum başlayabilirsiniz. Uygulamanızda kullanıcı hakkında bilgi almak için kimliği belirteçteki talepleri kullanabilirsiniz. Bu bilgiler kullanımları görüntüle ve kayıtları yetkilendirme içerir.

## <a name="get-a-token"></a>Bir belirteç Al

Kullanıcı akışları yalnızca çalıştırmak üzere web uygulamanız gerekiyorsa, birkaç sonraki bölümlere atlayabilirsiniz. Bu bölümlerde, yapmanız gereken uygulamaları çağrıları bir Web API'sine kimliği doğrulanmış ve aynı zamanda Azure AD B2C tarafından korunan web geçerlidir.

Aldığınız yetkilendirme kodunu almak (kullanarak `response_type=code+id_token`) için bir belirteç göndererek istenen kaynağa bir `POST` isteği `/token` uç noktası. Şu anda bir belirteci isteyebileceği yalnızca uygulamanın kendi arka uç web API'nizi kaynaktır. Kendiniz için bir belirteç isteyen Kural kapsamı olarak uygulamanızın istemci kimliği kullanmaktır:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parametre | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| p | Evet | Yetkilendirme kodunu almak için kullanılan kullanıcı akışı. Bu istekte farklı kullanıcı akışı kullanamazsınız. Bu parametre, sorgu dizesi için değil, POST gövdesini ekleyin. |
| client_id | Evet | Uygulama Kimliği [Azure portalında](https://portal.azure.com/) uygulamanıza atanmış. |
| grant_type | Evet | Olmalıdır verme türünü `authorization_code` yetkilendirme kod akışı için. |
| scope | Hayır | Kapsamları boşlukla ayrılmış listesi. `openid` Kapsamı kullanıcının oturum açmasını ve id_token parametreleri biçiminde bir kullanıcı hakkında veri alma izni gösterir. Aynı uygulama kimliği istemci tarafından temsil edilen uygulamanın kendi arka uç Web API'si, belirteçlerini almak için kullanılabilir. `offline_access` Kapsamını belirtir, uygulamanızın kaynakları genişletilmiş erişim için bir yenileme belirteci gerekiyor. |
| code | Evet | Kullanıcı akışı, başlangıçta satın aldığınız yetkilendirme kodu. |
| redirect_uri | Evet | `redirect_uri` Yetkilendirme kodu aldığınız uygulama parametresi. |
| client_secret | Evet | Oluşturulduğu uygulama gizli anahtarı [Azure portalında](https://portal.azure.com/). Bu uygulama gizli anahtarı önemli güvenlik bir yapıdır. Onu güvenli bir şekilde sunucunuzda depolamanız gerekir. Bu gizli düzenli olarak değiştirin. |

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
| --------- | ----------- |
| not_before | Saat, belirteç geçerli dönem zamanı kabul edilir. |
| token_type | Belirteç türü değeri. `Bearer` desteklenen tek türüdür. |
| access_token | İstediğiniz imzalı JWT belirteci. |
| scope | Belirtecin geçerli olduğu kapsamları. |
| expires_in | Erişim belirteci (saniye olarak) geçerli olduğu süre uzunluğu. |
| refresh_token | OAuth 2.0 yenileme belirteci. Uygulamanın geçerli belirtecin süresi dolduktan sonra ek belirteçlerini almak için bu belirteci kullanabilirsiniz. Yenileme belirteçleri, uzun süre için kaynaklarına erişimi korumak için kullanılabilir. Kapsam `offline_access` yetkilendirme ve belirteç isteklerinin bir yenileme belirteci almak için kullanılmış gerekir. |

Hata yanıtları şöyle görünür:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametre | Açıklama |
| --------- | ----------- |
| error | Oluşan hataları türlerini sınıflandırmak için kullanılan kod. |
| error_description | Bir kimlik doğrulama hatası kök nedenini belirlemeye yardımcı olabilecek bir ileti. |

## <a name="use-the-token"></a>Belirteci kullanın

Bir erişim belirteci başarıyla edindiğiniz, belirteç isteklerini kullanabilirsiniz arka ucunuz için web API'leri dahil ederek `Authorization` üst bilgi:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Belirteci Yenile

Kimlik belirteçlerini kısa bir süre içinde süresi dolar. Kaynaklara erişmeye devam edebilmek için süresi dolan belirteçleri yenileyin. Bir belirteç başka göndererek yenileyebilirsiniz `POST` isteği `/token` uç noktası. Bu süre `refresh_token` yerine parametre `code` parametresi:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parametre | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| p | Evet | Özgün yenileme belirteci almak için kullanılan kullanıcı akışı. Bu istekte farklı kullanıcı akışı kullanamazsınız. Bu parametre, sorgu dizesi için değil, POST gövdesini ekleyin. |
| client_id | Evet | Uygulama Kimliği [Azure portalında](https://portal.azure.com/) uygulamanıza atanmış. |
| grant_type | Evet | Yetkilendirme kodu akışı bu kısmı için bir yenileme belirteci olmalıdır verme türü. |
| scope | Hayır | Kapsamları boşlukla ayrılmış listesi. `openid` Kapsamı kullanıcının oturum açmasını ve kullanıcı kimliği belirteçleri şeklinde hakkında veri alma izni gösterir. Aynı uygulama kimliği istemci tarafından temsil edilen, uygulamanın kendi arka uç web API'si, belirteçleri göndermek için kullanılabilir. `offline_access` Kapsamını belirtir, uygulamanızın kaynakları genişletilmiş erişim için bir yenileme belirteci gerekiyor. |
| redirect_uri | Hayır | `redirect_uri` Yetkilendirme kodu aldığınız uygulama parametresi. |
| refresh_token | Evet | Akış ikinci kısmında alındı özgün bir yenileme belirteci. `offline_access` Kapsamı kullanılmalıdır yetkilendirme ve belirteç isteklerinin bir yenileme belirteci almak için. |
| client_secret | Evet | Oluşturulduğu uygulama gizli anahtarı [Azure portalında](https://portal.azure.com/). Bu uygulama gizli anahtarı önemli güvenlik bir yapıdır. Onu güvenli bir şekilde sunucunuzda depolamanız gerekir. Bu gizli düzenli olarak değiştirin. |

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
| --------- | ----------- |
| not_before | Saat, belirteç geçerli dönem zamanı kabul edilir. |
| token_type | Belirteç türü değeri. `Bearer` desteklenen tek türüdür. |
| access_token | İstenen imzalı JWT belirteci. |
| scope | Belirtecin geçerli olduğu kapsamı. |
| expires_in | Erişim belirteci (saniye olarak) geçerli olduğu süre uzunluğu. |
| refresh_token | OAuth 2.0 yenileme belirteci. Uygulamanın geçerli belirtecin süresi dolduktan sonra ek belirteçlerini almak için bu belirteci kullanabilirsiniz. Yenileme belirteçleri, uzun süre için kaynaklarına erişimi korumak için kullanılabilir. |

Hata yanıtları şöyle görünür:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametre | Açıklama |
| --------- | ----------- |
| error | Oluşan hataları türlerini sınıflandırmak için kullanılan kod. |
| error_description | Bir kimlik doğrulama hatası kök nedenini belirlemeye yardımcı olabilecek bir ileti. |

## <a name="send-a-sign-out-request"></a>Oturum kapatma isteği gönder

Uygulama kullanıcının oturum açmak istediğinizde, uygulamanın tanımlama bilgilerini temizleyin veya aksi takdirde kullanıcı oturumu sonlandırmak için yeterli değil. Kullanıcı oturumu kapatmak için Azure AD B2C yönlendirin. Bunu yapmak başarısız olursa, kullanıcı kimlik bilgilerini yeniden girmeye gerek kalmadan uygulamanızı yeniden kimlik doğrulaması yapmasını mümkün olabilir.

Yalnızca kullanıcıya yönlendirebilirsiniz `end_session` daha önce açıklanan Openıd Connect meta veri belgesinde listelenen uç noktası:

```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parametre | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| p | Evet | Kullanıcının uygulamanızı imzalamak için kullanmak istediğiniz kullanıcı akışı. |
| post_logout_redirect_uri | Hayır | Kullanıcı için başarılı oturum kapatma sonrasında yeniden yönlendirilmesi gereken URL. Dahil edilen değilse, Azure AD B2C kullanıcı genel bir ileti gösterilir. |

Kullanıcıya yönlendiren `end_session` uç nokta Azure AD B2C ile kullanıcının çoklu oturum açma durumunu bazıları temizler, ancak kullanıcının sosyal kimlik sağlayıcısı (IDP) oturumuna dışında oturum değil. Kullanıcı bir sonraki oturum açma sırasında aynı IDP seçerse, bunlar, kimlik bilgilerini girmesini olmadan yeniden kimlik doğrulaması. Bir kullanıcı uygulamadaki oturumunu açmak isterse, kullanıcıların Facebook hesabın oturumu kapatılsın istedikleri mutlaka gelmez. Ancak, yerel hesapları kullanılıyorsa, kullanıcının oturum düzgün bir şekilde sona erer.

