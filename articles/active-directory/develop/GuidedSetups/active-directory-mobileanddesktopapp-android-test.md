---
title: "Azure AD v2 Android alma başlatıldı - Test | Microsoft Docs"
description: "Nasıl bir Android uygulaması bir erişim belirteci alın ve Microsoft Graph API veya Azure Active Directory v2 uç noktasından erişim belirteçleri gerektiren API'larını çağırma"
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
ms.openlocfilehash: 6df64f4820f8409bd8897d5ac24f81bffeeef102
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a>Kodunuzu test

1. Kodunuzu Aygıt/Öykünücüsü için dağıtın.
2. Test etmek hazır olduğunuzda, oturum açmak için bir Microsoft Azure Active Directory (kuruluş hesabı) veya bir Microsoft Account (live.com, outlook.com) hesabı kullanın. 

![Örnek ekran görüntüsü](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
<br/><br/>
![Oturum açma](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)

### <a name="consent"></a>Onay
Bir kullanıcı oturum açtığında, uygulama için ilk kez bunlar benzer bir onay ekranı sunulur aşağıda açıkça kabul etmek gereken yeri: 

![Onay](media/active-directory-mobileanddesktopapp-android-test/androidconsent.png)


### <a name="expected-results"></a>Beklenen sonuçları
Microsoft Graph API çağrısının sonuçlarını 'me' görmelisiniz uç noktası için kullanıcı profili - https://graph.microsoft.com/v1.0/me elde etmek üzere kullanılır. Genel Microsoft Graph uç noktaları listesi için lütfen bu bakın [makale](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API gerektiriyor `user.read` kullanıcının profilini okumak için kapsam. Bu kapsam, varsayılan olarak, kayıt portal Kaydedilmekte her uygulama otomatik olarak eklenir. Microsoft Graph için diğer bazı API'leri ve aynı zamanda, arka uç sunucusu için özel API'leri ek kapsamlar gerektirebilir. Örneğin, Microsoft Graph, kapsam `Calendars.Read` kullanıcının takvimleri listelemek için gereklidir. Kullanıcının Takvim bir uygulama bağlamında erişmek için eklemeniz gerekir `Calendars.Read` izin uygulama kayıt ait bilgileri için temsilci ve ardından ekleyin `Calendars.Read` için kapsam `acquireTokenSilentAsync` çağırın. Kapsam sayısı arttıkça, kullanıcı için ek onayları istenebilir.

<!--end-collapse-->
