---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 494f5fb6f2bfdec86cad01a37e57c3ec219a77f9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133599"
---
## <a name="test-your-code"></a>Kodunuzu test etme

Şu ortamlarda yer kullanarak kodunuzu test.

### <a name="test-with-nodejs"></a>Node.js ile test

Visual Studio kullanmıyorsanız, web sunucunuza başlatıldığından emin olun.

1. Sunucusunu konumunu temel alarak bir TCP bağlantı noktasını dinlemek üzere yapılandırmak, *index.html* dosya. Node.js için uygulama klasöründeki bir komut satırı isteminde aşağıdaki komutları çalıştırarak bağlantı noktasını dinlemek için web sunucusunu başlatın:

    ```bash
    npm install
    node server.js
    ```
1. Tarayıcınızda girin **http://\<span >\</span > localhost:30662** veya **http://\<span >\</span > localhost: {port}** , Burada *bağlantı noktası* , web sunucusunun dinleme yaptığı bağlantı noktası. İçeriğini görmelisiniz, *index.html* dosya ve **oturum** düğmesi.

### <a name="test-with-visual-studio"></a>Visual Studio ile test

Visual Studio kullanıyorsanız, proje çözümü seçin ve ardından projenizi çalıştırmak için F5'i seçin. Tarayıcı açılır http://<span></span>localhost: {port} konumu ve **oturum** düğmesi görünür.

## <a name="test-your-application"></a>Uygulamanızı test edin

Sonra tarayıcıyı yükler, *index.html* dosyası **oturum**. Microsoft kimlik platformu uç noktası ile oturum açmanız istenir:

![Oturum açma JavaScript SPA hesabını penceresi](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)

### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için rıza sağlamanın

Uygulamanıza oturum ilk kez oturum açın ve profiliniz için erişim vermeye istenir:

!["İstenen izinleri" penceresi](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Uygulama sonuçlarını görüntüle

Oturum açtıktan sonra kullanıcı profili bilgilerinize sayfasında gösterilen Microsoft Graph API yanıt döndürülür.

![Microsoft Graph API çağrısından sonuçları](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API'sini gerektirir *user.read* kapsamı, bir kullanıcının profilini okuma için. Bu kapsam kayıt portalı üzerinde kayıtlı her uygulamada varsayılan olarak otomatik olarak eklenir. Diğer Microsoft Graph API'leri yanı sıra özel API'ler, arka uç sunucusu için ek kapsamlarla gerektirebilir. Örneğin, Microsoft Graph API'sini gerektirir *Calendars.Read* kullanıcının takvimleri listelemek için kapsam.

Bir uygulama bağlamında kullanıcının takvimler erişmek için ekleme *Calendars.Read* izin uygulama kayıt bilgileri için temsilci. Ardından, ekleme *Calendars.Read* için kapsam `acquireTokenSilent` çağırın.

>[!NOTE]
>Kapsamların sayısı arttıkça, kullanıcı için ek bir onayları istenebilir.

Arka uç API (önerilmez) bir kapsam gerektirmiyorsa kullanabileceğiniz *ClientID* belirteçlerini almak için çağrıları kapsamda olarak.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
