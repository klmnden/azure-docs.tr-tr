---
title: Çeşitli kaynaklar (Microsoft kimlik doğrulama kitaplığı .NET için) için onay alın | Azure
description: Bir kullanıcı için Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanarak çeşitli kaynaklar öncesi onayı nasıl edinebildiğini öğrenin.
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
ms.openlocfilehash: 8f7d24a1e14cfbb1163ab78b94dd36ec288dce50
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544043"
---
# <a name="user-gets-consent-for-several-resources-using-msalnet"></a>Kullanıcı onayı MSAL.NET kullanarak çeşitli kaynaklar için alır.
Microsoft kimlik platformu uç noktası için çeşitli kaynaklar tek seferde bir belirteç almak üzere izin vermez. Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanırken, kapsamları parametreyi alma belirteci yöntemi yalnızca tek bir kaynak için kapsamları içermelidir. Ancak, ön maliyet bazı kaynakları kullanarak ek kapsamlarla belirterek önceden onaylayabileceğini `.WithExtraScopeToConsent` Oluşturucu yöntemi.

> [!NOTE]
> Microsoft kimlik platformu, ancak Azure AD B2C için çeşitli kaynakları Works izninizi almadan. Azure AD B2C kullanıcı onayı yalnızca yönetici onayı destekler.

Örneğin, sahip iki kaynaklarınız varsa 2 her kapsamları:

- https://mytenant.onmicrosoft.com/customerapi (2 kapsamlı `customer.read` ve `customer.write`)
- https://mytenant.onmicrosoft.com/vendorapi (2 kapsamlı `vendor.read` ve `vendor.write`)

Kullanmanız gereken `.WithExtraScopeToConsent` olan değiştirici *extraScopesToConsent* aşağıdaki örnekte gösterildiği gibi parametre:

```csharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.onmicrosoft.com/customerapi/customer.read",
  "https://mytenant.onmicrosoft.com/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

Bu ilk web API'si için bir erişim belirteci alırsınız. Ardından, ikinci bir web API'sine erişim gerektiğinde belirteç önbellekten belirteç sessizce elde edebilirsiniz:

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```
