---
title: Adını veya bir kuruluş uygulama Azure Active Directory'de logosunu değiştirme | Microsoft Docs
description: Adı veya logosu Azure Active Directory'de özel Kurumsal uygulama için nasıl değiştirileceğini
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: d01303ce-e6cb-4f3b-a4d6-ec29dfd68146
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 18e5555d4eaf49fceb5aec673eedce8ed90d46a3
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="change-the-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a>Adını veya bir kuruluş uygulama Azure Active Directory'de logosunu değiştirme
Ad veya Azure Active Directory'de (Azure AD) bir özel Kurumsal uygulama için logosunu değiştirmek çok kolaydır. Bu değişiklikleri yapmak için uygun izinlere sahip olmalıdır ve özel uygulama oluşturucusu olmalıdır.

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a>Bir kurumsal uygulamanın adı veya logosu nasıl değiştiririm?
1. Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2. Seçin **tüm hizmetleri**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
3. Üzerinde **Azure Active Directory - *directoryname***  bölmesi (diğer bir deyişle, Azure AD yönettiğiniz dizin için), seçin **kurumsal uygulamalar**.

    ![Açılış Kurumsal uygulamaları](./media/active-directory-coreapps-change-app-logo-azure-portal/open-enterprise-apps.png)
4. Üzerinde **kurumsal uygulamalar** bölmesinde, **tüm uygulamaları**. Yönetebileceğiniz uygulamaların bir listesini görürsünüz.
5. Üzerinde **kurumsal uygulamalar - tüm uygulamaları** bölmesinde, bir uygulama seçin.
6. Üzerinde ***appname*** bölmesi (diğer bir deyişle, başlık seçilen uygulamanın adını), seçin **özellikleri**.

    ![Özellikler komutu seçme](./media/active-directory-coreapps-change-app-logo-azure-portal/select-app.png)
7. Üzerinde ***appname*** **-özellikleri** bölmesinde, yeni bir logo olarak kullanın veya uygulama adını veya her ikisi de düzenlemek bir dosya için göz atın.

    ![Uygulama logosu veya nameproperties komutu değiştirme](./media/active-directory-coreapps-change-app-logo-azure-portal/change-logo.png)
8. Seçin **kaydetmek** komutu.

## <a name="next-steps"></a>Sonraki adımlar
* [Tüm my gruplarının bakın](active-directory-groups-view-azure-portal.md)
* [Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Kullanıcı oturum açma işlemleri bir kurumsal uygulama için devre dışı bırak](active-directory-coreapps-disable-app-azure-portal.md)
