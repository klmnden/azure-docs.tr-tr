---
title: "Azure AD v2 Windows Masaüstü tıklatmalarını başlatıldı - Test | Microsoft Docs"
description: "Windows Masaüstü .NET (XAML) uygulamaları, Azure Active Directory v2 bitiş noktası tarafından erişim belirteçleri gerektiren bir API nasıl çağırabilirsiniz"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 972cc48057c13271d725b0c973c3ccf651ad27c4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a>Kodunuzu test

Uygulamanızı test etmek için basın `F5` Visual Studio'da projeyi çalıştırın. Ana penceresi görünmelidir:

![Örnek ekran görüntüsü](media/active-directory-mobileanddesktopapp-windowsdesktop-test/samplescreenshot.png)

Test etmek hazır olduğunuzda, tıklatın *Microsoft Graph API çağrısı* ve oturum açmak için bir Microsoft Azure Active Directory (kuruluş hesabı) veya bir Microsoft Account (live.com, outlook.com) hesabı kullanın. Bu ilk kez, oturum açmak için kullanıcı isteyen bir pencere görürsünüz:

![oturum açma](media/active-directory-mobileanddesktopapp-windowsdesktop-test/signinscreenshot.png)

### <a name="consent"></a>Onay
İlk kez oturum açtığınızda, uygulamanıza, benzer bir onay ekranı sunulur Aşağıda, burada açıkça kabul etmeniz gerekir:

![Onay ekranı](media/active-directory-mobileanddesktopapp-windowsdesktop-test/consentscreen.png)

### <a name="expected-results"></a>Beklenen sonuçları
Kullanıcı profili bilgilerini API çağrısı sonuçları ekranda Microsoft Graph API çağrısı tarafından döndürülen görmeniz gerekir.

Aracılığıyla edinilen belirteci hakkında temel bilgiler görmeniz gerekir `AcquireTokenAsync` veya `AcquireTokenSilentAsync` belirteci bilgisi kutusunda:

|Özellik  |Biçimi  |Açıklama |
|---------|---------|---------|
|Ad | {Tam} kullanıcı adı |Kullanıcı adı ve Soyadı|
|Kullanıcı adı |<span>user@domain.com</span> |Kullanıcıyı tanımlamak için kullanılan kullanıcı adı|
|Belirtecinin süresi |{DateTime}         |Üzerinde belirtecin süresinin dolduğu zaman. MSAL sona erme tarihini sizin için gerekli olduğunda belirteci yenileyerek uzatır|
|erişim belirteci |{Dize}         |Belirteç dizesini bir authorization üstbilgisi gerektiren HTTP isteklerini gönderilecek gönderilen|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi
Grafik API'si gerektirir `user.read` kullanıcı profilini okuma için kapsamı. Bu kapsam, varsayılan olarak, kayıt portal Kaydedilmekte her uygulama otomatik olarak eklenir. Bazı diğer grafik API'leri yanı sıra, arka uç sunucusu için özel API'leri ek kapsamlar gerektirir. Örneğin, bir grafik `Calendars.Read` liste kullanıcının takvimleri için gereklidir. Kullanıcının Takvim bir uygulama bağlamında erişmek için eklemeniz gerekir `Calendars.Read` uygulama kaydı'nın bilgi temsilci ve ardından ekleyin `Calendars.Read` için `AcquireTokenAsync` çağırın. Kapsam sayısı arttıkça, kullanıcı için ek onayları istenebilir.

<!--end-collapse-->



