---
title: Arka plan programı uygulama çağıran web API'leri (çağıran web API'leri) - Microsoft kimlik platformu
description: Daemon uygulamasının nasıl oluşturulacağını öğrenin çağrıları veritabanını web API'leri (çağıran web API'leri)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
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
ms.openlocfilehash: aff375f996126d9e8b64361fc0e5673c25d30c19
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076278"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>Arka plan programı uygulama çağrıları API'ler - web uygulamasından web API'si çağırın

Bir arka plan programı uygulaması, bir .NET arka plan programı uygulamasından web API'si çağırma ya da birkaç önceden onaylanmış web API'lerini çağırma.

## <a name="calling-a-web-api-from-a-net-daemon-application"></a>Bir .NET arka plan programı uygulamasından web API'si çağırma

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->

## <a name="calling-several-apis"></a>Çeşitli API'leri çağırma

Arka plan programı uygulamaları için web API'leri çağırmanız önceden onaylanması gerekir. Herhangi bir artımlı izin (hiçbir kullanıcı etkileşimi yoktur) arka plan programları ile olmayacak. Kiracı Yöneticisi uygulama ve tüm API izinleri öncesi onayı gerekiyor. Çeşitli API'leri çağırmak istiyorsanız, her bir kaynak için bir belirteç almak ihtiyacınız her zaman arama `AcquireTokenForClient`. MSAL, gereksiz hizmet çağrıları önlemek için uygulama belirteç önbelleği kullanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Arka plan programı app - üretim Git](./scenario-daemon-production.md)