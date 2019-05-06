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
ms.openlocfilehash: 6fb240b54c3d698ead9d3427f603acca2b53745a
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075933"
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
