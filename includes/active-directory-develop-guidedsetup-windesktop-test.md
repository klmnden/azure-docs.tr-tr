---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: a11b291ab89dc9f8159e00e1f2304706f041068e
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "60297751"
---
## <a name="test-your-code"></a>Kodunuzu test etme

Visual Studio, projenizi çalıştırmak için seçin **F5**. Uygulamanızı **MainWindow** , burada gösterildiği gibi görüntülenir:

![Uygulamanızı test edin](./media/active-directory-develop-guidedsetup-windesktop-test/samplescreenshot.png)

Uygulamayı çalıştırdığınızda ilk kez seçip **Microsoft Graph API çağrısı** , istenir oturum açın düğmesi. Bir Azure Active Directory hesabı (iş veya Okul hesabı) veya bir Microsoft hesabı (live.com, outlook.com) test etmek için kullanın.

![Uygulamada oturum açma](./media/active-directory-develop-guidedsetup-windesktop-test/signinscreenshot.png)

### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için rıza sağlamanın

Uygulamanız için de istenir sağlamak için oturum ilk kez uygulamanın profilinizi erişmek ve içinde burada gösterildiği gibi oturum izin vermek için onay:

![Uygulama erişimi için izninizi sağlayın](./media/active-directory-develop-guidedsetup-windesktop-test/consentscreen.png)

### <a name="view-application-results"></a>Uygulama sonuçlarını görüntüle

Oturum açtıktan sonra Microsoft Graph API çağrısı tarafından döndürülen kullanıcı profili bilgilerini görmeniz gerekir. Sonuçları görüntülenir **API çağrısı sonuçları** kutusu. Çağrısı aracılığıyla edinilen belirteci ile ilgili temel bilgileri `AcquireTokenInteractive` veya `AcquireTokenSilent` görünür olmalıdır **belirteci bilgilerini** kutusu. Sonuçlar aşağıdaki özellikleri içerir:

|Özellik  |Biçimlendir  |Açıklama |
|---------|---------|---------|

|**Kullanıcı adı**  | <span> user@domain.com </span> | Kullanıcıyı tanımlamak için kullanılan kullanıcı adı. | | **Belirtecinin süresi dolmadan** | DateTime | Belirtecin süresinin dolma zamanı. MSAL, gerekirse belirteci yenilemeye tarafından sona erme tarihini genişleten. |


<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API'sini gerektirir *user.read* kapsamı, bir kullanıcının profilini okuma için. Bu kapsam, uygulama kayıt Portalı'nda kayıtlı her uygulamada varsayılan olarak otomatik olarak eklenir. Diğer Microsoft Graph API'leri yanı sıra özel API'ler, arka uç sunucusu için ek kapsamlarla gerektirebilir. Microsoft Graph API'sini gerektirir *Calendars.Read* kullanıcının takvimleri listelemek için kapsam.

Bir uygulama bağlamında kullanıcının takvimler erişmek için ekleme *Calendars.Read* izin uygulama kayıt bilgileri için temsilci. Ardından, ekleme *Calendars.Read* için kapsam `acquireTokenSilent` çağırın.

>[!NOTE]
>Kapsamların sayısı arttıkça, kullanıcı için ek bir onayları istenebilir.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
