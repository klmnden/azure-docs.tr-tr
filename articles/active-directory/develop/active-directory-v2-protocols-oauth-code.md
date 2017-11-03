---
title: "Azure AD v2.0 OAuth yetkilendirme kodu akışı | Microsoft Docs"
description: "Azure AD uygulama OAuth 2.0 kimlik doğrulama protokolü kullanarak web uygulamaları oluşturma."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: ae1d7d86-7098-468c-aa32-20df0a10ee3d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 1cffe40c14b931485cc5cec48a95e02ae770764e
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2017
---
# v2.0 protokolleri - OAuth 2.0 yetkilendirme kodu akışı
OAuth 2.0 yetkilendirme kodu verme web API'leri gibi korunan kaynakları erişim kazanmak için bir aygıtta yüklü uygulamalar kullanılabilir.  Uygulama modeli v2.0 's uygulamayı OAuth 2.0 kullanarak oturum açın ve API erişmek için mobil ve Masaüstü uygulamalarınızı ekleyebilirsiniz.  Bu kılavuzda dilden bağımsızdır ve bizim açık kaynak kitaplıkları kullanmadan HTTP iletileri almasına ve göndermesine açıklar.

> [!NOTE]
> Tüm Azure Active Directory senaryolarını ve özelliklerini v2.0 uç noktası tarafından desteklenir.  V2.0 uç kullanmanızın gerekli olup olmadığını belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
> 
> 

OAuth 2.0 yetkilendirme kodu akışını içinde açıklanan [4.1 OAuth 2.0 belirtimi bölüm](http://tools.ietf.org/html/rfc6749).  Kimlik doğrulama ve yetkilendirme dahil olmak üzere uygulama türleri çoğunu gerçekleştirmeniz için kullanılan [web uygulamaları](active-directory-v2-flows.md#web-apps) ve [uygulamaları'yerel olarak yüklenen](active-directory-v2-flows.md#mobile-and-native-apps).  Güvenli bir şekilde v2.0 uç noktası kullanılarak güvenlik altına kaynaklara erişmek için kullanılan access_tokens edinmeye uygulamaları etkinleştirir.  

## Protokol diyagramı
Yerel/mobil uygulama için tüm kimlik doğrulama akışı, yüksek düzeyde, şöyle bir bit görünür:

![OAuth kimlik doğrulama kodu akışı](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## İstek bir kimlik doğrulama kodu
Yetkilendirme kodu akışını kullanıcıya yönlendirerek istemci ile başlayan `/authorize` uç noktası.  Bu istekte istemci kullanıcıdan almak için gereken izinleri gösterir:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [!TIP]
> Bu isteğin yürütülmesi için aşağıdaki bağlantıya tıklayın! Oturum açtıktan sonra tarayıcınızı için yeniden yönlendirilmesi gereken `https://localhost/myapp/` ile bir `code` adres çubuğundaki.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>
> 
> 

| Parametre |  | Açıklama |
| --- | --- | --- |
| Kiracı |Gerekli |`{tenant}` İstek yolunu değerinde uygulamasına oturum denetlemek için kullanılabilir.  İzin verilen değerler: `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları.  Daha fazla ayrıntı için [protokol Temelleri](active-directory-v2-protocols.md#endpoints). |
| client_id |Gerekli |Uygulama kimliği, kayıt Portalı'nı ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uygulamanızı atanmış. |
| response_type |Gerekli |İçermelidir `code` yetkilendirme kodu akışı. |
| redirect_uri |Önerilen |Burada kimlik doğrulama yanıtları gönderilebilen veya uygulamanız tarafından alınan, uygulamanızın redirect_uri.  Bu tam bir url kodlanmış olmalıdır dışında Portalı'nda kayıtlı redirect_uris eşleşmelidir.  Yerel & mobil uygulamaları için varsayılan değeri kullanması gereken `https://login.microsoftonline.com/common/oauth2/nativeclient`. |
| Kapsam |Gerekli |Boşlukla ayrılmış bir listesini [kapsamları](active-directory-v2-scopes.md) sayılırsınız kullanıcıya istiyor. |
| response_mode |Önerilen |Sonuçta elde edilen belirteci geri uygulamanıza göndermek için kullanılacak yöntemi belirtir.  Olabilir `query` veya `form_post`. |
| durum |Önerilen |Belirteç yanıtta döndürülen de istekte bulunan bir değer.  İstediğiniz herhangi bir içerik dizesi olabilir.  Rastgele oluşturulan benzersiz bir değer tipik olarak kullanılan [siteler arası istek sahteciliğini saldırılarını önleme](http://tools.ietf.org/html/rfc6749#section-10.12).  Durumu, sayfa veya görünüm üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce uygulama kullanıcının durumu hakkındaki bilgileri kodlamak için de kullanılır. |
| istemi |İsteğe bağlı |Gerekli bir kullanıcı etkileşimi türünü belirtir.  Geçerli değerler yalnızca şu anda 'oturum açma', 'none' olan ve 'onay'.  `prompt=login`Bu isteğin negating çoklu oturum açma kimlik bilgilerini girmesini zorunlu tutar.  `prompt=none`- tersidir kullanıcı hiçbir etkileşimli istemi doğabilecek sunulmayan garanti eder.  İstek sessizce çoklu oturum açma aracılığıyla tamamlanamazsa, v2.0 uç noktası bir hata döndürür.  `prompt=consent`Uygulama izinleri vermek için kullanıcı isteyen kullanıcı, oturum sonra OAuth onay iletişim tetikler. |
| login_hint |İsteğe bağlı |Kullanıcı adlarını önceden biliyorsanız, oturum açma sayfasında kullanıcının kullanıcı adı/e-posta adresi alanının önceden doldurmak için kullanılabilir.  Username önceki oturum açma kullanarak bir zaten ayıklanan yeniden kimlik doğrulaması sırasında bu parametre genellikle uygulamaları kullanacak `preferred_username` talep. |
| domain_hint |İsteğe bağlı |Aşağıdakilerden biri olabilir `consumers` veya `organizations`.  Dahil edilmişse, e-posta tabanlı bulma işlemini atlar kullanıcı v2.0 oturum açma sayfasında, biraz daha kolay bir kullanıcı deneyimi baştaki geçtiği.  Genellikle uygulamaları kullanacağınız Bu parametre yeniden kimlik doğrulaması sırasında çıkartarak `tid` gelen bir önceki oturum açma.  Varsa `tid` değer talep `9188040d-6c67-4c5b-b112-36a304b66dad`, kullanmanız gereken `domain_hint=consumers`.  Aksi takdirde kullanın `domain_hint=organizations`. |

Bu noktada, kullanıcı kimlik bilgilerini girin ve kimlik doğrulamasını tamamlamak için istenir.  V2.0 uç noktası, ayrıca kullanıcı belirtilen izinleri seçtiği sağlayacak `scope` sorgu parametresi.  Kullanıcı bu izinleri hiçbirine seçtiği değil kullanıcı gerekli izinleri onayı geçersiz sorar.  Ayrıntılarını [izinleri, onay ve çok kiracılı uygulamalara sağlanan burada](active-directory-v2-scopes.md).

Kullanıcı kimliğini doğrular ve izin veren sonra v2.0 uç noktası belirtilen uygulamanızı yanıt döndürülecek `redirect_uri`, bölümünde belirtilen yöntemi kullanarak `response_mode` parametresi.

#### Başarılı yanıt
Başarılı yanıt kullanarak `response_mode=query` gibi görünüyor:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| Kod |Uygulama istenen authorization_code. Uygulama, hedef kaynak için bir erişim belirteci istemek için yetkilendirme kodu kullanabilirsiniz.  Authorization_codes çok kısa süreli, genellikle bu, yaklaşık 10 dakika sonra süresi dolacak. |
| durum |İstekte bir durum parametresi eklenirse, aynı değeri yanıt olarak görünmelidir. Uygulama istek ve yanıt durum değerleri özdeş olduğunu doğrulamanız gerekir. |

#### Hata yanıtı
Hata yanıtları da gönderilebilir için `redirect_uri` uygulama bunları uygun şekilde işleyebilmesi için:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanılan ve hataları tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

#### Yetkilendirme uç noktası hataları için hata kodları
Aşağıdaki tabloda, döndürülen çeşitli hata kodları açıklanmaktadır `error` hata yanıtı parametresi.

| Hata kodu | Açıklama | İstemci eylemi |
| --- | --- | --- |
| invalid_request |Eksik gerekli parametre gibi protokol hatası. |Düzeltin ve isteği yeniden gönderin. Bir geliştirme budur hata genellikle yakalanan ilk test sırasında. |
| unauthorized_client |İstemci uygulaması bir kimlik doğrulama kodu isteme izni yok. |Bu durum genellikle, istemci uygulaması Azure AD'de kayıtlı değil veya kullanıcının Azure AD kiracısı eklenmez genellikle ortaya çıkar. Uygulama yönerge Azure AD'ye ekleme ve uygulama yükleme için kullanıcıya sor |
| ACCESS_DENIED |Kaynak sahibi izin reddedildi |İstemci uygulaması kullanıcı izin sürece devam edemiyor kullanıcı bildirebilir. |
| unsupported_response_type |Yetkilendirme sunucusu isteğine yanıt türünü desteklemiyor. |Düzeltin ve isteği yeniden gönderin. Bir geliştirme budur hata genellikle yakalanan ilk test sırasında. |
| server_error |Sunucu beklenmeyen bir hatayla karşılaştı. |İsteği yeniden deneyin. Bu hatalar geçici koşulları neden olabilir. İstemci uygulaması yanıt geçici bir hata gecikti kullanıcıya açıklayabilir. |
| temporarily_unavailable |Sunucunun geçici olarak istek işlemek için çok meşgul. |İsteği yeniden deneyin. İstemci uygulaması yanıt geçici bir durum gecikti kullanıcıya açıklayabilir. |
| invalid_resource |Hedef kaynak mevcut değil, Azure AD, bulunamıyor veya düzgün yapılandırılmamış olduğundan geçerli değil. |Bu, varsa, kaynak kiracısında yapılandırılmadı gösterir. Uygulama yönerge Azure AD'ye ekleme ve uygulama yükleme için kullanıcıya sor |

## İstek bir erişim belirteci
Bir authorization_code edindiğiniz ve kullanıcı tarafından izin verilen göre kullanmak `code` için bir `access_token` göndererek istenen kaynak için bir `POST` isteği `/token` uç noktası:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [!TIP]
> Postman içinde bu isteği yürütmeden deneyin! (Değiştirmeyi unutmayın `code`) [ ![Postman Çalıştır](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

| Parametre |  | Açıklama |
| --- | --- | --- |
| Kiracı |Gerekli |`{tenant}` İstek yolunu değerinde uygulamasına oturum denetlemek için kullanılabilir.  İzin verilen değerler: `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları.  Daha fazla ayrıntı için [protokol Temelleri](active-directory-v2-protocols.md#endpoints). |
| client_id |Gerekli |Uygulama kimliği, kayıt Portalı'nı ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uygulamanızı atanmış. |
| grant_type |Gerekli |Olmalıdır `authorization_code` yetkilendirme kodu akışı. |
| Kapsam |Gerekli |Kapsamları boşlukla ayrılmış listesi.  Bu leg istenen kapsamları eşdeğer veya bir alt kümesini ilk bacaktaki istenen kapsamları olmalıdır.  Bu istekte belirtilen kapsamları birden çok kaynak sunucuları yayılıyorsa v2.0 uç ilk kapsamında belirtilen kaynak için bir belirteç döndürür.  Kapsamları hakkında daha ayrıntılı açıklaması için başvurmak [izinleri, onay ve kapsamları](active-directory-v2-scopes.md). |
| Kod |Gerekli |Akış ilk leg içinde aldığınız authorization_code. |
| redirect_uri |Gerekli |Authorization_code almak için kullanılan aynı redirect_uri değer. |
| client_secret |Web uygulamaları için gerekli |Uygulamanız için uygulama kayıt Portalı'nda oluşturulan uygulama gizli anahtarı.  Client_secrets güvenilir bir şekilde cihazlarda depolanamaz çünkü yerel bir uygulamada kullanılmamalıdır.  Web uygulamaları ve web sunucu tarafında client_secret güvenli bir şekilde saklayın olanağına sahip API'leri için gereklidir. |

#### Başarılı yanıt
Başarılı bir token yanıt gibi görünür:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametre | Açıklama |
| --- | --- |
| access_token |İstenen erişim belirteci. Uygulama, web API'si gibi güvenli kaynak kimliğini doğrulamak için bu belirteci kullanabilirsiniz. |
| token_type |Belirteç türü değeri gösterir. Azure AD destekleyen tek taşıyıcı türüdür |
| expires_in |Ne kadar süreyle erişim belirteci (saniye olarak) geçerli değil. |
| Kapsam |Access_token için geçerli olan kapsamlar. |
| refresh_token |Bir OAuth 2.0 yenileme belirteci. Uygulama bu belirteci kullanabilirsiniz geçerli erişim belirtecinin süresi dolduktan sonra ek erişim belirteçleri almak.  Refresh_tokens uzun süreli ve uzun süre için kaynaklara erişimi korumak için kullanılır.  Daha fazla ayrıntı için bkz [v2.0 belirteç başvurusu](active-directory-v2-tokens.md). <br> **Not:** yalnızca sağlanan IF `offline_access` kapsam istendi. |
| id_token |Bir imzasız JSON Web Token (JWT). Uygulama can base64Url bu belirteç için oturum kullanıcı hakkında bilgi parçalarını kodunu çözer. Uygulama değerleri önbelleğe ve bunları görüntüler, ancak, bunlar üzerinde herhangi bir yetkilendirme veya güvenlik sınırları için güvenmemelisiniz.  İd_tokens hakkında daha fazla bilgi için bkz: [v2.0 uç noktası belirteç başvurusu](active-directory-v2-tokens.md). <br> **Not:** yalnızca sağlanan IF `openid` kapsam istendi. |
#### Hata yanıtı
Hata yanıtları gibi görünür:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanılan ve hataları tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |
| error_codes |Tanılamada yardımcı olabilecek STS belirli hata kodlarının listesi. |
| timestamp |Hatanın gerçekleştiği zaman. |
| trace_id |Tanılamada yardımcı olabilecek isteği için benzersiz bir tanımlayıcı. |
| correlation_id |Tanılamada bileşenlerinde yardımcı olabilecek isteği için benzersiz bir tanımlayıcı. |

#### Belirteç uç noktası hataları için hata kodları
| Hata kodu | Açıklama | İstemci eylemi |
| --- | --- | --- |
| invalid_request |Eksik gerekli parametre gibi protokol hatası. |Düzeltin ve isteği yeniden gönderin |
| invalid_grant |Yetkilendirme kodu geçersiz veya süresi doldu. |Yeni bir istek deneyin `/authorize` uç noktası |
| unauthorized_client |Kimliği doğrulanmış istemci bu yetkilendirme verme türünü kullanmak için yetkili değil. |Bu durum genellikle, istemci uygulaması Azure AD'de kayıtlı değil veya kullanıcının Azure AD kiracısı eklenmez genellikle ortaya çıkar. Uygulama yönerge Azure AD'ye ekleme ve uygulama yükleme için kullanıcıya sor |
| invalid_client |İstemci kimlik doğrulaması başarısız oldu. |İstemci kimlik bilgileri geçerli değil. Düzeltmek için uygulama Yöneticisi kimlik bilgilerini güncelleştirir. |
| unsupported_grant_type |Yetkilendirme sunucusu yetkilendirme verme türünü desteklemiyor. |İstekteki grant türünü değiştirin. Bu tür bir hata yalnızca geliştirme sırasında gerçekleşmesi gerektiğini ve ilk test sırasında algılandı. |
| invalid_resource |Hedef kaynak mevcut değil, Azure AD, bulunamıyor veya düzgün yapılandırılmamış olduğundan geçerli değil. |Bu, varsa, kaynak kiracısında yapılandırılmadı gösterir. Uygulama yönerge Azure AD'ye ekleme ve uygulama yükleme için kullanıcıya sor |
| interaction_required |İstek kullanıcı etkileşimi gerektirir. Örneğin, bir ek kimlik doğrulama adım gereklidir. |Aynı kaynak isteği yeniden deneyin. |
| temporarily_unavailable |Sunucunun geçici olarak istek işlemek için çok meşgul. |İsteği yeniden deneyin. İstemci uygulaması yanıt geçici bir durum gecikti kullanıcıya açıklayabilir. |

## Erişim belirteci kullanın
Başarıyla edindiğiniz göre bir `access_token`, belirteç isteklerini Web API'lerine dahil ederek kullanabileceğiniz `Authorization` üstbilgisi:

> [!TIP]
> Bu istek içinde Postman yürütme! (Değiştir `Authorization` üstbilgi ilk) [ ![Postman Çalıştır](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## Erişim belirteci Yenile
Access_tokens kısa süreli ve kaynaklara erişmeye devam etmek için parolalarının süresi dolduktan sonra bunları yenilemeniz gerekir.  Başka bir göndererek yapabilirsiniz `POST` isteği `/token` uç nokta, bu sefer sağlanması `refresh_token` yerine `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for web apps
```

> [!TIP]
> Postman içinde bu isteği yürütmeden deneyin! (Değiştirmeyi unutmayın `refresh_token`) [ ![Postman Çalıştır](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

| Parametre |  | Açıklama |
| --- | --- | --- |
| Kiracı |Gerekli |`{tenant}` İstek yolunu değerinde uygulamasına oturum denetlemek için kullanılabilir.  İzin verilen değerler: `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları.  Daha fazla ayrıntı için [protokol Temelleri](active-directory-v2-protocols.md#endpoints). |
| client_id |Gerekli |Uygulama kimliği, kayıt Portalı'nı ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uygulamanızı atanmış. |
| grant_type |Gerekli |Olmalıdır `refresh_token` bu leg yetkilendirme kodu akışı için. |
| Kapsam |Gerekli |Kapsamları boşlukla ayrılmış listesi.  Bu leg istenen kapsamları eşdeğer veya bir alt kümesini özgün authorization_code isteği bacaktaki istenen kapsamları olmalıdır.  Bu istekte belirtilen kapsamları birden çok kaynak sunucuları yayılıyorsa v2.0 uç ilk kapsamında belirtilen kaynak için bir belirteç döndürür.  Kapsamları hakkında daha ayrıntılı açıklaması için başvurmak [izinleri, onay ve kapsamları](active-directory-v2-scopes.md). |
| refresh_token |Gerekli |Akış ikinci leg içinde aldığınız refresh_token. |
| redirect_uri |Gerekli |Authorization_code almak için kullanılan aynı redirect_uri değer. |
| client_secret |Web uygulamaları için gerekli |Uygulamanız için uygulama kayıt Portalı'nda oluşturulan uygulama gizli anahtarı.  Client_secrets güvenilir bir şekilde cihazlarda depolanamaz çünkü yerel bir uygulamada kullanılmamalıdır.  Web uygulamaları ve web sunucu tarafında client_secret güvenli bir şekilde saklayın olanağına sahip API'leri için gereklidir. |

#### Başarılı yanıt
Başarılı bir token yanıt gibi görünür:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametre | Açıklama |
| --- | --- |
| access_token |İstenen erişim belirteci. Uygulama, web API'si gibi güvenli kaynak kimliğini doğrulamak için bu belirteci kullanabilirsiniz. |
| token_type |Belirteç türü değeri gösterir. Azure AD destekleyen tek taşıyıcı türüdür |
| expires_in |Ne kadar süreyle erişim belirteci (saniye olarak) geçerli değil. |
| Kapsam |Access_token için geçerli olan kapsamlar. |
| refresh_token |Yeni bir OAuth 2.0 yenileme belirteci. Eski yenileme belirtecini yenileme belirteçleri mümkün olduğunca uzun bir süredir geçerli kalmasını sağlamak için bu yeni alınan yenileme belirteci ile değiştirmeniz gerekir. <br> **Not:** yalnızca sağlanan IF `offline_access` kapsam istendi. |
| id_token |Bir imzasız JSON Web Token (JWT). Uygulama can base64Url bu belirteç için oturum kullanıcı hakkında bilgi parçalarını kodunu çözer. Uygulama değerleri önbelleğe ve bunları görüntüler, ancak, bunlar üzerinde herhangi bir yetkilendirme veya güvenlik sınırları için güvenmemelisiniz.  İd_tokens hakkında daha fazla bilgi için bkz: [v2.0 uç noktası belirteç başvurusu](active-directory-v2-tokens.md). <br> **Not:** yalnızca sağlanan IF `openid` kapsam istendi. |

#### Hata yanıtı
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanılan ve hataları tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |
| error_codes |Tanılamada yardımcı olabilecek STS belirli hata kodlarının listesi. |
| timestamp |Hatanın gerçekleştiği zaman. |
| trace_id |Tanılamada yardımcı olabilecek isteği için benzersiz bir tanımlayıcı. |
| correlation_id |Tanılamada bileşenlerinde yardımcı olabilecek isteği için benzersiz bir tanımlayıcı. |

Hata kodları ve önerilen istemci eylemi açıklaması için lütfen bkz [belirteç uç noktası hataları için hata kodları](#error-codes-for-token-endpoint-errors).

