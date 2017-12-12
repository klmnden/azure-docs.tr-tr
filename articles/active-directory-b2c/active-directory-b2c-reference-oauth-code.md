---
title: "Yetkilendirme kodu akışı - Azure AD B2C | Microsoft Docs"
description: "Azure AD B2C ve Openıd Connect kimlik doğrulama protokolünü kullanarak Web uygulamaları oluşturmayı öğrenin."
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: mtillman
editor: parakhj
ms.assetid: c371aaab-813a-4317-97df-b62e2f53d865
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: 99a292c6be66016264e528525a5920667207b605
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure Active Directory B2C: OAuth 2.0 yetkilendirme kodu akışı
Web API'leri gibi korunan kaynakları erişim kazanmak için OAuth 2.0 yetkilendirme kodu verme bir aygıtta yüklü uygulamalar kullanabilirsiniz. Azure Active Directory B2C kullanarak (Azure AD B2C) uygulama OAuth 2.0 ekleyebilirsiniz kaydolma, oturum açma ve diğer kimlik yönetimi görevleri, mobil ve Masaüstü uygulamalarınızı. Bu makalede dilden bağımsızdır. Makalede, tüm açık kaynak kitaplıkları kullanmadan HTTP iletileri almasına ve göndermesine değiştireceğinizi açıklar.

<!-- TODO: Need link to libraries -->

OAuth 2.0 yetkilendirme kodu akışını açıklanan [4.1 OAuth 2.0 belirtimi bölüm](http://tools.ietf.org/html/rfc6749). Kimlik doğrulama ve yetkilendirme dahil olmak üzere çoğu uygulama türleri için kullandığınız [web uygulamaları](active-directory-b2c-apps.md#web-apps) ve [uygulamaları'yerel olarak yüklenen](active-directory-b2c-apps.md#mobile-and-native-apps). Güvenli bir şekilde almak için OAuth 2.0 yetkilendirme kodu akışını kullanabilir *erişim belirteçleri* , uygulamalarınız için kullanılabilecek tarafından güvenliği sağlanan kaynaklara erişmek için bir [yetkilendirme sunucusu](active-directory-b2c-reference-protocols.md#the-basics).

Bu makalede odaklanır **ortak istemcileri** OAuth 2.0 yetkilendirme kodu akışı. Ortak bir istemci güvenli bir şekilde gizli bir parola bütünlüğünü güvenilemez herhangi bir istemci uygulama olur. Bu, mobil uygulamaları, Masaüstü uygulamaları ve aslında bir cihazda çalışan ve erişim belirteçleri almak gereken tüm uygulamaları içerir. 

> [!NOTE]
> Azure AD B2C kullanarak bir web uygulaması için Kimlik Yönetimi eklemek için kullanın [Openıd Connect](active-directory-b2c-reference-oidc.md) OAuth 2.0 yerine.

Azure AD B2C birden çok basit kimlik doğrulama ve yetkilendirme yapmak için OAuth 2.0 akar standart genişletir. Tanıttığı [İlkesi parametresi](active-directory-b2c-reference-policies.md). Yerleşik ilkeleriyle, uygulamanız için kullanıcı deneyimleri gibi eklemek için OAuth 2.0 kullanabilirsiniz kaydolma, oturum açma ve profil yönetimi. Bu makalede, sizi, OAuth 2.0 ve ilkeleri her bu deneyimleri yerel uygulamalarınızda uygulamak için nasıl kullanılacağı gösterilmektedir. Ayrıca web API'leri erişmek için erişim belirteçleri almak nasıl gösteriyoruz.

Bu makaledeki örnek HTTP isteklerinde kullanırız bizim örnek Azure AD B2C dizini **fabrikamb2c.onmicrosoft.com**. Ayrıca bizim örnek uygulama ve ilkeleri kullanırız. Bu değerleri kullanarak kendiniz istekleri deneyebilir ya da kendi değerlerinizle değiştirin.
Bilgi edinmek için nasıl [kendi Azure AD B2C dizini, uygulama ve ilkeleri alma](#use-your-own-azure-ad-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. Bir yetkilendirme kodu alın
Yetkilendirme kodu akışını kullanıcıya yönlendirerek istemci ile başlayan `/authorize` uç noktası. Burada kullanıcının eylemde akışının etkileşimli parçası budur. İstemci bu istekte gösterir `scope` parametresi, kullanıcıdan almak için gereken izinleri. İçinde `p` parametresi, yürütmek için ilke gösterir. Aşağıdaki üç örnekler (okunabilirlik için satır sonuyla) her başka bir ilke kullanın.

### <a name="use-a-sign-in-policy"></a>Bir oturum açma ilkesini kullanın
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-policy"></a>Kayıt İlkesi'ni kullanın
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-policy"></a>Bir profili Düzenle ilkesini kullanın
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
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
| client_id |Gerekli |Uygulamanıza atanan uygulama kimliği [Azure portal](https://portal.azure.com). |
| response_type |Gerekli |İçermelidir yanıt türü `code` yetkilendirme kodu akışı. |
| redirect_uri |Gerekli |Yeniden yönlendirme URI'si kimlik doğrulaması yanıtları nereden gönderildiğini ve uygulamanız tarafından alındığını, uygulamanızın. URL kodlanmış olmalıdır dışında tam olarak yeniden yönlendirme Portalı'nda kayıtlı URI'ler biriyle eşleşmelidir. |
| Kapsam |Gerekli |Kapsamları boşlukla ayrılmış listesi. Tek bir kapsam değeri Azure Active Directory'ye (Azure AD) her iki talep edilen izinler gösterir. Uygulamanızın kendi hizmeti veya web API karşı kullanılabilir bir erişim belirteci gerektiğini kapsamı anlaşılacağı gibi istemci kimliği kullanılarak, aynı istemci kimliğine göre temsil  `offline_access` Kapsamını uygulamanızın uzun süreli kaynaklarına erişim için bir yenileme belirteci gerektiğini belirtir. Aynı zamanda `openid` Azure AD B2C ' bir kimliği belirteç istemek için kapsam. |
| response_mode |Önerilen |Ortaya çıkan yetkilendirme kodu uygulamanıza geri göndermek için kullandığı yöntem. Bu olabilir `query`, `form_post`, veya `fragment`. |
| durum |Önerilen |Belirteç yanıtta döndürülen istek dahil bir değer. Kullanmak istediğiniz herhangi bir içerik dizesi olabilir. Genellikle, rastgele oluşturulan benzersiz bir değer, siteler arası istek sahtekarlığı saldırıları önlemek için kullanılır. Durum, kimlik doğrulama isteği oluşmadan önce uygulama kullanıcının durumu hakkındaki bilgileri kodlamak için de kullanılır. Örneğin, kullanıcı açıktı sayfa veya yürütülmekte olan ilke. |
| P |Gerekli |Yürütülen ilkesi. Azure AD B2C dizininizde oluşturulan bir ilkeyi adıdır. İlke adı değeri ile başlaması gereken **b2c\_1\_**. İlkeleri hakkında daha fazla bilgi için bkz: [Azure AD B2C yerleşik ilkeleri](active-directory-b2c-reference-policies.md). |
| istemi |İsteğe bağlı |Gerekli bir kullanıcı etkileşimi türü. Şu anda, yalnızca geçerli değer: `login`, kullanıcının bu isteği kimlik bilgilerini girmesini zorlar. Çoklu oturum açma etkili olmaz. |

Bu noktada, ilkenin iş akışını tamamlamak için kullanıcının istedi. Bu dizini ya da başka bir sayıyı adımları için kaydolan bir sosyal kimlik bilgilerinizle oturum, kullanıcı adını ve parolasını girerek kullanıcı gerektirebilir. İlkenin nasıl tanımlanan kullanıcı eylemlerini bağlıdır.

Kullanıcı ilkeyi tamamladıktan sonra Azure AD için kullanılır ve uygulamanızın değerde bir yanıt döndürür `redirect_uri`. Bölümünde belirtilen yöntemi kullanan `response_mode` parametresi. Yanıt tam olarak yürütülen ilke bağımsız kullanıcı eylemi senaryoların her biri için aynıdır.

Kullanan başarılı yanıt `response_mode=query` şöyle görünür:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parametre | Açıklama |
| --- | --- |
| Kod |Uygulama istenen yetkilendirme kodu. Uygulama, bir hedef kaynak için bir erişim belirteci istemek için yetkilendirme kodu kullanabilirsiniz. Yetkilendirme kodları çok kısa ömürlüdür. Genellikle, yaklaşık 10 dakika sonra süresi. |
| durum |Önceki bölümde tablosundaki tam açıklamasına bakın. Varsa bir `state` parametresi, aynı değeri yanıt olarak görünmesi gereken istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerler aynı. |

Böylece uygulama bunları uygun şekilde işleyebilir hata yanıtları yeniden yönlendirme URI'si da gönderilebilir:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hataları türlerini sınıflandırmak için kullanabileceğiniz bir hata kodu dizesi. Dize, hataları tepki vermek için de kullanabilirsiniz. |
| error_description |Yardımcı olabilecek belirli bir hata iletisi kimlik doğrulama hatası kök nedenini tanımlayın. |
| durum |Önceki tabloda tam açıklamasına bakın. Varsa bir `state` parametresi, aynı değeri yanıt olarak görünmesi gereken istekte bulunur. Uygulama olduğunu doğrulamanız gerekir `state` istek ve yanıt değerler aynı. |

## <a name="2-get-a-token"></a>2. Belirteç alın
Bir yetkilendirme kodu edindiğiniz, kullanmak `code` bir POST isteği göndererek istenen kaynak için bir belirteç için `/token` uç noktası. Azure AD B2C'de, uygulamanın kendi arka uç web API için bir belirteç isteği yalnızca kaynak değil. Kendinize bir belirteç istemek için kullanılan kuralı, uygulamanın istemci Kimliğini kapsamı olarak kullanmaktır:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
| P |Gerekli |Yetkilendirme kodu almak için kullanılan bir ilke. Bu isteği başka bir ilke kullanamazsınız. Bu parametreyi eklemek Not *sorgu dizesi*, POST gövdesinde yok. |
| client_id |Gerekli |Uygulamanıza atanan uygulama kimliği [Azure portal](https://portal.azure.com). |
| grant_type |Gerekli |Grant türü. Yetkilendirme kod akış için sağlama türü olmalıdır `authorization_code`. |
| Kapsam |Önerilen |Kapsamları boşlukla ayrılmış listesi. Azure AD'ye istenecek izinlerin hem tek bir kapsam değeri gösterir. Uygulamanızın kendi hizmeti veya web API karşı kullanılabilir bir erişim belirteci gerektiğini kapsamı anlaşılacağı gibi istemci kimliği kullanılarak, aynı istemci kimliğine göre temsil  `offline_access` Kapsamını uygulamanızın uzun süreli kaynaklarına erişim için bir yenileme belirteci gerektiğini belirtir.  Aynı zamanda `openid` Azure AD B2C ' bir kimliği belirteç istemek için kapsam. |
| Kod |Gerekli |Akış ilk leg içinde aldığınız yetkilendirme kodu. |
| redirect_uri |Gerekli |Yeniden yönlendirme URI'si yetkilendirme kodu aldığınız uygulama. |

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
| not_before |Hangi belirtecin geçerli dönem zamanında değerlendirilir süre. |
| token_type |Belirteç türü değeri. Azure AD destekleyen tek taşıyıcı türüdür. |
| access_token |İmzalı JSON Web Token (istediğiniz JWT). |
| Kapsam |Belirteç için geçerli olur; kapsam. Önbellek belirteçleri kapsamına daha sonra kullanmak üzere de kullanabilirsiniz. |
| expires_in |Süreyi (saniye olarak) geçerli bir belirteçtir. |
| refresh_token |Bir OAuth 2.0 yenileme belirteci. Uygulama geçerli belirtecin süresi dolduktan sonra ek belirteçleri almak için bu belirteci kullanabilirsiniz. Yenileme belirteçleri uzun süreli. Uzun süre için kaynaklara erişimi korumak için bunları kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). |

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
| error_description |Yardımcı olabilecek belirli bir hata iletisi kimlik doğrulama hatası kök nedenini tanımlayın. |

## <a name="3-use-the-token"></a>3. Belirteci kullanın
Bir erişim belirteci başarıyla edindiğiniz, belirteç isteklerini kullanabilirsiniz dahil ederek API'leri, arka uç web `Authorization` üstbilgisi:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4. Belirteç Yenile
Erişim belirteçleri ve kimlik belirteçlerini ömürlüdür. Zaman aşımına uğradığında kaynaklara erişmeye devam etmek için bunları yenilemeniz gerekir. Bunu yapmak için başka bir POST isteği gönderin `/token` uç noktası. Bu süre sağlamak `refresh_token` yerine `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
| P |Gerekli |Özgün yenileme belirtecini almak için kullanılan bir ilke. Bu isteği başka bir ilke kullanamazsınız. Bu parametreyi eklemek Not *sorgu dizesi*, POST gövdesinde yok. |
| client_id |Önerilen |Uygulamanıza atanan uygulama kimliği [Azure portal](https://portal.azure.com). |
| grant_type |Gerekli |Grant türü. Bu yetkilendirme kodu akışını Bacak için sağlama türü olmalıdır `refresh_token`. |
| Kapsam |Önerilen |Kapsamları boşlukla ayrılmış listesi. Azure AD'ye istenecek izinlerin hem tek bir kapsam değeri gösterir. Uygulamanızın kendi hizmeti veya web API karşı kullanılabilir bir erişim belirteci gerektiğini kapsamı anlaşılacağı gibi istemci kimliği kullanılarak, aynı istemci kimliğine göre temsil  `offline_access` Kapsamını belirtir, uygulamanızın uzun süreli kaynaklarına erişim için bir yenileme belirteci gerekir.  Aynı zamanda `openid` Azure AD B2C ' bir kimliği belirteç istemek için kapsam. |
| redirect_uri |İsteğe bağlı |Yeniden yönlendirme URI'si yetkilendirme kodu aldığınız uygulama. |
| refresh_token |Gerekli |Akış ikinci leg içinde aldığınız özgün yenileme belirteci. |

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
| not_before |Hangi belirtecin geçerli dönem zamanında değerlendirilir süre. |
| token_type |Belirteç türü değeri. Azure AD destekleyen tek taşıyıcı türüdür. |
| access_token |İstediğiniz imzalı JWT. |
| Kapsam |Belirteç için geçerli olur; kapsam. Daha sonra kullanmak üzere kapsamları önbellek belirteçleri için de kullanabilirsiniz. |
| expires_in |Süreyi (saniye olarak) geçerli bir belirteçtir. |
| refresh_token |Bir OAuth 2.0 yenileme belirteci. Uygulama geçerli belirtecin süresi dolduktan sonra ek belirteçleri almak için bu belirteci kullanabilirsiniz. Yenileme belirteçleri uzun süreli ve uzun süre için kaynaklara erişimi korumak için kullanılabilir. Daha fazla bilgi için bkz: [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md). |

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
| error_description |Yardımcı olabilecek belirli bir hata iletisi kimlik doğrulama hatası kök nedenini tanımlayın. |

## <a name="use-your-own-azure-ad-b2c-directory"></a>Kendi Azure AD B2C dizini kullanın
Bu istekler kendiniz denemek için aşağıdaki adımları tamamlayın. Bu makalede, kendi değerlerle kullandık örnek değerleri değiştirin.

1. [Azure AD B2C dizini oluşturma](active-directory-b2c-get-started.md). Dizininizin adını isteklerinde kullanın.
2. [Uygulama oluşturma](active-directory-b2c-app-registration.md) bir uygulama kimliği ve yeniden yönlendirme URI'si elde edilir. Yerel istemci uygulamanızı içerir.
3. [İlkelerinizi oluşturma](active-directory-b2c-reference-policies.md) ilke adları elde edilir.

