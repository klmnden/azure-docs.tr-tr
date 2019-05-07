---
title: Microsoft kimlik platformu çağrıları API'leri (üretim taşıma) - web, masaüstü uygulaması
description: Bir masaüstü uygulaması oluşturmayı öğrenin çağrıları veritabanını web API'leri (üretim taşıma)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3ca66a41f26c54bf04273682d14889a36b688c70
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075138"
---
# <a name="desktop-app-that-calls-web-apis---move-to-production"></a>Web çağıran masaüstü uygulaması API'ler - üretim ortamına taşıyın

Bu makalede, uygulamanızı daha da geliştirmek ve üretim ortamına taşımak için Ayrıntılar sunulmaktadır.

## <a name="handling-errors-in-desktop-applications"></a>Masaüstü uygulamalarındaki hataları işleme

Farklı akışlarında (kod parçacıkları gösterildiği gibi) Sessiz akışlar için hataları işlemek öğrendiniz. Ayrıca gördüğünüz gerekli (artımlı onay ve koşullu erişim) etkileşim olduğu durumlar vardır.

## <a name="how-to-have--the-user-consent-upfront-for-several-resources"></a>Kullanıcı onayı çeşitli kaynaklar için önceden sağlama

> [!NOTE]
> Microsoft kimlik platformu, ancak Azure Active Directory (Azure AD) B2C için çeşitli kaynakları Works izninizi almadan. Azure AD B2C kullanıcı onayı yalnızca yönetici onayı destekler.

Microsoft kimlik Platformu (v2.0) uç noktası için çeşitli kaynaklar tek seferde bir belirteç almak üzere izin vermez. Bu nedenle, `scopes` parametresi yalnızca tek bir kaynak için kapsamı içerir. Kullanıcı bazı kaynakları kullanarak önceden toplanmasına onay verir emin olun `extraScopesToConsent` parametresi.

Örneğin, iki kaynaklarınız varsa, iki olan her kapsamları:

- `https://mytenant.onmicrosoft.com/customerapi` -2 kapsamlı `customer.read` ve `customer.write`
- `https://mytenant.onmicrosoft.com/vendorapi` -2 kapsamlı `vendor.read` ve `vendor.write`

Kullanmanız gereken `.WithAdditionalPromptToConsent` olan değiştirici `extraScopesToConsent` parametresi.

Örneğin:

```CSharp
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

Bu çağrı ilke web API'niz için bir erişim belirteci alırsınız.

İkinci web API'sini çağırmak gerektiğinde çağırabilirsiniz:

```CSharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```

### <a name="microsoft-personal-account-requires-reconsenting-each-time-the-app-is-run"></a>Uygulama her çalıştırıldığında reconsenting kişisel Microsoft hesabı gerektirir.

Microsoft Kişisel hesapları kullanıcılar, her yerel istemci (masaüstü/mobil uygulama) çağrı korunmasına yetki vermek için onay reprompting amaçlanan bir davranış içindir. Yerel istemci kimliği, (aykırı bir gizli dizi kimliklerini kanıtlamak için Microsoft Identity platformuyla exchange gizli istemci uygulaması) kendiliğinden güvenli değil. Bu insecurity tüketici Hizmetleri için kullanıcıdan onayı, her zaman uygulama yetkisi isteyerek azaltmak Microsoft kimlik platformu seçtiniz.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
