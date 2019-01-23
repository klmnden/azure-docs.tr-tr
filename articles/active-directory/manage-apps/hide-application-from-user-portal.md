---
title: Bir uygulamayı Azure Active Directory'de kullanıcı deneyiminden gizleme | Microsoft Docs
description: Uygulamanın Azure Active Directory erişim panellerinde veya Office 365 launchers kullanıcı deneyiminden gizleme yapma.
services: active-directory
author: barbkess
manager: daveba
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: barbkess
ms.reviewer: kasimpso
ms.openlocfilehash: d5443f5b33f85d7a4b1e69cecc6bcdf68859abf4
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54474918"
---
# <a name="hide-applications-from-end-users-in-azure-active-directory"></a>Azure Active Directory'de son kullanıcıların uygulamaları Gizle

Son MyApps paneli veya Office 365 Başlatıcısı uygulamalarından gizleme hakkında yönergeler. Uygulama gizli olduğunda, kullanıcılar yine de uygulamayı izinlere sahip olursunuz. 

## <a name="prerequisites"></a>Önkoşullar

Bir uygulamadan MyApps panelinde ve Office 365 Başlatıcısı gizlemek için uygulama yönetici ayrıcalıkları gerekir.

Tüm Office 365 uygulamalarını gizlemek için genel yönetici ayrıcalıkları gerekir.


## <a name="hide-an-application-from-the-end-user"></a>Son kullanıcıdan bir uygulamayı Gizle
MyApps panelinde ve Office 365 uygulama başlatıcısında bir uygulamadan gizlemek için aşağıdaki adımları kullanın.

1.  Oturum [Azure portalında](https://portal.azure.com) dizininizin genel Yöneticisi olarak.
2.  **Azure Active Directory**'yi seçin.
3.  Seçin **kurumsal uygulamalar**. **Kurumsal uygulamalar - tüm uygulamalar** dikey penceresi açılır.
4.  Altında **uygulama türü**seçin **kurumsal uygulamalar**, zaten seçili değilse.
5.  Gizlemek ve uygulama için istediğiniz uygulamayı arayın.  Uygulamanın genel bakış açılır.
6.  **Özellikler**'e tıklayın. 
7.  İçin **kullanıcılara görünür?** soru ye **Hayır**.
8.  **Kaydet**’e tıklayın.


## <a name="hide-office-365-applications-from-the-myapps-panel"></a>MyApps Masası'ndan Office 365 uygulamaları Gizle

Tüm Office 365 uygulamalarını MyApps panelinden gizlemek için aşağıdaki adımları kullanın. Uygulamalar Office 365 portalında hala görülebilir.

1.  Oturum [Azure portalında](https://portal.azure.com) dizininizin genel Yöneticisi olarak.
2.  **Azure Active Directory**'yi seçin.
3.  Seçin **kullanıcı ayarları**.
4.  Altında **kurumsal uygulamalar**, tıklayın **son kullanıcılara nasıl başlatın ve uygulamalarını görüntüleyin yönetin.**
5.  İçin **kullanıcılar yalnızca Office 365 uygulamaları Office 365 portalında görebilir**, tıklayın **Evet**.
6.  **Kaydet**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
* [Tüm Gruplarım bakın](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Kurumsal bir uygulamayı kullanıcı veya grup atama](assign-user-or-group-access-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](remove-user-or-group-access-portal.md)
* [Adını veya kurumsal bir uygulamanın logoyu değiştirme](change-name-or-logo-portal.md)

