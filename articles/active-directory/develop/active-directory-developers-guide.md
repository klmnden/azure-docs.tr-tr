---
title: Geliştiriciler için Azure Active Directory | Microsoft Docs
description: Bu makale, Azure Active Directory kullanarak Microsoft iş ve okul hesaplarında oturum açmaya genel bakış sunmaktadır.
services: active-directory
author: dstrockis
manager: mtillman
editor: ''
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 8d70f36c5e434a26fce4d6b4bd1ddefc22234ab5
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-active-directory-for-developers"></a>Geliştiriciler için Azure Active Directory
Azure Active Directory (Azure AD), geliştiricilerin Microsoft iş veya okul hesabına sahip kullanıcıların güvenli şekilde oturumunu açan uygulamalar derlemesine olanak sağlayan bir bulut kimlik hizmetidir. Azure AD, tek kiracılı, iş kolu (LOB) uygulamalarını derleyen geliştiricileri ve çok kiracılı uygulamalar geliştirmek isteyen geliştiricileri destekler. Temel oturum açmaya ek olarak Azure AD, uygulamaların hem [Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/overview) gibi Microsoft API’lerini hem de Azure AD platformunda derlenen özel API’leri çağırmasını sağlar.  Bu belgeler, OAuth2.0 ve OpenID Connect gibi sektör standardı protokolleri kullanarak Azure AD desteğini uygulamanıza nasıl ekleyeceğinizi göstermektedir. 

> [!NOTE]
> Bu sayfadaki içeriğin büyük bir kısmı, yalnızca Microsoft iş veya okul hesaplarını destekleyen Azure AD v1 uç noktası ile ilgilidir. Tüketici veya kişisel Microsoft hesaplarının oturumunu açtırmak istiyorsanız lütfen [Azure AD v2.0 uç noktası](active-directory-appmodel-v2-overview.md) ile ilgili daha fazla bilgi görüntüleyin. Azure AD v2.0 uç noktası, Azure AD hesaplarına (iş ve okul) ve kişisel Microsoft hesaplarına sahip kullanıcıların oturumunu açmak istediğiniz uygulamalar için birleştirilmiş bir geliştirici deneyimi sunar. 

| | |
| --- | --- |
|[Kimlik doğrulaması temel bilgileri](active-directory-authentication-scenarios.md) | Azure AD ile kimlik doğrulamaya giriş. |
|[Uygulama türleri](active-directory-authentication-scenarios.md#application-types-and-scenarios) | Azure AD tarafından desteklenen kimlik doğrulama senaryolarına genel bakış. |                                
                                                                              
## <a name="get-started"></a>başlarken
Aşağıdaki kılavuzlu kurulum adımları, Azure Active Directory Library (ADAL) SDK’sını kullanarak tercih ettiğiniz platformda uygulama derleme işlemi boyunca size yol gösterir. Microsoft Authentication Library (MSAL) kullanma hakkında bilgi edinmek için lütfen [Azure AD v2.0 uç noktası](active-directory-appmodel-v2-overview.md) ile ilgili belgelerimize bakın.

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobil ve masaüstü uygulamaları](./media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobil ve masaüstü uygulamaları</center> | [Genel Bakış](active-directory-authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](active-directory-devquickstarts-ios.md)<br /><br />[Android](active-directory-devquickstarts-android.md) | [.NET (WPF)](active-directory-devquickstarts-dotnet.md)<br /><br />[.NET (UWP)](active-directory-devquickstarts-windowsstore.md)<br /><br />[Xamarin](active-directory-devquickstarts-xamarin.md) | [Cordova](active-directory-devquickstarts-cordova.md) |
| <center>![Web uygulamaları](./media/active-directory-developers-guide/Web_app.png)<br />Web uygulamaları</center> | [Genel Bakış](active-directory-authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](active-directory-devquickstarts-webapp-dotnet.md)<br /><br />[Java](active-directory-devquickstarts-webapp-java.md) | [Node.js](active-directory-devquickstarts-openidconnect-nodejs.md) |  |
| <center>![Tek sayfa uygulamaları](./media/active-directory-developers-guide/SPA.png)<br />Tek sayfa uygulamaları</center> | [Genel Bakış](active-directory-authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |  |
| <center>![Web API'leri](./media/active-directory-developers-guide/Web_API.png)<br />Web API'leri</center> | [Genel Bakış](active-directory-authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[Node.js](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Hizmetten hizmete](./media/active-directory-developers-guide/Service_App.png)<br />Hizmetten hizmete</center> | [Genel Bakış](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#server-or-daemon-application-to-web-api)|  |

## <a name="how-to-guides"></a>Nasıl yapılır kılavuzları
Aşağıdaki kılavuzlar, Azure AD’deki bazı yaygın görevler boyunca size yol göstermektedir.

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


[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
