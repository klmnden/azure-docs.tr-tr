---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: e48aaa03463bb968ec366b27308ca908aab30d99
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59805366"
---
## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>İOS uygulamanızın içinden Microsoft Graph API'si sorgulanırken test

Benzetici kodu çalıştırmak için basın **komut** + **R**.

![Simülatörde uygulamanızı test edin](media/active-directory-develop-guidedsetup-ios-test/iostestscreenshot.png)

Test için hazır olduğunuzda **Microsoft Graph API çağrısı**. İstendiğinde kullanıcı adınızı ve parolanızı girin.

### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için rıza sağlamanın

Uygulamanıza oturum ilk kez profilinizi erişmek ve oturum açmak için uygulama izin vermek için onay vermeniz istenir:

![Uygulama erişimi için izninizi sağlayın](media/active-directory-develop-guidedsetup-ios-test/iosconsentscreen.png)

### <a name="view-application-results"></a>Uygulama sonuçlarını görüntüle

Oturum açtıktan sonra Microsoft Graph API çağrısı tarafından döndürülen kullanıcı profili bilgilerinize görmelisiniz **günlüğü** bölümü.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API'sini gerektirir **user.read** kapsamı, bir kullanıcının profilini okuma için. Bu kapsam kayıt portalı üzerinde kayıtlı her uygulamada varsayılan olarak otomatik olarak eklenir. Diğer Microsoft Graph API'leri yanı sıra özel API'ler, arka uç sunucusu için ek kapsamlarla gerektirebilir. Microsoft Graph API'sini gerektirir **Calendars.Read** kullanıcının takvimleri listelemek için kapsam.

Bir uygulama bağlamında kullanıcının takvimler erişmek için ekleme **Calendars.Read** izin uygulama kayıt bilgileri için temsilci. Ardından, ekleme **Calendars.Read** için kapsam **acquireTokenSilent** çağırın.

>[!NOTE]
>Kapsamların sayısı arttıkça, kullanıcı için ek bir onayları istenebilir.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
