---
title: Örtük akış, Azure Active Directory B2C kullanarak tek sayfalı uygulamalar | Microsoft Docs
description: Azure Active Directory B2C ile OAuth 2.0 örtük akışını kullanarak doğrudan tek sayfa uygulamaları oluşturmayı öğrenin.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 081adc9421a97f7cafcf7fba946ce0b901a00a0c
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439439"
---
# <a name="azure-ad-b2c-single-page-app-sign-in-by-using-oauth-20-implicit-flow"></a>Azure AD B2C: Tek sayfalı uygulama OAuth 2.0 örtük akışını kullanarak oturum

Birçok modern uygulamanın, JavaScript'te öncelikli olarak yazılmış tek sayfa uygulaması ön ucu vardır. Genellikle, uygulama AngularJS, Ember.js veya Durandal gibi bir çerçeve kullanılarak yazılır. Tek sayfa uygulamaları ve öncelikle bir tarayıcıda çalışan diğer JavaScript uygulamaları için kimlik doğrulaması ek bazı zorluklar vardır:

* Bu uygulamalar güvenlik özelliklerini geleneksel sunucu tabanlı web uygulamalarından önemli ölçüde farklılık gösterir.
* Çok sayıda yetkilendirme sunucularını ve kimlik sağlayıcıları çıkış noktaları arası kaynak paylaşımı (CORS) isteklerini desteklemez.
* Tam tarayıcı yeniden yönlendirmeleri uygulama ayrılmak için kullanıcı deneyimini önemli ölçüde bozucu olabilir.

Bu uygulamaları Azure Active Directory B2C'yi desteklemek için OAuth 2.0 örtük akışını (Azure AD B2C) kullanır. OAuth 2.0 yetkilendirme örtük verme akışı açıklanan [OAuth 2.0 belirtiminin 4.2 bölümünde](https://tools.ietf.org/html/rfc6749). Örtük akış, uygulama belirteçleri, doğrudan Azure Active Directory'den (Azure AD) alır, herhangi bir sunucudan sunucuya exchange olmadan son noktanın yetkilendirilmesi. Tüm kimlik doğrulama mantığı ve gerçekleştirilen işlemlerin işleme oturumu tamamen JavaScript istemcisi, ek sayfa yeniden yönlendirmeleri olmadan yerleştirin.

Azure AD B2C, standart OAuth 2.0 örtük akışını birden çok basit kimlik doğrulaması ve yetkilendirme genişletir. Azure AD B2C'yi tanıtır [ilke parametresi](active-directory-b2c-reference-policies.md). İlke parametresi ile ilkeleri gibi uygulamanıza eklemek için OAuth 2.0 kullanabilirsiniz kaydolma, oturum açma ve profil Yönetimi kullanıcı akışları. Bu makalede, Azure AD ve örtük akış, her biri bu deneyimler, tek sayfalık uygulamalarınızda uygulamak için kullanma gösteriyoruz.

Bizim örnek Azure AD B2C dizini kullandığımız bu makaledeki örnek HTTP isteklerinde **fabrikamb2c.onmicrosoft.com**. Kendi örnek uygulama ve kullanıcı akışları de kullanırız. Bu değerleri kullanarak kendiniz istekleri deneyebilir ya da kendi değerlerinizle değiştirin.
Bilgi edinmek için nasıl [kendi Azure AD B2C dizini, uygulama ve kullanıcı Akışları Al](#use-your-own-azure-ad-b2c-tenant).


## <a name="protocol-diagram"></a>Protokol diyagramı

Örtük oturum açma akışını aşağıdaki şekilde şöyle görünür. Her adım, makalenin ilerleyen bölümlerinde ayrıntılı açıklanmıştır.

![Openıd Connect Kulvarlar](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-authentication-requests"></a>Kimlik doğrulama isteği gönder
Web uygulamanız, kullanıcının kimliğini doğrulamak ve kullanıcı akışı yürütmek gerektiğinde kullanıcıya yönlendirir. `/authorize` uç noktası. Bu etkileşimli akış burada kullanıcı, kullanıcı akışı bağlı olarak bir eylemde bölümüdür. Kullanıcı Kimliği Azure AD uç noktasından belirteç alır.

İstemci bu istekte gösterir `scope` parametresi, kullanıcıdan almak için gereken izinleri. İçinde `p` parametresi yürütmek için kullanıcı akışı gösterir. Aşağıdaki üç örnekler (okunabilirlik için satır sonuyla) her farklı kullanıcı akışı kullanın. Nasıl çalıştığını her istek için bir genel görünüm almak için isteği bir tarayıcıya yapıştırarak bunu çalıştırmayı deneyin.

### <a name="use-a-sign-in-user-flow"></a>Bir kullanıcı oturum açma akışını kullanın
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-user-flow"></a>Kaydolma kullanıcı akışı kullanın
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-user-flow"></a>Bir profili Düzenle kullanıcı akışını kullanın
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
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
| client_id |Gerekli |Uygulamanıza atanan uygulama kimliği [Azure portalında](https://portal.azure.com). |
| response_type |Gerekli |İçermelidir `id_token` Openıd Connect oturum açma için. Yanıt türü içerebilir `token`. Kullanırsanız `token`, uygulamanızı hemen bir erişim belirteci authorize uç noktasından ikinci bir istek için Authorize son noktası yapmadan alabilir.  Kullanırsanız `token` yanıt türü `scope` parametresi için bir belirteç vermek üzere hangi kaynak gösteren bir kapsam içermelidir. |
| redirect_uri |Önerilen |Yeniden yönlendirme URI'si uygulamanızın, burada kimlik doğrulama yanıtlarının gönderilebilen veya uygulamanız tarafından alındı. URL olarak kodlanmış olmalıdır dışında tam olarak yeniden yönlendirme Portalı'nda kayıtlı bir URI'leri biriyle eşleşmelidir. |
| response_mode |Önerilen |Uygulamanıza elde edilen belirteç geri göndermek için kullanılacak yöntemi belirtir.  Örtük akış için kullanmak `fragment`. |
| scope |Gerekli |Kapsamları boşlukla ayrılmış listesi. Azure AD'ye istenecek izinlerin hem tek bir kapsam değeri gösterir. `openid` Kapsamı kullanıcının oturum açmasını ve kullanıcı kimliği belirteçleri şeklinde hakkında veri alma izni gösterir. (Bu konuda daha sonra makalesinde konuşacağız.) `offline_access` Kapsamı, web uygulamaları için isteğe bağlıdır. Bu, uygulamanızı kaynaklarına uzun süreli erişim için bir yenileme belirteci gerektiğini belirtir. |
| durum |Önerilen |Belirteç yanıtta de dahil bir değer döndürülür. Bu, kullanmak istediğiniz herhangi bir içerik dizesi olabilir. Genellikle, rastgele oluşturulmuş, benzersiz bir değer, siteler arası istek sahteciliği saldırılarına önlemek için kullanılır. Durumu kimlik doğrulama isteği oluşmadan önce uygulama kullanıcının durumu hakkında bilgi kodlamak için de kullanılır oldukları üzerinde sayfa gibi. |
| nonce |Gerekli |Sonuçta elde edilen kimlik belirtecinde talep olarak dahil edilen (uygulama tarafından oluşturulan) istek içindeki bir değer. Uygulama, belirteç yeniden yürütme saldırıları azaltmak için bu değer daha sonra doğrulayabilirsiniz. Genellikle, isteğin kaynağı tanımlamak için kullanılan rastgele, benzersiz bir dize değeridir. |
| p |Gerekli |Yürütme ilkesi. Azure AD B2C kiracınız oluşturulur (kullanıcı akışı) bir ilke adıdır. İlke adı değeri ile başlaması gereken **b2c\_1\_**. Daha fazla bilgi için [Azure AD B2C kullanıcı akışları](active-directory-b2c-reference-policies.md). |
| istemi |İsteğe bağlı |Gerekli bir kullanıcı etkileşimi türü. Şu anda geçerli olan `login`. Bu, isteği kimlik bilgilerini girmesini zorlar. Çoklu oturum açma etkili olmaz. |

Bu noktada, ilkenin iş akışını tamamlamak için kullanıcının istenir. Bu dizinin veya herhangi bir adım sayısı için kaydolan bir sosyal kimlik bilgilerinizle oturum, kullanıcı adını ve parolasını girerek kullanıcı gerektirebilir. Kullanıcı akışı nasıl tanımlandığını kullanıcı eylemlerine bağlıdır.

Kullanıcının kullanıcı akışı tamamlandıktan sonra Azure AD uygulamanız için kullanılan değerde bir yanıt döndürür `redirect_uri`. Bölümünde belirtilen yöntemi kullanan `response_mode` parametresi. Yanıt tam olarak yürütülen kullanıcı akışını bağımsız kullanıcı eylem senaryoların her biri için aynıdır.

### <a name="successful-response"></a>Başarılı yanıt
Kullanan başarılı bir yanıt `response_mode=fragment` ve `response_type=id_token+token` Okunaklılık için satır sonları ile aşağıdaki gibi görünür:

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
| access_token |İstenen uygulama erişim belirteci.  Erişim belirteci çözülmüş veya aksi halde inceledi. Donuk bir dize olarak işlenebilir. |
| token_type |Belirteç türü değeri. Azure AD destekleyen tek taşıyıcı türüdür. |
| expires_in |Erişim belirteci (saniye olarak) geçerli olduğu süre uzunluğu. |
| scope |İçin belirteç geçerliyse kapsamları. Önbellek belirteçleri kapsamları daha sonra kullanmak üzere de kullanabilirsiniz. |
| id_token |Uygulama istenen kimlik belirteci. Kimlik belirteci, kullanıcının kimliğini doğrulamak ve kullanıcıyı bir oturum başlatmak için kullanabilirsiniz. Kimlik belirteçlerini ve içerikleri hakkında daha fazla bilgi için bkz. [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). |
| durum |Varsa bir `state` aynı değeri yanıt olarak görünmesi gereken parametresi istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerleri aynıdır. |

### <a name="error-response"></a>Hata yanıtı
Uygulama bunları uygun şekilde işleyebilmeniz hata yanıtları da yeniden yönlendirme URI'si gönderilebilir:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanılan bir hata kodu dizesi. Hata kodu hata işleme için de kullanabilirsiniz. |
| error_description |Belirli bir hata iletisi yardımcı olabilecek bir kimlik doğrulama hatası kök nedenini tanımlayın. |
| durum |Önceki tabloda tam açıklamasına bakın. Varsa bir `state` aynı değeri yanıt olarak görünmesi gereken parametresi istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerleri aynıdır.|

## <a name="validate-the-id-token"></a>Kimlik belirteci doğrulama
Bir kimlik belirteci alma kullanıcının kimliğini doğrulamak yeterli değil. Kimlik belirtecinin imzası doğrulama ve uygulamanızın gereksinimlerini başına belirteçteki talepleri doğrulayın. Azure AD B2C'yi kullanan [JSON Web belirteçleri (Jwt'ler)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ve Belirteçleri imzalamak ve geçerli olduğunu doğrulamak için ortak anahtar şifrelemesi.

Birçok açık kaynak kitaplıkları kullanmayı tercih ettiğiniz dile bağlı olarak, Jwt'ler doğrulamak için kullanılabilir. Kendi doğrulama mantığını uygulama yerine kullanılabilir açık kaynak kitaplıkları keşfetmeye göz önünde bulundurun. Bu kitaplıkları düzgün bir şekilde kullanmayı öğrenmenize yardımcı olması için bu makaledeki bilgileri kullanın.

Azure AD B2C'yi bir Openıd Connect meta veri uç noktası vardır. Uygulama uç noktası, çalışma zamanında Azure AD B2C hakkında bilgi almak için kullanabilirsiniz. Bu bilgiler, uç noktaları, belirteç içeriği ve belirteç imzalama anahtarı içerir. Her kullanıcı Akış Azure AD B2C kiracınızdaki bir JSON meta verileri belgesi yoktur. Örneğin, meta veri belgesi fabrikamb2c.onmicrosoft.com kiracısındaki b2c_1_sign_in kullanıcı akışı için aşağıdaki konumda bulunur:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Bu yapılandırma belgenin özelliklerinden biri `jwks_uri`. Aynı kullanıcı akışı için bir değer olacaktır:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`

Hangi kullanıcı akışı kimlik belirteci imzalamak için kullanılan (ve meta verileri almak nereye) belirlemek için iki seçeneğiniz vardır. İlk olarak, kullanıcı Akış adının yer aldığı `acr` içinde talep `id_token`. Bir kimlik belirteci gelen talepleri ayrıştırma hakkında daha fazla bilgi için bkz: [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). Kullanıcı akışını değerini kodlamak için kullanabileceğiniz diğer seçenek olan `state` isteği gönderdiğinizde parametresi. Ardından, kod çözme `state` hangi kullanıcı akışı kullanılan belirlemek için parametre. Her iki yöntem geçerli değil.

Openıd Connect meta veri uç noktasından meta veri belgesi edindiğiniz sonra kimlik belirteci imzasını doğrulamak için (Bu uç noktada bulunur) 256 RSA ortak anahtarları kullanabilirsiniz. Birden çok anahtar herhangi bir zamanda Bu uç noktada listelenen olabilir, her tarafından tanımlanan bir `kid`. Üst `id_token` de içeren bir `kid` talep. Bu, bu anahtarların hangisinin kimlik belirteci imzalamak için kullanılan gösterir. Hakkında daha fazla bilgi dahil daha fazla bilgi için [belirteçleri doğrulama](active-directory-b2c-reference-tokens.md#token-validation), bkz: [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md).
<!--TODO: Improve the information on this-->

Kimlik belirteci imzası doğruladıktan sonra birkaç talep doğrulama gerektirir. Örneğin:

* Doğrulama `nonce` belirteç yeniden yürütme saldırıları önlemek talep. Oturum açma isteği belirtilen değeri olmalıdır.
* Doğrulama `aud` kimlik belirteci uygulamanız için verilmiş emin olmak talep. Değeri, uygulamanızın uygulama kimliği olmalıdır.
* Doğrulama `iat` ve `exp` kimlik belirteci sona ermediğinden emin olmak talep.

Gerçekleştirmeniz gereken birkaç daha fazla doğrulamaları ayrıntılı olarak açıklanan [Openıd Connect çekirdek Spec](https://openid.net/specs/openid-connect-core-1_0.html). Senaryonuza bağlı olarak ek istekleri doğrulamak isteyebilirsiniz. Bazı ortak doğrulamaları şunlardır:

* Kullanıcı veya kuruluş uygulama için kaydolmuş emin olma.
* Kullanıcı uygun yetkilendirme ve ayrıcalıklara sahip olduğundan emin olmak.
* Belirli bir kimlik doğrulama gücü oluştuğunu gibi Azure multi-Factor Authentication kullanarak olarak sağlama.

Bir kimliği belirteçteki talepleri hakkında daha fazla bilgi için bkz. [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md).

Kimlik belirteci tamamen doğrulandıktan sonra kullanıcı ile oturum başlayabilirsiniz. Uygulamanızda, kullanıcı hakkında bilgi almak için kimliği belirteçteki talepleri kullanın. Bu bilgiler, görüntü, kayıtları, yetkilendirme ve benzeri için kullanılabilir.

## <a name="get-access-tokens"></a>Erişim belirteci alma
Web uygulamalarınızı gereken yapmak için gereken tek şey, kullanıcı akışları yürütme ise, sonraki birkaç bölümlere atlayabilirsiniz. Aşağıdaki bölümlerde yer alan bilgiler, bir web API'sine kimliği doğrulanmış çağrılar yapmak gerekir ve hangi Azure AD B2C tarafından korunan yalnızca web uygulamaları için geçerlidir.

Tek sayfalı uygulamanızı kullanıcı oturumunuz açıldıktan sonra çağıran web güvenliği Azure AD tarafından sağlanan API'ler için erişim belirteçleri elde edebilirsiniz. Zaten bir belirteç kullanarak aldığınız bile `token` yanıt türü, kullanıcı yeniden oturum açmak için yeniden yönlendirme olmadan ek kaynaklar için belirteçlerini almak için bu yöntemi kullanabilirsiniz.

Tipik bir web uygulaması akışı, bu istek yaparak yaptığınız `/token` uç noktası.  Ancak, uç nokta, almak ve belirteçleri yenilemek için AJAX çağrıları yapma bir seçenek değildir, CORS istekleri desteklemez. Bunun yerine, gizli bir HTML iframe öğesi içinde örtük akış diğer web API'leri için yeni belirteçlerini almak için kullanabilirsiniz. Okunaklılık için satır sonları ile bir örnek aşağıda verilmiştir:

```

https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
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
| client_id |Gerekli |Uygulamanıza atanan uygulama kimliği [Azure portalında](https://portal.azure.com). |
| response_type |Gerekli |İçermelidir `id_token` Openıd Connect oturum açma için.  Yanıt türü içerebilir `token`. Kullanırsanız `token` Burada, uygulamanızı hemen bir erişim belirteci authorize uç noktasından ikinci bir istek için Authorize son noktası yapmadan alabilir. Kullanırsanız `token` yanıt türü `scope` parametresi için bir belirteç vermek üzere hangi kaynak gösteren bir kapsam içermelidir. |
| redirect_uri |Önerilen |Yeniden yönlendirme URI'si uygulamanızın, burada kimlik doğrulama yanıtlarının gönderilebilen veya uygulamanız tarafından alındı. URL olarak kodlanmış olmalıdır dışında tam olarak yeniden yönlendirme URI'leri Portalı'nda kayıtlı biriyle eşleşmelidir. |
| scope |Gerekli |Kapsamları boşlukla ayrılmış listesi.  Belirteçleri almak için istenen kaynak için ihtiyaç duyduğunuz tüm kapsamlar içerir. |
| response_mode |Önerilen |Uygulamanıza elde edilen belirteç geri göndermek için kullanılan yöntemi belirtir.  Olabilir `query`, `form_post`, veya `fragment`. |
| durum |Önerilen |Belirteç yanıtta döndürülen isteğinde bulunan bir değer.  Bu, kullanmak istediğiniz herhangi bir içerik dizesi olabilir.  Genellikle, rastgele oluşturulmuş, benzersiz bir değer, siteler arası istek sahteciliği saldırılarına önlemek için kullanılır.  Durum, kimlik doğrulama isteği oluşmadan önce uygulamasında kullanıcının durumu hakkında bilgi kodlamak için de kullanılır. Örneğin, sayfa veya Görünüm kullanıcı açıktı. |
| nonce |Gerekli |Sonuçta elde edilen kimlik belirtecinde talep olarak dahil edilen uygulama tarafından oluşturulan bu isteği dahil bir değer.  Uygulama, belirteç yeniden yürütme saldırıları azaltmak için bu değer daha sonra doğrulayabilirsiniz. Genellikle, isteğin kaynağını tanımlayan bir rastgele, benzersiz bir dize değeridir. |
| istemi |Gerekli |Yenileme ve gizli bir iframe içine belirteçlerini almak için kullandığınız `prompt=none` sağlamak iframe oturum açma sayfasında bir yerde tıkanıp değil ve hemen döndürür. |
| login_hint |Gerekli |Yenileyin ve gizli bir iframe içine belirteçlerini almak için bu ipucu kullanıcı belirli bir zamanda sahip birden çok oturumları arasında ayrım yapmak için kullanıcı adını içerir. Kullanarak bir önceki oturum açma kullanıcı ayıklayabilir `preferred_username` talep. |
| domain_hint |Gerekli |`consumers` veya `organizations` olabilir.  Yenileme ve gizli bir iframe içindeki belirteçleri almak için içermelidir `domain_hint` istek değeri.  Ayıklama `tid` kullanmak üzere hangi değeri belirlemek için bir önceki oturum açma kimlik belirteci talep.  Varsa `tid` değer talep `9188040d-6c67-4c5b-b112-36a304b66dad`, kullanın `domain_hint=consumers`.  Aksi takdirde kullanın `domain_hint=organizations`. |

Ayarlayarak `prompt=none` parametresi, bu isteği ya da başarılı olur veya hemen başarısız olur ve uygulamanıza döndürür.  Başarılı bir yanıt belirtilen yeniden yönlendirme URI'si, uygulamanızı bölümünde belirtilen yöntemi kullanarak gönderilen `response_mode` parametresi.

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
| access_token |İstenen uygulama belirteci. |
| token_type |Belirteç türü her zaman taşıyıcı olur. |
| durum |Varsa bir `state` aynı değeri yanıt olarak görünmesi gereken parametresi istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerleri aynıdır. |
| expires_in |Ne kadar süreyle erişim belirteci (saniye olarak) geçerli değil. |
| scope |Erişim belirteci için geçerli olan kapsamları. |

### <a name="error-response"></a>Hata yanıtı
Uygulama bunları uygun şekilde işleyebilmeniz hata yanıtları yeniden yönlendirme URI'si da gönderilebilir.  İçin `prompt=none`, beklenen hata şöyle görünür:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanılan bir hata kodu dizesi. Dize, hataları tepki vermek için de kullanabilirsiniz. |
| error_description |Belirli bir hata iletisi yardımcı olabilecek bir kimlik doğrulama hatası kök nedenini tanımlayın. |

İframe istekte bu hata iletisini alırsanız kullanıcının etkileşimli olarak tekrar yeni bir belirteç almak oturum açmanız gerekir. Bu uygulama için anlamlı bir şekilde işleyebilir.

## <a name="refresh-tokens"></a>Yenileme belirteçlerini
Kimlik belirteçlerini ve erişim belirteçleri kısa bir süre sonra süresi dolar. Uygulamanızı, bu belirteçleri düzenli aralıklarla yenilenmesi için hazırlanması gerekir.  İki tür belirteci yenilemek için kullandığımız aynı gizli iframe isteği bir önceki örnekte, kullanarak gerçekleştirmek `prompt=none` Azure AD'ye adımları denetlemek için parametre.  Yeni bir almaya `id_token` kullandığınızdan emin olun, değer `response_type=id_token` ve `scope=openid`ve `nonce` parametresi.

## <a name="send-a-sign-out-request"></a>Oturum kapatma isteği gönder
Uygulama dışında kullanıcı oturum açmak istediğinizde, kullanıcı oturumu kapatmak için Azure AD yeniden yönlendirme. Bunu yapmazsanız, kullanıcının kimlik bilgilerini yeniden girmeye gerek kalmadan uygulamanızı yeniden kimlik doğrulaması yapmasını mümkün olabilir. Geçerli tek bir oturum açma oturumu Azure AD ile olacağından budur.

Yalnızca kullanıcıya yönlendirebilirsiniz `end_session_endpoint` meta veri belgesi açıklanan bağlanmak, aynı Openıd içinde listelenen [kimlik belirteci doğrulamak](#validate-the-id-token). Örneğin:

```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
| p |Gerekli |Kullanıcının uygulamanızı imzalamak için kullanılacak ilke. |
| post_logout_redirect_uri |Önerilen |Kullanıcı için sonra yeniden yönlendirilmesi gereken URL'yi başarılı oturum kapatma. Dahil edilmezse, Azure AD B2C kullanıcı için genel bir ileti görüntülenir. |

> [!NOTE]
> Kullanıcıya yönlendiren `end_session_endpoint` bazı Azure AD B2C ile kullanıcının çoklu oturum açma durumunu temizler. Ancak, kullanıcının sosyal kimlik sağlayıcısı oturum dışında kullanıcı oturum değil. Kullanıcının seçtiği aynı sağlayıcısı bir sonraki oturum açma sırasında tanımlamak, kullanıcı, kimlik bilgilerini girmesini olmadan kimlik doğrulaması yeniden yapılır. Bir kullanıcı Azure AD B2C uygulamanızdan oturum isterse, bunu mutlaka tamamen kendi Facebook hesabınız dışında örneğin oturum istedikleri anlamına gelmez. Ancak, yerel hesaplar için kullanıcının oturumunu düzgün sonlandırılır.
> 
> 

## <a name="use-your-own-azure-ad-b2c-tenant"></a>Azure AD B2C kiracınızı kullanın
Bu istekler kendiniz denemek için aşağıdaki üç adımı tamamlayın. Bu makalede kendi değerlerinizle kullandığımız örnek değerleri değiştirin:

1. [Bir Azure AD B2C kiracısı oluşturmayı](active-directory-b2c-get-started.md). Kiracınızın adını isteklerini kullanın.
2. [Uygulama oluşturma](active-directory-b2c-app-registration.md) uygulama Kimliğini almak için ve bir `redirect_uri` değeri. Bir web uygulaması veya web API uygulamanıza dahil edin. İsteğe bağlı olarak, bir uygulama gizli dizisi oluşturabilirsiniz.
3. [Kullanıcı akışlarınızı oluşturun](active-directory-b2c-reference-policies.md) akışı adları, kullanıcı elde edilir.

