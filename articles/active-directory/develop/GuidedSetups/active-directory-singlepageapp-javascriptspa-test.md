---
title: Azure AD v2 JS SPA Test Kurulumu - Destekli | Microsoft Docs
description: "JavaScript SPA uygulamaları Azure Active Directory v2 bitiş noktası tarafından erişim belirteçleri gerektiren bir API nasıl çağırabilirsiniz"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: e10e5bbb035878eb62d9295b91263602f8128230
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
## <a name="test-your-code"></a>Kodunuzu test

> ### <a name="testing-with-visual-studio"></a>Visual Studio ile test etme
> Visual Studio kullanıyorsanız, basın `F5` projenizi çalıştırmak için: tarayıcı açılır ve size yönlendirir *http://localhost: {port}* gördüğünüz *Microsoft Graph API çağrısı* düğmesi.

<p/><!-- -->

> ### <a name="testing-with-python-or-another-web-server"></a>Python veya başka bir web sunucusu ile test etme
> Visual Studio kullanmıyorsanız, web sunucunuz başlatıldığından ve içeren klasör dayalı bir TCP bağlantı noktası dinleyecek şekilde yapılandırıldığından emin olun, *index.html* dosya. Python için dinleme bağlantı noktası için çalıştırarak başlatabilirsiniz komut komut istemi / terminal, uygulamanın klasöründen:
> 
> ```bash
> python -m http.server 8080
> ```
>  Ardından, tarayıcı açıp *http://localhost: 8080* veya *http://localhost: {port}* - nerede *bağlantı noktası* web sunucunuz dinlediği bağlantı noktasını karşılık gelir. İndex.html sayfanızın içeriği görmelisiniz *Microsoft Graph API çağrısı* düğmesi.

## <a name="test-your-application"></a>Uygulamanızı test edin

Sonra tarayıcı yükler, *index.html*, tıklatın *Microsoft Graph API çağrısı* düğmesi. İlk kez kullanıyorsanız, tarayıcı, burada oturum açmak için istenir Microsoft Azure Active Directory v2 endpoint yönlendirir.
 
![Örnek ekran görüntüsü](media/active-directory-singlepageapp-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="consent"></a>Onay
Uygulamanız için oturum çok ilk kez, aşağıdakine benzer bir onay ekranı burada kabul etmeniz gerekir sunulur:

 ![Onay ekranı](media/active-directory-singlepageapp-javascriptspa-test/javascriptspaconsent.png)


### <a name="expected-results"></a>Beklenen sonuçları
Kullanıcı profili bilgilerini Microsoft Graph API çağrısı yanıtıyla döndürülen görmeniz gerekir.
 
 ![Sonuçlar](media/active-directory-singlepageapp-javascriptspa-test/javascriptsparesults.png)

İçinde edinilen belirteci hakkındaki temel bilgileri de görebilirsiniz *erişim belirteci* ve *belirteç talep kimliği* kutuları.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API gerektiriyor `user.read` kullanıcının profilini okumak için kapsam. Bu kapsam, varsayılan olarak, kayıt portal Kaydedilmekte her uygulama otomatik olarak eklenir. Microsoft Graph için diğer bazı API'leri ve aynı zamanda, arka uç sunucusu için özel API'leri ek kapsamlar gerektirebilir. Örneğin, Microsoft Graph, kapsam `Calendars.Read` kullanıcının takvimleri listelemek için gereklidir. Kullanıcının Takvim bir uygulama bağlamında erişmek için eklemeniz gerekir `Calendars.Read` izin uygulama kayıt ait bilgileri için temsilci ve ardından ekleyin `Calendars.Read` için kapsam `acquireTokenSilent` çağırın. Kapsam sayısı arttıkça, kullanıcı için ek onayları istenebilir.

Arka uç API'si (önerilmez) bir kapsam gerektirmiyorsa kullanabileceğiniz `clientId` kapsamda olarak `acquireTokenSilent` ve/veya `acquireTokenRedirect` çağrıları.

<!--end-collapse-->

[!INCLUDE  [Help and Support Options](../../../../includes/active-directory-develop-help-support-include.md)]
