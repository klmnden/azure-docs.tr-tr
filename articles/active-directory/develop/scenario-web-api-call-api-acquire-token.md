---
title: Web API çağrıları diğer API'ler (uygulama için bir belirteç almak) - Microsoft kimlik platformu web
description: Web aramaları diğer API'ler (uygulama için bir belirteç alınırken) web API'si oluşturmayı öğrenin.
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
ms.openlocfilehash: 986e2e0f8a481d61dc870af2548290658b44d2d3
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65231103"
---
# <a name="web-api-that-calls-web-apis---acquire-a-token-for-the-app"></a>Web API - çağıran bir web API uygulaması için bir belirteç Al

Bir istemci uygulaması oluşturduktan sonra nesne, bir web API'sini çağırmak için kullanabileceğiniz bir belirteç almak için kullanın.

## <a name="code-in-the-controller"></a>Denetleyicide kod

Eylemler API denetleyicilerinin (todolist adlı) bir aşağı akış API'ye çağrı yapma, çağrılacak koduna ilişkin bir örnek aşağıda verilmiştir.

```CSharp
private async Task GetTodoList(bool isAppStarting)
{
 ...
 //
 // Get an access token to call the To Do service.
 //
 AuthenticationResult result = null;
 try
 {
  app = BuildConfidentialClient(HttpContext, HttpContext.User);
  result = await app.AcquireTokenSilent(Scopes, account)
                     .ExecuteAsync()
                     .ConfigureAwait(false);
 }
...
}
```

`BuildConfidentialClient()` makalesinde ne gördünüz için benzer [çağıran bir Web API'si web API'lerini - uygulama yapılandırma](scenario-web-api-call-api-app-configuration.md). `BuildConfidentialClient()` başlatır `IConfidentialClientApplication` yalnızca bir hesap bilgilerini içeren bir önbellek. Hesap tarafından sağlanan `GetAccountIdentifier` yöntemi.

`GetAccountIdentifier` Yöntemini kullanan web API'si için JWT alınan kullanıcı kimliğiyle ilişkili talep:

```CSharp
public static string GetMsalAccountId(this ClaimsPrincipal claimsPrincipal)
{
 string userObjectId = GetObjectId(claimsPrincipal);
 string tenantId = GetTenantId(claimsPrincipal);

 if (    !string.IsNullOrWhiteSpace(userObjectId)
      && !string.IsNullOrWhiteSpace(tenantId))
 {
  return $"{userObjectId}.{tenantId}";
 }

 return null;
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API'si çağırma](scenario-web-api-call-api-call-api.md)
