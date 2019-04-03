---
title: Azure AD'de Openıd Connect kimlik doğrulaması kod akışını anlayın | Microsoft Docs
description: Bu makalede, web uygulamalarına ve Azure Active Directory ve Openıd Connect kullanarak kiracınızda web API'lerine erişim yetkisi vermek için HTTP iletilerini kullanmayı açıklar.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 29142f7e-d862-4076-9a1a-ecae5bcd9d9b
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/4/2019
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1e39f271eaf0eccd0b3f3439492205e0d3398358
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851195"
---
# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>Openıd Connect ile Azure Active Directory kullanarak web uygulamalarına erişim yetkisi verme

[Openıd Connect](https://openid.net/specs/openid-connect-core-1_0.html) olan OAuth 2.0 protokolünü üzerinde oluşturulan Basit kimlik katmanı. OAuth 2.0 tanımlar ve mekanizmaları [ **erişim belirteçlerini** ](access-tokens.md) erişmek için korumalı kaynaklara, ancak bunlar tanımlamıyor kimlik bilgilerini sağlamak için standart yöntemleri. Openıd Connect kimlik doğrulaması için OAuth 2.0 Yetkilendirme işlemi bir genişletme olarak uygular. Biçiminde son kullanıcı hakkında bilgi sağlayan bir [ `id_token` ](id-tokens.md) , kullanıcının kimliğini doğrular ve kullanıcının temel profil bilgilerini sağlar.

Openıd Connect, bir sunucuda barındırılan ve tarayıcı erişilen bir web uygulaması oluşturuyorsanız, bizim kullanılması önerilir.


[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)] 

## <a name="authentication-flow-using-openid-connect"></a>Openıd Connect'i kullanarak kimlik doğrulama akışı

En temel oturum açma akışını aşağıdaki adımları içerir: her biri aşağıda ayrıntılı olarak açıklanmıştır.

![Openıd Connect kimlik doğrulama akışı](./media/v1-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)

## <a name="openid-connect-metadata-document"></a>OpenID Connect meta veri belgesi

Openıd Connect oturum açma gerçekleştirmek bir uygulama için gerekli olan ilgili bilgilerin çoğunu içeren bir meta veri belgesinin açıklar. Bu, hizmetin ortak İmzalama anahtarları konumunu ve kullanılacak URL'leri gibi bilgileri içerir. Openıd Connect meta veri belgesi bulabilirsiniz:

```
https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration
```
Meta veriler bir basit bir JavaScript nesne gösterimi (JSON) belgedir. Aşağıdaki kod parçacığını bir örnek için bkz. İş parçacığının içeriği tamamen açıklanan [Openıd Connect belirtimi](https://openid.net). Sağlama, Kiracı Not yerine `common` yerinde {tenant}, yukarıdaki kiracıya özgü URI'ler döndürülen JSON nesnesinde neden olur.

```
{
    "authorization_endpoint": "https://login.microsoftonline.com/common/oauth2/authorize",
    "token_endpoint": "https://login.microsoftonline.com/common/oauth2/token",
    "token_endpoint_auth_methods_supported":
    [
        "client_secret_post",
        "private_key_jwt",
        "client_secret_basic"
    ],
    "jwks_uri": "https://login.microsoftonline.com/common/discovery/keys"
    "userinfo_endpoint":"https://login.microsoftonline.com/{tenant}/openid/userinfo",
    ...
}
```

## <a name="send-the-sign-in-request"></a>Oturum açma isteği gönder

Web uygulamanız kullanıcı kimlik doğrulaması gerektiğinde, kullanıcıya yönlendirmelisiniz `/authorize` uç noktası. Bu istek için ilk oluşturan benzer [OAuth 2.0 yetkilendirme kod akış](v1-protocols-oauth-code.md), bazı önemli farklılıklar ile:

* İstek kapsamı içermelidir `openid` içinde `scope` parametresi.
* `response_type` Parametre içermelidir `id_token`.
* İstek içermelidir `nonce` parametresi.

Bu nedenle örnek istek şöyle görünebilir:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%3a12345
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| kiracı |gerekli |`{tenant}` İstek yolunda değer, uygulamaya oturum denetimi için kullanılabilir. Kiracı tanımlayıcıları, örneğin, izin verilen değerler: `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` veya `contoso.onmicrosoft.com` veya `common` Kiracı bağımsız belirteçleri |
| client_id |gerekli |Azure AD ile kaydettiğinizde, uygulamanıza atanan uygulama kimliği. Bunu Azure Portal'da bulabilirsiniz. Tıklayın **Azure Active Directory**, tıklayın **uygulama kayıtları**, uygulamayı seçin ve uygulama sayfasında uygulama kimliğini bulun. |
| response_type |gerekli |İçermelidir `id_token` Openıd Connect oturum açma için. Diğer response_types gibi de dahil `code` veya `token`. |
| scope | Önerilen | Openıd Connect belirtimi kapsamı gerektirir `openid`, onay için UI "oturumunuzu açma" izni çevirir. Bu ve diğer OIDC kapsamları v1.0 uç noktada göz ardı edilir, ancak yine de standartlarıyla uyumlu istemciler için en iyi uygulama. |
| nonce |gerekli |Ortaya çıkan dahil olan ve uygulama tarafından oluşturulan isteğinde bulunan bir değer `id_token` talep olarak. Uygulama, belirteç yeniden yürütme saldırıları azaltmak için bu değer daha sonra doğrulayabilirsiniz. Genellikle bir rastgele, benzersiz bir dize veya isteğinin kaynağı tanımlamak için kullanılan GUID değeridir. |
| redirect_uri | Önerilen |Burada kimlik doğrulama yanıtlarının gönderilebilen veya uygulamanız tarafından alınan uygulamanızın redirect_uri. Bu url olarak kodlanmış olması dışında Portalı'nda kayıtlı redirect_uris biri tam olarak eşleşmesi gerekir. Eksikse, kullanıcı aracısı geri bir URI'leri rastgele bir uygulama için kayıtlı yeniden yönlendirme gönderilir. En fazla uzunluk 255 bayttır |
| response_mode |isteğe bağlı |Sonuçta elde edilen authorization_code uygulamanıza geri göndermek için kullanılması gereken yöntemini belirtir. Desteklenen değerler şunlardır: `form_post` için *HTTP form post* ve `fragment` için *URL parçası belirtmesini*. Web uygulamaları için kullanılması önerilir `response_mode=form_post` belirteçleri, uygulamanızın en güvenli aktarımı emin olmak için. Bir id_token dahil olmak üzere herhangi bir akış için varsayılan değer `fragment`.|
| durum |Önerilen |Belirteç yanıtta döndürülen isteğinde bulunan bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Rastgele oluşturulmuş bir benzersiz değer için genellikle kullanılan [siteler arası istek sahteciliğini saldırılarını önleme](https://tools.ietf.org/html/rfc6749#section-10.12). Durum, uygulama kullanıcının durumu hakkındaki bilgileri sayfasında ya da görünümü üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için de kullanılır. |
| istemi |isteğe bağlı |Gerekli olan kullanıcı etkileşimi türünü belirtir. Şu anda, geçerli değerler yalnızca 'login', 'none' olan ve 'onay'. `prompt=login` Bu isteğin negating çoklu oturum açma kimlik bilgilerini girmesini zorlar. `prompt=none` -tersidir kullanıcı hiçbir etkileşimli istemi olmadan sunulmayan sağlar. İstek sessiz bir şekilde çoklu oturum açma aracılığıyla tamamlanamıyorsa, uç nokta bir hata döndürür. `prompt=consent` Kullanıcı uygulamayı izinler istendiği anın, oturum açtıktan sonra iletişim Tetikleyicileri OAuth onay vermiş olursunuz. |
| login_hint |isteğe bağlı |Önceden, kullanıcı adını biliyorsanız, kullanıcı için oturum açma sayfası kullanıcı adı/e-posta adresi alanının önceden doldurmak için kullanılabilir. Uygulamalar genellikle kullanıcı adı önceki oturum açma kullanarak bir zaten ayıklanan yeniden kimlik doğrulaması sırasında bu parametreyi kullanın `preferred_username` talep. |

Bu noktada, kullanıcı kimlik bilgilerini girin ve kimlik doğrulamasını tamamlamak istenir.

### <a name="sample-response"></a>Örnek yanıt

Kullanıcı kimliğini doğrulamasından sonra bir örnek yanıt şöyle görünebilir:

```
POST / HTTP/1.1
Host: localhost:12345
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| id_token |`id_token` İstenen uygulama. Kullanabileceğiniz `id_token` kullanıcının kimliğini doğrulamak ve kullanıcı bir oturumla başlayın. |
| durum |Ayrıca belirteci yanıtta döndürülen isteğinde bulunan bir değer. Rastgele oluşturulmuş bir benzersiz değer için genellikle kullanılan [siteler arası istek sahteciliğini saldırılarını önleme](https://tools.ietf.org/html/rfc6749#section-10.12). Durum, uygulama kullanıcının durumu hakkındaki bilgileri sayfasında ya da görünümü üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için de kullanılır. |

### <a name="error-response"></a>Hata yanıtı

Hata yanıtları da gönderilebilir için `redirect_uri` uygulama bunları uygun şekilde işleyebilmesi için:

```
POST / HTTP/1.1
Host: localhost:12345
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanılabilir ve hatalara tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Yetkilendirme uç noktası hataları için hata kodları

Aşağıdaki tabloda, döndürülen çeşitli hata kodları açıklanmaktadır `error` hata yanıtı parametresi.

| Hata Kodu | Açıklama | İstemci eylemi |
| --- | --- | --- |
| invalid_request |Gerekli parametre eksik gibi protokol hatası. |Düzeltin ve isteği yeniden gönderin. Bu geliştirme hata ve genellikle ilk test sırasında yakalandı. |
| unauthorized_client |İstemci uygulama bir yetkilendirme kodu istek izin verilmiyor. |Bu durum genellikle, istemci uygulaması, Azure AD'de kayıtlı değil veya kullanıcının Azure AD kiracısına eklenmez genellikle ortaya çıkar. Uygulama, uygulama yükleme ve Azure AD'ye ekleme yönerge kullanıcıyla isteyebilir. |
| access_denied |Kaynak sahibi onayı reddedildi |İstemci uygulama, kullanıcı onay sürece devam edemiyor kullanıcıya bildirebilir. |
| unsupported_response_type |Yetkilendirme sunucusu isteği yanıt türü desteklemiyor. |Düzeltin ve isteği yeniden gönderin. Bu geliştirme hata ve genellikle ilk test sırasında yakalandı. |
| server_error |Sunucu beklenmeyen bir hatayla karşılaştı. |İsteği yeniden deneyin. Bu hataların geçici koşullar neden olabilir. İstemci uygulama, kullanıcıya yanıtına geçici bir hata nedeniyle ertelendi açıklayabilir. |
| temporarily_unavailable |Sunucunun geçici olarak isteği işleyemeyecek kadar meşgul. |İsteği yeniden deneyin. İstemci uygulama, kullanıcıya, yanıtına bir geçici koşul nedeniyle ertelendi açıklayabilir. |
| invalid_resource |Hedef kaynak yoksa, Azure AD, bulunamıyor veya doğru şekilde yapılandırılmadığı için geçersiz. |Bu, varsa, kaynak kiracıda yapılandırılmadı gösterir. Uygulama, uygulama yükleme ve Azure AD'ye ekleme yönerge kullanıcıyla isteyebilir. |

## <a name="validate-the-idtoken"></a>İd_token doğrula

Alma yalnızca bir `id_token` ; kullanıcının kimliğini doğrulamak yeterli değil imzayı doğrulamak ve talepleri doğrulamak `id_token` uygulamanızın gereksinimlerini başına. Azure AD uç noktası, geçerli olduğunu doğrulayın ve Belirteçleri imzalamak için JSON Web belirteçleri (Jwt'ler) ve ortak anahtar şifrelemesi kullanır.

Doğrulamak seçebileceğiniz `id_token` istemci kodu, ancak yaygın bir uygulama olan göndermek için `id_token` bir arka uç sunucusuna ve orada doğrulama gerçekleştirin. 

Senaryonuza bağlı olarak ek istekleri doğrulamak isteyebilirsiniz. Bazı ortak doğrulamaları şunlardır:

* Kullanıcı/kuruluş sağlama uygulama için kaydolmuş.
* Kullanıcı sağlama kullanarak uygun yetkilendirme/ayrıcalıklarına sahip `wids` veya `roles` talep. 
* Belirli bir kimlik doğrulama gücü sağlamak, çok faktörlü kimlik doğrulaması gibi oluştu.

Doğrulandıktan sonra `id_token`, kullanıcı bir oturumla başlar ve talepleri kullanan `id_token` uygulamanızda kullanıcı hakkında bilgi edinmek için. Bu bilgiler kullanılabilir görüntüleme, kayıtları, kişiselleştirme, vb. için. Hakkında daha fazla bilgi için `id_tokens` ve talepler, okuma [AAD id_tokens](id-tokens.md).

## <a name="send-a-sign-out-request"></a>Oturum kapatma isteği gönder

Uygulama dışında kullanıcı oturum açmak istediğinizde, uygulamanızın tanımlama ya da aksi takdirde son kullanıcı ile oturum temizlemek yeterli değil. Ayrıca kullanıcıya yönlendirmelisiniz `end_session_endpoint` için oturum kapatma. Bunu yapmak başarısız olursa, kullanıcı kimlik bilgilerini yeniden girmeye gerek kalmadan uygulamanıza geçerli tek bir oturum açma oturumu ile Azure AD uç noktası olacağından sağlamalarını mümkün olacaktır.

Yalnızca kullanıcıya yönlendirebilirsiniz `end_session_endpoint` Openıd Connect meta veri belgesinde listelenen:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parametre |  | Açıklama |
| --- | --- | --- |
| post_logout_redirect_uri |Önerilen |Kullanıcı başarılı oturum kapatma yönlendirilmesi gereken URL. Dahil edilmezse, kullanıcının genel bir ileti gösterilir. |

## <a name="single-sign-out"></a>Çoklu oturum kapatma

Ne zaman yeniden yönlendirmek için kullanıcı `end_session_endpoint`, Azure AD, kullanıcının oturumunu bir tarayıcıdan temizler. Ancak, kullanıcı kimlik doğrulaması için Azure AD'yi kullanan diğer uygulamalar için açtığınız oturuma devam. Kullanıcı aynı anda oturumu kapatmak bu uygulamaları etkinleştirmek için Azure AD bir HTTP GET isteği kayıtlı gönderir `LogoutUrl` kullanıcı şu anda oturum açmış tüm uygulamalar. Kullanıcıyı tanımlayan hiçbir oturum temizlemek ve döndürme, uygulamalar bu istek için yanıt gereken bir `200` yanıt. Çoklu oturum kapatma uygulamanızda desteklemek istiyorsanız, örneğin uygulamalıdır bir `LogoutUrl` uygulamanızın kod. Ayarlayabileceğiniz `LogoutUrl` Azure portalından:

1. Gidin [Azure portalında](https://portal.azure.com).
2. Active Directory, sayfanın sağ üst köşesinde hesabınıza tıklayarak seçin.
3. Sol taraftaki gezinti panelinden seçin **Azure Active Directory**, ardından **uygulama kayıtları** ve uygulamanızı seçin.
4. Tıklayarak **ayarları**, ardından **özellikleri** ve bulma **oturum kapatma URL'si** metin kutusu. 

## <a name="token-acquisition"></a>Belirteç edinme
Birçok web uygulamaları, yalnızca kullanıcının oturum açmasını, ancak aynı zamanda bir web hizmeti OAuth kullanarak bu kullanıcı adına erişim gerekir. Aynı anda alma sırasında kullanıcı kimlik doğrulaması için Openıd Connect bu senaryo birleştiren bir `authorization_code` almak için kullanılabilecek `access_tokens` kullanarak [OAuth yetkilendirme kod akış](v1-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token).

## <a name="get-access-tokens"></a>Erişim belirteci alma
Erişim belirteçlerini almak için oturum açma isteği yukarıdaki değiştirmeniz gerekir:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%3a12345          // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // `form_post' or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F        // The identifier of the protected resource (web API) that your application needs access to
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

İstekte izin kapsamları dahil olmak üzere ve kullanarak `response_type=code+id_token`, `authorize` uç noktası, kullanıcı için belirtilen izinleri onaylamasını sağlar `scope` sorgu parametresi ve uygulamanız için Exchange bir yetkilendirme kodu döndürür bir erişim belirteci.

### <a name="successful-response"></a>Başarılı yanıt

Başarılı yanıt kullanarak bir `response_mode=form_post` gibi görünür:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| id_token |`id_token` İstenen uygulama. Kullanabileceğiniz `id_token` kullanıcının kimliğini doğrulamak ve kullanıcı bir oturumla başlayın. |
| kod |Uygulama istenen authorization_code. Uygulama, hedef kaynağı için bir erişim belirteci istemek için yetkilendirme kodu kullanabilirsiniz. Authorization_codes kısa süreli ve genellikle yaklaşık 10 dakika sonra süresi dolar. |
| durum |State parametresi istekte yer alıyorsa aynı değeri yanıt olarak görünmelidir. Uygulama istek ve yanıt durum değerleri özdeş olduğunu doğrulamanız gerekir. |

### <a name="error-response"></a>Hata yanıtı

Hata yanıtları da gönderilebilir için `redirect_uri` uygulama bunları uygun şekilde işleyebilmesi için:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanılabilir ve hatalara tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

Olası hata kodları ve bunların önerilen istemci eylemi açıklaması için bkz: [yetkilendirme uç noktası hataları için hata kodlarını](#error-codes-for-authorization-endpoint-errors).

Bir yetkilendirme yönettiniz sonra `code` ve `id_token`, kullanıcı oturum açabilir ve alma [erişim belirteçlerini](access-tokens.md) gerçekleştirilemeyeceğine ilişkin. Kullanıcının oturum açmak için doğrulamalıdır `id_token` tam olarak yukarıda açıklandığı gibi. Erişim belirteçlerini almak için "bir erişim belirteci istemek için yetkilendirme kodu kullan" bölümünde açıklanan adımları takip edebilirsiniz bizim [OAuth kod flow belgeleri](v1-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token).

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [erişim belirteçlerini](access-tokens.md).
* Daha fazla bilgi edinin [ `id_token` ve talepleri](id-tokens.md).
