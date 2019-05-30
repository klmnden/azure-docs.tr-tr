---
title: Bir HttpClient ve proxy (MSAL.NET) sağlayın | Azure
description: Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanarak Azure AD'ye bağlanmak için kendi HttpClient ve proxy sağlama hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/23/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 72ab8a85ecc5649352382469e09d7dfd83a5ddfa
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66305718"
---
# <a name="providing-your-own-httpclient-and-proxy-using-msalnet"></a>Kendi HttpClient ve proxy MSAL.NET kullanarak sağlama
Zaman [genel istemci uygulaması başlatılırken](msal-net-initializing-client-applications.md), kullanabileceğiniz `.WithHttpClientFactory method` kendi HttpClient sağlamak için.  Kendi HttpClient sağlayan gelişmiş senaryoları, kullanıcı aracısı üstbilgilerini özelleştirme veya belirli bir HttpClient (Örneğin ASP.NET Core web apps/API'leri) kullanmak için MSAL zorlama bir HTTP proxy'sinin gibi ayrıntılı denetim olanağı sağlar.

## <a name="initialize-with-httpclientfactory"></a>HttpClientFactory ile başlatılamıyor
Oluşturmak için aşağıdaki örnekte bir `HttpClientFactory` ve ardından bir ortak istemci uygulaması ile başlatma:

```csharp
IMsalHttpClientFactory httpClientFactory = new MyHttpClientFactory();

var pca = PublicClientApplicationBuilder.Create(MsalTestConstants.ClientId) 
                                        .WithHttpClientFactory(httpClientFactory)
                                        .Build();
```

## <a name="httpclient-and-xamarin-ios"></a>HttpClient ve Xamarin iOS
Xamarin iOS kullanırken oluşturmanız önerilir bir `HttpClient` açıkça kullanan `NSURLSession`-iOS 7 ve üzeri için işleyici temel. MSAL.NET otomatik olarak oluşturur bir `HttpClient` kullanan `NSURLSessionHandler` iOS 7 ve daha yeni. Daha fazla bilgi için okuma [HttpClient için Xamarin iOS belgeleri](/xamarin/cross-platform/macios/http-stack).