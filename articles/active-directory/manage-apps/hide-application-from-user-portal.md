---
title: Bir uygulamayı Azure Active Directory'de kullanıcı deneyiminden gizleme | Microsoft Docs
description: Uygulamanın Azure Active Directory erişim panellerinde veya Office 365 launchers kullanıcı deneyiminden gizleme yapma.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/04/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 55f80396df4cbfe7d0a16a6a5066b68aadc0bdd3
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39369348"
---
# <a name="hide-an-application-from-users-experience-in-azure-active-directory"></a>Bir uygulamadan Azure Active Directory'de kullanıcı deneyimini Gizle

Kullanıcıların erişim panellerinde veya Office 365 launchers göstermek istemediğiniz bir uygulamanız varsa, bu uygulama kutucuğunu gizleme seçenekleri vardır.  Aşağıdaki iki seçenek, kullanıcının uygulama launchers uygulamalarından gizlemek için kullanılabilir.

- Bir üçüncü taraf uygulamasını kullanıcılar erişim panellerinde ve Office 365 uygulama launchers Gizle
- Kullanıcılara erişim panelleri üzerinden tüm Office 365 uygulamalarından gizleme

Gizleyerek uygulama kullanıcılarının hala uygulama izinleri ancak bunları kendi uygulama launchers üzerinde görünen görmez. Kurumsal uygulamasını yönetmek için uygun izinlere sahip olmalıdır ve dizin için genel yönetici olması gerekir.


## <a name="hiding-an-application-from-users-end-user-experiences"></a>Kullanıcının son kullanıcı deneyimlerini uygulamadan gizleme
Erişim paneli uygulamalarından gizlemek için durumunuza bağlı olarak, aşağıdaki adımları kullanabilirsiniz.

### <a name="how-do-i-hide-a-third-party-app-from-users-access-panel-and-o365-app-launchers"></a>Kullanıcının erişim panelinde ve O365 uygulama launchers üçüncü taraf bir uygulamadan nasıl Gizle?
Bir kullanıcının erişim panelinde ve Office 365 uygulama launchers uygulamasından gizlemek için aşağıdaki adımları kullanın.

1.  Dizin için genel yönetici olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açın.
2.  Seçin **tüm hizmetleri**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
3.  Üzerinde **Azure Active Directory - *directoryname***  ekranında (diğer bir deyişle, Azure AD ekran için yönetirken dizin) seçin **kurumsal uygulamalar**.
![Kurumsal uygulamalar](./media/hide-application-from-user-portal/app1.png)
4.  Üzerinde **kurumsal uygulamalar** ekranındayken **tüm uygulamaları**. Yönetebileceğiniz uygulamaların listesini görürsünüz.
5.  Üzerinde **kurumsal uygulamalar - tüm uygulamalar** ekranında, bir uygulama seçin.</br>
![Kurumsal uygulamalar](./media/hide-application-from-user-portal/app2.png)
6.  Üzerinde ***appname*** ekran (diğer bir deyişle, ekran başlığını seçilen uygulamanın adını), özellikleri seçin.
7.  Üzerinde  ***appname* -Özellikler** ekranındayken **Evet** için **kullanıcılara görünür?**.
![Kurumsal uygulamalar](./media/hide-application-from-user-portal/app3.png)
8.  Seçin **Kaydet** komutu.

### <a name="how-do-i-hide-office-365-applications-from-users-access-panel"></a>Kullanıcının erişim paneli Office 365 uygulamalarını nasıl Gizle?

Tüm Office 365 uygulamaları erişim panelinden gizlemek için aşağıdaki adımları kullanın. Bu uygulamalar Office 365 portalında görünür olacaktır.

1.  Dizin için genel yönetici olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açın.
2.  Seçin **tüm hizmetleri**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
3.  Üzerinde **Azure Active Directory - *directoryname***  ekranında (diğer bir deyişle, Azure AD ekran için yönetirken dizin) seçin **kullanıcı ayarları**.
4.  Üzerinde **kullanıcı ayarları** altında ekran **kurumsal uygulamalar** seçin **Evet** için **kullanıcılar yalnızca Office 365 uygulamalarınıOffice365portalındagörebilir**.

![Kurumsal uygulamalar](./media/hide-application-from-user-portal/apps4.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Tüm Gruplarım bakın](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Kurumsal bir uygulamayı kullanıcı veya grup atama](assign-user-or-group-access-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](remove-user-or-group-access-portal.md)
* [Adını veya kurumsal bir uygulamanın logoyu değiştirme](change-name-or-logo-portal.md)

