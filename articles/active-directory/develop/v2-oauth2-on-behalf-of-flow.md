---
title: Azure AD v2.0 OAuth2.0 On-Behalf-Of akışı | Microsoft Docs
description: Bu makalede, HTTP iletileri OAuth2.0 On-Behalf-Of akışı kullanarak hizmet kimlik doğrulaması uygulamak için kullanmayı açıklar.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: ad7bb3c3a7bd50521b968b7c1a4e21027fbe18f2
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49986061"
---
# <a name="azure-active-directory-v20-and-oauth-20-on-behalf-of-flow"></a>Azure Active Directory v2.0 ve OAuth 2.0 On-Behalf-Of akış

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

OAuth 2.0 akışı bir uygulama hizmeti/sırayla başka çağırmak için gereken web API'si, burada çağırır kullanım örneği hizmet On-Behalf-Of hizmet/web API'si. İstek zincirinin aracılığıyla izinleri ve yetkilendirilmiş kullanıcının kimlik yayılması olur. Orta katman hizmet kimliği doğrulanmış istekler aşağı akış hizmetinize, Azure Active Directory'den (Azure AD), bir erişim belirteci güvenliğini sağlamak kullanıcının adına olmalıdır.

> [!NOTE]
> V2.0 uç noktası, tüm Azure Active Directory senaryolarını ve özelliklerini desteklemez. V2.0 uç noktası kullanması gerekip gerekmediğini belirlemek için aşağıdaki hakkında bilgi edinin: [v2.0 sınırlamaları](active-directory-v2-limitations.md).
>

> [!IMPORTANT]
> Mayıs 2018'den itibaren bir `id_token` kullanılamaz On-Behalf-Of akışı için - Spa'lar geçmesi gereken bir **erişim** OBO gerçekleştirmek için gizli bir istemci bir orta katman belirtecini akar. Bkz: [sınırlamaları](#client-limitations) üzerinde istemcileri On-Behalf-Of çağrıları gerçekleştirebilir daha fazla ayrıntı için.

## <a name="protocol-diagram"></a>Protokol diyagramı
Kullanıcı bir uygulama kullanarak doğrulandıktan varsayar [OAuth 2.0 yetkilendirme kodu verme akışı](v2-oauth2-auth-code-flow.md). Bu noktada, uygulama erişim belirteci sahip *API a* (simge, A) kullanıcı talepleri ve orta katmanda erişmek için bir onay ile API (API A) web. Şimdi, API bir aşağı akış web API'sine (API B) kimliği doğrulanmış bir istekte gerekir.

Aşağıdaki adımları On-Behalf-Of akışı oluşturan ve aşağıdaki diyagramda yardımıyla açıklanmıştır.

![OAuth2.0 üzerinde-Behalf-Of akışı](./media/v1-oauth2-on-behalf-of-flow/active-directory-protocols-oauth-on-behalf-of-flow.png)


1. İstemci uygulama bir API A belirteciyle bir istekte (ile bir `aud` API bir talep).
2. API bir Azure AD belirteç yayınında uç noktaya kimliğini doğrular ve API B'nin erişmek için bir belirteç istekleri
3. Azure AD belirteç yayınında uç noktası, API A'ın kimlik belirteci A ile doğrular ve API B (belirteç B) için erişim belirteci verir.
4. ' % S'belirteci B API b isteğin yetkilendirme üst bilgisinde ayarlama
5. Güvenli kaynaktan veri API b tarafından döndürülür.

> [!NOTE]
> Bu senaryoda, kullanıcı etkileşimi olmadan aşağı akış API'ye erişmek için kullanıcı onayı almak için orta katman hizmet vardır. Bu nedenle, adım onay bir parçası olarak kimlik doğrulaması sırasında önceden aşağı akış API'sine erişim izni seçeneği sunulur.
>

## <a name="service-to-service-access-token-request"></a>Hizmet erişim belirteci isteği için hizmeti
Bir HTTP POST yapmak için kiracıya özgü bir erişim belirteci istemek için Azure AD v2.0 uç noktası aşağıdaki parametrelere sahip.

```
https://login.microsoftonline.com/<tenant>/oauth2/v2.0/token
```

İstemci uygulaması paylaşılan bir gizli dizi veya bir sertifika tarafından güvenli hale seçti olup olmadığına bağlı olarak iki durum vardır.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk durumda: paylaşılan bir gizli dizi ile erişim belirteci isteği
Paylaşılan gizlilik kullanırken, hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type değeri |gerekli | Belirteç isteği türü. JWT'nin kullanarak bir istek için bir değer olmalıdır **urn: ietf:params:oauth:grant-türü: jwt-taşıyıcı**. |
| client_id |gerekli | Uygulama Kimliği [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) uygulamanıza atanan. |
| client_secret |gerekli | Uygulama kayıt portalında uygulamanıza için oluşturulan uygulama gizli anahtarı. |
| onaylama |gerekli | İstekte kullanılan belirteç değeri. |
| scope |gerekli | Boşlukla ayrılmış belirteci isteği için kapsam listesi. Daha fazla bilgi için [kapsamları](v2-permissions-and-consent.md).|
| requested_token_use |gerekli | İsteğin nasıl işleneceğini belirtir. On-Behalf-Of akışı değer olmalıdır **on_behalf_of**. |

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

### <a name="second-case-access-token-request-with-a-certificate"></a>İkinci durumda: bir sertifika ile erişim belirteci isteği
Bir sertifika ile hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type değeri |gerekli | Belirteç isteği türü. JWT'nin kullanarak bir istek için bir değer olmalıdır **urn: ietf:params:oauth:grant-türü: jwt-taşıyıcı**. |
| client_id |gerekli | Uygulama Kimliği [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) uygulamanıza atanan. |
| client_assertion_type |gerekli |Değer olmalıdır `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |gerekli | Onaylama (bir JSON Web belirteci) oluşturmak ve sertifika ile imzalamak için gereken kimlik bilgileri olarak uygulamanız için kayıtlı. Hakkında bilgi edinin [sertifika kimlik bilgileri](active-directory-certificate-credentials.md) sertifikanız ve onaylama biçimi kaydetme hakkında bilgi edinmek için.|
| onaylama |gerekli | İstekte kullanılan belirteç değeri. |
| requested_token_use |gerekli | İsteğin nasıl işleneceğini belirtir. On-Behalf-Of akışı değer olmalıdır **on_behalf_of**. |
| scope |gerekli | Boşlukla ayrılmış belirteci isteği için kapsam listesi. Daha fazla bilgi için [kapsamları](v2-permissions-and-consent.md).|

Client_secret parametresi tarafından iki parametre değiştirilir dışında parametreler neredeyse aynı paylaşılan gizli diziyi isteğiyle olduğu gibi olduğuna dikkat edin: client_assertion_type ve client_assertion.

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
| token_type |Belirteç türü değeri gösterir. Azure AD destekleyen tek tür **taşıyıcı**. Taşıyıcı belirteçleri hakkında daha fazla bilgi için bkz: [OAuth 2.0 yetkilendirme Framework: taşıyıcı belirteç kullanımı (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| scope |Erişim belirtecinde verilen kapsam. |
| expires_in |Süre (saniye cinsinden) erişim belirteci geçerlidir. |
| access_token |İstenen erişim belirteci. Arama hizmeti, alıcı hizmetinde kimlik doğrulaması için bu belirteci kullanabilirsiniz. |
| refresh_token |İstenen erişim belirtecini yenileme belirteci. Arama hizmeti geçerli erişim belirtecinin süresi dolduktan sonra başka bir erişim belirteci istemek için bu belirteci kullanabilirsiniz. Yenileme belirteci yalnızca, sağlanan `offline_access` kapsam istendi.|

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
> Yukarıdaki erişim belirteci V1 olarak biçimlendirilmiş belirteç olduğuna dikkat edin. Belirteç erişilen kaynak dayalı olarak sunulur olmasıdır. Bir istemci için Microsoft Graph belirteçleri istediğinde Azure AD V1 erişim belirteçleri oluşturur. Bu nedenle, Microsoft Graph V1 belirteçleri, ister. Uygulamaları erişim belirteçleri görünmelidir - istemciler, bunları inceleyin gerekmez. yalnızca. 


### <a name="error-response-example"></a>Hata yanıtı örneği
Aşağı Akış API ayarlayabilirsiniz çok faktörlü kimlik doğrulaması gibi bir koşullu erişim ilkesi varsa aşağı akış API'si için bir erişim belirteci almak çalışırken, bir hata yanıtı Azure AD belirteç uç noktası tarafından döndürülür. İstemci uygulaması koşullu erişim ilkesi karşılamak için kullanıcı etkileşimi sağlayabilmesi orta katman hizmet istemci uygulamayı bu hata ortaya.

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

## <a name="client-limitations"></a>İstemci sınırlamaları
Örtük akış bir id_token almak için istemcinin ve istemci joker karakterler bir yanıt URL'si da vardır., id_token bir OBO akışı için kullanılamaz.  Ancak, kayıtlı bir joker karakter yanıt URL'si başlatma istemci sahip olsa bile örtük verme akışı edinilen erişim belirteçleri hala gizli bir istemci tarafından ödenebilecek. 

## <a name="next-steps"></a>Sonraki adımlar
OAuth 2.0 protokolünü ve istemci kimlik bilgilerini kullanarak hizmet kimlik doğrulaması gerçekleştirmek için başka bir yöntem hakkında daha fazla bilgi edinin.
* [Azure AD v2.0 OAuth 2.0 istemci kimlik bilgileri verme](v2-oauth2-client-creds-grant-flow.md)
* [Azure AD v2.0, OAuth 2.0](v2-oauth2-auth-code-flow.md)
