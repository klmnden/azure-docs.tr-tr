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
ms.date: 10/17/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 73edb518a7e0c89b3ef510c6e528b6fa76f8dca5
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49393430"
---
# <a name="azure-active-directory-code-samples-v2-endpoint"></a>Azure Active Directory kod örnekleri (V2 uç noktası)

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Microsoft Azure Active Directory (Azure AD) için kullanabilirsiniz:

- Kimlik doğrulama ve yetkilendirme, web uygulamalarınıza ekleyin ve web API'leri.
- Korumalı web API'sine erişmek için bir erişim belirteci gerektirir.

Bu makalede kısaca açıklayan ve Azure AD V2 uç nokta için örneklere bağlantılar sağlar. Bu örnekler nasıl, uygulamalarınızda kullanabileceğiniz kod parçacıkları birlikte yapıldığını gösterir. Kod örnek sayfasında yardımcı gereksinimleri, yükleme ve ayarlama ayrıntılı Benioku konuları bulabilirsiniz. Kritik bölümler anlamanıza yardımcı olması için kod açıklamaları vardır.

> [!NOTE]
> V1 örnekleri ilgileniyorsanız bkz [Azure AD kod örneklerinin (V1 uç noktası)](sample-v1-code.md).

Her örnek türü için temel senaryo anlamak için bkz: [uygulama türleri için Azure Active Directory v2.0 uç noktası](v2-app-types.md).

GitHub üzerindeki örneklere katkıda bulunabilir. Bilgi edinmek için bkz. nasıl [Microsoft Azure Active Directory örnekler ve belgeler](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="single-page-applications-spa"></a>Tek sayfa uygulamaları (SPA)

Bu örnek, Azure AD ile güvenli hale getirilmiş bir tek sayfalı uygulama yazma işlemi gösterilmektedir. Bu örnekler MSAL.js türde birini kullanın: [JavaScript için Microsoft kimlik doğrulama Kitaplığı](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core), [Angular için Microsoft kimlik doğrulama Kitaplığı](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular), [Microsoft kimlik doğrulama kitaplığı AngularJS için](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs)

 Platform |  Microsoft Graph çağrıları
 -------- |  ---------------------
![JavaScript](media/sample-v2-code/logo_js.png) JavaScript (msal.js)  | [JavaScript-graphapi-web-v2](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2)
![Angular JS](media/sample-v2-code/logo_angular.png) JavaScript (MSAL AngularJS) | [MsalAngularjsDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angularjs/samples/MsalAngularjsDemoApp)
![Angular](media/sample-v2-code/logo_angular.png) JavaScript (MSAL Angular) | [MSALAngularDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angular/samples/MSALAngularDemoApp)

## <a name="web-applications"></a>Web uygulamaları

Aşağıdaki örnekleri kullanıcılarının oturumunu, web uygulamaları gösterir. Bazı örnekler, Microsoft Graph veya kendi kullanıcı kimliği ile web API çağırma uygulama da gösterir.

 Platform | Yalnızca kullanıcılar oturum açtığında | Kullanıcılar oturum açtığında ve Microsoft Graph çağırır
 -------- | ------------------- | ---------------------------------
![ASP.NET](media/sample-v2-code/logo_NETframework.png)<p/> ASP.NET | [webapp Openıdconnect dotNet appmodelv2](https://GitHub.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) <p/> [DotNet webapp openıdconnect v2](https://GitHub.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |              [ASP.NET bağlanmak rest örneği](https://github.com/microsoftgraph/aspnet-connect-rest-sample)
![ASP.NET](media/sample-v2-code/logo_NETcore.png)<p/>ASP.NET Core 2.0 sürümüne | [aspnetcore webapp openıdconnect v2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) |              [aspnetcore-connect-sample](https://github.com/microsoftgraph/aspnetcore-connect-sample)
![Node.js](media/sample-v2-code/logo_nodejs.png)  |                   | [WebApp Openıdconnect nodejs AppModelv2](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs)
![Ruby](media/sample-v2-code/logo_ruby.png) |                   | [Ruby bağlanmak rest örneği](https://github.com/microsoftgraph/ruby-connect-rest-sample)

## <a name="desktop-and-mobile-public-client-apps"></a>Masaüstü ve mobil genel istemci uygulamaları

Aşağıdaki örnekler, Microsoft Graph veya kendi kullanıcı adını Web API'nizi erişen uygulamalar (masaüstü/mobil uygulamalar için) ortak istemci gösterir. Tüm istemci uygulamaları MicroSoft kimlik doğrulama kitaplığı (MSAL) kullanma

İstemci uygulaması | Platform | Akış/verme | Microsoft Graph çağrıları | Bir ASP.NET Core 2.0 Web API'si çağıran
------------------ | -------- |  ----------| ---------- | -------------------------
Masaüstü (WPF)      | ![.NET / C#](media/sample-v2-code/logo_NET.png) | Etkileşimli | [DotNet Masaüstü msgraph v2](http://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) <p/> [DotNet-admin-kısıtlı-kapsamları-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) | [DotNet-yerel-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)
Masaüstü (konsol)   | ![.NET / C# (Masaüstü)](media/sample-v2-code/logo_NET.png) | Tümleşik Windows Kimlik Doğrulaması |[DotNet IWA v2](https://github.com/azure-samples/active-directory-dotnet-iwa-v2)
Masaüstü (konsol)   | ![.NET / C# (Masaüstü)](media/sample-v2-code/logo_NETcore.png) | Kullanıcı adı/parola |[dotnetcore yukarı v2](https://github.com/azure-samples/active-directory-dotnetcore-up-v2)
Mobile (UWP)   | ![.NET / C# (UWP)](media/sample-v2-code/logo_windows.png) | Etkileşimli |[DotNet-yerel-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) |
Mobile (Android, iOS, UWP)   | ![.NET / C# (Xamarin)](media/sample-v2-code/logo_xamarin.png) | Etkileşimli |[xamarin yerel v2](https://Github.com/azure-samples/active-directory-xamarin-native-v2) |
Mobile (iOS)       | ![iOS / Objective C ya da swift'te](media/sample-v2-code/logo_iOS.png) | Etkileşimli |[iOS swift yerel v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) <p/> [iOS-yerel-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |
Mobile (Android)   | ![Android / Java](media/sample-v2-code/logo_Android.png) | Etkileşimli |  [Android yerel v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |

## <a name="daemon-applications"></a>Arka plan programı uygulamaları

Kendi kimliğiyle (kullanıcı) ile Microsoft Graph erişen bir uygulama örneği.

İstemci uygulaması | Platform | Akış/verme | Microsoft Graph çağrıları
------------------ | -------- | ---------- | --------------------
Konsol | ![.NET Core](media/sample-v2-code/logo_NETcore.png)<p/> ASP.NET  | İstemci kimlik bilgileri | [dotnetcore arka plan programı v2](https://github.com/azure-samples/active-directory-dotnetcore-daemon-v2)
Web uygulaması | ![ASP.NET](media/sample-v2-code/logo_NETframework.png)<p/> ASP.NET  | İstemci kimlik bilgileri | [DotNet arka plan programı v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2)

## <a name="headless-applications"></a>Gözetimsiz uygulamalar

Aşağıdaki örnekler, genel istemci olmadan bir Web tarayıcı bir cihazda çalışan uygulamalar gösterir. Bu uygulamalar, komut satırı araçları veya uygulamaları Linux/Mac veya IOT uygulaması üzerinde çalışıyor olabilir. Örnek uygulamalara erişme etkileşimli olarak (örneğin bir cep telefonu) başka bir cihazda oturum açtığında bir kullanıcı adına Microsoft Graph özelliği. Tüm istemci uygulamaları MicroSoft kimlik doğrulama kitaplığı (MSAL) kullanma

İstemci uygulaması | Platform | Akış/verme | Microsoft Graph çağrıları
------------------ | -------- |  ----------| ----------
Masaüstü (konsol)   | ![.NET / C# (Masaüstü)](media/sample-v2-code/logo_NETcore.png) | Cihaz kod akışı |[dotnetcore devicecodeflow v2](https://github.com/azure-samples/active-directory-dotnetcore-devicecodeflow-v2)

## <a name="web-apis"></a>Web API'leri

Aşağıdaki örnek, Azure AD V2 uç noktası ile bir web API'sini korumak nasıl gösterir. Bu API, bir WPF uygulaması tarafından gerçekleştirilen (ancak herhangi bir uygulama tarafından gerçekten çağrılabilir)

Platform | Örnek
 -------- | -------------------
![.NET / C#](media/sample-v2-code/logo_NET.png) | [DotNet-yerel-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)

## <a name="other-microsoft-graph-samples"></a>Diğer Microsoft Graph örnekleri

Hakkında bilgi edinmek için [örnekleri](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) ve Azure AD ile kimlik doğrulaması dahil olmak üzere Microsoft Graph API için farklı kullanım düzenlerinin gösteren öğreticileri [Microsoft Graph topluluk örnekler ve öğreticiler](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Ayrıca bkz.

[Azure Active Directory geliştirici kılavuzu](azure-ad-developers-guide.md)

[Azure AD Graph API kavramsal ve başvuru](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API Yardımcısı kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
