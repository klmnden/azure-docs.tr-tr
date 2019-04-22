---
title: Microsoft kimlik platformu örtük akışını kullanarak tek sayfalı uygulamaları güvenli hale getirin | Azure
description: Tek sayfalı uygulamalar için Microsoft kimlik platformu uygulaması örtük akışını kullanarak web uygulamaları oluşturma.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 3605931f-dc24-4910-bb50-5375defec6a8
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
ms.openlocfilehash: d517828b30629cd9dfba5459b1d90913d8bc4f77
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59698461"
---
# <a name="microsoft-identity-platform-and-implicit-grant-flow"></a>Microsoft kimlik platformu ve örtük akış verin

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Microsoft kimlik platformu uç nokta ile kullanarak tek sayfalı uygulamanızla hem kişisel ve iş veya Okul hesaplarını Microsoft gelen kullanıcılar oturum açabilir. Tek sayfa ve birkaç ilgi çekici bir tarayıcı yüz temel olarak çalışan diğer JavaScript uygulamaları için kimlik doğrulaması geldiğinde sorunları:

* Bu uygulamalar güvenlik özelliklerini geleneksel sunucu tabanlı web uygulamalarından önemli ölçüde farklılık gösterir.
* Çok sayıda yetkilendirme sunucularını ve kimlik sağlayıcıları CORS isteklerini desteklemez.
* Tam sayfada tarayıcı kullanıcı deneyimiyle özellikle bozucu haline uygulama uzağa yeniden yönlendirir.

Bu uygulamalar için (AngularJS, Ember.js, React.js vb.), Microsoft kimlik platformu OAuth 2.0 örtülü izin akışını destekler. Örtük akış açıklanan [OAuth 2.0 belirtimini](https://tools.ietf.org/html/rfc6749#section-4.2). Bunun yararı belirteçleri Microsoft kimlik platformu ' bir arka uç sunucusuna gerçekleştirmeden kimlik bilgisi alışverişinin ve uygulamayı vermesidir. Bu, kullanıcının oturumunu, oturumu korumak ve istemcideki tüm diğer web API'leri belirteçleri JavaScript kodu alma okumasına izin verir. Örtük akış özellikle yaklaşık kullanırken dikkate almanız birkaç önemli güvenlik konuları vardır [istemci](https://tools.ietf.org/html/rfc6749#section-10.3) ve [kullanıcı kimliğe bürünme](https://tools.ietf.org/html/rfc6749#section-10.3).

Microsoft kimlik platformu ve örtük akış, JavaScript uygulamanıza kimlik doğrulaması eklemek için kullanmak istiyorsanız, açık kaynaklı JavaScript Kitaplığı kullanmanız önerilir [msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js).

Ancak, bir tek sayfalı uygulamanızı kitaplıkta kullanma ve kendinize protokol iletilerini göndermek isterseniz, aşağıdaki genel adımları izleyin.

> [!NOTE]
> Tüm Azure Active Directory (Azure AD) senaryoları ve özellikleri Microsoft kimlik platformu uç noktası tarafından desteklenir. Microsoft kimlik platformu uç noktası kullanıyorsanız belirlemek için aşağıdaki hakkında bilgi edinin: [Microsoft Identity platform sınırlamaları](active-directory-v2-limitations.md).

## <a name="protocol-diagram"></a>Protokol diyagramı

Aşağıdaki diyagramda tüm örtük oturum açma akışı nasıl göründüğünü gösterir ve aşağıdaki bölümlerde her adımda daha fazla ayrıntı tanımlayın.

![Openıd Connect Kulvarlar](./media/v2-oauth2-implicit-grant-flow/convergence-scenarios-implicit.svg)

## <a name="send-the-sign-in-request"></a>Oturum açma isteği gönder

İlk kullanıcı, uygulamada oturum açması için size gönderebilir bir [Openıd Connect](v2-protocols-oidc.md) kimlik doğrulama isteği ve get bir `id_token` Microsoft kimlik platformu uç noktasından.

> [!IMPORTANT]
> Başarılı bir şekilde uygulama kaydında bir kimlik belirteci istemek için [Azure Portalı - Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfa doğru seçerek etkinleştirilmiş örtük verme akışı olmalıdır **erişim belirteçlerini** ve **Kimlik belirteçlerini** altında **örtük vermeyi** bölümü. Bunu etkin değilse, bir `unsupported_response` hata döndürülür: **'Response_type' giriş parametresi için sağlanan değer bu istemci için izin verilmiyor. 'Code' beklenen değerdir**

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid
&response_mode=fragment
&state=12345
&nonce=678910
```

> [!TIP]
> Örtük akışını kullanarak test etmek için <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=fragment&state=12345&nonce=678910" target="_blank"> https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a> Oturum açtıktan sonra tarayıcınızı için yeniden yönlendirilmesi gereken `https://localhost/myapp/` ile bir `id_token` adres çubuğundaki.
>

| Parametre |  | Açıklama |
| --- | --- | --- |
| `tenant` | gerekli |`{tenant}` İstek yolunda değer, uygulamaya oturum denetimi için kullanılabilir. İzin verilen değerler `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları. Daha fazla ayrıntı için [protokolü temel](active-directory-v2-protocols.md#endpoints). |
| `client_id` | gerekli | (İstemci) uygulama kimliği [Azure Portalı - Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) uygulamanıza atanan sayfası. |
| `response_type` | gerekli |İçermelidir `id_token` Openıd Connect oturum açma için. Response_type içerebilir `token`. Kullanarak `token` hemen bir erişim belirteci ikinci bir istek için Authorize son noktası yapmak zorunda kalmadan Authorize son noktası almak üzere uygulamanızı buraya izin verir. Kullanırsanız `token` response_type, `scope` parametresi için bir belirteç vermek üzere hangi kaynak belirten bir kapsam içermelidir. |
| `redirect_uri` | Önerilen |Burada kimlik doğrulama yanıtlarının gönderilebilen veya uygulamanız tarafından alınan uygulamanızın redirect_uri. Bu url olarak kodlanmış olması dışında Portalı'nda kayıtlı redirect_uris biri tam olarak eşleşmesi gerekir. |
| `scope` | gerekli |Boşlukla ayrılmış bir listesini [kapsamları](v2-permissions-and-consent.md). Openıd Connect için kapsamı içermesi gerekir `openid`, onay için UI "oturumunuzu açma" izni çevirir. İsteğe bağlı olarak ayrıca eklemek isteyebileceğiniz `email` veya `profile` ek kullanıcı verilerine erişim kazanmak için kapsamları. Bu isteği çeşitli kaynaklara onay isteme diğer kapsamları da içerebilir. |
| `response_mode` | isteğe bağlı |Uygulamanıza elde edilen belirteç geri göndermek için kullanılması gereken yöntemini belirtir. İstek bir id_token içeriyorsa, bir erişim belirteci ancak parça için sorgulamak için varsayılan değeri. |
| `state` | Önerilen |Belirteç yanıtta döndürülecek isteğinde bulunan bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Rastgele oluşturulmuş bir benzersiz değer için genellikle kullanılan [siteler arası istek sahteciliğini saldırılarını önleme](https://tools.ietf.org/html/rfc6749#section-10.12). Durum, uygulama kullanıcının durumu hakkındaki bilgileri sayfasında ya da görünümü üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için de kullanılır. |
| `nonce` | gerekli |Sonuçta elde edilen id_token talep olarak dahil edilecek uygulama tarafından oluşturulan bu isteği dahil bir değer. Uygulama, belirteç yeniden yürütme saldırıları azaltmak için bu değer daha sonra doğrulayabilirsiniz. Genellikle istek kaynağı tanımlamak için kullanılan rastgele, benzersiz bir dize değeridir. Yalnızca bir id_token istendiğinde gereklidir. |
| `prompt` | isteğe bağlı |Gerekli olan kullanıcı etkileşimi türünü belirtir. Şu anda yalnızca geçerli değerler 'login', 'none', 'select_account' olan ve 'onay'. `prompt=login` Bu isteğin negating çoklu oturum açma kimlik bilgilerini girmesini zorunlu tutar. `prompt=none` -tersidir kullanıcı hiçbir etkileşimli istemi olmadan sunulan değil sağlayacaktır. Microsoft kimlik platformu uç nokta isteği aracılığıyla çoklu oturum açma sessiz bir şekilde tamamlanamıyor, bir hata döndürür. `prompt=select_account` kullanıcı için bir hesap Seçici, tüm oturumda anımsanacak hesapları nerede görüneceğini gönderir. `prompt=consent` Kullanıcı uygulamayı izinler istendiği anın, oturum açtıktan sonra OAuth onay iletişim tetikler. |
| `login_hint`  |isteğe bağlı |Önceden, kullanıcı adını biliyorsanız, oturum açma sayfasında kullanıcı için kullanıcı adı/e-posta adresi alanları önceden doldurmak için kullanılabilir. Kullanıcı adı önceki oturum açma kullanarak bir zaten ayıklanan yeniden kimlik doğrulaması sırasında bu parametre genellikle uygulamaları kullanacağı `preferred_username` talep.|
| `domain_hint` | isteğe bağlı |Herhangi birini `consumers` veya `organizations`. Eklenirse, e-posta tabanlı bulma işlemi atlanacak kullanıcı oturum açma sayfasında, biraz daha kolay bir kullanıcı deneyimi için önde gelen geçer. Uygulamalar genellikle kullanır bu parametreyi yeniden kimlik doğrulaması sırasında ayıklayarak `tid` id_token talep. Varsa `tid` değer talep `9188040d-6c67-4c5b-b112-36a304b66dad` (Microsoft Account tüketici Kiracı), kullanmanız gereken `domain_hint=consumers`. Aksi takdirde, kullanabileceğiniz `domain_hint=organizations` yeniden kimlik doğrulaması sırasında. |

Bu noktada, kullanıcı kimlik bilgilerini girin ve kimlik doğrulamasını tamamlaması istenecektir. Microsoft kimlik platformu uç noktası, ayrıca kullanıcı için belirtilen izinleri onaylamasını sağlayacak `scope` sorgu parametresi. Kullanıcının seçtiği varsa **hiçbiri** bu izinleri, kullanıcı gerekli izinleri onay ister. Daha fazla bilgi için bkz. [izinleri, onay ve çok kiracılı uygulamalar](v2-permissions-and-consent.md).

Kullanıcının kimliğini doğrular ve onayı veren sonra Microsoft kimlik platformu uç nokta uygulamanızı belirtilen bir yanıt döndürür `redirect_uri`, bölümünde belirtilen yöntemi kullanarak `response_mode` parametresi.

#### <a name="successful-response"></a>Başarılı yanıt

Başarılı yanıt kullanarak bir `response_mode=fragment` ve `response_type=id_token+token` (Okunaklılık için satır sonuyla) aşağıdaki gibi görünür:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fuser.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| `access_token` |Dahil edilen if `response_type` içerir `token`. Uygulama, bu durumda Microsoft Graph için istenen erişim belirteci. Erişim belirteci olmamalıdır çözülmüş veya aksi halde inceledi, genel olmayan bir dize olarak değerlendirilmelidir. |
| `token_type` |Dahil edilen if `response_type` içerir `token`. Her zaman `Bearer`. |
| `expires_in`|Dahil edilen if `response_type` içerir `token`. Belirteç tarafından önbelleğe alma işlemleri için geçerli kaldığı saniye sayısını gösterir. |
| `scope` |Dahil edilen if `response_type` içerir `token`. Access_token geçerli olacağı kapsamlar gösterir. (Söz konusu olduğunda, Azure yalnızca AD kapsamları kişisel hesapla oturum açmak için kullanıldığında, istenen) kullanıcı için geçerli değil, tüm istenen kapsam içermeyebilir. |
| `id_token` | Bir imzalı JSON Web Token (JWT). Uygulama isteği açan kullanıcı hakkında bilgi için bu belirteci parçalarını çözebilen. Uygulama değerleri önbelleğe ve bunları görüntüleyebilirsiniz, ancak, bunlar üzerinde herhangi bir yetkilendirme veya güvenlik sınırları için güvenmemelisiniz. İd_tokens hakkında daha fazla bilgi için bkz: [ `id_token reference` ](id-tokens.md). <br> **Not:** Yalnızca belirtilen if `openid` kapsam istendi. |
| `state` |State parametresi istekte yer alıyorsa aynı değeri yanıt olarak görünmelidir. Uygulama istek ve yanıt durum değerleri özdeş olduğunu doğrulamanız gerekir. |

#### <a name="error-response"></a>Hata yanıtı

Hata yanıtları da gönderilebilir için `redirect_uri` uygulama bunları uygun şekilde işleyebilmesi için:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| `error` |Oluşan hataları türlerini sınıflandırmak için kullanılabilir ve hatalara tepki vermek için kullanılan bir hata kodu dizesi. |
| `error_description` |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

## <a name="validate-the-idtoken"></a>İd_token doğrula

Yalnızca bir id_token alma kullanıcının kimliğini doğrulamak için yeterli değildir; Ayrıca, id_token'ın imzayı doğrulamak ve uygulamanızın gereksinimlerine göre belirteçteki talepleri doğrulamak gerekir. Microsoft kimlik platformu uç noktasını kullanan [JSON Web belirteçleri (Jwt'ler)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ve Belirteçleri imzalamak ve geçerli olduğunu doğrulamak için ortak anahtar şifrelemesi.

Doğrulamak seçebileceğiniz `id_token` istemci kodu, ancak yaygın bir uygulama olan göndermek için `id_token` bir arka uç sunucusuna ve orada doğrulama gerçekleştirin. İd_token imzası doğruladıktan sonra doğrulama gerekecek birkaç talep vardır. Bkz: [ `id_token` başvuru](id-tokens.md) daha fazla bilgi edinmek için de dahil olmak üzere [belirteçleri doğrulama](id-tokens.md#validating-an-id_token) ve [imzalama anahtarı geçiş işlemi hakkında önemli bilgiler](active-directory-signing-key-rollover.md). Öneririz belirteçleri doğrulamak ve ayrıştırmak için bir kitaplık yapmayı kullanımı - en az bir dil ve platform için kullanılabilir.

Senaryonuza bağlı olarak ek istekleri doğrulamak isteyebilirsiniz. Bazı ortak doğrulamaları şunlardır:

* Kullanıcı/kuruluş sağlama uygulama için kaydolmuş.
* Kullanıcı sağlama uygun yetkilendirme/ayrıcalıklara sahip değil.
* Belirli bir kimlik doğrulama gücü sağlamak, çok faktörlü kimlik doğrulaması gibi oluştu.

İd_token doğruladıktan sonra bir kullanıcı oturumu başlatmak ve uygulamanızdaki kullanıcı hakkında bilgi edinmek için id_token talepleri kullanın. Bu bilgiler, görüntü, kayıtları, kişiselleştirme ve daha fazla bilgi için kullanılabilir.

## <a name="get-access-tokens"></a>Erişim belirteci alma

Tek sayfalı uygulamanızı kullanıcı oturum açma, erişim belirteçleri çağıran web API'leri gibi Microsoft kimlik platformu tarafından güvenlik altına alabilirsiniz [Microsoft Graph](https://developer.microsoft.com/graph). Zaten bir belirteci kullanarak alınan bile `token` response_type, kullanıcı yeniden oturum açmak için yeniden yönlendirme gerek kalmadan ek kaynaklara belirteçlerini almak için bu yöntemi kullanabilirsiniz.

Normal Openıd Connect/OAuth akışını bu Microsoft kimlik platformu için bir istekte yaptığınız `/token` uç noktası. Ancak, Microsoft kimlik platformu uç nokta, almak ve belirteçleri yenilemek için AJAX çağrıları yapma soru dışında bu nedenle, CORS istekleri desteklemez. Bunun yerine, diğer web API'leri için yeni belirteçlerini almak için gizli bir iframe içinde örtük akış kullanabilirsiniz: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read 
&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

URL'deki sorgu parametreleri hakkında daha fazla bilgi için bkz: [oturum açma isteği Gönder](#send-the-sign-in-request).

> [!TIP]
> Kopyalama ve yapıştırma deneyin isteği bir tarayıcı sekmesi içine aşağıdaki! (Değiştirmeyi unutmayın `login_hint` değerleri, kullanıcı için doğru değeri ile)
>
>`https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2user.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&login_hint=your-username`
>

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
| `access_token` |Dahil edilen if `response_type` içerir `token`. Uygulama, bu durumda Microsoft Graph için istenen erişim belirteci. Erişim belirteci olmamalıdır çözülmüş veya aksi halde inceledi, genel olmayan bir dize olarak değerlendirilmelidir. |
| `token_type` | Her zaman `Bearer`. |
| `expires_in` | Belirteç tarafından önbelleğe alma işlemleri için geçerli kaldığı saniye sayısını gösterir. |
| `scope` | Access_token geçerli olacağı kapsamlar gösterir. (Söz konusu olduğunda, Azure yalnızca AD kapsamları kişisel hesapla oturum açmak için kullanıldığında, istenen) kullanıcı için geçerli değil, tüm istenen kapsam içermeyebilir. |
| `id_token` | Bir imzalı JSON Web Token (JWT). Dahil edilen if `response_type` içerir `id_token`. Uygulama isteği açan kullanıcı hakkında bilgi için bu belirteci parçalarını çözebilen. Uygulama değerleri önbelleğe ve bunları görüntüleyebilirsiniz, ancak, bunlar üzerinde herhangi bir yetkilendirme veya güvenlik sınırları için güvenmemelisiniz. İd_tokens hakkında daha fazla bilgi için bkz: [ `id_token` başvuru](id-tokens.md). <br> **Not:** Yalnızca belirtilen if `openid` kapsam istendi. |
| `state` |State parametresi istekte yer alıyorsa aynı değeri yanıt olarak görünmelidir. Uygulama istek ve yanıt durum değerleri özdeş olduğunu doğrulamanız gerekir. |

#### <a name="error-response"></a>Hata yanıtı

Hata yanıtları da gönderilebilir için `redirect_uri` uygulama bunları uygun şekilde işleyebilmesi için. Durumunda, `prompt=none`, beklenen hata olacaktır:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parametre | Açıklama |
| --- | --- |
| `error` |Oluşan hataları türlerini sınıflandırmak için kullanılabilir ve hatalara tepki vermek için kullanılan bir hata kodu dizesi. |
| `error_description` |Bir geliştirici bir kimlik doğrulama hatası kök nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

İframe istekte bu hata iletisini alırsanız kullanıcının etkileşimli olarak tekrar yeni bir belirteç almak oturum açmanız gerekir. Uygulamanız için mantıklı yolu, bu durumun üstesinden seçebilirsiniz.

## <a name="validating-access-tokens"></a>Erişim belirteçleri doğrulama

Bir access_token aldıktan sonra aşağıdaki talep yanı sıra, belirteç imzası doğruladığınızdan emin olun. Ayrıca, senaryoya göre ek talep doğrulamak seçebilirsiniz.

* **Hedef kitle** talep belirteci uygulamanıza verilmesi amaçlanmamıştır emin olmak için
* **veren** talep belirteci uygulamanıza Microsoft kimlik platformu uç noktası tarafından verildiğini doğrulamak için
* **öncesine** ve **süre sonu** talep belirtecin süresinin sona ermediğini doğrulamak için

Erişim belirtecinde talep hakkında daha fazla bilgi için bkz: [erişim belirteci başvurusu](access-tokens.md)

## <a name="refreshing-tokens"></a>Yenileme belirteçleri

Örtük verme, yenileme belirteçleri sağlamaz. Her ikisi de `id_token`s ve `access_token`s, saati, uygulamanız bu yenilemek için hazırlanması gerekir böylece kısa bir süre düzenli aralıklarla belirteçler sonra dolacak. İki tür belirteci yenilemek için yukarıdaki kullanarak aynı gizli iframe isteği gerçekleştirebilirsiniz `prompt=none` kimlik platformun davranışını denetlemek için parametre. Yeni bir almak istiyorsanız `id_token`, kullandığınızdan emin olun `response_type=id_token` ve `scope=openid`, hem de bir `nonce` parametresi.

## <a name="send-a-sign-out-request"></a>Bir oturum kapatma isteği gönder

Openıd Connect `end_session_endpoint` Microsoft kimlik platformu uç noktası tarafından ayarlanmış tanımlama bilgilerini temizleyin ve bir kullanıcının oturumunu sona erdirmek için Microsoft kimlik platformu uç noktasına bir istek göndermek uygulamanızı sağlar. Tam olarak bir web uygulaması dışında bir kullanıcı oturum açmak için uygulamanızı kendi kullanıcı (genellikle bir belirteç önbelleği temizlemek veya tanımlama bilgilerini bırakarak) oturumunu ve tarayıcı yönlendirin:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/logout?post_logout_redirect_uri=https://localhost/myapp/
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| `tenant` |gerekli |`{tenant}` İstek yolunda değer, uygulamaya oturum denetimi için kullanılabilir. İzin verilen değerler `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları. Daha fazla ayrıntı için [protokolü temel](active-directory-v2-protocols.md#endpoints). |
| `post_logout_redirect_uri` | Önerilen | Oturum kapatma tamamlandıktan sonra kullanıcı döndürülmesi gereken URL. Bu değer bir URI'leri kayıtlı uygulama için yeniden yönlendirme eşleşmelidir. Dahil edilmezse, kullanıcının Microsoft kimlik platformu uç noktası tarafından genel bir ileti gösterilir. |

## <a name="next-steps"></a>Sonraki adımlar

* Git üzerinden [MSAL JS örnekleri](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Samples) kodlamaya başlamak.
