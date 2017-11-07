---
title: "Azure AD v2 iOS Başlarken - Test | Microsoft Docs"
description: "İOS (Swift) uygulamaları, Azure Active Directory v2 bitiş noktası tarafından erişim belirteçleri gerektiren bir API nasıl çağırabilirsiniz"
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
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: c002387c6d2c8ec83610398c337dc504e7253028
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>İOS uygulamanızdan Microsoft grafik API'si sorgulanırken test

Tuşuna `Command`  +  `R` benzeticisinde kodu çalıştırmak için.

![Örnek ekran görüntüsü](media/active-directory-mobileanddesktopapp-ios-test/iostestscreenshot.png)

Test etmek hazır olduğunuzda, dokunun *'Microsoft Graph API çağrısı'* ve kullanıcı adı ve parola yazın istenir.

### <a name="consent"></a>Onay
İlk kez oturum açtığınızda, uygulamanıza, benzer bir onay ekranı sunulur Aşağıda, burada açıkça kabul etmeniz gerekir:

![Onay ekranı](media/active-directory-mobileanddesktopapp-ios-test/iosconsentscreen.png)

### <a name="expected-results"></a>Beklenen sonuçları
Microsoft Graph API tarafından döndürülen kullanıcı profili bilgilerini Çağır görmelisiniz *günlüğü* bölümü.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API gerektiriyor `user.read` kullanıcının profilini okumak için kapsam. Bu kapsam, varsayılan olarak, kayıt portal Kaydedilmekte her uygulama otomatik olarak eklenir. Microsoft Graph için diğer bazı API'leri ve aynı zamanda, arka uç sunucusu için özel API'leri ek kapsamlar gerektirebilir. Örneğin, Microsoft Graph, kapsam `Calendars.Read` kullanıcının takvimleri listelemek için gereklidir. Kullanıcının Takvim bir uygulama bağlamında erişmek için eklemeniz gerekir `Calendars.Read` izin uygulama kayıt ait bilgileri için temsilci ve ardından ekleyin `Calendars.Read` için kapsam `acquireTokenSilent` çağırın. Kapsam sayısı arttıkça, kullanıcı için ek onayları istenebilir.

<!--end-collapse-->

[!INCLUDE  [Help and Support Options](../../../../includes/active-directory-develop-help-support-include.md)]