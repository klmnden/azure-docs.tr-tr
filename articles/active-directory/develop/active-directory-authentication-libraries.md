---
title: "Azure Active Directory kimlik doğrulama kitaplıkları | Microsoft Docs"
description: "Azure AD Authentication Library (ADAL) istemci kolayca bulut için kullanıcıların kimliğini doğrulamak uygulama geliştiricilerin sağlar veya şirket içi Active Directory (AD) ve API çağrıları güvenliğini sağlamak için erişim belirteci alın."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: mbaldwin
ms.assetid: 2e4fc79a-0285-40be-8c77-65edee408a22
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/25/2017
ms.author: bryanla
ms.custom: aaddev
<<<<<<< HEAD
ms.openlocfilehash: 1b79fb5b280b0cb4e087c2acde07796fd51e81fb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
=======
ms.openlocfilehash: f017e3d323b98660fdee902770652b3165e70e5e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory kimlik doğrulama kitaplıkları
Azure Active Directory Authentication Library (ADAL) bulut için kullanıcıların kimliğini doğrulamak uygulama geliştiricileri sağlar veya şirket içi Active Directory (AD) ve API çağrıları güvenliğini sağlamak için belirteç alın. ADAL kimlik doğrulaması özellikler sayesinde geliştiriciler için gibi kolaylaştırır:
 - Depoları belirteçleri erişmek ve yenileme belirteçlerini yapılandırılabilir belirteç önbelleği
 - bir erişim belirtecinin süresi ve yenileme belirteci kullanılabilir olduğunda otomatik belirteci yenileme
 - zaman uyumsuz yöntem çağrıları için destek
 - ve daha fazlası

> [!NOTE]
> Azure AD v2.0 kitaplıkları (MSAL) için mi arıyorsunuz? Checkout [MSAL kitaplığı Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries). 
> 
> 

### <a name="client-libraries"></a>İstemci Kitaplıkları

| Platform | Kitaplık | İndir | Kaynak kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| .NET istemci, Windows mağazası, UWP, Xamarin iOS ve Android |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Masaüstü uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-dotnet) |[Başvuru](https://docs.microsoft.com/dotnet/api/?view=identitymodelclientsad-3.13.9) | 
| .NET istemci, Windows mağazası, Windows Phone 8.1 |ADAL .NET v2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.4) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.4) | [Masaüstü uygulaması](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) | | 
| JavaScript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[Tek sayfa uygulaması](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS, macOS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[iOS uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-ios) | [Başvuru](https://cocoapods.org/pods/ADAL)|
| Android |ADAL |[Merkezi bir depoya](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Android uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-android) | [JavaDocs](http://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | | |
| Java |ADAL4J |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Java web uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-java) | |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) | | |

### <a name="server-libraries"></a>Sunucu kitaplıkları 

| Platform | Kitaplık | İndir | Kaynak kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN için AzureAD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[CodePlex](http://katanaproject.codeplex.com) |[MVC uygulama](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |OWIN Openıdconnect için |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[CodePlex](http://katanaproject.codeplex.com) |[Web Uygulaması](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| Node.js |Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web API](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |
| .NET |WS-Federasyon için OWIN |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[CodePlex](http://katanaproject.codeplex.com) |[MVC Web uygulaması](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |.NET 4.5 kimlik Protokolü uzantıları |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |.NET 4.5 için JWT işleyicisi |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |

### <a name="v20-client-libraries-msal"></a>v2.0 istemci kitaplıkları (MSAL)

[Azure AD v2.0 uç](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare) Azure AD bir araya getirir ve tek bir uç nokta arkasında Microsoft Accounts. Geliştiricilerin Bu uç noktasına erişmek için kullanabileceğiniz [üretim desteklenen Önizleme MSAL kitaplıkları](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) ADAL yerine.

| Platform | Kitaplık | İndir | Kaynak kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| .NET istemci, Windows mağazası, UWP, Xamarin iOS ve Android |.NET (Önizleme) için MSAL |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client/1.1.0-preview) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Masaüstü uygulaması](~/articles/active-directory/develop/guidedsetups/active-directory-windesktop.md) |[Başvuru](https://docs.microsoft.com/dotnet/api/?view=identityclient-1.1.0-preview) | 
| JavaScript |MSAL JavaScript (Önizleme) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [Tek sayfa uygulaması](~/articles/active-directory/develop/GuidedSetups/active-directory-javascriptspa.md) | [Başvuru](https://htmlpreview.github.io/?https://raw.githubusercontent.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/docs/classes/_useragentapplication_.msal.useragentapplication.html) | 
| iOS |MSAL iOS (Önizleme) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [iOS uygulaması](~/articles/active-directory/develop/GuidedSetups/active-directory-ios.md) | [Başvuru](https://azuread.github.io/docs/objc/) |
| Android |MSAL Android (Önizleme) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Android uygulaması](~/articles/active-directory/develop/GuidedSetups/active-directory-android.md) | [Başvuru](http://javadoc.io/doc/com.microsoft.identity.client/msal/0.1.1) |

## <a name="scenarios"></a>Senaryolar

ADAL içinde kullanılabilir uzaktaki bir kaynağa erişen istemci kimlik doğrulaması için üç yaygın senaryolar şunlardır:  

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Aygıtta çalışan bir yerel istemci uygulaması, kullanıcıların kimlik doğrulaması 

Bu senaryoda, bir geliştirici web API'si gibi Azure AD tarafından güvenli hale getirilmiş uzaktaki bir kaynağa erişmek için gereken bir WPF istemci uygulaması sahiptir. Kendisi bir Azure aboneliğine sahip, Aşağı Akış web API'sini çağırmak nasıl bilir ve web API kullanan Azure AD kiracısı bilir. Sonuç olarak, kendisinin tam olarak adal kimlik doğrulaması deneyimi için temsilci seçme veya kullanıcı kimlik bilgilerini açıkça işleme Azure AD ile kimlik doğrulamasını kolaylaştırmak için ADAL kullanabilirsiniz. ADAL yapar, kullanıcının kimliğini doğrulamak, Azure AD'den bir erişim belirteci ve yenileme belirteci edinmek ve yapmak için erişim belirteci kullanın kolay web API ister.

Azure ad kimlik doğrulama kullanarak bu senaryoyu gösteren bir kod örneği için bkz: [yerel istemci WPF uygulaması Web API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>Bir web sunucusunda çalışan bir gizli istemci kimlik doğrulaması

Bu senaryoda, bir geliştirici web API'si gibi Azure AD tarafından güvenli hale getirilmiş uzaktaki bir kaynağa erişmek için gereken bir sunucu üzerinde çalışan bir uygulama sahiptir. Kendisi bir Azure aboneliğine sahip, aşağı akış hizmetini çağırmak nasıl bilir ve Azure AD Kiracı web API'sini kullanan bilir. Sonuç olarak, kendisine açıkça uygulamanın kimlik bilgileri işleyerek Azure AD ile kimlik doğrulamasını kolaylaştırmak için ADAL kullanabilirsiniz. ADAL yapar, uygulamanın istemci kimlik bilgilerini kullanarak Azure AD'den bir belirteç almak ve yapmak için belirtecini kullanmak kolay web API ister. ADAL, ayrıca, önbelleğe alarak ve gerektiği gibi yenileme erişim belirteci ömrü yönetme işler. Bu senaryo gösteren bir kod örneği için bkz: [arka plan programı konsol Web API uygulamasına](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Bir kullanıcı adına bir sunucu üzerinde çalışan bir gizli istemci kimlik doğrulaması 

Bu senaryoda, bir geliştirici web API'si gibi Azure AD tarafından güvenli hale getirilmiş uzaktaki bir kaynağa erişmek için gereken bir sunucu üzerinde çalışan bir uygulama sahiptir. İstek, ayrıca bir Azure AD kullanıcı adına yapılması gerekir. Kendisi bir Azure aboneliğine sahip, Aşağı Akış web API'sini çağırmak nasıl bilir ve hizmet kullandığı Kiracı Azure AD bilir. Uygulama web uygulamasına kullanıcının kimliği doğrulandıktan sonra bir kimlik doğrulama kodu kullanıcı için Azure AD'den elde edebilirsiniz. Web uygulaması, bir erişim belirteci edinmek ve Azure AD'den uygulamayla ilişkili yetkilendirme kodu ve istemci kimlik bilgilerini kullanarak bir kullanıcı adına belirteci yenilemek için ADAL sonra kullanabilirsiniz. Web uygulaması erişim belirteci elinde eklendiğinde, belirtecin süresi doluncaya kadar web API'si çağırabilirsiniz. Belirtecin süresi dolduğunda, web uygulamasını daha önce alındı yenileme kullanarak yeni bir erişim belirteci belirtecini almak için ADAL kullanabilirsiniz. Bu senaryo gösteren bir kod örneği için bkz: [Web API Web API Native client](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof).

## <a name="see-also"></a>Ayrıca Bkz.

- [Azure Active Directory Geliştirici Kılavuzu](active-directory-developers-guide.md)
- [Azure Active directory için kimlik doğrulama senaryoları](active-directory-authentication-scenarios.md)
- [Azure Active Directory kod örnekleri](active-directory-code-samples.md)
