---
title: Aşağı Akış çağıran API web API'leri (genel bakış) - Microsoft kimlik platformu
description: Web API'si, çağrıları Aşağı Akış web API'leri (genel bakış) oluşturmayı öğrenin.
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
ms.openlocfilehash: 497134b7f3cc535f7b3f180db13cd04ef56787db
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075408"
---
# <a name="scenario-web-api-that-calls-web-apis"></a>Senaryo: Web API'leri çağıran bir web API'si

Tek bir web API'si oluşturmaya ihtiyacınız çağıran web API'leri hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

Bu senaryo, korumalı web API çağrıları API'leri, web üzerinde "bir web API'sini korumak" senaryosu oluşturur. Bu temel senaryo hakkında daha fazla bilgi için bkz. [korunan Web API'sine - senaryo](scenario-protected-web-api-overview.md) ilk.

## <a name="overview"></a>Genel Bakış

- -Aşağıdaki diyagramda temsil edilmeyen - istemci (web, masaüstü, mobil veya tek sayfalı uygulama), korumalı bir web API'sini çağırır ve kendi "Yetkilendirme" Http üst bilgisinde bir JWT taşıyıcı belirteç sağlar.
- Korumalı web API'sini belirteci doğrular ve MSAL kullanan `AcquireTokenOnBehalfOf` alabilir ve böylece, kendisini (Azure AD) farklı bir belirteç istemek için yöntemi çağırın kullanıcı adına ikinci bir web API (Aşağı Akış web API'si olarak adlandırılır).
- Korumalı web API'sini bir aşağı akış API'sini çağırmak için bu belirteci kullanır. Ayrıca çağırabilir `AcquireTokenSilent`sonraki isteği belirteçleri diğer aşağı akış API'leri (ancak yine de aynı kullanıcı adına). `AcquireTokenSilent` gerektiğinde belirtecini yeniler.

![Web API'sini bir web API'si çağırma](media/scenarios/web-api.svg)

## <a name="specifics"></a>Özellikleri

Klasik API izinlerle ilgili uygulama kaydı, parçasıdır. Uygulama yapılandırması, bir aşağı akış API için bir belirteç karşı JWT taşıyıcı belirteç değişimi için OAuth 2.0 on-behalf-of akışı kullanmayı içerir. Bu belirteç, burada, web API'SİNİN denetleyicileri kullanılabilir ve sessiz bir şekilde aşağı akış API'leri çağırmak için bir belirteç elde edebilir belirteç önbelleğe eklenir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-web-api-call-api-app-registration.md)
