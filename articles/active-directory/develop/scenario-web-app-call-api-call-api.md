---
title: Web uygulaması çağrıları API'leri (bir web API'sini çağırma) - web Microsoft kimlik platformu
description: Web API (web API'si çağırma) çağıran bir Web uygulaması oluşturmayı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
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
ms.openlocfilehash: 3624f4e859081e53ee27b6f8415eb3f9b5a2a5fa
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67785469"
---
# <a name="web-app-that-calls-web-apis---call-a-web-api"></a>Web API - çağıran bir web uygulaması web API'si çağırma

Bir belirteç olduğuna göre korumalı web API'si çağırabilirsiniz.

## <a name="aspnet-core"></a>ASP.NET Core

Basitleştirilmiş bir kod eylemini işte `HomeController`. Bu kod Microsoft Graph'i çağırmaya yönelik bir belirteç alır. REST API olarak Microsoft Graph'i çağırmaya yönelik gösteren bu zaman kod eklendi. Graph API için URL'yi sağlanan `appsettings.json` dosya ve adlı bir değişkende okuma `webOptions`:

```JSon
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    ...
  },
  ...
  "GraphApiUrl": "https://graph.microsoft.com"
}
```

```CSharp
public async Task<IActionResult> Profile()
{
 var application = BuildConfidentialClientApplication(HttpContext, HttpContext.User);
 string accountIdentifier = claimsPrincipal.GetMsalAccountId();
 string loginHint = claimsPrincipal.GetLoginHint();

 // Get the account
 IAccount account = await application.GetAccountAsync(accountIdentifier);

 // Special case for guest users as the Guest iod / tenant id are not surfaced.
 if (account == null)
 {
  var accounts = await application.GetAccountsAsync();
  account = accounts.FirstOrDefault(a => a.Username == loginHint);
 }

 AuthenticationResult result;
 result = await application.AcquireTokenSilent(new []{"user.read"}, account)
                            .ExecuteAsync();
 var accessToken = result.AccessToken;

 // Calls the web API (here the graph)
 HttpClient httpClient = new HttpClient();
 httpClient.DefaultRequestHeaders.Authorization =
     new AuthenticationHeaderValue(Constants.BearerAuthorizationScheme,accessToken);
 var response = await httpClient.GetAsync($"{webOptions.GraphApiUrl}/beta/me");

 if (response.StatusCode == HttpStatusCode.OK)
 {
  var content = await response.Content.ReadAsStringAsync();

  dynamic me = JsonConvert.DeserializeObject(content);
  return me;
 }

 ViewData["Me"] = me;
 return View();
}
```

> [!NOTE]
> Herhangi bir web API'sini çağırmak için de aynı ilke kullanabilirsiniz.
>
> Çoğu Azure web API'leri, bunu çağırma basitleştiren bir SDK'sı sağlar. Bu ayrıca Microsoft Graph'i durumudur. Sonraki makalede bu görünüşler gösteren bir öğretici nerede bulacağını öğreneceksiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretim aşamasına geçme](scenario-web-app-call-api-production.md)
