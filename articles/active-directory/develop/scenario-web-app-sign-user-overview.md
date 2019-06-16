---
title: Kullanıcılar (genel bakış) - Microsoft kimlik platformu imzalar web uygulaması
description: (Genel bakış) kullanıcılar oturum açtığında bir web uygulaması oluşturmayı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6ce534c6eeecba220fd829be829caa679df52055
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65833093"
---
# <a name="scenario-web-app-that-signs-in-users"></a>Senaryo: Kullanıcıların oturum açtığı web uygulaması

Microsoft kimlik platformu ile oturum açtığında kullanıcıların bir web uygulaması oluşturmak için gereken her şeyi öğrenin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Başlarken

Kullanıcıların oturum açmasını ilk taşınabilir (ASP.NET Core) web uygulamalarınızı oluşturmak istiyorsanız, bu hızlı başlangıcı takip:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: ASP.NET Core web uygulaması, kullanıcıların oturum açtığında](quickstart-v2-aspnet-core-webapp.md)

ASP.NET ile kalmayı tercih ederseniz, aşağıdaki öğreticiyi deneyin:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: ASP.NET web uygulaması, kullanıcıların oturum açtığında](quickstart-v2-aspnet-webapp.md)

## <a name="overview"></a>Genel Bakış

Kullanıcılar oturum açabilirsiniz, böylece web uygulamanıza kimlik doğrulaması ekleyin. Ekleme kimlik doğrulaması sınırlı profilinizdeki bilgilere erişmek ve örneğin, kullanıcılara sunmak deneyimi özelleştirmek, web uygulamanızı sağlar. Web uygulamaları bir web tarayıcısında kullanıcı kimlik doğrulaması. Bu senaryoda, Azure AD'de oturum açmak için kullanıcının tarayıcısına web uygulamasının yönlendirir. Azure AD oturum açma yanıt içeren bir güvenlik belirteci kullanıcı hakkında talepler kullanıcının tarayıcısından döndürür. Kullanıcıların oturum açma yararlanarak [Open ID Connect](./v2-protocols-oidc.md) standart protokolün kendisini ara yazılım tarafından kolay [kitaplıkları](scenario-web-app-sign-user-app-configuration.md#libraries-used-to-protect-web-apps).

![Web uygulama kullanıcıları, oturum açtığında](./media/scenario-webapp/scenario-webapp-signs-in-users.svg)

İkinci aşama uygulamanızın oturum açmış kullanıcı adına Web API'leri çağırmak de etkinleştirebilirsiniz. Bu, bulabilirsiniz farklı bir senaryo, bir sonraki aşamasıdır [Web uygulaması, Web API'leri çağıran](scenario-web-app-call-api-overview.md)

> [!NOTE]
> Oturum açma ekleme bir web uygulaması web uygulamasını koruma hakkında bilgi ve bir kullanıcı belirteci doğrulanırken, hangi ne **ara yazılım** kitaplıkları yapın. Bu senaryo gerektirmez, ancak API'leri çağırmak için bir belirteç edinme hakkında olan Microsoft kimlik doğrulama kitaplıkları'nı (MSAL) korumalı. Web uygulaması, web API'leri çağırmak gerektiğinde, kimlik doğrulama kitaplıkları izleme senaryosunda yalnızca sunulacaktır.

## <a name="specifics"></a>Özellikleri

- Uygulama kaydı sırasında birini sağlamanız gerekir ve birkaç (çeşitli konumlara'de uygulamanızı dağıtırsanız) URI'ler yanıtla. Bazı durumlarda (ASP.NET/ASP.NET çekirdek) Idtoken etkinleştirmeniz gerekir. Son olarak uygulamanızı imzalama genişletme kullanıcılara tepki verir. böylece, oturum kapatma URI ayarlamak da istersiniz.
- Uygulamanızın kodunda uygulama temsilciler oturum açma web yetkiyi sağlamak gerekir. (Belirli ISV senaryolarda) belirteci doğrulama özelleştirmek isteyebilirsiniz.
- Web uygulamaları, tüm hesap türleri desteklemez. Daha fazla bilgi için bkz. [desteklenen hesap türleri](v2-supported-account-types.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-web-app-sign-user-app-registration.md)
