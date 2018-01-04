---
title: "Bir uygulamayı Azure Active Directory'de kullanıcı deneyimini Gizle | Microsoft Docs"
description: "Azure Active Directory'de kullanıcı deneyimini uygulamadan gizleme"
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
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
ms.openlocfilehash: 667fdd45bc9eb1f01ce3883006bb29274478cb83
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="hide-an-application-from-users-experience-in-azure-active-directory"></a>Bir uygulamayı Azure Active Directory'de kullanıcı deneyimini Gizle

Kullanıcıların erişim panelleri veya Office 365 launchers Göster istemediğiniz bir uygulamanız varsa, bu uygulama kutucuğu gizlemek için bir seçenek yoktur. Bu seçenek, yalnızca üçüncü taraf uygulamaların Microsoft tarafından yayımlanmayan için kullanılabilir. Gizleme tarafından uygulama kullanıcılar hala uygulama izinlerine sahip ancak bunları kendi uygulama launchers görünür görmez. Kuruluş uygulama yönetmek için uygun izinlere sahip olmalıdır ve dizin için genel yönetici olmanız gerekir. 

## <a name="hiding-an-application-from-users-end-user-experiences"></a>Kullanıcının son kullanıcı deneyimleri uygulamadan gizleme
Bir kullanıcının erişim paneli ve Office 365 uygulama launchers bir uygulamadan gizlemek için aşağıdaki adımları kullanın

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
