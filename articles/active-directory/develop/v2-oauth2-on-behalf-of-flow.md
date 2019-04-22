---
title: Microsoft kimlik platformu ve On-Behalf-Of akışı OAuth2.0 | Azure
description: Bu makalede, HTTP iletileri OAuth2.0 On-Behalf-Of akışı kullanarak hizmet kimlik doğrulaması uygulamak için kullanmayı açıklar.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d0c7c29bf3094c3d5fc99b9906ee4469a6643317
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59501612"
---
# <a name="microsoft-identity-platform-and-oauth-20-on-behalf-of-flow"></a>Microsoft kimlik platformu ve OAuth 2.0 On-Behalf-Of akış

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

OAuth 2.0 On-Behalf-Of akış (OBO) sırayla başka bir hizmet/web API'si çağırmayı gerektiren bir uygulama hizmeti/web API'si, burada çağırır kullanım örneği sunar. İstek zincirinin aracılığıyla izinleri ve yetkilendirilmiş kullanıcının kimlik yayılması olur. Orta katman hizmet aşağı akış hizmeti için kimliği doğrulanmış isteğinde bulunmak Microsoft kimlik platformu, kullanıcı adına bir erişim belirteci güvenliğini sağlamak gerekir.

> [!NOTE]
>
> - Microsoft kimlik platformu uç nokta, tüm senaryolar ve Özellikler desteklemiyor. Microsoft kimlik platformu uç noktasını kullanması gerekip gerekmediğini belirlemek için aşağıdaki hakkında bilgi edinin: [Microsoft Identity platform sınırlamaları](active-directory-v2-limitations.md). Özellikle, bilinen istemci uygulamaları, Microsoft hesabı (MSA) ve Azure AD hedef kitlelere sahip uygulamalar için desteklenmez. Bu nedenle, OBO için yaygın bir onay Düzen hem kişisel ve iş veya Okul hesaplarında oturum istemciler için çalışmaz. Akışın bu adımın nasıl işleneceği hakkında daha fazla bilgi için bkz: [Orta katmanlı uygulama için onay sağlamasını](#gaining-consent-for-the-middle-tier-application).
> - Mayıs 2018'den itibaren bazı akış örtük türetilmiş `id_token` OBO akış için kullanılamaz. Tek sayfa uygulamaları (Spa'lar) geçirmesi gerekir bir **erişim** bir orta katman belirtecini OBO gerçekleştirmek için gizli bir istemci yerine akar. Hangi istemcilerin OBO çağrıları gerçekleştirebilir daha fazla bilgi için bkz. [sınırlamaları](#client-limitations).

## <a name="protocol-diagram"></a>Protokol diyagramı

Kullanıcı bir uygulama kullanarak doğrulandıktan varsayar [OAuth 2.0 yetkilendirme kodu verme akışı](v2-oauth2-auth-code-flow.md). Bu noktada, uygulama erişim belirteci sahip *API a* (simge, A) kullanıcı talepleri ve orta katmanda erişmek için bir onay ile API (API A) web. Şimdi, API bir aşağı akış web API'sine (API B) kimliği doğrulanmış bir istekte gerekir.

Aşağıdaki adımları OBO akışı oluşturan ve aşağıdaki diyagramda yardımıyla açıklanmıştır.

![OAuth2.0 On-Behalf-Of akışı](./media/v2-oauth2-on-behalf-of-flow/protocols-oauth-on-behalf-of-flow.png)

1. İstemci uygulama bir API A belirteciyle bir istekte (ile bir `aud` API bir talep).
1. API A için Microsoft kimlik platformu belirteç yayınında uç nokta kimlik doğrulaması ve API B'nin erişmek için bir belirteç istekleri
1. Microsoft kimlik platformu belirteç yayınında uç noktası, API A'ın kimlik belirteci A ile doğrular ve API B (belirteç B) için erişim belirteci verir.
1. API b isteğin yetkilendirme üst bilgisi belirteç B ayarlanır
1. Güvenli kaynaktan veri API b tarafından döndürülür.

> [!NOTE]
> Bu senaryoda, kullanıcı etkileşimi olmadan aşağı akış API'ye erişmek için kullanıcı onayı almak için orta katman hizmet vardır. Bu nedenle, adım onay bir parçası olarak kimlik doğrulaması sırasında önceden aşağı akış API'sine erişim izni seçeneği sunulur. Uygulamanız için bunu öğrenmek için bkz. [Orta katmanlı uygulama için onay sağlamasını](#gaining-consent-for-the-middle-tier-application).

## <a name="service-to-service-access-token-request"></a>Hizmetten hizmete erişim belirteci isteği

Bir erişim belirteci istemek için kiracıya özgü Microsoft kimlik platformu belirteç uç noktası aşağıdaki parametrelerle bir HTTP POST olun.

```
https://login.microsoftonline.com/<tenant>/oauth2/v2.0/token
```

İstemci uygulaması paylaşılan bir gizli dizi veya bir sertifika tarafından güvenli hale seçti olup olmadığına bağlı olarak iki durum vardır.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk Durum: Paylaşılan gizlilik ile erişim belirteci isteği

Paylaşılan gizlilik kullanırken, hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| `grant_type` | Gerekli | Belirteç isteği türü. JWT'nin kullanarak bir istek için bir değer olmalıdır `urn:ietf:params:oauth:grant-type:jwt-bearer`. |
| `client_id` | Gerekli | (İstemci) uygulama kimliği [Azure Portalı - Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfa uygulamanıza atanan. |
| `client_secret` | Gerekli | Uygulamanızı Azure portalında - uygulama kayıtları sayfası için oluşturulan istemci gizli anahtarı. |
| `assertion` | Gerekli | İstekte kullanılan belirteç değeri. |
| `scope` | Gerekli | Boşlukla ayrılmış belirteci isteği için kapsam listesi. Daha fazla bilgi için [kapsamları](v2-permissions-and-consent.md). |
| `requested_token_use` | Gerekli | İsteğin nasıl işleneceğini belirtir. OBO flow'da değeri ayarlamak `on_behalf_of`. |

#### <a name="example"></a>Örnek

Bir erişim belirteci ve yenileme belirteci ile aşağıdaki HTTP POST istekleri `user.read` için kapsam https://graph.microsoft.com web API'si.

```
//line breaks for legibility only

POST /oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer
&client_id=2846f71b-a7a4-4987-bab3-760035b2f389
&client_secret=BYyVnAt56JpLwUcyo47XODd
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3L3YyLjAiLCJpYXQiOjE0OTM5MjA5MTYsIm5iZiI6MTQ5MzkyMDkxNiwiZXhwIjoxNDkzOTI0ODE2LCJhaW8iOiJBU1FBMi84REFBQUFnZm8vNk9CR0NaaFV2NjJ6MFFYSEZKR0VVYUIwRUlIV3NhcGducndMMnVrPSIsIm5hbWUiOiJOYXZ5YSBDYW51bWFsbGEiLCJvaWQiOiJkNWU5NzljNy0zZDJkLTQyYWYtOGYzMC03MjdkZDRjMmQzODMiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJuYWNhbnVtYUBtaWNyb3NvZnQuY29tIiwic3ViIjoiZ1Q5a1FMN2hXRUpUUGg1OWJlX1l5dVZNRDFOTEdiREJFWFRhbEQzU3FZYyIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInV0aSI6IjN5U3F4UHJweUVPd0ZsTWFFMU1PQUEiLCJ2ZXIiOiIyLjAifQ.TPPJSvpNCSCyUeIiKQoLMixN1-M-Y5U0QxtxVkpepjyoWNG0i49YFAJC6ADdCs5nJXr6f-ozIRuaiPzy29yRUOdSz_8KqG42luCyC1c951HyeDgqUJSz91Ku150D9kP5B9-2R-jgCerD_VVuxXUdkuPFEl3VEADC_1qkGBiIg0AyLLbz7DTMp5DvmbC09DhrQQiouHQGFSk2TPmksqHm3-b3RgeNM1rJmpLThis2ZWBEIPx662pjxL6NJDmV08cPVIcGX4KkFo54Z3rfwiYg4YssiUc4w-w3NJUBQhnzfTl4_Mtq2d7cVlul9uDzras091vFy32tWkrpa970UvdVfQ
&scope=https://graph.microsoft.com/user.read+offline_access
&requested_token_use=on_behalf_of
```

### <a name="second-case-access-token-request-with-a-certificate"></a>İkinci durum: Bir sertifika ile erişim belirteci isteği

Bir sertifika ile hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| `grant_type` | Gerekli | Belirteç isteği türü. JWT'nin kullanarak bir istek için bir değer olmalıdır `urn:ietf:params:oauth:grant-type:jwt-bearer`. |
| `client_id` | Gerekli |  (İstemci) uygulama kimliği [Azure Portalı - Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfa uygulamanıza atanan. |
| `client_assertion_type` | Gerekli | Değer olmalıdır `urn:ietf:params:oauth:client-assertion-type:jwt-bearer`. |
| `client_assertion` | Gerekli | Oluşturma ve sertifika ile imzalamak için gereken onaylama (bir JSON web belirteci) uygulamanız için kimlik bilgileri olarak kayıtlı. Sertifikanızı ve onaylama biçimi kaydetme hakkında bilgi için bkz: [sertifika kimlik bilgileri](active-directory-certificate-credentials.md). |
| `assertion` | Gerekli | İstekte kullanılan belirteç değeri. |
| `requested_token_use` | Gerekli | İsteğin nasıl işleneceğini belirtir. OBO flow'da değeri ayarlamak `on_behalf_of`. |
| `scope` | Gerekli | Belirteç isteği için kapsamları boşlukla ayrılmış listesi. Daha fazla bilgi için [kapsamları](v2-permissions-and-consent.md).|

Parametreler neredeyse aynı paylaşılan gizli diziyi hariç isteğiyle gibi söz konusu olduğunda olduğunu fark `client_secret` parametresi tarafından iki parametre değiştirilir: bir `client_assertion_type` ve `client_assertion`.

#### <a name="example"></a>Örnek

Bir erişim belirteci ile aşağıdaki HTTP POST istekleri `user.read` için kapsam https://graph.microsoft.com bir sertifika ile web API'si.

```
// line breaks for legibility only

POST /oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=https://graph.microsoft.com/user.read+offline_access
```

## <a name="service-to-service-access-token-response"></a>Hizmet erişim belirteci yanıtı hizmetine

Başarılı yanıtı aşağıdaki parametrelerle bir JSON OAuth 2.0 yanıtındaki ' dir.

| Parametre | Açıklama |
| --- | --- |
| `token_type` | Belirteç türü değeri gösterir. Microsoft kimlik platformu destekleyen tek yazın `Bearer`. Taşıyıcı belirteçleri hakkında daha fazla bilgi için bkz. [OAuth 2.0 yetkilendirme Framework: Taşıyıcı belirteç kullanımı (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). |
| `scope` | Erişim belirtecinde verilen kapsam. |
| `expires_in` | Erişim belirtecinin geçerli olduğu saniye cinsinden süre uzunluğu. |
| `access_token` | İstenen erişim belirteci. Arama hizmeti, alıcı hizmetinde kimlik doğrulaması için bu belirteci kullanabilirsiniz. |
| `refresh_token` | İstenen erişim belirtecini yenileme belirteci. Arama hizmeti geçerli erişim belirtecinin süresi dolduktan sonra başka bir erişim belirteci istemek için bu belirteci kullanabilirsiniz. Yenileme belirteci yalnızca, sağlanan `offline_access` kapsam istendi. |

### <a name="success-response-example"></a>Başarılı yanıt örneği

Aşağıdaki örnek, bir başarı isteğine yanıt olarak bir erişim belirteci için gösterir https://graph.microsoft.com web API'si.

```
{
  "token_type": "Bearer",
  "scope": "https://graph.microsoft.com/user.read",
  "expires_in": 3269,
  "ext_expires_in": 0,
  "access_token": "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQTZOVGFlN0NkV1c3UWZkQ0NDYy0tY0hGa18wZE50MVEtc2loVzRMd2RwQVZISGpnTVdQZ0tQeVJIaGlDbUN2NkdyMEpmYmRfY1RmMUFxU21TcFJkVXVydVJqX3Nqd0JoN211eHlBQSIsImFsZyI6IlJTMjU2IiwieDV0IjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIiwia2lkIjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIn0.eyJhdWQiOiJodHRwczovL2dyYXBoLm1pY3Jvc29mdC5jb20iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaWF0IjoxNDkzOTMwMzA1LCJuYmYiOjE0OTM5MzAzMDUsImV4cCI6MTQ5MzkzMzg3NSwiYWNyIjoiMCIsImFpbyI6IkFTUUEyLzhEQUFBQU9KYnFFWlRNTnEyZFcxYXpKN1RZMDlYeDdOT29EMkJEUlRWMXJ3b2ZRc1k9IiwiYW1yIjpbInB3ZCJdLCJhcHBfZGlzcGxheW5hbWUiOiJUb2RvRG90bmV0T2JvIiwiYXBwaWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJhcHBpZGFjciI6IjEiLCJmYW1pbHlfbmFtZSI6IkNhbnVtYWxsYSIsImdpdmVuX25hbWUiOiJOYXZ5YSIsImlwYWRkciI6IjE2Ny4yMjAuMC4xOTkiLCJuYW1lIjoiTmF2eWEgQ2FudW1hbGxhIiwib2lkIjoiZDVlOTc5YzctM2QyZC00MmFmLThmMzAtNzI3ZGQ0YzJkMzgzIiwib25wcmVtX3NpZCI6IlMtMS01LTIxLTIxMjc1MjExODQtMTYwNDAxMjkyMC0xODg3OTI3NTI3LTI2MTE4NDg0IiwicGxhdGYiOiIxNCIsInB1aWQiOiIxMDAzM0ZGRkEwNkQxN0M5Iiwic2NwIjoiVXNlci5SZWFkIiwic3ViIjoibWtMMHBiLXlpMXQ1ckRGd2JTZ1JvTWxrZE52b3UzSjNWNm84UFE3alVCRSIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInVuaXF1ZV9uYW1lIjoibmFjYW51bWFAbWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hY2FudW1hQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJWR1ItdmtEZlBFQ2M1dWFDaENRSkFBIiwidmVyIjoiMS4wIn0.cubh1L2VtruiiwF8ut1m9uNBmnUJeYx4x0G30F7CqSpzHj1Sv5DCgNZXyUz3pEiz77G8IfOF0_U5A_02k-xzwdYvtJUYGH3bFISzdqymiEGmdfCIRKl9KMeoo2llGv0ScCniIhr2U1yxTIkIpp092xcdaDt-2_2q_ql1Ha_HtjvTV1f9XR3t7_Id9bR5BqwVX5zPO7JMYDVhUZRx08eqZcC-F3wi0xd_5ND_mavMuxe2wrpF-EZviO3yg0QVRr59tE3AoWl8lSGpVc97vvRCnp4WVRk26jJhYXFPsdk4yWqOKZqzr3IFGyD08WizD_vPSrXcCPbZP3XWaoTUKZSNJg",
  "refresh_token": "OAQABAAAAAABnfiG-mA6NTae7CdWW7QfdAALzDWjw6qSn4GUDfxWzJDZ6lk9qRw4AnqPnvFqnzS3GiikHr5wBM1bV1YyjH3nUeIhKhqJWGwqJFRqs2sE_rqUfz7__3J92JDpi6gDdCZNNaXgreQsH89kLCVNYZeN6kGuFGZrjwxp1wS2JYc97E_3reXBxkHrA09K5aR-WsSKCEjf6WI23FhZMTLhk_ZKOe_nWvcvLj13FyvSrTMZV2cmzyCZDqEHtPVLJgSoASuQlD2NXrfmtcmgWfc3uJSrWLIDSn4FEmVDA63X6EikNp9cllH3Gp7Vzapjlnws1NQ1_Ff5QrmBHp_LKEIwfzVKnLLrQXN0EzP8f6AX6fdVTaeKzm7iw6nH0vkPRpUeLc3q_aNsPzqcTOnFfgng7t2CXUsMAGH5wclAyFCAwL_Cds7KnyDLL7kzOS5AVZ3Mqk2tsPlqopAiHijZaJumdTILDudwKYCFAMpUeUwEf9JmyFjl2eIWPmlbwU7cHKWNvuRCOYVqbsTTpJthwh4PvsL5ov5CawH_TaV8omG_tV6RkziHG9urk9yp2PH9gl7Cv9ATa3Vt3PJWUS8LszjRIAJmyw_EhgHBfYCvEZ8U9PYarvgqrtweLcnlO7BfnnXYEC18z_u5wemAzNBFUje2ttpGtRmRic4AzZ708tBHva2ePJWGX6pgQbiWF8esOrvWjfrrlfOvEn1h6YiBW291M022undMdXzum6t1Y1huwxHPHjCAA"
}
```

> [!NOTE]
> Yukarıdaki erişim belirteci v1.0 biçimli bir belirteçtir. Belirteç erişilen kaynak dayalı olarak sunulur olmasıdır. Bir istemci için Microsoft Graph belirteçleri istediğinde Microsoft kimlik platformu v1.0 erişim belirteçleri oluşturur. Bu nedenle, Microsoft Graph v1.0 belirteçleri, ister. Yalnızca uygulamalara erişim belirteçleri görünmelidir. İstemciler bunları inceleyin yapmanız gerekmez.

### <a name="error-response-example"></a>Hata yanıtı örneği

Aşağı Akış API koşullu erişim ilkesi ayarlayabilirsiniz (örneğin, çok faktörlü kimlik doğrulaması) varsa aşağı akış API'si için bir erişim belirteci almak çalışırken, bir hata yanıtı belirteç uç noktası tarafından döndürülür. İstemci uygulaması koşullu erişim ilkesi karşılamak için kullanıcı etkileşimi sağlayabilmesi orta katman hizmet istemci uygulamayı bu hata ortaya.

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

Orta katman hizmet belirteci ayarlayarak, Aşağı Akış web API'sine, kimliği doğrulanmış isteğinde bulunmak için yukarıda edinilen belirteci artık `Authorization` başlığı.

### <a name="example"></a>Örnek

```
GET /v1.0/me HTTP/1.1
Host: graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQTZOVGFlN0NkV1c3UWZkSzdNN0RyNXlvUUdLNmFEc19vdDF3cEQyZjNqRkxiNlVrcm9PcXA2cXBJclAxZVV0QktzMHEza29HN3RzXzJpSkYtQjY1UV8zVGgzSnktUHZsMjkxaFNBQSIsImFsZyI6IlJTMjU2IiwieDV0IjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIiwia2lkIjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIn0.eyJhdWQiOiJodHRwczovL2dyYXBoLm1pY3Jvc29mdC5jb20iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaWF0IjoxNDkzOTMwMDE2LCJuYmYiOjE0OTM5MzAwMTYsImV4cCI6MTQ5MzkzMzg3NSwiYWNyIjoiMCIsImFpbyI6IkFTUUEyLzhEQUFBQUlzQjN5ZUljNkZ1aEhkd1YxckoxS1dlbzJPckZOUUQwN2FENTVjUVRtems9IiwiYW1yIjpbInB3ZCJdLCJhcHBfZGlzcGxheW5hbWUiOiJUb2RvRG90bmV0T2JvIiwiYXBwaWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJhcHBpZGFjciI6IjEiLCJmYW1pbHlfbmFtZSI6IkNhbnVtYWxsYSIsImdpdmVuX25hbWUiOiJOYXZ5YSIsImlwYWRkciI6IjE2Ny4yMjAuMC4xOTkiLCJuYW1lIjoiTmF2eWEgQ2FudW1hbGxhIiwib2lkIjoiZDVlOTc5YzctM2QyZC00MmFmLThmMzAtNzI3ZGQ0YzJkMzgzIiwib25wcmVtX3NpZCI6IlMtMS01LTIxLTIxMjc1MjExODQtMTYwNDAxMjkyMC0xODg3OTI3NTI3LTI2MTE4NDg0IiwicGxhdGYiOiIxNCIsInB1aWQiOiIxMDAzM0ZGRkEwNkQxN0M5Iiwic2NwIjoiVXNlci5SZWFkIiwic3ViIjoibWtMMHBiLXlpMXQ1ckRGd2JTZ1JvTWxrZE52b3UzSjNWNm84UFE3alVCRSIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInVuaXF1ZV9uYW1lIjoibmFjYW51bWFAbWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hY2FudW1hQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJzUVlVekYxdUVVS0NQS0dRTVFVRkFBIiwidmVyIjoiMS4wIn0.Hrn__RGi-HMAzYRyCqX3kBGb6OS7z7y49XPVPpwK_7rJ6nik9E4s6PNY4XkIamJYn7tphpmsHdfM9lQ1gqeeFvFGhweIACsNBWhJ9Nx4dvQnGRkqZ17KnF_wf_QLcyOrOWpUxdSD_oPKcPS-Qr5AFkjw0t7GOKLY-Xw3QLJhzeKmYuuOkmMDJDAl0eNDbH0HiCh3g189a176BfyaR0MgK8wrXI_6MTnFSVfBePqklQeLhcr50YTBfWg3Svgl6MuK_g1hOuaO-XpjUxpdv5dZ0SvI47fAuVDdpCE48igCX5VMj4KUVytDIf6T78aIXMkYHGgW3-xAmuSyYH_Fr0yVAQ
```

## <a name="gaining-consent-for-the-middle-tier-application"></a>Orta katman uygulama için onay elde etme

Uygulamanız için hedef kitle bağlı olarak, OBO akışın başarılı olduğu için farklı stratejiler düşünebilirsiniz. Her durumda, nihai amacıyla uygun izin verilen sağlamak içindir. Ancak oluşan nasıl hangi kullanıcıların uygulamanızı bağlıdır destekler.

### <a name="consent-for-azure-ad-only-applications"></a>Azure yalnızca AD uygulamaları için onay

#### <a name="default-and-combined-consent"></a>/.default ve birleştirilmiş onay

Yalnızca iş veya Okul hesapları için gerek duyan uygulamalar için geleneksel "Bilinen istemci uygulamalar" yaklaşım yeterli olur. Orta katman uygulama istemci bildirimi bilinen istemci uygulamalar listesinde ekler ve ardından istemci birleştirilmiş onay akışı hem kendisi ve orta katman uygulama için tetikleyebilirsiniz. Microsoft kimlik platformu noktadaki yapıldığını kullanarak [ `/.default` kapsam](v2-permissions-and-consent.md#the-default-scope). Bilinen istemci uygulamalarını kullanarak bir onay ekranında tetiklerken ve `/.default`, onay ekranında hem de istemci orta katman API için izinler gösterir ve ayrıca izinlere orta katman API'sı tarafından gerekli istek. Kullanıcı her iki uygulama için onay sağlar ve ardından OBO akış çalışır.

Şu anda kişisel Microsoft hesap sistemi, birleştirilmiş onay desteklemiyor ve bu nedenle bu yaklaşım özellikle kişisel hesaplarında oturum açmak istediğiniz uygulamalar için çalışmaz. Kişisel Microsoft hesapları bir kiracıdaki Konuk hesabı olarak kullanılan Azure AD sistem kullanılarak ele alınır ve birleşik onayı ile gidebilirsiniz.

#### <a name="pre-authorized-applications"></a>Önceden yetkilendirilmiş uygulamalar

"Önceden yetkilendirilmiş uygulamalar" uygulama portalı özelliğidir. Bu şekilde, her zaman belirli bir uygulama belirli kapsamları alma izni olan bir kaynak belirtebilirsiniz. Bu, özellikle istemci ön uç ve arka uç kaynağı arasındaki bağlantıları daha sorunsuz hale getirmek yararlıdır. Kaynak birden fazla önceden yetkilendirilmiş uygulamalar bildirebilirsiniz: Bu tür bir uygulama bu izinlerin bir OBO akış ve bunları onay veren kullanıcıya sorulmadan almak isteyebilirsiniz.

#### <a name="admin-consent"></a>Yönetici onayı

Bir kiracı Yöneticisi, uygulamaları Orta katmanlı bir uygulama için yönetici onayı sağlayarak kendi gerekli API'leri çağırma izni olduğunu garanti edebilir. Bunu yapmak için yönetici kendilerine ait kiracıda orta katman uygulamayı bulun, gerekli izinleri sayfasını açın ve uygulama için izin vermek seçin. Yönetici onayı hakkında daha fazla bilgi için bkz: [onay ve izinlerini belgeleri](v2-permissions-and-consent.md).

### <a name="consent-for-azure-ad--microsoft-account-applications"></a>Azure AD için onay + Microsoft hesabı uygulaması

Kişisel hesapları için izinler modeli ve değerlendirip Kiracı eksikliği kısıtlamaları nedeniyle, kişisel hesapları için izin gereksinimleri Azure AD'den bir bit büyük/küçük harf farklıdır. Kiracı genelinde izin sağlamak için Kiracı yok ya da olan var. yapabilme birleştirilmiş onay. Bu nedenle, mevcut diğer stratejiler kendilerini - bu uygulamalar için Azure AD hesapları desteklemek için tek gereken iş unutmayın.

#### <a name="use-of-a-single-application"></a>Tek bir uygulama kullanımı

Bazı senaryolarda, yalnızca tek bir orta katman ve ön uç istemcisi eşlemesine sahip olabilir. Bu senaryoda, bunu tek bir uygulama bir orta katman uygulama gereksinimini tamamen negating daha kolay. Ön uç ve web API'si arasında kimlik doğrulaması için tanımlama bilgileri, bir id_token veya uygulama için istenen bir erişim belirteci kullanabilirsiniz. Ardından, arka uç kaynağa tek bu uygulamadan onayı isteyin.

## <a name="client-limitations"></a>İstemci sınırlamaları

Örtük akış bir id_token almak için istemcinin ve istemci joker karakterler bir yanıt URL'si da vardır., id_token bir OBO akışı için kullanılamaz.  Ancak, kayıtlı bir joker karakter yanıt URL'si başlatma istemci sahip olsa bile örtük verme akışı edinilen erişim belirteçleri hala gizli bir istemci tarafından ödenebilecek.

## <a name="next-steps"></a>Sonraki adımlar

OAuth 2.0 protokolünü ve istemci kimlik bilgilerini kullanarak hizmet kimlik doğrulaması gerçekleştirmek için başka bir yöntem hakkında daha fazla bilgi edinin.

* [Microsoft kimlik platformu OAuth 2.0 istemci kimlik bilgileri verme](v2-oauth2-client-creds-grant-flow.md)
* [Microsoft kimlik platformu OAuth 2.0 kod akışı](v2-oauth2-auth-code-flow.md)
* [Kullanarak `/.default` kapsamı](v2-permissions-and-consent.md#the-default-scope)
