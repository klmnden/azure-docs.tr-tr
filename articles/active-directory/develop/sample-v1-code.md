---
title: Azure Active Directory v1.0 kod örnekleri | Microsoft Docs
description: Azure Active Directory (v1.0 uç nokta) dizinini senaryoya göre düzenlenen, kod örnekleri sağlar.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca03c8d52de1580b755833dd990d1d7671f372ca
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67785709"
---
# <a name="azure-active-directory-code-samples-v10-endpoint"></a>Azure Active Directory kod örnekleri (v1.0 uç noktası)

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Microsoft Azure Active Directory (Azure AD) kimlik doğrulama ve yetkilendirme, web uygulamaları ve web API'leri eklemek için kullanabilirsiniz.

Bu bölümde, Azure AD v1.0 uç noktası hakkında daha fazla bilgi edinmek için kullanabileceğiniz örneklere bağlantılar sağlar. Bu örnekler, uygulamalarınızda kullanabileceğiniz kod parçacıkları birlikte nasıl yapıldığını gösterir. Kod örnek sayfada ayrıntılı okuma bulabilirsiniz-bana gereksinimleri, yükleme ve Kurulum Yardım konuları. Ve kod kritik bölümler anlamanıza yardımcı olması için yorum yaptı.

> [!NOTE]
> Azure AD V2 kod örnekleri ilgileniyorsanız bkz [senaryoya göre v2.0 kod örnekleri](sample-v2-code.md).

Her örnek türü için temel senaryo anlamak için bkz: [Azure AD için kimlik doğrulama senaryoları](authentication-scenarios.md).

Ayrıca örneklerimizi github'da katkıda bulunabilir. Bilgi edinmek için bkz. nasıl [Microsoft Azure Active Directory örnekler ve belgeler](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="single-page-applications"></a>Tek sayfa uygulamaları

Bu örnek, Azure AD ile güvenli hale getirilmiş bir tek sayfalı uygulama yazma işlemi gösterilmektedir.

 Platform | Kendi API çağrıları | Başka bir Web API'sini çağırır
 -------- |  --------------------- | ------------------ 
![Bu görüntü JavaScript logosu gösterir](media/sample-v2-code/logo_js.png) | [JavaScript singlepageapp](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |
![Bu görüntü Angular JS logosu gösterir](media/sample-v2-code/logo_angular.png) | [angularjs singlepageapp](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | [angularjs singlepageapp cors](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp-dotnet-webapi)

## <a name="web-applications"></a>Web Uygulamaları

### <a name="web-applications-signing-in-users-calling-microsoft-graph-or-a-web-api-with-the-users-identity"></a>Web uygulamaları kullanıcıların imzalama, Microsoft Graph veya kullanıcının kimliği ile bir Web API'si çağırma

Aşağıdaki örnekleri kullanıcılar imzalama Web uygulamaları gösterir. Bu uygulamalardan bazıları aynı zamanda Microsoft Graph veya kendi Web API, oturum açmış kullanıcı adını arayın.

 Platform | Yalnızca kullanıcılar oturum açtığında | Çağrıları Microsoft Graph veya AAD Graph| Başka bir ASP.NET veya ASP.NET Core 2.0 Web API'sini çağırır.
 -------- | ------------------- | --------------------- | -------------------------
![Bu görüntü logo ASP.NET gösterir](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2.0 sürümüne | [DotNet webapp openıdconnect aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore) | [webapp-webapi-multitenant-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-webapp-webapi-multitenant-openidconnect-aspnetcore/) </p>(AAD Graph) | [dotnet-webapp-webapi-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore)
![Bu görüntü logo ASP.NET gösterir](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET 4.5 | [webApp openıdconnect dotnet](quickstart-v1-aspnet-webapp.md) </p> [webapp-WSFederation-dotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) </p> [dotnet-webapp-webapi-oauth2-useridentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | [DotNet webapp multitenant openıdconnect](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect)</p> (AAD Graph) |
![Bu görüntü gösterir ve Python logosu](media/sample-v2-code/logo_python.png) | | [Python webapp graphapi](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)  |
![Bu görüntü, Java günlük gösterir](media/sample-v2-code/logo_java.png)  | | [java-webapp-openidconnect](https://github.com/azure-samples/active-directory-java-webapp-openidconnect)  |
![Bu görüntü PHP logosu gösterir](media/sample-v2-code/logo_php.png) | | [PHP graphapi web](https://github.com/Azure-Samples/active-directory-php-graphapi-web)  |

### <a name="web-applications-demonstrating-role-based-access-control-authorization"></a>Rol tabanlı erişim denetimi (yetkilendirme) gösteren web uygulamaları

Aşağıdaki örnekler, rol tabanlı erişim denetimi (RBAC) uygulamak gösterilmektedir. RBAC, belirli kullanıcılara bir web uygulamasındaki bazı özelliklerin izinlerini kısıtlamak için kullanılır. Kullanıcıların mi ait oldukları bağlı olarak yetkilendirilmiş bir **Azure AD grubu** veya belirli bir uygulama **rol**.

Platform | Örnek |
 -------- | ------------------- |
![Bu görüntü logo ASP.NET gösterir](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET 4.5 | [DotNet webapp groupclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) </p>  [DotNet webapp roleclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Azure AD kullanan bir .NET 4.5 MVC web uygulaması **rolleri** için yetkilendirme

## <a name="desktop-and-mobile-public-client-applications-calling-microsoft-graph-or-a-web-api"></a>Masaüstü ve mobil genel istemci uygulamaları Microsoft Graph veya bir Web API'si çağırma

Aşağıdaki örnekleri, Microsoft Graph veya bir Web API'sini bir kullanıcı adına erişim ortak istemci uygulamaları (deskto/pmobile uygulamaları) gösterir. Cihazları ve platformları bağlı olarak, uygulamaları farklı yöntemlerle (akışlar/verir) içinde kullanıcılar oturum açabilir:

- Etkileşimli olarak
- Sessizce (tümleşik Windows kimlik doğrulaması ile Windows ya da kullanıcı adı/parola)
- Etkileşimli oturum açma yükünü başka bir cihaza (cihaz kod akışı, web denetimleri sağlamayan cihazlarda kullanılan) temsilci tarafından

İstemci uygulaması | Platform | Akış/verme | Microsoft Graph çağrıları | Bir ASP.NET veya ASP.NET Core 2.x Web API'si çağıran
------------------ | -------- | ---------- | -------------------- | -------------------------
Masaüstü (WPF)           | ![Bu görüntü .NET gösterir /C# logosu](media/sample-v2-code/logo_NET.png)  | Etkileşimli | Parçası [dotnet yerel multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) | [DotNet yerel Masaüstü](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) </p> [DotNet yerel aspnetcore](https://azure.microsoft.com/resources/samples/active-directory-dotnet-native-aspnetcore/)</p> [dotnet-webapi-manual-jwt-validation](https://github.com/azure-samples/active-directory-dotnet-webapi-manual-jwt-validation)
Mobile (UWP)            | .![Bu görüntü .NET gösterir /C#/UWP](media/sample-v2-code/logo_Windows.png)   | Etkileşimli | [DotNet yerel uwp wam](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) </p> Bu örnekte [WAM](/windows/uwp/security/web-account-manager)değil [ADAL.NET](https://aka.ms/adalnet)|  [DotNet windows mağazası](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) (tek bir kiracının Web API'sini çağırmak için ADAL.NET kullanarak UWP uygulaması) </p> [DotNet-webapı-multitenant-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) (bir çok kiracılı bir Web API'sini çağırmak için ADAL.NET kullanarak UWP uygulaması)|
Mobile (Android, iOS, UWP)   | ![Bu görüntü .NET gösterir /C# (Xamarin)](media/sample-v2-code/logo_xamarin.png) | Etkileşimli | [DotNet yerel multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) |
Mobile (Android)           | ![Bu görüntü Android logosu gösterir](media/sample-v2-code/logo_Android.png) | Etkileşimli |   [Android](https://github.com/Azure-Samples/active-directory-android) |
Mobile (iOS)           | ![Bu görüntü iOS gösterir / Objective C ya da Swift'te](media/sample-v2-code/logo_iOS.png) | Etkileşimli |   [nativeClient-iOS](https://github.com/azureadquickstarts/nativeclient-ios) |
Masaüstü (konsol)          | ![Bu görüntü .NET gösterir /C# logosu](media/sample-v2-code/logo_NET.png) | Kullanıcı adı / parola </p>  Tümleşik Windows kimlik doğrulaması | | [DotNet-yerel-gözetimsiz](https://github.com/azure-samples/active-directory-dotnet-native-headless)
Masaüstü (konsol)          | ![Bu görüntü Java logosu gösterir](media/sample-v2-code/logo_Java.png) | Kullanıcı adı / parola | | [Java-yerel-gözetimsiz](https://github.com/Azure-Samples/active-directory-java-native-headless)
Masaüstü (konsol)           | ![.NET Core bu görüntüde gösterilmektedir /C# logosu](media/sample-v2-code/logo_NETcore.png) | Cihaz kod akışı | | [DotNet deviceprofile](https://github.com/Azure-Samples/active-directory-dotnet-deviceprofile)

## <a name="daemon-applications-accessing-web-apis-with-the-applications-identity"></a>Arka plan programı uygulamalar (web API'leri ile uygulamanın kimliğine erişim)

Aşağıdaki örnekler, Microsoft Graph veya bir web API'si (uygulama kimliği ile) hiçbir kullanıcıyla erişim Masaüstü ve web uygulamaları gösterir.

İstemci uygulaması | Platform | Akış/verme | Bir ASP.NET veya ASP.NET Core 2.0 Web API'sini çağırır
------------------ | -------- | ---------- | -------------------- 
Arka plan programı uygulama (konsol)          | ![Bu görüntü .NET logosu gösterir](media/sample-v2-code/logo_NETframework.png) | Uygulama gizli anahtarı veya sertifika ile istemci kimlik bilgileri | [DotNet-arka plan programı](https://github.com/azure-samples/active-directory-dotnet-daemon)</p> [DotNet-arka plan programı-sertifika-credential](https://github.com/azure-samples/active-directory-dotnet-daemon-certificate-credential)
Arka plan programı uygulama (konsol)         | ![Bu görüntü .NET logosu gösterir](media/sample-v2-code/logo_NETcore.png) | Sertifika istemci kimlik bilgileri| [dotnetcore-arka plan programı-sertifika-credential](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-certificate-credential)
ASP.NET Web uygulaması  | ![Bu görüntü .NET logosu gösterir](media/sample-v2-code/logo_NETframework.png) | İstemci kimlik bilgileri | [dotnet-webapp-webapi-oauth2-appidentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity)

## <a name="web-apis"></a>Web API'leri

### <a name="web-api-protected-by-azure-active-directory"></a>Azure Active Directory tarafından korunan Web API'sini

Aşağıdaki örnek, bir node.js web API'si Azure AD ile korunacak gösterilmektedir.

Bu makalenin önceki kısımlarında gösteren bir istemci uygulamanın diğer örnekleri de bulabilirsiniz **çağırma** bir ASP.NET veya ASP.NET Core **Web API**. Bu örnekleri yeniden bu bölümde açıklanan değil, ancak son sütunda tablo üzerinde veya altında bulabilirsiniz

| Platform | Örnek |
|--------|-------------------|
| ![Bu görüntü Node.js logosu gösterir](media/sample-v2-code/logo_nodejs.png)  | [webapı düğümü](https://github.com/Azure-Samples/active-directory-node-webapi) |

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>Web API'sini Microsoft Graph veya başka bir Web API'si çağırma

Aşağıdaki örnekler, başka bir web API'si çağıran bir web API'si gösterir. İkinci örnek, koşullu erişim nasıl ele alınacağını gösterir.

| Platform |  Microsoft Graph çağrıları | Başka bir ASP.NET veya ASP.NET Core 2.0 Web API'sini çağırır. |
| -------- |  --------------------- | ------------------------- |
| ![Bu görüntü logo ASP.NET gösterir](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET 4.5 | [DotNet webapı onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) </p> [dotnet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) | [DotNet webapı onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) </p> [dotnet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) |

## <a name="other-microsoft-graph-samples"></a>Diğer Microsoft Graph örnekleri

Örnekler ve Azure AD ile kimlik doğrulaması dahil olmak üzere Microsoft Graph API için farklı kullanım düzenlerini göstermek öğreticiler için bkz. [Microsoft Graph topluluk örnekler ve öğreticiler](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Ayrıca bkz.

[Azure Active Directory Geliştirici Kılavuzu](v1-overview.md)

[Azure Active Directory kimlik doğrulama kitaplıkları](active-directory-authentication-libraries.md)

[Azure AD Graph API kavramsal ve başvuru](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API Yardımcısı kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
