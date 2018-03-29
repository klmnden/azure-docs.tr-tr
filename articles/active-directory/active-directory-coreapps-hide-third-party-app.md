---
title: Bir uygulamayı Azure Active Directory'de kullanıcı deneyimini Gizle | Microsoft Docs
description: Bir uygulama Azure Active Directory erişimi paneller veya Office 365 launchers kullanıcı deneyimini Gizle yapma.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2018
ms.author: markvi
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 7da8c60feed7cbb630e5b48653c657cc8fed9e99
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="hide-an-application-from-users-experience-in-azure-active-directory"></a>Bir uygulamayı Azure Active Directory'de kullanıcı deneyimini Gizle

Kullanıcıların erişim panelleri veya Office 365 launchers Göster istemediğiniz bir uygulamanız varsa, bu uygulama kutucuğu gizlemek için seçenek vardır.  Aşağıdaki iki seçenek, kullanıcının uygulama launchers uygulamalardan gizlemek için kullanılabilir.

- Bir üçüncü taraf uygulaması kullanıcıların erişim paneller ve Office 365 uygulama launchers Gizle
- Kullanıcıların erişim paneller tüm Office 365 uygulamalardan Gizle

Gizleme tarafından uygulama kullanıcılar hala uygulama izinlerine sahip ancak bunları kendi uygulama launchers görünür görmez. Kuruluş uygulama yönetmek için uygun izinlere sahip olmalıdır ve dizin için genel yönetici olmanız gerekir.


## <a name="hiding-an-application-from-users-end-user-experiences"></a>Kullanıcının son kullanıcı deneyimleri uygulamadan gizleme
Erişim paneli uygulamalardan gizlemek için durumunuza bağlı olarak, aşağıdaki adımları kullanın.

### <a name="how-do-i-hide-a-third-party-app-from-users-access-panel-and-o365-app-launchers"></a>Kullanıcının erişim paneli ve O365 uygulama launchers üçüncü taraf uygulamasından nasıl Gizle?
Bir kullanıcının erişim paneli ve Office 365 uygulama launchers bir uygulamadan gizlemek için aşağıdaki adımları kullanın.

1.  Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2.  Seçin **tüm hizmetleri**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
3.  Üzerinde **Azure Active Directory - *directoryname***  ekran (diğer bir deyişle, Azure AD ekranı yönettiğiniz dizin için), seçin **kurumsal uygulamalar**.
![Kurumsal uygulamaları](media/active-directory-coreapps-hide-third-party-app/app1.png)
4.  Üzerinde **kurumsal uygulamalar** ekran, select **tüm uygulamaları**. Yönetebileceğiniz uygulamaların bir listesini görürsünüz.
5.  Üzerinde **kurumsal uygulamalar - tüm uygulamaları** ekranında, bir uygulama seçin.</br>
![Kurumsal uygulamaları](media/active-directory-coreapps-hide-third-party-app/app2.png)
6.  Üzerinde ***appname*** ekran (diğer bir deyişle ekran başlığında seçilen uygulamanın adını) özellikleri seçin.
7.  Üzerinde  ***appname* -özellikleri** ekran, select **Evet** için **kullanıcılara görünür?**.
![Kurumsal uygulamaları](media/active-directory-coreapps-hide-third-party-app/app3.png)
8.  Seçin **kaydetmek** komutu.

### <a name="how-do-i-hide-office-365-applications-from-users-access-panel"></a>Kullanıcının erişim paneli uygulamalardan Office 365 nasıl Gizle?

Erişim paneli tüm Office 365 uygulamalardan gizlemek için aşağıdaki adımları kullanın. Bu uygulamaları hala Office 365 Portalı'nda görünür.

1.  Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2.  Seçin **tüm hizmetleri**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
3.  Üzerinde **Azure Active Directory - *directoryname***  ekran (diğer bir deyişle, Azure AD ekranı yönettiğiniz dizin için), seçin **kullanıcı ayarlarını**.
4.  Üzerinde **kullanıcı ayarları** altında ekran **kurumsal uygulamalar** seçin **Evet** için **kullanıcılar yalnızca Office 365 uygulamaları Office 365 portalındabakın**.

![Kurumsal uygulamalar](media/active-directory-coreapps-hide-third-party-app/apps4.png)

## <a name="next-steps"></a>Sonraki adımlar
* [My tüm grupları görmek](active-directory-groups-view-azure-portal.md)
* [Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Adını veya bir kuruluş uygulama logosunu değiştirme](active-directory-coreapps-change-app-logo-user-azure-portal.md)

