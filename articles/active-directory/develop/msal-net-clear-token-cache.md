---
title: .NET - Azure için Microsoft kimlik doğrulama kitaplığı kullanarak belirteç Önbelleği Temizle
description: Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanarak belirteç Önbelleği Temizle öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6763c6b2b1f9b4de7d8669a50a4979a7aac00c7
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544111"
---
# <a name="clear-the-token-cache-using-msalnet"></a>MSAL.NET kullanarak belirteç Önbelleği Temizle

Olduğunda, [erişim belirteci alma](msal-acquire-cache-tokens.md) Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanarak, belirteç önbelleğe alınır. Uygulamasının bir belirteç gerektiğinde, ilk çağırmalıdır `AcquireTokenSilent` kabul edilebilir bir belirteci önbellekte olup olmadığını doğrulamak için yöntem. 

Önbelleği temizleme, önbellekten hesapları kaldırarak elde edilir. Bu, tarayıcıda, ancak oturum tanımlama bilgisinin kaldırmaz.  Aşağıdaki örnek genel istemci uygulamasının örneğini oluşturan, uygulama için hesapları alır ve hesaplarını kaldırır.

```csharp
private readonly IPublicClientApplication _app;
private static readonly string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
private static readonly string Authority = string.Format(CultureInfo.InvariantCulture, AadInstance, Tenant);

_app = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(Authority)
                .Build();

var accounts = (await _app.GetAccountsAsync()).ToList();

// clear the cache
while (accounts.Any())
{
   await _app.RemoveAsync(accounts.First());
   accounts = (await _app.GetAccountsAsync()).ToList();
}

```

Ve hakkında daha fazla alınırken belirteçleri önbelleğe almayı öğrenmek için [erişim belirteci alma](msal-acquire-cache-tokens.md).
