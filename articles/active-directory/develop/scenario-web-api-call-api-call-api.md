---
title: Web API'si çağıran web API'lerini (arama API'leri) - Microsoft kimlik platformu
description: Web API'si, çağrıları Aşağı Akış web API'leri (bir web API'sini çağırma) oluşturmayı öğrenin.
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
ms.openlocfilehash: 1cd093cc68a21558dc326221eeaa8c034c24f1c2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65074733"
---
# <a name="web-api-that-calls-web-apis---call-an-api"></a>Web API - çağıran web API'si bir API çağırma

Bir belirteci aldıktan sonra korumalı web API'si çağırabilirsiniz. Bu ASP.NET/ASP.NET Core Web API denetleyicisi gerçekleştirilir.

## <a name="controller-code"></a>Denetleyici kodlarının

Gösterilen örnek kodun devamlılık işte [korumalı web API çağrıları web API'lerini - belirteç alınırken bir](scenario-web-api-call-api-acquire-token.md)adlı APİ'si denetleyicilerinin eylemleri (todolist adlı) bir aşağı akış API çağırma.

Belirteç aldığınız sonra aşağı akış API'sini çağırmak için bir taşıyıcı belirteç kullanın.

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

// Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
_httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the To Do list service.
HttpResponseMessage response = await _httpClient.GetAsync(TodoListBaseAddress + "/api/todolist");
...
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretim aşamasına geçme](scenario-web-api-call-api-production.md)
