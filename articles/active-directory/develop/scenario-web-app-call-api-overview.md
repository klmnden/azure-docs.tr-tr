---
title: Web uygulaması web API'leri (genel bakış) - Microsoft kimlik platformu çağrıları
description: Bir web uygulaması oluşturmayı öğrenin çağrıları veritabanını web API'leri (Genel)
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
ms.openlocfilehash: ce45f11a697b72ebdd0fe01166a70e7b47aa8e9f
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076308"
---
# <a name="scenario-web-app-that-calls-web-apis"></a>Senaryo: Web uygulaması web API'leri çağrıları

Bir web uygulaması Microsoft kimlik platformu üzerinde oturum açma kullanıcı oluşturmayı öğrenin ve web API'leri oturum açmış kullanıcının adına çağırır.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

Bu senaryo, aşağıdaki senaryoyu adım incelediğinize supposes:

> [!div class="nextstepaction"]
> [Oturum açtığında, kullanıcıların web uygulaması](scenario-web-app-sign-user-overview.md)

## <a name="overview"></a>Genel Bakış

Bu nedenle kullanıcılar oturum açabilir ve oturum açmış kullanıcı adına bir web API'si çağıran Web uygulamanıza, kimlik doğrulaması ekleyin.

![Web uygulaması web API'leri çağrıları](./media/scenario-webapp/web-app.svg)

Web API'leri çağıran web uygulamaları:

- Gizli istemci uygulamalardır.
- İşte bu Azure AD ile bir gizli dizi (uygulama parola veya sertifika) kaydınız. Geçirilen bir belirteç almak üzere Azure ad araması sırasında bu gizli dizi

## <a name="specifics"></a>Özellikleri

> [!NOTE]
> Bu Web uygulamasını koruma hakkında olduğu gibi bir Web uygulamasına oturum açma ekleme MSAL kitaplıkları kullanmaz. Kitaplıkları koruma kitaplıkları adlı ara yazılım tarafından sağlanır. Bu nesne önceki senaryonun olan [kullanıcılara bir Web uygulaması oturum açma](scenario-web-app-sign-user-overview.md)
>
> Bir Web uygulamasından Web API çağırma, bu web API'lerine erişim belirteçlerini almak gerekir. MSAL kitaplıkları, bu belirteçleri almak için kullanabilirsiniz.

Geliştiriciler bu senaryo için uçtan uca bir deneyim, bu nedenle, olarak belirli görünüşlere sahiptir:

- Sırasında [uygulama kaydı](scenario-web-app-call-api-app-registration.md), biri sağlamanız gerekir veya birkaç (çeşitli konumlara'de uygulamanızı dağıtırsanız) URI'ler, gizli dizileri, Yanıtla veya sertifikaları Azure AD ile paylaşılması gerekir.
- [Uygulama yapılandırması](scenario-web-app-call-api-app-configuration.md) uygulama kaydı sırasında Azure AD ile paylaşılan olarak istemci kimlik bilgileri sağlaması gerekir

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-web-app-call-api-app-registration.md)
