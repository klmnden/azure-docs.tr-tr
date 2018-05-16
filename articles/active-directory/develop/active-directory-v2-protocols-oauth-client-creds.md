---
title: Kullanıcı etkileşimi olmadan güvenli kaynaklara erişmek için Azure AD v2.0 kullanın | Microsoft Docs
description: Web uygulamaları Azure AD uygulama OAuth 2.0 kimlik doğrulama protokolü kullanarak oluşturun.
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
ms.date: 01/07/2017
ms.author: celested
ms.reviewer: dastrock
ms.custom: aaddev
ms.openlocfilehash: db466a3ae416c47f86bb66b3bb8ba4bcd7741f5f
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="azure-active-directory-v20-and-the-oauth-20-client-credentials-flow"></a>Azure Active Directory v2.0 ve OAuth 2.0 istemci kimlik bilgileri akışının
Kullanabileceğiniz [OAuth 2.0 istemci kimlik bilgileri vermenizi](http://tools.ietf.org/html/rfc6749#section-4.4) olarak da adlandırılan RFC 6749 içinde belirtilen *iki bacaklı OAuth*, uygulamanın kimliğini kullanarak web bulunan kaynaklara erişmek için. Yaygın olarak grant bu tür bir kullanıcıyla hemen etkileşimi olmadan arka planda çalıştırılmalıdır server sunucusu etkileşimleri için kullanılır. Bu tür uygulamalar genellikle denir *deamon'lar* veya *hizmet hesapları*.

> [!NOTE]
> V2.0 uç noktası, tüm Azure Active Directory senaryolarını ve özelliklerini desteklemez. V2.0 uç kullanması gerekip gerekmediğini belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
>
>

Daha fazla tipik *üç bacaklı OAuth*, bir istemci uygulaması belirli bir kullanıcı adına bir kaynağa erişim izni verilir. İzni kullanıcıdan uygulamaya genellikle sırasında temsilci [onayı](active-directory-v2-scopes.md) işlemi. Ancak, istemci kimlik bilgileri akışının, uygulamanın kendisinin doğrudan izinler verilir. Bir kaynak, kaynak için bir belirteç zorlar uygulama sunar uygulamanın kendi bir eylem ve değil, gerçekleştirme yetkisi varsa kullanıcı yetkilendirme sahiptir.

## <a name="protocol-diagram"></a>Protokol diyagramı
Tüm istemci kimlik bilgileri akışının sonraki diyagrama benzer. Biz bu makalenin sonraki bölümlerinde adımların her biri açıklanmaktadır.

![İstemci kimlik bilgileri akışının](../../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## <a name="get-direct-authorization"></a>Doğrudan yetkilendirme Al
Bir uygulama genellikle iki yoldan biriyle bir kaynağa erişmek için doğrudan yetkilendirme alır: Azure Active Directory'de (Azure AD) uygulama izni atama veya kaynakta bir erişim denetimi listesi (ACL) üzerinden. Bu iki yöntem Azure AD'de en yaygın olarak bulunur ve bunları istemciler ve istemci gerçekleştirmek kaynakları için kimlik bilgileri akışını öneririz. Bir kaynak istemcilerine başka yollarla ancak yetkilendirmek seçebilirsiniz. Her kaynak sunucu, uygulama için en anlamlı yöntemi seçebilirsiniz.

### <a name="access-control-lists"></a>Erişim denetimi listeleri
Bir kaynak sağlayıcısı bilir ve belirli düzeyde bir erişim verir uygulama kimlikleri listesini dayalı bir yetkilendirme onay zorlayabilir. Kaynak v2.0 uç noktasından belirteç aldığında, bu belirteç kodunu çözer ve istemcinin uygulama kimliği ayıklamak `appid` ve `iss` talep. Ardından uygulamaya sakladığı bir ACL karşı karşılaştırır. Önemli ölçüde ACL'leri ayrıntı düzeyi ve yöntemi kaynaklar arasında değişebilir.

Genel kullanım örneği, bir web uygulaması veya Web API'si testleri çalıştırmak için bir ACL kullanmaktır. Web API tam izinleri yalnızca bir kısmı için belirli bir istemci için izin verebilir. API uçtan uca testleri çalıştırmak için belirteçleri v2.0 uç noktasından alır ve bunları API için gönderen bir test istemci oluşturun. API test istemcinin uygulama kimliği tam erişim API'nin tüm işlevselliği için ACL arar. Bu tür bir ACL kullanırsanız, yalnızca arayanın doğrulamak mutlaka `appid` değeri. Ayrıca doğrulamak `iss` belirteç değeri güvenilir.

Bu tür bir kimlik doğrulama, arka plan programları ve kişisel Microsoft hesabına sahip tüketici kullanıcılara ait verilere erişmek için gereken hizmet hesapları için yaygındır. Kuruluşlar tarafından ait verileri için uygulama izinleri aracılığıyla gerekli yetki alma öneririz.

### <a name="application-permissions"></a>Uygulama izinleri
ACL'ler kullanmak yerine, uygulama izinleri kullanıma sunmak için API'ler kullanabilirsiniz. Bir uygulama izni bir uygulamaya bir kuruluşun Yöneticisi tarafından verilir ve yalnızca belirli bir kuruluş ve çalışanlarının tarafından ait veri erişimi için kullanılabilir. Örneğin, Microsoft Graph aşağıdakileri yapmak için birkaç uygulama izinleri sunar:

* Tüm posta kutularındaki postaları okuma
* Tüm posta kutularındaki postaları okuma ve yazma
* Herhangi bir kullanıcı adına posta gönderme
* Dizin verilerini okuma

Uygulama izinleri hakkında daha fazla bilgi için Git [Microsoft Graph](https://graph.microsoft.io).

Uygulama izinlerini uygulamanızda kullanmak için sonraki bölümlerde aşağıdakiler ele adımları yapın.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Uygulama kayıt Portalı'nda izinleri iste
1. Uygulamanızda gidin [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), veya [bir uygulama oluşturmak](active-directory-v2-app-registration.md), henüz yapmadıysanız. Uygulamanızı oluşturduğunuzda en az bir uygulama gizli anahtarı kullanmanız gerekir.
2. Bulun **Mcrosoft grafik izinleri** bölümünde ve ardından ekleyin **uygulama izinleri** , uygulamanızı gerektirir.
3. **Kaydet** uygulama kaydı.

#### <a name="recommended-sign-the-user-in-to-your-app"></a>Önerilir: kullanıcı uygulamanızda oturum
Genellikle, uygulama izinleri kullanan bir uygulama oluşturduğunuzda, uygulamanın bir sayfa ya da görünüm üzerinde yönetici uygulamanın izinleri onaylar gerektirir. Bu sayfa uygulamanın oturum açma akışını, uygulamanın ayarlarının bir parçası parçası olabilir veya özel bir "Bağlan" akış olabilir. Çoğu durumda, "yalnızca bir kullanıcı bir iş veya Okul Microsoft hesabı oturum imzaladığı sonra uygulamanın bunu göstermek için bağlantı görünümü" mantıklıdır.

Kullanıcının uygulamanıza oturum açarsanız, uygulama izinleri onaylamak için kullanıcıya sor önce kullanıcının ait olduğu kuruluş tanımlayabilirsiniz. Kesinlikle gerekli olmasa da, kullanıcılarınızın daha sezgisel bir deneyim oluşturmanıza yardımcı olabilir. Kullanıcıyla oturum açmak için izleyin bizim [v2.0 protokol öğreticileri](active-directory-v2-protocols.md).

#### <a name="request-the-permissions-from-a-directory-admin"></a>Bir dizin yönetici olarak izinleri iste
Kuruluş yönetici olarak izinleri istemek hazır olduğunuzda, kullanıcıyı v2.0 yönlendirebilirsiniz *yönetici onayı uç noktası*.

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
| kiracı |Gerekli |İzni istemek için istediğiniz dizin Kiracı. Bu GUID veya kolay ad biçiminde olabilir. Kullanıcının ait olduğu için Kiracı ve oturum ile hiçbir Kiracı, kullanın bildirmek istediğiniz bilmiyorsanız, `common`. |
| client_id |Gerekli |Uygulama Kimliği [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) uygulamanıza atanmış. |
| redirect_uri |Gerekli |Yeniden yönlendirme işlemek uygulamanız için gönderilecek yanıt istediğiniz URI'si. URL kodlanmış olmalıdır ancak bu, tam olarak yeniden yönlendirme Portalı'nda kayıtlı URI'ler eşleşmelidir ve ek yol kesimine sahip olabilir. |
| durum |Önerilen |Ayrıca belirteci yanıtta döndürülen istek yer aldığı bir değer. İstediğiniz herhangi bir içerik dizesi olabilir. Durum, uygulama kullanıcının durumu hakkında bilgi sayfa veya görünüm üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için kullanılır. |

Bu noktada, Azure AD yalnızca bir kiracı Yöneticisi isteği tamamlamak oturum açabildiğinizi zorlar. Yönetici uygulama kayıt Portalı'nda, uygulamanız için istenen tüm doğrudan uygulama izinleri onaylamanız istenir.

##### <a name="successful-response"></a>Başarılı yanıt
Yönetici, uygulamanızın izinlerini onaylarsa, başarılı yanıtı şuna benzer:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametre | Açıklama |
| --- | --- | --- |
| kiracı |Uygulamanız bu GUID biçiminde istenen izinlerin dizin Kiracı. |
| durum |Ayrıca belirteci yanıtta döndürülen istek yer aldığı bir değer. İstediğiniz herhangi bir içerik dizesi olabilir. Durum, uygulama kullanıcının durumu hakkında bilgi sayfa veya görünüm üzerinde oldukları gibi kimlik doğrulama isteği oluşmadan önce kodlamak için kullanılır. |
| admin_consent |Kümesine **doğru**. |

##### <a name="error-response"></a>Hata yanıtı
Yönetici, uygulamanızın izinlerini onaylamaz, başarısız yanıtı şuna benzer:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametre | Açıklama |
| --- | --- | --- |
| error |Hata, türleri sınıflandırmak için kullanabileceğiniz bir hata kodu dizesi ve hangi hataların tepki vermek için kullanabilirsiniz. |
| error_description |Yardımcı olabilecek belirli bir hata iletisi bir hatasının kök nedenini tanımlayın. |

Uygulama sağlama uç noktasından başarılı bir yanıt aldık sonra uygulamanızı istendiğinde doğrudan uygulama izinleri kazanmıştır. Şimdi istediğiniz kaynak için bir belirteç talep edebilirsiniz.

## <a name="get-a-token"></a>Belirteç alın
Uygulamanız için gerekli yetkilendirmeyi edindiğiniz sonra erişim belirteçleri API'ler alınırken ile devam edin. Kimlik bilgileri sağlama istemcisini kullanarak bir belirteç almak üzere bir POST isteği Gönder `/token` v2.0 uç noktası:

### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk durumda: bir paylaşılan gizlilik ile erişim belirteci isteği

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
| kiracı |Gerekli | Uygulama dizini Kiracı, GUID veya etki alanı adı biçiminde boşluğunun planlar. |
| client_id |Gerekli |Uygulama Kimliği [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) uygulamanıza atanmış. |
| scope |Gerekli |Geçirilen değeri `scope` bu isteği parametresinde istediğiniz ile yapıştırılmış kaynağının kaynak tanımlayıcısı (uygulama kimliği URI) olmalıdır `.default` soneki. Microsoft Graph örneğin değerdir `https://graph.microsoft.com/.default`. Bu değer v2.0 uç noktası, uygulamanız için yapılandırdığınız tüm doğrudan uygulama izinlerini, onu olanlar için kullanmak istediğiniz kaynakla ilişkili bir belirteç vermek bildirir. |
| client_secret |Gerekli |Uygulama uygulama kayıt Portalı'nda, uygulamanız için oluşturulan gizli anahtarı. |
| grant_type |Gerekli |Olmalıdır `client_credentials`. |

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
| kiracı |Gerekli | Uygulama dizini Kiracı, GUID veya etki alanı adı biçiminde boşluğunun planlar. |
| client_id |Gerekli |Uygulama Kimliği [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) uygulamanıza atanmış. |
| scope |Gerekli |Geçirilen değeri `scope` bu isteği parametresinde istediğiniz ile yapıştırılmış kaynağının kaynak tanımlayıcısı (uygulama kimliği URI) olmalıdır `.default` soneki. Microsoft Graph örneğin değerdir `https://graph.microsoft.com/.default`. Bu değer v2.0 uç noktası, uygulamanız için yapılandırdığınız tüm doğrudan uygulama izinlerini, onu olanlar için kullanmak istediğiniz kaynakla ilişkili bir belirteç vermek bildirir. |
| client_assertion_type |gerekli |Değer olmalıdır `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |gerekli | Oluşturma ve sertifika ile imzalamak için gereken bir onaylama işlemi (bir JSON Web belirteci) uygulamanız için kimlik bilgileri olarak kayıtlı. Hakkında bilgi edinin [sertifika kimlik bilgileri](active-directory-certificate-credentials.md) sertifikanızı ve onaylama biçimi kaydetme hakkında bilgi edinmek için.|
| grant_type |Gerekli |Olmalıdır `client_credentials`. |

Client_secret parametresi tarafından iki parametre değiştirilir dışında parametreler neredeyse aynı paylaşılan gizliliği isteğiyle durumunda olduğu gibi olduğuna dikkat edin: client_assertion_type ve client_assertion.

### <a name="successful-response"></a>Başarılı yanıt
Başarılı yanıt şöyle görünür:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parametre | Açıklama |
| --- | --- |
| access_token |İstenen erişim belirteci. Uygulama güvenli kaynağa gibi bir Web API kimliğini doğrulamak için bu belirteci kullanabilirsiniz. |
| token_type |Belirteç türü değeri gösterir. Azure AD destekler yalnızca türü `bearer`. |
| expires_in |Ne kadar süreyle erişim belirteci (saniye olarak) geçerli değil. |

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
| error |Oluşan hataları türlerini sınıflandırmak ve hataları tepki vermek için kullanabileceğiniz bir hata kodu dizesi. |
| error_description |Yardımcı olabilecek belirli bir hata iletisi kimlik doğrulama hatası kök nedenini tanımlayın. |
| error_codes |Tanılama ile yardımcı olabilecek STS özgü hata kodları listesi. |
| timestamp |Hatanın gerçekleştiği zaman. |
| trace_id |Tanılama ile yardımcı olabilecek isteği için benzersiz bir tanımlayıcı. |
| correlation_id |Tanılama ile bileşenlerinde yardımcı olabilecek isteği için benzersiz bir tanımlayıcı. |

## <a name="use-a-token"></a>Bir belirteci kullanın
Bir belirteç edindiğiniz, kaynağa isteği yapmak için belirteci kullanın. Belirtecin süresi dolduğunda, isteği tekrarlayın `/token` yeni erişim belirteci alması için uç noktası.

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
İstemci kimlik bilgileri vermenizi yönetici kullanarak uygulayan uç nokta onayı bir örnek uygulamanın görmek için bkz: bizim [v2.0 arka plan kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
