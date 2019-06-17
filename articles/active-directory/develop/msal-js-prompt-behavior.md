---
title: İstemi davranışı etkileşimli istekleri (JavaScript için Microsoft kimlik doğrulama kitaplığı) | Azure
description: JavaScript (MSAL.js) için Microsoft kimlik doğrulama kitaplığı kullanarak etkileşimli çağrılarındaki istemi davranışı özelleştirme hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: dd0d736345f312f1a1d6f8f029b41429a3e5f0a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65544264"
---
# <a name="prompt-behavior-in-msaljs-interactive-requests"></a>Komut istemi davranışı MSAL.js etkileşimli istekleri

Bir kullanıcı bir etkin Azure AD ile birden çok kullanıcı hesabı oturumun olduğunda, Azure AD oturum açma sayfasında varsayılan olarak oturum açmak için devam etmeden önce bir hesap seçmek için girmesini ister. Kullanıcılar tek bir Azure AD kimliği doğrulanmış oturumla ise deneyimi bir hesap seçimi görmez.

(İçinde v0.2.4 başlayarak) MSAL.js kitaplığı istemi parametresinin sırasında etkileşimli istekleri göndermez (`loginRedirect`, `loginPopup`, `acquireTokenRedirect` ve `acquireTokenPopup`) ve böylece herhangi bir komut istemi davranışı zorlamaz. Sessiz belirteç isteklerini kullanarak `acquireTokenSilent` yöntemi, MSAL.js ayarlamak bir istem parametresi geçirir `none`.

Uygulama senaryonuza bağlı olarak, istek parametrelerini komut istemi parametreyi yöntemlere geçilen etkileşimli istekler ayarlayarak istemi davranışını kontrol edebilirsiniz. Örneğin, hesabı seçme deneyimi çağırmak istiyorsanız:

```javascript
var request = {
    scopes: ["user.read"],
    prompt: 'select_account',
}

userAgentApplication.loginRedirect(request);
```


Aşağıdaki komut istemi değerleri, Azure AD ile kimlik doğrulaması yapılırken geçirilebilir:

**Oturum açma:** Bu değer, kullanıcının kimlik doğrulama isteği kimlik bilgilerini girmesini zorunlu tutar.

**select_account:** Bu değer kullanıcı oturumunda tüm hesapları listeleme bir hesabı seçme deneyimi sağlayacak.

**onay:** Bu değer, kullanıcılara uygulama izinleri vermek OAuth onay iletişim kutusunu çağırır.

**Hiçbiri:** Bu değer, kullanıcının etkileşimli bir istemi görmez sağlayacaktır. Sahip olduğunuz bu değer MSAL.js etkileşimli yöntemlere geçirmemesi için önerilen beklenmeyen davranışlar. Bunun yerine, `acquireTokenSilent` sessiz çağrıları elde etmek için yöntemi.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin `prompt` parametresinde [OAuth 2.0 örtülü izin](v2-oauth2-implicit-grant-flow.md) Protokolü hangi MSAL.js kitaplığını kullanır.
