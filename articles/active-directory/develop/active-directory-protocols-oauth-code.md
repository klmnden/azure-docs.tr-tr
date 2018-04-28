---
title: Azure AD'de OAuth 2.0 yetkilendirme kodu akışını anlama
description: Bu makalede, web uygulamaları ve web API'leri Azure Active Directory ve OAuth 2.0 kullanarak kiracınızda erişim yetkisi vermek için HTTP iletileri kullanmayı açıklar.
services: active-directory
documentationcenter: .net
author: hpsin
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: hirsin
ms.custom: aaddev
ms.openlocfilehash: d2a160d75f89768a3884beff9ea10cbc168d3dda
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="authorize-access-to-azure-active-directory-web-applications-using-the-oauth-20-code-grant-flow"></a>OAuth 2.0 kod grant akışını kullanarak Azure Active Directory web uygulamalarına erişim yetkisi
Web uygulamaları ve web Apı'lerinize Azure AD kiracınıza erişim yetkisi vermek etkinleştirmek için azure Active Directory (Azure AD) kullanan OAuth 2.0. Bu kılavuz dilden bağımsızdır ve herhangi birini kullanmadan HTTP iletileri almasına ve göndermesine açıklar bizim [açık kaynak kitaplıkları](active-directory-authentication-libraries.md).

OAuth 2.0 yetkilendirme kodu akışını açıklanan [4.1 OAuth 2.0 belirtimi bölüm](https://tools.ietf.org/html/rfc6749#section-4.1). Web uygulamaları dahil olmak üzere birçok uygulama türü, kimlik doğrulama ve yetkilendirme gerçekleştirmek için kullanılır ve yerel olarak yüklü uygulamalar.

[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)]

## <a name="oauth-20-authorization-flow"></a>OAuth 2.0 yetkilendirme akışı
Yüksek bir düzeyde bir uygulama için tüm yetkilendirme akışı biraz şöyle:

![OAuth kimlik doğrulama kodu akışı](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)

## <a name="request-an-authorization-code"></a>İstek bir kimlik doğrulama kodu
Yetkilendirme kodu akışını kullanıcıya yönlendirerek istemci ile başlayan `/authorize` uç noktası. Bu istekte istemcisi kullanıcıdan almak için gerekli olan izinleri belirtir. OAuth 2.0 yetkilendirme uç noktası seçerek kiracınız için elde edebilirsiniz **uygulama kayıtlar > uç noktaları** Azure portalında.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%3A%12345
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| kiracı |Gerekli |`{tenant}` İstek yolunu değerinde uygulamasına oturum denetlemek için kullanılabilir.  İzin verilen değerler Kiracı, örneğin, tanımlayıcılardır `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` veya `contoso.onmicrosoft.com` veya `common` Kiracı bağımsız belirteçleri |
| client_id |Gerekli |Azure AD ile kaydolurken uygulamanıza atanan uygulama kimliği. Bu Azure Portalı'nda bulabilirsiniz. Tıklatın **Azure Active Directory** Hizmetleri Kenar Çubuğu'nda tıklatın **uygulama kayıtlar**ve uygulamayı seçin. |
| response_type |Gerekli |İçermelidir `code` yetkilendirme kodu akışı. |
| redirect_uri |Önerilen |Burada kimlik doğrulama yanıtları gönderilebilen veya uygulamanız tarafından alınan, uygulamanızın redirect_uri.  Bu tam bir url kodlanmış olmalıdır dışında Portalı'nda kayıtlı redirect_uris eşleşmelidir.  Yerel & mobil uygulamaları için varsayılan değeri kullanması gereken `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode |Önerilen |Sonuçta elde edilen belirteci geri uygulamanıza göndermek için kullanılacak yöntemi belirtir.  `query` veya `form_post` olabilir. `query` bir sorgu dizesi parametresi üzerinde yeniden yönlendirme URI'si olarak kodu sağlar ancak `form_post` , yeniden yönlendirme URI'sine kodu içeren bir yürütür. |
| durum |Önerilen |Ayrıca belirteci yanıtta döndürülen istek dahil bir değer. Rastgele oluşturulan benzersiz bir değer tipik olarak kullanılan [siteler arası istek sahteciliğini saldırılarını önleme](http://tools.ietf.org/html/rfc6749#section-10.12).  Durumu, sayfa veya görünüm üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce uygulama kullanıcının durumu hakkındaki bilgileri kodlamak için de kullanılır. |
| kaynak | Önerilen |Hedef web API (güvenli kaynak) uygulama kimliği URI'si. Uygulama Kimliği URI'sini Azure Portalı'nda bulmak için tıklatın **Azure Active Directory**, tıklatın **uygulama kayıtlar**, uygulamanın açmak **ayarları** sayfasında ve 'itıklatın **Özellikler**. Gibi harici bir kaynak olabilir `https://graph.microsoft.com`.  Bu, yetkilendirme veya belirteç isteklerini birinde gereklidir.  Daha az kimlik doğrulaması sağlamak için istemleri, kullanıcıdan izin alınan emin olmak için yetkilendirme isteği yerleştirin. |
| scope | **göz ardı** | V1 Azure AD uygulamalarınız için kapsamları uygulamaları altında Azure portalındaki statik olarak yapılandırılmalıdır **ayarları**, **gerekli izinler**. |
| istemi |isteğe bağlı |Gerekli bir kullanıcı etkileşimi türünü gösterir.<p> Geçerli değerler şunlardır: <p> *oturum açma*: kullanıcının yeniden kimlik doğrulaması yapmak istenir. <p> *onay*: kullanıcı izni verildi, ancak güncelleştirilmesi gerekiyor. Kullanıcının onaylaması istenir. <p> *admin_consent*: bir yönetici kendi kuruluşunuzdaki tüm kullanıcılar adına onaylaması istenir |
| login_hint |isteğe bağlı |Kullanıcı adlarını önceden biliyorsanız, kullanıcı için oturum açma sayfası kullanıcı adı/e-posta adresi alanının önceden doldurmak için kullanılabilir.  Genellikle uygulamaları yeniden kimlik doğrulaması, kullanıcı adı önceki oturum açma kullanarak bir zaten ayıklanan sırasında bu parametreyi kullanın `preferred_username` talep. |
| domain_hint |isteğe bağlı |Kiracı veya kullanıcı oturum açmak için kullanması gereken etki alanını hakkında bir ipucu sağlar. Kiracı için kaydedilmiş bir etki alanı domain_hint değeri. Bir şirket içi dizin için Kiracı birleştirildiyse AAD belirtilen Kiracı federasyon sunucusuna yeniden yönlendirir. |
| code_challenge_method | isteğe bağlı    | Kodlanması için kullanılan yöntem `code_verifier` için `code_challenge` parametresi. Aşağıdakilerden biri olabilir `plain` veya `S256`.  Dışlanan, `code_challenge` düz metin olarak kabul edilir `code_challenge` bulunur.  Azure AAD v1.0 destekler `plain` ve `S256`. Daha fazla bilgi için bkz: [PKCE RFC](https://tools.ietf.org/html/rfc7636). |
| code_challenge        | isteğe bağlı    | Yetkilendirme kodu verir kanıt anahtarı aracılığıyla kodu Exchange (PKCE) için bir yerel veya genel istemciden güvenliğini sağlamak için kullanılır. Gerekli olursa `code_challenge_method` bulunur.  Daha fazla bilgi için bkz: [PKCE RFC](https://tools.ietf.org/html/rfc7636). |

> [!NOTE]
> Kullanıcı kuruluşun parçası ise, kuruluş yöneticisi onayı veya kullanıcı adına reddetmek ya kullanıcının onayını istemeniz izin verir. Kullanıcı seçeneği yalnızca yönetici izin verdiğinde izin verilir.
>
>

Bu noktada, kullanıcı kimlik bilgilerini girin ve Azure Portalı'nda uygulama tarafından istenen izinler için izin istenir. Kullanıcı kimliğini doğrular ve izin veren sonra Azure AD uygulamanızı yanıt gönderir `redirect_uri` isteğiniz koduyla adresi.

### <a name="successful-response"></a>Başarılı yanıt
Başarılı yanıt şöyle:

```
GET  HTTP/1.1 302 Found
Location: http://localhost:12345/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parametre | Açıklama |
| --- | --- |
| admin_consent |Değeri True ise bir yönetici onayı isteğinin rıza. |
| kod |İstenen uygulama yetkilendirme kodu. Uygulama, hedef kaynak için bir erişim belirteci istemek için yetkilendirme kodu kullanabilirsiniz. |
| session_state |Geçerli kullanıcı oturumunu tanımlayan benzersiz bir değerdir. Bu değer bir GUID olduğundan ancak İnceleme geçirilen genel olmayan bir değer olarak değerlendirilmelidir. |
| durum |İstekte bir durum parametresi eklenirse, aynı değeri yanıt olarak görünmelidir. Yanıt kullanmadan önce istek ve yanıt durum değerleri özdeş olduğunu doğrulamak uygulama için iyi bir uygulamadır. Bu algılamaya yardımcı olur [siteler arası istek sahteciliği (CSRF) saldırılarını](https://tools.ietf.org/html/rfc6749#section-10.12) istemci karşı. |

### <a name="error-response"></a>Hata yanıtı
Hata yanıtları da gönderilebilir için `redirect_uri` böylece uygulama bunları uygun şekilde işleyebilir.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| error |Bir hata kodu değeri bölüm 5.2 içinde tanımlanan [OAuth 2.0 yetkilendirme Framework](http://tools.ietf.org/html/rfc6749). Sonraki tabloda Azure AD döndürür hata kodları açıklanmaktadır. |
| error_description |Hatanın ayrıntılı bir açıklaması. Bu ileti olması amaçlanmamıştır son kullanıcı dostu. |
| durum |Durum değeri istekte gönderilen ve siteler arası istek sahtekarlığı (CSRF) saldırılarını önlemek için yanıtta döndürülen bir rastgele oluşturulmuş yeniden olmayan değerdir. |

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

## <a name="use-the-authorization-code-to-request-an-access-token"></a>Bir erişim belirteci istemek için yetkilendirme kodu kullanın
Bir yetkilendirme kodu edindiğiniz ve kullanıcı tarafından izin verilen göre bir POST isteği göndererek istenen kaynak için bir erişim belirteci kodunu kullanmak `/token` uç noktası:

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
| kiracı |Gerekli |`{tenant}` İstek yolunu değerinde uygulamasına oturum denetlemek için kullanılabilir.  İzin verilen değerler Kiracı, örneğin, tanımlayıcılardır `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` veya `contoso.onmicrosoft.com` veya `common` Kiracı bağımsız belirteçleri |
| client_id |Gerekli |Azure AD ile kaydolurken uygulamanıza atanan uygulama kimliği. Bu Azure portalında bulabilirsiniz. Uygulama kimliği, uygulama kaydı ayarlarında görüntülenir.  |
| grant_type |Gerekli |Olmalıdır `authorization_code` yetkilendirme kodu akışı. |
| kod |Gerekli |`authorization_code` Önceki bölümde edindiğiniz |
| redirect_uri |Gerekli |Aynı `redirect_uri` almak için kullanılan değer `authorization_code`. |
| client_secret |web uygulamaları, ortak istemciler için izin verilmiyor gerekli |Azure Portalı'nda altında uygulamanız için oluşturduğunuz uygulama gizli anahtarı **anahtarları**.  Client_secrets güvenilir bir şekilde cihazlarda depolanamaz çünkü yerel bir uygulamada (ortak istemci) kullanılamaz.  Web uygulamaları ve web API'leri (tüm gizli istemciler), depolama yeteneği olan gereklidir `client_secret` sunucu tarafında güvenli bir şekilde. |
| kaynak | Önerilen |Hedef web API (güvenli kaynak) uygulama kimliği URI'si. Uygulama Kimliği URI'sini Azure Portalı'nda bulmak için tıklatın **Azure Active Directory**, tıklatın **uygulama kayıtlar**, uygulamanın açmak **ayarları** sayfasında ve 'itıklatın **Özellikler**. Gibi harici bir kaynak olabilir `https://graph.microsoft.com`.  Bu, yetkilendirme veya belirteç isteklerini birinde gereklidir.  Daha az kimlik doğrulaması sağlamak için istemleri, kullanıcıdan izin alınan emin olmak için yetkilendirme isteği yerleştirin.  Yetkilendirme isteği ve belirteç isteği, kaynak If' parametreleri aynı olmalıdır. | 
| code_verifier | isteğe bağlı | Authorization_code elde etmek için kullanılan aynı code_verifier.  PKCE yetkilendirme kodu verme istekte kullanılan ise gereklidir.  Daha fazla bilgi için bkz: [PKCE RFC](https://tools.ietf.org/html/rfc7636)   |

Uygulama Kimliği URI'sini Azure Portalı'nda bulmak için tıklatın **Azure Active Directory**, tıklatın **uygulama kayıtlar**, uygulamanın açmak **ayarları** sayfasında ve 'itıklatın **Özellikler**.

### <a name="successful-response"></a>Başarılı yanıt
Azure AD, bir erişim belirteci üzerine başarılı bir yanıt döndürür. Ağ çağrılarından istemci uygulaması ve bunların ilişkili gecikme süresi en aza indirmek için istemci uygulaması erişim belirteçleri OAuth 2.0 yanıt olarak belirtilen belirteç ömrü için önbelleğe. Belirteç ömrü belirlemek için kullanın ya da `expires_in` veya `expires_on` parametre değerleri.

Bir web API kaynak döndürürse bir `invalid_token` hata kodu, bu gösterebilir kaynak belirtecin süresinin dolduğunu algıladı. İstemci ve kaynak saatleri farklı (bir "saat eğriltme" olarak bilinir) varsa, kaynak belirteci istemci önbelleğinden temizlenmeden önce süresinin belirteç düşünebilirsiniz. Bu meydana gelirse, hala hesaplanan yaşam süresi içinde olsa bile, önbellekten belirteç temizleyin.

Başarılı yanıt şöyle:

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
| access_token |İstenen erişim belirteci olarak imzalanmış JSON Web Token (JWT). Uygulama, web API'si gibi güvenli kaynak kimliğini doğrulamak için bu belirteci kullanabilirsiniz. |
| token_type |Belirteç türü değeri gösterir. Azure AD destekleyen tek taşıyıcı türüdür. Taşıyıcı belirteçlerini hakkında daha fazla bilgi için bkz: [OAuth2.0 yetkilendirme Framework: taşıyıcı belirteci kullanımı (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt) |
| expires_in |Ne kadar süreyle erişim belirteci (saniye olarak) geçerli değil. |
| expires_on |Erişim belirtecinin süresi dolduğunda süre. Tarih 1970'ten saniyeyi temsil edilir-01-01T0:0:0Z UTC sona erme zamanı kadar. Bu değer, önbelleğe alınan belirteç ömrü belirlemek için kullanılır. |
| kaynak |Web API (güvenli kaynak) uygulama kimliği URI'si. |
| scope |İstemci uygulaması için kimliğe bürünme izinler. Varsayılan izni `user_impersonation`. Güvenli kaynağın sahibi Azure AD içinde ek değerler kaydedebilirsiniz. |
| refresh_token |Bir OAuth 2.0 yenileme belirteci. Uygulama geçerli erişim belirtecinin süresi dolduktan sonra ek erişim belirteçleri almak için bu belirteci kullanabilirsiniz.  Yenileme belirteçleri uzun süreli ve uzun süre için kaynaklara erişimi korumak için kullanılabilir. |
| id_token |Bir imzasız JSON Web Token (JWT). Uygulama can base64Url bu belirteç için oturum kullanıcı hakkında bilgi parçalarını kodunu çözer. Uygulama değerleri önbelleğe ve bunları görüntüler, ancak, bunlar üzerinde herhangi bir yetkilendirme veya güvenlik sınırları için güvenmemelisiniz. |

### <a name="jwt-token-claims"></a>JWT belirteci talepleri
JWT belirteci değeri `id_token` parametresi aşağıdaki taleplerine kodu:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

JSON web belirteçlerini hakkında daha fazla bilgi için bkz: [JWT IETF taslak belirtimi](http://go.microsoft.com/fwlink/?LinkId=392344). Talep ve belirteç türleri hakkında daha fazla bilgi için okuma [desteklenen belirteç ve talep türleri](active-directory-token-and-claims.md)

`id_token` Parametresi aşağıdaki talep türlerini içerir:

| Talep türü | Açıklama |
| --- | --- |
| aud |Belirtecin hedef kitle. Belirtecin bir istemci uygulama, İzleyici olduğunda `client_id` istemcinin. |
| exp |Süre sonu zamanı. Belirtecin süresinin dolduğu saati. Belirtecin geçerli olması, geçerli tarih/saat veya buna eşit olmalıdır `exp` değeri. Saat 1 Ocak 1970'ten saniye sayısı olarak temsil edilir (1970'ten-01-01T0:0:0Z) UTC belirteci geçerlilik süresinin dolduğu zaman kadar.|
| family_name |Kullanıcının soyadı. Uygulama bu değeri görüntüleyebilirsiniz. |
| given_name |Kullanıcının adı. Uygulama bu değeri görüntüleyebilirsiniz. |
| IAT |Aynı anda verdi. JWT zaman verilmiş saat. Saat 1 Ocak 1970'ten saniye sayısı olarak temsil edilir (1970'ten-01-01T0:0:0Z) UTC belirteç yayımlandığında zamana kadar. |
| ISS |Belirteç Verenin tanımlar |
| NBF |Değil süreden önce. Belirteç etkin olduğunda süre. Belirtecin geçerli olması, geçerli tarih/saat Nbf değerine eşit veya daha büyük olmalıdır. Saat 1 Ocak 1970'ten saniye sayısı olarak temsil edilir (1970'ten-01-01T0:0:0Z) UTC belirteç yayımlandığında zamana kadar. |
| OID |Azure AD'de kullanıcı nesnesinin nesne tanımlayıcısı (ID). |
| Sub |Belirteç konu tanımlayıcısı. Bu bir kalıcı ve sabit belirteç açıklar kullanıcı tanımlayıcısıdır. Mantığı önbelleğe alma işleminde, bu değeri kullanın. |
| komutu |Belirtecin Azure AD Kiracı Kiracı tanımlayıcısını (ID). |
| unique_name |Kullanıcıya, için benzersiz bir tanımlayıcı görüntülenebilir. Genellikle bir kullanıcı asıl adı (UPN) budur. |
| UPN |Kullanıcının kullanıcı asıl adı. |
| ver |Sürüm. JWT belirteci, genellikle 1.0 sürümü. |

### <a name="error-response"></a>Hata yanıtı
Belirteç verme uç nokta hataları HTTP hata kodları, olduklarından istemci belirteci verme uç noktası doğrudan çağırır. HTTP durum kodu ek olarak, Azure AD belirteci verme endpoint de hatayı açıklayan nesnelerinin ile bir JSON belgesini döndürür.

Bir örnek hata yanıtı şuna:

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
| error |Oluşan hataları türlerini sınıflandırmak için kullanılan ve hataları tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |
| error_codes |Tanılamada yardımcı olabilecek STS özgü hata kodları listesi. |
| timestamp |Hatanın gerçekleştiği zaman. |
| trace_id |Tanılamada yardımcı olabilecek isteği için benzersiz bir tanımlayıcı. |
| correlation_id |Tanılamada bileşenlerinde yardımcı olabilecek isteği için benzersiz bir tanımlayıcı. |

#### <a name="http-status-codes"></a>HTTP durum kodları
Aşağıdaki tabloda, belirteç yayınında son noktasını döndürür HTTP durum kodları listelenmektedir. Bazı durumlarda, hata kodu yanıtı açıklamak yeterli olur, ancak hatalar varsa eşlik eden JSON belgesini ayrıştırma ve hata kodunun incelemek gerekir.

| HTTP kodu | Açıklama |
| --- | --- |
| 400 |Varsayılan HTTP kodu. Çoğu durumda kullanıldığı ve genellikle nedeniyle yanlış biçimli istek. Düzeltin ve isteği yeniden gönderin. |
| 401 |Kimlik doğrulama başarısız oldu. Örneğin, istek client_secret parametresi eksik. |
| 403 |Yetkilendirme başarısız oldu. Örneğin, kullanıcının kaynağa erişim izni yok. |
| 500 |Hizmeti bir iç hata oluştu. İsteği yeniden deneyin. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Belirteç uç noktası hataları için hata kodları
| Hata Kodu | Açıklama | İstemci eylemi |
| --- | --- | --- |
| invalid_request |Eksik gerekli parametre gibi protokol hatası. |Düzeltin ve isteği yeniden gönderin |
| invalid_grant |Yetkilendirme kodu geçersiz veya süresi doldu. |Yeni bir istek deneyin `/authorize` uç noktası |
| unauthorized_client |Kimliği doğrulanmış istemci bu yetkilendirme verme türünü kullanmak için yetkili değil. |Bu durum genellikle, istemci uygulaması Azure AD'de kayıtlı değil veya kullanıcının Azure AD kiracısı eklenmez genellikle ortaya çıkar. Uygulama yönerge Azure AD'ye ekleme ve uygulama yükleme için kullanıcıya sor |
| invalid_client |İstemci kimlik doğrulaması başarısız oldu. |İstemci kimlik bilgileri geçerli değil. Düzeltmek için uygulama Yöneticisi kimlik bilgilerini güncelleştirir. |
| unsupported_grant_type |Yetkilendirme sunucusu yetkilendirme verme türünü desteklemiyor. |İstekteki grant türünü değiştirin. Bu tür bir hata yalnızca geliştirme sırasında gerçekleşmesi gerektiğini ve ilk test sırasında algılandı. |
| invalid_resource |Hedef kaynak mevcut değil, Azure AD, bulunamıyor veya düzgün yapılandırılmamış olduğundan geçerli değil. |Bu, varsa, kaynak kiracısında yapılandırılmadı gösterir. Uygulama yönerge Azure AD'ye ekleme ve uygulama yükleme için kullanıcıya sor |
| interaction_required |İstek kullanıcı etkileşimi gerektirir. Örneğin, bir ek kimlik doğrulama adım gereklidir. | Etkileşimli olmayan bir istek yerine, aynı kaynak için etkileşimli yetkilendirme isteği ile yeniden deneyin. |
| temporarily_unavailable |Sunucunun geçici olarak istek işlemek için çok meşgul. |İsteği yeniden deneyin. İstemci uygulaması yanıt geçici bir koşul nedeniyle gecikti kullanıcıya açıklayabilir. |

## <a name="use-the-access-token-to-access-the-resource"></a>Kaynağa erişmek için erişim belirteci kullanın
Başarıyla edindiğiniz göre bir `access_token`, belirteç Web API ' larını isteklere dahil ederek kullanabileceğiniz `Authorization` üstbilgi. [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) belirtimi taşıyıcı belirteçlerini HTTP isteklerinde korunan kaynaklara erişim için nasıl kullanılacağını açıklar.

### <a name="sample-request"></a>Örnek istek
```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>Hata yanıtı
RFC 6750 sorunu HTTP durum kodları uygulamak güvenli kaynaklar. İstek kimlik doğrulama kimlik bilgileri içermiyor ya da belirteci eksik, yanıtı içeren bir `WWW-Authenticate` üstbilgi. Bir isteği başarısız olduğunda, kaynak sunucuda HTTP durum kodu ve bir hata kodu ile yanıt verir.

İstemci isteği taşıyıcı belirteci içermiyor, başarısız bir yanıt örneği verilmiştir:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.microsoftonline.com/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>Hata parametreleri
| Parametre | Açıklama |
| --- | --- |
| authorization_uri |URI'si (fiziksel uç noktası) yetkilendirme sunucusu. Bu değer, bir bulma uç noktasından sunucusu hakkında daha fazla bilgi için arama anahtarı olarak da kullanılır. <p><p> İstemci yetkilendirme sunucusunun güvenilir olduğunu doğrulamanız gerekir. Kaynak Azure AD tarafından korunuyorsa, URL ile başlayan doğrulamak yeterli https://login.microsoftonline.com veya Azure AD destekleyen başka bir ana bilgisayar adı. Bir kiracı özgü kaynak her zaman bir kiracı özel yetkilendirme URI döndürmelidir. |
| error |Bir hata kodu değeri bölüm 5.2 içinde tanımlanan [OAuth 2.0 yetkilendirme Framework](http://tools.ietf.org/html/rfc6749). |
| error_description |Hatanın ayrıntılı bir açıklaması. Bu ileti olması amaçlanmamıştır son kullanıcı dostu. |
| resource_id |Kaynak benzersiz tanımlayıcısını döndürür. İstemci uygulaması bu tanımlayıcı değeri olarak kullanabilirsiniz `resource` kaynak için bir belirteç istediğinde parametresi. <p><p> Bu değeri doğrulamak için istemci uygulaması için önemli, aksi takdirde kötü amaçlı bir hizmete anlamına olabilir bir **ayrıcalık yükseltme** saldırısı <p><p> Saldırının engellemek için önerilen doğrulamak için stratejidir `resource_id` erişilen web API'si URL tabanı eşleşen. Örneğin, varsa https://service.contoso.com/data erişiliyor, `resource_id` htttps://service.contoso.com/ olabilir. İstemci uygulaması Reddet gerekir bir `resource_id` , değil başlamak temel URL ile kimlik doğrulamak için güvenilir bir alternatif yolu değilse. |

#### <a name="bearer-scheme-error-codes"></a>Taşıyıcı düzeni hata kodları
RFC 6750 belirtimi WWW-Authenticate üstbilgisi ve taşıyıcı düzeni yanıtta kullanan kaynaklar için aşağıdaki hataları tanımlar.

| HTTP durum kodu | Hata Kodu | Açıklama | İstemci eylemi |
| --- | --- | --- | --- |
| 400 |invalid_request |İstek düzgün biçimlendirilmemiş. Örneğin, bu bir parametre eksik veya aynı parametre iki kez kullanma. |Hatayı düzeltin ve isteği yeniden deneyin. Bu tür bir hata yalnızca geliştirme sırasında gerçekleşmesi gerektiğini ve ilk testinde algılandı. |
| 401 |invalid_token |Erişim belirteci eksik, geçersiz veya iptal edilir. Error_description parametresinin değeri ek ayrıntılar sağlar. |Yeni bir belirteci yetkilendirme sunucusundan isteyin. Yeni bir belirteç başarısız olursa, beklenmeyen bir hata oluştu. Bir hata iletisi rastgele gecikme sonra yeniden deneyin ve kullanıcı için gönderin. |
| 403 |insufficient_scope |Erişim belirteci kaynağa erişmek için gerekli kimliğe bürünme izinlere sahip değil. |Yetkilendirme uç noktası için yeni bir yetkilendirme isteği gönderin. Yanıt kapsam parametresi içeriyorsa, kaynak isteğinde kapsam değeri kullanın. |
| 403 |insufficient_access |Belirteç konu kaynağa erişmek için gerekli izinlere sahip değil. |Farklı bir hesap kullanın veya belirtilen kaynak izni istemek için kullanıcıya sor. |

## <a name="refreshing-the-access-tokens"></a>Erişim belirteçleri yenileme
Erişim belirteçleri, kısa süreli ve kaynaklara erişmeye devam etmek için zaman aşımına uğradığında yenilenmesi gerekir. Yenileyebileceğiniz `access_token` başka gönderme tarafından `POST` isteği `/token` uç noktası, ancak bu süre sağlama `refresh_token` yerine `code`.

Yenileme belirteçleri belirtilen yaşam süresi yok. Genellikle, yenileme belirteçleri ömürleri oldukça uzun. Ancak, bazı durumlarda, yenileme belirteçleri sona, iptal edilen veya istenen eylem için yeterli ayrıcalıkları yok. Beklediğiniz ve doğru belirteci verme bitiş noktası tarafından döndürülen hataları işlemek uygulamanız gerekir.

Bir yenileme belirteci hatası ile bir yanıt aldığınızda, geçerli yenileme belirteci atmak ve yeni bir yetkilendirme kodu isteyin veya belirteç erişim. Özellikle, ne zaman bir yenileme kullanarak belirteci yetkilendirme kodu verme akışında yanıtta alırsanız `interaction_required` veya `invalid_grant` hata kodları, yenileme belirtecini atmak ve yeni bir yetkilendirme kodu isteyin.

Bir örnek isteğine **Kiracı özgü** uç noktası (de kullanabilirsiniz **ortak** uç nokta) yeni bir erişim almak için bir yenileme belirteci kullanarak belirteci şöyle görünür:

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
Başarılı bir token yanıt gibi görünür:

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
| token_type |Simge türü. Desteklenen tek değer: **taşıyıcı**. |
| expires_in |Saniye cinsinden kalan belirteç ömrü. Tipik bir değer 3600 (bir saat) ' dir. |
| expires_on |Tarih ve saat üzerinde belirtecinin süresi doluyor. Tarih 1970'ten saniyeyi temsil edilir-01-01T0:0:0Z UTC sona erme zamanı kadar. |
| kaynak |Erişim belirteci olabilir güvenli kaynağı tanımlayan erişmek için kullanılan. |
| scope |Yerel istemci uygulaması için kimliğe bürünme izinler. Varsayılan izni **user_impersonation**. Hedef kaynak sahibi Azure AD içinde alternatif değerler kaydedebilirsiniz. |
| access_token |İstenen yeni bir erişim belirteci. |
| refresh_token |Bu yanıt süresi dolduğunda yeni erişim belirteçleri istemek için kullanılan yeni bir OAuth 2.0 refresh_token. |

### <a name="error-response"></a>Hata yanıtı
Bir örnek hata yanıtı şuna:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant.  You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
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
| error |Oluşan hataları türlerini sınıflandırmak için kullanılan ve hataları tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |
| error_codes |Tanılamada yardımcı olabilecek STS özgü hata kodları listesi. |
| timestamp |Hatanın gerçekleştiği zaman. |
| trace_id |Tanılamada yardımcı olabilecek isteği için benzersiz bir tanımlayıcı. |
| correlation_id |Tanılamada bileşenlerinde yardımcı olabilecek isteği için benzersiz bir tanımlayıcı. |

Hata kodları ve önerilen istemci eylemi açıklaması için bkz: [belirteç uç noktası hataları için hata kodları](#error-codes-for-token-endpoint-errors).
