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
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: c8e5e4f826d7835a1fd38d1db5bfeab19b679b30
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60250676"
---
## <a name="test-your-app"></a>Uygulamanızı test etme

1. Cihaz/öykünücüsü için kodunuzu çalıştırın.
2. Bir Azure Active Directory hesabı (iş veya Okul hesabı) veya Microsoft hesabı (live.com, outlook.com) kullanarak oturum açmayı deneyin. 

    ![Uygulamanızı test edin](media/active-directory-develop-guidedsetup-android-test/mainwindow.png)
    <br/><br/>
    ![Kullanıcı adı ve parola girin](media/active-directory-develop-guidedsetup-android-test/usernameandpassword.png)

### <a name="consent-to-your-app"></a>Uygulamanıza onay

Uygulamanız için bir kullanıcı oturum açtığında ilk kez bunlar aşağıda gösterildiği gibi uygulama ihtiyaçlarınızı izinleri onay istenir: 

![Uygulama erişimi için izninizi sağlayın](media/active-directory-develop-guidedsetup-android-test/androidconsent.png)

### <a name="success"></a>Başarılı!

Oturum & onay sonra uygulamayı Microsoft Graph API yanıtından görüntüler. Bu belirli çağrı olmaktır **/me** uç noktası ve döndürür [kullanıcı profili](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_get). Diğer Microsoft Graph uç noktalar listesi için bkz. [Microsoft Graph API Geliştirici belgeleri](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri

Microsoft Graph API'sini gerektirir *User.Read* kapsamı, bir kullanıcının profilini okuma için. Bu kapsamı otomatik olarak uygulama kayıt Portalı'nda kayıtlı her uygulamada olur. Diğer API'lara ek kapsamlarla gerektirir. Örneğin, Microsoft Graph API'sini gerektirir *Calendars.Read* kullanıcının takvimleri listelemek için kapsam.

Kullanıcı takvimlerini erişmek için eklemeniz *Calendars.Read* izin uygulama kayıt bilgileri için temsilci. Ardından, ekleme *Calendars.Read* için kapsam `acquireTokenSilent` çağırın. 

> [!NOTE]
> Uygulama kaydınızı değiştirirseniz kullanıcılarınız için ek onayı istenebilir.

<!--end-collapse-->

[!INCLUDE [Help and support](active-directory-develop-help-support-include.md)]
