---
title: Tek sayfalı uygulama (bir web API çağrısı) - Microsoft kimlik platformu
description: Tek sayfalı uygulama (bir web API çağrısı) oluşturmayı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: ryanwi
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 77a4ed01ac55a1153a62c672b33056a543b912ed
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65545605"
---
# <a name="single-page-application---call-a-web-api"></a>Tek sayfalı uygulama - bir web API çağrısı

Çağırmanızı öneririz `acquireTokenSilent` almak veya bir web API'sine çağrı yapmadan önce bir erişim belirteci yenilemek için yöntemi. Bir belirteci aldıktan sonra korumalı web API'si çağırabilirsiniz.

## <a name="call-a-web-api"></a>Web API çağrısı

### <a name="javascript"></a>JavaScript

Alınan erişim belirteci Microsoft Graph API gibi bir web API'sini çağırmak için bir HTTP isteği bir taşıyıcı olarak kullanın. Örneğin:

```javascript
    var headers = new Headers();
    var bearer = "Bearer " + access_token;
    headers.append("Authorization", bearer);
    var options = {
         method: "GET",
         headers: headers
    };
    var graphEndpoint = "https://graph.microsoft.com/v1.0/me";

    fetch(graphEndpoint, options)
        .then(function (response) {
             //do something with response
        }
```

### <a name="angular"></a>Angular

Belirtildiği gibi [belirteçleri Bölümü alınırken](scenario-spa-acquire-token.md), otomatik olarak erişim belirteçlerini sessiz bir şekilde almak ve bunları API'lerin HTTP isteklerine eklemek için HTTP dinleyiciyi MSAL Angular sarmalayıcı yararlanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretim aşamasına geçme](scenario-spa-production.md)
