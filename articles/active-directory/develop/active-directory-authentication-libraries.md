---
title: Azure Active Directory kimlik doğrulama kitaplıkları | Microsoft Docs
description: Azure AD Authentication Library (ADAL) istemci kolayca bulut için kullanıcıların kimliğini doğrulamak uygulama geliştiricilerin sağlar veya şirket içi Active Directory (AD) ve API çağrıları güvenliğini sağlamak için erişim belirteci alın.
services: active-directory
documentationcenter: ''
author: SaeedAkhter-MSFT
manager: mtillman
editor: mbaldwin
ms.assetid: 2e4fc79a-0285-40be-8c77-65edee408a22
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/13/2018
ms.author: saeeda
ms.custom: aaddev
ms.openlocfilehash: 1e82fa62c86c80b1e68fea995ca6f90861688d5c
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory kimlik doğrulama kitaplıkları

Azure Active Directory Authentication Library (ADAL) v1.0 bulut için kullanıcıların kimliğini doğrulamak uygulama geliştiricileri sağlar veya şirket içi Active Directory (AD) ve API çağrıları güvenliğini sağlamak için belirteç alın. ADAL kimlik doğrulaması özellikler sayesinde geliştiriciler için gibi kolaylaştırır:

- Depoları belirteçleri erişmek ve yenileme belirteçlerini yapılandırılabilir belirteç önbelleği
- bir erişim belirtecinin süresi ve yenileme belirteci kullanılabilir olduğunda otomatik belirteci yenileme
- zaman uyumsuz yöntem çağrıları için destek

> [!NOTE]
> Azure AD v2.0 kitaplıkları (MSAL) için mi arıyorsunuz? Checkout [MSAL kitaplığı Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).
>
>

## <a name="microsoft-supported-client-libraries"></a>Microsoft tarafından desteklenen istemci kitaplıkları

| Platform | Kitaplık | İndirme | Kaynak kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| .NET istemci, Windows mağazası, UWP, Xamarin iOS ve Android |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Masaüstü uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-dotnet) |[Başvuru](https://docs.microsoft.com/dotnet/api/?view=identitymodelclientsad-3.13.9) |
| .NET istemci, Windows mağazası, Windows Phone 8.1 |ADAL .NET v2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.4) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.4) | [Masaüstü uygulaması](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) | |
| JavaScript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[Tek sayfa uygulaması](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS, macOS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[iOS uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-ios) | [Başvuru](https://cocoadocs.org/docsets/ADAL/)|
| Android |ADAL |[Merkezi bir depoya](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Android uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-android) | [JavaDocs](http://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | [Node.js web uygulaması](https://github.com/Azure-Samples/active-directory-node-webapp-openidconnect)| |
| Java |ADAL4J |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Java web uygulaması](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect) | |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[Python web uygulaması](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi) | |

## <a name="microsoft-supported-server-libraries"></a>Microsoft tarafından desteklenen sunucu kitaplıkları

| Platform | Kitaplık | İndirme | Kaynak kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN için AzureAD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[CodePlex](http://katanaproject.codeplex.com) |[MVC uygulama](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |OWIN Openıdconnect için |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[CodePlex](http://katanaproject.codeplex.com) |[Web Uygulaması](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| .NET |WS-Federasyon için OWIN |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[CodePlex](http://katanaproject.codeplex.com) |[MVC Web uygulaması](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |.NET 4.5 kimlik Protokolü uzantıları |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |.NET 4.5 için JWT işleyicisi |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web API](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |

## <a name="scenarios"></a>Senaryolar

Uzaktaki bir kaynağa erişen istemci olarak ADAL kullanımı için üç yaygın senaryolar şunlardır:

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Aygıtta çalışan bir yerel istemci uygulaması, kullanıcıların kimlik doğrulaması

Bu senaryoda, bir mobil istemci veya web API'si gibi uzak bir kaynak erişmesi gereken masaüstü uygulaması bir geliştirici sahiptir. Web API anonim çağrılarına izin vermez ve kimliği doğrulanmış bir kullanıcı bağlamında çağrılmalıdır. Web API'si tarafından belirli bir verilen erişim belirteçleri güven önceden yapılandırılmış Azure AD kiracısı. Azure AD kaynağı için erişim belirteçleri vermek üzere önceden yapılandırılmıştır. İstemci web API çağırmak için Azure AD ile kimlik doğrulamasını kolaylaştırmak için ADAL Geliştirici kullanır. ADAL kullanmak için en güvenli (tarayıcı penceresini işlenen) kullanıcı kimlik bilgilerini toplamak için kullanıcı arabirimini oluşturmak için bir yoludur.

ADAL, kullanıcının kimliğini doğrulamak, Azure AD'den bir erişim belirteci ve yenileme belirteci edinmek ve web erişimi kullanarak API çağrısı belirteci kolaylaştırır.

Azure ad kimlik doğrulama kullanarak bu senaryoyu gösteren bir kod örneği için bkz: [yerel istemci WPF uygulaması Web API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>Bir web sunucusunda çalışan bir gizli istemci kimlik doğrulaması

Bu senaryoda, bir geliştirici web API'si gibi uzak bir kaynağa erişmek için gereken bir sunucu üzerinde çalışan bir uygulama sahiptir. API anonim tanımayan web yetkili hizmetinden çağrılmalıdır şekilde çağırır. Web API'si tarafından belirli bir verilen erişim belirteçleri güven önceden yapılandırılmış Azure AD kiracısı. Azure AD kaynağı için erişim belirteçleri (istemci kimliği ve parolası) istemci kimlik bilgileriyle bir hizmet vermek üzere önceden yapılandırılmıştır. ADAL kimlik doğrulama hizmeti web API'sini çağırmak için kullanılan bir erişim belirteci döndürme Azure AD ile kolaylaştırır. ADAL, ayrıca, önbelleğe alarak ve gerektiği gibi yenileme erişim belirteci ömrü yönetme işler. Bu senaryo gösteren bir kod örneği için bkz: [arka plan programı konsol Web API uygulamasına](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Bir kullanıcı adına bir sunucu üzerinde çalışan bir gizli istemci kimlik doğrulaması

Bu senaryoda, bir geliştirici web API'si gibi uzak bir kaynağa erişmek için gereken bir sunucu üzerinde çalışan bir web uygulaması sahiptir. Kimliği doğrulanmış bir kullanıcı adına bir yetkili hizmetinden çağrılmalıdır şekilde web API'si anonim çağrılarına izin vermez. Web API önceden yapılandırılmış belirli bir Azure AD tarafından verilen erişim belirteçleri güvenmeyi Kiracı ve Azure AD erişim belirteçleri kaynağı için istemci kimlik bilgileriyle bir hizmete vermek için önceden yapılandırılmış. Web uygulamasında kullanıcının kimliği doğrulandıktan sonra uygulama Azure AD kullanıcı için bir kimlik doğrulama kodu alabilirsiniz. Web uygulaması, bir erişim belirteci edinmek ve Azure AD'den uygulamayla ilişkili yetkilendirme kodu ve istemci kimlik bilgilerini kullanarak bir kullanıcı adına belirteci yenilemek için ADAL sonra kullanabilirsiniz. Web uygulaması erişim belirteci elinde eklendiğinde, belirtecin süresi doluncaya kadar web API'si çağırabilirsiniz. Belirtecin süresi dolduğunda, web uygulamasını daha önce alındı yenileme kullanarak yeni bir erişim belirteci belirtecini almak için ADAL kullanabilirsiniz. Bu senaryo gösteren bir kod örneği için bkz: [Web API Web API Native client](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof).

## <a name="see-also"></a>Ayrıca Bkz.

- [Azure Active Directory Geliştirici Kılavuzu](active-directory-developers-guide.md)
- [Azure Active directory için kimlik doğrulama senaryoları](active-directory-authentication-scenarios.md)
- [Azure Active Directory kod örnekleri](active-directory-code-samples.md)
