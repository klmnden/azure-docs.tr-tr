---
title: Web uygulaması çağrıları API'ler (uygulama kaydı) - web Microsoft kimlik platformu
description: Bir web uygulaması oluşturmayı öğrenin çağrıları veritabanını web API'leri (uygulama kaydı)
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
ms.openlocfilehash: 5becdc287f7cad28aa6c7c07ebc107586e9a2b2a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65075198"
---
# <a name="web-app-that-calls-web-apis---app-registration"></a>Web uygulaması web API'leri - uygulama kaydı çağrıları

Bir Web uygulamasını çağırma web API'leri, bir Web uygulaması oturum açma kullanıcı olarak aynı kayıt vardır. Bu nedenle yönergeleri gerekir [Web uygulaması oturum açtığında kullanıcıların - uygulama kaydı](scenario-web-app-sign-user-app-registration.md)

Ancak çağrıları web API'leri, artık Web uygulaması bu yana bu gizli bir istemci uygulaması olur. İşte bu biraz ek kayıt gerekli yoktur: gizli anahtarları (istemci kimlik bilgileri) Microsoft kimlik platformu ile paylaşmak gerekiyor.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>API izinleri

Web uygulamaları, oturum açmış kullanıcı adına API'lerini çağırma. Temsilci izinleri istemek gerekir. Ayrıntılar için bkz. [web API'lerine erişim izni ekleyin](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kodu yapılandırma](scenario-web-app-call-api-app-configuration.md)
