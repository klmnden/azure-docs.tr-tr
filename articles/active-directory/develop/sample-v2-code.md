---
title: Microsoft kimlik platformu kod örnekleri | Microsoft Docs
description: Bir dizinin mevcut Microsoft kimlik Platformu (v2.0 uç noktası) kod örnekleri, senaryoya göre düzenlenmiş sağlar.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/26/2018
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 326b69bffa0cd5728b939a91cce4fab3f3a329f7
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272683"
---
# <a name="microsoft-identity-platform-code-samples-v20-endpoint"></a>Microsoft kimlik platformu kod örnekleri (v2.0 uç noktası)

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Microsoft identity platformuna kullanabilirsiniz:

- Kimlik doğrulama ve yetkilendirme, web uygulamalarınıza ekleyin ve web API'leri.
- Korumalı web API'sine erişmek için bir erişim belirteci gerektirir.

Bu makalede kısaca açıklayan ve Microsoft kimlik platformu uç nokta için örneklere bağlantılar sağlar. Bu örnekler nasıl yapılır ve ayrıca, uygulamalarınızda kullanabileceğiniz kod parçacıklarını sağlar gösterir. Kod örnek sayfasında, gereksinimleri, yükleme ve Kurulum Yardımı ayrıntılı Benioku konuları bulabilirsiniz. Kod açıklamaları kritik bölümler anlamanıza yardımcı olur.

> [!NOTE]
> V1.0 örnekleri ilgileniyorsanız bkz [Azure AD kod örneklerinin (v1.0 uç nokta)](sample-v1-code.md).

Her örnek türü için temel senaryo anlamak için bkz: [Microsoft kimlik platformu uç noktası için uygulama türleri](v2-app-types.md).

GitHub üzerindeki örneklere katkıda bulunabilir. Bilgi edinmek için bkz. nasıl [Microsoft Azure Active Directory örnekler ve belgeler](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="single-page-applications"></a>Tek sayfa uygulamaları

Bu örnekler, Microsoft kimlik platformu ile güvenli bir tek sayfalı uygulamasını yazma işlemi gösterilmektedir. Bu örnekler, MSAL.js türde birini kullanın.

| Platform | Açıklama | Bağlantı |
| -------- | --------------------- | -------- |
| ![JavaScript](media/sample-v2-code/logo_js.png) [JavaScript (msal.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | Microsoft Graph çağrıları |[javascript-graphapi-web-v2](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |
| ![JavaScript](media/sample-v2-code/logo_js.png) [JavaScript (msal.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | B2C çağırır |[javascript msal singlepageapp B2C](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) |
| ![JavaScript](media/sample-v2-code/logo_js.png) [JavaScript (msal.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | Kendi web API çağrıları |[javascript-singlepageapp-dotnet-webapi-v2](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |
| ![Angular JS](media/sample-v2-code/logo_angular.png) [JavaScript (MSAL AngularJS)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs)| Microsoft Graph çağrıları  | [MsalAngularjsDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angularjs/samples/MsalAngularjsDemoApp)
| ![Angular](media/sample-v2-code/logo_angular.png) [JavaScript (MSAL Angular)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular)| Microsoft Graph çağrıları  | [MSALAngularDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angular/samples/MSALAngularDemoApp) |

## <a name="web-applications"></a>Web uygulamaları

Aşağıdaki örnekleri kullanıcılarının oturumunu, web uygulamaları gösterir. Bazı örnekler, Microsoft Graph veya kendi kullanıcı kimliği ile web API çağırma uygulama da gösterir.

| Platform | Yalnızca kullanıcılar oturum açtığında | Kullanıcılar oturum açtığında ve Microsoft Graph çağırır |
| -------- | ------------------- | --------------------------------- |
| ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2.2 | [ASP.NET Core WebApp kullanıcılar oturum açtığında Öğreticisi](https://aka.ms/aspnetcore-webapp-sign-in) | Aynı örnek [ASP.NET Core Web uygulaması, Microsoft Graph çağırır](https://aka.ms/aspnetcore-webapp-call-msgraph) aşaması |
| ![ASP.NET](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET | [ASP.NET hızlı başlangıç](https://github.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) </p> [dotnet-webapp-openidconnect-v2](https://github.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |  [DotNet-admin-kısıtlı-kapsamları-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) </p> |[msgraph eğitim aspnetmvcapp](https://github.com/microsoftgraph/msgraph-training-aspnetmvcapp)
| ![Node.js](media/sample-v2-code/logo_nodejs.png)  |                   | [Node.js hızlı başlangıç](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs) |
| ![Ruby](media/sample-v2-code/logo_ruby.png) |                   | [msgraph eğitim rubyrailsapp](https://github.com/microsoftgraph/msgraph-training-rubyrailsapp) |

## <a name="desktop-and-mobile-public-client-apps"></a>Masaüstü ve mobil genel istemci uygulamaları

Aşağıdaki örnekler, genel istemci Microsoft Graph API'si veya kendi kullanıcı adını web API'sine erişen uygulamalar (Masaüstü ve mobil uygulamaları) gösterir. Tüm istemci uygulamaları, Microsoft kimlik doğrulama kitaplığı (MSAL) kullanın.

| İstemci uygulaması | Platform | Akış/verme | Microsoft Graph çağrıları | Bir ASP.NET Core 2.0 web API'si çağıran |
| ------------------ | -------- |  ----------| ---------- | ------------------------- |
| Masaüstü (WPF)      | ![.NET/C#](media/sample-v2-code/logo_NET.png) | [etkileşimli](msal-authentication-flows.md#interactive)| [DotNet Masaüstü msgraph v2](https://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) | [DotNet-yerel-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi) |
| Masaüstü (konsol)   | ![.NET / C# (Masaüstü)](media/sample-v2-code/logo_NET.png) | [Tümleşik Windows kimlik doğrulaması](msal-authentication-flows.md#integrated-windows-authentication) | [dotnet-iwa-v2](https://github.com/azure-samples/active-directory-dotnet-iwa-v2) |  |
| Masaüstü (konsol)   | ![.NET / C# (Masaüstü)](media/sample-v2-code/logo_NETcore.png) | [Kullanıcı Adı/Parola](msal-authentication-flows.md#usernamepassword) |[dotnetcore yukarı v2](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2) |  |
| Mobile (Android, iOS, UWP)   | ![.NET/C# (Xamarin)](media/sample-v2-code/logo_xamarin.png) | [etkileşimli](msal-authentication-flows.md#interactive) |[xamarin yerel v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) |  |
| Mobile (iOS)       | ![iOS / Objective C ya da swift'te](media/sample-v2-code/logo_iOS.png) | [etkileşimli](msal-authentication-flows.md#interactive) |[ios-swift-native-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) </p> [ios-native-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |  |
| Mobile (Android)   | ![Android / Java](media/sample-v2-code/logo_Android.png) | [etkileşimli](msal-authentication-flows.md#interactive) |  [android-native-v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |  |

## <a name="daemon-applications"></a>Arka plan programı uygulamaları

Aşağıdaki örnekler, kendi kimliğiyle (kullanıcı) ile Microsoft Graph API erişen bir uygulama gösterir.

| İstemci uygulaması | Platform | Akış/verme | Microsoft Graph çağrıları |
| ------------------ | -------- | ---------- | -------------------- |
| Konsol | ![.NET Core](media/sample-v2-code/logo_NETcore.png)</p> ASP.NET  | [İstemci kimlik bilgileri](msal-authentication-flows.md#client-credentials) | [dotnetcore arka plan programı v2](https://github.com/azure-samples/active-directory-dotnetcore-daemon-v2) |
| Web uygulaması | ![ASP.NET](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET  | [İstemci kimlik bilgileri](msal-authentication-flows.md#client-credentials) | [dotnet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) |

## <a name="headless-applications"></a>Gözetimsiz uygulamalar

Aşağıdaki örnek, bir web tarayıcısı olmadan bir cihazda çalışan bir ortak istemci uygulaması gösterir. Uygulamayı, Linux veya Mac ya da bir IOT uygulaması üzerinde çalışan bir uygulamanın bir komut satırı aracı olabilir. Örnek bir uygulama etkileşimli olarak (örneğin, bir cep telefonu) başka bir cihazda oturum açtığında bir kullanıcı adını, Microsoft Graph API'sine erişim sunar. Bu istemci uygulaması, Microsoft kimlik doğrulama kitaplığı (MSAL) kullanır.

| İstemci uygulaması | Platform | Akış/verme | Microsoft Graph çağrıları |
| ------------------ | -------- |  ----------| ---------- |
| Masaüstü (konsol)   | ![.NET / C# (Masaüstü)](media/sample-v2-code/logo_NETcore.png) | [Cihaz kod akışı](msal-authentication-flows.md#device-code) |[dotnetcore devicecodeflow v2](https://github.com/azure-samples/active-directory-dotnetcore-devicecodeflow-v2) |

## <a name="web-apis"></a>Web API'leri

Aşağıdaki örnekler, Microsoft kimlik platformu uç noktası ile bir web API'sini korumak nasıl ve bir aşağı akış API web API'sini çağırmak nasıl gösterir.

| Platform | Örnek |
| -------- | ------------------- |
| ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2.2 | ASP.NET Core web API'si (hizmet) [dotnet-yerel-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi-calls-msgraph)  |
| ![ASP.NET](media/sample-v2-code/logo_NET.png)</p>ASP.NET MVC | Web API'si (hizmet) [ms-kimlik-ASP.NET-webapı-onbehalfof](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof) |

## <a name="other-microsoft-graph-samples"></a>Diğer Microsoft Graph örnekleri

Hakkında bilgi edinmek için [örnekleri](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) ve Azure AD ile kimlik doğrulaması dahil olmak üzere Microsoft Graph API için farklı kullanım düzenlerinin gösteren öğreticileri [Microsoft Graph topluluk örnekler ve öğreticiler](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Active Directory (v1.0) Geliştirici Kılavuzu](v1-overview.md)
- [Azure AD Graph API kavramsal ve başvuru](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Azure AD Graph API Yardımcısı kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
