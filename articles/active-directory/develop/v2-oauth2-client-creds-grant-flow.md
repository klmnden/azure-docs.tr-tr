---
title: Kullanıcı etkileşimi olmadan güvenli kaynaklara erişmek için Azure AD v2.0 kullanın | Microsoft Docs
description: Azure AD uygulaması OAuth 2.0 kimlik doğrulama protokolü kullanarak Web uygulamaları oluşturun.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 9b7cfbd7-f89f-4e33-aff2-414edd584b07
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 3ecf837af735b97e269eb3fdc01d2e56ec40fb6e
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624514"
---
# <a name="azure-active-directory-v20-and-the-oauth-20-client-credentials-flow"></a>Azure Active Directory v2.0 ve OAuth 2.0 istemci kimlik bilgileri akışı

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Kullanabileceğiniz [OAuth 2.0 istemci kimlik bilgileri verme](http://tools.ietf.org/html/rfc6749#section-4.4) RFC olarak da adlandırılır 6749 içinde belirtilen *iki bacaklı OAuth*, uygulamanın kimliğini kullanarak web barındırılan kaynaklara erişmek için. Hemen bir kullanıcı etkileşimi olmadan arka planda çalışması gereken sunucudan sunucuya etkileşimleri için bu tür bir verme yaygın olarak kullanılır. Bu tür uygulamalar genellikle olarak ifade edilir *Daemon'ları* veya *hizmet hesapları*.

OAuth 2.0 istemci kimlik bilgileri, başka bir web hizmetini çağırırken bir kullanıcının kimliğine bürünmek yerine kendi kimlik bilgilerini kullanmak için bir web hizmetinin (gizli istemci) akış izinleri verin. Bu senaryoda istemci genellikle bir orta katman web hizmeti, arka plan programı hizmeti veya web sitesi olur. Daha yüksek bir güvence düzeyi için Azure Active Directory (Azure AD) kimlik bilgisi olarak (bir paylaşılan gizlilik) yerine bir sertifika kullanmak arama hizmeti de sağlar.

> [!NOTE]
> V2.0 uç noktası, tüm Azure AD senaryoları ve özellikleri desteklemiyor. V2.0 uç noktası kullanması gerekip gerekmediğini belirlemek için aşağıdaki hakkında bilgi edinin: [v2.0 sınırlamaları](active-directory-v2-limitations.md).

Daha fazla tipik *üç bacaklı OAuth*, bir istemci uygulamanın belirli bir kullanıcı adına bir kaynağa erişim izni verilir. İzin, genellikle sırasında uygulamaya kullanıcıdan görevlendirildiğini [onay](v2-permissions-and-consent.md) işlem. Ancak, istemci kimlik bilgileri akışı, uygulamanın kendisi doğrudan izinleri verilir. Kaynak bir kaynak için bir belirteç, zorunlu uygulama sunar uygulamanın kendi bir eylem ve değil, gerçekleştirme yetkisi varsa kullanıcı yetkilendirme sahiptir.

## <a name="protocol-diagram"></a>Protokol diyagramı

Tüm istemci kimlik bilgileri akışı, aşağıdaki diyagrama benzer. Bu makaledeki adımların her biri açıklanmaktadır.

![İstemci kimlik bilgileri akışı](./media/v2-oauth2-client-creds-grant-flow/convergence_scenarios_client_creds.png)

## <a name="get-direct-authorization"></a>Doğrudan yetkilendirme alın

Bir uygulama genellikle iki yöntemden biriyle bir kaynağa erişmek için doğrudan yetkilendirme alır: 

* Erişim denetimi listesi (ACL) kaynakta
* Azure AD'de uygulama iznini atama

Bu iki yöntem Azure AD'de en yaygın olarak bulunur ve bunları istemcilerin ve istemci gerçekleştirmek kaynaklar için kimlik bilgileri akışı öneririz. Ancak herhangi bir şekilde istemcileri yetkilendirmek bir kaynak seçebilirsiniz. Her kaynak sunucu, uygulama için en anlamlı yöntemi seçebilirsiniz.

### <a name="access-control-lists"></a>Erişim denetimi listeleri

Bir kaynak sağlayıcısı uygulama (istemci) bilir ve belirli bir düzeyde erişim verir kimlikleri listesini dayalı bir yetkilendirme onay zorlayabilir. Kaynak v2.0 uç noktasından bir belirteç aldığında, bu belirteç kodunu çözme ve istemci uygulama kimliği ayıklamak `appid` ve `iss` talep. Ardından uygulama sakladığı bir erişim denetimi listesi (ACL) karşı karşılaştırır. ACL'leri ayrıntı düzeyi ve yöntemi, kaynakları arasında önemli ölçüde değişiklik gösterebilir.

Genel bir kullanım örneği, bir web uygulaması veya web API'si için testleri çalıştırmak için bir ACL kullanmaktır. Web API'si, belirli bir istemci için yalnızca bir alt kümesini tam izinleri verebilir. API üzerinde uçtan uca testleri çalıştırmak için v2.0 uç noktasından belirteç edinme ve ardından bunları API'sine gönderir. bir test istemcisi oluşturun. API bu durumda test istemci uygulama kimliği API'nin tamamını işlevine tam erişim ACL'si denetler. Bu tür bir ACL kullanırsanız, yalnızca arayanın doğruladığınızdan emin olması `appid` değer ancak aynı zamanda doğrulamak `iss` belirteç değeri güvenilir.

Bu tür bir kimlik doğrulama, Daemon'ları ve kişisel Microsoft hesabına sahip tüketici kullanıcılara ait verilere erişim gerektiren hizmet hesapları için yaygındır. Kuruluşlar tarafından ait veriler için gerekli yetki aracılığıyla uygulama izinleri alma öneririz.

### <a name="application-permissions"></a>Uygulama izinleri

ACL'ler kullanmak yerine, uygulama izinleri kümesi kullanıma sunmak için API'leri kullanabilirsiniz. Bir uygulama izni bir uygulamaya, bir kuruluşun Yöneticisi tarafından verilir ve yalnızca, kuruluş ve çalışanlarına tarafından sahip olunan veri erişimi için kullanılabilir. Örneğin, Microsoft Graph, aşağıdakileri yapmak için birkaç uygulama izinleri kullanıma sunar:

* Tüm posta kutularındaki postaları okuma
* Tüm posta kutularındaki postaları okuma ve yazma
* Herhangi bir kullanıcı adına posta gönderme
* Dizin verilerini okuma

Uygulama izinleri hakkında daha fazla bilgi için Git [Microsoft Graph](https://developer.microsoft.com/graph).

Uygulama izinleri, uygulamanızda kullanmak için sonraki bölümde açıklanan adımları izleyin.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Uygulama kayıt portalında izinlere ilişkin istek

1. Kaydetme ve uygulama üzerinden oluşturma [uygulama kayıt portalı](quickstart-v2-register-an-app.md) veya yeni [uygulama kayıtları (Önizleme) deneyimi](quickstart-register-app.md).
1. Uygulamanızı kaydetmek veya uygulamanızı oluşturmak için kullanılan portalında gidin. Uygulamanızı oluşturduğunuzda en az bir uygulama gizli anahtarı kullanmanız gerekir.
2. Bulun **Microsoft Graph izinleri** bölümüne ve ardından ekleyin **uygulama izinleri** , uygulamanız için gerekli.
3. **Kaydet** uygulama kaydı.

#### <a name="recommended-sign-the-user-in-to-your-app"></a>Önerilir: kullanıcı uygulamanızda oturum

Genellikle, uygulama izinleri kullanan bir uygulama oluşturduğunuzda, uygulama bir sayfa ya da görünüm üzerinde yönetici uygulamanın izinlerini onaylar gerekiyor. Bu sayfada uygulamanın oturum açma akışı, uygulama ayarlarının parçası parçası olabilir veya ayrılmış "Bağlan" akış olabilir. Çoğu durumda, "yalnızca bir kullanıcı bir iş veya Okul hesabı Microsoft ile imzaladığı sonra bu göstermek uygulama için Görünüm Bağlan" mantıklıdır.

Kullanıcı uygulamanızda oturum açarsa, uygulama izinleri onaylamak için kullanıcıya sor önce kullanıcının ait olduğu kuruluş tanımlayabilirsiniz. Kesinlikle gerekli olmasa da, kullanıcılarınızın daha sezgisel bir deneyim oluşturmanıza yardımcı olur. Kullanıcının oturum açmasını için izleyin bizim [v2.0 protokol öğreticiler](active-directory-v2-protocols.md).

#### <a name="request-the-permissions-from-a-directory-admin"></a>Bir dizin yönetici izinleri iste

Kuruluşun yönetici izinleri istemek hazır olduğunuzda, kullanıcıyı v2.0 için yönlendirebilirsiniz *yönetici onay uç noktası*.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting the following request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| `tenant` | Gerekli | İzni istemek için istediğiniz dizinin Kiracı. Bu GUID veya kolay adı biçiminde olabilir. Kullanıcının ait olduğu için Kiracı ve herhangi bir kiracıyla oturum oturum, kullanın bildirmek istediğiniz bilmiyorsanız, `common`. |
| `client_id` | Gerekli | Uygulamanıza atanan uygulama (istemci) kimliği. Uygulamanızı kayıtlı olduğu portalda bu bilgiyi bulabilirsiniz. |
| `redirect_uri` | Gerekli | Yeniden yönlendirme URI'si, uygulamanızı işlemek gönderilecek yanıt istediğiniz. Bu URL olarak kodlanmış olması dışında tam olarak bir yeniden yönlendirme Portalı'nda kayıtlı bir URI'leri eşleşmesi gerekir ve ek yol kesimine sahip olabilir. |
| `state` | Önerilen | Ayrıca belirteci yanıtta döndürülen istek yer aldığı bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Durumu, uygulama kullanıcının durumu hakkındaki bilgileri sayfasında ya da görünümü üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için kullanılır. |

Bu noktada, Azure AD, yalnızca Kiracı Yöneticisi isteği tamamlamak oturum açabildiğinizi uygular. Yönetici uygulama kayıt portalında uygulamanıza için istenen tüm doğrudan uygulama izinleri de onaylaması istenir.

##### <a name="successful-response"></a>Başarılı yanıt

Yönetici izinleri uygulamanız için onaylarsa, başarılı yanıt şöyle görünür:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametre | Açıklama |
| --- | --- |
| `tenant` | Uygulamanız, GUID biçiminde istenen izinler directory kiracısı. |
| `state` | Ayrıca belirteci yanıtta döndürülen istek yer aldığı bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Durumu, uygulama kullanıcının durumu hakkındaki bilgileri sayfasında ya da görünümü üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için kullanılır. |
| `admin_consent` | Kümesine **true**. |

##### <a name="error-response"></a>Hata yanıtı

Yönetici izinleri uygulamanız için onaylamaz, başarısız bir yanıt şöyle görünür:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametre | Açıklama |
| --- | --- |
| `error` | Tür hataları, sınıflandırmak için kullanabileceğiniz bir hata kodu dizesi ve olan hataları tepki vermek için kullanabilirsiniz. |
| `error_description` | Yardımcı olabilecek belirli bir hata iletisi, hata nedenini tanımlayın. |

Uygulama sağlama uç noktasından başarılı bir yanıt alındı sonra uygulamanızı istendiğinde doğrudan uygulama izinleri kazanmıştır. Şimdi istediğiniz kaynak için bir belirteç isteğinde bulunabilirsiniz.

## <a name="get-a-token"></a>Bir belirteç Al

Uygulamanız için gerekli yetkilendirmeyi edindiğiniz sonra API'ler için erişim belirteçlerini almak ile devam edin. Kimlik bilgileri verme istemcisini kullanarak bir belirteç almak için bir POST isteği gönderin. `/token` v2.0 uç noktası:

### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk durumda: paylaşılan bir gizli dizi ile erişim belirteci isteği

```
POST /{tenant}/oauth2/v2.0/token HTTP/1.1           //Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_secret=qWgdYAmab0YSkuL1qKv5bPX
&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| `tenant` | Gerekli | Dizin Kiracı uygulama, GUID ya da etki alanı adı biçiminde boşluğunun planlamaktadır. |
| `client_id` | Gerekli | Uygulamanıza atanan uygulama kimliği. Uygulamanızı kayıtlı olduğu portalda bu bilgiyi bulabilirsiniz. |
| `scope` | Gerekli |Geçirilen değer `scope` bu istek parametresi istediğiniz ile yapıştırılmış kaynağın kaynak tanımlayıcısı (uygulama kimliği URI'si) olmalıdır `.default` soneki. Microsoft Graph örnekte değeri, `https://graph.microsoft.com/.default`. Bu değer, v2.0 uç noktası, uygulamanız için yapılandırdığınız tüm doğrudan uygulama izinleri, bunu olanlar için kullanmak istediğiniz kaynakla ilişkili bir belirteç vermek bildirir. |
| `client_secret` | Gerekli | Uygulama kayıt portalında uygulamanıza için oluşturulan uygulama gizli anahtarı. İstemci gizli anahtarı gönderilmeden önce URL kodlamalı olmalıdır. |
| `grant_type` | Gerekli | Olmalıdır `client_credentials`. |

### <a name="second-case-access-token-request-with-a-certificate"></a>İkinci durumda: bir sertifika ile erişim belirteci isteği

```
POST /{tenant}/oauth2/v2.0/token HTTP/1.1               // Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&grant_type=client_credentials
```

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| `tenant` | Gerekli | Dizin Kiracı uygulama, GUID ya da etki alanı adı biçiminde boşluğunun planlamaktadır. |
| `client_id` | Gerekli |Uygulamanıza atanan uygulama kimliği. |
| `scope` | Gerekli | Geçirilen değer `scope` bu istek parametresi istediğiniz ile yapıştırılmış kaynağın kaynak tanımlayıcısı (uygulama kimliği URI'si) olmalıdır `.default` soneki. Microsoft Graph örnekte değeri, `https://graph.microsoft.com/.default`. <br>Bu değer, v2.0 uç noktası, uygulamanız için yapılandırdığınız tüm doğrudan uygulama izinleri, bunu olanlar için kullanmak istediğiniz kaynakla ilişkili bir belirteç vermek bildirir. |
| `client_assertion_type` | Gerekli | Değer olmalıdır `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| `client_assertion` | Gerekli | Onaylama (bir JSON Web belirteci) oluşturmak ve sertifika ile imzalamak için gereken kimlik bilgileri olarak uygulamanız için kayıtlı. Hakkında bilgi edinin [sertifika kimlik bilgileri](active-directory-certificate-credentials.md) sertifikanız ve onaylama biçimi kaydetme hakkında bilgi edinmek için.|
| `grant_type` | Gerekli | Ayarlanmalıdır `client_credentials`. |

Client_secret parametresi tarafından iki parametre değiştirilir dışında parametreler neredeyse aynı paylaşılan gizli diziyi isteğiyle olduğu gibi olduğuna dikkat edin: client_assertion_type ve client_assertion.

### <a name="successful-response"></a>Başarılı yanıt

Başarılı bir yanıt şöyle görünür:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parametre | Açıklama |
| --- | --- |
| `access_token` | İstenen erişim belirteci. Uygulama, güvenli kaynağa gibi bir Web API'ye ilişkin kimlik doğrulaması için bu belirteci kullanabilirsiniz. |
| `token_type` | Belirteç türü değeri gösterir. Azure AD destekleyen tek tür `bearer`. |
| `expires_in` | Bir erişim belirteci (saniye olarak) geçerli olduğu süre miktarı. |

### <a name="error-response"></a>Hata yanıtı

Bir hata yanıtı şuna benzer:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametre | Açıklama |
| --- | --- |
| `error` | Oluşan hataları türlerini sınıflandırmak ve hataları tepki vermek için kullanabileceğiniz bir hata kodu dizesi. |
| `error_description` | Belirli bir hata iletisi yardımcı olabilecek bir kimlik doğrulama hatası kök nedenini tanımlayın. |
| `error_codes` | Tanılama ile yardımcı olabilecek özel STS hata kodlarının listesi. |
| `timestamp` | Hata oluştuğunda süre. |
| `trace_id` | Tanılama ile Yardım isteği için benzersiz bir tanımlayıcı. |
| `correlation_id` | Tanılama ile bileşenlerinde Yardım isteği için benzersiz bir tanımlayıcı. |

## <a name="use-a-token"></a>Bir belirteç kullanın

Bir belirteç edindiğiniz, kaynağa isteğinde bulunmak için belirteci kullanın. Belirtecin süresi dolduğunda, isteği tekrarlayın `/token` yeni erişim belirteci almak için uç nokta.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro tip: Try the following command! (Replace the token with your own.)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-sample"></a>Kod örneği

İstemci kimlik bilgileri verme yönetici kullanarak uygulayan uç nokta onay uygulamanın bir örneğini görmek için bkz: bizim [v2.0 arka plan kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
