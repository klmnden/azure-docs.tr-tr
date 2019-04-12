---
title: Microsoft kimlik platformu ve OAuth yetkilendirme kod akış | Azure
description: Microsoft kimlik platformu uygulaması OAuth 2.0 kimlik doğrulama protokolü kullanarak web uygulamaları oluşturma.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: ae1d7d86-7098-468c-aa32-20df0a10ee3d
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 79e0ebce5704e7b61956568f5ebbce6ea6cbc3af
ms.sourcegitcommit: 41015688dc94593fd9662a7f0ba0e72f044915d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59500966"
---
# <a name="microsoft-identity-platform-and-oauth-20-authorization-code-flow"></a>Microsoft kimlik platformu ve OAuth 2.0 yetkilendirme kod akışı

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

OAuth 2.0 yetkilendirme kodu verme, web API'leri gibi korunan kaynakları erişim kazanmak için cihazda yüklü olan uygulamalarda kullanılabilir. Microsoft kimlik platformu uygulaması OAuth 2.0 kullanarak oturum açın ve API'ye erişmek için mobil ve Masaüstü uygulamaları ekleyebilirsiniz. Bu kılavuz dilden bağımsızdır ve HTTP iletileri gönderip herhangi birini kullanmadan açıklar [Azure açık kaynak kimlik doğrulama kitaplıkları](active-directory-authentication-libraries.md).

> [!NOTE]
> Tüm Azure Active Directory senaryolarını ve özelliklerini Microsoft kimlik platformu uç noktası tarafından desteklenir. Microsoft kimlik platformu uç noktası kullanıyorsanız belirlemek için aşağıdaki hakkında bilgi edinin: [Microsoft Identity platform sınırlamaları](active-directory-v2-limitations.md).

OAuth 2.0 yetkilendirme kod akışı açıklanan [OAuth 2.0 belirtiminin 4.1 bölümünde](https://tools.ietf.org/html/rfc6749). Kimlik doğrulama ve yetkilendirme dahil olmak üzere, uygulama türleri çoğunu gerçekleştirmek için kullanılan [web uygulamaları](v2-app-types.md#web-apps) ve [yerel olarak yüklenen uygulamalar](v2-app-types.md#mobile-and-native-apps). Güvenli bir şekilde Microsoft kimlik platformu uç noktası tarafından güvenli hale getirilmiş kaynaklara erişmek için kullanılan access_tokens almak üzere uygulama akışını sağlar.

## <a name="protocol-diagram"></a>Protokol diyagramı

Yerel/mobil uygulama için tüm kimlik doğrulama akışı, yüksek düzeyde, şöyle bir bit görünür:

![OAuth yetkilendirme kod akışı](./media/v2-oauth2-auth-code-flow/convergence-scenarios-native.svg)

## <a name="request-an-authorization-code"></a>İstek bir yetkilendirme kodu

Kullanıcıya yönlendiren istemci yetkilendirme kod akışı başlar `/authorize` uç noktası. Bu istekte istemci kullanıcıdan almak için gerekli olan izinleri belirtir:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&state=12345
```

> [!TIP]
> Bu isteğin yürütülmesi için aşağıdaki bağlantıya tıklayın! Oturum açtıktan sonra tarayıcınızı için yeniden yönlendirilmesi gereken `https://localhost/myapp/` ile bir `code` adres çubuğundaki.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fuser.read&state=12345" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| Parametre    | Gerekli/isteğe bağlı | Açıklama |
|--------------|-------------|--------------|
| `tenant`    | gerekli    | `{tenant}` İstek yolunda değer, uygulamaya oturum denetimi için kullanılabilir. İzin verilen değerler `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları. Daha fazla ayrıntı için [protokolü temel](active-directory-v2-protocols.md#endpoints).  |
| `client_id`   | gerekli    | **Uygulama (istemci) kimliği** , [Azure portalında – uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) uygulamanıza atanan deneyimi.  |
| `response_type` | gerekli    | İçermelidir `code` yetkilendirme kod akışı için.       |
| `redirect_uri`  | gerekli | Burada kimlik doğrulama yanıtlarının gönderilebilen veya uygulamanız tarafından alınan uygulamanızın redirect_uri. Bu url olarak kodlanmış olması dışında Portalı'nda kayıtlı redirect_uris biri tam olarak eşleşmesi gerekir. Yerel & mobil uygulamaları için varsayılan değeri kullanması gereken `https://login.microsoftonline.com/common/oauth2/nativeclient`.   |
| `scope`  | gerekli    | Boşlukla ayrılmış bir listesini [kapsamları](v2-permissions-and-consent.md) onay kullanıcıya istiyor. |
| `response_mode`   | Önerilen | Uygulamanıza elde edilen belirteç geri göndermek için kullanılması gereken yöntemini belirtir. Aşağıdakilerden biri olabilir:<br/><br/>- `query`<br/>- `fragment`<br/>- `form_post`<br/><br/>`query` kod, yeniden yönlendirme URI'si üzerinde bir sorgu dizesi parametresi olarak sağlar. Örtük akışını kullanarak bir kimlik belirteci istediği, kullanamazsınız `query` belirtilmiş [Openıd spec](https://openid.net/specs/oauth-v2-multiple-response-types-1_0.html#Combinations). Yalnızca kodum istediği, kullanabileceğiniz `query`, `fragment`, veya `form_post`. `form_post` kodu, yeniden yönlendirme URI'sini içeren bir GÖNDERİ yürütür. Daha fazla bilgi için bkz. [Openıd Connect protokolünü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).  |
| `state`                 | Önerilen | Belirteç yanıtta döndürülecek isteğinde bulunan bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Rastgele oluşturulmuş bir benzersiz değer için genellikle kullanılan [siteler arası istek sahteciliğini saldırılarını önleme](https://tools.ietf.org/html/rfc6749#section-10.12). Sayfa ya da görünümü üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce değeri uygulamasında kullanıcının durumu hakkında bilgi de şifreleyebilirsiniz. |
| `prompt`  | isteğe bağlı    | Gerekli olan kullanıcı etkileşimi türünü belirtir. Şu anda yalnızca geçerli değerler `login`, `none`, ve `consent`.<br/><br/>- `prompt=login` Bu isteğin negating çoklu oturum açma kimlik bilgilerini girmesini zorunlu tutar.<br/>- `prompt=none` -tersidir kullanıcı hiçbir etkileşimli istemi olmadan sunulan değil sağlayacaktır. Microsoft kimlik platformu uç nokta isteği sessizce aracılığıyla çoklu oturum açma işleminin neden tamamlanamadığına varsa, döndürür bir `interaction_required` hata.<br/>- `prompt=consent` Kullanıcı uygulamayı izinler istendiği anın, oturum açtıktan sonra OAuth onay iletişim tetikler. |
| `login_hint`  | isteğe bağlı    | Önceden, kullanıcı adını biliyorsanız, kullanıcı için oturum açma sayfası kullanıcı adı/e-posta adresi alanının önceden doldurmak için kullanılabilir. Kullanıcı adı önceki oturum açma kullanarak bir zaten ayıklanan yeniden kimlik doğrulaması sırasında bu parametre genellikle uygulamaları kullanacağı `preferred_username` talep.   |
| `domain_hint`  | isteğe bağlı    | Herhangi birini `consumers` veya `organizations`.<br/><br/>Bu onay kutusu eklediyseniz, e-posta tabanlı bulma işlemi atlanacak biraz daha kolay bir kullanıcı deneyimi için önde gelen oturum açma sayfasında kullanıcı geçer. Uygulamalar genellikle kullanır bu parametreyi yeniden kimlik doğrulaması sırasında ayıklayarak `tid` gelen bir önceki oturum açma. Varsa `tid` değer talep `9188040d-6c67-4c5b-b112-36a304b66dad`, kullanmanız gereken `domain_hint=consumers`. Aksi takdirde kullanın `domain_hint=organizations`.  |
| `code_challenge_method` | isteğe bağlı    | Kodlamak için kullanılan yöntemin `code_verifier` için `code_challenge` parametresi. Aşağıdaki değerlerden biri olabilir:<br/><br/>- `plain` <br/>- `S256`<br/><br/>Dışlanırsa, `code_challenge` düz metin olarak kabul edilir `code_challenge` dahildir. Microsoft kimlik platformu destekler `plain` ve `S256`. Daha fazla bilgi için [PKCE RFC](https://tools.ietf.org/html/rfc7636). |
| `code_challenge`  | isteğe bağlı | Yetkilendirme kodu elde kavram anahtarı aracılığıyla kod Exchange (PKCE) için yerel bir istemciden güvenliğini sağlamak için kullanılır. Gerekli if `code_challenge_method` dahildir. Daha fazla bilgi için [PKCE RFC](https://tools.ietf.org/html/rfc7636). |

Bu noktada, kullanıcı kimlik bilgilerini girin ve kimlik doğrulamasını tamamlaması istenecektir. Microsoft kimlik platformu uç noktası, ayrıca kullanıcı için belirtilen izinleri onaylamasını sağlayacak `scope` sorgu parametresi. Kullanıcının tüm bu izinleri olmayan etmişse, onayı gerekli izinleri kullanıcıya sorar. Ayrıntılarını [izinleri, onay ve çok kiracılı uygulamalar sağlanan burada](v2-permissions-and-consent.md).

Kullanıcının kimliğini doğrular ve onayı veren sonra Microsoft kimlik platformu uç nokta uygulamanızı belirtilen bir yanıt döndürür `redirect_uri`, bölümünde belirtilen yöntemi kullanarak `response_mode` parametresi.

#### <a name="successful-response"></a>Başarılı yanıt

Başarılı yanıt kullanarak bir `response_mode=query` gibi görünür:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parametre | Açıklama  |
|-----------|--------------|
| `code` | Uygulama istenen authorization_code. Uygulama, hedef kaynağı için bir erişim belirteci istemek için yetkilendirme kodu kullanabilirsiniz. Authorization_codes kısa süreli, genellikle bu, yaklaşık 10 dakika sonra süresi dolar. |
| `state` | State parametresi istekte yer alıyorsa aynı değeri yanıt olarak görünmelidir. Uygulama istek ve yanıt durum değerleri özdeş olduğunu doğrulamanız gerekir. |

#### <a name="error-response"></a>Hata yanıtı

Hata yanıtları da gönderilebilir için `redirect_uri` uygulama bunları uygun şekilde işleyebilmesi için:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama  |
|----------|------------------|
| `error`  | Oluşan hataları türlerini sınıflandırmak için kullanılabilir ve hatalara tepki vermek için kullanılan bir hata kodu dizesi. |
| `error_description` | Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Yetkilendirme uç noktası hataları için hata kodları

Aşağıdaki tabloda, döndürülen çeşitli hata kodları açıklanmaktadır `error` hata yanıtı parametresi.

| Hata Kodu  | Açıklama    | İstemci eylemi   |
|-------------|----------------|-----------------|
| `invalid_request` | Gerekli parametre eksik gibi protokol hatası. | Düzeltin ve isteği yeniden gönderin. Bu genellikle ilk sınama sırasında yakalanan geliştirme hatasıdır. |
| `unauthorized_client` | İstemci uygulama bir yetkilendirme kodu istemek için izin verilen değil. | Bu hata genellikle istemci uygulaması, Azure AD'de kayıtlı değil veya kullanıcının Azure AD kiracısına eklenmez oluşur. Uygulama, uygulama yükleme ve Azure AD'ye ekleme yönerge kullanıcıyla isteyebilir. |
| `access_denied`  | Kaynak sahibi onayı reddedildi  | İstemci uygulama, kullanıcı onay sürece devam edemiyor kullanıcıya bildirebilir. |
| `unsupported_response_type` | Yetkilendirme sunucusu isteği yanıt türü desteklemiyor. | Düzeltin ve isteği yeniden gönderin. Bu genellikle ilk sınama sırasında yakalanan geliştirme hatasıdır.  |
| `server_error`  | Sunucu beklenmeyen bir hatayla karşılaştı.| İsteği yeniden deneyin. Bu hataların geçici koşullar neden olabilir. İstemci uygulama, kullanıcıya yanıtına geçici bir hata ertelendi açıklayabilir. |
| `temporarily_unavailable`   | Sunucunun geçici olarak isteği işleyemeyecek kadar meşgul. | İsteği yeniden deneyin. İstemci uygulama, kullanıcıya yanıtına bir geçici koşul nedeniyle ertelendi açıklayabilir. |
| `invalid_resource`  | Hedef kaynak yoksa, Azure AD, bulunamıyor veya düzgün yapılandırılmış olduğundan geçersiz. | Bu hata varsa, kaynak kiracıda yapılandırılmadı gösterir. Uygulama, uygulama yükleme ve Azure AD'ye ekleme yönerge kullanıcıyla isteyebilir. |
| `login_required` | Çok fazla veya kullanıcı bulunamadı | İstemciyi sessiz kimlik doğrulaması (`prompt=none`), ancak tek bir kullanıcı bulunamadı. Bu, vardır birden fazla kullanıcı etkin oturum veya kullanıcı yok anlamına gelebilir. Bu seçilen Kiracı dikkate alır (örneğin, iki Azure AD hesap etkin ve bir Microsoft hesabınız varsa ve `consumers` seçildiyse, sessiz kimlik doğrulaması çalışır). |
| `interaction_required` | İstek, kullanıcı etkileşimi gerektirir. | Bir ek kimlik doğrulama adımı veya onay gereklidir. Gerek olmadan isteği yeniden deneyin `prompt=none`. |

## <a name="request-an-access-token"></a>İstek bir erişim belirteci

Bir authorization_code edindiğiniz ve kullanıcı tarafından izin verilen göre kullanmak `code` için bir `access_token` istenen kaynağa. Göndererek bunu bir `POST` isteği `/token` uç noktası:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [!TIP]
> Postman içinde bu isteğin yürütülmesi deneyin! (Değiştirmeyi unutmayın `code`) [ ![Postman içinde çalıştırın](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

| Parametre  | Gerekli/isteğe bağlı | Açıklama     |
|------------|-------------------|----------------|
| `tenant`   | gerekli   | `{tenant}` İstek yolunda değer, uygulamaya oturum denetimi için kullanılabilir. İzin verilen değerler `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları. Daha fazla ayrıntı için [protokolü temel](active-directory-v2-protocols.md#endpoints).  |
| `client_id` | gerekli  | (İstemci) uygulama kimliği [Azure portalında – uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) uygulamanıza atanan sayfası. |
| `grant_type` | gerekli   | Olmalıdır `authorization_code` yetkilendirme kod akışı için.   |
| `scope`      | gerekli   | Kapsamları boşlukla ayrılmış listesi. İçinde bu oluşturan istenen kapsamlar, eşdeğer veya ilk oluşturan içinde istenen kapsamları bir alt kümesi olmalıdır. Ardından bu istekte belirtilen kapsamlar birden çok kaynak sunucusu yayılıyorsa, Microsoft kimlik platformu uç nokta ilk kapsamında belirtilen kaynak için bir belirteç döndürür. Kapsamlar hakkında daha ayrıntılı açıklaması için başvurmak [izinler ve onay kapsamları](v2-permissions-and-consent.md). |
| `code`          | gerekli  | Akışın ilk oluşturan satın aldığınız authorization_code. |
| `redirect_uri`  | gerekli  | Authorization_code almak için kullanılan aynı redirect_uri değer. |
| `client_secret` | Web apps için gerekli | Uygulama kayıt Portalı'nda uygulamanız için oluşturduğunuz uygulama gizli anahtarı. Client_secrets güvenilir bir şekilde cihazlarda depolanan olamaz çünkü yerel bir uygulamada uygulama gizli anahtarı kullanmamalısınız. Web uygulamaları ve web client_secret güvenli bir şekilde sunucu tarafında depolama yeteneği olan API'leri için zorunludur.  İstemci gizli anahtarı gönderilmeden önce URL kodlamalı olmalıdır.  |
| `code_verifier` | isteğe bağlı  | Authorization_code elde etmek için kullanılan aynı code_verifier. PKCE bir yetkilendirme kodu verme istekte kullanılan gereklidir. Daha fazla bilgi için [PKCE RFC](https://tools.ietf.org/html/rfc7636). |

### <a name="successful-response"></a>Başarılı yanıt

Başarılı bir token yanıt şöyle görünecektir:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fuser.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```

| Parametre     | Açıklama   |
|---------------|------------------------------|
| `access_token`  | İstenen erişim belirteci. Uygulama, web API'si gibi güvenli kaynağına kimliğini doğrulamak için bu belirteci kullanabilirsiniz.  |
| `token_type`    | Belirteç türü değeri gösterir. Azure AD destekleyen tek taşıyıcı türüdür |
| `expires_in`    | Ne kadar süreyle erişim belirteci (saniye olarak) geçerli değil. |
| `scope`         | Access_token için geçerli olan kapsamları. |
| `refresh_token` | OAuth 2.0 yenileme belirteci. Bu belirteç kullanabilecek geçerli erişim belirtecinin süresi dolduktan sonra ek erişim belirteçlerini almak. Refresh_tokens uzun süreli ve uzun süre için kaynaklarına erişimi korumak için kullanılabilir. Bir erişim belirteci yenileme ile ilgili daha fazla ayrıntı için bkz [bölümüne](#refresh-the-access-token). <br> **Not:** Yalnızca belirtilen if `offline_access` kapsam istendi. |
| `id_token`      | A JSON Web Token (JWT). Uygulama isteği açan kullanıcı hakkında bilgi için bu belirteci parçalarını çözebilen. Uygulama değerleri önbelleğe ve bunları görüntüleyebilirsiniz, ancak, bunlar üzerinde herhangi bir yetkilendirme veya güvenlik sınırları için doğrulamamalısınız. İd_tokens hakkında daha fazla bilgi için bkz: [ `id_token reference` ](id-tokens.md). <br> **Not:** Yalnızca belirtilen if `openid` kapsam istendi. |

### <a name="error-response"></a>Hata yanıtı

Hata yanıtları gibi görünür:

```json
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

| Parametre         | Açıklama    |
|-------------------|----------------|
| `error`       | Oluşan hataları türlerini sınıflandırmak için kullanılabilir ve hatalara tepki vermek için kullanılan bir hata kodu dizesi. |
| `error_description` | Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |
| `error_codes` | Tanılama'da yardımcı olabilecek özel STS hata kodlarının listesi.  |
| `timestamp`   | Hatanın gerçekleştiği zaman. |
| `trace_id`    | Tanılama'da yardımcı olabilecek talebi için benzersiz bir tanımlayıcı. |
| `correlation_id` | Tanılama'da bileşenlerinde yardımcı olabilecek isteği için benzersiz bir tanımlayıcı. |

### <a name="error-codes-for-token-endpoint-errors"></a>Belirteç uç noktası hataları için hata kodları

| Hata Kodu         | Açıklama        | İstemci eylemi    |
|--------------------|--------------------|------------------|
| `invalid_request`  | Gerekli parametre eksik gibi protokol hatası. | Düzeltin ve isteği yeniden gönderin   |
| `invalid_grant`    | PKCE kodunu doğrulama ve yetkilendirme kodu geçersiz veya süresi doldu. | Yeni bir istek deneyin `/authorize` uç nokta ve code_verifier parametresi doğru olduğunu doğrulayın.  |
| `unauthorized_client` | Kimliği doğrulanmış istemci bu yetkilendirme verme türünün kullanmaya yetkili değil. | Bu durum genellikle, istemci uygulamanın Azure AD'de kayıtlı değil veya kullanıcının Azure AD kiracısına eklenmez genellikle ortaya çıkar. Uygulama, uygulama yükleme ve Azure AD'ye ekleme yönerge kullanıcıyla isteyebilir. |
| `invalid_client` | İstemci kimlik doğrulaması başarısız oldu.  | İstemci kimlik bilgileri geçerli değil. Düzeltmek için uygulama yöneticisinin kimlik bilgilerini güncelleştirir.   |
| `unsupported_grant_type` | Yetkilendirme sunucusu yetkilendirme verme türünü desteklemiyor. | İstek verme türünü değiştirin. Bu tür yalnızca geliştirme sırasında gerçekleşmesi gerektiğini ve ilk test sırasında algılandı. |
| `invalid_resource` | Hedef kaynak yoksa, Azure AD, bulunamıyor veya düzgün yapılandırılmış olduğundan geçersiz. | Bu, varsa, kaynak kiracıda yapılandırılmadı gösterir. Uygulama, uygulama yükleme ve Azure AD'ye ekleme yönerge kullanıcıyla isteyebilir.  |
| `interaction_required` | İstek, kullanıcı etkileşimi gerektirir. Örneğin, bir ek kimlik doğrulama adım gereklidir. | Aynı kaynak isteği yeniden deneyin.  |
| `temporarily_unavailable` | Sunucunun geçici olarak isteği işleyemeyecek kadar meşgul. | İsteği yeniden deneyin. İstemci uygulama, kullanıcıya yanıtına bir geçici koşul nedeniyle ertelendi açıklayabilir. |

## <a name="use-the-access-token"></a>Erişim belirteci kullanın

Başarıyla alındı göre bir `access_token`, içine ekleyerek Web API'lerine isteklerinde belirteci kullanabilirsiniz `Authorization` üst bilgi:

> [!TIP]
> Postman içinde bu isteğin yürütülmesi! (Değiştir `Authorization` üstbilgi ilk) [ ![Postman içinde çalıştırın](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Erişim belirteci yenileyin

Access_tokens kısa süreli ve kaynaklara erişmeye devam etmek için süresi dolduktan sonra bunları yenilemeniz gerekir. Başka bir göndererek yapabilirsiniz `POST` isteği `/token` uç nokta, bu sefer sağlayan `refresh_token` yerine `code`.  Yenileme belirteçleri geçerlidir-istemcinizi zaten aldığı tüm izinleri onayı için bu nedenle, bir yenileme belirteci verilen bir talebi `scope=mail.read` için yeni bir erişim belirteci istemek için kullanılan `scope=api://contoso.com/api/UseResource`.  

Yenileme belirteçleri belirtilen ömre sahip değil. Genellikle, yenileme belirteçleri ömrü oldukça uzun olabilir. Ancak, bazı durumlarda, yenileme belirteçleri sona, iptal edilen veya istenen eylemi için yeterli ayrıcalıkları yok. Uygulamanızın beklediğiniz ve işlemek için gereken [belirteç yayınında uç noktası tarafından döndürülen hataları](#error-codes-for-token-endpoint-errors) doğru. 

Yeni erişim belirteçlerini almak için kullanıldığında, yenileme belirteçleri iptal olmayan olsa da, eski yenileme belirtecini iptal beklenir. [OAuth 2.0 belirtimi](https://tools.ietf.org/html/rfc6749#section-6) söylüyor: "Yetkilendirme sunucusu, istemci durumu AT gerekir yeni bir yenileme belirteci eski yenileme belirteci vermek ve yeni bir yenileme belirteci ile değiştirin. Yetkilendirme sunucusu eski yenileme belirteci istemciye yeni bir yenileme belirteci kestikten sonra iptal edebilirsiniz."  

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for web apps
```

> [!TIP]
> Postman içinde bu isteğin yürütülmesi deneyin! (Değiştirmeyi unutmayın `refresh_token`) [ ![Postman içinde çalıştırın](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)
> 

| Parametre     |                | Açıklama        |
|---------------|----------------|--------------------|
| `tenant`        | gerekli     | `{tenant}` İstek yolunda değer, uygulamaya oturum denetimi için kullanılabilir. İzin verilen değerler `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları. Daha fazla ayrıntı için [protokolü temel](active-directory-v2-protocols.md#endpoints).   |
| `client_id`     | gerekli    | **Uygulama (istemci) kimliği** , [Azure portalında – uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) uygulamanıza atanan deneyimi. |
| `grant_type`    | gerekli    | Olmalıdır `refresh_token` yetkilendirme kod akışı, bu oluşturan için. |
| `scope`         | gerekli    | Kapsamları boşlukla ayrılmış listesi. İçinde bu oluşturan istenen kapsamlar, eşdeğer veya özgün authorization_code isteği oluşturan içinde istenen kapsamları bir alt kümesi olmalıdır. Ardından bu istekte belirtilen kapsamlar birden çok kaynak sunucusu yayılıyorsa, Microsoft kimlik platformu uç nokta ilk kapsamında belirtilen kaynak için bir belirteç döndürür. Kapsamlar hakkında daha ayrıntılı açıklaması için başvurmak [izinler ve onay kapsamları](v2-permissions-and-consent.md). |
| `refresh_token` | gerekli    | Akışın ikinci oluşturan satın aldığınız refresh_token. |
| `client_secret` | Web apps için gerekli | Uygulama kayıt Portalı'nda uygulamanız için oluşturduğunuz uygulama gizli anahtarı. Client_secrets güvenilir bir şekilde cihazlarda depolanan olamaz çünkü yerel bir uygulamada kullanılmamalıdır. Web uygulamaları ve web client_secret güvenli bir şekilde sunucu tarafında depolama yeteneği olan API'leri için zorunludur. |

#### <a name="successful-response"></a>Başarılı yanıt

Başarılı bir token yanıt şöyle görünecektir:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fuser.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametre     | Açıklama         |
|---------------|-------------------------------------------------------------|
| `access_token`  | İstenen erişim belirteci. Uygulama, web API'si gibi güvenli kaynağına kimliğini doğrulamak için bu belirteci kullanabilirsiniz. |
| `token_type`    | Belirteç türü değeri gösterir. Azure AD destekleyen tek taşıyıcı türüdür |
| `expires_in`    | Ne kadar süreyle erişim belirteci (saniye olarak) geçerli değil.   |
| `scope`         | Access_token için geçerli olan kapsamları.    |
| `refresh_token` | Yeni bir OAuth 2.0 yenileme belirteci. Eski yenileme belirteci, yenileme belirteçleri için mümkün olan en kısa sürece geçerli kalır emin olmak için bu yeni alım yenileme belirteci ile değiştirmeniz gerekir. <br> **Not:** Yalnızca belirtilen if `offline_access` kapsam istendi.|
| `id_token`      | Bir işaretsiz JSON Web Token (JWT). Uygulama isteği açan kullanıcı hakkında bilgi için bu belirteci parçalarını çözebilen. Uygulama değerleri önbelleğe ve bunları görüntüleyebilirsiniz, ancak, bunlar üzerinde herhangi bir yetkilendirme veya güvenlik sınırları için doğrulamamalısınız. İd_tokens hakkında daha fazla bilgi için bkz: [ `id_token reference` ](id-tokens.md). <br> **Not:** Yalnızca belirtilen if `openid` kapsam istendi. |

#### <a name="error-response"></a>Hata yanıtı

```json
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

| Parametre         | Açıklama                                                                                        |
|-------------------|----------------------------------------------------------------------------------------------------|
| `error`           | Oluşan hataları türlerini sınıflandırmak için kullanılabilir ve hatalara tepki vermek için kullanılan bir hata kodu dizesi. |
| `error_description` | Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi.           |
| `error_codes` |Tanılama'da yardımcı olabilecek özel STS hata kodlarının listesi. |
| `timestamp` | Hatanın gerçekleştiği zaman. |
| `trace_id` | Tanılama'da yardımcı olabilecek talebi için benzersiz bir tanımlayıcı. |
| `correlation_id` | Tanılama'da bileşenlerinde yardımcı olabilecek isteği için benzersiz bir tanımlayıcı. |

Hata kodlarını ve önerilen istemci eylemi açıklaması için bkz: [belirteç uç noktası hataları için hata kodlarını](#error-codes-for-token-endpoint-errors).
