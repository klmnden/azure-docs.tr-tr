---
title: Azure Active Directory kod örnekleri | Microsoft Docs
description: Kullanılabilir Azure Active Directory (V2 uç noktası) dizini senaryo tarafından düzenlenen kod örnekleri sağlar.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/26/2018
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 39c2c5adef72db830ef226228017065c1b70f496
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-active-directory-code-samples-v2-endpoint"></a>Azure Active Directory kod örnekleri (V2 uç noktası)

Microsoft Azure Active Directory (Azure AD) için kullanabilirsiniz:

- API web ve kimlik doğrulama ve yetkilendirme, web uygulamalarınızı ekleyin.
- Korumalı web API erişmek için bir erişim belirteci gerektirir.

Bu makalede kısaca açıklayan ve Azure AD V2 uç noktası için örnekleri için bağlantılar sağlar. Bu örnekler nasıl, uygulamalarınızda kullanabileceğiniz kod parçacıkları birlikte yapıldığını gösterir. Kod örnek sayfada gereksinimleri, yükleme, Yardım ve ayarlama ayrıntılı Benioku konuları bulabilirsiniz. Kritik bölümler anlamanıza yardımcı olması için kodundaki açıklamaları vardır.

> [!NOTE]
> V1 örneklerinde ilgileniyorsanız bkz [Azure AD kod örnekleri (V1 uç noktası)](active-directory-code-samples.md).

Her bir örnek türü için temel senaryo anlamak için bkz: [uygulama türleri için Azure Active Directory v2.0 uç](active-directory-v2-flows.md).

Ayrıca örnekler github'da katkıda bulunabilirsiniz. Bilgi edinmek için bkz [Microsoft Azure Active Directory örnekler ve belgeler](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="desktop-and-mobile-public-client-apps"></a>Masaüstü ve mobil ortak istemci uygulamaları

Aşağıdaki örnekler ortak istemci Microsoft Graph veya bir kullanıcı adına bir Web API erişim uygulamaları (masaüstü/mobil uygulamaları) gösterir.

istemci uygulaması | Platform | Akış/verin | Çağrıları Microsoft Graph | Bir ASP.NET Core 2.0 Web API'si çağıran
------------------ | -------- | ---------- | -------------------- | -------------------------
Masaüstü (WPF)      | .NET / C#  | Etkileşimli | [DotNet Masaüstü msgraph v2](http://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) <p/> [DotNet-admin-kısıtlı-kapsamları-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) | [DotNet yerel aspnetcore v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)
Mobil (UWP)   | .NET / C# (UWP) | Etkileşimli | [DotNet yerel uwp v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) |
Mobil (Android, iOS, UWP)   | .NET / C# (Xamarin) | Etkileşimli | [xamarin yerel v2](https://Github.com/azure-samples/active-directory-xamarin-native-v2) |
Mobil (iOS)       | iOS / Objective C ya da swift'te | Etkileşimli | [iOS-swift-yerel-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) <p/> [iOS-yerel-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |
Mobil (Android)   | Android / Java | Etkileşimli |   [Android yerel v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |

## <a name="web-applications"></a>Web uygulamaları

Aşağıdaki örnekler, kullanıcılar oturum, Microsoft Graph çağıran veya kullanıcının kimliği ile web API çağırma web uygulamaları gösterilmektedir.

 Platform | Yalnızca kullanıcılar işaretleri | Kullanıcılar oturum açtığında ve Microsoft Graph çağırır 
 -------- | ------------------- | --------------------------------- 
ASP.NET 4.x | [webapp Openıdconnect dotNet appmodelv2](https://GitHub.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) <p/> [webapp openıdconnect v2 dotnet](https://GitHub.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |              [ASPNET bağlanmak rest örnek](https://github.com/microsoftgraph/aspnet-connect-rest-sample)   
ASP.NET Core 2.0 | [webapp openıdconnect v2 aspnetcore](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) |              [aspnetcore bağlanmak örnek](https://github.com/microsoftgraph/aspnetcore-connect-sample)   
Node.js      |                   | [WebApp Openıdconnect nodejs AppModelv2](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs)     
Ruby      |                   | [Ruby bağlanmak rest örnek](https://github.com/microsoftgraph/ruby-connect-rest-sample)     

## <a name="daemon-applications"></a>Arka plan programı uygulamaları

Aşağıdaki örnekler, Microsoft Graph veya uygulama kimliği (kullanıcı) ile web API erişim Masaüstü veya web uygulamaları gösterir.

istemci uygulaması | Platform | Akış/verin | Çağrıları Microsoft Graph 
------------------ | -------- | ---------- | -------------------- 
Web uygulaması | .NET / C#  | İstemci kimlik bilgileri | [DotNet arka plan programı v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) 

## <a name="single-page-applications-spa"></a>Tek sayfalı uygulama (SPA)

Bu örnek, Azure AD ile güvenli bir tek sayfa uygulamasının nasıl yazılacağını göstermektedir.

 Platform |  Çağrıları Microsoft Graph 
 -------- |  --------------------- 
JavaScript (msal.js)  | [JavaScript graphapi v2](https://github.com/azure-samples/active-directory-javascript-graphapi-v2) 
JavaScript (msal.js + AngularJS) | [Açısal-connect-rest-sample](https://github.com/microsoftgraph/angular-connect-rest-sample) 
JavaScript (Hello.JS)  | [JavaScript-graphapi-web-v2](https://github.com/azure-samples/active-directory-javascript-graphapi-web-v2) 
JavaScript (hello.js + Açısal 4) | [angular4 bağlanmak örnek](https://github.com/microsoftgraph/angular4-connect-sample) 

## <a name="web-apis"></a>Web API'leri

### <a name="web-api-protected-by-azure-ad"></a>Web API Azure AD tarafından korunan

Aşağıdaki örnek, Azure AD V2 uç noktası ile web API korumak gösterilmektedir.

Platform | Örnek | Açıklama
 -------- | ------------------- | ---------------------
Node.js | [DotNet yerel aspnetcore v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2) | ASP.NET Core Web API kullanarak Azure AD V2 bir WPF uygulamasında çağırır.

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>Web API Microsoft Graph veya başka bir Web API çağırma

Bu örnek henüz kullanılabilir değil.

## <a name="other-microsoft-graph-samples"></a>Diğer Microsoft Graph örnekleri

Hakkında bilgi edinmek için [örnekleri](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) ve Azure AD ile kimlik doğrulaması dahil Microsoft Graph API için farklı kullanım desenlerini göstermek öğreticilere bakın [Microsoft Graph topluluğu örnekleri & öğreticiler](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Ayrıca bkz.

[Azure Active Directory geliştirici kılavuzu](active-directory-developers-guide.md)

[Azure AD Graph API kavramsal ve başvurusu](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API Yardımcısı kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
