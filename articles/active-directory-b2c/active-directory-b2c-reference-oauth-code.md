---
title: Yetkilendirme kodu akışı - Azure Active Directory B2C | Microsoft Docs
description: Azure AD B2C'yi ve Openıd Connect kimlik doğrulama protokolü kullanarak Web uygulamaları oluşturmayı öğrenin.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 4ee67f07965036a71151d7b6a5092b9a76d94999
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60317165"
---
# <a name="oauth-20-authorization-code-flow-in-azure-active-directory-b2c"></a>Azure Active Directory B2C, OAuth 2.0 yetkilendirme kod akışı

Web API'leri gibi korunan kaynakları erişim kazanmak için OAuth 2.0 yetkilendirme kodu verme bir cihazda yüklü uygulamaların kullanabilirsiniz. Azure Active Directory B2C kullanarak (Azure AD B2C) uygulama OAuth 2.0 ekleyebilirsiniz kaydolma, oturum açma ve diğer kimlik yönetimi görevleri, mobil ve Masaüstü uygulamaları için. Bu makalede dilden bağımsızdır. Makalede, biz nasıl HTTP iletileri gönderip herhangi bir açık kaynak kitaplıkları kullanmadan açıklar.

OAuth 2.0 yetkilendirme kod akışı açıklanan [OAuth 2.0 belirtiminin 4.1 bölümünde](https://tools.ietf.org/html/rfc6749). Kimlik doğrulaması ve yetkilendirme çoğu kullanabilirsiniz [uygulama türleri](active-directory-b2c-apps.md)web uygulamaları ve yerel olarak yüklü uygulamalar dahil olmak üzere. OAuth 2.0 yetkilendirme kod akışı tarafından güvenliği sağlanan kaynaklara erişmek için kullanılan, uygulamalarınız için yenileme belirteçlerini ve güvenli bir şekilde erişim belirteçlerini almak için kullanabileceğiniz bir [yetkilendirme sunucusu](active-directory-b2c-reference-protocols.md).  Erişim belirtecinin süresi sona erdiğinde, yenileme belirtecinin yeni erişim (Yenile) belirteçleri istemcinin ve genellikle bir saat sonra sağlar.

Bu makalede odaklanır **genel istemcilere** OAuth 2.0 yetkilendirme kod akışı. Genel bir istemci gizli parolasını bütünlüğünü güvenli bir şekilde korumak için güvenilir olmayan tüm istemci uygulamasıdır. Bu, mobil uygulamaları, Masaüstü uygulamaları ve aslında bir cihazda çalışan ve erişim belirteçlerini almak gereken tüm uygulamaları içerir. 

> [!NOTE]
> Azure AD B2C kullanarak bir web uygulaması için Kimlik Yönetimi eklemek için [Openıd Connect](active-directory-b2c-reference-oidc.md) OAuth 2.0 yerine.

Azure AD B2C, standart OAuth 2.0 daha basit kimlik doğrulaması ve yetkilendirme akışları genişletir. Tanıttığı [kullanıcı akışı parametre](active-directory-b2c-reference-policies.md). Kullanıcı akışları ile kullanıcı deneyimlerini uygulamanıza gibi eklemek için OAuth 2.0 kullanabilirsiniz kaydolma, oturum açma ve profil yönetimi. OAuth 2.0 protokolünü kullanan kimlik sağlayıcılarını içerir [Amazon](active-directory-b2c-setup-amzn-app.md), [Azure Active Directory](active-directory-b2c-setup-oidc-azure-active-directory.md), [Facebook](active-directory-b2c-setup-fb-app.md), [GitHub](active-directory-b2c-setup-github-app.md), [ Google](active-directory-b2c-setup-goog-app.md), ve [LinkedIn](active-directory-b2c-setup-li-app.md).

Bizim örnek Azure AD B2C dizini kullandığımız bu makaledeki örnek HTTP isteklerinde **fabrikamb2c.onmicrosoft.com**. Bizim örnek uygulama ve kullanıcı akışları de kullanırız. Bu değerleri kullanarak kendiniz istekleri deneyebilir ya da kendi değerlerinizle değiştirin.
Bilgi edinmek için nasıl [kendi Azure AD B2C dizini, uygulama ve kullanıcı Akışları Al](#use-your-own-azure-ad-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. Bir yetkilendirme kodunu alma
Kullanıcıya yönlendiren istemci yetkilendirme kod akışı başlar `/authorize` uç noktası. Bu etkileşimli akış burada kullanıcının eylemde parçasıdır. İstemci bu istekte gösterir `scope` parametresi, kullanıcıdan almak için gereken izinleri. İçinde `p` parametresi yürütmek için kullanıcı akışı gösterir. Aşağıdaki üç örnekler (okunabilirlik için satır sonuyla) her farklı kullanıcı akışı kullanın.

### <a name="use-a-sign-in-user-flow"></a>Bir kullanıcı oturum açma akışını kullanın
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-user-flow"></a>Kaydolma kullanıcı akışı kullanın
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-user-flow"></a>Bir profili Düzenle kullanıcı akışını kullanın
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
| client_id |Gerekli |Uygulamanıza atanan uygulama kimliği [Azure portalında](https://portal.azure.com). |
| response_type |Gerekli |İçermelidir yanıt türü `code` yetkilendirme kod akışı için. |
| redirect_uri |Gerekli |Yeniden yönlendirme URI'si, uygulamanız tarafından alınan kimlik doğrulama yanıtlarının burada gönderilen ve uygulama. URL olarak kodlanmış olmalıdır dışında tam olarak yeniden yönlendirme Portalı'nda kayıtlı bir URI'leri biriyle eşleşmelidir. |
| scope |Gerekli |Kapsamları boşlukla ayrılmış listesi. Azure Active Directory'ye (Azure AD) hem talep edilen izinler, tek bir kapsam değeri gösterir. Kapsamı uygulamanızı kendi hizmeti veya web API karşı kullanılabilir bir erişim belirteci gerektiğini belirtir. istemci Kimliğini kullanarak aynı istemci kimliği ile temsil edilen  `offline_access` Kapsamını belirtir uygulamanıza kaynaklarına uzun süreli erişim için bir yenileme belirteci gerekiyor. Ayrıca `openid` Azure AD B2C'den bir kimlik belirteci istemek için kapsam. |
| response_mode |Önerilen |Ortaya çıkan bir yetkilendirme kodu uygulamanıza geri göndermek için kullandığı yöntem. Bu olabilir `query`, `form_post`, veya `fragment`. |
| durum |Önerilen |Kullanmak istediğiniz herhangi bir içeriği bir dize olabilir isteğinde bir değer. Genellikle, rastgele oluşturulmuş bir benzersiz değeri, siteler arası istek sahteciliği saldırılarına önlemek için kullanılır. Durum, kimlik doğrulama isteği oluşmadan önce uygulamasında kullanıcının durumu hakkında bilgi kodlamak için de kullanılır. Örneğin, sayfa üzerindeki kullanıcı tarafından veya yürütülmekte olan kullanıcı akışı. |
| p |Gerekli |Yürütülen kullanıcı akışını. Azure AD B2C dizininizde oluşturulmuş bir kullanıcı akışı adıdır. Kullanıcı akışı ad değer ile başlaması gereken **b2c\_1\_**. Kullanıcı akışları hakkında daha fazla bilgi için bkz: [Azure AD B2C kullanıcı akışları](active-directory-b2c-reference-policies.md). |
| istemi |İsteğe bağlı |Gerekli olan kullanıcı etkileşimi türü. Şu anda geçerli olan `login`, bu isteği kimlik bilgilerini girmesini zorlar. Çoklu oturum açma etkili olmaz. |

Bu noktada, kullanıcı kullanıcı Akış iş akışı tamamlamanız istenir. Bu dizinin veya herhangi bir adım sayısı için kaydolan bir sosyal kimlik bilgilerinizle oturum, kullanıcı adını ve parolasını girerek kullanıcı gerektirebilir. Kullanıcı akışı nasıl tanımlandığını kullanıcı eylemlerine bağlıdır.

Kullanıcının kullanıcı akışı tamamlandıktan sonra Azure AD uygulamanız için kullanılan değerde bir yanıt döndürür `redirect_uri`. Bölümünde belirtilen yöntemi kullanan `response_mode` parametresi. Yanıt tam olarak yürütülen kullanıcı akışını bağımsız kullanıcı eylem senaryoların her biri için aynıdır.

Kullanan başarılı bir yanıt `response_mode=query` şöyle görünür:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parametre | Açıklama |
| --- | --- |
| kod |Uygulama talep yetkilendirme kodu. Uygulama, bir hedef kaynak için bir erişim belirteci istemek için yetkilendirme kodu kullanabilirsiniz. Yetkilendirme kodları çok kısa ömürlüdür. Genellikle, yaklaşık 10 dakika sonra süresi. |
| durum |Önceki bölümde bulunan tablodaki tam açıklamasına bakın. Varsa bir `state` aynı değeri yanıt olarak görünmesi gereken parametresi istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerleri aynıdır. |

Uygulama bunları uygun şekilde işleyebilmeniz hata yanıtları da yeniden yönlendirme URI'si gönderilebilir:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanabileceğiniz bir hata kodu dizesi. Dize, hataları tepki vermek için de kullanabilirsiniz. |
| error_description |Belirli bir hata iletisi yardımcı olabilecek bir kimlik doğrulama hatası kök nedenini tanımlayın. |
| durum |Önceki tabloda tam açıklamasına bakın. Varsa bir `state` aynı değeri yanıt olarak görünmesi gereken parametresi istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerleri aynıdır. |

## <a name="2-get-a-token"></a>2. Bir belirteç Al
Bir yetkilendirme kodu edindiğiniz, kullanmak `code` bir POST isteği göndererek istenen kaynak için bir belirteç için `/token` uç noktası. Azure AD B2C'de, uygulamanın kendi arka uç web API'si için bir belirteç isteğinde bulunabilirsiniz yalnızca kaynak var. Kendiniz için bir belirteç istemek için kullanılan kuralı, kapsam olarak uygulamanızın istemci kimliği kullanmaktır:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
| p |Gerekli |Yetkilendirme kodunu almak için kullanılan kullanıcı akışı. Bu istekte farklı kullanıcı akışı kullanamazsınız. Bu parametre için eklediğiniz Not *sorgu dizesi*, POST gövdesini içinde değil. |
| client_id |Gerekli |Uygulamanıza atanan uygulama kimliği [Azure portalında](https://portal.azure.com). |
| grant_type değeri |Gerekli |Verme türü. Yetkilendirme kod akışı, izin verme türü olmalıdır `authorization_code`. |
| scope |Önerilen |Kapsamları boşlukla ayrılmış listesi. Azure AD'ye istenecek izinlerin hem tek bir kapsam değeri gösterir. Kapsamı uygulamanızı kendi hizmeti veya web API karşı kullanılabilir bir erişim belirteci gerektiğini belirtir. istemci Kimliğini kullanarak aynı istemci kimliği ile temsil edilen  `offline_access` Kapsamını belirtir uygulamanıza kaynaklarına uzun süreli erişim için bir yenileme belirteci gerekiyor.  Ayrıca `openid` Azure AD B2C'den bir kimlik belirteci istemek için kapsam. |
| kod |Gerekli |Akışın ilk oluşturan içinde alınan yetkilendirme kodu. |
| redirect_uri |Gerekli |Yeniden yönlendirme URI'sini yetkilendirme kodu aldığınız uygulama. |

Başarılı bir token yanıt şöyle görünür:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametre | Açıklama |
| --- | --- |
| not_before |Saat, belirteç geçerli dönem zamanı kabul edilir. |
| token_type |Belirteç türü değeri. Azure AD destekleyen tek taşıyıcı türüdür. |
| access_token |İmzalı JSON Web Token (istediğiniz JWT). |
| scope |İçin belirteç geçerliyse kapsamları. Önbellek belirteçleri kapsamları daha sonra kullanmak üzere de kullanabilirsiniz. |
| expires_in |Süreyi (saniye olarak) geçerli bir belirteçtir. |
| refresh_token |OAuth 2.0 yenileme belirteci. Uygulama geçerli belirtecin süresi dolduktan sonra ek belirteçlerini almak için bu belirteci kullanabilirsiniz. Uzun süreli yenileme belirteçleri. Uzun süre için kaynaklarına erişimi korumak için bunları kullanabilirsiniz. Daha fazla bilgi için [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). |

Hata yanıtları şöyle görünür:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanabileceğiniz bir hata kodu dizesi. Dize, hataları tepki vermek için de kullanabilirsiniz. |
| error_description |Belirli bir hata iletisi yardımcı olabilecek bir kimlik doğrulama hatası kök nedenini tanımlayın. |

## <a name="3-use-the-token"></a>3. Belirteci kullanın
Bir erişim belirteci başarıyla edindiğiniz, belirteç isteklerini kullanabilirsiniz arka ucunuz için web API'leri dahil ederek `Authorization` üst bilgi:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4. Belirteci Yenile
Erişim ve kimliği belirteçler kısa ömürlüdür. Süresi dolan kaynaklara erişmeye devam etmek için bunları yenilemeniz gerekir. Bunu yapmak için başka bir POST isteği gönderin `/token` uç noktası. Bu süre `refresh_token` yerine `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&client_secret=JqQX2PNo9bpM0uEihUPzyrh&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
| p |Gerekli |Özgün yenileme belirteci almak için kullanılan kullanıcı akışı. Bu istekte farklı kullanıcı akışı kullanamazsınız. Bu parametre için eklediğiniz Not *sorgu dizesi*, POST gövdesini içinde değil. |
| client_id |Gerekli |Uygulamanıza atanan uygulama kimliği [Azure portalında](https://portal.azure.com). |
| client_secret |Gerekli |İçinde client_id ilişkili client_secret [Azure portalında](https://portal.azure.com). |
| grant_type değeri |Gerekli |Verme türü. Bu yetkilendirme kod akışı oluşturan için izin verme türü olmalıdır `refresh_token`. |
| scope |Önerilen |Kapsamları boşlukla ayrılmış listesi. Azure AD'ye istenecek izinlerin hem tek bir kapsam değeri gösterir. Kapsamı uygulamanızı kendi hizmeti veya web API karşı kullanılabilir bir erişim belirteci gerektiğini belirtir. istemci Kimliğini kullanarak aynı istemci kimliği ile temsil edilen  `offline_access` Kapsamını belirtir uygulamanızı kaynaklarına uzun süreli erişim için bir yenileme belirteci gerekir.  Ayrıca `openid` Azure AD B2C'den bir kimlik belirteci istemek için kapsam. |
| redirect_uri |İsteğe bağlı |Yeniden yönlendirme URI'sini yetkilendirme kodu aldığınız uygulama. |
| refresh_token |Gerekli |Akışın ikinci oluşturan içinde alınan özgün bir yenileme belirteci. |

Başarılı bir token yanıt şöyle görünür:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametre | Açıklama |
| --- | --- |
| not_before |Saat, belirteç geçerli dönem zamanı kabul edilir. |
| token_type |Belirteç türü değeri. Azure AD destekleyen tek taşıyıcı türüdür. |
| access_token |İstediğiniz imzalı JWT. |
| scope |İçin belirteç geçerliyse kapsamları. Önbellek belirteçleri kapsamların daha sonra kullanmak üzere de kullanabilirsiniz. |
| expires_in |Süreyi (saniye olarak) geçerli bir belirteçtir. |
| refresh_token |OAuth 2.0 yenileme belirteci. Uygulama geçerli belirtecin süresi dolduktan sonra ek belirteçlerini almak için bu belirteci kullanabilirsiniz. Yenileme belirteçleri uzun süreli ve uzun süre için kaynaklarına erişimi korumak için kullanılabilir. Daha fazla bilgi için [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). |

Hata yanıtları şöyle görünür:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanabileceğiniz bir hata kodu dizesi. Dize, hataları tepki vermek için de kullanabilirsiniz. |
| error_description |Belirli bir hata iletisi yardımcı olabilecek bir kimlik doğrulama hatası kök nedenini tanımlayın. |

## <a name="use-your-own-azure-ad-b2c-directory"></a>Kendi Azure AD B2C dizinini kullanın
Bu istekler kendiniz denemek için aşağıdaki adımları tamamlayın. Bu makalede kendi değerlerinizle kullandığımız örnek değerler değiştirin.

1. [Bir Azure AD B2C dizini oluşturma](active-directory-b2c-get-started.md). Dizininizin adını isteklerini kullanın.
2. [Uygulama oluşturma](active-directory-b2c-app-registration.md) bir uygulama kimliği ve yeniden yönlendirme URI'si elde edilir. Yerel bir istemci uygulamanıza dahil edin.
3. [Kullanıcı akışlarınızı oluşturun](active-directory-b2c-reference-policies.md) akışı adları, kullanıcı elde edilir.

