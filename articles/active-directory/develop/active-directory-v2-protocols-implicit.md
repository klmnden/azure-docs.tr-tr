---
title: "Azure AD v2.0 örtük akışını kullanarak tek sayfa uygulamaları güvenli | Microsoft Docs"
description: "Tek sayfa uygulamaları için Azure AD v2.0 uygulaması örtük akışını kullanarak web uygulamaları oluşturma."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 3605931f-dc24-4910-bb50-5375defec6a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 3bd8256814036a357b30b69286da6bb7c974162f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="v20-protocols---spas-using-the-implicit-flow"></a>v2.0 protokolleri - örtük akışını kullanarak SPAs
V2.0 uç noktası ile kullanıcılar, tek sayfa uygulamaları Microsoft'tan hem kişisel hem de iş/Okul hesaplarıyla içine imzalayabilirsiniz.  Tek sayfa ve diğer JavaScript uygulamalar, öncelikle birkaç ilginç sınar kimlik doğrulaması geldiğinde bir tarayıcı yazıtipi olarak çalıştırın:

* Bu uygulamaların güvenlik özelliklerini tabanlı geleneksel sunucu web uygulamalarından önemli ölçüde farklıdır.
* Çok sayıda yetkilendirme sunucularını & kimlik sağlayıcıları CORS isteklerini desteklemez.
* Kullanıcı deneyimi özellikle bozucu hale uygulama çıktığınızda tam sayfa tarayıcı yönlendirir.

Bu uygulamalar için (düşünün: AngularJS, Ember.js, React.js, vb.) Azure AD OAuth 2.0 örtük verme akışını destekler.  Örtük akış açıklanan [OAuth 2.0 belirtimi](http://tools.ietf.org/html/rfc6749#section-4.2).  Birincil avantajı belirteçleri Azure AD'den bir arka uç sunucusu yapmadan kimlik bilgisi exchange almak uygulama tanımasıdır.  Bu, kullanıcı oturum açabilir, oturumu korumak ve JavaScript kodu istemcisi içindeki tüm diğer Web API'leri belirteçleri almak yazmasına izin verir.  Örtük akış - özellikle yaklaşık kullanırken dikkate almanız gereken birkaç önemli güvenlik noktalar vardır [istemci](http://tools.ietf.org/html/rfc6749#section-10.3) ve [kullanıcı kimliğine bürünme özelliğini](http://tools.ietf.org/html/rfc6749#section-10.3).

Örtük akış ve Azure AD kimlik doğrulama JavaScript uygulamanıza eklemek için kullanmak istiyorsanız, bizim açık kaynak JavaScript Kitaplığı kullanmanız önerilir [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js).  Birkaç AngularJS öğreticileri kullanılabilir [burada](active-directory-appmodel-v2-overview.md#getting-started) başlamanıza yardımcı olacak.  

Ancak, tek sayfa uygulamanızı kitaplıkta kullanma ve kendiniz protokol iletilerini göndermek tercih ederseniz, aşağıdaki genel adımları izleyin.

> [!NOTE]
> Tüm Azure Active Directory senaryolarını ve özelliklerini v2.0 uç noktası tarafından desteklenir.  V2.0 uç kullanmanızın gerekli olup olmadığını belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
> 
> 

## <a name="protocol-diagram"></a>Protokol diyagramı
Tüm örtük oturum açma akışını şuna benzer - adımların her biri aşağıda ayrıntılı olarak açıklanmıştır.

![Openıd Connect kulvarları](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>Oturum açma isteği gönder
Başlangıçta kullanıcı uygulamanıza imzalamak için gönderebilirsiniz bir [Openıd Connect](active-directory-v2-protocols-oidc.md) yetkilendirme isteği ve get bir `id_token` v2.0 uç noktasından:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [!TIP]
> Bu isteğin yürütülmesi için aşağıdaki bağlantıya tıklayın! Oturum açtıktan sonra tarayıcınızı için yeniden yönlendirilmesi gereken `https://localhost/myapp/` ile bir `id_token` adres çubuğundaki.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>
> 
> 

| Parametre |  | Açıklama |
| --- | --- | --- |
| Kiracı |Gerekli |`{tenant}` İstek yolunu değerinde uygulamasına oturum denetlemek için kullanılabilir.  İzin verilen değerler: `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları.  Daha fazla ayrıntı için [protokol Temelleri](active-directory-v2-protocols.md#endpoints). |
| client_id |Gerekli |Uygulama kimliği, kayıt Portalı'nı ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uygulamanızı atanmış. |
| response_type |Gerekli |İçermelidir `id_token` Openıd Connect oturum açma için.  Response_type içerebilir `token`. Kullanarak `token` hemen bir erişim belirteci uç noktayı yetkilendirmek için ikinci bir isteği yapmak zorunda kalmadan authorize uç noktasından almak üzere uygulamanızı buraya izin verir.  Kullanırsanız `token` response_type, `scope` parametresi için belirteci vermek için hangi kaynak belirten bir kapsam içermesi gerekir. |
| redirect_uri |Önerilen |Burada kimlik doğrulama yanıtları gönderilebilen veya uygulamanız tarafından alınan, uygulamanızın redirect_uri.  Bu tam bir url kodlanmış olmalıdır dışında Portalı'nda kayıtlı redirect_uris eşleşmelidir. |
| Kapsam |Gerekli |Kapsamları boşlukla ayrılmış listesi.  Openıd Connect için kapsam içermesi gerekir `openid`, UI onay "oturum" izni çevirir.  İsteğe bağlı olarak, aynı zamanda eklemek isteyebilirsiniz `email` veya `profile` [kapsamları](active-directory-v2-scopes.md) ek kullanıcı verilerine erişim kazanmak için.  Bu istek çeşitli kaynaklara onay isteyen diğer kapsamları ekleyebiliriz. |
| response_mode |Önerilen |Sonuçta elde edilen belirteci geri uygulamanıza göndermek için kullanılacak yöntemi belirtir.  Olmalıdır `fragment` örtük akış için. |
| durum |Önerilen |Belirteç yanıtta döndürülen de istekte bulunan bir değer.  İstediğiniz herhangi bir içerik dizesi olabilir.  Rastgele oluşturulan benzersiz bir değer tipik olarak kullanılan [siteler arası istek sahteciliğini saldırılarını önleme](http://tools.ietf.org/html/rfc6749#section-10.12).  Durumu, sayfa veya görünüm üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce uygulama kullanıcının durumu hakkındaki bilgileri kodlamak için de kullanılır. |
| nonce |Gerekli |Bir talep olarak elde edilen id_token dahil edilecek ve uygulama tarafından üretilen istekte bulunan bir değer.  Uygulama sonra belirteç yeniden yürütme saldırıları azaltmak için bu değeri doğrulayabilirsiniz.  Genellikle istek kaynağını tanımlamak için kullanılan rastgele, benzersiz bir dize değeridir. |
| istemi |İsteğe bağlı |Gerekli bir kullanıcı etkileşimi türünü belirtir.  Geçerli değerler yalnızca şu anda 'oturum açma', 'none' olan ve 'onay'.  `prompt=login`Bu isteğin negating çoklu oturum açma kimlik bilgilerini girmesini zorunlu tutar.  `prompt=none`- tersidir kullanıcı hiçbir etkileşimli istemi doğabilecek sunulmayan garanti eder.  İstek sessizce çoklu oturum açma aracılığıyla tamamlanamazsa, v2.0 uç noktası bir hata döndürür.  `prompt=consent`Uygulama izinleri vermek için kullanıcı isteyen kullanıcı, oturum sonra OAuth onay iletişim tetikler. |
| login_hint |İsteğe bağlı |Kullanıcı adlarını önceden biliyorsanız, oturum açma sayfasında kullanıcının kullanıcı adı/e-posta adresi alanının önceden doldurmak için kullanılabilir.  Username önceki oturum açma kullanarak bir zaten ayıklanan yeniden kimlik doğrulaması sırasında bu parametre genellikle uygulamaları kullanacak `preferred_username` talep. |
| domain_hint |İsteğe bağlı |Aşağıdakilerden biri olabilir `consumers` veya `organizations`.  Dahil edilmişse, e-posta tabanlı bulma işlemini atlar kullanıcı v2.0 oturum açma sayfasında, biraz daha kolay bir kullanıcı deneyimi baştaki geçtiği.  Genellikle uygulamaları kullanacağınız Bu parametre yeniden kimlik doğrulaması sırasında çıkartarak `tid` id_token talep.  Varsa `tid` değer talep `9188040d-6c67-4c5b-b112-36a304b66dad`, kullanmanız gereken `domain_hint=consumers`.  Aksi takdirde kullanın `domain_hint=organizations`. |

Bu noktada, kullanıcı kimlik bilgilerini girin ve kimlik doğrulamasını tamamlamak için istenir.  V2.0 uç noktası, ayrıca kullanıcı belirtilen izinleri seçtiği sağlayacak `scope` sorgu parametresi.  Kullanıcı bu izinleri hiçbirine seçtiği değil kullanıcı gerekli izinleri onayı geçersiz sorar.  Ayrıntılarını [izinleri, onay ve çok kiracılı uygulamalara sağlanan burada](active-directory-v2-scopes.md).

Kullanıcı kimliğini doğrular ve izin veren sonra v2.0 uç noktası belirtilen uygulamanızı yanıt döndürülecek `redirect_uri`, bölümünde belirtilen yöntemi kullanarak `response_mode` parametresi.

#### <a name="successful-response"></a>Başarılı yanıt
Başarılı yanıt kullanarak `response_mode=fragment` ve `response_type=id_token+token` okunabilirliği için satır sonu ile aşağıdaki gibi görünür:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| access_token |Eklenen IF `response_type` içeren `token`. Uygulama, bu durumda Microsoft Graph için istenen erişim belirteci.  Erişim belirteci kodunu çözdü veya aksi halde Denetlenmekte, genel olmayan bir dize olarak işlenebilir. |
| token_type |Eklenen IF `response_type` içeren `token`.  Her zaman açık `Bearer`. |
| expires_in |Eklenen IF `response_type` içeren `token`.  Belirteç amacıyla önbelleğe alma için geçerli kaldığı saniye sayısını gösterir. |
| Kapsam |Eklenen IF `response_type` içeren `token`.  Access_token geçerli olacağı kapsamlar gösterir. |
| id_token |Uygulama istenen id_token. Kullanıcının kimliğini doğrulamak ve kullanıcı oturumu başlatmak için id_token kullanabilirsiniz.  İd_tokens ve içerikleri daha ayrıntılı bilgi yer almaktadır [v2.0 uç noktası belirteç başvurusu](active-directory-v2-tokens.md). |
| durum |İstekte bir durum parametresi eklenirse, aynı değeri yanıt olarak görünmelidir. Uygulama istek ve yanıt durum değerleri özdeş olduğunu doğrulamanız gerekir. |

#### <a name="error-response"></a>Hata yanıtı
Hata yanıtları da gönderilebilir için `redirect_uri` uygulama bunları uygun şekilde işleyebilmesi için:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanılan ve hataları tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

## <a name="validate-the-idtoken"></a>İd_token doğrula
Yalnızca bir id_token alma kullanıcının kimliğini doğrulamak için yeterli değildir; id_token'ın imzayı doğrulamak ve uygulamanızın gereksinimleri başına belirtecinizdeki talepleri doğrulamanız gerekir.  V2.0 uç noktası kullanan [JSON Web belirteçleri (Jwt'ler)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ve Belirteçleri imzalamak ve bunların geçerli olduğunu doğrulamak için ortak anahtar şifrelemesi.

Doğrulamayı seçebilirsiniz `id_token` istemci kodu ancak ortak bir uygulama olan göndermek için `id_token` bir arka uç sunucusuna ve doğrulama var. gerçekleştirin.  İd_token imza doğruladıktan sonra birkaç talep doğrulamak için gerekli vardır.  Bkz: [v2.0 belirteç başvurusu](active-directory-v2-tokens.md) daha fazla bilgi dahil olmak üzere [doğrulama belirteçleri](active-directory-v2-tokens.md#validating-tokens) ve [önemli bilgiler hakkında imzalama anahtarı Rollover](active-directory-v2-tokens.md#validating-tokens).  Öneririz ayrıştırma ve doğrulama yapmayı kullanımını kitaplık belirteçler - en az bir olduğundan çoğu diller ve platformlar için kullanılabilir.
<!--TODO: Improve the information on this-->

Senaryonuza bağlı olarak ek talep doğrulamak isteyebilir.  Bazı ortak doğrulamaları şunları içerir:

* Kullanıcı/kuruluş sağlama uygulama için kaydolmuş.
* Uygun yetkilendirme/ayrıcalıklara sahip kullanıcı sağlama
* Belirli bir kimlik doğrulama gücünü sağlayarak, çok faktörlü kimlik doğrulaması gibi oluştu.

Bir id_token talepleri hakkında daha fazla bilgi için bkz: [v2.0 uç noktası belirteç başvurusu](active-directory-v2-tokens.md).

İd_token tamamen doğruladıktan sonra kullanıcı oturumu başlatmak ve talepleri, uygulamanızda kullanıcı hakkında bilgi edinmek için id_token'ni kullanın.  Bu bilgiler kullanılabilir ekran kayıtları, yetkilerini, vb. için.

## <a name="get-access-tokens"></a>Erişim belirteci alın
Kullanıcı tek sayfa uygulamanıza oturum açtığınız, çağrıyı yapan web API'leri gibi Azure AD tarafından güvenli erişim belirteçleri elde edebilirsiniz [Microsoft Graph](https://graph.microsoft.io).  Zaten bir belirteç kullanılarak alınan olsa bile `token` response_type, kullanıcı yeniden oturum açmak için yeniden yönlendirme gerek kalmadan ek kaynaklara belirteçleri almak için bu yöntemi kullanabilirsiniz.

Normal Openıd Connect/OAuth akış içinde v2.0 için bir istekte bunu `/token` uç noktası.  Ancak, almak ve belirteçleri yenilemek için AJAX çağrıları yapma sorunun dışında olacak şekilde v2.0 uç CORS isteklerini desteklemez.  Bunun yerine, diğer web API'leri yeni belirteçleri almak için gizli bir iframe örtük akış kullanabilirsiniz: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [!TIP]
> Kopyalama ve yapıştırma deneyin isteği bir tarayıcı sekmesi içine aşağıda! (Değiştirmeyi unutmayın `domain_hint` ve `login_hint` değerleri, kullanıcı için doğru değerlerle)
> 
> 

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| Kiracı |Gerekli |`{tenant}` İstek yolunu değerinde uygulamasına oturum denetlemek için kullanılabilir.  İzin verilen değerler: `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları.  Daha fazla ayrıntı için [protokol Temelleri](active-directory-v2-protocols.md#endpoints). |
| client_id |Gerekli |Uygulama kimliği, kayıt Portalı'nı ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uygulamanızı atanmış. |
| response_type |Gerekli |İçermelidir `id_token` Openıd Connect oturum açma için.  Bu ayrıca diğer response_types gibi içerebilir `code`. |
| redirect_uri |Önerilen |Burada kimlik doğrulama yanıtları gönderilebilen veya uygulamanız tarafından alınan, uygulamanızın redirect_uri.  Bu tam bir url kodlanmış olmalıdır dışında Portalı'nda kayıtlı redirect_uris eşleşmelidir. |
| Kapsam |Gerekli |Kapsamları boşlukla ayrılmış listesi.  Belirteçleri almak için tüm dahil [kapsamları](active-directory-v2-scopes.md) kaynağın ilgi gerektirir. |
| response_mode |Önerilen |Sonuçta elde edilen belirteci geri uygulamanıza göndermek için kullanılacak yöntemi belirtir.  Aşağıdakilerden biri olabilir `query`, `form_post`, veya `fragment`. |
| durum |Önerilen |Belirteç yanıtta döndürülen de istekte bulunan bir değer.  İstediğiniz herhangi bir içerik dizesi olabilir.  Rastgele oluşturulan benzersiz bir değer genellikle siteler arası istek sahteciliği saldırılarına önlemek için kullanılır.  Durumu, sayfa veya görünüm üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce uygulama kullanıcının durumu hakkındaki bilgileri kodlamak için de kullanılır. |
| nonce |Gerekli |Bir talep olarak elde edilen id_token dahil edilecek ve uygulama tarafından üretilen istekte bulunan bir değer.  Uygulama sonra belirteç yeniden yürütme saldırıları azaltmak için bu değeri doğrulayabilirsiniz.  Genellikle istek kaynağını tanımlamak için kullanılan rastgele, benzersiz bir dize değeridir. |
| istemi |Gerekli |Yenileme & içinde gizli bir iframe belirteçleri almak için kullanmanız gereken `prompt=none` sağlamak IFRAME v2.0 oturum açma sayfası üzerinde askıda değil ve hemen döndürür. |
| login_hint |Gerekli |Yenileme & içinde gizli bir iframe belirteçleri almak için kullanıcının belirli bir anda zamanında olabilir birden çok oturumları arasında ayrım yapmak için bu ipucunda kullanıcının kullanıcı adı içermelidir. Önceki oturum açma kullanarak bir kullanıcı adı ayıklayabilirsiniz `preferred_username` talep. |
| domain_hint |Gerekli |Aşağıdakilerden biri olabilir `consumers` veya `organizations`.  Yenileme & içinde gizli bir iframe belirteçleri almak için istekte domain_hint eklemeniz gerekir.  Extract `tid` , bir önceki oturum kullanmak için hangi değeri belirlemek için açma id_token talep.  Varsa `tid` değer talep `9188040d-6c67-4c5b-b112-36a304b66dad`, kullanmanız gereken `domain_hint=consumers`.  Aksi takdirde kullanın `domain_hint=organizations`. |

Teşekkürler `prompt=none` parametresi, bu istek ya da başarılı ya da hemen başarısız olacak ve uygulamanıza döndürür.  Başarılı yanıt, belirtilen uygulamanızı gönderilir `redirect_uri`, bölümünde belirtilen yöntemi kullanarak `response_mode` parametresi.

#### <a name="successful-response"></a>Başarılı yanıt
Başarılı yanıt kullanarak `response_mode=fragment` gibi görünüyor:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parametre | Açıklama |
| --- | --- |
| access_token |Uygulama talep belirteci. |
| token_type |Her zaman açık `Bearer`. |
| durum |İstekte bir durum parametresi eklenirse, aynı değeri yanıt olarak görünmelidir. Uygulama istek ve yanıt durum değerleri özdeş olduğunu doğrulamanız gerekir. |
| expires_in |Ne kadar süreyle erişim belirteci (saniye olarak) geçerli değil. |
| Kapsam |Erişim belirteci için geçerli kapsam. |

#### <a name="error-response"></a>Hata yanıtı
Hata yanıtları da gönderilebilir için `redirect_uri` uygulama bunları uygun şekilde işleyebilmesi için.  Durumunda `prompt=none`, beklenen hata olacaktır:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanılan ve hataları tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

IFRAME istekte bu hatayı alırsanız, kullanıcı etkileşimli olarak yeniden yeni bir belirteç almak kaydolmalısınız.  Yolu, uygulamanız için anlamlı içinde bu durumun üstesinden seçebilirsiniz.

## <a name="refreshing-tokens"></a>Yenileme belirteçleri
Her ikisi de `id_token`s ve `access_token`s bir kısa süre uygulamanız bu yenilemek için hazırlanması gerekir böylece, düzenli aralıklarla belirteçler sonra dolacak.  İki tür belirteç yenilemek için kullanarak yukarıdaki aynı gizli IFRAME isteği gerçekleştirebilirsiniz `prompt=none` Azure AD davranışını denetlemek için parametre.  Yeni bir almak istiyorsanız `id_token`, kullandığınızdan emin olun `response_type=id_token` ve `scope=openid`, yanı sıra bir `nonce` parametresi.

## <a name="send-a-sign-out-request"></a>Bir oturum kapatma isteği gönder
Openıdconnect `end_session_endpoint` v2.0 uç noktası, bir kullanıcının oturumunu sona erdirmek ve v2.0 uç noktası tarafından ayarlanmış tanımlama bilgilerini temizlemek için bir istek göndermek uygulamanızı sağlar.  Tam olarak bir web uygulaması dışında bir kullanıcı oturum için uygulamanızı kendi kullanıcı (genellikle bir belirteç önbelleği temizlemek veya tanımlama bilgilerini silmek) oturumunu ve tarayıcıya yeniden yönlendirme:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/logout?post_logout_redirect_uri=https://localhost/myapp/
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| Kiracı |Gerekli |`{tenant}` İstek yolunu değerinde uygulamasına oturum denetlemek için kullanılabilir.  İzin verilen değerler: `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları.  Daha fazla ayrıntı için [protokol Temelleri](active-directory-v2-protocols.md#endpoints). |
| post_logout_redirect_uri | Önerilen | Oturum kapatma tamamlandıktan sonra kullanıcı döndürülmesi gerektiğini URL. Bu değer uygulama için URI kayıtlı yeniden yönlendirme biriyle eşleşmelidir. Dahil edilmezse, kullanıcı tarafından v2.0 uç genel bir ileti gösterilir. |
