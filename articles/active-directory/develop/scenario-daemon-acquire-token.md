---
title: Arka plan programı uygulama arama web API'lerini (uygulama belirteçlerini almak) - Microsoft kimlik platformu
description: Daemon uygulamasının nasıl oluşturulacağını öğrenin çağrıları (belirteçlerini almak) API'leri web
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: aa4f5dc7a5aceaf81f71eacd36d131471a57e5c0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65075378"
---
# <a name="daemon-app-that-calls-web-apis---acquire-a-token"></a>Web API'leri - çağıran daemon uygulamasının bir belirteç Al

Gizli bir istemci uygulama oluşturulur, sonra çağırarak uygulama için bir belirteç elde edebilir ``AcquireTokenForClient``, kapsam ve zorlama veya yenileme belirtecinin değil geçirme.

## <a name="scopes-to-request"></a>İstek için kapsamları

Bir istemci kimlik bilgisi akış kaynağının adı için istek kapsamı ve ardından `/.default`. Azure AD kullanmak için bu gösterimi bildirir **uygulama düzeyinde izinler** uygulama kaydı sırasında statik olarak bildirilir. Ayrıca, daha önce görüldüğü gibi bu API izinleri Kiracı Yöneticisi tarafından verilmesi gerekir

### <a name="net"></a>.NET

```CSharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

### <a name="python"></a>Python

MSAL içinde. Yapılandırma dosyası, Python, aşağıdaki kod parçacığı gibi görünür:

```Python
{
    "authority": "https://login.microsoftonline.com/organizations",
    "client_id": "your_client_id",
    "secret": "This is a sample only. You better NOT persist your password."
    "scope": ["https://graph.microsoft.com/.default"]
}
```

### <a name="java"></a>Java

```Java
public final static String KEYVAULT_DEFAULT_SCOPE = "https://vault.azure.net/.default";
```

### <a name="all"></a>Tümü

İstemci kimlik bilgileri için kullanılan kapsam her zaman ResourceId + olmalıdır "/ .default"

### <a name="case-of-v10-resources"></a>V1.0 kaynak durumu

> [!IMPORTANT]
> V1.0 erişim belirteci kabul eden bir kaynak için bir erişim belirteci isteyen MSAL için (v2.0 uç noktası), Azure AD'ye son eğik çizgiden önce her şeyi alma ve kaynak tanımlayıcısı kullanılarak istenen hedef istenen kapsamından ayrıştırır.
> Bu nedenle IF, Azure SQL gibi ( **https://database.windows.net** ) eğik çizgi ile biten bir hedef kitle kaynak bekliyor (Azure SQL: `https://database.windows.net/` ), istek bir kapsam gerekir `https://database.windows.net//.default` (çift eğik çizgi unutmayın). Ayrıca, MSAL.NET bkz sorunu [#747](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747): Sql kimlik doğrulama hatasına neden olan kaynak URL'nin sonunda eğik çizgi atlanır.

## <a name="acquiretokenforclient-api"></a>AcquireTokenForClient API

### <a name="net"></a>.NET

```CSharp
using Microsoft.Identity.Client;

// With client credentials flows the scopes is ALWAYS of the shape "resource/.default", as the
// application permissions need to be set statically (in the portal or by PowerShell), and then granted by
// a tenant administrator
string[] scopes = new string[] { "https://graph.microsoft.com/.default" };

AuthenticationResult result = null;
try
{
 result = await app.AcquireTokenForClient(scopes)
                  .ExecuteAsync();
}
catch (MsalUiRequiredException ex)
{
    // The application does not have sufficient permissions
    // - did you declare enough app permissions in during the app creation?
    // - did the tenant admin needs to grant permissions to the application.
}
catch (MsalServiceException ex) when (ex.Message.Contains("AADSTS70011"))
{
    // Invalid scope. The scope has to be of the form "https://resourceurl/.default"
    // Mitigation: change the scope to be as expected !
}
```

#### <a name="application-token-cache"></a>Uygulama belirteci önbelleği

İçinde MSAL.NET `AcquireTokenForClient` kullanan **uygulama belirteç önbelleği** (tüm diğer AcquireTokenXX yöntemler kullanıcı belirteç önbelleği kullanın) Remove() çağırmayın `AcquireTokenSilent` çağırmadan önce `AcquireTokenForClient` olarak `AcquireTokenSilent` kullanır**kullanıcı** belirteç önbelleği. `AcquireTokenForClient` denetler **uygulama** belirteci kendisini önbellek ve onu güncelleştirir.

### <a name="python"></a>Python

```Python
# Firstly, looks up a token from cache
# Since we are looking for token for the current app, NOT for an end user,
# notice we give account parameter as None.
result = app.acquire_token_silent(config["scope"], account=None)

if not result:
    logging.info("No suitable token exists in cache. Let's get a new one from AAD.")
    result = app.acquire_token_for_client(scopes=config["scope"])
```

### <a name="java"></a>Java

```Java
ClientCredentialParameters parameters = ClientCredentialParameters
        .builder(Collections.singleton(KEYVAULT_DEFAULT_SCOPE))
        .build();

CompletableFuture<AuthenticationResult> future = cca.acquireToken(parameters);

// You can complete the future in many different ways. Here we use .get() for simplicity
AuthenticationResult result = future.get();
```

### <a name="protocol"></a>Protocol

Bir kitaplık için istediğiniz dilde Protokolü doğrudan kullanmak isteyebilirsiniz henüz sahip değilseniz varsa:

#### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk Durum: Paylaşılan gizlilik ile erişim belirteci isteği

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1           //Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_secret=qWgdYAmab0YSkuL1qKv5bPX
&grant_type=client_credentials
```

#### <a name="second-case-access-token-request-with-a-certificate"></a>İkinci durum: Bir sertifika ile erişim belirteci isteği

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1               // Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&grant_type=client_credentials
```

### <a name="learn-more-about-the-protocol"></a>Protokolü hakkında daha fazla bilgi edinin

Daha fazla bilgi için protokol belgelerine bakın: [Azure Active Directory v2.0 ve OAuth 2.0 istemci kimlik bilgileri akışı](v2-oauth2-client-creds-grant-flow.md).

## <a name="troubleshooting"></a>Sorun giderme

### <a name="did-you-use-the-resourcedefault-scope"></a>Kaynak/.default kapsamı kullandınız mı?

Geçersiz bir kapsam kullanılan, büyük olasılıkla kullanmadığını belirten bir hata iletisi alırsanız `resource/.default` kapsam.

### <a name="did-you-forget-to-provide-admin-consent-daemon-apps-need-it"></a>Yönetici onayı sağlamak unuttunuz mu? Arka plan programları ihtiyaç!

API çağrılırken bir hata alırsanız **işlemi tamamlamak için yeterli ayrıcalık**, Kiracı Yöneticisi uygulamaya izinleri gerekiyor. 6\. adım kaydının yukarıdaki istemci uygulaması bakın.
Genellikle görürsünüz ve hata gibi aşağıdaki hata açıklaması:

```JSon
Failed to call the web API: Forbidden
Content: {
  "error": {
    "code": "Authorization_RequestDenied",
    "message": "Insufficient privileges to complete the operation.",
    "innerError": {
      "request-id": "<guid>",
      "date": "<date>"
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Arka plan programı app - bir web API'si çağırma](scenario-daemon-call-api.md)