---
title: "Bir üçüncü taraf uygulamayı Azure Active Directory'de kullanıcı deneyimini Gizle | Microsoft Docs"
description: "Bir üçüncü taraf uygulama Azure Active Directory'de kullanıcı deneyiminde gizleme"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: billmath
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 976cbb1341493186b9996d250ebca8f2f3688fdf
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="hide-a-third-party-application-from-users-experience-in-azure-active-directory"></a>Bir üçüncü taraf uygulamayı Azure Active Directory'de kullanıcı deneyimini Gizle

Bir üçüncü taraf uygulama (Microsoft diğerlerinden tarafından yayımlanan uygulama) varsa görünmesi kullanıcıların erişim panelleri veya Office 365 launchers, bu uygulama kutucuğu gizlemek için bir seçenek yoktur istemediğiniz. Gizleme tarafından uygulama kullanıcılar hala uygulama izinlerine sahip ancak bunları kendi uygulama launchers görünür görmez. Kuruluş uygulama yönetmek için uygun izinlere sahip olmalıdır ve dizin için genel yönetici olmanız gerekir.

## <a name="hiding-a-third-party-app-from-a-users-experience"></a>Kullanıcı deneyiminde bir üçüncü taraf uygulama gizleme
Bir kullanıcının erişim paneli ve Office 365 uygulama launchers üçüncü taraf uygulamadan gizlemek için aşağıdaki adımları kullanın

### <a name="how-do-i-hide-a-third-party-app-from-users-access-panel-and-o365-app-launchers"></a>Kullanıcının erişim paneli ve O365 uygulama launchers üçüncü taraf uygulamasından nasıl Gizle?

1.  Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2.  Seçin **daha fazla hizmet**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
3.  Üzerinde **Azure Active Directory - *directoryname***  ekran (diğer bir deyişle, Azure AD ekranı yönettiğiniz dizin için), seçin **kurumsal uygulamalar**.
![Kurumsal uygulamaları](media/active-directory-coreapps-hide-third-party-app/app1.png)
4.  Üzerinde **kurumsal uygulamalar** ekran, select **tüm uygulamaları**. Yönetebileceğiniz uygulamaların bir listesini görürsünüz.
5.  Üzerinde **kurumsal uygulamalar - tüm uygulamaları** ekranında, bir uygulama seçin.</br>
![Kurumsal uygulamaları](media/active-directory-coreapps-hide-third-party-app/app2.png)
6.  Üzerinde ***appname*** ekran (diğer bir deyişle ekran başlığında seçilen uygulamanın adını) özellikleri seçin.
7.  Üzerinde  ***appname* -özellikleri** ekran, select **Evet** için **kullanıcılara görünür?**.
![Kurumsal uygulamaları](media/active-directory-coreapps-hide-third-party-app/app3.png)
8.  Seçin **kaydetmek** komutu.

## <a name="next-steps"></a>Sonraki adımlar
* [My tüm grupları görmek](active-directory-groups-view-azure-portal.md)
* [Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Adını veya bir kuruluş uygulama logosunu değiştirme](active-directory-coreapps-change-app-logo-user-azure-portal.md)
