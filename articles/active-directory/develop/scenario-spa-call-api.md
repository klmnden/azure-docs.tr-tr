---
title: Tek sayfalı uygulama (bir web API çağrısı) - Microsoft kimlik platformu
description: Tek sayfalı uygulama (bir web API çağrısı) oluşturmayı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: CelesteDG
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01f141a5374c0e794b264f6e0135ca3e15ff8359
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074853"
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
