---
title: Azure AD'de Openıd Connect kimlik doğrulama kodu akışını anlamak | Microsoft Docs
description: Bu makalede, web uygulamaları ve web API kullanarak Azure Active Directory ve Openıd Connect kiracınızda erişim yetkisi vermek için HTTP iletileri kullanmayı açıklar.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 29142f7e-d862-4076-9a1a-ecae5bcd9d9b
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: a5383776aa787a087fffe1ab06bb62c2b1df073d
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>Openıd Connect ve Azure Active Directory kullanarak web uygulamalarına erişim yetkisi
[Openıd Connect](http://openid.net/specs/openid-connect-core-1_0.html) olan OAuth 2.0 protokolünü en üstünde oluşturulan Basit kimlik katmanı. OAuth 2.0 tanımlar edinme ve kullanma mekanizmaları **erişim belirteçleri** erişmek için korumalı kaynaklara, ancak bunlar tanımlamaz kimlik bilgilerini sağlamak için standart yöntemleri. Openıd Connect, OAuth 2.0 Yetkilendirme işlemi için bir uzantısı olarak kimlik doğrulaması gerçekleştirir. Biçiminde son kullanıcı hakkında bilgi sağlayan bir `id_token` , kullanıcının kimliğini doğrular ve kullanıcının temel profil bilgilerini sağlar.

Bir sunucuda barındırılan ve tarayıcı erişilen bir web uygulaması oluşturuyorsanız Openıd Connect bizim önerilir.


[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)] 

## <a name="authentication-flow-using-openid-connect"></a>Openıd Connect kullanarak kimlik doğrulama akışı
En basit oturum açma akışını aşağıdaki adımları içerir - bunların her biri aşağıda ayrıntılı olarak açıklanmıştır.

![Openıd Connect kimlik doğrulama akışı](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)

## <a name="openid-connect-metadata-document"></a>Openıd Connect meta veri belgesi

Openıd Connect oturum açma gerçekleştirmek bir uygulama için gerekli bilgileri çoğunu içeren bir meta veri belgesi açıklar. Bu, kullanılacak URL'ler gibi bilgileri ve hizmetin genel İmzalama anahtarları konumunu içerir. Openıd Connect meta veri belgesi yolda bulunabilir:

```
https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration
```
Basit bir JavaScript nesne gösterimi (JSON) belge meta verilerdir. Aşağıdaki kod parçacığını bir örnek için bkz. Kod parçacığını'nin içeriği tam olarak açıklanan [Openıd Connect belirtimi](https://openid.net).

```
{
    "authorization_endpoint": "https://login.microsoftonline.com/common/oauth2/authorize",
    "token_endpoint": "https://login.microsoftonline.com/common/oauth2/token",
    "token_endpoint_auth_methods_supported":
    [
        "client_secret_post",
        "private_key_jwt"
    ],
    "jwks_uri": "https://login.microsoftonline.com/common/discovery/keys"
    
    ...
}
```

## <a name="send-the-sign-in-request"></a>Oturum açma isteği gönder
Kullanıcının kimliğini doğrulamak, web uygulamanız gerektiğinde kullanıcıya doğrudan gerekir `/authorize` uç noktası. Bu istek için ilk Bacak için benzer [OAuth 2.0 yetkilendirme kodu akışı](active-directory-protocols-oauth-code.md), birkaç önemli farklılıklar ile:

* İstek kapsamı içermelidir `openid` içinde `scope` parametresi.
* `response_type` Parametre içermelidir `id_token`.
* İstek içermelidir `nonce` parametresi.

Bu nedenle örnek istek şöyle olabilir:

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
| kiracı |gerekli |`{tenant}` İstek yolunu değerinde uygulamasına oturum denetlemek için kullanılabilir. İzin verilen değerler Kiracı, örneğin, tanımlayıcılardır `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` veya `contoso.onmicrosoft.com` veya `common` Kiracı bağımsız belirteçleri |
| client_id |gerekli |Azure AD ile kaydolurken uygulamanıza atanan uygulama kimliği. Bu Azure Portalı'nda bulabilirsiniz. Tıklatın **Azure Active Directory**, tıklatın **uygulama kayıtlar**, uygulamayı seçin ve uygulama sayfasında uygulama kimliğini bulun. |
| response_type |gerekli |İçermelidir `id_token` Openıd Connect oturum açma için. Bu ayrıca diğer response_types gibi içerebilir `code`. |
| scope |gerekli |Kapsamları boşlukla ayrılmış listesi. Openıd Connect için kapsam içermesi gerekir `openid`, UI onay "oturum" izni çevirir. Bu isteği onay isteyen diğer kapsamları ekleyebiliriz. |
| nonce |gerekli |Elde edilen içinde bulunan uygulama tarafından üretilen istekteki dahil bir değer `id_token` bir talep olarak. Uygulama sonra belirteç yeniden yürütme saldırıları azaltmak için bu değeri doğrulayabilirsiniz. Tipik olarak bir rastgele, benzersiz dize veya istek kaynağını tanımlamak için kullanılan GUID değeridir. |
| redirect_uri |Önerilen |Burada kimlik doğrulama yanıtları gönderilebilen veya uygulamanız tarafından alınan, uygulamanızın redirect_uri. Bu tam bir url kodlanmış olmalıdır dışında Portalı'nda kayıtlı redirect_uris eşleşmelidir. |
| response_mode |Önerilen |Sonuçta elde edilen authorization_code uygulamanıza geri göndermek için kullanılacak yöntemi belirtir. Desteklenen değerler `form_post` için *HTTP form post* ve `fragment` için *URL parçası*. Web uygulamaları için kullanılmasını öneririz `response_mode=form_post` uygulamanız en güvenli aktarımını belirteçleri sağlamak için. Varsayılan olarak, `response_mode` dahil değildir, olan `fragment`.|
| durum |Önerilen |Belirteç yanıtta döndürülen istek dahil bir değer. İstediğiniz herhangi bir içerik dizesi olabilir. Rastgele oluşturulan benzersiz bir değer tipik olarak kullanılan [siteler arası istek sahteciliğini saldırılarını önleme](http://tools.ietf.org/html/rfc6749#section-10.12). Durumu, sayfa veya görünüm üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce uygulama kullanıcının durumu hakkındaki bilgileri kodlamak için de kullanılır. |
| istemi |isteğe bağlı |Gerekli bir kullanıcı etkileşimi türünü belirtir. Şu anda, geçerli değerler yalnızca 'oturum açma', 'none' olan ve 'onay'. `prompt=login` Bu isteğin negating çoklu oturum açma kimlik bilgilerini girmesini zorlar. `prompt=none` - tersidir kullanıcı hiçbir etkileşimli istemi doğabilecek sunulmayan sağlar. İstek sessizce çoklu oturum açma aracılığıyla tamamlanamazsa, uç nokta bir hata döndürür. `prompt=consent` Uygulama izinleri vermek için kullanıcı isteyen kullanıcı, oturum sonra Tetikleyicileri OAuth iletişim olursunuz. |
| login_hint |isteğe bağlı |Kullanıcı adlarını önceden biliyorsanız, kullanıcı için oturum açma sayfası kullanıcı adı/e-posta adresi alanının önceden doldurmak için kullanılabilir. Genellikle uygulamaları yeniden kimlik doğrulaması, kullanıcı adı önceki oturum açma kullanarak bir zaten ayıklanan sırasında bu parametreyi kullanın `preferred_username` talep. |

Bu noktada, kullanıcı kimlik bilgilerini girin ve kimlik doğrulamasını tamamlamak istenir.

### <a name="sample-response"></a>Örnek yanıt
Kullanıcı kimliğini doğrulamasından sonra bir örnek yanıt şu şekilde görünebilir:

```
POST / HTTP/1.1
Host: localhost:12345
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| id_token |`id_token` Uygulama istenen. Kullanabileceğiniz `id_token` kullanıcının kimliğini doğrulamak ve kullanıcı oturumu başlatmak için. |
| durum |Ayrıca belirteci yanıtta döndürülen istek dahil bir değer. Rastgele oluşturulan benzersiz bir değer tipik olarak kullanılan [siteler arası istek sahteciliğini saldırılarını önleme](http://tools.ietf.org/html/rfc6749#section-10.12). Durumu, sayfa veya görünüm üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce uygulama kullanıcının durumu hakkındaki bilgileri kodlamak için de kullanılır. |

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
| error |Oluşan hataları türlerini sınıflandırmak için kullanılan ve hataları tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Yetkilendirme uç noktası hataları için hata kodları
Aşağıdaki tabloda, döndürülen çeşitli hata kodları açıklanmaktadır `error` hata yanıtı parametresi.

| Hata Kodu | Açıklama | İstemci eylemi |
| --- | --- | --- |
| invalid_request |Eksik gerekli parametre gibi protokol hatası. |Düzeltin ve isteği yeniden gönderin. Bu geliştirme hata ve genellikle ilk test sırasında yakalandı. |
| unauthorized_client |İstemci uygulaması bir kimlik doğrulama kodu isteme izni yok. |Bu durum genellikle, istemci uygulaması Azure AD'de kayıtlı değil veya kullanıcının Azure AD kiracısı eklenmez genellikle ortaya çıkar. Uygulama yönerge Azure AD'ye ekleme ve uygulama yükleme için kullanıcıya sor |
| ACCESS_DENIED |Kaynak sahibi izin reddedildi |İstemci uygulaması kullanıcı izin sürece devam edemiyor kullanıcı bildirebilir. |
| unsupported_response_type |Yetkilendirme sunucusu isteğine yanıt türünü desteklemiyor. |Düzeltin ve isteği yeniden gönderin. Bu geliştirme hata ve genellikle ilk test sırasında yakalandı. |
| server_error |Sunucu beklenmeyen bir hatayla karşılaştı. |İsteği yeniden deneyin. Bu hatalar geçici koşulları neden olabilir. İstemci uygulaması yanıt geçici bir hata nedeniyle gecikti kullanıcıya açıklayabilir. |
| temporarily_unavailable |Sunucunun geçici olarak istek işlemek için çok meşgul. |İsteği yeniden deneyin. İstemci uygulaması yanıt geçici bir koşul nedeniyle gecikti kullanıcıya açıklayabilir. |
| invalid_resource |Hedef kaynak mevcut değil, Azure AD, bulunamıyor veya düzgün yapılandırılmamış olduğundan geçerli değil. |Bu, varsa, kaynak kiracısında yapılandırılmadı gösterir. Uygulama yönerge Azure AD'ye ekleme ve uygulama yükleme için kullanıcıya sor |

## <a name="validate-the-idtoken"></a>İd_token doğrula
Yalnızca alma bir `id_token` ; kullanıcının kimliğini doğrulamak yeterli değil imzayı doğrulamak ve talepleri doğrulayın `id_token` uygulamanızın gereksinimleri başına. Azure AD endpoint Belirteçleri imzalamak ve bunların geçerli olduğunu doğrulamak için JSON Web belirteçleri (Jwt'ler) ve ortak anahtar şifrelemesi kullanır.

Doğrulamayı seçebilirsiniz `id_token` istemci kodu ancak ortak bir uygulama olan göndermek için `id_token` bir arka uç sunucusuna ve doğrulama var. gerçekleştirin. İmzası doğruladıktan sonra `id_token`, doğrulamak için gerekli birkaç talep vardır.

Senaryonuza bağlı olarak ek talep doğrulamak isteyebilir. Bazı ortak doğrulamaları şunları içerir:

* Kullanıcı/kuruluş sağlama uygulama için kaydolmuş.
* Uygun yetkilendirme/ayrıcalıklara sahip kullanıcı sağlama
* Belirli bir kimlik doğrulama gücünü sağlayarak, çok faktörlü kimlik doğrulaması gibi oluştu.

Doğruladıktan sonra `id_token`, kullanıcı bir oturumla başlar ve talepleri kullanan `id_token` uygulamanızda kullanıcı hakkında bilgi edinmek için. Bu bilgiler kullanılabilir ekran kayıtları, yetkilerini, vb. için. Talep ve belirteç türleri hakkında daha fazla bilgi için okuma [desteklenen belirteç ve talep türleri](active-directory-token-and-claims.md).

## <a name="send-a-sign-out-request"></a>Oturum kapatma isteği gönder
Uygulama dışında kullanıcı oturum istediklerinde, uygulamanızın tanımlama bilgilerini veya aksi takdirde son kullanıcı oturumuyla temizlemek yeterli değil. Kullanıcıya yönlendirme gerekir `end_session_endpoint` için oturum kapatma. Bunu yapmak başarısız olursa, kullanıcı kimlik bilgilerini yeniden girmeye gerek kalmadan, uygulamanızın geçerli tek bir oturum açma oturumu Azure AD uç noktası ile olacağı sağlamalarını kuramaz.

Kullanıcıya yalnızca yönlendirebilirsiniz `end_session_endpoint` Openıd Connect meta veri belgesinde listelenen:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parametre |  | Açıklama |
| --- | --- | --- |
| post_logout_redirect_uri |Önerilen |Kullanıcı başarıyla oturum kapatma sonra yönlendirilmesi gereken URL'si. Dahil edilmezse, kullanıcı genel bir ileti gösterilir. |

## <a name="single-sign-out"></a>Çoklu oturum kapatma
Ne zaman yeniden yönlendirmek için kullanıcı `end_session_endpoint`, Azure AD, kullanıcının oturumunu tarayıcıdan temizler. Ancak, kullanıcı hala Azure AD için kimlik doğrulama kullanan diğer uygulamalar için oturum açmanız. Kullanıcı aynı anda oturumu kapatmak bu uygulamalara olanak tanımak için Azure AD bir HTTP GET isteği kayıtlı gönderir `LogoutUrl` kullanıcı şu anda oturum tüm uygulamaların. Uygulamalar kullanıcı tanımlayan herhangi bir oturumunda temizlenmesi ve döndürme bu istek için yanıt gerekir bir `200` yanıt. Çoklu oturum kapatma uygulamanızda desteklemek istiyorsanız, gibi uygulamalıdır bir `LogoutUrl` uygulamanızın kodu. Ayarlayabileceğiniz `LogoutUrl` Azure portalından:

1. Gidin [Azure Portal](https://portal.azure.com).
2. Active Directory, sayfanın sağ üst köşesinde hesabınızda tıklayarak seçin.
3. Sol taraftaki gezinti panelinden seçin **Azure Active Directory**, ardından **uygulama kayıtlar** ve uygulamanızı seçin.
4. Tıklayın **ayarları**, ardından **özellikleri** ve Bul **oturum kapatma URL'si** metin kutusu. 

## <a name="token-acquisition"></a>Belirteç edinme
Birçok web uygulamaları yalnızca kullanıcı oturum açabilir, ancak ayrıca OAuth kullanarak bu kullanıcı adına bir web hizmetine erişim gerekir. Bu senaryo Openıd Connect aynı anda alınırken hatayla kullanıcı kimlik doğrulaması için bir araya getirir bir `authorization_code` almak için kullanılabilir `access_tokens` kullanarak [OAuth yetkilendirme kod akış](active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token).

## <a name="get-access-tokens"></a>Erişim belirteci alın
Erişim belirteci almak için oturum açma isteği üstten değiştirmeniz gerekir:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%3a12345          // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // `form_post' or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                     
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

İstekte izin kapsamları dahil olmak üzere ve kullanarak `response_type=code+id_token`, `authorize` uç nokta sağlar kullanıcı belirtilen izinleri seçtiği `scope` sorgu parametresi ve uygulamanız için bir erişim belirteci gönderip almak için bir kimlik doğrulama kodu döndürür.

### <a name="successful-response"></a>Başarılı yanıt
Başarılı yanıt kullanarak `response_mode=form_post` gibi görünüyor:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| id_token |`id_token` Uygulama istenen. Kullanabileceğiniz `id_token` kullanıcının kimliğini doğrulamak ve kullanıcı oturumu başlatmak için. |
| Kod |Uygulama istenen authorization_code. Uygulama, hedef kaynak için bir erişim belirteci istemek için yetkilendirme kodu kullanabilirsiniz. Authorization_codes kısa süreli ve genellikle yaklaşık 10 dakika sonra süresi dolacak. |
| durum |İstekte bir durum parametresi eklenirse, aynı değeri yanıt olarak görünmelidir. Uygulama istek ve yanıt durum değerleri özdeş olduğunu doğrulamanız gerekir. |

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
| error |Oluşan hataları türlerini sınıflandırmak için kullanılan ve hataları tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

Olası hata kodları ve bunların önerilen istemci eylemi açıklaması için bkz: [yetkilendirme uç noktası hataları için hata kodları](#error-codes-for-authorization-endpoint-errors).

Bir yetkilendirme kabulünüzü sonra `code` ve bir `id_token`, kullanıcı oturum açabilir ve onların adına erişim belirteci alın. Kullanıcıyla oturum açmak için doğrulamalısınız `id_token` tam olarak yukarıda açıklandığı gibi. Erişim belirteci almak için "bir erişim belirteci istemek için yetkilendirme kodu kullan" bölümünde açıklanan adımları izleyebilirsiniz bizim [OAuth Protokolü belgeleri](active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token).
