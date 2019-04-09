---
title: Azure Active Directory v2.0 ve Openıd Connect protokolünü | Microsoft Docs
description: Azure AD v2.0 uygulaması Openıd Connect kimlik doğrulama protokolü kullanarak Web uygulamaları oluşturun.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: a4875997-3aac-4e4c-b7fe-2b4b829151ce
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1527a326ca0107df33857284774252b327b7d8bc
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59273269"
---
# <a name="azure-active-directory-v20-and-the-openid-connect-protocol"></a>Azure Active Directory v2.0 ve Openıd Connect Protokolü

Openıd Connect, güvenli bir şekilde bir web uygulaması için bir kullanıcı oturum açma için kullanabileceğiniz bir OAuth 2.0 üzerinde kurulu bir kimlik doğrulama protokolüdür. Openıd Connect v2.0 uç noktanın uygulamasını kullandığınızda, oturum açma ve API erişimi web tabanlı uygulamalarınıza ekleyebilirsiniz. Bu makalede, bu bağımsız dil olarak bunun nasıl yapılacağını göstermektedir ve HTTP iletileri gönderip tüm Microsoft açık kaynak kitaplıkları kullanmadan açıklar.

> [!NOTE]
> V2.0 uç noktası, tüm Azure Active Directory (Azure AD) senaryolarını ve özelliklerini desteklemez. V2.0 uç noktası kullanması gerekip gerekmediğini belirlemek için aşağıdaki hakkında bilgi edinin: [v2.0 sınırlamaları](active-directory-v2-limitations.md).

[Openıd Connect](https://openid.net/specs/openid-connect-core-1_0.html) OAuth 2.0 genişletir *yetkilendirme* protokolü olarak kullanılmak üzere bir *kimlik doğrulaması* protokol yapabileceğiniz böylece tek bir OAuth kullanarak oturum açmayı. Openıd Connect kavramını sunar bir *kimlik belirteci*, kullanıcının kimliğini doğrulamak istemci izin veren bir güvenlik belirteci olduğu. Kimlik belirteci, ayrıca kullanıcının temel profil bilgilerini alır. Openıd Connect OAuth 2.0 genişlettiğinden, uygulamaları güvenli bir şekilde edinebilir *erişim belirteçlerini*, tarafından güvenliği sağlanan kaynaklara erişmek için kullanılabilecek bir [yetkilendirme sunucusu](active-directory-v2-protocols.md#the-basics). V2.0 uç noktası için Web API'leri gibi güvenli kaynaklara erişim belirteçlerini vermek için Azure AD ile kayıtlı bir üçüncü taraf uygulamaları da sağlar. Uygulama erişim belirteçlerini vermek için ayarlama hakkında daha fazla bilgi için bkz: [v2.0 uç noktası ile bir uygulamayı kaydetme](quickstart-v2-register-an-app.md). Derliyorsanız Openıd Connect kullanmanızı öneririz bir [web uygulaması](v2-app-types.md#web-apps) bir sunucuda barındırılan ve tarayıcı yoluyla erişilir.

## <a name="protocol-diagram-sign-in"></a>Protokol diyagramı: oturum açma

En temel oturum açma akışını sonraki Diyagramda gösterilen adımlar vardır. Her adım, bu makalede ayrıntılı açıklanmıştır.

![Openıd Connect protokolü: oturum açma](./media/v2-protocols-oidc/convergence_scenarios_webapp.png)

## <a name="fetch-the-openid-connect-metadata-document"></a>Openıd Connect meta veri belgesi getirilemedi

Openıd Connect oturum açma gerçekleştirmek bir uygulama için gerekli olan ilgili bilgilerin çoğunu içeren bir meta veri belgesinin açıklar. Bu, hizmetin ortak İmzalama anahtarları konumunu ve kullanılacak URL'leri gibi bilgileri içerir. V2.0 uç noktası için bu kullanmalısınız Openıd Connect meta veri belgesi.

```
https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
```
> [!TIP]
> Deneyin! Tıklayın [ https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration ](https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration) görmek için `common` kiracılar yapılandırma.

`{tenant}` Dört değerden birini alabilir:

| Değer | Açıklama |
| --- | --- |
| `common` |Hem kişisel Microsoft hesabı hem de Azure Active Directory (Azure AD) bir iş veya Okul hesabı olan kullanıcılar uygulamada oturum açabilir. |
| `organizations` |Yalnızca kullanıcılar iş veya Okul hesapları Azure ad uygulaması için oturum açın. |
| `consumers` |Yalnızca kişisel Microsoft hesabı olan kullanıcıların uygulamada oturum açabilir. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` or `contoso.onmicrosoft.com` | Yalnızca kullanıcıların bir iş veya Okul hesabıyla belirli bir Azure ad Kiracı uygulamaya oturum açabilirsiniz. Azure AD Kiracı kolay etki alanı adını veya kiracının GUID tanımlayıcısı kullanılabilir. Tüketici Kiracı kullanabilirsiniz `9188040d-6c67-4c5b-b112-36a304b66dad`, yerine `consumers` Kiracı.  |

Meta veriler bir basit bir JavaScript nesne gösterimi (JSON) belgedir. Aşağıdaki kod parçacığını bir örnek için bkz. İş parçacığının içeriği tamamen açıklanan [Openıd Connect belirtimi](https://openid.net/specs/openid-connect-discovery-1_0.html#rfc.section.4.2).

```
{
  "authorization_endpoint": "https:\/\/login.microsoftonline.com\/{tenant}\/oauth2\/v2.0\/authorize",
  "token_endpoint": "https:\/\/login.microsoftonline.com\/{tenant}\/oauth2\/v2.0\/token",
  "token_endpoint_auth_methods_supported": [
    "client_secret_post",
    "private_key_jwt"
  ],
  "jwks_uri": "https:\/\/login.microsoftonline.com\/{tenant}\/discovery\/v2.0\/keys",

  ...

}
```
Uygulamanızı kullanarak sonucunda özel İmzalama anahtarları varsa [talep eşleme](active-directory-claims-mapping.md) gerekir ekleme özelliği, bir `appid` sorgu parametresi almak üzere uygulama Kimliğini içeren bir `jwks_uri` uygulamanıza işaret eden projenin imzalama özellikli anahtarı bilgiler. Örneğin: `https://login.microsoftonline.com/{tenant}/.well-known/v2.0/openid-configuration?appid=6731de76-14a6-49ae-97bc-6eba6914391e` içeren bir `jwks_uri` , `https://login.microsoftonline.com/{tenant}/discovery/v2.0/keys?appid=6731de76-14a6-49ae-97bc-6eba6914391e`.


Genellikle, bir kitaplık Openıd Connect veya SDK'sını yapılandırmak için bu meta veri belgesi kullanın; Kitaplığı, işlemini gerçekleştirmek için meta verileri kullanmanız gerekir. Ancak, önceden oluşturulmuş bir Openıd Connect kitaplığı kullanmıyorsanız, oturum açma v2.0 uç noktası'nı kullanarak bir web uygulamasında gerçekleştirmek için bu makalenin geri kalanında adımları takip edebilirsiniz.

## <a name="send-the-sign-in-request"></a>Oturum açma isteği gönder

Web uygulamanızı kullanıcının kimliğini doğrulaması gerektiğinde, kullanıcıya yönlendirebilir `/authorize` uç noktası. Bu istek için ilk oluşturan benzer [OAuth 2.0 yetkilendirme kod akışı](v2-oauth2-auth-code-flow.md), bu önemli farklılıkları ile:

* İstek içermelidir `openid` kapsamını `scope` parametresi.
* `response_type` Parametre içermelidir `id_token`.
* İstek içermelidir `nonce` parametresi.

> [!IMPORTANT]
> Başarılı bir şekilde uygulama kaydında /authorization uç noktasından kimlik belirteci isteğinde bulunmak için [kayıt portalı](https://portal.azure.com) , etkin kimlik doğrulama sekmede id_tokens örtük vermeyi olmalıdır ( Ayarlar`oauth2AllowIdTokenImplicitFlow`bayrağını [uygulama bildirimini](reference-app-manifest.md) için `true`). Bunu etkin değilse, bir `unsupported_response` hata döndürülür: "'Response_type' giriş parametresi için sağlanan değer bu istemci için izin verilmiyor. Beklenen değer 'kodu'."

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
> Bu isteğin yürütülmesi için aşağıdaki bağlantıya tıklayın. Oturum açtıktan sonra tarayıcınızı yönlendirilecek `https://localhost/myapp/`, adres çubuğundaki bir kimlik belirteci ile. Bu istek kullanan Not `response_mode=fragment` (yalnızca tanıtım amacıyla). Kullanmanızı öneririz `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| kiracı |Gerekli |Kullanabileceğiniz `{tenant}` değeri için uygulamada oturum denetimi için istek yolu. İzin verilen değerler `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları. Daha fazla bilgi için [protokolü temel](active-directory-v2-protocols.md#endpoints). |
| client_id |Gerekli |Uygulama Kimliği [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) uygulamanıza atanan. |
| response_type |Gerekli |İçermelidir `id_token` Openıd Connect oturum açma için. Bu ayrıca diğer içerebilir `response_type` gibi değerleri `code`. |
| redirect_uri |Önerilen |Yeniden yönlendirme URI'si uygulamanızın, burada kimlik doğrulama yanıtlarının gönderilebilen veya uygulamanız tarafından alındı. Bu URL olarak kodlanmış olması dışında tam olarak yeniden yönlendirme URI'leri Portalı'nda kayıtlı biriyle eşleşmelidir. Mevcut, uç nokta için kullanıcıya rastgele göndermek için çekme bir kayıtlı redirect_uri yedekler. |
| scope |Gerekli |Kapsamları boşlukla ayrılmış listesi. Openıd Connect için kapsamı içermesi gerekir `openid`, onay için UI "oturumunuzu açma" izni çevirir. Bu isteği onayı isteyen için diğer kapsamları de içerebilir. |
| nonce |Gerekli |Sonuç id_token değerini talep olarak dahil edilecek uygulama tarafından oluşturulan isteğinde bulunan bir değer. Uygulamanın belirteç yeniden yürütme saldırıları azaltmak için bu değer doğrulayabilirsiniz. Genellikle istek kaynağı tanımlamak için kullanılan rastgele, benzersiz bir dize değeridir. |
| response_mode |Önerilen |Ortaya çıkan bir yetkilendirme kodu uygulamanıza geri göndermek için kullanılması gereken yöntemini belirtir. `form_post` veya `fragment` olabilir. Web uygulamaları için kullanılması önerilir `response_mode=form_post`, uygulamanız için en güvenli belirteçleri aktarımını sağlamak için. |
| durum |Önerilen |Belirteç yanıtta döndürülecek isteğinde bulunan bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Rastgele oluşturulmuş bir benzersiz değer için genellikle kullanılır [siteler arası istek sahteciliğini saldırılarını](https://tools.ietf.org/html/rfc6749#section-10.12). Durumu, uygulama kullanıcının durumu hakkında bilgi sayfası ya da kullanıcı açıktı görünümü gibi kimlik doğrulama isteği oluşmadan önce kodlamak için de kullanılır. |
| istemi |İsteğe bağlı |Gerekli olan kullanıcı etkileşimi türünü belirtir. Şu anda yalnızca geçerli değerler `login`, `none`, ve `consent`. `prompt=login` Talep çoklu oturum açma olumsuz duruma getirir, isteği kimlik bilgilerini girmesini zorlar. `prompt=none` Talep karşıtı olduğu. Bu talep, kullanıcının hiçbir etkileşimli istemi olmadan sunulmayan sağlar. İstek sessizce çoklu oturum açma işleminin neden tamamlanamadığına v2.0 uç noktasına bir hata döndürür. `prompt=consent` Talep, kullanıcı oturum açtıktan sonra OAuth onay iletişim tetikler. İletişim kutusu, uygulamaya izinleri vermek için kullanıcıya sorar. |
| login_hint |İsteğe bağlı |Bu parametre, önceden kullanıcı adını biliyorsanız, kullanıcı için oturum açma sayfası kullanıcı adı ve e-posta adresi alanının önceden doldurmak için kullanabilirsiniz. Bu parametre uygulamaları sırasında zaten kullanıcı adının bir önceki oturum açma kullanarak ayıklama sonra yeniden kimlik doğrulaması, sık kullandığınız `preferred_username` talep. |
| domain_hint |İsteğe bağlı | Bir Federasyon dizindeki kullanıcı yayınladık.  Bu, v2.0 oturum açma sayfasında, biraz daha kolay bir kullanıcı deneyimi kullanıcının geçtiği e-posta tabanlı bulma işlemi atlanıyor. Aracılığıyla bir şirket içi dizin ADFS gibi Federasyon kiracıları için genellikle bir sorunsuz oturum açma mevcut oturum açma oturumu nedeniyle sonuçlanır. |

Bu noktada, kullanıcı kimlik bilgilerini girin ve kimlik doğrulamasını tamamlamak için istenir. Kullanıcı için belirtilen izinleri onaylamasını v2.0 uç noktası doğrular `scope` sorgu parametresi. Kullanıcının tüm bu izinleri olmayan etmişse, v2.0 uç noktası için gerekli izinleri kabul kullanıcıya sorar. Daha fazla bilgi edinebilirsiniz [izinleri, onay ve çok kiracılı uygulamaları](v2-permissions-and-consent.md).

Kullanıcının kimliğini doğrular ve v2.0 uç noktası için belirtilen uygulamanızı bir yanıt döndürür, onayı veren sonra bölümünde belirtilen yöntemi kullanarak yeniden yönlendirme URI'si `response_mode` parametresi.

### <a name="successful-response"></a>Başarılı yanıt

Başarılı bir yanıt kullandığınızda `response_mode=form_post` şöyle görünür:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| id_token |Uygulama istenen kimlik belirteci. Kullanabileceğiniz `id_token` kullanıcının kimliğini doğrulamak ve kullanıcıyı bir oturum başlatmak için parametre. Kimlik belirteçlerini ve içerikleri hakkında daha fazla bilgi için bkz. [ `id_tokens` başvuru](id-tokens.md). |
| durum |Varsa bir `state` aynı değeri yanıt olarak görünmesi gereken parametresi istekte bulunur. Uygulama istek ve yanıt durum değerleri özdeş olduğunu doğrulamanız gerekir. |

### <a name="error-response"></a>Hata yanıtı

Uygulama bunları işleyebilmesi hata yanıtları yeniden yönlendirme URI'si da gönderilebilir. Bir hata yanıtı şuna benzer:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak ve hataları tepki vermek için kullanabileceğiniz bir hata kodu dizesi. |
| error_description |Belirli bir hata iletisi yardımcı olabilecek bir kimlik doğrulama hatası kök nedenini tanımlayın. |

### <a name="error-codes-for-authorization-endpoint-errors"></a>Yetkilendirme uç noktası hataları için hata kodları

Aşağıdaki tabloda, döndürülen hata kodları açıklanmaktadır `error` parametresi hata yanıtı:

| Hata kodu | Açıklama | İstemci eylemi |
| --- | --- | --- |
| invalid_request |Protokol hatası, örneğin bir eksik parametre gereklidir. |Düzeltin ve isteği yeniden gönderin. Bu genellikle ilk sınama sırasında yakalanan bir geliştirme hatadır. |
| unauthorized_client |İstemci uygulama bir yetkilendirme kodu istenemiyor. |Bu durum genellikle, istemci uygulaması, Azure AD'de kayıtlı değil veya kullanıcının Azure AD kiracısına eklenmez genellikle ortaya çıkar. Uygulamayı Azure AD'ye ekleme ve uygulamayı yüklemek için yönergeleri kullanıcıyla isteyebilir. |
| access_denied |Kaynak sahibi onay reddedildi. |İstemci uygulama, kullanıcı onay sürece devam edemiyor kullanıcıya bildirebilir. |
| unsupported_response_type |Yetkilendirme sunucusu isteği yanıt türü desteklemiyor. |Düzeltin ve isteği yeniden gönderin. Bu genellikle ilk sınama sırasında yakalanan bir geliştirme hatadır. |
| server_error |Sunucu beklenmeyen bir hatayla karşılaştı. |İsteği yeniden deneyin. Bu hataların geçici koşullar neden olabilir. İstemci uygulama, kullanıcıya yanıtına geçici bir hata nedeniyle ertelendi açıklayabilir. |
| temporarily_unavailable |Sunucunun geçici olarak isteği işleyemeyecek kadar meşgul. |İsteği yeniden deneyin. İstemci uygulama, kullanıcıya, yanıtına bir geçici koşul nedeniyle ertelendi açıklayabilir. |
| invalid_resource |Hedef kaynak yoksa, Azure AD, bulunamıyor veya doğru şekilde yapılandırılmadığı için geçersiz. |Bu, varsa, kaynak kiracıda yapılandırılmış olduğunu gösterir. Uygulama, kullanıcının uygulama yükleme ve Azure AD'ye ekleme hakkında yönergeler isteyebilir. |

## <a name="validate-the-id-token"></a>Kimlik belirteci doğrulama

Yalnızca bir id_token alma, kullanıcının kimliğini doğrulamak için yeterli değildir; id_token'ın imzayı doğrulamak ve uygulamanızın gereksinimlerini başına belirteçteki talepleri doğrulamak gerekir. V2.0 uç noktası kullanan [JSON Web belirteçleri (Jwt'ler)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ve Belirteçleri imzalamak ve geçerli olduğunu doğrulamak için ortak anahtar şifrelemesi.

Doğrulamak seçebileceğiniz `id_token` istemci kodu, ancak yaygın bir uygulama olan göndermek için `id_token` bir arka uç sunucusuna ve orada doğrulama gerçekleştirin. İd_token imzası doğruladıktan sonra birkaç talepleri doğrulamak için gerekli vardır. Bkz [ `id_token` başvuru](id-tokens.md) daha fazla bilgi edinmek için de dahil olmak üzere [doğrulama belirteçleri](id-tokens.md#validating-an-id_token) ve [önemli bilgiler hakkında imzalama anahtar geçişi](active-directory-signing-key-rollover.md). Öneririz belirteçleri doğrulamak ve ayrıştırmak için bir kitaplık yapmayı kullanımı - en az bir dil ve platform için kullanılabilir.

Senaryonuza bağlı olarak ek istekleri doğrulamak isteyebilirsiniz. Bazı ortak doğrulamaları şunlardır:

* Kullanıcı/kuruluş sağlama uygulama için kaydolmuş.
* Uygun yetkilendirme/ayrıcalıklarına sahip kullanıcı sağlama
* Belirli bir kimlik doğrulama gücü sağlamak, çok faktörlü kimlik doğrulaması gibi oluştu.

İd_token tamamen doğruladıktan sonra bir kullanıcı oturumu başlatmak ve uygulamanızdaki kullanıcı hakkında bilgi edinmek için id_token talepleri kullanın. Bu bilgiler kullanılabilir görüntüleme, kayıtları, kişiselleştirme, vb. için.

## <a name="send-a-sign-out-request"></a>Oturum kapatma isteği gönder

Uygulama kullanıcının oturum açmak istediğinizde, uygulamanızın tanımlama bilgilerini temizleyin veya aksi halde kullanıcının oturumunu sona erdirmek için yeterli değil. Ayrıca, kullanıcının oturumu kapatmak için v2.0 uç noktası yönlendirmelisiniz. Bunu yapmazsanız, geçerli tek bir oturum açma oturumu v2.0 uç noktaya sahip olmanız gerekir, çünkü kullanıcı uygulamanızı kendi kimlik bilgilerini yeniden girmeye gerek kalmadan olan.

Kullanıcıya yeniden yönlendirebilirsiniz `end_session_endpoint` Openıd Connect meta veri belgesinde listelenen:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parametre | Koşul | Açıklama |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | Önerilen | Başarıyla oturum kapatma sonrasında kullanıcının yönlendireceği URL. Parametre dahil edilmezse kullanıcının v2.0 uç noktası tarafından oluşturulur, genel bir ileti gösterilir. Bu URL yeniden yönlendirme URI'leri uygulamanızı uygulama kayıt Portalı'nda kayıtlı biriyle eşleşmelidir. |

## <a name="single-sign-out"></a>Çoklu oturum kapatma

Ne zaman yeniden yönlendirmek için kullanıcı `end_session_endpoint`, kullanıcının oturumunu bir tarayıcıdan v2.0 uç noktası temizler. Ancak, kullanıcı yine de Microsoft hesapları için kimlik doğrulaması kullanan diğer uygulamalar için oturum açmanız. Bu uygulamaların kullanıcının aynı anda v2.0 oturumunu kapatmaz sağlamak için uç nokta bir HTTP GET isteği kayıtlı gönderir `LogoutUrl` kullanıcı şu anda oturum açmış tüm uygulamalar. Kullanıcıyı tanımlayan hiçbir oturum temizlemek ve döndürme, uygulamalar bu istek için yanıt gereken bir `200` yanıt. Uygulamanızda oturum kapatma tek desteklemek istiyorsanız, örneğin uygulamalıdır bir `LogoutUrl` uygulamanızın kod. Ayarlayabileceğiniz `LogoutUrl` uygulama kayıt Portalı'ndan.

## <a name="protocol-diagram-access-token-acquisition"></a>Protokol diyagramı: Erişim belirteci alma

Birçok web uygulamaları yalnızca kullanıcı de, aynı zamanda OAuth kullanarak kullanıcı adına bir web hizmetine erişmek için oturum açmanız gerekir. Bu senaryoda, kullanıcı kimlik doğrulaması, OAuth yetkilendirme kodu akışını kullanıyorsanız erişim belirteçlerini almak için kullanabileceğiniz bir yetkilendirme kodu aynı anda alınırken Openıd Connect birleştirir.

Tam Openıd Connect oturum açma ve belirteç edinme akışı sonraki diyagrama benzer. Her adım makalenin sonraki bölümlerde ayrıntılı olarak açıklanmaktadır.

![Openıd Connect protokolü: Belirteç edinme](./media/v2-protocols-oidc/convergence_scenarios_webapp_webapi.png)

## <a name="get-access-tokens"></a>Erişim belirteci alma
Erişim belirteçlerini almak için oturum açma isteği değiştirin:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application ID
&response_type=id_token%20code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered redirect URI, URL encoded
&response_mode=form_post                              // 'form_post' or 'fragment'
&scope=openid%20                                      // Include both 'openid' and scopes that your app needs  
offline_access%20                                         
https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

> [!TIP]
> Bu isteğin yürütülmesi için aşağıdaki bağlantıya tıklayın. Oturum açtıktan sonra tarayıcının yönlendirildiği `https://localhost/myapp/`kimlik belirteci ve adres çubuğuna bir kod. Bu istek kullanan Not `response_mode=fragment` yalnızca tanıtım amacıyla. Kullanmanızı öneririz `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token%20code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=fragment&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fuser.read&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

İstekte izin kapsamları ekleyerek ve kullanarak `response_type=id_token code`, v2.0 uç noktası kullanıcı için belirtilen izinleri onaylamasını sağlar `scope` sorgu parametresi. Uygulamanıza bir erişim belirteciyle değiştirilecek yetkilendirme kodu döndürür.

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
| id_token |Uygulama istenen kimlik belirteci. Kimlik belirteci, kullanıcının kimliğini doğrulamak ve kullanıcıyı bir oturum başlatmak için kullanabilirsiniz. Kimlik belirteçlerini ve bunların içeriğini hakkında daha fazla ayrıntı bulabilirsiniz [ `id_tokens` başvuru](id-tokens.md). |
| kod |Uygulama talep yetkilendirme kodu. Uygulama, hedef kaynağı için bir erişim belirteci istemek için yetkilendirme kodu kullanabilirsiniz. Çok kısa süreli bir yetkilendirme kodudur. Genellikle, bir yetkilendirme kodu yaklaşık 10 dakika içinde süresi dolar. |
| durum |State parametresi istekte yer alıyorsa aynı değeri yanıt olarak görünmelidir. Uygulama istek ve yanıt durum değerleri özdeş olduğunu doğrulamanız gerekir. |

### <a name="error-response"></a>Hata yanıtı

Uygulama bunları uygun şekilde işleyebilmeniz hata yanıtları yeniden yönlendirme URI'si da gönderilebilir. Bir hata yanıtı şuna benzer:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak ve hataları tepki vermek için kullanabileceğiniz bir hata kodu dizesi. |
| error_description |Belirli bir hata iletisi yardımcı olabilecek bir kimlik doğrulama hatası kök nedenini tanımlayın. |

Olası hata kodları ve önerilen istemci yanıtları açıklaması için bkz: [yetkilendirme uç noktası hataları için hata kodlarını](#error-codes-for-authorization-endpoint-errors).

Bir yetkilendirme kodunu ve bir kimlik belirteci varsa, kullanıcı oturum açın ve kendi adınıza erişim belirteçlerini almak. Kullanıcının oturum açmak için kimlik belirteci doğrulamak [açıklandığı gibi tam olarak](id-tokens.md#validating-an-id_token). Erişim belirteçlerini almak için açıklanan adımları izleyin. [OAuth kod flow belgeleri](v2-oauth2-auth-code-flow.md#request-an-access-token).
