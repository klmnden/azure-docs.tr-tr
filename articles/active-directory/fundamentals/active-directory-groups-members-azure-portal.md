---
title: Azure AD'de bir grubun üyelerini yönetme | Microsoft Docs
description: Azure Active Directory'de bir gruba kullanıcı ve cihaz ekleme veya gruptan kaldırma
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 08/28/2017
ms.author: lizross
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: 947b0c11aba211530e3ae25d6617079bcaf2995f
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37860490"
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a>Azure Active Directory kiracınızdaki kullanıcıların grup üyeliğini yönetme
Bu makalede Azure Active Directory'de (Azure AD) bir grubun üyelerini yönetme adımları açıklanmaktadır.

## <a name="how-do-i-find-the-members-and-manage-them"></a>Üyeleri nasıl bulup yönetebilirim?
1. Dizin için genel yönetici olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açın.
2. **Tüm hizmetler**’i seçin, metin kutusuna **Kullanıcılar ve gruplar** yazın ve ardından **Enter**’a basın.

   ![Kullanıcı yönetimini açma](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. **Kullanıcılar ve gruplar** dikey penceresinde **Tüm gruplar**’ı seçin.

   ![Gruplar dikey penceresini açma](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. **Kullanıcılar ve gruplar - Tüm gruplar** dikey penceresinde bir grup seçin.
5. **Grup - *groupname*** dikey penceresinde **Üyeler**’i seçin.

   ![Üyeler dikey penceresini açma](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. Gruba üye eklemek için **Grup - Üyeler** dikey penceresinde **Üye Ekle**’yi seçin.

   ![Üye Ekle komutu](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. **Üyeler** dikey penceresinde gruba eklemek üzere bir veya daha fazla kullanıcı ya da cihaz seçin ve bunları gruba eklemek için dikey pencerenin en altında yer alan **Seç** düğmesini seçin. **Kullanıcı** kutusu görünen sonuçları girişinizle eşleşen kullanıcı veya cihaz adlarını gösterecek şekilde filtreler. Bu kutuda joker karakter kullanılamaz.
8. Gruptan üye kaldırmak için **Grup - Üyeler** dikey penceresinde bir üye seçin.
9. ***membername*** dikey penceresinde **Kaldır** komutunu seçin ve komut isteminde seçiminizi onaylayın.

   ![Üye kaldır komutu](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. Grubun üyelerini değiştirmeyi bitirdiğinizde **Kaydet**’i seçin.

## <a name="additional-information"></a>Ek bilgiler
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Var olan grupları görme](active-directory-groups-view-azure-portal.md)
* [Yeni grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)
* [Bir grubun ayarlarını yönetme](active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyeliklerini yönetme](active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](../users-groups-roles/groups-dynamic-membership.md)
