---
title: On-Behalf-Of taslak belirtimi OAuth2.0 kullanan azure Active Directory hizmetten hizmete kimlik doğrulaması | Microsoft Docs
description: Bu makalede, HTTP iletileri OAuth2.0 On-Behalf-Of akışı ile hizmetten hizmete kimlik doğrulaması uygulamak için kullanmayı açıklar.
services: active-directory
documentationcenter: .net
author: navyasric
manager: CelesteDG
editor: ''
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2019
ms.author: ryanwi
ms.reviewer: hirsin, nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: bc352c6867779fd8f4487acdb1d11c0fabe4b9f7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67110983"
---
# <a name="service-to-service-calls-that-use-delegated-user-identity-in-the-on-behalf-of-flow"></a>Hizmetten hizmete temsilci kullanım kullanıcı kimliği On-Behalf-Of akışı çağırır.

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

OAuth 2.0 On-Behalf-Of (OBO) akış, bir hizmeti veya web API'si, başka bir hizmet için kullanıcı kimlik doğrulaması geçirilecek veya web API'si çağıran bir uygulama sağlar. İstek zincirinin aracılığıyla izinleri ve yetkilendirilmiş kullanıcının kimlik OBO akış yayar. Orta katman hizmet kimliği doğrulanmış istekler aşağı akış hizmetinize, bir erişim belirteci Azure Active Directory'den (Azure AD) kullanıcı adına güvenlik altına almanız gerekir.

> [!IMPORTANT]
> Mayıs 2018'den itibaren bir `id_token` On-Behalf-Of akışı için kullanılamaz.  Tek sayfa uygulamaları (Spa'lar) OBO akışları gerçekleştirmek için orta katman gizli bir istemci için bir erişim belirteci geçmesi gerekir. On-Behalf-Of aramalar gerçekleştirebilirsiniz istemcileri hakkında daha fazla ayrıntı için [sınırlamaları](#client-limitations).

## <a name="on-behalf-of-flow-diagram"></a>On-Behalf-Of akışı diyagramı

OBO flow kullanan bir uygulamayı kullanıcının kimliği doğrulandıktan sonra başlar [OAuth 2.0 yetkilendirme kodu verme akışı](v1-protocols-oauth-code.md). Bu noktada, uygulamasını orta katman web API'sine (API A) kullanıcı talepleri ve API A. erişmek için bir onay içeren bir erişim belirteci (belirteç A) gönderir. Ardından, API bir aşağı akış web API'sine (API B) kimliği doğrulanmış bir isteği yapar.

Bu adımlar, On-Behalf-Of akışı oluşturan: ![OAuth2.0 üzerinde-Behalf-Of akışı](./media/v1-oauth2-on-behalf-of-flow/active-directory-protocols-oauth-on-behalf-of-flow.png)

1. İstemci uygulama bir API A A. belirteciyle istekte
1. API bir Azure AD belirteç yayınında uç noktaya kimliğini doğrular ve API B'nin erişmek için bir belirteç istekleri
1. Azure AD belirteç yayınında uç noktası, API A'ın kimlik belirteci A ile doğrular ve API B (belirteç B) için erişim belirteci verir.
1. API B isteği yetkilendirme üst bilgisi belirteç B içerir.
1. API B güvenli kaynaktan verileri döndürür.

>[!NOTE]
>Bir aşağı akış hizmeti için bir belirteç istemek için bir erişim belirteci hedef kitlesi talebi OBO istekte hizmet kimliği olmalıdır. Belirteç de Azure Active Directory genel imzalama anahtarı ile imzalanması gerekir (aracılığıyla uygulamaları kayıtlı için varsayılan değerdir **uygulama kayıtları** Portalı'nda).

## <a name="register-the-application-and-service-in-azure-ad"></a>Uygulama ve hizmet, Azure AD'ye kaydetme

Orta katman hizmet hem istemci uygulaması, Azure AD'ye kaydetme.

### <a name="register-the-middle-tier-service"></a>Orta katman hizmet kaydı

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst çubuğunda, hesabınızı seçin ve altına bakın **dizin** uygulamanız için bir Active Directory kiracısı seçin.
1. Seçin **diğer hizmetler** sol bölmede seçin **Azure Active Directory**.
1. Seçin **uygulama kayıtları** ardından **yeni kayıt**.
1. Uygulama için kolay bir ad girin ve uygulama türünü seçin.
1. Altında **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında**.
1. Yeniden yönlendirme URI'si için temel URL'yi ayarlayın.
1. Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Azure portalında çıkmadan önce bir gizli anahtar oluşturun.
1. Azure portalında, uygulamanızı seçip **sertifikaları ve parolaları**.
1. Seçin **yeni gizli** ve gizli dizi süre bir yıl veya iki yıl ekleyin.
1. Bu sayfa kaydettiğinizde, Azure portalında gizli değeri görüntüler. Kopyalayın ve gizli değer güvenli bir konuma kaydedin.

> [!IMPORTANT]
> Uygulamanızda uygulama ayarlarını yapılandırmak için gizli anahtar gerekir. Bu gizli değer yeniden gösterilmez ve herhangi başka bir yolla alınabilir değil. Azure portalında görünür duruma geldiği kaydedin.

### <a name="register-the-client-application"></a>İstemci uygulamayı kaydetme

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst çubuğunda, hesabınızı seçin ve altına bakın **dizin** uygulamanız için bir Active Directory kiracısı seçin.
1. Seçin **diğer hizmetler** sol bölmede seçin **Azure Active Directory**.
1. Seçin **uygulama kayıtları** ardından **yeni kayıt**.
1. Uygulama için kolay bir ad girin ve uygulama türünü seçin.
1. Altında **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında**.
1. Yeniden yönlendirme URI'si için temel URL'yi ayarlayın.
1. Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Uygulama izinlerini yapılandırın. İçinde **API izinleri**seçin **bir izin eklemek** ardından **Apı'lerim**.
1. Orta katman hizmet adını metin alanına yazın.
1. Seçin **Select izinleri** seçip **erişim <service name>** .

### <a name="configure-known-client-applications"></a>Bilinen istemci uygulamalarını yapılandırın

Bu senaryoda, kullanıcı etkileşimi olmadan aşağı akış API'ye erişmek için kullanıcı onayı almak orta katman hizmet gerekir. Seçeneği aşağı akış API'sine erişim vermek için kimlik doğrulaması sırasında Ön onay adımının bir parçası olarak ayarlama sunulmalıdır.

Açıkça orta katman hizmet kaydı ile Azure AD'de istemci uygulamasının kaydı bağlamak için aşağıdaki adımları izleyin. Bu işlem istemci ve orta katman tarafından tek bir iletişim kutusuna gerekli onay birleştirir.

1. Orta katman hizmet kaydı için giderek seçin **bildirim** bildirim düzenleyicisini açın.
1. Bulun `knownClientApplications` dizi özelliği ve öğe olarak istemci uygulamasının istemci kimliği ekleyin.
1. Bildirimi seçerek kaydetmek **Kaydet**.

## <a name="service-to-service-access-token-request"></a>Hizmetten hizmete erişim belirteci isteği

Bir HTTP POST yapmak için kiracıya özgü bir erişim belirteci istemek için Azure AD uç noktası şu parametrelerle:

```
https://login.microsoftonline.com/<tenant>/oauth2/token
```

İstemci uygulama, paylaşılan bir gizli dizi veya bir sertifika tarafından sağlanır.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk Durum: Paylaşılan gizlilik ile erişim belirteci isteği

Paylaşılan gizlilik kullanırken, hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |Gerekli | Belirteç isteği türü. Değer olmalıdır bir OBO isteği bir JSON Web Token (JWT) kullanır, bu nedenle **urn: ietf:params:oauth:grant-türü: jwt-taşıyıcı**. |
| assertion |Gerekli | İstekte kullanılan erişim belirteci değeri. |
| client_id |Gerekli | Azure AD ile kayıt sırasında arama hizmete atanan uygulama kimliği. Azure portalında uygulama Kimliğini bulmak için seçin **Active Directory**dizini seçin ve ardından uygulama adı seçin. |
| client_secret |Gerekli | Anahtar arama hizmeti için Azure AD'de kayıtlı. Bu değeri kayıt zamanında Not. |
| resource |Gerekli | Uygulama Kimliği URI'SİNİN alma hizmeti (güvenli kaynak). Uygulama Kimliği URI'si Azure Portalı'nda bulmak için seçin **Active Directory** ve dizini seçin. Uygulama adı seçin, **tüm ayarlar**ve ardından **özellikleri**. |
| requested_token_use |Gerekli | İsteğin nasıl işleneceğini belirtir. On-Behalf-Of akışı değer olmalıdır **on_behalf_of**. |
| scope |Gerekli | Boşlukla ayrılmış belirteci isteği için kapsam listesi. Openıd Connect, kapsam için **openıd** belirtilmesi gerekir.|

#### <a name="example"></a>Örnek

Aşağıdaki HTTP POST istekleri için bir erişim belirteci https://graph.windows.net web API'si. `client_id` Erişim belirteci isteklerini hizmeti tanımlar.

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

### <a name="second-case-access-token-request-with-a-certificate"></a>İkinci durum: Bir sertifika ile erişim belirteci isteği

Bir sertifika ile hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |Gerekli | Belirteç isteği türü. Değer olmalıdır bir JWT belirteç OBO isteği kullandığı **urn: ietf:params:oauth:grant-türü: jwt-taşıyıcı**. |
| assertion |Gerekli | İstekte kullanılan belirteç değeri. |
| client_id |Gerekli | Azure AD ile kayıt sırasında arama hizmete atanan uygulama kimliği. Azure portalında uygulama Kimliğini bulmak için seçin **Active Directory**dizini seçin ve ardından uygulama adı seçin. |
| client_assertion_type |Gerekli |Değer olmalıdır `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Gerekli | Bir JSON Web oluşturan ve sertifika ile oturum belirteci, kimlik bilgileri olarak uygulamanız için kayıtlı. Bkz: [sertifika kimlik bilgileri](active-directory-certificate-credentials.md) sertifikanızı kaydetme ve onaylama biçimi hakkında bilgi edinmek için.|
| resource |Gerekli | Uygulama Kimliği URI'SİNİN alma hizmeti (güvenli kaynak). Uygulama Kimliği URI'si Azure Portalı'nda bulmak için seçin **Active Directory** ve dizini seçin. Uygulama adı seçin, **tüm ayarlar**ve ardından **özellikleri**. |
| requested_token_use |Gerekli | İsteğin nasıl işleneceğini belirtir. On-Behalf-Of akışı değer olmalıdır **on_behalf_of**. |
| scope |Gerekli | Boşlukla ayrılmış belirteci isteği için kapsam listesi. Openıd Connect, kapsam için **openıd** belirtilmesi gerekir.|

Bu parametreler neredeyse aynı paylaşılan gizli diziyi hariç isteğiyle gibi olan `client_secret parameter` tarafından iki parametre değiştirilir: `client_assertion_type` ve `client_assertion`.

#### <a name="example"></a>Örnek

Aşağıdaki HTTP POST istekleri için bir erişim belirteci https://graph.windows.net bir sertifika ile web API'si. `client_id` Erişim belirteci isteklerini hizmeti tanımlar.

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

## <a name="service-to-service-access-token-response"></a>Hizmetten hizmete erişim belirteci yanıtı

Başarılı yanıt, aşağıdaki parametrelerle bir JSON OAuth 2.0 yanıtındaki olan:

| Parametre | Açıklama |
| --- | --- |
| token_type |Belirteç türü değeri gösterir. Azure AD destekleyen tek tür **taşıyıcı**. Taşıyıcı belirteçleri hakkında daha fazla bilgi için bkz: [OAuth 2.0 yetkilendirme Framework: Taşıyıcı belirteç kullanımı (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). |
| scope |Erişim belirtecinde verilen kapsam. |
| expires_in |Süre (saniye cinsinden) erişim belirteci geçerlidir. |
| expires_on |Erişim belirtecinin süresinin sona erdiği zaman. Tarih 1970'ten saniye sayısı temsil edilen-01-kadar süre sonu UTC 01T0:0:0Z. Bu değer, önbelleğe alınan belirteç ömrünü belirlemek için kullanılır. |
| resource |Uygulama Kimliği URI'SİNİN alma hizmeti (güvenli kaynak). |
| access_token |İstenen erişim belirteci. Arama hizmeti, alıcı hizmetinde kimlik doğrulaması için bu belirteci kullanabilirsiniz. |
| id_token |İstenen kimlik belirteci. Arama Hizmeti kullanıcının kimliğini doğrulamak ve kullanıcıyı bir oturum başlatmak için bu belirteci kullanabilirsiniz. |
| refresh_token |İstenen erişim belirtecini yenileme belirteci. Arama hizmeti geçerli erişim belirtecinin süresi dolduktan sonra başka bir erişim belirteci istemek için bu belirteci kullanabilirsiniz. |

### <a name="success-response-example"></a>Başarılı yanıt örneği

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

Azure AD belirteç uç noktası, bir koşullu erişim ilkesi (örneğin, çok faktörlü kimlik doğrulaması) olarak ayarlanmış bir aşağı akış API için bir erişim belirteci almaya çalıştığında bir hata yanıtı döndürür. İstemci uygulaması koşullu erişim ilkesi karşılamak için kullanıcı etkileşimi sağlayabilmesi orta katman hizmet istemci uygulamayı bu hata ortaya.

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

## <a name="use-the-access-token-to-access-the-secured-resource"></a>Güvenli kaynak erişimi için erişim belirteci kullanın

Orta katman hizmet belirteci ayarlayarak Aşağı Akış web API'sine kimliği doğrulanmış istekler yapmasını alınan erişim belirteci kullanabilirsiniz `Authorization` başlığı.

### <a name="example"></a>Örnek

```
GET /me?api-version=2013-11-08 HTTP/1.1
Host: graph.windows.net
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ
```

## <a name="saml-assertions-obtained-with-an-oauth20-obo-flow"></a>SAML onaylamalarını bir OAuth2.0 OBO flow ile elde edilen

Bazı OAuth tabanlı web Hizmetleri, başka bir web hizmeti, etkileşimli olmayan akışlardaki SAML onaylamalarını kabul eden API'leri erişmeniz gerekebilir. Azure Active Directory SAML onaylama işlemi yanıt olarak bir hedef kaynak olarak SAML tabanlı web hizmeti kullanan bir On-Behalf-Of akışı sağlar.

>[!NOTE]
>Bu, bir OAuth2 tabanlı uygulamanın SAML belirteçleri tüketen web hizmeti API uç noktalarını erişmesine izin veren OAuth 2.0 On-Behalf-Of Flow standart olmayan bir uzantısıdır.

> [!TIP]
> Bir ön uç web uygulamasından bir SAML korumalı web hizmeti çağrısı, yalnızca API çağrısı ve mevcut kullanıcının oturumunu normal etkileşimli kimlik doğrulaması akışıyla başlatmak. Yalnızca bir hizmetten hizmete çağrı kullanıcı bağlam sağlamak için bir SAML belirteci gerektirdiğinde OBO akış kullanmanız gerekir.

### <a name="obtain-a-saml-token-by-using-an-obo-request-with-a-shared-secret"></a>OBO isteği ile paylaşılan bir gizli dizi kullanarak SAML belirteç edinme

Hizmetten hizmete istek SAML onaylama işlemi için aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |Gerekli | Belirteç isteği türü. JWT'nin kullanan bir istek için bir değer olmalıdır **urn: ietf:params:oauth:grant-türü: jwt-taşıyıcı**. |
| assertion |Gerekli | İstekte kullanılan erişim belirteci değeri.|
| client_id |Gerekli | Azure AD ile kayıt sırasında arama hizmete atanan uygulama kimliği. Azure portalında uygulama Kimliğini bulmak için seçin **Active Directory**dizini seçin ve ardından uygulama adı seçin. |
| client_secret |Gerekli | Anahtar arama hizmeti için Azure AD'de kayıtlı. Bu değeri kayıt zamanında Not. |
| resource |Gerekli | Uygulama Kimliği URI'SİNİN alma hizmeti (güvenli kaynak). SAML belirteç hedef kitlesi olacak kaynak budur. Uygulama Kimliği URI'si Azure Portalı'nda bulmak için seçin **Active Directory** ve dizini seçin. Uygulama adı seçin, **tüm ayarlar**ve ardından **özellikleri**. |
| requested_token_use |Gerekli | İsteğin nasıl işleneceğini belirtir. On-Behalf-Of akışı değer olmalıdır **on_behalf_of**. |
| requested_token_type | Gerekli | Belirteç talep türünü belirtir. Değer olabilir **urn: ietf:params:oauth:token-türü: saml2** veya **urn: ietf:params:oauth:token-türü: saml1** erişilen kaynak gereksinimlerine bağlı olarak. |

Yanıt Base64url UTF8 ile kodlanan bir SAML belirteci içerir.

- **SAML onaylama işlemi için SubjectConfirmationData bir OBO çağrısından kaynaklanan**: Hedef uygulama için bir alıcı değer gerekiyorsa **SubjectConfirmationData**, değeri kaynak uygulama yapılandırmasında bir joker karakter olmayan yanıt URL'si olmalıdır.
- **SubjectConfirmationData düğümü**: Düğüm içeremez bir **InResponseTo** SAML yanıtını parçası olmadığından özniteliği. SAML belirteci alma uygulama olmadan SAML onayı kabul etmeyi bir **InResponseTo** özniteliği.

- **Onay**: Onay OAuth akışını kullanıcı verilerini içeren bir SAML belirteci almak için verilmiş olmalıdır. İzinler ve yönetici onayı alma hakkında daha fazla bilgi için bkz: [izinler ve onay Azure Active Directory v1.0 uç noktasını](https://docs.microsoft.com/azure/active-directory/develop/v1-permissions-and-consent).

### <a name="response-with-saml-assertion"></a>SAML onaylaması Yanıtla

| Parametre | Açıklama |
| --- | --- |
| token_type |Belirteç türü değeri gösterir. Azure AD destekleyen tek tür **taşıyıcı**. Taşıyıcı belirteçleri hakkında daha fazla bilgi için bkz: [OAuth 2.0 yetkilendirme Framework: Taşıyıcı belirteç kullanımı (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). |
| scope |Erişim belirtecinde verilen kapsam. |
| expires_in |Süre (saniye cinsinden) erişim belirteci geçerlidir. |
| expires_on |Erişim belirtecinin süresinin sona erdiği zaman. Tarih 1970'ten saniye sayısı temsil edilen-01-kadar süre sonu UTC 01T0:0:0Z. Bu değer, önbelleğe alınan belirteç ömrünü belirlemek için kullanılır. |
| resource |Uygulama Kimliği URI'SİNİN alma hizmeti (güvenli kaynak). |
| access_token |SAML onaylaması döndüren parametre. |
| refresh_token |Yenileme belirteci. Arama hizmeti, geçerli bir SAML onayı süresi dolduktan sonra başka bir erişim belirteci istemek için bu belirteci kullanabilirsiniz. |

- token_type: Taşıyıcı
- expires_in: 3296
- ext_expires_in: 0
- expires_on: 1529627844
- Kaynak: `https://api.contoso.com`
- access_token: \<SAML onaylama\>
- issued_token_type: urn: ietf:params:oauth:token-türü: saml2
- refresh_token: \<Belirteci Yenile\>

## <a name="client-limitations"></a>İstemci sınırlamaları

Joker karakter yanıt URL'leri ile ortak istemcileri bir `id_token` OBO akışlar için. Ancak, gizli bir istemci yine de kullanmak **erişim** genel istemci bir joker karakter olsa bile örtük verme akışı edinilen belirteçleri yeniden yönlendirme URI'si kayıtlı.

## <a name="next-steps"></a>Sonraki adımlar

OAuth 2.0 protokolünü ve istemci kimlik bilgilerini kullanan hizmetten hizmete kimlik doğrulaması gerçekleştirmek için başka bir yolu hakkında daha fazla bilgi edinin:

* [OAuth 2.0 istemci kimlik bilgileri verme Azure AD'de kullanarak hizmet kimlik doğrulaması hizmetine](v1-oauth2-client-creds-grant-flow.md)
* [Azure AD'nin OAuth 2.0](v1-protocols-oauth-code.md)
