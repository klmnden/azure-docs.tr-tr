---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: d333f8ecd7e1044575f570d893227f9dcb394974
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843589"
---
## <a name="test-your-code"></a>Kodunuzu test etme

Visual Studio, projenizi çalıştırmak için seçin **F5**. Uygulamanızı **MainWindow** , burada gösterildiği gibi görüntülenir:

![Uygulamanızı test edin](./media/active-directory-develop-guidedsetup-windesktop-test/samplescreenshot.png)

Uygulamayı çalıştırdığınızda ilk kez seçip **Microsoft Graph API çağrısı** , istenir oturum açın düğmesi. Bir Azure Active Directory hesabı (iş veya Okul hesabı) veya bir Microsoft hesabı (live.com, outlook.com) test etmek için kullanın.

![Uygulama için oturum açın](./media/active-directory-develop-guidedsetup-windesktop-test/signinscreenshot.png)

### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için rıza sağlamanın
Uygulamanız için de istenir sağlamak için oturum ilk kez uygulamanın profilinizi erişmek ve içinde burada gösterildiği gibi oturum izin vermek için onay: 

![Uygulama erişimi için izninizi sağlayın](./media/active-directory-develop-guidedsetup-windesktop-test/consentscreen.png)

### <a name="view-application-results"></a>Uygulama sonuçlarını görüntüle
Oturum açtıktan sonra Microsoft Graph API çağrısı tarafından döndürülen kullanıcı profili bilgilerini görmeniz gerekir. Sonuçları görüntülenir **API çağrısı sonuçları** kutusu. Çağrısı aracılığıyla edinilen belirteci ile ilgili temel bilgileri `AcquireTokenAsync` veya `AcquireTokenSilentAsync` görünür olmalıdır **belirteci bilgilerini** kutusu. Sonuçlar aşağıdaki özellikleri içerir:

|Özellik  |Biçimlendir  |Açıklama |
|---------|---------|---------|
|**Ad** |Kullanıcının tam adı |Kullanıcı adı ve soyadı.|
|**Kullanıcı Adı** |<span>user@domain.com</span> |Kullanıcıyı tanımlamak için kullanılan kullanıcı adı.|
|**Belirteç süre sonu** |DateTime |Belirtecin süresinin dolma zamanı. MSAL, gerekirse belirteci yenilemeye tarafından sona erme tarihini genişleten.|
|**Erişim belirteci** |Dize |HTTP için gönderilen belirteç dizesini gerektiren istekleri bir *yetkilendirme üst bilgisi*.|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API'sini gerektirir *user.read* kapsamı, bir kullanıcının profilini okuma için. Bu kapsam, uygulama kayıt Portalı'nda kayıtlı her uygulamada varsayılan olarak otomatik olarak eklenir. Diğer Microsoft Graph API'leri yanı sıra özel API'ler, arka uç sunucusu için ek kapsamlarla gerektirebilir. Microsoft Graph API'sini gerektirir *Calendars.Read* kullanıcının takvimleri listelemek için kapsam.

Bir uygulama bağlamında kullanıcının takvimler erişmek için ekleme *Calendars.Read* izin uygulama kayıt bilgileri için temsilci. Ardından, ekleme *Calendars.Read* için kapsam `acquireTokenSilent` çağırın. 

>[!NOTE]
>Kapsamların sayısı arttıkça, kullanıcı için ek bir onayları istenebilir.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
