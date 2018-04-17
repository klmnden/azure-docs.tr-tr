---
title: 'Azure Active Directory B2C: örtük akışını kullanarak tek sayfa uygulamaları | Microsoft Docs'
description: Azure Active Directory B2C ile OAuth 2.0 örtük akışını kullanarak doğrudan tek sayfa uygulamaları oluşturmayı öğrenin.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 02/06/2017
ms.author: davidmu
ms.openlocfilehash: 3347eac16e447091ffcaaf403e1291e2c7175a2d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="azure-ad-b2c-single-page-app-sign-in-by-using-oauth-20-implicit-flow"></a>Azure AD B2C: Tek sayfalı uygulama oturum OAuth 2.0 örtük akışını kullanarak açma

Birçok modern uygulamanın, JavaScript'te öncelikli olarak yazılmış tek sayfa uygulaması ön ucu sahip. Genellikle, uygulama AngularJS, Ember.js veya Durandal gibi bir çerçeve kullanılarak yazılır. Tek sayfalı uygulamalar ve öncelikle tarayıcıda çalışan diğer JavaScript uygulamalar kimlik doğrulaması için ek bazı zorluklar vardır:

* Bu uygulamaların güvenlik özelliklerini geleneksel sunucu tabanlı web uygulamalarından önemli ölçüde farklıdır.
* Çok sayıda yetkilendirme sunucularını ve kimlik sağlayıcıları çıkış noktaları arası kaynak paylaşma (CORS) isteklerini desteklemez.
* Uygulama çıktığınızda tam tarayıcı yeniden yönlendirmeleri kullanıcı deneyimini önemli ölçüde bozucu olabilir.

Bu uygulamalar, Azure Active Directory B2C desteklemek için OAuth 2.0 örtük akışını (Azure AD B2C) kullanır. OAuth 2.0 yetkilendirme örtük grant akışını açıklanan [4.2 OAuth 2.0 belirtimi bölüm](http://tools.ietf.org/html/rfc6749). Örtük akışını uygulama belirteçlerini doğrudan Azure Active Directory'den (Azure AD) aldığı herhangi bir sunucudan sunucuya exchange olmadan son noktanın yetkilendirilmesi. Tüm kimlik doğrulaması mantığı ve gerçekleştirilen işlemlerin işleme oturumu tamamen ek sayfa yeniden yönlendirmeleri olmadan JavaScript istemci yerleştirin.

Azure AD B2C standart OAuth 2.0 örtük akışını birden çok basit kimlik doğrulama ve yetkilendirme genişletir. Azure AD B2C tanıtır [İlkesi parametresi](active-directory-b2c-reference-policies.md). İlke parametresiyle kullanıcı deneyimleri, uygulamanızın gibi eklemek için OAuth 2.0 kullanabilirsiniz kaydolma, oturum açma ve profil yönetimi. Bu makalede, Azure AD ve örtük akışını her bu deneyimler, tek sayfalı uygulamalarınızda uygulamak için nasıl kullanılacağını gösteriyoruz. Başlamanıza yardımcı olmak için bir göz atalım bizim [Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-nodejs-webapi) ve [Microsoft .NET](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-dotnet-webapi) örnekleri.

Bu makaledeki örnek HTTP isteklerinde kullanırız bizim örnek Azure AD B2C dizini **fabrikamb2c.onmicrosoft.com**. Ayrıca kendi örnek uygulama ve ilkeleri kullanırız. Bu değerleri kullanarak kendiniz istekleri deneyebilir ya da kendi değerlerinizle değiştirin.
Bilgi edinmek için nasıl [kendi Azure AD B2C dizini, uygulama ve ilkeleri alma](#use-your-own-b2c-tenant).


## <a name="protocol-diagram"></a>Protokol diyagramı

Örtük oturum açma akışını aşağıdaki şekilde şöyle görünür. Her adım ayrıntılı makalenin sonraki bölümlerinde açıklanmıştır.

![Openıd Connect kulvarları](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-authentication-requests"></a>Kimlik doğrulama isteklerini gönderme
Web uygulamanızı kullanıcının kimliğini doğrulamak ve ilke yürütmesini gerektiğinde kullanıcıya yönlendirir `/authorize` uç noktası. Bu etkileşimli akış burada kullanıcı ilkesine bağlı olarak eylem gerçekleştirir bölümüdür. Kullanıcı Kimliği Azure AD uç noktasından belirteç alır.

İstemci bu istekte gösterir `scope` parametresi, kullanıcıdan almak için gereken izinleri. İçinde `p` parametresi, yürütmek için ilke gösterir. Aşağıdaki üç örnekler (okunabilirlik için satır sonuyla) her başka bir ilke kullanın. Nasıl çalıştığı her istek için bir fikir almak için onu çalıştıran ve bir tarayıcıya istek yapıştırma deneyin.

### <a name="use-a-sign-in-policy"></a>Bir oturum açma ilkesini kullanın
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-policy"></a>Kayıt İlkesi'ni kullanın
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-policy"></a>Bir profili Düzenle ilkesini kullanın
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
| client_id |Gerekli |Uygulamanıza atanan uygulama kimliği [Azure portal](https://portal.azure.com). |
| response_type |Gerekli |İçermelidir `id_token` Openıd Connect oturum açma için. Yanıt türü içerebilir `token`. Kullanırsanız `token`, uygulamanızı hemen bir erişim belirteci authorize uç noktasından uç noktayı yetkilendirmek için ikinci bir isteği yapmadan alabilir.  Kullanırsanız `token` yanıt türü `scope` parametresi için belirteci vermek için hangi kaynak gösteren bir kapsam içermesi gerekir. |
| redirect_uri |Önerilen |Yeniden yönlendirme URI'si burada kimlik doğrulama yanıtları gönderilebilen veya uygulamanız tarafından alınan, uygulamanızın. URL kodlanmış olmalıdır dışında tam olarak yeniden yönlendirme Portalı'nda kayıtlı URI'ler biriyle eşleşmelidir. |
| response_mode |Önerilen |Sonuçta elde edilen belirteci geri uygulamanıza göndermek için kullanılacak yöntemi belirtir.  Örtük akışlar için kullanmak `fragment`. |
| scope |Gerekli |Kapsamları boşlukla ayrılmış listesi. Azure AD'ye istenecek izinlerin hem tek bir kapsam değeri gösterir. `openid` Kapsam kullanıcı oturumu ve kullanıcı kimliği belirteçleri biçiminde hakkında veri Al izni gösterir. (Biz bu konuda daha sonra makalesinde konuşun.) `offline_access` Kapsamı web uygulamaları için isteğe bağlı değil. Bu, uygulamanızın uzun süreli kaynaklarına erişim için bir yenileme belirteci gerektiğini gösterir. |
| durum |Önerilen |Ayrıca belirteci yanıtta döndürülen istek dahil bir değer. Kullanmak istediğiniz herhangi bir içerik dizesi olabilir. Genellikle, rastgele oluşturulmuş, benzersiz bir değer, siteler arası istek sahtekarlığı saldırıları önlemek için kullanılır. Durumu da kimlik doğrulama isteği oluşmadan önce uygulama kullanıcının durumu hakkında bilgi kodlanması için kullanılan oldukları üzerinde sayfa gibi. |
| nonce |Gerekli |Sonuçta elde edilen kimliği belirteç talep olarak dahil edilen (uygulama tarafından üretilen) istekte bulunan bir değer. Uygulama sonra belirteç yeniden yürütme saldırıları azaltmak için bu değeri doğrulayabilirsiniz. Genellikle, isteğin başlangıç noktasının tanımlamak için kullanılan rastgele, benzersiz bir dize değeridir. |
| P |Gerekli |Yürütme ilkesi. Azure AD B2C kiracınızda oluşturulan bir ilkeyi adıdır. İlke adı değeri ile başlaması gereken **b2c\_1\_**. Daha fazla bilgi için bkz: [Azure AD B2C yerleşik ilkeleri](active-directory-b2c-reference-policies.md). |
| istemi |İsteğe bağlı |Gerekli olan bir kullanıcı etkileşimi türü. Şu anda, yalnızca geçerli değer: `login`. Bu kullanıcının bu isteği kimlik bilgilerini girmesini zorlar. Çoklu oturum açma etkili olmaz. |

Bu noktada, ilkenin iş akışını tamamlamak için kullanıcının istedi. Bu dizini ya da başka bir sayıyı adımları için kaydolan bir sosyal kimlik bilgilerinizle oturum, kullanıcı adını ve parolasını girerek kullanıcı gerektirebilir. İlkenin nasıl tanımlanan kullanıcı eylemlerini bağlıdır.

Kullanıcı ilkeyi tamamladıktan sonra Azure AD için kullanılır ve uygulamanızın değerde bir yanıt döndürür `redirect_uri`. Bölümünde belirtilen yöntemi kullanan `response_mode` parametresi. Yanıt tam olarak yürütülen ilke bağımsız kullanıcı eylemi senaryoların her biri için aynıdır.

### <a name="successful-response"></a>Başarılı yanıt
Kullanan başarılı yanıt `response_mode=fragment` ve `response_type=id_token+token` okunabilirliği için satır sonu ile aşağıdaki gibi görünür:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
```

| Parametre | Açıklama |
| --- | --- |
| access_token |Uygulama istenen erişim belirteci.  Erişim belirteci kodunu çözdü veya gerekir aksi takdirde sahip denetlenir. Genel olmayan bir dize olarak işlenebilir. |
| token_type |Belirteç türü değeri. Azure AD destekleyen tek taşıyıcı türüdür. |
| expires_in |Erişim belirtecinin (saniye olarak) geçerli olduğu süreyi. |
| scope |Belirteç için geçerli olur; kapsam. Önbellek belirteçleri kapsamına daha sonra kullanmak üzere de kullanabilirsiniz. |
| id_token |İstenen uygulama kimliği belirteci. Kullanıcının kimliğini doğrulamak ve kullanıcı oturumu başlatmak için kimliği belirteci kullanabilirsiniz. Kimlik belirteçlerini ve içerikleri hakkında daha fazla bilgi için bkz: [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). |
| durum |Varsa bir `state` parametresi, aynı değeri yanıt olarak görünmesi gereken istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerler aynı. |

### <a name="error-response"></a>Hata yanıtı
Böylece uygulama bunları uygun şekilde işleyebilir hata yanıtları yeniden yönlendirme URI'si da gönderilebilir:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanılan bir hata kodu dizesi. Hata işleme için hata kodu de kullanabilirsiniz. |
| error_description |Yardımcı olabilecek belirli bir hata iletisi kimlik doğrulama hatası kök nedenini tanımlayın. |
| durum |Önceki tabloda tam açıklamasına bakın. Varsa bir `state` parametresi, aynı değeri yanıt olarak görünmesi gereken istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerler aynı.|

## <a name="validate-the-id-token"></a>Kimliği belirtecini doğrula
Bir kimliği belirteci alma kullanıcının kimliğini doğrulamak için yeterli değil. Kimliği belirtecin imzayı doğrulamak ve uygulamanızın gereksinimleri başına belirtecinizdeki talepleri doğrulamanız gerekir. Azure AD B2C kullanan [JSON Web belirteçleri (Jwt'ler)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ve Belirteçleri imzalamak ve bunların geçerli olduğunu doğrulamak için ortak anahtar şifrelemesi.

Birçok açık kaynak kitaplıkları kullanmayı tercih ederseniz dil bağlı olarak Jwt'ler, doğrulamak için kullanılabilir. Kendi doğrulama mantığı uygulamak yerine kullanılabilir açık kaynak kitaplıkları keşfetme göz önünde bulundurun. Bu kitaplıklar düzgün bir şekilde kullanmayı öğrenmenize yardımcı olmak için bu makaledeki bilgileri kullanın.

Azure AD B2C Openıd Connect meta veri son nokta vardır. Uygulama uç nokta çalışma zamanında Azure AD B2C hakkında bilgi almak için kullanabilirsiniz. Bu bilgiler, uç noktaları, belirteç içerikleri ve belirteç imzalama anahtarları içerir. Azure AD B2C kiracınızda her ilke için bir JSON meta veri belgesi yoktur. Örneğin, fabrikamb2c.onmicrosoft.com Kiracı b2c_1_sign_in ilkesinde meta veri belgesi şu konumdadır:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Bu yapılandırma belgesi özelliklerini biri `jwks_uri`. Aynı ilke için değer olacaktır:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`

Hangi ilke kimliği belirteç imzalamak için kullanılan (ve meta verilerini getirmek nereye) belirlemek için iki seçeneğiniz vardır. İlk olarak, ilke adını dahil `acr` içinde talep `id_token`. Bir kimliği belirteci talepleri ayrıştırma hakkında daha fazla bilgi için bkz: [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). Değerini ilkesinde kodlamak için diğer seçeneğiniz olduğunu `state` isteği gönderdiğinizde parametresi. Ardından, kod çözme `state` hangi ilke kullanılan belirlemek için parametre. Her iki yöntem geçerli değil.

Openıd Connect meta veri uç noktasından meta veri belgesi edindiğiniz sonra kimliği belirtecinin imzası doğrulamak için (Bu uç noktada bulunur) 256 RSA ortak anahtarları kullanabilirsiniz. Belirli bir zamanda Bu uç noktada listelenen birden çok anahtar olabilir, her tarafından tanımlanan bir `kid`. Üstbilgisi `id_token` de içeren bir `kid` talep. Bu anahtarları hangisinin kimliği belirteç imzalamak için kullanılan belirtir. Hakkında bilgi dahil daha fazla bilgi için [belirteçleri doğrulama](active-directory-b2c-reference-tokens.md#token-validation), bkz: [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md).
<!--TODO: Improve the information on this-->

Kimliği belirtecinin imzası doğrulama sonra birkaç talep doğrulama gerektirir. Örneğin:

* Doğrulama `nonce` belirteç yeniden yürütme saldırılarını önlemek talep. Oturum açma istekte belirtilen değeri olmalıdır.
* Doğrulama `aud` kimliği belirteci uygulamanız için verilmiş emin olmak talep. Değeri, uygulamanızın uygulama kimliği olmalıdır.
* Doğrulama `iat` ve `exp` kimliği belirtecin süresi geçmemiş emin olmak talep.

Gerçekleştirmeniz gereken birkaç daha fazla doğrulamaları ayrıntılı olarak açıklanmıştır [Openıd Connect çekirdek Spec](http://openid.net/specs/openid-connect-core-1_0.html). Senaryonuza bağlı olarak ek talep doğrulamak isteyebilirsiniz. Bazı ortak doğrulamaları şunları içerir:

* Kullanıcı veya kuruluş uygulaması için kaydolmuş emin olma.
* Kullanıcının uygun yetkilendirme ve ayrıcalıkları olduğundan emin olma.
* Belirli bir kimlik doğrulama gücünü oluştuğunu gibi Azure çok faktörlü kimlik doğrulamasını kullanarak gibi sağlama.

Bir kimliği belirtecinizdeki talepleri hakkında daha fazla bilgi için bkz: [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md).

Kimliği belirteci tamamen doğrulandıktan sonra kullanıcıyla oturum başlayabilirsiniz. Uygulamanızda, kullanıcı hakkında bilgi edinmek için kimliği belirteç talep kullanın. Bu bilgiler, görüntü, kayıtları, yetkilendirme ve benzeri için kullanılabilir.

## <a name="get-access-tokens"></a>Erişim belirteci alın
Web uygulamalarınızı yapmak için gereksinim duyduğu tek şey ilkeleri yürütme ise, sonraki birkaç bölümlerde atlayabilirsiniz. Aşağıdaki bölümlerde yer alan bilgiler, web API'si için kimlik doğrulaması yapılmış çağrılar yapmanız ve hangi Azure AD B2C tarafından korunan yalnızca web uygulamaları için geçerlidir.

Kullanıcı tek sayfalı uygulamanıza oturum açtığınız, çağrıyı yapan web Azure AD tarafından güvenliği sağlanan API'leri için erişim belirteçleri alabilir. Zaten bir belirteç kullanarak aldığınız olsa bile `token` yanıt türünü, kullanıcı yeniden oturum açmak için yeniden yönlendirme olmadan ek kaynaklar için belirteçleri almak için bu yöntemi kullanabilirsiniz.

Tipik web uygulama akışı için bir istek yaparak bunu `/token` uç noktası.  Ancak, almak ve belirteçleri yenilemek için AJAX çağrıları yapma bir seçenek değilse şekilde uç nokta CORS isteklerini desteklemez. Bunun yerine, diğer web API'leri yeni belirteçleri almak için gizli bir HTML iframe öğesi içinde örtük akış kullanabilirsiniz. İle okunabilirliği için satır sonu, örnek aşağıda verilmiştir:

```

https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
&response_mode=fragment
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
&p=b2c_1_sign_in
```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
| client_id |Gerekli |Uygulamanıza atanan uygulama kimliği [Azure portal](https://portal.azure.com). |
| response_type |Gerekli |İçermelidir `id_token` Openıd Connect oturum açma için.  Yanıt türü içerebilir `token`. Kullanırsanız `token` Burada, uygulamanızın hemen bir erişim belirteci authorize uç noktasından uç noktayı yetkilendirmek için ikinci bir isteği yapmadan alabilir. Kullanırsanız `token` yanıt türü `scope` parametresi için belirteci vermek için hangi kaynak gösteren bir kapsam içermesi gerekir. |
| redirect_uri |Önerilen |Yeniden yönlendirme URI'si burada kimlik doğrulama yanıtları gönderilebilen veya uygulamanız tarafından alınan, uygulamanızın. URL kodlanmış olmalıdır dışında tam olarak yeniden yönlendirme Portalı'nda kayıtlı URI'ler biriyle eşleşmelidir. |
| scope |Gerekli |Kapsamları boşlukla ayrılmış listesi.  Belirteçleri almak için istenen kaynak için ihtiyaç duyduğunuz tüm kapsamlar içerir. |
| response_mode |Önerilen |Sonuçta elde edilen belirteci geri uygulamanıza göndermek için kullanılan yöntemi belirtir.  Olabilir `query`, `form_post`, veya `fragment`. |
| durum |Önerilen |Belirteç yanıtta döndürülen istek dahil bir değer.  Kullanmak istediğiniz herhangi bir içerik dizesi olabilir.  Genellikle, rastgele oluşturulmuş, benzersiz bir değer, siteler arası istek sahtekarlığı saldırıları önlemek için kullanılır.  Durum, kimlik doğrulama isteği oluşmadan önce uygulama kullanıcının durumu hakkındaki bilgileri kodlamak için de kullanılır. Örneğin, sayfa veya Görünüm kullanıcı açıktı. |
| nonce |Gerekli |Elde edilen kimliği belirteç talep olarak dahil edilen uygulama tarafından üretilen istekte bulunan bir değer.  Uygulama sonra belirteç yeniden yürütme saldırıları azaltmak için bu değeri doğrulayabilirsiniz. Genellikle, istek kaynağını tanımlayan rastgele, benzersiz bir dize değeridir. |
| istemi |Gerekli |Yenileme ve gizli bir iframe içinde belirteçleri almak için kullanmak `prompt=none` sağlamak IFRAME oturum açma sayfasında takılı değil ve hemen döndürür. |
| login_hint |Gerekli |Yenileme ve gizli bir iframe içinde belirteçleri almak için kullanıcı belirli bir zamanda sahip birden çok oturumu ayırt etmek için bu ipucu kullanıcı adını içerir. Kullanarak bir önceki oturum bileşeninden kullanıcıadı ayıklayabilirsiniz `preferred_username` talep. |
| domain_hint |Gerekli |`consumers` veya `organizations` olabilir.  Yenileme ve gizli bir iframe içinde belirteçleri almak için içermelidir `domain_hint` istek değeri.  Extract `tid` kullanmak için hangi değeri belirlemek için bir önceki oturum açma kimliği belirteç talep.  Varsa `tid` değer talep `9188040d-6c67-4c5b-b112-36a304b66dad`, kullanmak `domain_hint=consumers`.  Aksi takdirde kullanın `domain_hint=organizations`. |

Ayarlayarak `prompt=none` parametresi, bu istek ya da başarılı ya da hemen başarısız olur ve uygulamanıza döndürür.  Başarılı yanıt uygulamanızı belirtilen yeniden yönlendirme URI'si bölümünde belirtilen yöntemi kullanarak gönderilir `response_mode` parametresi.

### <a name="successful-response"></a>Başarılı yanıt
Kullanarak başarılı bir yanıt `response_mode=fragment` şöyle görünür:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
```

| Parametre | Açıklama |
| --- | --- |
| access_token |Uygulama talep belirteci. |
| token_type |Belirteç türü her zaman taşıyıcı olacaktır. |
| durum |Varsa bir `state` parametresi, aynı değeri yanıt olarak görünmesi gereken istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerler aynı. |
| expires_in |Ne kadar süreyle erişim belirteci (saniye olarak) geçerli değil. |
| scope |Erişim belirteci için geçerli kapsam. |

### <a name="error-response"></a>Hata yanıtı
Böylece uygulama bunları uygun şekilde işleyebilir hata yanıtları yeniden yönlendirme URI'si da gönderilebilir.  İçin `prompt=none`, beklenen hata şöyle görünür:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanılan bir hata kodu dizesi. Dize, hataları tepki vermek için de kullanabilirsiniz. |
| error_description |Yardımcı olabilecek belirli bir hata iletisi kimlik doğrulama hatası kök nedenini tanımlayın. |

IFRAME istekte bu hatayı alırsanız, kullanıcı etkileşimli olarak yeniden yeni bir belirteç almak kaydolmalısınız. Bu, uygulamanız için anlamlı bir şekilde işleyebilir.

## <a name="refresh-tokens"></a>Yenileme belirteçlerini
Kimlik belirteçlerini ve erişim belirteçleri kısa bir süre sonra süresi dolacak. Uygulamanız bu belirteçleri düzenli aralıklarla yenilemek için hazırlanması gerekir.  İki tür belirteç yenilemek için kullandık aynı gizli IFRAME isteği bir önceki örnekte kullanarak gerçekleştirmek `prompt=none` Azure AD adımları denetlemek için parametre.  Yeni bir almaya `id_token` değeri, kullandığınızdan emin olun `response_type=id_token` ve `scope=openid`ve bir `nonce` parametresi.

## <a name="send-a-sign-out-request"></a>Oturum kapatma isteği gönder
Uygulama dışında kullanıcı oturum istediğinizde, kullanıcının oturumunu kapatmak için Azure ad yeniden yönlendir. Bunu yapmazsanız, kullanıcının kimlik bilgilerini yeniden girmeye gerek kalmadan, uygulamanızın yeniden kimlik doğrulamaya olabilir. Bu durum, geçerli tek bir oturum açma oturumu Azure AD ile gerekir çünkü.

Kullanıcıya yalnızca yönlendirebilirsiniz `end_session_endpoint` meta veri belgesi açıklanan bağlanmak, aynı Openıd içinde listelenen [kimliği belirtecini doğrula](#validate-the-id-token). Örneğin:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
| P |Gerekli |Kullanıcı uygulamanızı dışında imzalamak için kullandığınız ilke. |
| post_logout_redirect_uri |Önerilen |Kullanıcı için sonra yeniden yönlendirilmesi gereken URL başarılı oturum kapatma. Dahil edilmezse, Azure AD B2C kullanıcı için genel bir ileti görüntülenir. |

> [!NOTE]
> Kullanıcıya yönlendirerek `end_session_endpoint` bazı Azure AD B2C ile kullanıcının tek oturum açma durumu temizler. Ancak, kullanıcının sosyal kimlik sağlayıcısı oturumu dışında kullanıcı oturum değil. Kullanıcı seçerse aynı sağlayıcısı bir sonraki oturum açma sırasında tanımlamak, kullanıcı, kimlik bilgilerini girmeden kimlik doğrulaması yeniden yapılır. Bir kullanıcı oturumunu Azure AD B2C uygulamanızı imzalamak isterse, mutlaka tamamen Facebook firması dışında örneğin imzalamak istedikleri anlamına gelmez. Ancak, yerel hesaplar için kullanıcının oturumunu düzgün sonlandırılır.
> 
> 

## <a name="use-your-own-azure-ad-b2c-tenant"></a>Kendi Azure AD B2C kiracısı kullanın
Bu istekler kendiniz denemek için aşağıdaki üç adımı tamamlayın. Bu makalede, kendi değerlerle kullanırız örnek değerleri değiştirin:

1. [Bir Azure AD B2C kiracısı oluşturma](active-directory-b2c-get-started.md). Kiracı adını isteklerinde kullanın.
2. [Uygulama oluşturma](active-directory-b2c-app-registration.md) bir uygulama Kimliğini almak için ve bir `redirect_uri` değeri. Bir web uygulaması veya web API uygulamanızı içerir. İsteğe bağlı olarak, bir uygulama gizli anahtarı oluşturabilirsiniz.
3. [İlkelerinizi oluşturma](active-directory-b2c-reference-policies.md) ilke adları elde edilir.

## <a name="samples"></a>Örnekler

* [Node.js kullanarak bir tek sayfalı uygulama oluşturma](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-nodejs-webapi)
* [.NET kullanarak bir tek sayfalı uygulama oluşturma](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-dotnet-webapi)

