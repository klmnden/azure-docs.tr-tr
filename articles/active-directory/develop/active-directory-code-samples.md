---
title: Azure Active Directory kod örnekleri | Microsoft Docs
description: Azure Active Directory (v1 uç noktası) dizinini senaryoya göre düzenlenen, kod örnekleri sağlar.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: bd71124e9134fc4d5692bb3b95a1673f111ccbde
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444394"
---
# <a name="azure-active-directory-code-samples-v1-endpoint"></a>Azure Active Directory kod örnekleri (V1 uç noktası)

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Microsoft Azure Active Directory (Azure AD) kimlik doğrulama ve yetkilendirme, web uygulamaları ve web API'leri eklemek için kullanabilirsiniz.

Bu bölümde, Azure AD V1 uç noktası hakkında daha fazla bilgi için kullanabileceğiniz örneklere bağlantılar sağlar. Bu örnekler, uygulamalarınızda kullanabileceğiniz kod parçacıkları birlikte nasıl yapıldığını gösterir. Kod örnek sayfada ayrıntılı okuma bulabilirsiniz-bana gereksinimleri, yükleme ve Kurulum Yardım konuları. Ve kod kritik bölümler anlamanıza yardımcı olması için yorum yaptı.

> [!NOTE]
> Azure AD V2 kod örnekleri ilgileniyorsanız bkz [senaryoya göre v2.0 kod örnekleri](active-directory-v2-code-samples.md).

Her örnek türü için temel senaryo anlamak için bkz: [Azure AD için kimlik doğrulama senaryoları](active-directory-authentication-scenarios.md).

Ayrıca örneklerimizi github'da katkıda bulunabilir. Bilgi edinmek için bkz. nasıl [Microsoft Azure Active Directory örnekler ve belgeler](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="desktop-and-mobile-public-client-applications-calling-microsoft-graph-or-a-web-api"></a>Masaüstü ve mobil genel istemci uygulamaları Microsoft Graph veya bir Web API'si çağırma

Aşağıdaki örnekler, genel istemci Microsoft Graph veya bir kullanıcı adını Web API'si erişen uygulamalar (masaüstü/mobil uygulamalar) gösterir.

İstemci uygulaması | Platform | Akış/verme | Microsoft Graph çağrıları | Bir ASP.NET veya ASP.NET Core 2.0 Web API'sini çağırır
------------------ | -------- | ---------- | -------------------- | -------------------------
Masaüstü (WPF)           | .NET / C# | Etkileşimli | [DotNet yerel multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) | [DotNet yerel Masaüstü](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) </p> [DotNet yerel aspnetcore](https://azure.microsoft.com/resources/samples/active-directory-dotnet-native-aspnetcore/)</p> [DotNet-webapı-el ile-jwt-doğrulama](https://github.com/azure-samples/active-directory-dotnet-webapi-manual-jwt-validation)
Mobile (UWP)            | .NET / C#  | Etkileşimli | [DotNet yerel uwp wam](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) |  [DotNet windows mağazası](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) (tek kiracılı Web API'si) </p> [DotNet-webapı-multitenant-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) (çok kiracılı Web API'si)|
Mobile (Android, iOS, UWP)   | .NET / C# (Xamarin) | Etkileşimli | [DotNet yerel multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) |
Mobile (Android)           | Android/Java | Etkileşimli |   [Android](https://github.com/Azure-Samples/active-directory-android) |
Mobile (iOS)           | iOS/Objective C | Etkileşimli |   [nativeClient-iOS](https://github.com/azureadquickstarts/nativeclient-ios) |
Masaüstü (konsol)          | .NET / C# | Kullanıcı adı / parola </p> Tümleşik Windows Kimlik Doğrulaması | | [DotNet-yerel-gözetimsiz](https://github.com/azure-samples/active-directory-dotnet-native-headless)
Masaüstü (konsol)           | .NET core / C# | Cihaz profili | | [DotNet deviceprofile](https://github.com/Azure-Samples/active-directory-dotnet-deviceprofile)

## <a name="web-applications"></a>Web Uygulamaları

### <a name="web-applications-signing-in-users-calling-microsoft-graph-or-a-web-api-with-the-users-identity"></a>Web uygulamaları kullanıcıların imzalama, Microsoft Graph veya kullanıcının kimliği ile bir Web API'si çağırma

 Platform | Yalnızca kullanıcılar oturum açtığında | Çağrıları Microsoft Graph veya AAD Graph| Başka bir ASP.NET veya ASP.NET Core 2.0 Web API'sini çağırır.
 -------- | ------------------- | --------------------- | -------------------------
ASP.NET 4.5 | [webApp openıdconnect dotnet](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-aspnetwebapp-v1) </p> [WebApp WSFederation dotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | [DotNet webapp multitenant openıdconnect](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) (AAD Graph) |
ASP.NET Core 2.0 sürümüne | [DotNet webapp openıdconnect aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore) | [WebApp-webapı-multitenant-openıdconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-webapp-webapi-multitenant-openidconnect-aspnetcore/) (AAD Graph) | [DotNet-webapp-webapı-openıdconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore)
ASP.NET 4.5 | [DotNet-webapp-webapı-oauth2-userıdentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | |
Python | | [Python webapp graphapi](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)  |
Java | | [Java webapp openıdconnect](https://github.com/azure-samples/active-directory-java-webapp-openidconnect)  |
PHP | | [PHP graphapi web](https://github.com/Azure-Samples/active-directory-php-graphapi-web)  |

### <a name="web-applications-demonstrating-role-based-access-control-authorization"></a>Rol tabanlı erişim denetimi (yetkilendirme) gösteren web uygulamaları

Aşağıdaki örnek, belirli kullanıcılara bir web uygulamasının belirli özellikleri izinlerini kısıtlamak için kullanılan rol tabanlı erişim denetimi uygulamak gösterilmektedir. Kullanıcılar, yoksa bir Azure AD grubu veya rolüne ait oldukları bağlı olarak yetkilidir.

Platform | Örnek | Açıklama
 -------- | ------------------- | ---------------------
ASP.NET 4.5 | [DotNet webapp groupclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Azure AD kullanan bir .NET 4.5 MVC web uygulaması **grupları** için yetkilendirme
ASP.NET 4.5 | [DotNet webapp roleclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Azure AD kullanan bir .NET 4.5 MVC web uygulaması **rolleri** için yetkilendirme

## <a name="daemon-applications-accessing-web-apis-with-the-applications-identity"></a>Arka plan programı uygulamalar (Web API'leri ile uygulamanın kimliğine erişim)

Aşağıdaki örnekler, Microsoft Graph veya bir web API'si (uygulama kimliği ile) hiçbir kullanıcıyla erişim Masaüstü ve web uygulamaları gösterir.

İstemci uygulaması | Platform | Akış/verme | Microsoft Graph çağrıları | Bir ASP.NET veya ASP.NET Core 2.0 Web API'sini çağırır
------------------ | -------- | ---------- | -------------------- | -------------------------
Arka plan programı uygulama (konsol)          | .NET / C#  | Uygulama gizli anahtarı veya sertifika ile istemci kimlik bilgileri | | [DotNet-arka plan programı](https://github.com/azure-samples/active-directory-dotnet-daemon)</p> [DotNet-arka plan programı-sertifika-credential](https://github.com/azure-samples/active-directory-dotnet-daemon-certificate-credential)
Arka plan programı uygulama (konsol)         | .NET core / C# | Sertifika istemci kimlik bilgileri| | [dotnetcore-arka plan programı-sertifika-credential](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-certificate-credential)
Masaüstü            | Java | İstemci kimlik bilgileri |   [Java-yerel-gözetimsiz](https://github.com/azure-samples/active-directory-java-native-headless) |
ASP.NET Web uygulaması  | .NET / C# | İstemci kimlik bilgileri |    | [DotNet-webapp-webapı-oauth2-appidentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity)

## <a name="web-apis"></a>Web API'leri

### <a name="web-api-protected-by-azure-active-directory"></a>Azure Active Directory tarafından korunan Web API'sini

Aşağıdaki örnek, bir node.js web API'si Azure AD ile korunacak gösterilmektedir.

Platform | Örnek | Açıklama
 -------- | ------------------- | ---------------------
Node.js | [webapı düğümü](https://github.com/Azure-Samples/active-directory-node-webapi) |  NodeJS web API'si Azure AD kullanarak güvenli hale getirilen ve OAuth 2.0 erişim belirteçleri.

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>Web API'sini Microsoft Graph veya başka bir Web API'si çağırma

Aşağıdaki örnekler, başka bir web API'si çağıran bir web API'si gösterir. İkinci örnek, koşullu erişim nasıl ele alınacağını gösterir.

 Platform |  Microsoft Graph çağrıları | Başka bir ASP.NET veya ASP.NET Core 2.0 Web API'sini çağırır.
 -------- |  --------------------- | -------------------------
ASP.NET 4.5 | [DotNet webapı onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) |[DotNet webapı onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof)
ASP.NET 4.5 | [DotNet-webapı-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) |[DotNet-webapı-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca)

## <a name="single-page-applications"></a>Tek sayfa uygulamaları

Bu örnek, Azure AD ile güvenli hale getirilmiş bir tek sayfalı uygulama yazma işlemi gösterilmektedir.

 Platform |  Microsoft Graph çağrıları | Kendi API çağrıları | Başka bir Web API'sini çağırır
 -------- |  --------------------- | ------------------ | ----------------
JavaScript / ASP.NET 4.x |  | [JavaScript singlepageapp](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |
JavaScript (AngularJS) / ASP.NET 4.x |  | [angularjs singlepageapp](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) |
JavaScript (AngularJS) / ASP.NET 4.x |  |  | [angularjs singlepageapp cors](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp-dotnet-webapi)

## <a name="other-microsoft-graph-samples"></a>Diğer Microsoft Graph örnekleri

Örnekler ve Azure AD ile kimlik doğrulaması dahil olmak üzere Microsoft Graph API için farklı kullanım düzenlerini göstermek öğreticiler için bkz. [Microsoft Graph topluluk örnekler ve öğreticiler](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Ayrıca bkz.

[Azure Active Directory Geliştirici Kılavuzu](azure-ad-developers-guide.md)

[Azure AD Graph API kavramsal ve başvuru](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API Yardımcısı kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
