---
title: Belirteç sessizce almak (Microsoft kimlik doğrulama kitaplığı .NET için) | Azure
description: Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanılarak erişim belirteci sessizce (önbellekten belirteç) alma hakkında bilgi edinin.
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
ms.date: 04/30/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6331407067a39550d866d7c293a92fac9184b54e
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544230"
---
# <a name="get-a-token-from-the-token-cache-using-msalnet"></a>Bir belirteç, MSAL.NET kullanarak belirteç önbellekten alma

Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanarak bir erişim belirteci aldığınızda, belirteç önbelleğe alınır. Uygulamasının bir belirteç gerektiğinde, ilk çağırmalıdır `AcquireTokenSilent` kabul edilebilir bir belirteci önbellekte olup olmadığını doğrulamak için yöntem. Çoğu durumda, bir belirteç önbelleğe göre daha fazla kapsamlarla başka bir belirteç almak mümkündür. (Belirteç önbelleği yenileme belirteci içerecek şekilde) süresi doluyor bir belirteç yenilemeye mümkündür.

Önerilen Düzen çağırmaktır `AcquireTokenSilent` yöntemi ilk.  Varsa `AcquireTokenSilent` başarısız sonra diğer yöntemleri kullanarak bir belirteç edinme.

Aşağıdaki örnekte, uygulama belirteç önbellekten belirteç almak ilk önce çalışır.  Varsa bir `MsalUiRequiredException` özel durum oluştu, uygulamanın etkileşimli olarak bir belirteç alır. 

```csharp
AuthenticationResult result = null;
var accounts = await app.GetAccountsAsync();

try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
        .ExecuteAsync();
}
catch (MsalUiRequiredException ex)
{
 // A MsalUiRequiredException happened on AcquireTokenSilentAsync.
 // This indicates you need to call AcquireTokenAsync to acquire a token
 System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

 try
 {
    result = await app.AcquireTokenInteractive(scopes)
          .ExecuteAsync();
 }
 catch (MsalException msalex)
 {
    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
 }
}
catch (Exception ex)
{
 ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
 return;
}

if (result != null)
{
 string accessToken = result.AccessToken;
 // Use the token
}
```