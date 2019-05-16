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
ms.openlocfilehash: 234c9d0724021017ec8c411d637420b05284ea52
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544168"
---
# <a name="providing-your-own-httpclient-and-proxy-using-msalnet"></a>Kendi HttpClient ve proxy MSAL.NET kullanarak sağlama
Genel istemci uygulaması başlatılırken kullanabileceğiniz `.WithHttpClientFactory method` kendi HttpClient sağlamak için.  Kendi HttpClient sağlayan gelişmiş senaryoları, kullanıcı aracısı üstbilgilerini özelleştirme veya belirli bir HttpClient (Örneğin ASP.NET Core web apps/API'leri) kullanmak için MSAL zorlama bir HTTP proxy'sinin gibi ayrıntılı denetim olanağı sağlar.

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