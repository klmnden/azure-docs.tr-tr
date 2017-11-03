---
title: "Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup ataması kaldırma | Microsoft Docs"
description: "Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup erişimi atamasını kaldırmak nasıl"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7b2d365b-ae92-477f-9702-353cc6acc5ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 99cbc54b4daa988dbf741275ce92d1c863af6720
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup ataması kaldırma
Bir kullanıcı veya grup erişimi Azure Active Directory'de (Azure AD), Kurumsal uygulamalarınızın birini atanmasına kaldırmak daha kolaydır. Kuruluş uygulama yönetmek için uygun izinlere sahip olmalıdır ve dizin için genel yönetici olmanız gerekir.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Bir kullanıcı veya grup ataması nasıl kaldırabilirim?
1. Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2. Seçin **daha fazla hizmet**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
3. Üzerinde **Azure Active Directory - *directoryname***  (diğer bir deyişle, Azure AD dikey yönettiğiniz dizin için), dikey penceresinde seçin **kurumsal uygulamalar**.

    ![Açılış Kurumsal uygulamaları](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. Üzerinde **kurumsal uygulamalar** dikey penceresinde, select **tüm uygulamaları**. Yönetebileceğiniz uygulamaların bir listesini görürsünüz.
5. Üzerinde **kurumsal uygulamalar - tüm uygulamaları** dikey penceresinde, bir uygulama seçin.
6. Üzerinde ***appname*** dikey penceresinde (diğer bir deyişle, dikey penceresinde seçili uygulamasının başlık adı ile) seçin **kullanıcıları ve grupları**.

    ![Kullanıcıları veya grupları seçme](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. Üzerinde ***appname*** **-kullanıcı ve grup atama** dikey penceresinde, daha fazla kullanıcı veya grup seçin ve ardından **kaldırmak** komutu. Kararınızı satırında onaylayın.

    ![Remove komutu seçme](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Tüm my gruplarının bakın](active-directory-groups-view-azure-portal.md)
* [Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)
* [Kullanıcı oturum açma işlemleri bir kurumsal uygulama için devre dışı bırak](active-directory-coreapps-disable-app-azure-portal.md)
* [Adını veya bir kuruluş uygulama logosunu değiştirme](active-directory-coreapps-change-app-logo-user-azure-portal.md)
