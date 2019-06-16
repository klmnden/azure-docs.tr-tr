---
title: Azure Active Directory kimlik doğrulama kitaplıkları | Microsoft Docs
description: Azure AD Authentication Library (ADAL) istemci uygulama geliştiricilerin bulut kullanıcıların kimliklerini kolaylıkla doğrulayın sağlar veya şirket içi Active Directory (AD) ve ardından API çağrıları güvenliğini sağlamaya yönelik erişim belirteçleri edinin.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 2e4fc79a-0285-40be-8c77-65edee408a22
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2018
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b71585c178efbc30892cf95c5c2149818f0dcb3c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65764580"
---
# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory kimlik doğrulama kitaplıkları

Azure Active Directory kimlik doğrulama kitaplığı (ADAL) v1.0 bulut için kullanıcıların kimliğini doğrulamak uygulama geliştiricilerin sağlayan veya şirket içi Active Directory (AD) ve API çağrılarını güvenliğini sağlamak için belirteçleri elde etmek. ADAL kimlik doğrulaması özellikleri sayesinde geliştiriciler için gibi kolaylaştırır:

- Depoları erişim belirteçleri ve yenileme belirteçlerini yapılandırılabilir belirteç önbelleği
- Bir erişim belirtecinin süresi ve yenileme belirteci kullanılabilir olduğunda otomatik belirteç yenileme
- Zaman uyumsuz yöntem çağrıları için destek

> [!NOTE]
> Azure AD v2.0 kitaplığı (MSAL) için mi arıyorsunuz? Kullanıma alma [MSAL kitaplık Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).
>
>

## <a name="microsoft-supported-client-libraries"></a>Microsoft tarafından desteklenen istemci kitaplıkları

| Platform | Kitaplık | İndirme | Kaynak kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| .NET istemci, Windows Store, UWP, Xamarin iOS ve Android |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Masaüstü uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-dotnet) |[Başvuru](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory?view=azure-dotnet) |
| .NET istemci, Windows Store, Windows Phone 8.1 |ADAL .NET v2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.4) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.4) | [Masaüstü uygulaması](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) | |
| JavaScript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[Tek sayfalı uygulama](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS, macOS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[iOS uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-ios) | [Başvuru](http://cocoadocs.org/docsets/ADAL/2.5.1/)|
| Android |ADAL |[Maven](https://search.maven.org/search?q=g:com.microsoft.aad+AND+a:adal&core=gav) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Android uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-android) | [JavaDocs](https://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | [Node.js web uygulaması](https://github.com/Azure-Samples/active-directory-node-webapp-openidconnect)|[Başvuru](https://docs.microsoft.com/javascript/api/adal-node/?view=azure-node-latest) |
| Java |ADAL4J |[Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3Aadal4j%20g%3Acom.microsoft.azure) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Java web uygulaması](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect) |[Başvuru](https://javadoc.io/doc/com.microsoft.azure/adal4j) |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[Python web uygulaması](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi) |[Başvuru](https://adal-python.readthedocs.io/) |

## <a name="microsoft-supported-server-libraries"></a>Microsoft tarafından desteklenen sunucu kitaplıkları

| Platform | Kitaplık | İndirme | Kaynak kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN için AzureAD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[GitHub](https://github.com/aspnet/AspNetKatana/tree/dev/src/Microsoft.Owin.Security.ActiveDirectory) |[MVC uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |Openıdconnect için OWIN |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[GitHub](https://github.com/aspnet/AspNetKatana/tree/dev/src/Microsoft.Owin.Security.OpenIdConnect) |[Web Uygulaması](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| .NET |OWIN için WS-Federasyon |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[GitHub](https://github.com/aspnet/AspNetKatana/tree/dev/src/Microsoft.Owin.Security.WsFederation) |[MVC Web App](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |.NET 4.5 kimlik Protokolü uzantıları |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |.NET 4.5 için JWT işleyicisi |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web API](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |

## <a name="scenarios"></a>Senaryolar

ADAL kullanarak uzak bir kaynağa erişirken bir istemci üç yaygın senaryolar şunlardır:

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Bir cihaz üzerinde çalışan bir yerel istemci uygulaması, kullanıcıların kimliğini doğrulama

Bu senaryoda, bir geliştirici, bir mobil istemci veya bir web API gibi uzak bir kaynağa erişmesi gereken masaüstü uygulaması sahip. Web API'si, anonim çağrılarına izin vermemektedir ve kimliği doğrulanmış bir kullanıcı bağlamında çağrılmalıdır. Web API'si tarafından belirli bir verilen erişim belirteçleri güvenmek üzere önceden yapılandırılmış Azure AD kiracısı. Azure AD, bu kaynağa erişim belirteçlerini vermek için önceden yapılandırılmıştır. Geliştirici istemcisinden web API'sini çağırmak için Azure AD ile kimlik doğrulamasını kolaylaştırmak için ADAL kullanır. (Tarayıcı penceresi işlenmiş) kullanıcı kimlik bilgilerini toplamak için kullanıcı arabirimini oluşturmak için ADAL'ı kullanmak için en güvenli yolu var.

ADAL, bir kullanıcı kimlik doğrulaması, Azure AD'den bir erişim belirteci ve yenileme belirteci almak ve sonra web erişimi'ni kullanarak API çağırın belirteci kolaylaştırır.

Azure ad kimlik doğrulaması kullanarak bu senaryoyu gösteren bir kod örneği için bkz. [yerel istemci WPF uygulaması Web API'si için](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>Bir web sunucusunda çalışan bir gizli istemci uygulaması kimlik doğrulaması

Bu senaryoda, bir geliştirici, web API'si gibi uzak bir kaynağa erişmesi gereken bir sunucu üzerinde çalışan bir uygulamanın sahip. Web API anonim izin vermiyor yetkili bir hizmetten çağrılmalıdır şekilde çağırır. Web API'si tarafından belirli bir verilen erişim belirteçleri güvenmek üzere önceden yapılandırılmış Azure AD kiracısı. Azure AD, bu kaynağa erişim belirteçleri (istemci kimliği ve parolası) istemci kimlik bilgileriyle bir hizmette sorun şekilde önceden yapılandırılmıştır. ADAL kimlik doğrulaması hizmeti Azure AD ile web API'sini çağırmak için kullanılabilecek bir erişim belirteci döndüren kolaylaştırır. Erişim belirtecinin ömrü, önbelleğe alma ve gerektiği şekilde yenileme yönetmeye ADAL da işler. Bu senaryoyu gösteren bir kod örneği için bkz. [arka plan programı konsol Web API'sine uygulama](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Bir kullanıcı adına bir sunucu üzerinde çalışan bir gizli bir istemci uygulaması kimlik doğrulaması

Bu senaryoda, bir geliştirici, web API'si gibi uzak bir kaynağa erişmesi gereken bir sunucu üzerinde çalışan bir web uygulaması sahip. Web API'si için kimliği doğrulanmış bir kullanıcı adına bir yetkili hizmetinden çağrılmalıdır anonim çağrıları izin vermez. Web API'si ön yapılandırma olan belirli bir Azure AD tarafından verilen erişim belirteçlere güvenecek şekilde Kiracı ve Azure AD erişim belirteçleri bu kaynak için istemci kimlik bilgileriyle hizmete vermek için önceden yapılandırılmış. Web uygulamasında kullanıcının kimliği doğrulandıktan sonra uygulamanın Azure AD'den bir yetkilendirme kodu kullanıcı için alabilirsiniz. Web uygulaması, bir erişim belirteci edinmek ve Azure ad uygulaması ile ilişkili yetkilendirme kodunu ve istemci kimlik bilgilerini kullanarak bir kullanıcı adına belirtecini yenilemek için ADAL sonra kullanabilirsiniz. Web uygulamasına erişim belirteci elinde eklendiğinde, belirteç süresi dolana kadar web API'si çağırabilirsiniz. Belirtecin süresi dolduğunda, web uygulamasının daha önce alan yeni bir erişim belirteci yenileme belirteci almak için ADAL kullanabilirsiniz. Bu senaryoyu gösteren bir kod örneği için bkz. [Web API'si için Web API'si için yerel istemci](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof).

## <a name="see-also"></a>Ayrıca Bkz.

- [Azure Active Directory Geliştirici Kılavuzu](v1-overview.md)
- [Azure Active directory için kimlik doğrulama senaryoları](authentication-scenarios.md)
- [Azure Active Directory kod örnekleri](sample-v1-code.md)
