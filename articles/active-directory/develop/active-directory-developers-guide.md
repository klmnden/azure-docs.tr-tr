---
title: "Geliştiriciler için Azure Active Directory | Microsoft Docs"
description: "Bu makale, Azure Active Directory kullanarak Microsoft iş ve okul hesaplarında oturum açmaya genel bakış sunmaktadır."
services: active-directory
author: dstrockis
manager: mbaldwin
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
ms.translationtype: Human Translation
ms.sourcegitcommit: f7479260c7c2e10f242b6d8e77170d4abe8634ac
ms.openlocfilehash: dce813b885d492343428428056a2e8aada27b461
ms.contentlocale: tr-tr
ms.lasthandoff: 06/21/2017

---
<a id="azure-active-directory-for-developers" class="xliff"></a>

# Geliştiriciler için Azure Active Directory
Azure Active Directory, geliştiricilerin Microsoft tarafından desteklenen bir iş veya okul hesabına sahip herhangi bir kullanıcıya güvenli bir şekilde oturum açtırmasını sağlayan bir bulut kimlik hizmetidir.  Buradaki belgeler Azure AD’nin, sektör standardı kimlik doğrulama protokolleri olan OAuth ve OpenID Connect’i kullanarak uygulamanızı nasıl desteklediğini göstermektedir.

| | |
| --- | --- |
|[Auth temel bilgileri](active-directory-authentication-scenarios.md) | Azure AD ile kimlik doğrulamaya giriş |
|[Uygulama türleri](active-directory-authentication-scenarios.md#application-types-and-scenarios) | Azure AD tarafından desteklenen kimlik doğrulama senaryolarına genel bakış |                                
                                                                              
<a id="get-started" class="xliff"></a>

## başlarken
Bu kılavuzlu ayarlar, Azure Active Directory kullanıcılarının oturumunu açmak için kimlik doğrulama kitaplıklarımızı kullanma işleminde size yol gösterir.

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobil Uygulamalar ve Masaüstü Uygulamaları](./media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobil Uygulamalar ve Masaüstü Uygulamaları</center> | [Genel Bakış](active-directory-authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](active-directory-devquickstarts-ios.md)<br /><br />[Android](active-directory-devquickstarts-android.md) | [.NET](active-directory-devquickstarts-dotnet.md)<br /><br />[Windows](active-directory-devquickstarts-windowsstore.md)<br /><br />[Xamarin](active-directory-devquickstarts-xamarin.md) | [Cordova](active-directory-devquickstarts-cordova.md)<br /><br />[OAuth 2.0](active-directory-protocols-oauth-code.md) |
| <center>![Web Apps](./media/active-directory-developers-guide/Web_app.png)<br />Web Apps</center> | [Genel Bakış](active-directory-authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](active-directory-devquickstarts-webapp-dotnet.md)<br /><br />[Java](active-directory-devquickstarts-webapp-java.md) | [NodeJS](active-directory-devquickstarts-openidconnect-nodejs.md)<br /><br />[OpenID Connect 1.0](active-directory-protocols-openid-connect-code.md) |  |
| <center>![Tek Sayfa Uygulamaları](./media/active-directory-developers-guide/SPA.png)<br />Tek Sayfa Uygulamaları</center> | [Genel Bakış](active-directory-authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |  |
| <center>![Web API'leri](./media/active-directory-developers-guide/Web_API.png)<br />Web API'leri</center> | [Genel Bakış](active-directory-authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[NodeJS](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Hizmetten hizmete](./media/active-directory-developers-guide/Service_App.png)<br />Hizmetten hizmete</center> | [Genel Bakış](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#server-or-daemon-application-to-web-api)<br /><br />[OAuth 2.0 İstemci Kimlik Bilgileri](active-directory-protocols-oauth-service-to-service.md) |  |

<a id="guides" class="xliff"></a>

## Kılavuzlar
Bu makaleler Azure Active Directory ile genel görevlerin nasıl gerçekleştirileceği konusunda bilgi verir.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[Uygulama kaydı](active-directory-integrating-applications.md)           | Bir uygulamayı Azure AD’ye kaydetme |
|[Çok kiracılı uygulamalar](active-directory-devhowto-multi-tenant-overview.md)    | Herhangi bir Microsoft iş hesabında oturum açma |
|[OAuth ve OpenID Connect](active-directory-protocols-openid-connect-code.md)| Modern kimlik doğrulama protokollerimizi kullanarak kullanıcıların oturumunu açma ve web API’lerini çağırma |
|[Diğer kılavuzlar...](active-directory-developers-guide-index.md#guides)        |     |

<a id="reference" class="xliff"></a>

## Başvuru
Bu makaleler, Azure Active Directory’de kullanılan API'ler, protokol iletileri ve terimler hakkında ayrıntılı bilgi sağlar.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Kimlik Doğrulama Kitaplıkları (ADAL)](active-directory-authentication-libraries.md)   | Azure AD tarafından sağlanan kitaplıklara ve SDK’lara genel bakış |
| [Kod örnekleri](active-directory-code-samples.md)                                  | Tüm Azure AD kod örneklerinin listesi |
| [Sözlük](active-directory-dev-glossary.md)                                      | Bu belgede kullanılan terminoloji ve sözcük tanımları |
| [Diğer başvuru kaynakları...](active-directory-developers-guide-index.md#reference)|     |

<a id="help--support" class="xliff"></a>

## Yardım ve Destek
Azure Active Directory’de geliştirme konusunda yardım almak için en iyi yerler bunlardır.

|  |  
|---|
|[Stack Overflow’da `azure-active-directory` ve `adal` etiketleri](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)      |
|[Azure Active Directory geri bildirimleri](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)|
| [Microsoft Dev Chat'i (sınırlı bir süre için ücretsiz) deneyin](http://aka.ms/devchat) |

<br />

> [!NOTE]
> Kişisel Microsoft hesaplarınızda oturum açmanız gerekirse, [Azure AD v2.0 uç noktasını](active-directory-appmodel-v2-overview.md) kullanmanız gerekebilir.  Azure AD v2.0 uç noktası, kişisel Microsoft hesapları ile Microsoft iş hesaplarının (Azure AD’den) tek bir kimlik doğrulama sisteminde birleşmesidir.

