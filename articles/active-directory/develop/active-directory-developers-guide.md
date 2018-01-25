---
title: "Geliştiriciler için Azure Active Directory | Microsoft Docs"
description: "Bu makale, Azure Active Directory kullanarak Microsoft iş ve okul hesaplarında oturum açmaya genel bakış sunmaktadır."
services: active-directory
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 89a232af6387f6403e6e341cced16d06e9979dae
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
---
# <a name="azure-active-directory-for-developers"></a>Geliştiriciler için Azure Active Directory
Azure Active Directory (Azure AD), geliştiricilerin Microsoft iş veya okul hesabına sahip kullanıcılara güvenli bir şekilde oturum açtırmasını sağlayan bir bulut kimlik hizmetidir. Bu belgeler, sektör standardı protokolleri olan OAuth2.0 ve OpenID Connect’i kullanarak Azure AD desteğini uygulamanıza nasıl ekleyeceğinizi göstermektedir.

| | |
| --- | --- |
|[Kimlik doğrulaması temel bilgileri](active-directory-authentication-scenarios.md) | Azure AD ile kimlik doğrulamaya giriş. |
|[Uygulama türleri](active-directory-authentication-scenarios.md#application-types-and-scenarios) | Azure AD tarafından desteklenen kimlik doğrulama senaryolarına genel bakış. |                                
                                                                              
## <a name="get-started"></a>başlarken
Bu kılavuzlu ayarlar, Azure AD kullanıcılarının oturumunu açmak için Microsoft kimlik doğrulama kitaplıklarını kullanma işleminde size yol gösterir.

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobil ve masaüstü uygulamaları](./media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobil ve masaüstü uygulamaları</center> | [Genel Bakış](active-directory-authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](active-directory-devquickstarts-ios.md)<br /><br />[Android](active-directory-devquickstarts-android.md) | [.NET (WPF)](active-directory-devquickstarts-dotnet.md)<br /><br />[.NET (UWP)](active-directory-devquickstarts-windowsstore.md)<br /><br />[Xamarin](active-directory-devquickstarts-xamarin.md) | [Cordova](active-directory-devquickstarts-cordova.md) |
| <center>![Web uygulamaları](./media/active-directory-developers-guide/Web_app.png)<br />Web uygulamaları</center> | [Genel Bakış](active-directory-authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](active-directory-devquickstarts-webapp-dotnet.md)<br /><br />[Java](active-directory-devquickstarts-webapp-java.md) | [Node.js](active-directory-devquickstarts-openidconnect-nodejs.md) |  |
| <center>![Tek sayfa uygulamaları](./media/active-directory-developers-guide/SPA.png)<br />Tek sayfa uygulamaları</center> | [Genel Bakış](active-directory-authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |  |
| <center>![Web API'leri](./media/active-directory-developers-guide/Web_API.png)<br />Web API'leri</center> | [Genel Bakış](active-directory-authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[Node.js](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Hizmetten hizmete](./media/active-directory-developers-guide/Service_App.png)<br />Hizmetten hizmete</center> | [Genel Bakış](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#server-or-daemon-application-to-web-api)|  |

## <a name="how-to-guides"></a>Nasıl yapılır kılavuzları
Bu makaleler Azure AD ile genel görevlerin nasıl gerçekleştirileceği konusunda bilgi verir.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[Uygulama kaydı](active-directory-integrating-applications.md)           | Bir uygulamayı Azure AD’ye kaydetme. |
|[Çok kiracılı uygulamalar](active-directory-devhowto-multi-tenant-overview.md)    | Herhangi bir Microsoft iş hesabında oturum açma. |
|[OAuth ve OpenID Connect protokolleri](active-directory-protocols-openid-connect-code.md)| Microsoft kimlik doğrulama protokollerini kullanarak kullanıcıların oturumunu açma ve web API'lerini çağırma. |
|[Ek kılavuzlar](active-directory-developers-guide-index.md#guides)        |  Azure AD için kullanılabilir kılavuzların listesi.   |

## <a name="reference-topics"></a>Başvuru konuları
Bu makaleler, Azure AD’de kullanılan API'ler, protokol iletileri ve terimler hakkında ayrıntılı bilgi sağlar.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Kimlik Doğrulama Kitaplıkları (ADAL)](active-directory-authentication-libraries.md)   | Azure AD tarafından sağlanan kitaplıklara ve SDK’lara genel bakış. |
| [Kod örnekleri](active-directory-code-samples.md)                                  | Tüm Azure AD kod örneklerinin listesi. |
| [Sözlük](active-directory-dev-glossary.md)                                      | Bu belgelerde kullanılan terminoloji ve sözcük tanımları. |
| [Ek başvuru konuları](active-directory-developers-guide-index.md#reference)| Azure AD için kullanılabilir başvuru konuları.   |


> [!NOTE]
> Kişisel Microsoft hesaplarınızda oturum açmanız gerekirse, [Azure AD v2.0 uç noktasını](active-directory-appmodel-v2-overview.md) kullanmayı düşünebilirsiniz. Azure AD v2.0 uç noktası, kişisel Microsoft hesapları ile Microsoft iş hesaplarının (Azure AD’den) tek bir kimlik doğrulama sisteminde birleşmesidir.


[!INCLUDE  [Help and support](../../../includes/active-directory-develop-help-support-include.md)]