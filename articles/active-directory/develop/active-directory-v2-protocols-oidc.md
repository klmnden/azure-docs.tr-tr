---
title: Azure Active Directory v2.0 ve Openıd Connect Protokolü | Microsoft Docs
description: Web uygulamaları Azure AD v2.0 uygulama Openıd Connect kimlik doğrulama protokolü kullanarak oluşturun.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: a4875997-3aac-4e4c-b7fe-2b4b829151ce
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: a0cd077b1c6530c5794c92f131dffb814f5b341d
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34157726"
---
# <a name="azure-active-directory-v20-and-the-openid-connect-protocol"></a>Azure Active Directory v2.0 ve Openıd Connect Protokolü
Openıd Connect OAuth 2.0, bir kullanıcı için bir web uygulaması için güvenli bir şekilde imzalamak için kullanabileceğiniz yerleşik bir kimlik doğrulama protokolüdür. Openıd Connect v2.0 uç noktanın uygulamasını kullandığınızda, web tabanlı uygulamalar için oturum açma ve API erişimini ekleyebilirsiniz. Bu makalede, bu bağımsız dilinin nasıl gösteriyoruz. Size hiçbir Microsoft açık kaynak kitaplıkları kullanmadan HTTP iletileri almasına ve göndermesine açıklanır.

> [!NOTE]
> V2.0 uç noktası, tüm Azure Active Directory senaryolarını ve özelliklerini desteklemez. V2.0 uç kullanması gerekip gerekmediğini belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
> 
> 

[Openıd Connect](http://openid.net/specs/openid-connect-core-1_0.html) OAuth 2.0 genişletir *yetkilendirme* protokolü olarak kullanılmak üzere bir *kimlik doğrulaması* tek gerçekleştirebilmeleri için protokol OAuth kullanılarak oturum. Openıd Connect kavramı kullanılmaya başlanmıştır bir *kimliği belirteci*, kullanıcının kimliğini doğrulamak istemci izin veren bir güvenlik belirteci değil. Kimliği belirteci ayrıca kullanıcının temel profil bilgilerini alır. Openıd Connect OAuth 2.0 genişlettiğinden uygulamaları güvenli bir şekilde elde edebilirsiniz *erişim belirteçleri*, tarafından güvenliği sağlanan kaynaklara erişmek için kullanılabilecek bir [yetkilendirme sunucusu](active-directory-v2-protocols.md#the-basics). V2.0 uç noktası, ayrıca Web API'leri gibi güvenli kaynaklara erişim belirteçlerini vermek için Azure AD ile kayıtlı üçüncü taraf uygulamalar sağlar. Erişim belirteci vermek için bir uygulama kurulumu hakkında daha fazla bilgi için lütfen bkz [v2.0 uç noktası ile bir uygulama nasıl](active-directory-v2-app-registration.md). Oluşturmakta olduğunuz Openıd Connect kullanmanızı öneririz bir [web uygulaması](active-directory-v2-flows.md#web-apps) bir sunucuda barındırılan ve tarayıcı yoluyla erişilir.

## <a name="protocol-diagram-sign-in"></a>Protokol diyagramı: oturum açma
En basit oturum açma akışını sonraki diyagramda gösterildiği adım vardır. Biz bu makalede ayrıntılı her adım açıklanmaktadır.

![Openıd Connect protokolü: oturum açma](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

## <a name="fetch-the-openid-connect-metadata-document"></a>Openıd Connect meta veri belgesi getirme
Openıd Connect oturum açma gerçekleştirmek bir uygulama için gerekli bilgileri çoğunu içeren bir meta veri belgesi açıklar. Bu, kullanılacak URL'ler gibi bilgileri ve hizmetin genel İmzalama anahtarları konumunu içerir. V2.0 uç noktası için kullanmanız gereken Openıd Connect meta veri belgesi şudur:

```
https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
```
> [!TIP] 
> Deneyin! Tıklatın [ https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration ](https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration) görmek için `common` kiracılar yapılandırma. 
>

`{tenant}` Dört değerden birini alabilir:

| Değer | Açıklama |
| --- | --- |
| `common` |Kullanıcıların kişisel bir Microsoft hesabı ve bir iş veya Okul hesabı Azure Active Directory (Azure AD) ile uygulamaya oturum açabilir. |
| `organizations` |Yalnızca kullanıcılarla iş veya Okul hesapları Azure AD'den uygulamaya oturum açın. |
| `consumers` |Yalnızca kişisel bir Microsoft hesabı olan kullanıcılar uygulamaya oturum açabilir. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` veya `contoso.onmicrosoft.com` |Yalnızca kullanıcıların belirli bir Azure AD'den bir iş veya Okul hesabıyla Kiracı uygulamaya oturum açabilir. Azure AD kiracısı kolay etki alanı adı ya da kiracının GUID tanımlayıcısı kullanılabilir. |

Basit bir JavaScript nesne gösterimi (JSON) belge meta verilerdir. Aşağıdaki kod parçacığını bir örnek için bkz. Kod parçacığını'nin içeriği tam olarak açıklanan [Openıd Connect belirtimi](https://openid.net/specs/openid-connect-discovery-1_0.html#rfc.section.4.2).

```
{
  "authorization_endpoint": "https:\/\/login.microsoftonline.com\/common\/oauth2\/v2.0\/authorize",
  "token_endpoint": "https:\/\/login.microsoftonline.com\/common\/oauth2\/v2.0\/token",
  "token_endpoint_auth_methods_supported": [
    "client_secret_post",
    "private_key_jwt"
  ],
  "jwks_uri": "https:\/\/login.microsoftonline.com\/common\/discovery\/v2.0\/keys",

  ...

}
```

Genellikle, bir Openıd Connect kitaplığı ya da SDK'sını yapılandırmak için bu meta veri belgesi kullanın; Kitaplık işini yapmak için meta verileri kullanır. Ancak, Openıd Connect oluşturma öncesi kitaplığı kullanmıyorsanız, oturum açma v2.0 uç noktası kullanarak bir web uygulamasını gerçekleştirmek için bu makalenin sonraki bölümlerinde'ndaki adımları izleyin.

## <a name="send-the-sign-in-request"></a>Oturum açma isteği gönder
Kullanıcının kimliğini doğrulamak, web uygulamanız gerektiğinde kullanıcıya yönlendirebilirsiniz `/authorize` uç noktası. Bu istek için ilk Bacak için benzer [OAuth 2.0 yetkilendirme kodu akışını](active-directory-v2-protocols-oauth-code.md), bu önemli farklılıkları ile:

* İstek içermelidir `openid` içinde kapsam `scope` parametresi.
* `response_type` Parametre içermelidir `id_token`.
* İstek içermelidir `nonce` parametresi.

> [!IMPORTANT]
> Başarıyla sırada uygulama kaydında bir kimliği belirteci isteği [kayıt portalı](https://apps.dev.microsoft.com) olmalıdır **[örtük grant](active-directory-v2-protocols-implicit.md)** için etkin Web istemcisi. Etkinleştirilmemişse, bir `unsupported_response` hata döndürülecek: "'response_type' giriş parametresi için sağlanan değer bu istemci için izin verilmiyor. Beklenen değer 'kodu'."

Örneğin:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=678910
```

> [!TIP]
> Bu isteğin yürütülmesi için aşağıdaki bağlantıyı tıklatın. Oturum açtıktan sonra tarayıcınızı yönlendirilecek https://localhost/myapp/, adres çubuğundaki kimliği belirteci ile. Bu istek kullanan Not `response_mode=fragment` (yalnızca tanıtım amacıyla). Kullanmanızı öneririz `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| kiracı |Gerekli |Kullanabileceğiniz `{tenant}` uygulamaya oturum açarak denetim isteği yolunda değeri. İzin verilen değerler: `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları. Daha fazla bilgi için bkz: [protokol Temelleri](active-directory-v2-protocols.md#endpoints). |
| client_id |Gerekli |Uygulama Kimliği [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) uygulamanıza atanmış. |
| response_type |Gerekli |İçermelidir `id_token` Openıd Connect oturum açma için. Bu ayrıca diğer içerebilir `response_types` gibi değerler `code`. |
| redirect_uri |Önerilen |Yeniden yönlendirme URI'si burada kimlik doğrulama yanıtları gönderilebilen veya uygulamanız tarafından alınan, uygulamanızın. URL kodlanmış olmalıdır dışında tam olarak yeniden yönlendirme Portalı'nda kayıtlı URI'ler biriyle eşleşmelidir. |
| scope |Gerekli |Kapsamları boşlukla ayrılmış listesi. Openıd Connect için kapsam içermesi gerekir `openid`, UI onay "oturum" izni çevirir. Bu isteği onay isteyen diğer kapsamları de içerebilir. |
| nonce |Gerekli |Bir talep elde edilen id_token değerindeki dahil edilecek ve uygulama tarafından üretilen istekte bulunan bir değer. Uygulama belirteç yeniden yürütme saldırıları azaltmak için bu değeri doğrulayabilirsiniz. Genellikle istek kaynağını tanımlamak için kullanılan rastgele, benzersiz bir dize değeridir. |
| response_mode |Önerilen |Sonuçta elde edilen yetkilendirme kodu uygulamanıza geri göndermek için kullanılacak yöntemi belirtir. `form_post` veya `fragment` olabilir. Web uygulamaları için kullanılmasını öneririz `response_mode=form_post`, uygulamanızın en güvenli aktarımını belirteçleri sağlamak için. |
| durum |Önerilen |Aynı zamanda isteğinde bir değer belirteci yanıt olarak döndürülür. İstediğiniz herhangi bir içerik dizesi olabilir. Rastgele oluşturulan benzersiz bir değer için genellikle kullanılır [siteler arası istek sahteciliği saldırılarına engellemek](http://tools.ietf.org/html/rfc6749#section-10.12). Durumu, sayfa veya kullanıcı açıktı görünüm gibi kimlik doğrulama isteği oluşmadan önce uygulama kullanıcının durumu hakkındaki bilgileri kodlamak için de kullanılır. |
| istemi |İsteğe bağlı |Gerekli bir kullanıcı etkileşimi türünü belirtir. Şu anda yalnızca geçerli değerler `login`, `none`, ve `consent`. `prompt=login` Talep zorlar kullanıcıya, çoklu oturum açma üzerindeki geçersiz kılar, istek üzerinde kimlik bilgilerini girin. `prompt=none` Talep karşıtı olduğu. Bu talep kullanıcı hiçbir etkileşimli istemi doğabilecek sunulmayan sağlar. İstek sessizce çoklu oturum açma tamamlanamazsa, v2.0 uç noktası bir hata döndürür. `prompt=consent` Talep kullanıcı oturum açtıktan sonra OAuth onay iletişim tetikler. İletişim kutusu uygulama izinleri vermek için kullanıcıya sorar. |
| login_hint |İsteğe bağlı |Kullanıcı adı önceden biliyorsanız, kullanıcı için oturum açma sayfası kullanıcı adı ve e-posta adresi alanının önceden doldurmak için bu parametreyi kullanın. Genellikle, uygulamaları zaten kullanıcı adı bir önceki oturum bileşeninden kullanarak ayıklama sonra yeniden kimlik doğrulaması sırasında bu parametreyi kullanın `preferred_username` talep. |
| domain_hint |İsteğe bağlı |Bu değer `consumers` veya `organizations`. Dahil edilmişse, v2.0 oturum açma sayfasında, biraz daha kolay bir kullanıcı deneyimi kullanıcı geçtiği e-posta tabanlı bulma işlemi atlanıyor. Genellikle, uygulamaların bu parametreyi yeniden kimlik doğrulaması sırasında çıkartarak kullanın `tid` kimliği belirtecinden talep. Varsa `tid` değer talep `9188040d-6c67-4c5b-b112-36a304b66dad` (Microsoft Account tüketici Kiracı), kullanmak `domain_hint=consumers`. Aksi takdirde kullanın `domain_hint=organizations`. |

Bu noktada, kullanıcı kimlik bilgilerini girin ve kimlik doğrulamasını tamamlamak istenir. Kullanıcı belirtilen izinleri seçtiği v2.0 uç doğrular `scope` sorgu parametresi. Kullanıcı bu izinleri hiçbirine seçtiği değil, v2.0 uç noktası için gerekli izinlere onayı için kullanıcıya sorar. Daha fazla bilgi edinebilirsiniz [izinleri, onay ve çok müşterili uygulamalar](active-directory-v2-scopes.md).

Kullanıcının kimliğini doğrular ve onay, v2.0 uç döndürür belirtilen uygulamanızı yanıt verir sonra bölümünde belirtilen yöntemi kullanarak yeniden yönlendirme URI'si `response_mode` parametresi.

### <a name="successful-response"></a>Başarılı yanıt
Başarılı yanıt kullandığınızda `response_mode=form_post` şöyle görünür:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| id_token |İstenen uygulama kimliği belirteci. Kullanabileceğiniz `id_token` kullanıcının kimliğini doğrulamak ve kullanıcı oturumu başlatmak için parametre. Kimlik belirteçlerini ve içerikleri hakkında daha fazla ayrıntı için [v2.0 uç belirteçler başvuru](active-directory-v2-tokens.md). |
| durum |Varsa bir `state` parametresi, aynı değeri yanıt olarak görünmesi gereken istekte bulunur. Uygulama istek ve yanıt durum değerleri özdeş olduğunu doğrulamanız gerekir. |

### <a name="error-response"></a>Hata yanıtı
Böylece uygulama bunları işleyebilir hata yanıtları yeniden yönlendirme URI'si da gönderilebilir. Bir hata yanıtı şuna benzer:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak ve hataları tepki vermek için kullanabileceğiniz bir hata kodu dizesi. |
| error_description |Yardımcı olabilecek belirli bir hata iletisi kimlik doğrulama hatası kök nedenini tanımlayın. |

### <a name="error-codes-for-authorization-endpoint-errors"></a>Yetkilendirme uç noktası hataları için hata kodları
Aşağıdaki tabloda, döndürülen hata kodları açıklanmaktadır `error` hata yanıtı parametresinin:

| Hata kodu | Açıklama | İstemci eylemi |
| --- | --- | --- |
| invalid_request |Protokol hatası, bir eksik gibi parametresi gereklidir. |Düzeltin ve isteği yeniden gönderin. Bu genellikle ilk test sırasında yakalanan geliştirme hatasıdır. |
| unauthorized_client |İstemci uygulamasının yetkilendirme kodu isteğinde bulunamaz. |Bu durum genellikle, istemci uygulaması Azure AD'de kayıtlı değil veya kullanıcının Azure AD kiracısı eklenmez genellikle ortaya çıkar. Uygulama, uygulama yükleme ve Azure AD ile ekleme yönergeleri kullanıcıyla isteyebilir. |
| ACCESS_DENIED |Kaynak sahibi izni reddedildi. |İstemci uygulaması kullanıcı izin sürece devam edemiyor kullanıcı bildirebilir. |
| unsupported_response_type |Yetkilendirme sunucusu isteğine yanıt türünü desteklemiyor. |Düzeltin ve isteği yeniden gönderin. Bu genellikle ilk test sırasında yakalanan geliştirme hatasıdır. |
| server_error |Sunucu beklenmeyen bir hatayla karşılaştı. |İsteği yeniden deneyin. Bu hatalar geçici koşulları neden olabilir. İstemci uygulaması yanıt geçici bir hata nedeniyle gecikti kullanıcıya açıklayabilir. |
| temporarily_unavailable |Sunucunun geçici olarak istek işlemek için çok meşgul. |İsteği yeniden deneyin. İstemci uygulaması yanıt geçici bir koşul nedeniyle gecikti kullanıcıya açıklayabilir. |
| invalid_resource |Hedef kaynak mevcut değil, Azure AD, bulunamıyor veya düzgün yapılandırılmamış için geçersiz. |Bu, varsa, kaynak kiracısında yapılandırılmadı olduğunu gösterir. Uygulama, uygulama yükleme ve Azure AD ile ekleme yönergeleri kullanıcıyla isteyebilir. |

## <a name="validate-the-id-token"></a>Kimliği belirtecini doğrula
Bir kimliği belirteci alma kullanıcının kimliğini doğrulamak için yeterli değil. Ayrıca, kimliği belirtecin imzayı doğrulamak ve uygulamanızın gereksinimleri başına belirtecinizdeki talepleri doğrulamanız gerekir. V2.0 uç noktası kullanan [JSON Web belirteçleri (Jwt'ler)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ve Belirteçleri imzalamak ve bunların geçerli olduğunu doğrulamak için ortak anahtar şifrelemesi.

İstemci kodu kimliği belirteçte doğrulamayı seçebilirsiniz, ancak yaygın bir arka uç sunucusuna kimliği belirteci göndermek ve doğrulama var. gerçekleştirmek için bir uygulamadır. Kimliği belirtecinin imzası doğruladıktan sonra birkaç talep doğrulamanız gerekir. Daha fazla bilgi için daha fazla dahil olmak üzere [belirteçleri doğrulama](active-directory-v2-tokens.md#validating-tokens) ve [anahtar geçişi imzalama hakkında önemli bilgiler](active-directory-v2-tokens.md#validating-tokens), bkz: [v2.0 belirteçler başvuru](active-directory-v2-tokens.md). Ayrıştırma ve belirteçleri doğrulamak için bir kitaplık kullanmanızı öneririz. Bu kitaplıklar en az biri çoğu diller ve platformlar için mevcut değil.
<!--TODO: Improve the information on this-->

Senaryonuza bağlı olarak ek talep doğrulamak isteyebilirsiniz. Bazı ortak doğrulamaları şunları içerir:

* Kullanıcı veya kuruluş uygulaması için kaydolmuş emin olun.
* Kullanıcı yetkilendirme veya ayrıcalıkları gerekli olun.
* Belirli bir kimlik doğrulama gücü, çok faktörlü kimlik doğrulaması gibi oluştuğunu emin olun.

Bir kimliği belirtecinizdeki talepleri hakkında daha fazla bilgi için bkz: [v2.0 uç belirteçler başvuru](active-directory-v2-tokens.md).

Kimliği belirteci tamamen doğrulandıktan sonra kullanıcıyla oturum başlayabilirsiniz. Talep Kimliği belirteçte uygulamanızda kullanıcı hakkında bilgi almak için kullanın. Görüntü, kayıtları, yetkilerini ve benzeri için bu bilgileri kullanabilirsiniz.

## <a name="send-a-sign-out-request"></a>Oturum kapatma isteği gönder
Uygulamanızı kullanıcıdan çıkışı oturum istediğinizde, uygulamanızın tanımlama bilgilerini temizleyin veya aksi halde kullanıcının oturumunu sona erdirmek için yeterli değil. Oturumu kapatmak için v2.0 uç noktası için de kullanıcı yönlendirmesi gerekir. Bunu yapmazsanız, geçerli tek bir oturum açma oturumu v2.0 uç noktasına sahip olmanız gerekir, çünkü kullanıcı, uygulamanızın kimlik bilgilerini yeniden girmeye gerek kalmadan yeniden kimliğini doğrular.

Kullanıcıya yönlendirebilirsiniz `end_session_endpoint` Openıd Connect meta veri belgesinde listelenen:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parametre | Koşul | Açıklama |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | Önerilen | Başarıyla oturumunuzu sonra kullanıcı yönlendireceği URL. Parametre dahil edilmezse, kullanıcı v2.0 uç noktası tarafından oluşturulan genel bir ileti gösterilir. Bu URL uygulama kayıt Portalı'nda, uygulamanız için URI kayıtlı yeniden yönlendirme biriyle eşleşmelidir. |

## <a name="single-sign-out"></a>Çoklu oturum kapatma
Ne zaman yeniden yönlendirmek için kullanıcı `end_session_endpoint`, kullanıcının oturumunu tarayıcıdan v2.0 uç temizler. Ancak, kullanıcı hala Microsoft hesapları için kimlik doğrulama kullanan diğer uygulamalar için oturum açmanız. Kullanıcı aynı anda v2.0 çıkışı imzalamak bu uygulamalara olanak tanımak için son nokta bir HTTP GET isteği kayıtlı gönderir `LogoutUrl` kullanıcı şu anda oturum tüm uygulamaların. Uygulamalar kullanıcı tanımlayan herhangi bir oturumunda temizlenmesi ve döndürme bu istek için yanıt gerekir bir `200` yanıt. Çoklu oturum kapatma uygulamanızda desteklemek istiyorsanız, gibi uygulamalıdır bir `LogoutUrl` uygulamanızın kodu. Ayarlayabileceğiniz `LogoutUrl` uygulama kayıt Portalı'ndan.

## <a name="protocol-diagram-access-token-acquisition"></a>Protokol diyagramı: erişim belirteci edinme
Birçok web uygulamaları yalnızca kullanıcı olarak, aynı zamanda OAuth kullanarak kullanıcı adına bir web hizmetine erişmek için oturum açmanız gerekir. Bu senaryo, OAuth yetkilendirme kodu akışını kullanıyorsanız, erişim belirteçleri almak için kullanabileceğiniz bir kimlik doğrulama kodu aynı anda alınırken kullanıcı kimlik doğrulaması için Openıd Connect birleştirir.

Tam Openıd Connect oturum açma ve belirteç edinme akış sonraki diyagrama benzer. Biz her adım, makalenin sonraki bölümlerde ayrıntılı olarak açıklanmaktadır.

![Openıd Connect protokolü: belirteci edinme](../../media/active-directory-v2-flows/convergence_scenarios_webapp_webapi.png)

## <a name="get-access-tokens"></a>Erişim belirteci alın
Erişim belirteci edinmek için oturum açma isteği değiştirin:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application ID
&response_type=id_token%20code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered redirect URI, URL encoded
&response_mode=form_post                              // 'form_post' or 'fragment'
&scope=openid%20                                      // Include both 'openid' and scopes that your app needs  
offline_access%20                                         
https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

> [!TIP]
> Bu isteğin yürütülmesi için aşağıdaki bağlantıyı tıklatın. Oturum açtıktan sonra tarayıcının yönlendirildiği https://localhost/myapp/kimliği belirteci ve adres çubuğunda bir kod. Bu istek kullanan Not `response_mode=fragment` (yalnızca tanıtım amacıyla). Kullanmanızı öneririz `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token%20code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=fragment&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

İstekte izin kapsamları dahil olmak üzere ve kullanarak `response_type=id_token code`, kullanıcının belirtilen izinleri seçtiği v2.0 uç sağlar `scope` sorgu parametresi. Uygulamanız için bir erişim belirteci exchange için bir kimlik doğrulama kodu döndürür.

### <a name="successful-response"></a>Başarılı yanıt
Kullanarak başarılı bir yanıt `response_mode=form_post` şöyle görünür:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| id_token |İstenen uygulama kimliği belirteci. Kullanıcının kimliğini doğrulamak ve kullanıcı oturumu başlatmak için kimliği belirteci kullanabilirsiniz. Kimlik belirteçlerini ve bunların içerikleri hakkında daha fazla ayrıntı bulabilirsiniz [v2.0 uç belirteçler başvuru](active-directory-v2-tokens.md). |
| Kod |Uygulama istenen yetkilendirme kodu. Uygulama, hedef kaynak için bir erişim belirteci istemek için yetkilendirme kodu kullanabilirsiniz. Bir kimlik doğrulama kodu çok kısa süreli ' dir. Genellikle, bir kimlik doğrulama kodu yaklaşık 10 dakika içinde süresi dolar. |
| durum |İstekte bir durum parametresi eklenirse, aynı değeri yanıt olarak görünmelidir. Uygulama istek ve yanıt durum değerleri özdeş olduğunu doğrulamanız gerekir. |

### <a name="error-response"></a>Hata yanıtı
Böylece uygulama bunları uygun şekilde işleyebilir hata yanıtları yeniden yönlendirme URI'si da gönderilebilir. Bir hata yanıtı şuna benzer:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak ve hataları tepki vermek için kullanabileceğiniz bir hata kodu dizesi. |
| error_description |Yardımcı olabilecek belirli bir hata iletisi kimlik doğrulama hatası kök nedenini tanımlayın. |

Olası hata kodları ve önerilen istemci yanıtları açıklaması için bkz: [yetkilendirme uç noktası hataları için hata kodları](#error-codes-for-authorization-endpoint-errors).

Bir yetkilendirme kodu ve bir kimliği belirteci varsa, kullanıcı oturum açabilir ve onların adına erişim belirteci alın. Kullanıcı oturumu için kimliği belirtecini doğrula [açıklandığı gibi tam olarak](#validate-the-id-token). Erişim belirteci almak için açıklanan adımları izleyin bizim [OAuth Protokolü belgeleri](active-directory-v2-protocols-oauth-code.md#request-an-access-token).
