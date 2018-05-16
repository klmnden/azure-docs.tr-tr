---
title: Azure Active Directory v2.0 kimlik doğrulama kitaplıkları | Microsoft Docs
description: Uyumlu istemci kitaplıkları ve sunucu ara yazılım kitaplıkları ve ilgili kitaplık, kaynak ve Azure Active Directory v2.0 uç noktası için örnekleri bağlantılar.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 19cec615-e51f-4141-9f8c-aaf38ff9f746
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/13/2018
ms.author: celested
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 8fe3db09acbdec606f25d0bc81300bc4f5e87411
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory v2.0 kimlik doğrulama kitaplıkları

[Azure Active Directory (Azure AD) v2.0 uç](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare) endüstri standardı OAuth 2.0 ve Openıd Connect 1.0 protokollerini destekler. Microsoft kimlik doğrulama kitaplığı (MSAL) Azure AD v2.0 uç noktası ile çalışmak üzere tasarlanmıştır. OAuth 2.0 ve Openıd Connect 1.0 desteği açık kaynak kitaplıkları kullanmak da mümkündür.

Bir güvenlik geliştirme yaşam döngüsü (SDL) Metodoloji gibi izleyin Protokolü etki alanı uzmanlar tarafından yazılan kitaplıkları kullanmanız önerilir [bir Microsoft tarafından izlenen][Microsoft-SDL]. Elle kod destek protokoller için karar verirseniz, Microsoft'un SDL ve ödeme standartları özellikleri her protokol için güvenlik konuları dikkat kapatmak gibi bir Metodoloji izleyin.

> [!NOTE]
> Azure AD için sık sorulan sorular v1.0 kitaplıkları (ADAL) mi arıyorsunuz? Checkout [ADAL kitaplığı Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries).
>
>

## <a name="types-of-libraries"></a>Tür kitaplıkları

Azure AD v2.0 uç iki tür kitaplıklarını çalışır:

* **İstemci kitaplıkları**. Yerel istemcileri ve sunucuları Microsoft Graph gibi bir kaynak çağırmak için erişim belirteçleri almak için istemci kitaplıkları'nı kullanın.
* **Sunucu ara yazılım kitaplıkları**. Web uygulamaları, kullanıcı oturum açma için sunucu ara yazılım kitaplıkları kullanabilirsiniz. Web API'leri sunucusu ara yazılım kitaplıkları yerel istemciler veya diğer sunucular tarafından gönderilen belirteçleri doğrulamak için kullanın.

## <a name="library-support"></a>Kitaplık desteği

V2.0 uç noktası kullandığınızda herhangi bir standartlara uygun kitaplığı seçebilirsiniz çünkü desteği nereye bilmeniz önemlidir. Sorunları ve kitaplık kodu özellik istekleri için kitaplık sahibine başvurun. Sorunlar ve Hizmet tarafı protokolünü uygulamasındaki özellik istekleri için Microsoft'a başvurun. [Özellik isteği dosya](https://feedback.azure.com/forums/169401-azure-active-directory) protokolde görmek istediğiniz ek özellikler için. [Bir destek isteği oluşturmak](https://docs.microsoft.com/en-us/azure/azure-supportability/how-to-create-azure-support-request) bir sorun bulursanız burada Azure AD v2.0 uç OAuth 2.0 veya Openıd Connect 1.0 ile uyumlu değildir.

Kitaplıkları iki destek kategoride getirir:

* **Microsoft tarafından desteklenen**. Microsoft bu kitaplıklar için düzeltme sağlayan ve SDL yapmıştır Bu kitaplıklar üzerinde durum tespitlerini.
* **Uyumlu**. Microsoft, bu kitaplıklar temel senaryolarda test ve v2.0 uç noktası ile çalıştıkları onaylandı. Bu kitaplıklar için düzeltmeler ve bu kitaplıklar gözden yapılmadı Microsoft sağlamaz. Kitaplığın açık kaynaklı proje yönlendirilmiş sorunları ve özellik istekleri.

Bu makalenin sonraki bölümlerinde v2.0 uç noktası ile çalışma kitaplıkların bir listesi için bkz.

## <a name="microsoft-supported-client-libraries"></a>Microsoft tarafından desteklenen istemci kitaplıkları

> [!IMPORTANT]
> MSAL Önizleme kitaplıkları, bir üretim ortamında kullanım için uygundur. Microsoft, geçerli üretim kitaplıkları (ADAL) Bu kitaplıklar için aynı üretim düzeyi destek sağlar. Önizleme sırasında MSAL API, dahili önbellek biçimi ve diğer hata düzeltmeleri veya özellik geliştirmeleri ile birlikte alın gerekecektir uyarısı olmadan bu kitaplıklar mekanizmaları değişiklikler bekler. Bu, uygulamanızın etkileyebilir. Örneğin, önbellek biçimini bir değişiklik tekrar oturum açmalarını gerektirebilir. Bir API değişikliği kodunuzu güncelleştirin gerektirebilir. Genel kullanılabilirlik (GA) sürümü kullanılabilir olduğunda, kitaplık önizleme sürümünü kullanan tüm uygulamalar altı ay içinde güncelleştirmeniz gerekir veya bunlar çalışmamaya başlayabilir.

| Platform | Kitaplık | İndirme | Kaynak kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| .NET istemci, Windows mağazası, UWP, Xamarin iOS ve Android | MSAL .NET (Önizleme) |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Masaüstü uygulaması](guidedsetups/active-directory-mobileanddesktopapp-windowsdesktop-intro.md) |  |
| JavaScript | MSAL.js (Önizleme) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [Tek sayfa uygulaması](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |  |
| iOS, macOS | MSAL (Önizleme) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [iOS uygulaması](https://github.com/Azure-Samples/active-directory-msal-ios-swift) |  |
| Android | MSAL (Önizleme) | [Merkezi bir depoya](https://repo1.maven.org/maven2/com/microsoft/identity/client/msal/) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Android uygulaması](guidedsetups/active-directory-mobileanddesktopapp-android-intro.md) | [JavaDocs](http://javadoc.io/doc/com.microsoft.identity.client/msal) |

## <a name="microsoft-supported-server-middleware-libraries"></a>Microsoft tarafından desteklenen sunucu ara yazılım kitaplıkları

| Platform | Kitaplık | İndirme | Kaynak kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| .NET 4.x | OWIN Openıd Connect Ara |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[GitHub](https://github.com/aspnet/AspNetKatana/) |[MVC uygulama](guidedsetups/active-directory-serversidewebapp-aspnetwebappowin-intro.md) | |
| .NET 4.x | OWIN OAuth taşıyıcı ara yazılımı Azuread'i için |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[GitHub](https://github.com/aspnet/AspNetKatana/) |  | |
| .NET 4.x | .NET 4.5 için JWT işleyicisi | [NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt/4.0.4.403061554) | [GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET Core | ASP.NET Openıd Connect Ara |[Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] |[ASP.NET güvenliği (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] |[MVC uygulama](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2) |
| .NET Core | ASP.NET OAuth taşıyıcı ara yazılımı |[Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] |[ASP.NET güvenliği (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] |  |
| .NET Core | .NET Core için JWT işleyicisi  |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web uygulaması](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs)| |

## <a name="compatible-client-libraries"></a>Uyumlu istemci kitaplıkları

| Platform | Kitaplık adı | Test edilen sürüm | Kaynak kod | Örnek |
|:---:|:---:|:---:|:---:|:---:|
| Android |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/) |0.2.1 |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) |[Yerel uygulaması örneği](active-directory-v2-devquickstarts-android.md) |
| iOS |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |1.2.8 |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |[Yerel uygulaması örneği](active-directory-v2-devquickstarts-ios.md) |
| JavaScript |[Hello.js](https://adodson.com/hello.js/) |1.13.5 |[Hello.js](https://github.com/MrSwitch/hello.js) |[SPA](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |
| Java | [Scribe Java scribejava](https://github.com/scribejava/scribejava) | [Sürüm 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | [ScribeJava](https://github.com/scribejava/scribejava/) | |
| PHP | [PHP ligi oauth2 istemci](https://github.com/thephpleague/oauth2-client) | [Sürüm 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | [oauth2 istemci](https://github.com/thephpleague/oauth2-client/) | |
| Ruby |[OmniAuth](https://github.com/omniauth/omniauth/wiki) |omniauth:1.3.1</br>omniauth-oauth2:1.4.0 |[OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) |  |

## <a name="related-content"></a>İlgili içerik
Azure AD v2.0 uç hakkında daha fazla bilgi için bkz: [Azure AD uygulama modeli v2.0 genel bakış][AAD-App-Model-V2-Overview].

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: ../active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
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

[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
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
