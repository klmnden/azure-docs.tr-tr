---
title: Microsoft kimlik platformu kimlik doğrulama kitaplıkları | Microsoft Docs
description: Uyumlu bir istemci kitaplıkları ve sunucu ara yazılım kitaplıkları ile ilişkili kitaplık, kaynak ve Microsoft kimlik platformu uç noktası için örnek bağlantılar.
services: active-directory
documentationcenter: ''
author: negoe
manager: CelesteDG
editor: ''
ms.assetid: 19cec615-e51f-4141-9f8c-aaf38ff9f746
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: negoe
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5c3edfd9ef346407529eea1d887efd795e647808
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67440806"
---
# <a name="microsoft-identity-platform-authentication-libraries"></a>Microsoft kimlik platformu kimlik doğrulama kitaplıkları

[Microsoft kimlik platformu uç nokta](active-directory-v2-compare.md) endüstri standardı OAuth 2.0 ve Openıd Connect 1.0 protokollerini destekler. Microsoft Authentication Library (MSAL), Microsoft kimlik platformu uç noktası ile çalışacak şekilde tasarlanmıştır. OAuth 2.0 ve Openıd Connect 1.0 destekleyen açık kaynak kitaplıkları da kullanabilirsiniz.

Güvenlik geliştirme yaşam döngüsü (SDL) Metodoloji izleyen Protokolü etki alanı uzmanları tarafından yazılmış kitaplıkları kullanmanızı öneririz. Tür kitaplıkları içerir [Microsoft izleyen bir][Microsoft-SDL]. Kod protokoller için teslim, Microsoft SDL gibi bir Metodoloji izlemelidir. Kapat güvenlik konuları için her protokol için standartları özellikleri dikkat edin.

> [!NOTE]
> Azure Active Directory kimlik doğrulama kitaplığı (ADAL için) mı arıyorsunuz? Kullanıma [ADAL kitaplığını Kılavuzu](active-directory-authentication-libraries.md).

## <a name="types-of-libraries"></a>Tür kitaplıkları

Microsoft kimlik platformu uç nokta, iki tür kitaplıkları ile çalışır:

* **İstemci kitaplıkları**: Yerel istemcileri ve sunucuları bir kaynak gibi Microsoft Graph'i çağırmaya yönelik erişim belirteçlerini almak için istemci kitaplıkları'nı kullanın.
* **Server ara yazılım kitaplıklarını**: Web uygulamaları için kullanıcı oturum açma sunucusu ara yazılım kitaplıkları kullanın. Web API'leri, diğer sunucuları veya yerel istemciler tarafından gönderilen belirteçleri doğrulamak için sunucu ara yazılım kitaplıkları kullanın.

## <a name="library-support"></a>Kitaplık desteği

Kitaplıkları iki destek kategoriye ayrılır:

* **Microsoft tarafından desteklenen**: Microsoft bu kitaplıklar için düzeltme sağlayan ve SDL işlemi yapana aksaklıkla Bu kitaplıklar üzerinde.
* **Uyumlu**: Microsoft bu kitaplıklar temel senaryolarda test ve Microsoft kimlik platformu uç noktada çalıştığını doğrulamıştır. Microsoft, bu kitaplıklar için düzeltmeler ve bu kitaplıklar incelenmesi bitti taşınmadığından sunmaz. Sorunları ve özellik istekleri kitaplığın açık kaynak projesine yönlendirilebilir.

Microsoft kimlik platformu uç nokta ile çalışmaya kitaplıkların bir listesi için aşağıdaki bölümlere bakın.

## <a name="microsoft-supported-client-libraries"></a>Microsoft tarafından desteklenen istemci kitaplıkları

Korumalı bir web API'sini çağırmak için bir belirteç almak için istemci kimlik doğrulama kitaplıklarını kullanın.

| Platform | Kitaplık | İndirme | Kaynak kod | Örnek | Başvuru | Kavramsal belge | Yol Haritası |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ![JavaScript](media/sample-v2-code/logo_js.png) | MSAL.js  | [NPM](https://www.npmjs.com/package/msal) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/README.md) |  [Tek sayfalı uygulama](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) | [Başvuru](https://htmlpreview.github.io/?https://raw.githubusercontent.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core/docs/classes/_useragentapplication_.useragentapplication.html) | [Kavramsal belgeler](msal-overview.md)| [Yol Haritası](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki#roadmap)
|![Angular JS](media/sample-v2-code/logo_angular.png) | MSAL Angular JS | [NPM](https://www.npmjs.com/package/@azure/msal-angularjs) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs/README.md) |  |  | |
![Angular](media/sample-v2-code/logo_angular.png) | MSAL Angular (Önizleme) | [NPM](https://www.npmjs.com/package/@azure/msal-angular) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | | | |
| ![.NET Framework](media/sample-v2-code/logo_NET.png) ![UWP](media/sample-v2-code/logo_windows.png) ![Xamarin](media/sample-v2-code/logo_xamarin.png) | MSAL.NET  |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Masaüstü uygulaması](guidedsetups/active-directory-mobileanddesktopapp-windowsdesktop-intro.md) | [MSAL.NET](https://docs.microsoft.com/dotnet/api/microsoft.identity.client?view=azure-dotnet-preview) |[Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki#conceptual-documentation) | [Yol Haritası](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki#roadmap)
| ![Python](media/sample-v2-code/logo_python.png) | MSAL Python (Önizleme) | [PyPI](https://pypi.org/project/msal) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-python) | [Örnekler](https://github.com/AzureAD/microsoft-authentication-library-for-python/tree/dev/sample) | [ReadTheDocs](https://msal-python.rtfd.io/) | [Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki) | [Yol Haritası](https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki/Roadmap)
| ![Java](media/sample-v2-code/logo_java.png) | MSAL Java (Önizleme) | [Maven](https://mvnrepository.com/artifact/com.microsoft.azure/msal4j) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-java) | [Örnekler](https://github.com/AzureAD/microsoft-authentication-library-for-java/tree/dev/src/samples) | | | [Yol Haritası](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki)
| ![iOS / Objective C ya da swift'te](media/sample-v2-code/logo_iOS.png) | MSAL obj_c (Önizleme) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [iOS uygulaması](https://github.com/Azure-Samples/active-directory-msal-ios-swift) |  |
|![Android / Java](media/sample-v2-code/logo_Android.png) | MSAL (Önizleme) | [Merkezi depo](https://repo1.maven.org/maven2/com/microsoft/identity/client/msal/) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Android uygulaması](quickstart-v2-android.md) | [JavaDocs](https://javadoc.io/doc/com.microsoft.identity.client/msal) | | |

## <a name="microsoft-supported-server-middleware-libraries"></a>Microsoft tarafından desteklenen sunucusu ara yazılım kitaplıkları

Web uygulamaları ve web API'leri korumaya yardımcı olmak için bir ara yazılım kitaplıkları kullanın. Web uygulamaları veya web API ASP.NET veya ASP.NET Core ile yazılan, ara yazılım kitaplıkları kullanın.

| Platform | Kitaplık | İndirme | Kaynak kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| ![.NET](media/sample-v2-code/logo_NET.png) ![.NET Core](media/sample-v2-code/logo_NETcore.png) | ASP.NET güvenliği |[NuGet](https://www.nuget.org/packages/Microsoft.AspNet.Mvc/) |[GitHub](https://github.com/aspnet/AspNetCore) |[MVC uygulaması](quickstart-v2-aspnet-webapp.md) |[ASP.NET API Başvurusu](https://docs.microsoft.com/dotnet/api/?view=aspnetcore-2.0) |
| ![.NET](media/sample-v2-code/logo_NET.png)| .NET için IdentityModel uzantıları| |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | [MVC uygulaması](quickstart-v2-aspnet-webapp.md) |[Başvuru](https://docs.microsoft.com/dotnet/api/overview/azure/activedirectory/client?view=azure-dotnet) |
| ![Node.js](media/sample-v2-code/logo_nodejs.png) | Azure AD Passport |[NPM](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web uygulaması](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs) | |

## <a name="compatible-client-libraries"></a>Uyumlu bir istemci kitaplıkları

| Platform | Kitaplık adı | Test edilen sürüm | Kaynak kod | Örnek |
|:---:|:---:|:---:|:---:|:---:|
|![JavaScript](media/sample-v2-code/logo_js.png)|[Hello.js](https://adodson.com/hello.js/) | Sürüm 1.13.5 |[Hello.js](https://github.com/MrSwitch/hello.js) |[SPA](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |
| ![Java](media/sample-v2-code/logo_java.png) | [Java içerdiğini açıklayın](https://github.com/scribejava/scribejava) | [Sürüm 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | [ScribeJava](https://github.com/scribejava/scribejava/) | |
| ![Java](media/sample-v2-code/logo_java.png) | [Kitaplık Gluu Openıd Connect](https://github.com/GluuFederation/oxAuth) | [Sürüm 3.0.2](https://github.com/GluuFederation/oxAuth/releases/tag/3.0.2) | [Kitaplık Gluu Openıd Connect](https://github.com/GluuFederation/oxAuth) | |
| ![Python](media/sample-v2-code/logo_python.png) | [İstekleri OAuthlib](https://github.com/requests/requests-oauthlib) | [Sürümü 1.2.0](https://github.com/requests/requests-oauthlib/releases/tag/v1.2.0) | [İstekleri OAuthlib](https://github.com/requests/requests-oauthlib) | |
| ![Node.js](media/sample-v2-code/logo_nodejs.png) | [openıd istemcisi](https://github.com/panva/node-openid-client) | [Sürüm 2.4.5](https://github.com/panva/node-openid-client/releases/tag/v2.4.5) | [openıd istemcisi](https://github.com/panva/node-openid-client) | |
| ![PHP](media/sample-v2-code/logo_php.png) | [PHP ligi oauth2 istemci](https://github.com/thephpleague/oauth2-client) | [Sürüm 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | [oauth2 istemci](https://github.com/thephpleague/oauth2-client/) | |
| ![Ruby](media/sample-v2-code/logo_ruby.png) |[OmniAuth](https://github.com/omniauth/omniauth/wiki) |omniauth: 1.3.1<br />omniauth oauth2: 1.4.0 |[OmniAuth](https://github.com/omniauth/omniauth)<br />[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) |  |
| ![iOS](media/sample-v2-code/logo_iOS.png) ![Android](media/sample-v2-code/logo_Android.png) | [React Native App Auth](https://github.com/FormidableLabs/react-native-app-auth) | [Sürüm 4.2.0](https://github.com/FormidableLabs/react-native-app-auth/releases/tag/v4.2.0) | [React Native App Auth](https://github.com/FormidableLabs/react-native-app-auth) | |

Standartlara uygun herhangi bir kitaplığı için Microsoft kimlik platformu uç noktası kullanabilirsiniz. Destek için nereye bilmeniz önemlidir:

* Sorunlar ve yeni özellik istekleriniz kitaplığı kod için kitaplık sahibine başvurun.
* Sorunlar ve Hizmet tarafı protokolünü uygulamasındaki yeni özellik istekleri için Microsoft ile iletişime geçin.
* [Bir özellik isteği](https://feedback.azure.com/forums/169401-azure-active-directory) protokolünde görmek istediğiniz ek özellikler.
* [Bir destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) bir hata bulursanız Microsoft kimlik platformu uç nokta burada OAuth 2.0 veya Openıd Connect 1.0 ile uyumlu değil.

## <a name="related-content"></a>İlgili içerik

Microsoft kimlik platformu uç noktası hakkında daha fazla bilgi için bkz: [Microsoft Identity platformuna genel bakış][AAD-App-Model-V2-Overview].

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: v2-overview.md
[ClientLib-NET-Lib]: https://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:/
[ClientLib-Iosmac-Lib]:/
[ClientLib-Iosmac-Repo]:/
[ClientLib-Iosmac-Sample]:/
[ClientLib-Android-Lib]:/
[ClientLib-Android-Repo]:/
[ClientLib-Android-Sample]:/
[ClientLib-Js-Lib]:/
[ClientLib-Js-Repo]:/
[ClientLib-Js-Sample]:/

[Microsoft-SDL]: https://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: https://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: https://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/documentation/articles/active-directory-v2-devquickstarts-node-web/
