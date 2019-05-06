---
title: Web API'si, çağrıları Aşağı Akış web API'leri (uygulama kaydı) - Microsoft kimlik platformu
description: Web API'si, çağrıları Aşağı Akış web API'leri (uygulama kaydı) oluşturmayı öğrenin
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
ms.openlocfilehash: fb03869cdea2150b6e922e2d6d81e577c3be02da
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075393"
---
# <a name="web-api-that-calls-web-apis---app-registration"></a>Web API çağrıları web API'leri - uygulama kaydı

Aşağı Akış web API'si çağıran bir web API'si, korumalı web API'si olarak aynı kayıt vardır. Bu nedenle, yönergeleri izlemeniz gerekecek [korunan Web API'sine - uygulama kaydı](scenario-protected-web-api-app-registration.md).

Ancak, çağrıları web API'leri, artık web uygulaması bu yana bu gizli bir istemci uygulaması olur. İşte bu nedenle, gereken ek kayıt bilgileri yoktur: gizli anahtarları (istemci kimlik bilgileri) Microsoft kimlik platformu ile paylaşmak uygulamanın gerekir.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>API izinleri

Web uygulamaları, API'leri kendisi için taşıyıcı belirteç alındı kullanıcı adına çağırın. Temsilci izinleri istemek gerekir. Ayrıntılar için bkz [web API'lerine erişim izni ekleyin](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kodu yapılandırma](scenario-web-api-call-api-app-configuration.md)
