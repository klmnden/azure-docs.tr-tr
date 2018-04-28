---
title: Azure Active Directory kod örnekleri | Microsoft Docs
description: Azure Active Directory (v1 uç noktası) dizini senaryo tarafından düzenlenen kod örnekleri sağlar.
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
ms.date: 04/24/2018
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 046d633239b09ebdf21b5383b08684d81199d026
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-active-directory-code-samples-v1-endpoint"></a>Azure Active Directory kod örnekleri (V1 uç noktası)

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Microsoft Azure Active Directory (Azure AD), kimlik doğrulama ve yetkilendirme, web uygulamaları ve web API'leri eklemek için kullanabilirsiniz.

Bu bölümde, Azure AD V1 uç noktası hakkında daha fazla bilgi için kullanabileceğiniz örnek bağlantılar sağlar. Bu örnekler, uygulamalarınızda kullanabileceğiniz kod parçacıkları birlikte nasıl yapıldığını gösterir. Ayrıntılı okuma Bul kod örnek sayfada-bana gereksinimleri, yükleme ve Kurulum Yardım konuları. Ve kritik bölümler anlamanıza yardımcı olması için kod açıklanır.

> [!NOTE]
> Azure AD V2 kod örnekleri ilgileniyorsanız bkz [v2.0 kod örnekleri senaryo tarafından](active-directory-v2-code-samples.md).

Her bir örnek türü için temel senaryo anlamak için bkz: [Azure AD için kimlik doğrulama senaryoları](active-directory-authentication-scenarios.md).

Ayrıca bizim örneklerimizi github'da katkıda bulunabilirsiniz. Bilgi edinmek için bkz [Microsoft Azure Active Directory örnekler ve belgeler](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="desktop-and-mobile-public-client-applications-calling-microsoft-graph-or-a-web-api"></a>Microsoft Graph veya bir Web API çağırma Masaüstü ve mobil ortak istemci uygulamaları

Aşağıdaki örnekler ortak istemci Microsoft Graph veya bir kullanıcı adına bir Web API erişim uygulamaları (masaüstü/mobil uygulamaları) gösterir.

istemci uygulaması | Platform | Akış/verin | Çağrıları Microsoft Graph | Bir ASP.NET veya ASP.NET Core 2.0 Web API çağrıları
------------------ | -------- | ---------- | -------------------- | -------------------------
Masaüstü (WPF)           | .NET / C# | Etkileşimli | [DotNet yerel multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) | [DotNet yerel Masaüstü](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) </p> [DotNet yerel aspnetcore](https://azure.microsoft.com/resources/samples/active-directory-dotnet-native-aspnetcore/)</p> [DotNet webapı-el ile-jwt-doğrulama](https://github.com/azure-samples/active-directory-dotnet-webapi-manual-jwt-validation)
Mobil (UWP)            | .NET / C#  | Etkileşimli | [Yerel uwp wam dotnet](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) |  [DotNet windows mağazası](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) (tek kiracılı Web API) </p> [DotNet webapı-çok kullanıcılı-windows-deposu](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) (çok kiracılı Web API)|
Mobil (Android, iOS, UWP)   | .NET / C# (Xamarin) | Etkileşimli | [DotNet yerel multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) |
Mobil (Android)           | Android/Java | Etkileşimli |   [Android](https://github.com/Azure-Samples/active-directory-android) |
Mobil (iOS)           | iOS/Objective C | Etkileşimli |   [nativeClient iOS](https://github.com/azureadquickstarts/nativeclient-ios) |
Masaüstü (konsol)          | .NET / C# | Kullanıcı adı / parola </p> Windows tümleşik kimlik doğrulaması | | [DotNet-yerel-gözetimsiz](https://github.com/azure-samples/active-directory-dotnet-native-headless)
Masaüstü (konsol)           | .NET core / C# | Cihaz profili | | [DotNet deviceprofile](https://github.com/Azure-Samples/active-directory-dotnet-deviceprofile)

## <a name="web-applications"></a>Web Uygulamaları

### <a name="web-applications-signing-in-users-calling-microsoft-graph-or-a-web-api-with-the-users-identity"></a>Kullanıcılar imzalama, Microsoft Graph veya kullanıcının kimliğine sahip bir Web API çağırma web uygulamaları

 Platform | Yalnızca kullanıcılar işaretleri | Çağrıları Microsoft Graph veya AAD grafik| Başka bir ASP.NET veya ASP.NET Core 2.0 Web API çağrıları
 -------- | ------------------- | --------------------- | -------------------------
ASP.NET 4.5 | [webApp openıdconnect dotnet](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-aspnetwebapp-v1) </p> [WebApp WSFederation dotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | [webapp çok kullanıcılı openıdconnect dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) (AAD grafik) |
ASP.NET Core 2.0 | [webapp openıdconnect aspnetcore dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore) | [WebApp-webapı-çok kullanıcılı-openıdconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-webapp-webapi-multitenant-openidconnect-aspnetcore/) (AAD grafik) | [DotNet-webapp-webapı-openıdconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore)
ASP.NET 4.5 | [DotNet-webapp-webapı-oauth2-userıdentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | |
Python | | [Python webapp graphapi](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)  |
Java | | [Java webapp openıdconnect](https://github.com/azure-samples/active-directory-java-webapp-openidconnect)  |
PHP | | [PHP graphapi web](https://github.com/Azure-Samples/active-directory-php-graphapi-web)  |

### <a name="web-applications-demonstrating-role-based-access-control-authorization"></a>Rol tabanlı erişim denetimi (yetkilendirme) gösteren web uygulamaları

Aşağıdaki örnekler, belirli kullanıcılara bir web uygulaması'nin belirli özelliklerini izinlerini kısıtlamak için kullanılan rol tabanlı erişim denetimi gösterilmektedir. Kullanıcıların olup bir Azure AD grubu veya rolüne ait oldukları bağlı olarak yetkisine sahiptir.

Platform | Örnek | Açıklama
 -------- | ------------------- | ---------------------
ASP.NET 4.5 | [DotNet webapp groupclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Azure AD kullanan bir .NET 4.5 MVC web uygulaması **grupları** yetkilendirme için
ASP.NET 4.5 | [DotNet webapp roleclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Azure AD kullanan bir .NET 4.5 MVC web uygulaması **rolleri** yetkilendirme için

## <a name="daemon-applications-accessing-web-apis-with-the-applications-identity"></a>Arka plan programı uygulamaları (uygulama kimliği ile Web API erişme)

Aşağıdaki örnekler, Microsoft Graph veya web API'si kullanıcı olmadan (uygulama kimliği) ile erişim Masaüstü veya web uygulamaları gösterir.

istemci uygulaması | Platform | Akış/verin | Çağrıları Microsoft Graph | Bir ASP.NET veya ASP.NET Core 2.0 Web API çağrıları
------------------ | -------- | ---------- | -------------------- | -------------------------
Arka plan programı uygulama (konsol)          | .NET / C#  | Uygulama gizli anahtarı veya sertifika ile istemci kimlik bilgileri | | [DotNet arka plan programı](https://github.com/azure-samples/active-directory-dotnet-daemon)</p> [DotNet-arka plan programı-sertifika-credential](https://github.com/azure-samples/active-directory-dotnet-daemon-certificate-credential)
Arka plan programı uygulama (konsol)         | .NET core / C# | Sertifika ile istemci kimlik bilgileri| | [arka plan programı sertifika kimlik dotnetcore](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-certificate-credential)
Masaüstü            | Java | İstemci kimlik bilgileri |   [Java-yerel-gözetimsiz](https://github.com/azure-samples/active-directory-java-native-headless) |
ASP.NET Web uygulaması  | .NET / C# | İstemci kimlik bilgileri |    | [DotNet-webapp-webapı-oauth2-appidentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity)

## <a name="web-apis"></a>Web API'leri

### <a name="web-api-protected-by-azure-active-directory"></a>Web API Azure Active Directory tarafından korunan

Aşağıdaki örnek, bir node.js web API'si Azure AD ile korunacak gösterilmiştir.

Platform | Örnek | Açıklama
 -------- | ------------------- | ---------------------
Node.js | [düğüm webapı](https://github.com/Azure-Samples/active-directory-node-webapi) |  NodeJS web güvenliğinin Azure AD ile API ve OAuth 2.0 erişim belirteçleri.

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>Web API Microsoft Graph veya başka bir Web API çağırma

Aşağıdaki örnekler, başka bir web API'si çağıran bir web API gösterir. İkinci örnek koşullu erişim nasıl ele alınacağını gösterir.

 Platform |  Çağrıları Microsoft Graph | Başka bir ASP.NET veya ASP.NET Core 2.0 Web API çağrıları
 -------- |  --------------------- | -------------------------
ASP.NET 4.5 | [DotNet webapı onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) |[DotNet webapı onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof)
ASP.NET 4.5 | [DotNet webapı onbehalfof ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) |[DotNet webapı onbehalfof ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca)

## <a name="single-page-applications"></a>Tek sayfa uygulamaları

Bu örnek, Azure AD ile güvenli bir tek sayfa uygulamasının nasıl yazılacağını göstermektedir.

 Platform |  Çağrıları Microsoft Graph | Kendi API çağrıları
 -------- |  --------------------- | -------------------------
JavaScript (Açısal) / ASP.NET 4.x |  | [angularjs singlepageapp](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp)

## <a name="other-microsoft-graph-samples"></a>Diğer Microsoft Graph örnekleri

Örnekler ve Azure AD ile kimlik doğrulaması dahil Microsoft Graph API için farklı kullanım desenlerini göstermek öğreticiler için bkz: [Microsoft Graph topluluk örnekleri & öğreticiler](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Ayrıca bkz.

[Azure Active Directory Geliştirici Kılavuzu](active-directory-developers-guide.md)

[Azure AD Graph API kavramsal ve başvurusu](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API Yardımcısı kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
