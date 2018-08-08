---
title: Geliştiriciler için Azure Active Directory | Microsoft Docs
description: Bu makale, Azure Active Directory kullanarak Microsoft iş ve okul hesaplarında oturum açmaya genel bakış sunmaktadır.
services: active-directory
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/30/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 666a677943811af05cd3403eab4887271c1f87b3
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39591219"
---
# <a name="azure-active-directory-for-developers"></a>Geliştiriciler için Azure Active Directory

Azure Active Directory (Azure AD), geliştiricilerin Microsoft iş veya okul hesabına sahip kullanıcıların güvenli şekilde oturumunu açan uygulamalar derlemesine olanak sağlayan bir bulut kimlik hizmetidir. Azure AD, tek kiracılı, iş kolu (LOB) uygulamalarını derleyen geliştiricileri ve çok kiracılı uygulamalar geliştirmek isteyen geliştiricileri destekler. Temel oturum açmaya ek olarak Azure AD, uygulamaların hem [Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/overview) gibi Microsoft API’lerini hem de Azure AD platformunda derlenen özel API’leri çağırmasını sağlar. Bu belgeler, OAuth2.0 ve OpenID Connect gibi sektör standardı protokolleri kullanarak Azure AD desteğini uygulamanıza nasıl ekleyeceğinizi göstermektedir.

> [!NOTE]
> Bu sayfadaki içeriğin büyük bir kısmı, yalnızca Microsoft iş veya okul hesaplarını destekleyen Azure AD v1.0 uç noktası ile ilgilidir. Tüketici veya kişisel Microsoft hesaplarının oturumunu açtırmak istiyorsanız [Azure AD v2.0 uç noktası](active-directory-appmodel-v2-overview.md) ile ilgili bilgileri görüntüleyin. Azure AD v2.0 uç noktası, Azure AD hesaplarına (iş ve okul) ve kişisel Microsoft hesaplarına sahip kullanıcıların oturumunu açmak isteyen uygulamalar için birleştirilmiş bir geliştirici deneyimi sunar.

| | |
| --- | --- |
|[Kimlik doğrulaması temel bilgileri](authentication-scenarios.md) | Azure AD ile kimlik doğrulamaya giriş. |
|[Uygulama türleri](authentication-scenarios.md#application-types-and-scenarios) | Azure AD tarafından desteklenen kimlik doğrulama senaryolarına genel bakış. |      
| | |

## <a name="get-started"></a>başlarken
Aşağıdaki kılavuzlu kurulum adımları, Azure AD Authentication Library (ADAL) SDK’sını kullanarak tercih ettiğiniz platformda uygulama derleme işlemi boyunca size yol gösterir. Microsoft Authentication Library (MSAL) kullanma hakkında bilgi edinmek için [Azure AD v2.0 uç noktası](active-directory-appmodel-v2-overview.md) ile ilgili belgelerimize bakın.

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobil ve masaüstü uygulamaları](./media/azure-ad-developers-guide/NativeApp_Icon.png)<br />Mobil ve masaüstü uygulamaları</center> | [Genel Bakış](authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](quickstart-v1-ios.md)<br /><br />[Android](quickstart-v1-android.md) | [.NET (WPF)](quickstart-v1-dotnet.md)<br /><br />[Xamarin](quickstart-v1-xamarin.md) |
| <center>![Web uygulamaları](./media/azure-ad-developers-guide/Web_app.png)<br />Web uygulamaları</center> | [Genel Bakış](authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](quickstart-v1-aspnet-webapp.md)<br /><br />[Java](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect) | [Python](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)<br/><br/> [Node.js](quickstart-v1-openid-connect-code.md) |
| <center>![Tek sayfa uygulamaları](./media/azure-ad-developers-guide/SPA.png)<br />Tek sayfa uygulamaları</center> | [Genel Bakış](authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](quickstart-v1-angularjs-spa.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |
| <center>![Web API'leri](./media/azure-ad-developers-guide/Web_API.png)<br />Web API'leri</center> | [Genel Bakış](authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](quickstart-v1-dotnet-webapi.md)<br /><br />[Node.js](quickstart-v1-nodejs-webapi.md) | &nbsp; |
| <center>![Hizmetten hizmete](./media/azure-ad-developers-guide/Service_App.png)<br />Hizmetten hizmete</center> | [Genel Bakış](authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](sample-v1-code.md#daemon-applications-accessing-web-apis-with-the-applications-identity)|  |
|  |  |  |  |  |

## <a name="how-to-guides"></a>Nasıl yapılır kılavuzları
Aşağıdaki kılavuzlar, Azure AD’deki bazı yaygın görevler boyunca size yol göstermektedir.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[Uygulama kaydı](quickstart-v1-integrate-apps-with-azure-ad.md)           | Bir uygulamayı Azure AD’ye kaydetme. |
|[Çok kiracılı uygulamalar](howto-convert-app-to-be-multi-tenant.md)    | Herhangi bir Microsoft iş hesabında oturum açma. |
|[OAuth ve OpenID Connect protokolleri](v1-protocols-openid-connect-code.md)| Microsoft kimlik doğrulama protokollerini kullanarak kullanıcıların oturumunu açma ve web API'lerini çağırma. |
|  |  |

## <a name="reference-topics"></a>Başvuru konuları
Aşağıdaki makaleler, Azure AD’de kullanılan API'ler, protokol iletileri ve terimler hakkında ayrıntılı bilgi sağlar.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Kimlik Doğrulama Kitaplıkları (ADAL)](active-directory-authentication-libraries.md)   | Azure AD tarafından sağlanan kitaplıklara ve SDK’lara genel bakış. |
| [Kod örnekleri](sample-v1-code.md)                                  | Tüm Azure AD kod örneklerinin listesi. |
| [Sözlük](developer-glossary.md)                                      | Bu belgelerde kullanılan terminoloji ve sözcük tanımları. |
|  |  |


[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
