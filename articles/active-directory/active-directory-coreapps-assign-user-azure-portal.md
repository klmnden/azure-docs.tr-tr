---
title: "Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup atamak | Microsoft Docs"
description: "Bir kullanıcı veya grup için Azure Active Directory'de atamak için bir kuruluş uygulama seçme"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5817ad48-d916-492b-a8d0-2ade8c50a224
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: asteen
ms.openlocfilehash: 8e61044f261033a473241e2de152026bf49c4c70
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a>Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup atayın
Kurumsal uygulamalarınızda Azure Active Directory (Azure AD) bir kullanıcıya veya gruba atamak kolaydır. Kuruluş uygulama yönetmek için uygun izinlere sahip olmalıdır ve dizin için genel yönetici olmanız gerekir.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>Bir kurumsal uygulama için nasıl kullanıcı erişimi atamak?
1. Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2. Seçin **daha fazla hizmet**, Azure Active Directory metin kutusuna girin ve ardından **Enter**.
3. Üzerinde **Azure Active Directory - *directoryname***  (diğer bir deyişle, Azure AD dikey yönettiğiniz dizin için), dikey penceresinde seçin **kurumsal uygulamalar**.

    ![Açılış Kurumsal uygulamaları](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. Üzerinde **kurumsal uygulamalar** dikey penceresinde, select **tüm uygulamaları**. Yönetebileceğiniz uygulamaların bir listesini görürsünüz.
5. Üzerinde **kurumsal uygulamalar - tüm uygulamaları** dikey penceresinde, bir uygulama seçin.
6. Üzerinde ***appname*** dikey penceresinde (diğer bir deyişle, dikey penceresinde seçili uygulamasının başlık adı ile) seçin **kullanıcıları ve grupları**.

    ![Tüm uygulamalar komutu seçme](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. Üzerinde ***appname*** **-kullanıcı ve grup atama** dikey penceresinde, select **Ekle** komutu.
8. Üzerinde **eklemek atama** dikey penceresinde, select **kullanıcılar ve gruplar**.

    ![Bir kullanıcı veya grup için uygulama atama](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. Üzerinde **kullanıcılar ve gruplar** dikey penceresinde, listeden bir veya daha fazla kullanıcı veya grup seçin ve ardından **seçin** dikey pencerenin altındaki düğmesini.
10. Üzerinde **eklemek atama** dikey penceresinde, select **rol**. Ardından **rolü Seç** dikey penceresinde, seçilen kullanıcılarla veya gruplarla uygulayın ve ardından için rolü seçin **Tamam** dikey pencerenin altındaki düğmesini.
11. Üzerinde **eklemek atama** dikey penceresinde, select **atamak** dikey pencerenin altındaki düğmesini. Atanan kullanıcılar veya gruplar bu kurumsal uygulama için seçili rol tarafından tanımlanan izinlerine sahip olacaktır.

## <a name="next-steps"></a>Sonraki adımlar
* [Tüm my gruplarının bakın](active-directory-groups-view-azure-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Kullanıcı oturum açma işlemleri bir kurumsal uygulama için devre dışı bırak](active-directory-coreapps-disable-app-azure-portal.md)
* [Adını veya bir kuruluş uygulama logosunu değiştirme](active-directory-coreapps-change-app-logo-user-azure-portal.md)
