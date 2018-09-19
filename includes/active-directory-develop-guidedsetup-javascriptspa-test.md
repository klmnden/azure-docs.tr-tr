---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 9817e94ba77c83b8620274ba41f9d862b6a5ce6d
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293705"
---
## <a name="test-your-code"></a>Kodunuzu test etme

### <a name="test-with-visual-studio"></a>Visual Studio ile test
Visual Studio kullanıyorsanız, basın **F5** projeyi çalıştırın. Http:// tarayıcı açılır<span></span>localhost: {port} konumu göreceksiniz **Microsoft Graph API çağrısı** düğmesi.

<p/><!-- -->

### <a name="test-with-node-or-other-web-server"></a>Düğüm veya başka bir web sunucusu ile test
Visual Studio kullanmıyorsanız, web sunucunuza başlatıldığından emin olun. Sunucusunu konumunu temel alarak bir TCP bağlantı noktasını dinlemek üzere yapılandırmak, **index.html** dosya. Düğüm için uygulama klasöründeki bir komut satırı isteminde aşağıdaki komutları çalıştırarak bağlantı noktasını dinlemek için web sunucusunu başlatın:

```bash
npm install
node server.js
```
Tarayıcıyı açın ve http:// yazın<span></span>localhost:30662 veya http://<span></span>localhost: {port} burada **bağlantı noktası** , web sunucusunun dinleme yaptığı bağlantı noktası. İndex.html dosyanızın içeriğini görmeniz gerekir ve **Microsoft Graph API çağrısı** düğmesi.

## <a name="test-your-application"></a>Uygulamanızı test edin

Tarayıcı, bir index.html dosyası yüklendikten sonra seçin **Microsoft Graph API çağrısı**. İlk kez, uygulamanızı çalıştırdığınızda, tarayıcı Microsoft Azure Active Directory (Azure AD) v2.0 uç noktasına yönlendirir ve oturum açmanız istenir:

![JavaScript SPA'ya hesabınızda oturum açın](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için rıza sağlamanın

Uygulamanıza oturum ilk kez profilinizi erişmek ve oturum açmak için uygulama izin vermek için onay vermeniz istenir:

![Uygulama erişimi için izninizi sağlayın](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Uygulama sonuçlarını görüntüle
Oturum açtıktan sonra kullanıcı profili bilgilerinize görmelisiniz **Graph API çağrısı yanıtı** kutusu.
 
![Microsoft Graph API çağrısından beklenen sonuçları](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

Ayrıca, edinilen belirteci ile ilgili temel bilgileri görmeniz gerekir **erişim belirteci** ve **belirteç talep kimliği** kutuları.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API'sini gerektirir **user.read** kapsamı, bir kullanıcının profilini okuma için. Bu kapsam kayıt portalı üzerinde kayıtlı her uygulamada varsayılan olarak otomatik olarak eklenir. Diğer Microsoft Graph API'leri yanı sıra özel API'ler, arka uç sunucusu için ek kapsamlarla gerektirebilir. Microsoft Graph API'sini gerektirir **Calendars.Read** kullanıcının takvimleri listelemek için kapsam.

Bir uygulama bağlamında kullanıcının takvimler erişmek için ekleme **Calendars.Read** izin uygulama kayıt bilgileri için temsilci. Ardından, ekleme **Calendars.Read** için kapsam **acquireTokenSilent** çağırın. 

>[!NOTE]
>Kapsamların sayısı arttıkça, kullanıcı için ek bir onayları istenebilir.

Arka uç API (önerilmez) bir kapsam gerektirmiyorsa kullanabileceğiniz **ClientID** kapsamda olarak **acquireTokenSilent** ve **acquireTokenRedirect** çağrıları.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
