---
title: Azure AD v2.0 OAuth2.0 üzerinde-temsili akış | Microsoft Docs
description: Bu makalede HTTP iletisi üzerinde-temsili akış OAuth2.0 kullanarak hizmet kimlik doğrulaması uygulamak için nasıl kullanılacağını açıklar.
services: active-directory
documentationcenter: ''
author: hpsin
manager: mtillman
editor: ''
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: hirsin
ms.custom: aaddev
ms.openlocfilehash: ccec8df0741870f3dd3ed21be43f96aa8ba90927
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-active-directory-v20-and-oauth-20-on-behalf-of-flow"></a>Azure Active Directory v2.0 ve OAuth 2.0 On-Behalf-Of akış
Burada bir hizmet/sırayla başka çağırmak için gereken web API, uygulamanın çağırır kullanım örneği akış hizmet OAuth 2.0 On-Behalf-Of hizmeti/web API. Temsilci atanan kullanıcı kimliğini ve izinleri istek zincirinin aracılığıyla yaymak için kullanılan uygulamadır. Orta katman hizmet kimliği doğrulanmış istekler için aşağı akış hizmeti yapmak, Azure Active Directory'den (Azure AD), bir erişim belirteci güvenli kullanıcı adına gerekir.

> [!NOTE]
> V2.0 uç noktası, tüm Azure Active Directory senaryolarını ve özelliklerini desteklemez. V2.0 uç kullanması gerekip gerekmediğini belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
>
>

## <a name="protocol-diagram"></a>Protokol diyagramı
Kullanıcı bir uygulama kullanarak doğrulandı varsayın [OAuth 2.0 yetkilendirme kodu izin akışı](active-directory-v2-protocols-oauth-code.md).  Bu noktada, uygulamanın bir erişim belirteci var. *API a* (simge A) kullanıcı talepleri ve orta katmanda erişmek için izniniz web API'si (API A). Şimdi, API bir aşağı akış web API'si (API B) kimliği doğrulanmış bir istekte bulunun gerekiyor.

> [!IMPORTANT]
> Belirteçleri kullanarak edinilen [örtük grant](active-directory-v2-protocols-implicit.md) On-temsili akış için kullanılamaz.  İmplcit akışları istemcisinde (örn. bir istemci parolası) doğrulanmaz ve bu nedenle başka bir, büyük olasılıkla daha güçlü belirtece bootstrap izin verilmesi gerektiğini değil.

Adımları On-temsili akış oluşturur ve aşağıdaki diyagramda yardımıyla açıklanmıştır.

![OAuth2.0 üzerinde-temsili akış](media/active-directory-protocols-oauth-on-behalf-of/active-directory-protocols-oauth-on-behalf-of-flow.png)


1. İstemci uygulaması API A'ya A belirteciyle istekte (ile bir `aud` sayfasının API A talep).
2. API bir Azure AD belirteci verme uç noktasına kimliğini doğrular ve API B'nin erişmek için bir belirteç istekleri
3. Azure AD belirteci verme endpoint belirteci bir API A'ın kimlik bilgilerini doğrular ve API B (belirteci B) için erişim belirteci verir.
4. API b isteğinin yetkilendirme üst B belirteci ayarlayın
5. Veriler güvenli kaynaktan API B'nin tarafından döndürülür

> [!NOTE]
> Bu senaryoda, orta katman hizmet aşağı akış API erişmek için kullanıcı onayı almak için kullanıcı etkileşimi olmadan sahiptir. Bu nedenle, adım onay bir parçası olarak kimlik doğrulaması sırasında önceden aşağı akış API'sine erişim seçeneği sunulur.
>

## <a name="service-to-service-access-token-request"></a>Hizmet erişim belirteci isteği hizmetine
Bir erişim belirteci istemek için bir HTTP POST için Kiracı özgü olun Azure AD v2.0 uç aşağıdaki parametrelerle.

```
https://login.microsoftonline.com/<tenant>/oauth2/v2.0/token
```

İstemci uygulaması olup bir paylaşılan gizlilik ya da bir sertifika tarafından güvenli hale seçti bağlı olarak iki durumlar vardır.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk durumda: bir paylaşılan gizlilik ile erişim belirteci isteği
Paylaşılan gizlilik kullanırken, hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |Gerekli | Belirteç isteği türü. JWT'nin kullanarak bir istek için değer olmalıdır **urn: ietf:params:oauth:grant-türü: jwt-taşıyıcı**. |
| client_id |Gerekli | Uygulama Kimliği [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) uygulamanıza atanmış. |
| client_secret |Gerekli | Uygulama kayıt Portalı'nda, uygulamanız için oluşturulan uygulama gizli anahtarı. |
| onaylama işlemi |Gerekli | İstekte kullanılan belirteç değeri. |
| scope |Gerekli | Boşlukla ayrılmış belirteç isteği kapsamları listesi. Daha fazla bilgi için bkz: [kapsamları](active-directory-v2-scopes.md).|
| requested_token_use |Gerekli | İsteğin nasıl işleneceğini belirtir. On adına, akış değeri olmalıdır **on_behalf_of**. |

#### <a name="example"></a>Örnek
Bir erişim belirteci ve yenileme belirteciyle aşağıdaki HTTP POST istekleri `user.read` için kapsam https://graph.microsoft.com web API'si.

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
Hizmetten hizmete erişim belirteci isteği bir sertifika ile aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |Gerekli | Belirteç isteği türü. JWT'nin kullanarak bir istek için değer olmalıdır **urn: ietf:params:oauth:grant-türü: jwt-taşıyıcı**. |
| client_id |Gerekli | Uygulama Kimliği [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) uygulamanıza atanmış. |
| client_assertion_type |Gerekli |Değer olmalıdır `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Gerekli | Oluşturma ve sertifika ile imzalamak için gereken bir onaylama işlemi (bir JSON Web belirteci) uygulamanız için kimlik bilgileri olarak kayıtlı.  Hakkında bilgi edinin [sertifika kimlik bilgileri](active-directory-certificate-credentials.md) sertifikanızı ve onaylama biçimi kaydetme hakkında bilgi edinmek için.|
| onaylama işlemi |Gerekli | İstekte kullanılan belirteç değeri. |
| requested_token_use |Gerekli | İsteğin nasıl işleneceğini belirtir. On adına, akış değeri olmalıdır **on_behalf_of**. |
| scope |Gerekli | Boşlukla ayrılmış belirteç isteği kapsamları listesi. Daha fazla bilgi için bkz: [kapsamları](active-directory-v2-scopes.md).|

Client_secret parametresi tarafından iki parametre değiştirilir dışında parametreler neredeyse aynı paylaşılan gizliliği isteğiyle durumunda olduğu gibi olduğuna dikkat edin: client_assertion_type ve client_assertion.

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
Başarılı yanıt aşağıdaki parametrelerle bir JSON OAuth 2.0 yanıt olan.

| Parametre | Açıklama |
| --- | --- |
| token_type |Belirteç türü değeri gösterir. Azure AD destekler yalnızca türü **taşıyıcı**. Taşıyıcı belirteçlerini hakkında daha fazla bilgi için bkz: [OAuth 2.0 yetkilendirme Framework: taşıyıcı belirteci kullanımı (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| scope |Belirtecinde verilen erişim kapsamı. |
| expires_in |Süre (saniye cinsinden) erişim belirteci geçerlidir. |
| access_token |İstenen erişim belirteci. Arama hizmeti alıcı hizmete kimlik doğrulaması için bu belirteci kullanabilirsiniz. |
| refresh_token |İstenen erişim belirteci için yenileme belirteci. Arama hizmeti geçerli erişim belirtecinin süresi dolduktan sonra başka bir erişim belirteci istemek için bu belirteci kullanabilirsiniz. Yenileme belirteci yalnızca, sağlanan `offline_access` kapsam istendi.|

### <a name="success-response-example"></a>Başarılı yanıt örnek
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
> Yukarıdaki erişim belirteci V1 olarak biçimlendirilmiş belirteç olduğuna dikkat edin.  Erişilen kaynak tabanlı belirteç sağlanan olmasıdır.  Bir istemci için Microsoft Graph belirteçleri istediğinde Azure AD V1 erişim belirteçleri üretir şekilde Microsoft Graph V1 belirteçleri ister.  Uygulamalar erişim belirteçleri görünmelidir - istemciler, bunları inceleyin gerekmez. yalnızca. 


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
GET /v1.0/me HTTP/1.1
Host: graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQTZOVGFlN0NkV1c3UWZkSzdNN0RyNXlvUUdLNmFEc19vdDF3cEQyZjNqRkxiNlVrcm9PcXA2cXBJclAxZVV0QktzMHEza29HN3RzXzJpSkYtQjY1UV8zVGgzSnktUHZsMjkxaFNBQSIsImFsZyI6IlJTMjU2IiwieDV0IjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIiwia2lkIjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIn0.eyJhdWQiOiJodHRwczovL2dyYXBoLm1pY3Jvc29mdC5jb20iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaWF0IjoxNDkzOTMwMDE2LCJuYmYiOjE0OTM5MzAwMTYsImV4cCI6MTQ5MzkzMzg3NSwiYWNyIjoiMCIsImFpbyI6IkFTUUEyLzhEQUFBQUlzQjN5ZUljNkZ1aEhkd1YxckoxS1dlbzJPckZOUUQwN2FENTVjUVRtems9IiwiYW1yIjpbInB3ZCJdLCJhcHBfZGlzcGxheW5hbWUiOiJUb2RvRG90bmV0T2JvIiwiYXBwaWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJhcHBpZGFjciI6IjEiLCJmYW1pbHlfbmFtZSI6IkNhbnVtYWxsYSIsImdpdmVuX25hbWUiOiJOYXZ5YSIsImlwYWRkciI6IjE2Ny4yMjAuMC4xOTkiLCJuYW1lIjoiTmF2eWEgQ2FudW1hbGxhIiwib2lkIjoiZDVlOTc5YzctM2QyZC00MmFmLThmMzAtNzI3ZGQ0YzJkMzgzIiwib25wcmVtX3NpZCI6IlMtMS01LTIxLTIxMjc1MjExODQtMTYwNDAxMjkyMC0xODg3OTI3NTI3LTI2MTE4NDg0IiwicGxhdGYiOiIxNCIsInB1aWQiOiIxMDAzM0ZGRkEwNkQxN0M5Iiwic2NwIjoiVXNlci5SZWFkIiwic3ViIjoibWtMMHBiLXlpMXQ1ckRGd2JTZ1JvTWxrZE52b3UzSjNWNm84UFE3alVCRSIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInVuaXF1ZV9uYW1lIjoibmFjYW51bWFAbWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hY2FudW1hQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJzUVlVekYxdUVVS0NQS0dRTVFVRkFBIiwidmVyIjoiMS4wIn0.Hrn__RGi-HMAzYRyCqX3kBGb6OS7z7y49XPVPpwK_7rJ6nik9E4s6PNY4XkIamJYn7tphpmsHdfM9lQ1gqeeFvFGhweIACsNBWhJ9Nx4dvQnGRkqZ17KnF_wf_QLcyOrOWpUxdSD_oPKcPS-Qr5AFkjw0t7GOKLY-Xw3QLJhzeKmYuuOkmMDJDAl0eNDbH0HiCh3g189a176BfyaR0MgK8wrXI_6MTnFSVfBePqklQeLhcr50YTBfWg3Svgl6MuK_g1hOuaO-XpjUxpdv5dZ0SvI47fAuVDdpCE48igCX5VMj4KUVytDIf6T78aIXMkYHGgW3-xAmuSyYH_Fr0yVAQ
```

## <a name="next-steps"></a>Sonraki adımlar
OAuth 2.0 protokolünü ve istemci kimlik bilgilerini kullanarak hizmet kimlik doğrulama gerçekleştirmek için başka bir yöntem hakkında daha fazla bilgi edinin.
* [Azure AD v2.0 için OAuth 2.0 istemci kimlik bilgileri verin](active-directory-v2-protocols-oauth-client-creds.md)
* [Azure AD v2.0 içinde OAuth 2.0](active-directory-v2-protocols-oauth-code.md)
