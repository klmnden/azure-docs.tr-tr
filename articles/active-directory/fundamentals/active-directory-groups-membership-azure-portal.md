---
title: Azure AD'de grubunuzun ait olduğu grupları yönetme | Microsoft Docs
description: Azure Active Directory'de gruplar diğer grupları içerebilir. Bu üyeliklerin nasıl yönetileceği burada açıklanmıştır.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 10/10/2017
ms.author: lizross
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: 8a71677ae3ceb5617f0a817a8eff438d5e3f2774
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37860442"
---
# <a name="manage-to-which-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a>Azure Active Directory kiracınızda grubunuzun ait olduğu grupları yönetme
Azure Active Directory'de gruplar diğer grupları içerebilir. Bu üyeliklerin nasıl yönetileceği burada açıklanmıştır.

## <a name="how-do-i-find-the-groups-of-which-my-group-is-a-member"></a>Grubumun üyesi olduğu grupları nasıl bulabilirim?
1. Dizin için genel yönetici olan bir hesapla [Azure AD yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
2. **Kullanıcı ve gruplar**'ı seçin.

   ![Kullanıcılar ve gruplar görüntüsünü açma](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
1. **Tüm uygulamalar**’ı seçin.

   ![Gruplar görüntüsünü seçme](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
1. Grup seçin.
2. **Grup üyelikleri**’ni seçin.

   ![Grup üyelikleri görüntüsünü açma](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
1. Grubunuzu başka bir grubun üyesi olarak eklemek için **Grup - Grup üyelikleri** dikey penceresinde **Ekle** komutunu seçin.
2. **Grup Seç** dikey penceresinden bir grup seçin ve ardından dikey pencerenin alt kısmındaki **Seç** düğmesini belirleyin. Grubunuzu bir kerede yalnızca bir gruba ekleyebilirsiniz. **Kullanıcı** kutusu görünen sonuçları girişinizle eşleşen kullanıcı veya cihaz adlarını gösterecek şekilde filtreler. Bu kutuda joker karakter kullanılamaz.

   ![Grup üyeliği ekleme](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. Grubunuzu başka bir grubun üyeliğinden kaldırmak için **Grup - Grup üyelikleri** dikey penceresinde bir grup seçin.
9. **Kaldır** komutunu seçin ve komut isteminde seçiminizi onaylayın.

   ![üyeliği kaldır komutu](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. Grubunuzun grup üyeliklerini değiştirme işleminiz bitince **Kaydet**’i seçin.

## <a name="additional-information"></a>Ek bilgiler
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Var olan grupları görme](active-directory-groups-view-azure-portal.md)
* [Yeni grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)
* [Bir grubun ayarlarını yönetme](active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyelerini yönetme](active-directory-groups-members-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](../users-groups-roles/groups-dynamic-membership.md)
