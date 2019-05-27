---
title: Azure AD'de OAuth 2.0 yetkilendirme kod akışı anlama
description: Bu makalede, web uygulamaları ve web API'leri, kiracınızın Azure Active Directory ve OAuth 2.0 kullanarak erişim yetkisi vermek için HTTP iletilerini kullanmayı açıklar.
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e05d79773cfd2ebae8047e75d41684de9101787a
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65962189"
---
# <a name="authorize-access-to-azure-active-directory-web-applications-using-the-oauth-20-code-grant-flow"></a>OAuth 2.0 kodu verme akışı kullanarak Azure Active Directory web uygulamalarına erişim yetkisi verme

Web uygulamalarına ve Azure AD kiracınızdaki web API'lerine erişim yetkisi vermek sağlamak için azure Active Directory (Azure AD) kullanan OAuth 2.0. Bu kılavuz dilden bağımsızdır ve HTTP iletileri gönderip herhangi birini kullanmadan açıklar bizim [açık kaynak kitaplıkları](active-directory-authentication-libraries.md).

OAuth 2.0 yetkilendirme kod akışı açıklanan [OAuth 2.0 belirtiminin 4.1 bölümünde](https://tools.ietf.org/html/rfc6749#section-4.1). Bu kimlik doğrulama ve yetkilendirme, web uygulamaları dahil olmak üzere çoğu uygulama türlerinde gerçekleştirmek için kullanılır ve yerel olarak yüklenen uygulamalar.

[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)]

## <a name="oauth-20-authorization-flow"></a>OAuth 2.0 yetkilendirme akışı

Yüksek düzeyde, bir uygulama için tüm yetkilendirme akışı biraz şuna benzer:

![OAuth yetkilendirme kod akışı](./media/v1-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)

## <a name="request-an-authorization-code"></a>İstek bir yetkilendirme kodu

Kullanıcıya yönlendiren istemci yetkilendirme kod akışı başlar `/authorize` uç noktası. Bu istekte istemcinin kullanıcıdan almak için gerekli olan izinleri belirtir. Kiracınız için OAuth 2.0 yetkilendirme uç noktası seçerek alabilirsiniz **uygulama kayıtları > uç noktalar** Azure portalında.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%3A12345
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| tek |gerekli |`{tenant}` İstek yolunda değer, uygulamaya oturum denetimi için kullanılabilir. Kiracı tanımlayıcıları, örneğin, izin verilen değerler: `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` veya `contoso.onmicrosoft.com` veya `common` Kiracı bağımsız belirteçleri |
| client_id |gerekli |Azure AD ile kaydettiğinizde, uygulamanıza atanan uygulama kimliği. Bunu Azure Portal'da bulabilirsiniz. Tıklayın **Azure Active Directory** Hizmetleri Kenar çubuğunda tıklatın **uygulama kayıtları**, uygulamayı seçin. |
| response_type |gerekli |İçermelidir `code` yetkilendirme kod akışı için. |
| redirect_uri |Önerilen |Burada kimlik doğrulama yanıtlarının gönderilebilen veya uygulamanız tarafından alınan uygulamanızın redirect_uri. Bu url olarak kodlanmış olması dışında Portalı'nda kayıtlı redirect_uris biri tam olarak eşleşmesi gerekir. Yerel & mobil uygulamaları için varsayılan değeri kullanması gereken `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode |isteğe bağlı |Uygulamanıza elde edilen belirteç geri göndermek için kullanılması gereken yöntemini belirtir. Olabilir `query`, `fragment`, veya `form_post`. `query` kod, yeniden yönlendirme URI'si üzerinde bir sorgu dizesi parametresi olarak sağlar. Örtük akışını kullanarak bir kimlik belirteci istediği, kullanamazsınız `query` belirtilmiş [Openıd spec](https://openid.net/specs/oauth-v2-multiple-response-types-1_0.html#Combinations). Yalnızca kodum istediği, kullanabileceğiniz `query`, `fragment`, veya `form_post`. `form_post` kodu, yeniden yönlendirme URI'sini içeren bir GÖNDERİ yürütür. Varsayılan değer `query` kod akış.  |
| durum |Önerilen |Ayrıca belirteci yanıtta döndürülen isteğinde bulunan bir değer. Rastgele oluşturulmuş bir benzersiz değer için genellikle kullanılan [siteler arası istek sahteciliğini saldırılarını önleme](https://tools.ietf.org/html/rfc6749#section-10.12). Durum, uygulama kullanıcının durumu hakkındaki bilgileri sayfasında ya da görünümü üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için de kullanılır. |
| kaynak | Önerilen |Hedef web API (kaynak güvenli) uygulama kimliği URI'si. Uygulama Kimliği URI'si, Azure Portalı'nda bulmak için tıklatın **Azure Active Directory**, tıklayın **uygulama kayıtları**, uygulamanın açın **ayarları** sayfasında ve 'ıtıklatın. **Özellikleri**. Gibi bir dış kaynağa olabilir `https://graph.microsoft.com`. Bu, yetkilendirme veya belirteç isteklerini birinde gereklidir. Daha az kimlik doğrulaması sağlamak için istemleri onay kullanıcıdan alınan emin olmak için yetkilendirme isteği yerleştirin. |
| kapsam | **yoksayıldı** | V1 Azure AD uygulamaları için kapsamları uygulamaları altında Azure portalında statik olarak yapılandırılmalıdır **ayarları**, **gerekli izinler**. |
| istemi |isteğe bağlı |Gerekli olan kullanıcı etkileşimi türünü gösterir.<p> Geçerli değerler şunlardır: <p> *Oturum açma*: Kullanıcının yeniden kimlik doğrulamaya zorlayabilir sorulması gerekir. <p> *select_account*: Kullanıcıdan bir hesap seçmek için çoklu oturum açma üzerinde kesintiye uğratma istenir. Kullanıcının, var olan bir oturum açma hesabını seçin, hatırlanan bir hesabın kimlik bilgilerini girin veya tamamen farklı bir hesap kullanmak için seçin. <p> *Onay*: Kullanıcı onayı verildi, ancak güncelleştirilmesi gerekiyor. Kullanıcı onayı istenir. <p> *admin_consent*: Bir yönetici, kuruluş içindeki tüm kullanıcılar adına kabul sorulması gerekir |
| login_hint |isteğe bağlı |Önceden, kullanıcı adını biliyorsanız, kullanıcı için oturum açma sayfası kullanıcı adı/e-posta adresi alanının önceden doldurmak için kullanılabilir. Uygulamalar genellikle kullanıcı adı önceki oturum açma kullanarak bir zaten ayıklanan yeniden kimlik doğrulaması sırasında bu parametreyi kullanın `preferred_username` talep. |
| domain_hint |isteğe bağlı |Kiracı veya kullanıcı, oturum açmak için kullanması gereken etki alanı ile ilgili bir ipucu sağlar. Kaydedilmiş bir etki alanı kiracının domain_hint değeridir. AAD Kiracı için bir şirket içi dizin birleştiriliyorsa belirtilen Kiracı federasyon sunucusuna yeniden yönlendirir. |
| code_challenge_method | Önerilen    | Kodlamak için kullanılan yöntemin `code_verifier` için `code_challenge` parametresi. Herhangi birini `plain` veya `S256`. Dışlanırsa, `code_challenge` düz metin olarak kabul edilir `code_challenge` dahildir. Azure AAD v1.0 destekler `plain` ve `S256`. Daha fazla bilgi için [PKCE RFC](https://tools.ietf.org/html/rfc7636). |
| code_challenge ile        | Önerilen    | Yetkilendirme kodu elde kavram anahtarı aracılığıyla kod Exchange (PKCE) için yerel veya genel bir istemciden güvenliğini sağlamak için kullanılır. Gerekli if `code_challenge_method` dahildir. Daha fazla bilgi için [PKCE RFC](https://tools.ietf.org/html/rfc7636). |

> [!NOTE]
> Kullanıcı, bir kuruluşun parçasıysa, kuruluş yöneticisi onayı veya kullanıcı adına reddedebilir veya kullanıcının onayını istemeniz izin. Kullanıcı seçeneği yalnızca yönetici izin verdiğinde izin verilir.
>
>

Bu noktada, kullanıcı kimlik bilgilerini girin ve Azure Portalı'nda uygulama tarafından istenen izinleri onay vermesi istenir. Kullanıcının kimliğini doğrular ve onayı veren sonra Azure AD uygulamanızı yanıt gönderir. `redirect_uri` isteğinizdeki kodla adresi.

### <a name="successful-response"></a>Başarılı yanıt
Başarılı bir yanıt şöyle görünebilir:

```
GET  HTTP/1.1 302 Found
Location: http://localhost:12345/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parametre | Açıklama |
| --- | --- |
| admin_consent |Değer True ise bir onay isteğinin bir yönetici tarafından onaylanan. |
| kod |Bu uygulama istendiğinde yetkilendirme kodu. Uygulama, hedef kaynağı için bir erişim belirteci istemek için yetkilendirme kodu kullanabilirsiniz. |
| session_state |Geçerli kullanıcı oturumunu tanımlayan benzersiz bir değerdir. Bu değer bir GUID'dir ancak İnceleme geçirilen genel olmayan bir değer olarak değerlendirilmesi gerektiğini. |
| durum |State parametresi istekte yer alıyorsa aynı değeri yanıt olarak görünmelidir. Yanıt kullanmadan önce state değerlerini istek ve yanıt özdeş olduğunu doğrulamak uygulama için iyi bir uygulamadır. Bu algılama yardımcı olur [siteler arası istek sahteciliği (CSRF) saldırılarını](https://tools.ietf.org/html/rfc6749#section-10.12) istemci karşı. |

### <a name="error-response"></a>Hata yanıtı
Hata yanıtları da gönderilebilir için `redirect_uri` uygulama bunları uygun şekilde işleyebilmeniz.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| hata |Bir hata kodu değeri bölüm 5.2 içinde tanımlanan [OAuth 2.0 yetkilendirme Framework](https://tools.ietf.org/html/rfc6749). Sonraki tabloda, Azure AD'ye verir hata kodları açıklanmaktadır. |
| error_description |Hatanın ayrıntılı bir açıklaması. Bu ileti olacak şekilde tasarlanmamıştır son kullanıcı dostu. |
| durum |İstekte gönderilen ve siteler arası istek sahteciliği (CSRF) saldırılarını önlemek için yanıtta döndürülen rastgele oluşturulmuş yeniden olmayan değeri durumu değerdir. |

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

## <a name="use-the-authorization-code-to-request-an-access-token"></a>Yetkilendirme kodu bir erişim belirteci istemek için kullanın
Bir yetkilendirme kodu edindiğiniz ve kullanıcı tarafından izin verilen göre bir POST isteği göndererek istenen kaynağa erişim belirteci kodunu kullanmak `/token` uç noktası:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%3A12345
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| tek |gerekli |`{tenant}` İstek yolunda değer, uygulamaya oturum denetimi için kullanılabilir. Kiracı tanımlayıcıları, örneğin, izin verilen değerler: `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` veya `contoso.onmicrosoft.com` veya `common` Kiracı bağımsız belirteçleri |
| client_id |gerekli |Azure AD ile kaydettiğinizde, uygulamanıza atanan uygulama kimliği. Bunu Azure portalında bulabilirsiniz. Uygulama Kimliği uygulama kaydını ayarlarında görüntülenir. |
| grant_type değeri |gerekli |Olmalıdır `authorization_code` yetkilendirme kod akışı için. |
| kod |gerekli |`authorization_code` Önceki bölümde aldığınız |
| redirect_uri |gerekli | A `redirect_uri`istemci uygulamasında kayıtlı. |
| client_secret |Genel istemciler için izin verilmiyor, web uygulamaları için gerekli |Azure Portal'da uygulamanızın altında oluşturduğunuz uygulama gizli anahtarı **anahtarları**. Client_secrets güvenilir bir şekilde cihazlarda depolanan olamaz çünkü yerel bir uygulamada (ortak istemci) kullanılamaz. Web uygulamaları ve web API'leri (tüm gizli istemciler) sahip depolama yeteneği için gerekli olan `client_secret` sunucu tarafında güvenli bir şekilde. Client_secret gönderilmeden önce URL kodlamalı olmalıdır. |
| kaynak | Önerilen |Hedef web API (kaynak güvenli) uygulama kimliği URI'si. Uygulama Kimliği URI'si, Azure Portalı'nda bulmak için tıklatın **Azure Active Directory**, tıklayın **uygulama kayıtları**, uygulamanın açın **ayarları** sayfasında ve 'ıtıklatın. **Özellikleri**. Gibi bir dış kaynağa olabilir `https://graph.microsoft.com`. Bu, yetkilendirme veya belirteç isteklerini birinde gereklidir. Daha az kimlik doğrulaması sağlamak için istemleri onay kullanıcıdan alınan emin olmak için yetkilendirme isteği yerleştirin. Yetkilendirme isteği hem de kaynak belirteci isteği ise ' parametreleri eşleşmesi gerekir. | 
| code_verifier | isteğe bağlı | Authorization_code elde etmek için kullanılan aynı code_verifier. PKCE bir yetkilendirme kodu verme istekte kullanılan gereklidir. Daha fazla bilgi için [PKCE RFC](https://tools.ietf.org/html/rfc7636)   |

Uygulama Kimliği URI'si, Azure Portalı'nda bulmak için tıklatın **Azure Active Directory**, tıklayın **uygulama kayıtları**, uygulamanın açın **ayarları** sayfasında ve 'ıtıklatın. **Özellikleri**.

### <a name="successful-response"></a>Başarılı yanıt
Azure AD döndürür bir [erişim belirteci](access-tokens.md) başarılı bir yanıt temel alır. Ağ çağrıları istemci uygulaması ve bunların ilişkili gecikme süresini en aza indirmek için istemci uygulaması önbellek erişim belirteçleri OAuth 2.0 yanıtta belirtilen belirteç ömrü. Belirteç süresini belirlemek için kullanın `expires_in` veya `expires_on` parametre değerleri.

Bir web API'si kaynağına döndürürse bir `invalid_token` hata kodu, bu gösterebilir kaynak belirtecin süresinin dolduğunu algıladı. İstemci ve kaynak saatleri farklı (bir "zaman eğimi" olarak bilinir), kaynak belirteci istemci önbelleğinden temizlenmeden önce süresi dolmuş belirteç göz önünde bulundurabilirsiniz. Bu meydana gelirse, yine de hesaplanan yaşam süresi içinde olsa bile, önbellekten belirteç temizleyin.

Başarılı bir yanıt şöyle görünebilir:

```
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
  "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ."
}

```

| Parametre | Açıklama |
| --- | --- |
| access_token |İstenen [erişim belirteci](access-tokens.md) olarak bir imzalı JSON Web Token (JWT). Uygulama, web API'si gibi güvenli kaynağına kimliğini doğrulamak için bu belirteci kullanabilirsiniz. |
| token_type |Belirteç türü değeri gösterir. Azure AD destekleyen tek taşıyıcı türüdür. Taşıyıcı belirteçleri hakkında daha fazla bilgi için bkz: [OAuth2.0 yetkilendirme Framework: Bearer Token Usage (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt) |
| expires_in |Ne kadar süreyle erişim belirteci (saniye olarak) geçerli değil. |
| expires_on |Erişim belirtecinin süresinin sona erdiği zaman. Tarih 1970'ten saniye sayısı temsil edilen-01-kadar süre sonu UTC 01T0:0:0Z. Bu değer, önbelleğe alınan belirteç ömrünü belirlemek için kullanılır. |
| kaynak |Uygulama Kimliği URI'si, web API (kaynak güvenli). |
| kapsam |Kimliğe bürünme, istemci uygulamaya verilen izinler. Varsayılan izin `user_impersonation`. Güvenli kaynağın sahibi ek değerler Azure AD'ye kaydedebilir. |
| refresh_token |OAuth 2.0 yenileme belirteci. Uygulama geçerli erişim belirtecinin süresi dolduktan sonra ek erişim belirteçlerini almak için bu belirteci kullanabilirsiniz. Yenileme belirteçleri uzun süreli ve uzun süre için kaynaklarına erişimi korumak için kullanılabilir. |
| id_token |JSON Web Token (JWT) işaretsiz bir temsil eden bir [kimlik belirteci](id-tokens.md). Uygulama can base64Url isteğini açan kullanıcı hakkında bilgi için bu belirteci parçalarını kodunu çözer. Uygulama değerleri önbelleğe ve bunları görüntüleyebilirsiniz, ancak, bunlar üzerinde herhangi bir yetkilendirme veya güvenlik sınırları için doğrulamamalısınız. |

JSON web belirteçlerini hakkında daha fazla bilgi için bkz: [JWT IETF taslak belirtimi](https://go.microsoft.com/fwlink/?LinkId=392344).   Hakkında daha fazla bilgi edinmek için `id_tokens`, bkz: [v1.0 Openıd Connect akışı](v1-protocols-openid-connect-code.md).

### <a name="error-response"></a>Hata yanıtı
İstemci belirteci verme uç noktası doğrudan çağırdığı belirteç yayınında uç noktası HTTP hata kodları hatalardır. HTTP durum kodu yanı sıra, Azure AD belirteç yayınında uç noktası da nesnelerle hatayı açıklayan bir JSON belgesini döndürür.

Bir örnek hata yanıtı şuna benzer:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parametre | Açıklama |
| --- | --- |
| hata |Oluşan hataları türlerini sınıflandırmak için kullanılabilir ve hatalara tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |
| error_codes |Tanılama'da yardımcı olabilecek özel STS hata kodlarının listesi. |
| timestamp |Hatanın gerçekleştiği zaman. |
| trace_id |Tanılama'da yardımcı olabilecek talebi için benzersiz bir tanımlayıcı. |
| correlation_id |Tanılama'da bileşenlerinde yardımcı olabilecek isteği için benzersiz bir tanımlayıcı. |

#### <a name="http-status-codes"></a>HTTP durum kodları
Aşağıdaki tabloda, belirteç yayınında uç noktanın döndürdüğü HTTP durum kodları listeler. Bazı durumlarda, hata kodu yanıt tanımlamak yeterlidir ancak hatalar varsa eşlik eden JSON belgesini ayrıştırmak ve hata kodunu incelemek gerekir.

| HTTP kodu | Açıklama |
| --- | --- |
| 400 |Varsayılan HTTP kodu. Çoğu durumda kullanılan ve genellikle bir hatalı istek nedeniyle. Düzeltin ve isteği yeniden gönderin. |
| 401 |Kimlik doğrulama başarısız oldu. Örneğin, istek client_secret parametresi eksik. |
| 403 |Yetkilendirme başarısız oldu. Örneğin, kullanıcının kaynağa erişim izni yok. |
| 500 |Hizmeti bir iç hata oluştu. İsteği yeniden deneyin. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Belirteç uç noktası hataları için hata kodları
| Hata Kodu | Açıklama | İstemci eylemi |
| --- | --- | --- |
| invalid_request |Gerekli parametre eksik gibi protokol hatası. |Düzeltin ve isteği yeniden gönderin |
| invalid_grant |Yetkilendirme kodu geçersiz veya süresi doldu. |Yeni bir istek deneyin `/authorize` uç noktası |
| unauthorized_client |Kimliği doğrulanmış istemci bu yetkilendirme verme türünü kullanmaya yetkili değil. |Bu durum genellikle, istemci uygulaması, Azure AD'de kayıtlı değil veya kullanıcının Azure AD kiracısına eklenmez genellikle ortaya çıkar. Uygulama, uygulama yükleme ve Azure AD'ye ekleme yönerge kullanıcıyla isteyebilir. |
| invalid_client |İstemci kimlik doğrulaması başarısız oldu. |İstemci kimlik bilgileri geçerli değil. Düzeltmek için uygulama yöneticisinin kimlik bilgilerini güncelleştirir. |
| unsupported_grant_type |Yetkilendirme sunucusu yetkilendirme verme türünü desteklemiyor. |İstek verme türünü değiştirin. Bu tür yalnızca geliştirme sırasında gerçekleşmesi gerektiğini ve ilk test sırasında algılandı. |
| invalid_resource |Hedef kaynak yoksa, Azure AD, bulunamıyor veya doğru şekilde yapılandırılmadığı için geçersiz. |Bu, varsa, kaynak kiracıda yapılandırılmadı gösterir. Uygulama, uygulama yükleme ve Azure AD'ye ekleme yönerge kullanıcıyla isteyebilir. |
| interaction_required |İstek, kullanıcı etkileşimi gerektirir. Örneğin, bir ek kimlik doğrulama adım gereklidir. | Yerine etkileşimli olmayan bir istek, aynı kaynak için etkileşimli yetkilendirme isteği ile yeniden deneyin. |
| temporarily_unavailable |Sunucunun geçici olarak isteği işleyemeyecek kadar meşgul. |İsteği yeniden deneyin. İstemci uygulama, kullanıcıya, yanıtına bir geçici koşul nedeniyle ertelendi açıklayabilir. |

## <a name="use-the-access-token-to-access-the-resource"></a>Kaynağa erişmek için erişim belirteci kullanın
Başarıyla alındı göre bir `access_token`, içine ekleyerek Web API'leri, isteklerinde belirteci kullanabilirsiniz `Authorization` başlığı. [RFC 6750](https://www.rfc-editor.org/rfc/rfc6750.txt) belirtimi taşıyıcı belirteçleri HTTP isteklerini korunan kaynaklara erişim için nasıl kullanılacağını açıklar.

### <a name="sample-request"></a>Örnek istek
```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>Hata yanıtı
RFC 6750 sorunu HTTP durum kodları uygulayan güvenli kaynaklar. İstek belirteci eksik veya kimlik doğrulama bilgilerini içermez, yanıt içeriyor. bir `WWW-Authenticate` başlığı. Bir istek başarısız olduğunda, kaynak sunucuda bir hata kodu ile HTTP durum kodu ile yanıt verir.

İstemci isteği taşıyıcı belirteç içermiyor, başarısız bir yanıt örneği verilmiştir:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.microsoftonline.com/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>Hata parametreleri
| Parametre | Açıklama |
| --- | --- |
| authorization_uri |URI'si (fiziksel uç nokta) yetkilendirme sunucusu. Bu değer, bulma uç noktasından sunucusu hakkında daha fazla bilgi almak için arama anahtarı olarak da kullanılır. <p><p> İstemci, yetkilendirme sunucusunun güvenilir olduğunu doğrulamanız gerekir. Kaynak Azure AD tarafından korunmaya başladıktan sonra URL ile başlayan doğrulamak yeterli https://login.microsoftonline.com veya Azure AD destekleyen başka bir ana bilgisayar adı. Bir kiracıya özgü kaynak her zaman bir kiracıya özgü yetkilendirme URI döndürmelidir. |
| hata |Bir hata kodu değeri bölüm 5.2 içinde tanımlanan [OAuth 2.0 yetkilendirme Framework](https://tools.ietf.org/html/rfc6749). |
| error_description |Hatanın ayrıntılı bir açıklaması. Bu ileti olacak şekilde tasarlanmamıştır son kullanıcı dostu. |
| resource_id |Kaynağın benzersiz tanımlayıcısını döndürür. İstemci uygulaması bu tanıtıcıyı değeri olarak kullanabilirsiniz `resource` kaynak için bir belirteç isteğinde bulunduğunda parametre. <p><p> Bu değeri doğrulamak istemci uygulaması için önemlidir, aksi takdirde, kötü amaçlı bir hizmete anlamına mümkün olabilir bir **ayrıcalıklar yükseltme** saldırı <p><p> Doğrulamak için bir saldırı engelleme için önerilen strateji olduğunu `resource_id` , erişilen web API URL'si temeli eşleşir. Örneğin, varsa https://service.contoso.com/data erişiliyor, `resource_id` htttps://service.contoso.com/ olabilir. İstemci uygulaması Reddet gerekir bir `resource_id` , değil başlamak temel URL ile olmadığı sürece kimliğini doğrulamak için güvenilir bir alternatif yolu. |

#### <a name="bearer-scheme-error-codes"></a>Taşıyıcı düzeni hata kodları
RFC 6750 belirtimi taşıyıcı düzeni ve WWW-Authenticate üstbilgisi yanıt olarak kullandığınız kaynaklar için aşağıdaki hataları tanımlar.

| HTTP durum kodu | Hata Kodu | Açıklama | İstemci eylemi |
| --- | --- | --- | --- |
| 400 |invalid_request |İstek doğru biçimlendirilmemiş. Örneğin, dosya bir parametre eksik veya aynı parametre iki kez kullanma. |Hatayı düzeltin ve isteği yeniden deneyin. Bu tür, yalnızca geliştirme sırasında gerçekleşmesi gerektiğini ve ilk test algılandı. |
| 401 |invalid_token |Erişim belirteci eksik, geçersiz veya iptal edildi. Error_description parametresinin değerini, ek ayrıntılı bilgi verilmiştir. |Yetkilendirme sunucusundan yeni bir belirteç isteyin. Yeni belirteç başarısız olursa, beklenmeyen bir hata oluştu. Bir hata iletisi kullanıcı ve yeniden deneyin, sonra rastgele gecikmeler gönderin. |
| 403 |insufficient_scope |Erişim belirteci kaynağa erişmek için gereken kimliğe bürünme izinleri içermiyor. |Yetkilendirme uç noktasına yeni bir yetkilendirme isteği gönderin. Yanıt kapsam parametresi içeriyorsa, kaynak isteğinde kapsam değeri kullanın. |
| 403 |insufficient_access |Belirtecin konu kaynağa erişmek için gerekli izinlere sahip değil. |Farklı bir hesap kullanın veya belirtilen kaynak için izin istemek için kullanıcıya sor |

## <a name="refreshing-the-access-tokens"></a>Erişim belirteçlerini yenileme

Erişim belirteçleri kısa ömürlüdür ve kaynaklara erişmeye devam etmek için süresi dolduktan sonra yenilenmesi gerekiyor. Yenileme işlemini `access_token` başka göndererek `POST` isteği `/token` uç noktası, ancak bu zaman sağlama `refresh_token` yerine `code`.  Yenileme belirteçleri tüm geçerli istemci verilen kaynaklar onay erişmek için - böylece bir talebi veren bir yenileme belirteci `resource=https://graph.microsoft.com` için yeni bir erişim belirteci istemek için kullanılan `resource=https://contoso.com/api`. 

Yenileme belirteçleri belirtilen ömre sahip değil. Genellikle, yenileme belirteçleri ömrü oldukça uzun olabilir. Ancak, bazı durumlarda, yenileme belirteçleri sona, iptal edilen veya istenen eylemi için yeterli ayrıcalıkları yok. Beklediğiniz ve doğru belirteci verme uç noktası tarafından döndürülen hataları işlemek uygulamanız gerekir.

Bir yenileme belirteci hatası ile bir yanıt aldığınızda, geçerli bir yenileme belirteci iptal ve yeni bir yetkilendirme kodu isteyin veya erişim belirteci. Özellikle, ne zaman kullanarak bir yenileme belirteci yetkilendirme kodu verme akışı ile bir yanıt alıyorsanız `interaction_required` veya `invalid_grant` hata kodları, yenileme belirtecini atmak ve yeni bir yetkilendirme kodu istek.

Örnek istek **kiracıya özgü** uç noktası (Ayrıca **ortak** uç nokta) yeni bir erişim almak için bir yenileme belirteci kullanılarak belirteç şuna benzer:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

### <a name="successful-response"></a>Başarılı yanıt
Başarılı bir token yanıt şöyle görünecektir:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```
| Parametre | Açıklama |
| --- | --- |
| token_type |Belirteç türü. Desteklenen tek değer: **taşıyıcı**. |
| expires_in |Belirteç kalan kullanım ömrünü saniye. 3600 (bir saat) buna tipik bir değerdir. |
| expires_on |Tarih ve saat, belirtecin süresi dolar. Tarih 1970'ten saniye sayısı temsil edilen-01-kadar süre sonu UTC 01T0:0:0Z. |
| kaynak |Erişim belirteci olabilir güvenli kaynağı tanımlayan erişimi için kullanıldı. |
| kapsam |Yerel istemci uygulaması için kimliğe bürünme izinler. Varsayılan izin **user_impersonation**. Diğer değerleri, hedef kaynağın sahibi Azure AD'de kaydedebilirsiniz. |
| access_token |İstenen yeni bir erişim belirteci. |
| refresh_token |Bu yanıt süresi dolduğunda yeni erişim belirteçlerini istemek için kullanılan yeni bir OAuth 2.0 refresh_token. |

### <a name="error-response"></a>Hata yanıtı
Bir örnek hata yanıtı şuna benzer:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872. This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant. You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Parametre | Açıklama |
| --- | --- |
| hata |Oluşan hataları türlerini sınıflandırmak için kullanılabilir ve hatalara tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |
| error_codes |Tanılama'da yardımcı olabilecek özel STS hata kodlarının listesi. |
| timestamp |Hatanın gerçekleştiği zaman. |
| trace_id |Tanılama'da yardımcı olabilecek talebi için benzersiz bir tanımlayıcı. |
| correlation_id |Tanılama'da bileşenlerinde yardımcı olabilecek isteği için benzersiz bir tanımlayıcı. |

Hata kodlarını ve önerilen istemci eylemi açıklaması için bkz: [belirteç uç noktası hataları için hata kodlarını](#error-codes-for-token-endpoint-errors).
