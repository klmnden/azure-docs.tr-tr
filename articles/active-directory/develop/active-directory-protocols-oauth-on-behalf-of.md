---
title: OAuth2.0 üzerinde-adına-Of taslak belirtim kullanarak azure AD hizmet auth | Microsoft Docs
description: Bu makalede HTTP iletisi üzerinde-temsili akış OAuth2.0 kullanarak hizmet kimlik doğrulaması uygulamak için nasıl kullanılacağını açıklar.
services: active-directory
documentationcenter: .net
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: celested
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 2f7566bc696d07ad3a8003b3493a382f494c4599
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="service-to-service-calls-using-delegated-user-identity-in-the-on-behalf-of-flow"></a>On-temsili akış kullanıcı kimliğini kullanarak hizmeti çağrıları için hizmet temsilcisi
Burada bir hizmet/sırayla başka çağırmak için gereken web API, uygulamanın çağırır kullanım örneği akış hizmet OAuth 2.0 On-Behalf-Of hizmeti/web API. Temsilci atanan kullanıcı kimliğini ve izinleri istek zincirinin aracılığıyla yaymak için kullanılan uygulamadır. Orta katman hizmet kimliği doğrulanmış istekler için aşağı akış hizmeti yapmak, Azure Active Directory'den (Azure AD), bir erişim belirteci güvenli kullanıcı adına gerekir.

## <a name="on-behalf-of-flow-diagram"></a>Üzerinde-adına-Akış Diyagramı
Kullanıcı bir uygulama kullanarak doğrulandı varsayın [OAuth 2.0 yetkilendirme kodu izin akışı](active-directory-protocols-oauth-code.md). Bu noktada, uygulama kullanıcının talepleri ve orta katman web API (API A) erişmek için izniniz bir erişim belirteci (belirteci A) sahip. Şimdi, API bir aşağı akış web API'si (API B) kimliği doğrulanmış bir istekte bulunun gerekiyor.

Adımları On-temsili akış oluşturur ve aşağıdaki diyagramda yardımıyla açıklanmıştır.

![OAuth2.0 üzerinde-temsili akış](media/active-directory-protocols-oauth-on-behalf-of/active-directory-protocols-oauth-on-behalf-of-flow.png)


1. İstemci uygulaması API A'ya A. belirteciyle istekte
2. API bir Azure AD belirteci verme uç noktasına kimliğini doğrular ve API B'nin erişmek için bir belirteç istekleri
3. Azure AD belirteci verme endpoint belirteci bir API A'ın kimlik bilgilerini doğrular ve API B (belirteci B) için erişim belirteci verir.
4. API b isteğinin yetkilendirme üst B belirteci ayarlayın
5. Veriler güvenli kaynaktan API B'nin tarafından döndürülür

## <a name="register-the-application-and-service-in-azure-ad"></a>Azure AD'de uygulama ve hizmet kaydetme
İstemci uygulaması ve orta katman hizmet Azure AD'de kaydedin.
### <a name="register-the-middle-tier-service"></a>Orta katman hizmet kaydı
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda hesabınızda altında tıklatıp **Directory** listesinde, Active Directory Kiracı uygulamanızı kaydetmek için istediğiniz yeri seçin.
3. Tıklayın **daha Hizmetleri** sol taraftaki gezinti içinde ve **Azure Active Directory**.
4. Tıklayın **uygulama kayıtlar** ve **yeni uygulama kaydı**.
5. Uygulama için kolay bir ad girin ve uygulama türünü seçin. Oturum açma URL'si veya yeniden yönlendirme URL'si temel URL için uygulama türü kümesine bağlı. Tıklayın **oluşturma** uygulaması oluşturmak için.
6. Halen Azure portalında sırasında uygulamanızı seçin ve tıklayın **ayarları**. Ayarları menüsünden **anahtarları** ve bir anahtar add - 1 yıl veya 2 yıl anahtar bir süre seçin. Bu sayfayı kaydedin, anahtar değeri görüntülenir, kopyalama ve güvenli bir yerde değeri kaydetmek,-daha sonra uygulamanızda - uygulama ayarlarını yapılandırmak için bu anahtar gerekir, bu anahtarı değeri diğer herhangi bir yolla yeniden görüntülenen ya da alınabilir olmayacaktır. , böylece Lütfen Azure Portalı'ndan görülebilir hemen kaydedin.

### <a name="register-the-client-application"></a>İstemci uygulaması kaydetme
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda hesabınızda altında tıklatıp **Directory** listesinde, Active Directory Kiracı uygulamanızı kaydetmek için istediğiniz yeri seçin.
3. Tıklayın **daha Hizmetleri** sol taraftaki gezinti içinde ve **Azure Active Directory**.
4. Tıklayın **uygulama kayıtlar** ve **yeni uygulama kaydı**.
5. Uygulama için kolay bir ad girin ve uygulama türünü seçin. Oturum açma URL'si veya yeniden yönlendirme URL'si temel URL için uygulama türü kümesine bağlı. Tıklayın **oluşturma** uygulaması oluşturmak için.
6. -Ayarlar menüsünden uygulamanızın izinlerini yapılandırabilir, seçin **gerekli izinleri** bölümünde, tıklayın **Ekle**, ardından **bir API seçin**ve adını yazın Orta katman hizmet metin kutusuna. Ardından, tıklatın **Select izinleri** seçip ' erişim *hizmet adı*'.

### <a name="configure-known-client-applications"></a>Bilinen istemci uygulamaları yapılandır
Bu senaryoda, orta katman hizmet aşağı akış API erişmek için kullanıcı onayı almak için kullanıcı etkileşimi olmadan sahiptir. Bu nedenle, adım onay bir parçası olarak kimlik doğrulaması sırasında aşağı akış API'sine erişim seçeneği önceden sunulmalıdır.
Bunu başarmak için istemci uygulamasının kayıt izni hem istemci hem de orta katman tarafından tek bir iletişim kutusuna gerekli birleştirir orta katman hizmet kaydının ile Azure AD'de açıkça bağlamak için aşağıdaki adımları izleyin.
1. Orta katman hizmet kayıt gidin ve tıklayın **bildirim** bildirim Düzenleyicisi'ni açın.
2. Bildiriminde bulun `knownClientApplications` dizi özelliği ve öğe istemci uygulamanın istemci Kimliğini ekleyin.
3. Kaydetme tıklayarak bildirim Kaydet düğmesi.

## <a name="service-to-service-access-token-request"></a>Hizmet erişim belirteci isteği hizmetine
Bir erişim belirteci istemek için bir HTTP POST için Kiracı özgü olun aşağıdaki parametrelerle Azure AD uç noktası.

```
https://login.microsoftonline.com/<tenant>/oauth2/token
```
İstemci uygulaması olup bir paylaşılan gizlilik ya da bir sertifika tarafından güvenli hale seçti bağlı olarak iki durumlar vardır.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk durumda: bir paylaşılan gizlilik ile erişim belirteci isteği
Paylaşılan gizlilik kullanırken, hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |gerekli | Belirteç isteği türü. JWT'nin kullanarak bir istek için değer olmalıdır **urn: ietf:params:oauth:grant-türü: jwt-taşıyıcı**. |
| onaylama işlemi |gerekli | İstekte kullanılan belirteç değeri. |
| client_id |gerekli | Azure AD ile kayıt sırasında arama hizmete atanan uygulama kimliği. Uygulama Kimliği Azure Yönetim Portalı'nda bulmak için tıklatın **Active Directory**dizin'i tıklatın ve ardından uygulama adına tıklayın. |
| client_secret |gerekli | Anahtar arama hizmeti için Azure AD'de kayıtlı. Bu değer kayıt aynı anda not. |
| kaynak |gerekli | Uygulama Kimliği URI'sini alma hizmetinin (güvenli kaynak). Uygulama Kimliği URI'sini Azure Yönetim Portalı'nda bulmak için tıklatın **Active Directory**dizini tıklatın, uygulama adı'ı tıklatın, tıklatın **tüm ayarları** ve ardından **özellikleri**. |
| requested_token_use |gerekli | İsteğin nasıl işleneceğini belirtir. On adına, akış değeri olmalıdır **on_behalf_of**. |
| scope |gerekli | Boşlukla ayrılmış belirteç isteği kapsamları listesi. Openıd Connect, kapsam için **openıd** belirtilmesi gerekir.|

#### <a name="example"></a>Örnek
Bir erişim belirteci için aşağıdaki HTTP POST istekleri https://graph.windows.net web API'si. `client_id` Erişim belirteci istekleri hizmeti tanımlar.

```
// line breaks for legibility only

POST /oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_secret=0Y1W%2BY3yYb3d9N8vSjvm8WrGzVZaAaHbHHcGbcgG%2BoI%3D
&resource=https%3A%2F%2Fgraph.windows.net
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=openid
```

### <a name="second-case-access-token-request-with-a-certificate"></a>İkinci durumda: bir sertifika ile erişim belirteci isteği
Hizmetten hizmete erişim belirteci isteği bir sertifika ile aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |gerekli | Belirteç isteği türü. JWT'nin kullanarak bir istek için değer olmalıdır **urn: ietf:params:oauth:grant-türü: jwt-taşıyıcı**. |
| onaylama işlemi |gerekli | İstekte kullanılan belirteç değeri. |
| client_id |gerekli | Azure AD ile kayıt sırasında arama hizmete atanan uygulama kimliği. Uygulama Kimliği Azure Yönetim Portalı'nda bulmak için tıklatın **Active Directory**dizin'i tıklatın ve ardından uygulama adına tıklayın. |
| client_assertion_type |gerekli |Değer olmalıdır `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |gerekli | Oluşturma ve sertifika ile imzalamak için gereken bir onaylama işlemi (bir JSON Web belirteci) uygulamanız için kimlik bilgileri olarak kayıtlı. Hakkında bilgi edinin [sertifika kimlik bilgileri](active-directory-certificate-credentials.md) sertifikanızı ve onaylama biçimi kaydetme hakkında bilgi edinmek için.|
| kaynak |gerekli | Uygulama Kimliği URI'sini alma hizmetinin (güvenli kaynak). Uygulama Kimliği URI'sini Azure Yönetim Portalı'nda bulmak için tıklatın **Active Directory**dizini tıklatın, uygulama adı'ı tıklatın, tıklatın **tüm ayarları** ve ardından **özellikleri**. |
| requested_token_use |gerekli | İsteğin nasıl işleneceğini belirtir. On adına, akış değeri olmalıdır **on_behalf_of**. |
| scope |gerekli | Boşlukla ayrılmış belirteç isteği kapsamları listesi. Openıd Connect, kapsam için **openıd** belirtilmesi gerekir.|

Client_secret parametresi tarafından iki parametre değiştirilir dışında parametreler neredeyse aynı paylaşılan gizliliği isteğiyle durumunda olduğu gibi olduğuna dikkat edin: client_assertion_type ve client_assertion.

#### <a name="example"></a>Örnek
Bir erişim belirteci için aşağıdaki HTTP POST istekleri https://graph.windows.net bir sertifika ile web API'si. `client_id` Erişim belirteci istekleri hizmeti tanımlar.

```
// line breaks for legibility only

POST /oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&resource=https%3A%2F%2Fgraph.windows.net
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=openid
```

## <a name="service-to-service-access-token-response"></a>Hizmet erişim belirteci yanıtı hizmetine
Başarılı yanıt aşağıdaki parametrelerle bir JSON OAuth 2.0 yanıt olan.

| Parametre | Açıklama |
| --- | --- |
| token_type |Belirteç türü değeri gösterir. Azure AD destekler yalnızca türü **taşıyıcı**. Taşıyıcı belirteçlerini hakkında daha fazla bilgi için bkz: [OAuth 2.0 yetkilendirme Framework: taşıyıcı belirteci kullanımı (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| scope |Belirtecinde verilen erişim kapsamı. |
| expires_in |Süre (saniye cinsinden) erişim belirteci geçerlidir. |
| expires_on |Erişim belirtecinin süresi dolduğunda süre. Tarih 1970'ten saniyeyi temsil edilir-01-01T0:0:0Z UTC sona erme zamanı kadar. Bu değer, önbelleğe alınan belirteç ömrü belirlemek için kullanılır. |
| kaynak |Uygulama Kimliği URI'sini alma hizmetinin (güvenli kaynak). |
| access_token |İstenen erişim belirteci. Arama hizmeti alıcı hizmete kimlik doğrulaması için bu belirteci kullanabilirsiniz. |
| id_token |İstenen kimliği belirteci. Arama Hizmeti kullanıcının kimliğini doğrulamak ve kullanıcı oturumu başlatmak için bunu kullanabilirsiniz. |
| refresh_token |İstenen erişim belirteci için yenileme belirteci. Arama hizmeti geçerli erişim belirtecinin süresi dolduktan sonra başka bir erişim belirteci istemek için bu belirteci kullanabilirsiniz. |

### <a name="success-response-example"></a>Başarılı yanıt örnek
Aşağıdaki örnek, bir başarı isteğine yanıt olarak bir erişim belirteci için gösterir https://graph.windows.net web API'si.

```
{
    "token_type":"Bearer",
    "scope":"User.Read",
    "expires_in":"43482",
    "ext_expires_in":"302683",
    "expires_on":"1493466951",
    "not_before":"1493423168",
    "resource":"https://graph.windows.net",
    "access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ",
    "refresh_token":"AQABAAAAAABnfiG-mA6NTae7CdWW7QfdjKGu9-t1scy_TDEmLi4eLQMjJGt_nAoVu6A4oSu1KsRiz8XyQIPKQxSGfbf2FoSK-hm2K8TYzbJuswYusQpJaHUQnSqEvdaCeFuqXHBv84wjFhuanzF9dQZB_Ng5za9xKlUENrNtlq9XuLNVKzxEyeUM7JyxzdY7JiEphWImwgOYf6II316d0Z6-H3oYsFezf4Xsjz-MOBYEov0P64UaB5nJMvDyApV-NWpgklLASfNoSPGb67Bc02aFRZrm4kLk-xTl6eKE6hSo0XU2z2t70stFJDxvNQobnvNHrAmBaHWPAcC3FGwFnBOojpZB2tzG1gLEbmdROVDp8kHEYAwnRK947Py12fJNKExUdN0njmXrKxNZ_fEM33LHW1Tf4kMX_GvNmbWHtBnIyG0w5emb-b54ef5AwV5_tGUeivTCCysgucEc-S7G8Cz0xNJ_BOiM_4bAv9iFmrm9STkltpz0-Tftg8WKmaJiC0xXj6uTf4ZkX79mJJIuuM7XP4ARIcLpkktyg2Iym9jcZqymRkGH2Rm9sxBwC4eeZXM7M5a7TJ-5CqOdfuE3sBPq40RdEWMFLcrAzFvP0VDR8NKHIrPR1AcUruat9DETmTNJukdlJN3O41nWdZOVoJM-uKN3uz2wQ2Ld1z0Mb9_6YfMox9KTJNzRzcL52r4V_y3kB6ekaOZ9wQ3HxGBQ4zFt-2U0mSszIAA",
    "id_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC8yNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IvIiwiaWF0IjoxNDkzNDIzMTY4LCJuYmYiOjE0OTM0MjMxNjgsImV4cCI6MTQ5MzQ2Njk1MSwiYW1yIjpbInB3ZCJdLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXRpIjoieEN3ZnpoYS1QMFdKUU9MeENHZ0tBQSIsInZlciI6IjEuMCJ9."
}
```

### <a name="error-response-example"></a>Hata yanıtı örneği
Aşağı Akış API koşullu erişim ilkesi ayarlayabilirsiniz çok faktörlü kimlik doğrulaması gibi varsa aşağı akış API için bir erişim belirteci almaya çalışırken bir hata yanıtı Azure AD belirteç uç noktası tarafından döndürülür. Böylece istemci uygulama, koşullu erişim ilkesi karşılamak için kullanıcı etkileşimi sağlayabilir orta katman hizmet bu hata istemci uygulamasına yüzey.

```
{
    "error":"interaction_required",
    "error_description":"AADSTS50079: Due to a configuration change made by your administrator, or because you moved to a new location, you must enroll in multi-factor authentication to access 'bf8d80f9-9098-4972-b203-500f535113b1'.\r\nTrace ID: b72a68c3-0926-4b8e-bc35-3150069c2800\r\nCorrelation ID: 73d656cf-54b1-4eb2-b429-26d8165a52d7\r\nTimestamp: 2017-05-01 22:43:20Z",
    "error_codes":[50079],
    "timestamp":"2017-05-01 22:43:20Z",
    "trace_id":"b72a68c3-0926-4b8e-bc35-3150069c2800",
    "correlation_id":"73d656cf-54b1-4eb2-b429-26d8165a52d7",
    "claims":"{\"access_token\":{\"polids\":{\"essential\":true,\"values\":[\"9ab03e19-ed42-4168-b6b7-7001fb3e933a\"]}}}"
}
```

## <a name="use-the-access-token-to-access-the-secured-resource"></a>Güvenli kaynağa erişmek için erişim belirteci kullanın
Artık orta katman hizmet belirteci ayarlayarak Aşağı Akış web API'si, kimliği doğrulanmış istekler yapmasını yukarıda edinilen belirteci kullanarak `Authorization` üstbilgi.

### <a name="example"></a>Örnek
```
GET /me?api-version=2013-11-08 HTTP/1.1
Host: graph.windows.net
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ
```

## <a name="next-steps"></a>Sonraki adımlar
OAuth 2.0 protokolünü ve istemci kimlik bilgilerini kullanarak hizmet kimlik doğrulama gerçekleştirmek için başka bir yöntem hakkında daha fazla bilgi edinin.
* [OAuth 2.0 istemci kimlik bilgileri sağlama Azure AD'de kullanarak hizmet kimlik doğrulama hizmeti](active-directory-protocols-oauth-service-to-service.md)
* [Azure AD'de OAuth 2.0](active-directory-protocols-oauth-code.md)
