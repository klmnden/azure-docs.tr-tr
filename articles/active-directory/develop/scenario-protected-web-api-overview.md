---
title: Korumalı Web API'si - genel bakış | Azure
description: Korumalı web API'si (genel bakış) oluşturmayı öğrenin.
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
ms.openlocfilehash: c5bc0075e6604bd1f94531040f2e4a0628e70667
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65074898"
---
# <a name="scenario-protected-web-api"></a>Senaryo: Korumalı web API'si

Bu senaryoda, bir web API'sini nasıl kullanıma sunabileceğiniz göstereceğiz ve nasıl, böylece yalnızca kimliği doğrulanmış kullanıcılar Koruyabileceğiniz API erişebilir. Hem iş ve Okul hesapları veya kişisel Microsoft Kişisel hesapları ile kimliği doğrulanmış kullanıcıların web API'nizi kullanmasını sağlamak istersiniz.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>Özellikleri

Web API'leri korumak için bilmeniz gereken bazı özellikleri şunlardır:

- Uygulama kaydınızı en az bir kapsam kullanıma sunması gerekir. Oturum açma İzleyici web API'niz tarafından kabul edilen belirteci sürümüne bağlıdır.
- Web API'si çağrılırken kullanılan belirtecin web API'si için kod yapılandırmasını doğrulamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-protected-web-api-app-registration.md)
