---
title: Adını veya kurumsal bir uygulamayı Azure Active Directory'de logosunu değiştirme | Microsoft Docs
description: Adı veya Azure Active Directory içinde bir özel Kurumsal uygulaması için logoyu değiştirme
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/28/2017
ms.author: celested
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 62578fe037dc1c9672bd0a4cf28500c658344c53
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60440400"
---
# <a name="change-the-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a>Adını veya kurumsal bir uygulamayı Azure Active Directory'de logoyu değiştirme
Adı veya logosu özel kuruluş uygulaması Azure Active Directory'de (Azure AD) değiştirmek kolay bir işlemdir. Bu değişiklikleri yapmak için uygun izinlere sahip olmalıdır ve özel uygulama oluşturucusu olmalıdır.

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a>Bir kurumsal uygulamanın adı veya logosu nasıl değiştirebilirim?
1. Dizin için genel yönetici olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
3. Üzerinde **Azure Active Directory - *directoryname***  bölmesi (diğer bir deyişle, Azure AD dizini yönettiğiniz için), seçin **kurumsal uygulamalar**.

    ![Açılış kurumsal uygulamalar](./media/change-name-or-logo-portal/open-enterprise-apps.png)
4. Üzerinde **kurumsal uygulamalar** bölmesinde **tüm uygulamaları**. Yönetebileceğiniz uygulamaların listesini görürsünüz.
5. Üzerinde **kurumsal uygulamalar - tüm uygulamalar** bölmesinde bir uygulama seçin.
6. Üzerinde ***appname*** bölmesi (diğer bir deyişle, başlık seçilen uygulamanın adını), seçin **özellikleri**.

    ![Özellikler komutu seçme](./media/change-name-or-logo-portal/select-app.png)
7. Üzerinde ***appname*** **-Özellikler** bölmesinde, yeni logo olarak kullanın veya uygulama adı veya her ikisi de düzenlemek bir dosya için Gözat.

    ![Uygulama logosu veya nameproperties komutu değiştirme](./media/change-name-or-logo-portal/change-logo.png)
8. Seçin **Kaydet** komutu.

## <a name="next-steps"></a>Sonraki adımlar
* [Tüm Gruplarım](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Kurumsal bir uygulamayı kullanıcı veya grup atama](assign-user-or-group-access-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](remove-user-or-group-access-portal.md)
* [Kullanıcı oturum açma Kurumsal uygulama için devre dışı bırak](disable-user-sign-in-portal.md)
