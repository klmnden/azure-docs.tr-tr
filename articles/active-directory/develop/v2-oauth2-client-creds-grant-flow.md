---
title: Kullanıcı etkileşimi olmadan güvenli kaynaklara erişmek için Microsoft kimlik platformu kullan | Azure
description: Microsoft kimlik platformu uygulaması OAuth 2.0 kimlik doğrulama protokolü kullanarak Web uygulamaları oluşturun.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 9b7cfbd7-f89f-4e33-aff2-414edd584b07
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
ms.openlocfilehash: 3073d34a6ffeadd1c1c0022b5c1636f06cc6210a
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190828"
---
# <a name="microsoft-identity-platform-and-the-oauth-20-client-credentials-flow"></a>Microsoft kimlik platformu ve OAuth 2.0 istemci kimlik bilgileri akışı

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Kullanabileceğiniz [OAuth 2.0 istemci kimlik bilgileri verme](https://tools.ietf.org/html/rfc6749#section-4.4) RFC olarak da adlandırılır 6749 içinde belirtilen *iki bacaklı OAuth*, uygulamanın kimliğini kullanarak web barındırılan kaynaklara erişmek için. Bu tür bir verme, hemen bir kullanıcı etkileşimi olmadan arka planda çalışması gereken sunucudan sunucuya etkileşimleri için yaygın olarak kullanılır. Bu tür uygulamalar olarak anılır *Daemon'ları* veya *hizmet hesapları*.

OAuth 2.0 istemci kimlik bilgileri, başka bir web hizmetini çağırırken bir kullanıcının kimliğine bürünmek yerine kendi kimlik bilgilerini kullanmak için bir web hizmetinin (gizli istemci) akış izinleri verin. Bu senaryoda istemci genellikle bir orta katman web hizmeti, bir arka plan programı hizmeti veya web sitesi olur. Daha yüksek bir güvence düzeyi için Microsoft identity platformuna (yerine, paylaşılan gizlilik) bir sertifika bir kimlik bilgisi olarak kullanılacak arama hizmeti de sağlar.

> [!NOTE]
> Microsoft kimlik platformu uç nokta, tüm Azure AD senaryoları ve özellikleri desteklemiyor. Microsoft kimlik platformu uç noktasını kullanması gerekip gerekmediğini belirlemek için aşağıdaki hakkında bilgi edinin: [Microsoft Identity platform sınırlamaları](active-directory-v2-limitations.md).

Daha fazla tipik *üç bacaklı OAuth*, bir istemci uygulamanın belirli bir kullanıcı adına bir kaynağa erişim izni verilir. İzin, genellikle sırasında uygulamaya kullanıcıdan görevlendirildiğini [onay](v2-permissions-and-consent.md) işlem. Ancak, istemci kimlik bilgileri de (*iki bacaklı OAuth*) akışı, izinler uygulamanın kendisi doğrudan verilir. Uygulama bir kaynağa bir belirteç sunar, uygulama bir eylem ve kullanıcının gerçekleştirme yetkisi olan kaynak zorlar.

## <a name="protocol-diagram"></a>Protokol diyagramı

Tüm istemci kimlik bilgileri akışı, aşağıdaki diyagrama benzer. Bu makaledeki adımların her biri açıklanmaktadır.

![İstemci kimlik bilgileri akışı](./media/v2-oauth2-client-creds-grant-flow/convergence-scenarios-client-creds.svg)

## <a name="get-direct-authorization"></a>Doğrudan yetkilendirme alın

Bir uygulama genellikle iki yöntemden biriyle bir kaynağa erişmek için doğrudan yetkilendirme alır:

* [Erişim denetimi listesi (ACL) kaynakta](#access-control-lists)
* [Azure AD'de uygulama iznini atama](#application-permissions)

Bu iki yöntem Azure AD'de en yaygın olarak bulunur ve bunları istemcilerin ve istemci gerçekleştirmek kaynaklar için kimlik bilgileri akışı öneririz. Bir kaynak, istemcileri farklı yollarla yetkilendirmek de seçebilirsiniz. Her kaynak sunucu, uygulama için en anlamlı yöntemi seçebilirsiniz.

### <a name="access-control-lists"></a>Erişim denetimi listeleri

Bir kaynak sağlayıcısı uygulama (istemci) bilir ve belirli bir düzeyde erişim verir kimlikleri listesini dayalı bir yetkilendirme onay zorlayabilir. Kaynak Microsoft kimlik platformu uç noktasından bir belirteç aldığında, bu belirteç kodunu çözme ve istemci uygulama kimliği ayıklamak `appid` ve `iss` talep. Ardından uygulama sakladığı bir erişim denetimi listesi (ACL) karşı karşılaştırır. ACL'leri ayrıntı düzeyi ve yöntemi, kaynakları arasında önemli ölçüde değişiklik gösterebilir.

Genel bir kullanım örneği, bir web uygulaması veya web API'si için testleri çalıştırmak için bir ACL kullanmaktır. Web API'si, belirli bir istemci için yalnızca bir alt kümesini tam izinleri verebilir. API üzerinde uçtan uca testleri çalıştırmak için Microsoft kimlik platformu uç noktasından belirteç edinme ve ardından bunları API'sine gönderir. bir test istemcisi oluşturun. API bu durumda test istemci uygulama kimliği API'nin tamamını işlevine tam erişim ACL'si denetler. Bu tür bir ACL kullanırsanız, yalnızca arayanın doğruladığınızdan emin olması `appid` değer ancak aynı zamanda doğrulamak `iss` belirteç değeri güvenilir.

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

1. Kaydolun ve yeni aracılığıyla uygulama oluşturma [uygulama kayıtları (Önizleme) deneyimi](quickstart-register-app.md).
2. Uygulama kayıtları (Önizleme) deneyimi uygulamanıza gidin. Gidin **sertifikaları ve parolaları** bölümünde ve ekleme bir **yeni gizli**, bir belirteç istemek için en az bir istemci gizli anahtarı gerekir.
3. Bulun **API izinleri** bölümüne ve ardından ekleyin **uygulama izinleri** , uygulamanız için gerekli.
4. **Kaydet** uygulama kaydı.

#### <a name="recommended-sign-the-user-into-your-app"></a>Önerilen: Kullanıcı, uygulamada oturum açması

Genellikle, uygulama izinleri kullanan bir uygulama oluşturduğunuzda, uygulama bir sayfa ya da görünüm üzerinde yönetici uygulamanın izinlerini onaylar gerekiyor. Bu sayfada uygulamanın oturum açma akışı, uygulama ayarlarının parçası parçası olabilir veya ayrılmış "Bağlan" akış olabilir. Çoğu durumda, "yalnızca bir kullanıcı bir iş veya Okul hesabı Microsoft ile imzaladığı sonra bu göstermek uygulama için Görünüm Bağlan" mantıklıdır.

Kullanıcı uygulamanızda oturum açarsanız, uygulama izinleri onaylamak için kullanıcıya sor önce kullanıcının ait olduğu kuruluş tanımlayabilirsiniz. Kesinlikle gerekli olmasa da, kullanıcılarınızın daha sezgisel bir deneyim oluşturmanıza yardımcı olur. Kullanıcının oturum açmasını için izleyin bizim [Microsoft kimlik platformu Protokolü öğreticiler](active-directory-v2-protocols.md).

#### <a name="request-the-permissions-from-a-directory-admin"></a>Bir dizin yönetici izinleri iste

Kuruluşun yönetici izinleri istemek hazır olduğunuzda, kullanıcının Microsoft identity platformuna yönlendirebilirsiniz *yönetici onay uç noktası*.

> [!TIP]
> Postman içinde bu isteğin yürütülmesi deneyin! (En iyi sonuçları almak için kendi uygulama kimliği kullanın - öğretici uygulama yararlı izinlere ilişkin istek gerekmez.) [![Postman içinde çalıştırın](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting the following request in a browser.
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| `tenant` | Gerekli | İzni istemek için istediğiniz dizinin Kiracı. Bu GUID veya kolay adı biçiminde olabilir. Kullanıcının ait olduğu için Kiracı ve herhangi bir kiracıyla oturum oturum, kullanın bildirmek istediğiniz bilmiyorsanız, `common`. |
| `client_id` | Gerekli | **Uygulama (istemci) kimliği** , [Azure portalında – uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) uygulamanıza atanan deneyimi. |
| `redirect_uri` | Gerekli | Yeniden yönlendirme URI'si, uygulamanızı işlemek gönderilecek yanıt istediğiniz. Bu URL olarak kodlanmış olması dışında tam olarak bir yeniden yönlendirme Portalı'nda kayıtlı bir URI'leri eşleşmesi gerekir ve ek yol kesimine sahip olabilir. |
| `state` | Önerilen | Ayrıca belirteci yanıtta döndürülen istek yer aldığı bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Durumu, uygulama kullanıcının durumu hakkındaki bilgileri sayfasında ya da görünümü üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için kullanılır. |

Bu noktada, bir kiracı Yöneticisi olarak tam istek oturum açarak yalnızca Azure AD, uygular. Yönetici uygulama kayıt portalında uygulamanıza için istenen tüm doğrudan uygulama izinleri de onaylaması istenir.

##### <a name="successful-response"></a>Başarılı yanıt

Yönetici izinleri uygulamanız için onaylarsa, başarılı yanıt şöyle görünür:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametre | Açıklama |
| --- | --- |
| `tenant` | Uygulamanız, GUID biçiminde istenen izinler directory kiracısı. |
| `state` | Ayrıca belirteci yanıtta döndürülen istek yer aldığı bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Durumu, uygulama kullanıcının durumu hakkındaki bilgileri sayfasında ya da görünümü üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için kullanılır. |
| `admin_consent` | Kümesine **True**. |

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

Uygulamanız için gerekli yetkilendirmeyi edindiğiniz sonra API'ler için erişim belirteçlerini almak ile devam edin. Kimlik bilgileri verme istemcisini kullanarak bir belirteç almak için bir POST isteği gönderin. `/token` Microsoft kimlik platformu uç noktası:

> [!TIP]
> Postman içinde bu isteğin yürütülmesi deneyin! (En iyi sonuçları almak için kendi uygulama kimliği kullanın - öğretici uygulama yararlı izinlere ilişkin istek gerekmez.) [![Postman içinde çalıştırın](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk Durum: Paylaşılan gizlilik ile erişim belirteci isteği

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
| `scope` | Gerekli | Geçirilen değer `scope` bu istek parametresi istediğiniz ile yapıştırılmış kaynağın kaynak tanımlayıcısı (uygulama kimliği URI'si) olmalıdır `.default` soneki. Microsoft Graph örnekte değeri, `https://graph.microsoft.com/.default`. <br/>Bu değer, uygulamanız için yapılandırdığınız tüm doğrudan uygulama izinleri, uç nokta olanlar için kullanmak istediğiniz kaynakla ilişkili bir belirteç vermek, Microsoft kimlik platformu uç nokta söyler. Hakkında daha fazla bilgi edinmek için `/.default` kapsam için bkz: [belge onay](v2-permissions-and-consent.md#the-default-scope). |
| `client_secret` | Gerekli | Uygulama kayıt portalında uygulamanıza için oluşturulan istemci gizli anahtarı. İstemci gizli anahtarı gönderilmeden önce URL kodlamalı olmalıdır. |
| `grant_type` | Gerekli | Ayarlanmalıdır `client_credentials`. |

### <a name="second-case-access-token-request-with-a-certificate"></a>İkinci durum: Bir sertifika ile erişim belirteci isteği

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
| `client_id` | Gerekli |Uygulamanıza atanan uygulama (istemci) kimliği. |
| `scope` | Gerekli | Geçirilen değer `scope` bu istek parametresi istediğiniz ile yapıştırılmış kaynağın kaynak tanımlayıcısı (uygulama kimliği URI'si) olmalıdır `.default` soneki. Microsoft Graph örnekte değeri, `https://graph.microsoft.com/.default`. <br/>Bu değer, Microsoft kimlik platformu uç noktası, uygulamanız için yapılandırdığınız tüm doğrudan uygulama izinleri, bunu olanlar için kullanmak istediğiniz kaynakla ilişkili bir belirteç vermek bildirir. Hakkında daha fazla bilgi edinmek için `/.default` kapsam için bkz: [belge onay](v2-permissions-and-consent.md#the-default-scope). |
| `client_assertion_type` | Gerekli | Değer ayarlanmalıdır `urn:ietf:params:oauth:client-assertion-type:jwt-bearer`. |
| `client_assertion` | Gerekli | Oluşturma ve sertifika ile imzalamak için gereken onaylama (bir JSON web belirteci) uygulamanız için kimlik bilgileri olarak kayıtlı. Hakkında bilgi edinin [sertifika kimlik bilgileri](active-directory-certificate-credentials.md) sertifikanız ve onaylama biçimi kaydetme hakkında bilgi edinmek için.|
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
| `token_type` | Belirteç türü değeri gösterir. Microsoft kimlik platformu destekleyen tek yazın `bearer`. |
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

> [!NOTE]
> V2 belirteci almak, uygulamanın bildirim dosyasını azure portalındaki uygulamadan güncelleştirebilirsiniz. Öznitelik ekleyebilirsiniz `accessTokenAcceptedVersion` ve değer 2 olarak için ayarlamak `"accessTokenAcceptedVersion": 2`. Makaleyi gözden geçirin [uygulama bildirimini](https://docs.microsoft.com/en-us/azure/active-directory/develop/reference-app-manifest#manifest-reference) aynı daha iyi anlamak için. Varsayılan uygulama şu anda recieves v1 belirteci. Bu bildirimde bu öznitelik değeri 1 olarak varsayılan olarak, uygulama/Web API'si bildirimi içinde tanımlı değil ve bu nedenle uygulama v1 belirteci alırsınız.  


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

## <a name="code-samples-and-other-documentation"></a>Kod örnekleri ve diğer belgeler

Okuma [istemci kimlik bilgileri, genel bakış belgeleri](https://aka.ms/msal-net-client-credentials) Microsoft kimlik doğrulama Kitaplığı'ndan

| Örnek | Platform |Açıklama |
|--------|----------|------------|
|[Active-directory-dotnetcore-arka plan programı-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) | .NET core 2.1 konsol | Bir kullanıcı adına yerine uygulama kimliğini kullanarak Microsoft Graph sorgulama Kiracı kullanıcıları görüntüler basit bir .NET Core uygulaması. Örnek ayrıca kimlik doğrulaması için sertifikalar kullanılarak değişim gösterilmektedir. |
|[active-directory-dotnet-daemon-v2](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2)|ASP.NET MVC | Verileri bir kullanıcı adına yerine uygulama kimliğini kullanarak Microsoft Graph web uygulaması. |
