---
title: Azure AD v2.0 örtük akışını kullanarak tek sayfalı uygulamaları güvenli hale getirin | Microsoft Docs
description: Tek sayfalı uygulamalar için Azure AD'nin v2.0 uygulaması örtük akışını kullanarak web uygulamaları oluşturma.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 3605931f-dc24-4910-bb50-5375defec6a8
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 676e23f3136836975616865a9b9dc97605a97929
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866262"
---
# <a name="v20-protocols---spas-using-the-implicit-flow"></a>v2.0 protokolleri - örtük akışını kullanarak Spa'lar
V2.0 uç noktası ile kullanarak tek sayfalı uygulamanızla hem kişisel hem de iş/Okul hesapları Microsoft gelen kullanıcılar oturum açabilir. Tek sayfalı ve diğer JavaScript uygulamaları, öncelikle birkaç ilgi çekici sorunları, kimlik doğrulaması için söz konusu olduğunda bir tarayıcı yüz çalıştırın:

* Bu uygulamalar güvenlik özelliklerini geleneksel sunucu tabanlı web uygulamalarından önemli ölçüde farklılık gösterir.
* Çok sayıda yetkilendirme sunucularını & kimlik sağlayıcıları CORS isteklerini desteklemez.
* Tam sayfada tarayıcı kullanıcı deneyimiyle özellikle bozucu haline uygulama uzağa yeniden yönlendirir.

Bu uygulamalar için (düşünün: AngularJS, Ember.js React.js, vb.) Azure AD OAuth 2.0 örtülü izin akışını destekler. Örtük akış açıklanan [OAuth 2.0 belirtimini](http://tools.ietf.org/html/rfc6749#section-4.2). Bunun yararı belirteçleri Azure AD'den bir arka uç sunucusuna gerçekleştirmeden kimlik bilgisi alışverişinin ve uygulamayı vermesidir. Bu, kullanıcının oturumunu, oturumu korumak ve istemcideki tüm diğer web API'leri belirteçleri JavaScript kodu alma okumasına izin verir. Örtük akış - özellikle yaklaşık kullanırken dikkate almanız birkaç önemli güvenlik konuları vardır [istemci](http://tools.ietf.org/html/rfc6749#section-10.3) ve [kullanıcı kimliğe bürünme](http://tools.ietf.org/html/rfc6749#section-10.3).

Örtük akış ve Azure AD kimlik doğrulaması JavaScript uygulamanıza eklemek için kullanmak istiyorsanız, bizim açık kaynak JavaScript Kitaplığı kullanmanız önerilir [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js). Kullanılabilir birkaç AngularJS öğreticiler [burada](active-directory-appmodel-v2-overview.md#getting-started) başlamanıza yardımcı olmak için. 

Ancak, bir tek sayfalı uygulamanızı kitaplıkta kullanma ve kendinize iletişim kuralı iletileri göndermek tercih ederseniz, aşağıdaki genel adımları izleyin.

> [!NOTE]
> Tüm Azure Active Directory senaryolarını ve özelliklerini v2.0 uç noktası tarafından desteklenir. V2.0 uç noktası kullanıyorsanız belirlemek için aşağıdaki hakkında bilgi edinin: [v2.0 sınırlamaları](active-directory-v2-limitations.md).
> 
> 

## <a name="protocol-diagram"></a>Protokol diyagramı
Tüm örtük oturum açma akışı aşağıdaki gibi görünür - adımların her biri aşağıda ayrıntılı olarak açıklanmıştır.

![Openıd Connect Kulvarlar](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>Oturum açma isteği gönder
İlk kullanıcı, uygulamada oturum açması için size gönderebilir bir [Openıd Connect](active-directory-v2-protocols-oidc.md) yetkilendirme isteği ve get bir `id_token` v2.0 uç noktasından:

> [!IMPORTANT]
> Uygulama kaydı içinde bir kimlik belirteci başarıyla sırada istek [kayıt portalı](https://apps.dev.microsoft.com) olmalıdır **[örtük vermeyi](active-directory-v2-protocols-implicit.md)** Web istemcisi için etkin. Bunu etkin değilse, bir `unsupported_response` hata döndürülür: "'response_type' giriş parametresi için sağlanan değer bu istemci için izin verilmiyor. Beklenen değer 'kodu'."

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
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

| Parametre |  | Açıklama |
| --- | --- | --- |
| kiracı |gerekli |`{tenant}` İstek yolunda değer, uygulamaya oturum denetimi için kullanılabilir. İzin verilen değerler `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları. Daha fazla ayrıntı için [protokolü temel](active-directory-v2-protocols.md#endpoints). |
| client_id |gerekli |Uygulama kimliği, kayıt portalı ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uygulamanızı atanmış. |
| response_type |gerekli |İçermelidir `id_token` Openıd Connect oturum açma için. Response_type içerebilir `token`. Kullanarak `token` hemen bir erişim belirteci ikinci bir istek için Authorize son noktası yapmak zorunda kalmadan Authorize son noktası almak üzere uygulamanızı buraya izin verir. Kullanırsanız `token` response_type, `scope` parametresi için bir belirteç vermek üzere hangi kaynak belirten bir kapsam içermelidir. |
| redirect_uri |Önerilen |Burada kimlik doğrulama yanıtlarının gönderilebilen veya uygulamanız tarafından alınan uygulamanızın redirect_uri. Bu url olarak kodlanmış olması dışında Portalı'nda kayıtlı redirect_uris biri tam olarak eşleşmesi gerekir. |
| scope |gerekli |Kapsamları boşlukla ayrılmış listesi. Openıd Connect için kapsamı içermesi gerekir `openid`, onay için UI "oturumunuzu açma" izni çevirir. İsteğe bağlı olarak ayrıca eklemek isteyebileceğiniz `email` veya `profile` [kapsamları](active-directory-v2-scopes.md) erişmek için ek kullanıcı verileri. Bu isteği çeşitli kaynaklara onay isteme diğer kapsamları da içerebilir. |
| response_mode |Önerilen |Uygulamanıza elde edilen belirteç geri göndermek için kullanılması gereken yöntemini belirtir. Olmalıdır `fragment` örtük akış. |
| durum |Önerilen |Belirteç yanıtta döndürülecek isteğinde bulunan bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Rastgele oluşturulmuş bir benzersiz değer için genellikle kullanılan [siteler arası istek sahteciliğini saldırılarını önleme](http://tools.ietf.org/html/rfc6749#section-10.12). Durum, uygulama kullanıcının durumu hakkındaki bilgileri sayfasında ya da görünümü üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için de kullanılır. |
| nonce |gerekli |Sonuçta elde edilen id_token talep olarak dahil edilecek uygulama tarafından oluşturulan bu isteği dahil bir değer. Uygulama, belirteç yeniden yürütme saldırıları azaltmak için bu değer daha sonra doğrulayabilirsiniz. Genellikle istek kaynağı tanımlamak için kullanılan rastgele, benzersiz bir dize değeridir. |
| istemi |isteğe bağlı |Gerekli olan kullanıcı etkileşimi türünü belirtir. Şu anda yalnızca geçerli değerler 'login', 'none' olan ve 'onay'. `prompt=login` Bu isteğin negating çoklu oturum açma kimlik bilgilerini girmesini zorunlu tutar. `prompt=none` -tersidir kullanıcı hiçbir etkileşimli istemi olmadan sunulmayan sağlayacaktır. İstek sessiz bir şekilde çoklu oturum açma aracılığıyla tamamlanamıyorsa, v2.0 uç noktasına bir hata döndürür. `prompt=consent` Kullanıcı uygulamayı izinler istendiği anın, oturum açtıktan sonra OAuth onay iletişim tetikler. |
| login_hint |isteğe bağlı |Önceden, kullanıcı adını biliyorsanız, oturum açma sayfasında kullanıcı için kullanıcı adı/e-posta adresi alanları önceden doldurmak için kullanılabilir. Kullanıcı adı önceki oturum açma kullanarak bir zaten ayıklanan yeniden kimlik doğrulaması sırasında bu parametre genellikle uygulamaları kullanacağı `preferred_username` talep. |
| domain_hint |isteğe bağlı |Herhangi birini `consumers` veya `organizations`. Bu onay kutusu eklediyseniz, e-posta tabanlı bulma işlemi atlanacak kullanıcı v2.0 oturum açma sayfasında, biraz daha kolay bir kullanıcı deneyimi için önde gelen geçer. Uygulamalar genellikle kullanır bu parametreyi yeniden kimlik doğrulaması sırasında ayıklayarak `tid` id_token talep. Varsa `tid` değer talep `9188040d-6c67-4c5b-b112-36a304b66dad` (Microsoft Account tüketici Kiracı), kullanmanız gereken `domain_hint=consumers`. Aksi takdirde kullanın `domain_hint=organizations`. |


Bu noktada, kullanıcı kimlik bilgilerini girin ve kimlik doğrulamasını tamamlaması istenecektir. V2.0 uç noktası, aynı zamanda kullanıcı için belirtilen izinleri onaylamasını sağlayacak `scope` sorgu parametresi. Kullanıcının tüm bu izinleri olmayan etmişse, onayı gerekli izinleri kullanıcıya sorar. Ayrıntılarını [izinleri, onay ve çok kiracılı uygulamalar sağlanan burada](active-directory-v2-scopes.md).

V2.0 uç noktası kullanıcının kimliğini doğrular ve onayı veren sonra uygulamanızı belirtilen bir yanıt döndürür `redirect_uri`, bölümünde belirtilen yöntemi kullanarak `response_mode` parametresi.

#### <a name="successful-response"></a>Başarılı yanıt
Başarılı yanıt kullanarak bir `response_mode=fragment` ve `response_type=id_token+token` Okunaklılık için satır sonları ile aşağıdaki gibi görünür:

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
| access_token |Dahil edilen if `response_type` içerir `token`. Uygulama, bu durumda Microsoft Graph için istenen erişim belirteci. Erişim belirteci çözülmüş veya aksi halde inceledi, genel olmayan bir dize olarak işlenebilir. |
| token_type |Dahil edilen if `response_type` içerir `token`. Her zaman `Bearer`. |
| expires_in |Dahil edilen if `response_type` içerir `token`. Belirteç tarafından önbelleğe alma işlemleri için geçerli kaldığı saniye sayısını gösterir. |
| scope |Dahil edilen if `response_type` içerir `token`. Access_token geçerli olacağı kapsamlar gösterir. |
| id_token |Uygulama istenen id_token. İd_token, kullanıcının kimliğini doğrulamak ve kullanıcıyı bir oturum başlatmak için kullanabilirsiniz. İd_tokens ve içerikleri daha fazla ayrıntı dahil [v2.0 uç noktası belirteç başvurusu](active-directory-v2-tokens.md). |
| durum |State parametresi istekte yer alıyorsa aynı değeri yanıt olarak görünmelidir. Uygulama istek ve yanıt durum değerleri özdeş olduğunu doğrulamanız gerekir. |

#### <a name="error-response"></a>Hata yanıtı
Hata yanıtları da gönderilebilir için `redirect_uri` uygulama bunları uygun şekilde işleyebilmesi için:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanılabilir ve hatalara tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

## <a name="validate-the-idtoken"></a>İd_token doğrula
Yalnızca bir id_token alma, kullanıcının kimliğini doğrulamak için yeterli değildir; id_token'ın imzayı doğrulamak ve uygulamanızın gereksinimlerini başına belirteçteki talepleri doğrulamak gerekir. V2.0 uç noktası kullanan [JSON Web belirteçleri (Jwt'ler)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ve Belirteçleri imzalamak ve geçerli olduğunu doğrulamak için ortak anahtar şifrelemesi.

Doğrulamak seçebileceğiniz `id_token` istemci kodu, ancak yaygın bir uygulama olan göndermek için `id_token` bir arka uç sunucusuna ve orada doğrulama gerçekleştirin. İd_token imzası doğruladıktan sonra birkaç talepleri doğrulamak için gerekli vardır. Bkz [v2.0 belirteç başvurusu](active-directory-v2-tokens.md) daha fazla bilgi edinmek için de dahil olmak üzere [doğrulama belirteçleri](active-directory-v2-tokens.md#validating-tokens) ve [önemli bilgiler hakkında imzalama anahtar geçişi](active-directory-v2-tokens.md#validating-tokens). Öneririz belirteçleri doğrulamak ve ayrıştırmak için bir kitaplık yapmayı kullanımı - en az bir dil ve platform için kullanılabilir.
<!--TODO: Improve the information on this-->

Senaryonuza bağlı olarak ek istekleri doğrulamak isteyebilirsiniz. Bazı ortak doğrulamaları şunlardır:

* Kullanıcı/kuruluş sağlama uygulama için kaydolmuş.
* Uygun yetkilendirme/ayrıcalıklarına sahip kullanıcı sağlama
* Belirli bir kimlik doğrulama gücü sağlamak, çok faktörlü kimlik doğrulaması gibi oluştu.

Bir id_token Taleplerde hakkında daha fazla bilgi için bkz. [v2.0 uç noktası belirteç başvurusu](active-directory-v2-tokens.md).

İd_token tamamen doğruladıktan sonra bir kullanıcı oturumu başlatmak ve uygulamanızdaki kullanıcı hakkında bilgi edinmek için id_token talepleri kullanın. Bu bilgiler kullanılabilir görüntüleme, kayıtları, yetkilendirmeleri, vb. için.

## <a name="get-access-tokens"></a>Erişim belirteci alma
Tek sayfalı uygulamanızın kullanıcı oturum açma, erişim belirteçleri çağıran web API'leri gibi Azure AD tarafından güvenlik altına alabilirsiniz [Microsoft Graph](https://graph.microsoft.io). Zaten bir belirteci kullanarak alınan bile `token` response_type, kullanıcı yeniden oturum açmak için yeniden yönlendirme gerek kalmadan ek kaynaklara belirteçlerini almak için bu yöntemi kullanabilirsiniz.

Normal Openıd Connect/OAuth akışını bu v2.0 için bir istekte yaptığınız `/token` uç noktası. Ancak, v2.0 uç noktası almak ve belirteçleri yenilemek için AJAX çağrıları yapma soru dışında bu nedenle CORS isteklerini desteklemez. Bunun yerine, diğer web API'leri için yeni belirteçlerini almak için gizli bir iframe içinde örtük akış kullanabilirsiniz: 

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
> Kopyalama ve yapıştırma deneyin isteği bir tarayıcı sekmesi içine aşağıdaki! (Değiştirmeyi unutmayın `domain_hint` ve `login_hint` kullanıcınız için doğru değerlerle değerler)
> 
> 

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| kiracı |gerekli |`{tenant}` İstek yolunda değer, uygulamaya oturum denetimi için kullanılabilir. İzin verilen değerler `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları. Daha fazla ayrıntı için [protokolü temel](active-directory-v2-protocols.md#endpoints). |
| client_id |gerekli |Uygulama kimliği kayıt portalı ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uygulamanızı atanmış. |
| response_type |gerekli |İçermelidir `id_token` Openıd Connect oturum açma için. Diğer response_types gibi de dahil `code`. |
| redirect_uri |Önerilen |Burada kimlik doğrulama yanıtlarının gönderilebilen veya uygulamanız tarafından alınan uygulamanızın redirect_uri. Bu url olarak kodlanmış olması dışında Portalı'nda kayıtlı redirect_uris biri tam olarak eşleşmesi gerekir. |
| scope |gerekli |Kapsamları boşlukla ayrılmış listesi. Belirteçleri almak için tüm ekleme [kapsamları](active-directory-v2-scopes.md) ilgi kaynak gerektirir. |
| response_mode |Önerilen |Uygulamanıza elde edilen belirteç geri göndermek için kullanılması gereken yöntemini belirtir. Herhangi birini `query`, `form_post`, veya `fragment`. |
| durum |Önerilen |Belirteç yanıtta döndürülecek isteğinde bulunan bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Rastgele oluşturulmuş bir benzersiz değeri, genellikle siteler arası istek sahteciliği saldırılarına önlemek için kullanılır. Durum, uygulama kullanıcının durumu hakkındaki bilgileri sayfasında ya da görünümü üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için de kullanılır. |
| nonce |gerekli |Sonuçta elde edilen id_token talep olarak dahil edilecek uygulama tarafından oluşturulan bu isteği dahil bir değer. Uygulama, belirteç yeniden yürütme saldırıları azaltmak için bu değer daha sonra doğrulayabilirsiniz. Genellikle istek kaynağı tanımlamak için kullanılan rastgele, benzersiz bir dize değeridir. |
| istemi |gerekli |Yenileme & Gizli bir iframe içindeki belirteçleri almak için kullanmanız gereken `prompt=none` sağlamak iframe v2.0 oturum açma sayfasında askıda değil ve hemen döndürür. |
| login_hint |gerekli |Yenileme & Gizli bir iframe içindeki belirteçleri almak için kullanıcının belirli bir anda sürede olabilir birden çok oturumları arasında ayrım yapmak için bu ipucunda kullanıcının kullanıcı adı içermelidir. Önceki oturum açma kullanarak bir kullanıcı adı ayıklayabilir `preferred_username` talep. |
| domain_hint |gerekli |Herhangi birini `consumers` veya `organizations`. Yenileme & Gizli bir iframe içindeki belirteçleri almak için istekte domain_hint eklemeniz gerekir. Ayıklamak `tid` bir önceki kullanmak üzere hangi değeri belirlemek için oturum açın, id_token talep. Varsa `tid` değer talep `9188040d-6c67-4c5b-b112-36a304b66dad`, kullanmanız gereken `domain_hint=consumers`. Aksi takdirde kullanın `domain_hint=organizations`. |

Performanstan `prompt=none` parametresi, bu isteği ya da başarılı olması veya hemen başarısız olur ve uygulamanıza dönün. Başarılı yanıt, belirtilen uygulamanızı gönderilecek `redirect_uri`, bölümünde belirtilen yöntemi kullanarak `response_mode` parametresi.

#### <a name="successful-response"></a>Başarılı yanıt
Başarılı yanıt kullanarak bir `response_mode=fragment` gibi görünür:

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
| access_token |İstenen uygulama belirteci. |
| token_type |Her zaman `Bearer`. |
| durum |State parametresi istekte yer alıyorsa aynı değeri yanıt olarak görünmelidir. Uygulama istek ve yanıt durum değerleri özdeş olduğunu doğrulamanız gerekir. |
| expires_in |Ne kadar süreyle erişim belirteci (saniye olarak) geçerli değil. |
| scope |Erişim belirteci için geçerli olan kapsamları. |

#### <a name="error-response"></a>Hata yanıtı
Hata yanıtları da gönderilebilir için `redirect_uri` uygulama bunları uygun şekilde işleyebilmesi için. Durumunda, `prompt=none`, beklenen hata olacaktır:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanılabilir ve hatalara tepki vermek için kullanılan bir hata kodu dizesi. |
| error_description |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

İframe istekte bu hata iletisini alırsanız kullanıcının etkileşimli olarak tekrar yeni bir belirteç almak oturum açmanız gerekir. Uygulamanız için mantıklı yolu, bu durumun üstesinden seçebilirsiniz.

## <a name="validating-access-tokens"></a>Erişim belirteçleri doğrulama

Bir access_token aldıktan sonra aşağıdaki talep yanı sıra, belirteç imzası doğruladığınızdan emin olun. Ayrıca, senaryoya göre ek talep doğrulamak seçebilirsiniz. 

* **Hedef kitle** talep belirteci uygulamanıza verilmesi amaçlanmamıştır emin olmak için
* **veren** talep belirteci uygulamanıza v2.0 uç noktası tarafından verildiğini doğrulamak için
* **öncesine** ve **süre sonu** talep belirtecin süresinin sona ermediğini doğrulamak için

Erişim belirtecinde talep hakkında daha fazla bilgi için bkz: [v2.0 uç noktası belirteç başvurusu](active-directory-v2-tokens.md)

## <a name="refreshing-tokens"></a>Yenileme belirteçleri
Örtük verme, yenileme belirteçleri sağlamaz. Her ikisi de `id_token`s ve `access_token`s, saati, uygulamanız bu yenilemek için hazırlanması gerekir böylece kısa bir süre düzenli aralıklarla belirteçler sonra dolacak. İki tür belirteci yenilemek için yukarıdaki kullanarak aynı gizli iframe isteği gerçekleştirebilirsiniz `prompt=none` Azure AD'nin davranışını denetlemek için parametre. Yeni bir almak istiyorsanız `id_token`, kullandığınızdan emin olun `response_type=id_token` ve `scope=openid`, hem de bir `nonce` parametresi.

## <a name="send-a-sign-out-request"></a>Bir oturum kapatma isteği gönder
Openıdconnect `end_session_endpoint` v2.0 uç noktası tarafından ayarlanmış tanımlama bilgilerini temizleyin ve bir kullanıcının oturumunu sona erdirmek için v2.0 uç noktasına bir istek göndermek uygulamanızı sağlar. Tam olarak bir web uygulaması dışında bir kullanıcı oturum açmak için uygulamanızı kendi kullanıcı (genellikle bir belirteç önbelleği temizlemek veya tanımlama bilgilerini bırakarak) oturumunu ve tarayıcı yönlendirin:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/logout?post_logout_redirect_uri=https://localhost/myapp/
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| kiracı |gerekli |`{tenant}` İstek yolunda değer, uygulamaya oturum denetimi için kullanılabilir. İzin verilen değerler `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları. Daha fazla ayrıntı için [protokolü temel](active-directory-v2-protocols.md#endpoints). |
| post_logout_redirect_uri | Önerilen | Oturum kapatma tamamlandıktan sonra kullanıcı döndürülmesi gereken URL. Bu değer bir URI'leri kayıtlı uygulama için yeniden yönlendirme eşleşmelidir. Kullanıcı dahil edilmezse, v2.0 uç noktası tarafından genel bir ileti gösterilir. |
