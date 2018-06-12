---
title: Adını veya bir kuruluş uygulama Azure Active Directory'de logosunu değiştirme | Microsoft Docs
description: Adı veya logosu Azure Active Directory'de özel Kurumsal uygulama için nasıl değiştirileceğini
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
ms.topic: article
ms.date: 08/28/2017
ms.author: barbkess
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: ad424d6ca8ea8c35aa502a3d1bd98940591c38e8
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35303897"
---
# <a name="change-the-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a>Adını veya bir kuruluş uygulama Azure Active Directory'de logosunu değiştirme
Ad veya Azure Active Directory'de (Azure AD) bir özel Kurumsal uygulama için logosunu değiştirmek çok kolaydır. Bu değişiklikleri yapmak için uygun izinlere sahip olmalıdır ve özel uygulama oluşturucusu olmalıdır.

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a>Bir kurumsal uygulamanın adı veya logosu nasıl değiştiririm?
1. Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2. Seçin **tüm hizmetleri**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
3. Üzerinde **Azure Active Directory - *directoryname***  bölmesi (diğer bir deyişle, Azure AD yönettiğiniz dizin için), seçin **kurumsal uygulamalar**.

    ![Açılış Kurumsal uygulamaları](./media/change-name-or-logo-portal/open-enterprise-apps.png)
4. Üzerinde **kurumsal uygulamalar** bölmesinde, **tüm uygulamaları**. Yönetebileceğiniz uygulamaların bir listesini görürsünüz.
5. Üzerinde **kurumsal uygulamalar - tüm uygulamaları** bölmesinde, bir uygulama seçin.
6. Üzerinde ***appname*** bölmesi (diğer bir deyişle, başlık seçilen uygulamanın adını), seçin **özellikleri**.

    ![Özellikler komutu seçme](./media/change-name-or-logo-portal/select-app.png)
7. Üzerinde ***appname*** **-özellikleri** bölmesinde, yeni bir logo olarak kullanın veya uygulama adını veya her ikisi de düzenlemek bir dosya için göz atın.

    ![Uygulama logosu veya nameproperties komutu değiştirme](./media/change-name-or-logo-portal/change-logo.png)
8. Seçin **kaydetmek** komutu.

## <a name="next-steps"></a>Sonraki adımlar
* [Tüm my gruplarının bakın](../active-directory-groups-view-azure-portal.md)
* [Bir kullanıcı veya grup için bir kuruluş uygulama atama](assign-user-or-group-access-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](remove-user-or-group-access-portal.md)
* [Kullanıcı oturum açma işlemleri bir kurumsal uygulama için devre dışı bırak](disable-user-sign-in-portal.md)
