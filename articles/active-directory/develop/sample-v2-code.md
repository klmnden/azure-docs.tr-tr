---
title: Azure Active Directory kod örnekleri | Microsoft Docs
description: Kullanılabilir Azure Active Directory (V2 uç noktası) dizinini senaryoya göre düzenlenen, kod örnekleri sağlar.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/26/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 8d2ce5bb2f44d9e21fb12e96f9c68d4ecc0fc501
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39581873"
---
# <a name="azure-active-directory-code-samples-v2-endpoint"></a>Azure Active Directory kod örnekleri (V2 uç noktası)

Microsoft Azure Active Directory (Azure AD) için kullanabilirsiniz:

- Kimlik doğrulama ve yetkilendirme, web uygulamalarınıza ekleyin ve web API'leri.
- Korumalı web API'sine erişmek için bir erişim belirteci gerektirir.

Bu makalede kısaca açıklayan ve Azure AD V2 uç nokta için örneklere bağlantılar sağlar. Bu örnekler nasıl, uygulamalarınızda kullanabileceğiniz kod parçacıkları birlikte yapıldığını gösterir. Kod örnek sayfasında yardımcı gereksinimleri, yükleme ve ayarlama ayrıntılı Benioku konuları bulabilirsiniz. Kritik bölümler anlamanıza yardımcı olması için kod açıklamaları vardır.

> [!NOTE]
> V1 örnekleri ilgileniyorsanız bkz [Azure AD kod örneklerinin (V1 uç noktası)](sample-v1-code.md).

Her örnek türü için temel senaryo anlamak için bkz: [uygulama türleri için Azure Active Directory v2.0 uç noktası](active-directory-v2-flows.md).

GitHub üzerindeki örneklere katkıda bulunabilir. Bilgi edinmek için bkz. nasıl [Microsoft Azure Active Directory örnekler ve belgeler](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="desktop-and-mobile-public-client-apps"></a>Masaüstü ve mobil genel istemci uygulamaları

Aşağıdaki örnekler, genel istemci Microsoft Graph veya bir kullanıcı adını Web API'si erişen uygulamalar (masaüstü/mobil uygulamalar) gösterir.

İstemci uygulaması | Platform | Akış/verme | Microsoft Graph çağrıları | Bir ASP.NET Core 2.0 Web API'si çağıran
------------------ | -------- | ---------- | -------------------- | -------------------------
Masaüstü (WPF)      | .NET / C#  | Etkileşimli | [DotNet Masaüstü msgraph v2](http://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) <p/> [DotNet-admin-kısıtlı-kapsamları-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) | [DotNet-yerel-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)
Mobile (UWP)   | .NET / C# (UWP) | Etkileşimli | [DotNet-yerel-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) |
Mobile (Android, iOS, UWP)   | .NET / C# (Xamarin) | Etkileşimli | [xamarin yerel v2](https://Github.com/azure-samples/active-directory-xamarin-native-v2) |
Mobile (iOS)       | iOS / Objective C ya da swift'te | Etkileşimli | [iOS swift yerel v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) <p/> [iOS-yerel-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |
Mobile (Android)   | Android / Java | Etkileşimli |   [Android yerel v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |

## <a name="web-applications"></a>Web uygulamaları

Aşağıdaki örnekleri kullanıcılarının oturumunu, Microsoft Graph'i çağırmaya veya kullanıcının kimliği ile bir web API'si çağırma web uygulamaları gösterir.

 Platform | Yalnızca kullanıcılar oturum açtığında | Kullanıcılar oturum açtığında ve Microsoft Graph çağırır 
 -------- | ------------------- | --------------------------------- 
ASP.NET 4.x | [webapp Openıdconnect dotNet appmodelv2](https://GitHub.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) <p/> [DotNet webapp openıdconnect v2](https://GitHub.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |              [ASP.NET bağlanmak rest örneği](https://github.com/microsoftgraph/aspnet-connect-rest-sample)   
ASP.NET Core 2.0 sürümüne | [aspnetcore webapp openıdconnect v2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) |              [aspnetcore-connect-sample](https://github.com/microsoftgraph/aspnetcore-connect-sample)   
Node.js      |                   | [WebApp Openıdconnect nodejs AppModelv2](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs)     
Ruby      |                   | [Ruby bağlanmak rest örneği](https://github.com/microsoftgraph/ruby-connect-rest-sample)     

## <a name="daemon-applications"></a>Arka plan programı uygulamaları

Aşağıdaki örnekler, Microsoft Graph veya uygulama kimliği (kullanıcı) ile bir web API'si erişim Masaüstü ve web uygulamaları gösterir.

İstemci uygulaması | Platform | Akış/verme | Microsoft Graph çağrıları 
------------------ | -------- | ---------- | -------------------- 
Web uygulaması | .NET / C#  | İstemci kimlik bilgileri | [DotNet arka plan programı v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) 

## <a name="single-page-applications-spa"></a>Tek sayfalı uygulama (SPA)

Bu örnek, Azure AD ile güvenli hale getirilmiş bir tek sayfalı uygulama yazma işlemi gösterilmektedir.

 Platform |  Microsoft Graph çağrıları 
 -------- |  --------------------- 
JavaScript (msal.js)  | [JavaScript graphapi v2](https://github.com/azure-samples/active-directory-javascript-graphapi-v2) 
JavaScript (msal.js + AngularJS) | [angular-connect-rest-sample](https://github.com/microsoftgraph/angular-connect-rest-sample) 
JavaScript (Hello.JS)  | [JavaScript-graphapi-web-v2](https://github.com/azure-samples/active-directory-javascript-graphapi-web-v2) 
JavaScript (hello.js + Angular 4) | [angular4 bağlanma örneği](https://github.com/microsoftgraph/angular4-connect-sample) 

## <a name="web-apis"></a>Web API'leri

### <a name="web-api-protected-by-azure-ad"></a>Azure AD tarafından korunan Web API'sini

Aşağıdaki örnek, Azure AD V2 uç noktası ile bir web API'sini korumak nasıl gösterir.

Platform | Örnek | Açıklama
 -------- | ------------------- | ---------------------
Node.js | [DotNet-yerel-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2) | Azure AD V2 kullanarak bir WPF uygulamasından ASP.NET Core Web API'si çağırır.

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>Web API'sini Microsoft Graph veya başka bir Web API'si çağırma

Bu örnek henüz kullanılamıyor.

## <a name="other-microsoft-graph-samples"></a>Diğer Microsoft Graph örnekleri

Hakkında bilgi edinmek için [örnekleri](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) ve Azure AD ile kimlik doğrulaması dahil olmak üzere Microsoft Graph API için farklı kullanım düzenlerinin gösteren öğreticileri [Microsoft Graph topluluk örnekler ve öğreticiler](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Ayrıca bkz.

[Azure Active Directory geliştirici kılavuzu](azure-ad-developers-guide.md)

[Azure AD Graph API kavramsal ve başvuru](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API Yardımcısı kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
