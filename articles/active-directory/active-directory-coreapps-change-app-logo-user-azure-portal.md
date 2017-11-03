---
title: "Adını veya bir kuruluş uygulama Azure Active Directory'de logosunu değiştirme | Microsoft Docs"
description: "Adı veya logosu Azure Active Directory'de özel Kurumsal uygulama için nasıl değiştirileceğini"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d01303ce-e6cb-4f3b-a4d6-ec29dfd68146
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 1345f77df1945d3fa5bc7adc185ee5e6b6c0cc3f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="change-the-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a>Adını veya bir kuruluş uygulama Azure Active Directory'de logosunu değiştirme
Ad veya Azure Active Directory'de (Azure AD) bir özel Kurumsal uygulama için logosunu değiştirmek çok kolaydır. Bu değişiklikleri yapmak için uygun izinlere sahip olmalıdır ve özel uygulama oluşturucusu olmalıdır.

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a>Bir kurumsal uygulamanın adı veya logosu nasıl değiştiririm?
1. Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2. Seçin **daha fazla hizmet**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
3. Üzerinde **Azure Active Directory - *directoryname***  (diğer bir deyişle, Azure AD dikey yönettiğiniz dizin için), dikey penceresinde seçin **kurumsal uygulamalar**.

    ![Açılış Kurumsal uygulamaları](./media/active-directory-coreapps-change-app-logo-azure-portal/open-enterprise-apps.png)
4. Üzerinde **kurumsal uygulamalar** dikey penceresinde, select **tüm uygulamaları**. Yönetebileceğiniz uygulamaların bir listesini görürsünüz.
5. Üzerinde **kurumsal uygulamalar - tüm uygulamaları** dikey penceresinde, bir uygulama seçin.
6. Üzerinde ***appname*** dikey penceresinde (diğer bir deyişle, dikey penceresinde seçili uygulamasının başlık adı ile) seçin **özellikleri**.

    ![Özellikler komutu seçme](./media/active-directory-coreapps-change-app-logo-azure-portal/select-app.png)
7. Üzerinde ***appname*** **-özellikleri** dikey penceresinde, yeni bir logo olarak kullanın veya uygulama adını veya her ikisi de düzenlemek bir dosya için göz atın.

    ![Uygulama logosu veya nameproperties komutu değiştirme](./media/active-directory-coreapps-change-app-logo-azure-portal/change-logo.png)
8. Seçin **kaydetmek** komutu.

## <a name="next-steps"></a>Sonraki adımlar
* [Tüm my gruplarının bakın](active-directory-groups-view-azure-portal.md)
* [Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Kullanıcı oturum açma işlemleri bir kurumsal uygulama için devre dışı bırak](active-directory-coreapps-disable-app-azure-portal.md)
