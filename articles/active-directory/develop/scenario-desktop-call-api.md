---
title: Masaüstü uygulaması çağrıları API'leri (bir web API'sini çağırma) - web Microsoft kimlik platformu
description: Web API (web API'si çağırma) çağıran bir masaüstü uygulaması oluşturmayı öğrenin
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
ms.openlocfilehash: 4abaf234d3b216e0f67501e5d2f2f5c3f874c5d7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111237"
---
# <a name="desktop-app-that-calls-web-apis---call-a-web-api"></a>Web API - çağıran masaüstü uygulaması web API'si çağırma

Bir belirteç olduğuna göre korumalı web API'si çağırabilirsiniz.

## <a name="calling-a-web-api-from-net"></a>.NET web API'si çağırma

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->

## <a name="calling-several-apis---incremental-consent-and-conditional-access"></a>Çeşitli API'ler - artımlı rızası ve koşullu erişim çağırma

İlk API için bir belirteç alındı sonra aynı kullanıcı için çeşitli API'leri çağırmak gerekiyorsa, yalnızca çağırabilirsiniz `AcquireTokenSilent`, bir belirteç için diğer API'ler saati sessizce çoğu elde edebilirsiniz.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

Etkileşim gerekli olduğu durumda olduğunda:

- Kullanıcı için ilk API tarafından onaylanan, ancak artık daha fazla kapsam (artımlı onayı) için onay gerekiyor
- İlk API birden çok öğeli kimlik doğrulama gerektiren olmadı, ancak bir sonraki yapar.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

try
{
 result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
 result = await app.AcquireTokenInteractive("scopeApi2")
                  .WithClaims(ex.Claims)
                  .ExecuteAsync();
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretim aşamasına geçme](scenario-desktop-production.md)
