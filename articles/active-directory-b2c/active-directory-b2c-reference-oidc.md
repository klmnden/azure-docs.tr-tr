---
title: Web Openıd Connect - Azure AD B2C ile oturum açma | Microsoft Docs
description: Azure Active Directory uygulaması Openıd Connect kimlik doğrulama protokolü kullanarak Web uygulamaları oluşturma
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
ms.openlocfilehash: e787ea36ab5099705f151504385dd5dc97029e37
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: Web oturum açma Openıd Connect ile
Openıd Connect kullanıcılar web uygulamaları için güvenli bir şekilde oturum açmak için kullanılan OAuth 2.0 üstünde yerleşik bir kimlik doğrulama, protokolüdür. Azure Active Directory B2C kullanarak Openıd Connect (Azure AD B2C) uygulaması, devredebilir kaydolma, oturum açma ve Azure Active Directory'ye (Azure AD), web uygulamalarında diğer kimlik yönetimi deneyimleri. Bu kılavuz bir dilden bağımsız biçimde bunun nasıl yapılacağı gösterir. Bu bizim açık kaynak kitaplıkları kullanmadan HTTP iletileri almasına ve göndermesine açıklar.

[Openıd Connect](http://openid.net/specs/openid-connect-core-1_0.html) OAuth 2.0 genişletir *yetkilendirme* protokolü olarak kullanmak için bir *kimlik doğrulaması* protokolü. Bu, OAuth kullanarak, çoklu oturum açma gerçekleştirmenizi sağlar. Kavramını sunmaktadır bir *kimliği belirteci*, kullanıcının kimliğini doğrulamak ve kullanıcının temel profil bilgilerini almak istemci izin veren bir güvenlik belirteci değil.

OAuth 2.0 genişlettiğinden, güvenli bir şekilde edinmeye uygulamalar ayrıca sağlar *erişim belirteçleri*. Access_tokens tarafından güvenliği sağlanan kaynaklara erişmek için kullanabileceğiniz bir [yetkilendirme sunucusu](active-directory-b2c-reference-protocols.md#the-basics). Bir sunucuda barındırılan ve tarayıcı aracılığıyla erişilen bir web uygulaması oluşturuyorsanız Openıd Connect öneririz. Kimlik Yönetimi, mobil veya Masaüstü uygulamalarınızı Azure AD B2C kullanarak eklemek istiyorsanız, kullanması gereken [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) Openıd Connect yerine.

Azure AD B2C birden çok basit kimlik doğrulama ve yetkilendirme yapmak için standart Openıd Connect Protokolü genişletir. Tanıttığı [İlkesi parametresi](active-directory-b2c-reference-policies.md), Openıd Connect gibi kullanıcı deneyimlerini--eklemek için kullanmanıza olanak sağlayan kaydolma, oturum açma ve profil yönetimi--uygulamanıza. Burada, biz, Openıd Connect ve ilkeleri her bu deneyimleri web uygulamalarınızda uygulamak için nasıl kullanılacağı gösterilmektedir. Ayrıca web API'leri erişmek için erişim belirteçleri almak nasıl göstereceğiz.

Sonraki bölümde örnek HTTP istekleri bizim örnek B2C dizini, fabrikamb2c.onmicrosoft.com yanı sıra, örnek uygulamamız kullanmak https://aadb2cplayground.azurewebsites.netve ilkeleri. İstekleri bu değerleri kullanarak kendiniz denemek boş ya da bunları kendi ile değiştirebilirsiniz.
Bilgi edinmek için nasıl [kendi B2C Kiracı, uygulama ve ilkeleri alma](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Kimlik doğrulama isteklerini gönderme
Web uygulamanızı kullanıcının kimliğini doğrulamak ve ilke yürütmesini gerektiğinde kullanıcıya yönlendirebilirsiniz `/authorize` uç noktası. Bu etkileşimli akış burada kullanıcı ilkesine bağlı olarak eylem gerçekleştirir bölümüdür.

Bu istekte istemcisi kullanıcıdan almak için gereken izinleri belirtir. `scope` parametre ve içinde yürütmek için ilke `p` parametresi. Üç örnekler, her farklı ilke kullanılarak (satır sonuyla okunabilirlik için), aşağıdaki bölümlerde verilmiştir. Nasıl çalıştığı her istek için bir fikir almak için onu çalıştıran ve bir tarayıcıya istek yapıştırma deneyin.

#### <a name="use-a-sign-in-policy"></a>Bir oturum açma ilkesini kullanın
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Kayıt İlkesi'ni kullanın
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Bir profili Düzenle ilkesini kullanın
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
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
| client_id |Gerekli |Uygulama Kimliği [Azure portal](https://portal.azure.com/) uygulamanıza atanmış. |
| response_type |Gerekli |Yanıt türü kimliği belirteci Openıd Connect eklemeniz gerekir. Ayrıca web uygulamanızı bir web API'sini çağırmak için belirteçleri ihtiyaç duyuyorsa kullanabileceğiniz `code+id_token`biz burada yaptığınız gibi. |
| redirect_uri |Önerilen |`redirect_uri` Burada kimlik doğrulama yanıtları gönderilebilen veya uygulamanız tarafından alınan, uygulamanızın parametresi. Bu tam olarak eşleşmelidir `redirect_uri` URL kodlanmış olmalıdır ancak bu Portalı'nda kayıtlı parametreleri. |
| Kapsam |Gerekli |Kapsamları boşlukla ayrılmış listesi. Tek bir kapsam değeri Azure AD ile istenen iki izinleri gösterir. `openid` Kapsam kullanıcı oturumu ve kullanıcı kimliği belirteçleri (daha fazla bu makalenin sonraki bölümlerinde gelmesini) biçiminde hakkında veri Al izni gösterir. `offline_access` Kapsamı web uygulamaları için isteğe bağlı değil. Uygulamanıza gerekecek gösteren bir *yenileme belirteci* uzun süreli kaynaklarına erişim için. |
| response_mode |Önerilen |Sonuçta elde edilen yetkilendirme kodu uygulamanıza geri göndermek için kullanılması gereken yöntem. Ya da olabilir `query`, `form_post`, veya `fragment`.  `form_post` Yanıt modu için en iyi güvenlik önerilir. |
| durum |Önerilen |Ayrıca belirteci yanıtta döndürülen istek dahil bir değer. İstediğiniz herhangi bir içerik dizesi olabilir. Rastgele oluşturulan benzersiz bir değer genellikle siteler arası istek sahteciliği saldırılarına önlemek için kullanılır. Durumu, sayfanın üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce uygulama kullanıcının durumu hakkındaki bilgileri kodlamak için de kullanılır. |
| nonce |Gerekli |Bir talep elde edilen kimliği belirtecindeki dahil (uygulama tarafından üretilen) istek içindeki bir değer. Uygulama sonra belirteç yeniden yürütme saldırıları azaltmak için bu değeri doğrulayabilirsiniz. Genellikle istek kaynağını tanımlamak için kullanılan rastgele, benzersiz bir dize değeridir. |
| p |Gerekli |Yürütülecek ilkesi. B2C kiracınızda oluşturulan bir ilkeyi adıdır. İlke adı değeri ile başlaması gereken `b2c\_1\_`. İlkeleri hakkında daha fazla bilgi edinin ve [Genişletilebilir ilke çerçevesini](active-directory-b2c-reference-policies.md). |
| istemi |İsteğe bağlı |Gerekli bir kullanıcı etkileşimi türü. Şu anda geçerli tek değer `login`, kullanıcının bu isteği kimlik bilgilerini girmesini zorlar. Çoklu oturum açma etkili olmaz. |

Bu noktada, ilkenin iş akışını tamamlamak için kullanıcının istedi. Bu dizini ya da herhangi bir ilkenin nasıl tanımlanır bağlı olarak adım sayısı için kaydolan bir sosyal kimlik bilgilerinizle oturum, kullanıcı adını ve parolasını girerek kullanıcı gerektirebilir.

Azure AD kullanıcı ilkeyi tamamladıktan sonra uygulamanızı belirtilen bir yanıt döndürür `redirect_uri` belirtilen yöntemi kullanarak parametresi `response_mode` parametresi. Yanıt, her yürütülen ilke bağımsız önceki örneklerinin aynıdır.

Başarılı yanıt kullanarak `response_mode=fragment` gibi görünür:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --- | --- |
| id_token |İstenen uygulama kimliği belirteci. Kullanıcının kimliğini doğrulamak ve kullanıcı oturumu başlatmak için kimliği belirteci kullanabilirsiniz. Kimlik belirteçlerini ve içerikleri daha ayrıntılı bilgi yer alan [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). |
| kod |Kullandıysanız, uygulama, istenen yetkilendirme kodu `response_type=code+id_token`. Uygulama, bir hedef kaynak için bir erişim belirteci istemek için yetkilendirme kodu kullanabilirsiniz. Yetkilendirme kodları çok kısa ömürlüdür. Genellikle, yaklaşık 10 dakika sonra süresi. |
| durum |Varsa bir `state` parametresi, aynı değeri yanıt olarak görünmesi gereken istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerler aynı. |

Hata yanıtları da gönderilebilir için `redirect_uri` parametre böylece uygulama bunları uygun şekilde işleyebilir:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --- | --- |
| error |Hata oluşan ve hataları tepki vermek için kullanılabilir türleri sınıflandırmak için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |
| durum |Bu bölümdeki ilk tablodaki tam açıklamasına bakın. Varsa bir `state` parametresi, aynı değeri yanıt olarak görünmesi gereken istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerler aynı. |

## <a name="validate-the-id-token"></a>Kimliği belirtecini doğrula
Yalnızca bir kimliği belirteci alma kullanıcının kimliğini doğrulamak için yeterli değil. Kimliği belirtecin imzayı doğrulamak ve uygulamanızın gereksinimleri başına belirtecinizdeki talepleri doğrulamanız gerekir. Azure AD B2C kullanan [JSON Web belirteçleri (Jwt'ler)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ve Belirteçleri imzalamak ve bunların geçerli olduğunu doğrulamak için ortak anahtar şifrelemesi.

Jwt'ler, tercih dilinizi bağlı olarak doğrulamak için kullanılabilen birçok açık kaynak kitaplıkları vardır. Kendi doğrulama mantığı uygulamak yerine bu seçenekleri keşfetme öneririz. Burada yer alan bilgiler düzgün bir şekilde bu kitaplıkları kullanma nasıl anlamak yararlı olacaktır.

Azure AD B2C çalışma zamanında Azure AD B2C hakkında bilgi almak uygulama izin veren bir Openıd Connect meta veri uç noktası, vardır. Bu bilgiler, uç noktaları, belirteç içerikleri ve belirteç imzalama anahtarları içerir. B2C kiracınızda her ilke için bir JSON meta veri belgesi yoktur. Örneğin, meta veri belgesi için `b2c_1_sign_in` ilkesinde `fabrikamb2c.onmicrosoft.com` bulunur:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Bu yapılandırma belgesi özelliklerini biri `jwks_uri`, aynı ilke olacaktır için bir değer:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Hangi ilke kimliği oturum açma kullanılan belirlemek için belirteci (ve meta veri Getir nereden), iki seçeneğiniz vardır. İlk olarak, ilke adını dahil `acr` kimliği belirteçte talep. Bir kimliği belirteci talepleri ayrıştırma hakkında daha fazla bilgi için bkz: [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). Değerini ilkesinde kodlamak için diğer seçeneğiniz olduğunu `state` isteği gönderin ve ardından ilkeyi kullanılan belirlemek için kod çözme parametresi. Her iki yöntem geçerli değil.

Openıd Connect meta veri uç noktasından meta veri belgesi edindiğiniz sonra (hangi Bu uç noktada bulunur) RSA 256 ortak anahtarlar kullanabilirsiniz kimliği belirtecinin imzası doğrulanamadı. Verilen herhangi bir noktada Bu uç noktada zamanında listelenen birden çok anahtar olabilir, her tarafından tanımlanan bir `kid` talep. Kimliği belirteci üstbilgisinin de içeren bir `kid` talep, bu anahtarlar hangisinin kimliği belirteç imzalamak için kullanılan gösterir. Daha fazla bilgi için bkz: [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md) (bölüm [belirteçleri doğrulama](active-directory-b2c-reference-tokens.md#token-validation), özellikle).
<!--TODO: Improve the information on this-->

Kimliği belirtecinin imzası doğruladıktan sonra doğrulamak için gereken birkaç talep vardır. Örneğin:

* Doğrulama `nonce` belirteç yeniden yürütme saldırılarını önlemek talep. Oturum açma istekte belirtilen değeri olmalıdır.
* Doğrulama `aud` kimliği belirteci uygulamanız için verilmiş emin olmak talep. Değeri, uygulamanızın uygulama kimliği olmalıdır.
* Doğrulama `iat` ve `exp` kimliği belirtecin süresi geçmemiş emin olmak talep.

Gerçekleştirmeniz gereken birkaç daha fazla doğrulama vardır. Bunlar içinde ayrıntılı olarak açıklanmıştır [Openıd Connect çekirdek Spec](http://openid.net/specs/openid-connect-core-1_0.html).  Senaryonuza bağlı olarak ek talep doğrulamak isteyebilirsiniz. Bazı ortak doğrulamaları şunları içerir:

* Kullanıcı/kuruluş uygulaması için kaydolmuş emin olma.
* Kullanıcının uygun yetkilendirme/ayrıcalıklara sahip olma.
* Belirli bir kimlik doğrulama gücünü, Azure multi-Factor Authentication gibi oluştuğunu sağlama.

Bir kimliği belirtecinizdeki talepleri hakkında daha fazla bilgi için bkz: [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md).

Kimliği belirteci doğrulandıktan sonra kullanıcıyla oturum başlayabilirsiniz. Uygulamanızda kullanıcı hakkında bilgi edinmek için talep kimliği belirteçte kullanabilirsiniz. Bu bilgiler kullanımları görüntüleme, kayıtları ve yetkilendirme içerir.

## <a name="get-a-token"></a>Belirteç alın
Yalnızca İlkeleri yürütmek için web uygulamanız gerekiyorsa, sonraki birkaç bölümlerde atlayabilirsiniz. Bu bölüm, yapmanız gereken uygulamaları bir web API çağrıları kimlik doğrulaması ve Azure AD B2C tarafından korunmuş web geçerlidir.

Aldığınız yetki kodunu kullanın (kullanarak `response_type=code+id_token`) göndererek istenen kaynak için bir belirteç için bir `POST` isteği `/token` uç noktası. Şu an için bir belirteç isteği yalnızca kaynak uygulamanın kendi arka uç web API değil. Kendinize bir belirteç isteyen kuralı, uygulamanın istemci Kimliğini kapsamı olarak kullanmaktır:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
| p |Gerekli |Yetkilendirme kodu almak için kullanılan bir ilke. Bu isteği başka bir ilke kullanamazsınız. Bu parametre sorgu dizesi değil eklediğiniz Not `POST` gövdesi. |
| client_id |Gerekli |Uygulama Kimliği [Azure portal](https://portal.azure.com/) uygulamanıza atanmış. |
| grant_type |Gerekli |Olmalıdır grant türünü `authorization_code` yetkilendirme kodu akışı. |
| Kapsam |Önerilen |Kapsamları boşlukla ayrılmış listesi. Tek bir kapsam değeri Azure AD ile istenen iki izinleri gösterir. `openid` Kapsamını, kullanıcıyla oturum açmak ve id_token parametreleri biçiminde kullanıcı hakkında veri Al izni belirtir. Aynı uygulama kimliği istemci tarafından temsil edilen, uygulamanın kendi arka uç web API'si, belirteçleri almak için kullanılabilir. `offline_access` Kapsamını belirtir, uygulamanızın uzun süreli kaynaklarına erişim için bir yenileme belirteci gerekir. |
| kod |Gerekli |Akış ilk leg içinde aldığınız yetkilendirme kodu. |
| redirect_uri |Gerekli |`redirect_uri` Yetkilendirme kodu aldığınız uygulama parametresi. |
| client_secret |Gerekli |İçinde oluşturulan uygulama gizli anahtarı [Azure portal](https://portal.azure.com/). Bu uygulama gizli anahtarı bir önemli güvenlik yapıdır. Onu güvenli bir şekilde sunucunuzda depolamanız gerekir. Ayrıca bu gizli düzenli aralıklarla döndürme. |

Başarılı bir token yanıt şuna benzer:

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
| not_before |Hangi belirtecin geçerli dönem zamanında değerlendirilir süre. |
| token_type |Belirteç türü değeri. Azure AD destekler yalnızca türü `Bearer`. |
| access_token |İstediğiniz imzalı JWT belirteci. |
| Kapsam |Belirtecin geçerli olduğu kapsamlar. Bu, daha sonra kullanmak için belirteç önbelleğe almak için kullanılabilir. |
| expires_in |Erişim belirtecinin (saniye olarak) geçerli olduğu süreyi. |
| refresh_token |Bir OAuth 2.0 yenileme belirteci. Uygulama geçerli belirtecin süresi dolduktan sonra ek belirteçleri almak için bu belirteci kullanabilirsiniz. Yenileme belirteçleri uzun süreli ve uzun süre için kaynaklara erişimi korumak için kullanılabilir. Daha fazla ayrıntı için başvurmak [B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). Kapsam kullanılan gerekir Not `offline_access` yetkilendirme ve bir yenileme belirteci almak için belirteç isteklerini. |

Hata yanıtları gibi görünür:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametre | Açıklama |
| --- | --- |
| error |Hata oluşan ve hataları tepki vermek için kullanılabilir türleri sınıflandırmak için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

## <a name="use-the-token"></a>Belirteci kullanın
Bir erişim belirteci başarıyla edindiğiniz, belirteç isteklerini kullanabilirsiniz dahil ederek API'leri, arka uç web `Authorization` üstbilgisi:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Belirteç Yenile
Kimlik belirteçlerini ömürlüdür. Kaynaklara erişmeye devam edebilmek için parolalarının süresi dolduktan sonra bunları yenilemeniz gerekir. Başka bir göndererek yapabilirsiniz `POST` isteği `/token` uç noktası. Bu süre sağlamak `refresh_token` yerine parametre `code` parametre:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parametre | Gerekli | Açıklama |
| --- | --- | --- |
| p |Gerekli |Özgün yenileme belirtecini almak için kullanılan bir ilke. Bu isteği başka bir ilke kullanamazsınız. Bu parametre eklediğiniz sorgu dizesi için değil, POST gövde unutmayın. |
| client_id |Gerekli |Uygulama Kimliği [Azure portal](https://portal.azure.com/) uygulamanıza atanmış. |
| grant_type |Gerekli |Bu leg yetkilendirme kodu akışı için bir yenileme belirteci olmalıdır grant türü. |
| Kapsam |Önerilen |Kapsamları boşlukla ayrılmış listesi. Tek bir kapsam değeri Azure AD ile istenen iki izinleri gösterir. `openid` Kapsam kullanıcı oturumu ve kullanıcı kimliği belirteçleri biçiminde hakkında veri Al izni gösterir. Aynı uygulama kimliği istemci tarafından temsil edilen, uygulamanın kendi arka uç web API'si, belirteçleri almak için kullanılabilir. `offline_access` Kapsamını belirtir, uygulamanızın uzun süreli kaynaklarına erişim için bir yenileme belirteci gerekir. |
| redirect_uri |Önerilen |`redirect_uri` Yetkilendirme kodu aldığınız uygulama parametresi. |
| refresh_token |Gerekli |Akış ikinci leg içinde aldığınız özgün yenileme belirteci. Kapsam kullanılan gerekir Not `offline_access` yetkilendirme ve bir yenileme belirteci almak için belirteç isteklerini. |
| client_secret |Gerekli |İçinde oluşturulan uygulama gizli anahtarı [Azure portal](https://portal.azure.com/). Bu uygulama gizli anahtarı bir önemli güvenlik yapıdır. Onu güvenli bir şekilde sunucunuzda depolamanız gerekir. Ayrıca bu gizli düzenli aralıklarla döndürme. |

Başarılı bir token yanıt şuna benzer:

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
| not_before |Hangi belirtecin geçerli dönem zamanında değerlendirilir süre. |
| token_type |Belirteç türü değeri. Azure AD destekler yalnızca türü `Bearer`. |
| access_token |İstediğiniz imzalı JWT belirteci. |
| Kapsam |Daha sonra kullanmak için belirteç önbelleğe alma işlemi için kullanılan belirteci için geçerli olan kapsamı. |
| expires_in |Erişim belirtecinin (saniye olarak) geçerli olduğu süreyi. |
| refresh_token |Bir OAuth 2.0 yenileme belirteci. Uygulama geçerli belirtecin süresi dolduktan sonra ek belirteçleri almak için bu belirteci kullanabilirsiniz.  Yenileme belirteçleri uzun süreli ve uzun süre için kaynaklara erişimi korumak için kullanılabilir. Daha fazla ayrıntı için bkz [B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). |

Hata yanıtları gibi görünür:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametre | Açıklama |
| --- | --- |
| error |Hata oluşan ve hataları tepki vermek için kullanılabilir türleri sınıflandırmak için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

## <a name="send-a-sign-out-request"></a>Oturum kapatma isteği gönder
Uygulama dışında kullanıcı oturum istediğinizde, uygulamanızın tanımlama bilgilerini veya aksi takdirde son kullanıcı oturumuyla temizlemek yeterli değil. Ayrıca, oturumu kapatmak için Azure ad kullanıcı yönlendirmesi gerekir. Bunu yapmak başarısız olursa, kullanıcı kimlik bilgilerini yeniden girmeye gerek kalmadan, uygulamanızın yeniden kimlik doğrulamaya olabilir. Bu durum, geçerli tek bir oturum açma oturumu Azure AD ile gerekir çünkü.

Kullanıcıya yalnızca yönlendirebilirsiniz `end_session` Openıd Connect meta veri belgesinde listelenen endpoint önceki içinde "doğrulama kimliği belirteci" bölümünde açıklanan:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
| p |Gerekli |Uygulamanızı dışında kullanıcı oturum için kullanmak istediğiniz ilke. |
| post_logout_redirect_uri |Önerilen |Kullanıcı için sonra yeniden yönlendirilmesi gereken URL başarılı oturum kapatma. Dahil edilmezse, Azure AD B2C kullanıcı genel bir ileti gösterir. |

> [!NOTE]
> Kullanıcıya yönlendirerek rağmen `end_session` uç nokta bazı Azure AD B2C ile kullanıcının tek oturum açma durumu Temizle, sosyal kimlik sağlayıcıyı (IDP) oturumuna dışında kullanıcı oturum yok. Kullanıcı bir sonraki oturum açma sırasında aynı IDP seçerse, bunlar, kullanıcıların kimlik bilgilerini girmeden kimliğinin yeniden doğrulanması. Bir kullanıcı oturumunu B2C uygulamanızı imzalamak isterse, mutlaka dışında kendi Facebook hesabıyla oturum istediği anlamına gelmez. Ancak, yerel hesaplar için söz konusu olduğunda, kullanıcının oturumunu düzgün sonlandırılır.
> 
> 

## <a name="use-your-own-b2c-tenant"></a>B2C kiracısına kullanın
Bu istekleri kendiniz için denemek istiyorsanız, önce bu üç adımı gerçekleştirin ve ardından daha önce kendi açıklanan örnek değerleri değiştirin:

1. [B2C kiracısı oluşturma](active-directory-b2c-get-started.md)ve isteklerinde kiracınızın adını kullanın.
2. [Uygulama oluşturma](active-directory-b2c-app-registration.md) bir uygulama kimliğini almak için Bir web uygulaması/web API uygulamanızı içerir. İsteğe bağlı olarak, bir uygulama gizli anahtarı oluşturun.
3. [İlkelerinizi oluşturma](active-directory-b2c-reference-policies.md) ilke adları elde edilir.

