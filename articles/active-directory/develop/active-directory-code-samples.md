---
title: "Azure Active Directory kod örnekleri | Microsoft Docs"
description: "Azure Active Directory kod örnekleri, senaryo tarafından düzenlenen bir dizini."
services: active-directory
documentationcenter: dev-center-name
author: msmbaldwin
manager: mtillman
editor: 
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: mbaldwin
ms.custom: aaddev
ms.openlocfilehash: 5f47f03594e64281b55161edb1c391ed0be83a73
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="azure-active-directory-code-samples"></a>Azure Active Directory kod örnekleri
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Microsoft Azure Active Directory (Azure AD), kimlik doğrulama ve yetkilendirme, web uygulamaları ve web API'leri eklemek için kullanabilirsiniz. Bu bölümde nasıl yapıldığını göster örnekleri ve uygulamalarınızda kullanabileceğiniz kod parçacıkları bağlar. Ayrıntılı okuma Bul kod örnek sayfada-bana gereksinimleri, yükleme ve Kurulum Yardım konuları. Ve kritik bölümler anlamanıza yardımcı olması için kod açıklanır.

Her bir örnek türü için temel senaryo anlamak için Azure AD kimlik doğrulama senaryoları bakın.

Bizim örneklerimizi github'da katkıda: [Microsoft Azure Active Directory örnekler ve belgeler](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-to-web-application"></a>Web uygulaması için Web tarayıcısı
Bu örnekler, Azure AD ile oturum açmak için kullanıcının tarayıcısına yönlendiren bir web uygulamasının nasıl yazılacağını gösterir.

| Dil/Platform | Örnek | Açıklama |
| --- | --- | --- |
| C#/.NET |[WebApp-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) |Openıd Connect (ASP.Net Openıd Connect OWIN ara yazılımı) Azure AD kiracısı kullanıcıların kimliğini doğrulamak için kullanın. |
| C#/.NET |[WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) |Çok kiracılı Openıd Connect (ASP.Net Openıd Connect OWIN ara yazılımı) kullanıcıların kimliklerini doğrulamak için birden çok Azure AD kiracılardan kullanan .NET MVC web uygulaması. |
| C#/.NET |[WebApp-WSFederation-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) |WS-Federasyon (ASP.Net WS-Federasyon OWIN ara yazılımı) kullanıcıların kimliklerini doğrulamak için bir Azure AD kiracısı kullanın. |

## <a name="single-page-application-spa"></a>Tek sayfalı uygulama (SPA)
Bu örnek, Azure AD ile güvenli bir tek sayfa uygulamasının nasıl yazılacağını göstermektedir.  

| Dil/Platform | Örnek | Açıklama |
| --- | --- | --- |
| JavaScript, C#/.NET |[SinglePageApp-DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) |Bir ASP.NET web API'si arka uç ile uygulanan bir AngularJS tabanlı tek sayfalı uygulama güvenliğini sağlamak için JavaScript ve Azure AD için ADAL kullanır. |

## <a name="native-application-to-web-api"></a>Web API yerel uygulama
Bu kod örnekleri web Azure AD tarafından güvenliği sağlanan API'leri çağırma yerel istemci uygulamalarının nasıl oluşturulacağını gösterir. Kullandıkları [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) ve [OAuth 2.0 Azure AD'de](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Dil/Platform | Örnek | Açıklama |
| --- | --- | --- |
| Javascript |[NativeClient-MultiTarget-Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) |Apache Cordova için ADAL eklentisi web API'si çağıran ve Azure AD kimlik doğrulaması için kullandığı bir Apache Cordova uygulamanızı oluşturmak için kullanın. |
| C#/.NET |[NativeClient-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) |Azure AD kullanarak bir web API'si çağıran bir .NET WPF uygulaması korunmaktadır. |
| C#/.NET |[NativeClient-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) |Web API'si çağıran bir Windows mağazası uygulaması, Azure AD ile güvenli hale getirilir. |
| C#/.NET |[NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) |Çok kiracılı web Azure AD ile güvenli hale getirilen API'si çağrılırken bir Windows mağazası uygulaması. |
| C#/.NET |[WebAPI-OnBehalfOf-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) |Bir web özgün kullanıcı adına hareket etmesi için bir belirteç alır ve ardından başka bir web API'sini çağırmak için belirteci kullanır, API çağıran bir yerel istemci uygulaması. |
| C#/.NET |[NativeClient-WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) |Web API'si çağıran bir Windows Phone 8.1 için Windows mağazası uygulaması, Azure AD tarafından korunmaktadır. |
| ObjC |[NativeClient-iOS](https://github.com/Azure-Samples/active-directory-ios) |Web API'si çağıran bir iOS uygulamasına Azure AD için kimlik doğrulaması gerektirir. |
| C#/.NET |[WebAPI-ManuallyValidateJwt-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) |OWIN ara yazılımı kullanmak yerine Web API'de, bir JWT belirteci işlemek için mantığı içeren bir yerel istemci uygulaması. |
| C#/Xamarin |[NativeClient-Xamarin-Android](https://github.com/Azure-Samples/active-directory-xamarin-android) |Xamarin için bir bağlama için yerel Azure AD Authentication Library (ADAL) Android kitaplığıdır. |
| C#/Xamarin |[NativeClient-Xamarin-iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) |İOS için Xamarin bağlama için yerel Azure AD Authentication Library (ADAL). |
| C#/Xamarin |[NativeClient-MultiTarget-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) |Beş platformları hedefler ve web API'si çağıran bir Xamarin projeyi Azure AD tarafından korunmaktadır. |
| C#/.NET |[NativeClient-Headless-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) |Etkileşimli olmayan kimlik doğrulaması gerçekleştiren ve bir web API'si çağıran bir yerel uygulamayı Azure AD tarafından korunmaktadır. |

## <a name="web-application-to-web-api"></a>Web uygulaması Web API'si
Bu kod örnekleri nasıl kullanılacağı gösterilmektedir [OAuth 2.0 Azure AD'de](https://msdn.microsoft.com/library/azure/dn645545.aspx) bu çağrı web Azure AD tarafından güvenliği sağlanan API'leri web uygulamaları geliştirin.

| Dil/Platform | Örnek | Açıklama |
| --- | --- | --- |
| C#/.NET |[WebApp-WebAPI-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) |Oturum açmış kullanıcının izinleriyle web API'si çağırma. |
| C#/.NET |[WebApp-WebAPI-OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) |Uygulamanın izinlerle web API'si çağırma. |
| C#/.NET |[WebApp-WebAPI-OAuth2-UserIdentity-Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) |Yetkilendirme ile ekleme [OAuth 2.0 Azure AD'de](https://msdn.microsoft.com/library/azure/dn645545.aspx) web API'si çağırabilirsiniz şekilde bir var olan bir web uygulaması için. |
| JavaScript |[Webapı Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) |API koruma için Azure AD ile tümleşik bir REST API hizmeti ayarlayın. Bir Web API ile bir Node.js sunucusu içerir. |
| C#/.NET |[WebApp-Webapı-çok kullanıcılı-Openıdconnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |Çok kiracılı MVC web Openıd Connect (ASP.Net Openıd Connect OWIN ara yazılımı) kullanıcıların kimliklerini doğrulamak için bir Azure AD kiracısı kullanan uygulama. Grafik API'sini çağırmak için yetki kodunu kullanır. |

## <a name="server-or-daemon-application-to-web-api"></a>Sunucu veya Web API'si arka plan programı uygulamaya
Bu kod örnekleri kullanarak bir web API kaynakları alır arka plan programı veya sunucu uygulamasının nasıl oluşturulacağını Göster [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) ve [OAuth 2.0 Azure AD'de](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Dil/Platform | Örnek | Açıklama |
| --- | --- | --- |
| C#/.NET |[Daemon-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) |Bir konsol uygulaması web API'si çağırır. İstemci kimlik bilgileri bir parola değil. |
| C#/.NET |[Daemon-CertificateCredential-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) |Web API'si çağıran bir konsol uygulaması. İstemci kimlik bilgileri bir sertifikadır. |

## <a name="calling-microsoft-graph-api"></a>Microsoft Graph API çağırma
Bu kod örnekleri, dizin verilerini okuma ve yazma için Microsoft Graph API çağrısı uygulamalarının nasıl oluşturulacağını gösterir.

| Dil/Platform | Örnek | Açıklama |
| --- | --- | --- |
| C#/.NET |[WebApp-MSGraphAPI-DotNet](https://github.com/microsoftgraph/aspnet-snippets-sample) |Azure AD directory verilere erişmek için Microsoft Graph API'si kullanan bir web uygulaması. |
| C#/.NET |[UWPApp-MSGraphAPI-DotNet](https://github.com/microsoftgraph/uwp-csharp-snippets-sample) |Bu Evrensel Windows platformu uygulama Microsoft Azure Active Directory (AD) dahil olmak üzere birden çok kaynaklara erişmek gösterilmiştir ve Office 365 API'leri yaparak bir Windows 10 uygulamasında Microsoft Graph API ister. |

## <a name="calling-azure-ad-graph-api"></a>Azure AD grafik API'si çağırma
Bu kod örnekleri, dizin verilerini okuma ve yazma için Azure AD Graph API çağrısı uygulamalarının nasıl oluşturulacağını gösterir.

| Dil/Platform | Örnek | Açıklama |
| --- | --- | --- |
| Java |[WebApp-GraphAPI-Java](https://github.com/Azure-Samples/active-directory-java-graphapi-web) |Azure AD directory verilere erişmek için grafik API'sini kullanan bir web uygulaması. |
| PHP |[WebApp-GraphAPI-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) |Azure AD directory verilere erişmek için grafik API'sini kullanan bir web uygulaması. |
| C#/.NET |[ConsoleApp-GraphAPI-DiffQuery-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) |Azure AD kiracısı kullanıcı nesnelerine fark sorgu düzenli almak için grafik API'sini kullanan bir konsol uygulaması değiştirir. |
| C#/.NET |[WebApp GraphAPI DirectoryExtensions DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) |Bir MVC uygulaması grafik API'si sorguları basit şirket kuruluş hesap oluşturmak için kullanılır. |
| PHP |[WebApp GraphAPI DirectoryExtensions PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) |Uzantı kaydedin ve ardından okuma, güncelleştirme ve uzantı öznitelik değerleri silmek için grafik API'si çağıran bir PHP uygulaması. |

## <a name="authorization"></a>Yetkilendirme
Bu kod örnekleri, yetkilendirme için Azure AD kullanmayı gösterir.

| Dil/Platform | Örnek | Açıklama |
| --- | --- | --- |
| C#/.NET |[WebApp-GroupClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) |Rol tabanlı erişim denetimi (RBAC) Azure AD ile tümleşik bir uygulamada Azure Active Directory grup talepleri kullanarak gerçekleştirin. |
| C#/.NET |[WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) |Azure Active Directory uygulama rolleri Azure AD ile tümleşik bir uygulama kullanarak rol tabanlı erişim denetimi (RBAC) gerçekleştirin. |

## <a name="legacy-walkthroughs"></a>Eski izlenecek yollar
Bu izlenecek yollar biraz daha eski teknolojiyi kullanır, ancak yine de ilgi olabilir.

| Dil/Platform | Örnek | Açıklama |
| --- | --- | --- |
| C#/.NET |[Microsoft Azure AD uygulama rolü ve ACL tabanlı yetkilendirme](http://go.microsoft.com/fwlink/?LinkId=331694) |Rol tabanlı yetkilendirme (RBAC) ve ACL tabanlı bir yetkilendirme Azure AD ile tümleşik bir uygulamada gerçekleştirin. |
| C#/.NET |[AAL - Windows mağazası uygulaması - REST hizmeti için kimlik doğrulaması](http://go.microsoft.com/fwlink/?LinkId=330605) |Kullanım [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) (önceki adıyla AAL) için Windows mağazası uygulaması için kullanıcı kimlik doğrulama özellikleri eklemek Windows mağazası Beta. |
| C#/.NET |[Tarayıcı iletişim kutusu üzerinden aad'ye ADAL - REST hizmeti için yerel uygulama - kimlik doğrulama](http://go.microsoft.com/fwlink/?LinkId=259814) |Kullanım [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) bir WPF istemciye kullanıcı kimlik doğrulama özellikleri eklemek için. |
| C#/.NET |[Tarayıcı iletişim kutusu üzerinden ACS ile - REST hizmeti için yerel uygulama - ADAL kimlik doğrulaması](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |Kullanım [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) ve [erişim denetimi hizmeti 2.0 (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) bir WPF istemciye kullanıcı kimlik doğrulama özellikleri eklemek için. |
| C#/.NET |[ADAL - sunucu kimlik doğrulaması](http://go.microsoft.com/fwlink/?LinkId=259816) |Kullanım [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) MVC4 Web API REST hizmeti için bir sunucu tarafı işleminden hizmeti çağrıları güvenli hale getirmek için. |
| C#/.NET |[Azure AD kullanarak, Web uygulamanıza oturum açma ekleme](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) |Web çoklu oturum açma Azure AD kuruluş dizininizin karşı gerçekleştirmek için bir .NET uygulaması yapılandırın. |
| C#/.NET |[Azure AD ile çok Kiracılı Web uygulamaları geliştirme](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) |Azure AD çoklu oturum açma ve birden çok kuruluşta çalışmak için bir .NET uygulama dizini erişim yetenekleri eklemek için kullanın. |
| JAVA |[Azure AD grafik API'si için Java örnek uygulama](http://go.microsoft.com/fwlink/?LinkId=263969) |Grafik API'sini Azure AD directory verilerine erişmek için kullanır. |
| PHP |[Azure AD grafik API'si için PHP örnek uygulama](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) |Grafik API'sini Azure AD directory verilerine erişmek için kullanır. |
| C#/.NET |[Azure AD Graph API için örnek uygulama](http://go.microsoft.com/fwlink/?LinkID=262648) |Grafik API'sini Azure AD directory verilerine erişmek için kullanır. |
| C#/.NET |[Azure AD Graph fark sorgu için örnek uygulama](http://go.microsoft.com/fwlink/?LinkId=275398) |Fark sorgu grafik API'sini bir Azure AD Kiracı kullanıcı nesneleri düzenli değişiklikleri almak için kullanın. |
| C#/.NET |[Azure AD için çok Kiracılı bulut uygulaması tümleştirmek için örnek uygulama](http://go.microsoft.com/fwlink/?LinkId=275397) |Çok kiracılı uygulama Azure AD ile tümleştirin. |
| C#/.NET |[Bir Windows mağazası uygulaması ve Azure AD kullanarak REST Web hizmeti güvenli hale getirme](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) |Basit bir web API kaynak ve Azure AD kullanarak bir Windows mağazası istemci uygulaması oluşturma ve [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md). |
| C#/.NET |[Azure AD sorgulamak için grafik API'si kullanma](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) |Bir Azure AD Kiracı dizininden verilerine erişmek için Azure AD grafik API'si kullanan bir Microsoft .NET uygulaması yapılandırın. |

## <a name="see-also"></a>Ayrıca bkz.
##### <a name="other-resources"></a>Diğer Kaynaklar
[Azure Active Directory Geliştirici Kılavuzu](active-directory-developers-guide.md)

[Azure AD Graph API kavramsal ve başvurusu](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API Yardımcısı kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
