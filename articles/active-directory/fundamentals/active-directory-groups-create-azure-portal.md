---
title: Azure AD'de kullanıcı grubu oluşturma | Microsoft Docs
description: Azure Active Directory'de grup oluşturma ve gruba üye ekleme
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 08/04/2017
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 82d475e5adadb4e7670f24a6193348c9e1b37a16
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37767582"
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a>Azure Active Directory'de grup oluşturma ve üye ekleme
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [PowerShell](../users-groups-roles/groups-settings-v2-cmdlets.md)

Bu makalede Azure Active Directory'de yeni bir grup oluşturma ve üye ekleme adımları açıklanmaktadır. Aynı anda birkaç kullanıcıya veya cihaza lisans ya da izin atama gibi yönetim görevlerini gerçekleştirmek için grup kullanırsınız.

## <a name="how-do-i-create-a-group"></a>Nasıl grup oluşturulur?
1. Dizin için genel yönetici olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açın.
2. **Tüm hizmetler**’i seçin, metin kutusuna **Kullanıcılar ve gruplar** yazın ve ardından **Enter**’a basın.

   ![Kullanıcı yönetimini açma](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. **Kullanıcılar ve gruplar** dikey penceresinde **Tüm gruplar**’ı seçin.

   ![Gruplar dikey penceresini açma](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. **Kullanıcılar ve gruplar - Tüm gruplar** dikey penceresinde **Ekle** komutunu seçin.

   ![Ekle komutunu seçme](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. **Grup** dikey penceresinde grup için bir ad ve açıklama girin.
6. Gruba eklenecek üyeleri seçmek için **Üyelik türü** kutusunda **Atanmış**'ı ve ardından **Üyeler**'i seçin. Grup üyeliklerini dinamik olarak yönetme hakkında daha fazla bilgi için bkz. [Grup üyeliği için gelişmiş kurallar oluşturmak üzere öznitelikleri kullanma](../active-directory-groups-dynamic-membership-azure-portal.md).

   ![Eklenecek üyeleri seçme](./media/active-directory-groups-create-azure-portal/select-members.png)
7. **Üyeler** dikey penceresinde gruba eklemek üzere bir veya daha fazla kullanıcı ya da cihaz seçin ve bunları gruba eklemek için dikey pencerenin en altında yer alan **Seç** düğmesini seçin. **Kullanıcı** kutusu görünen sonuçları girişinizle eşleşen kullanıcı veya cihaz adlarını gösterecek şekilde filtreler. Bu kutuda joker karakter kullanılamaz.
8. Gruba üye eklemeyi tamamladıktan sonra **Grup** dikey penceresinde **Oluştur**'u seçin.    

   ![Grup oluşturma onayı](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Var olan grupları görme](active-directory-groups-view-azure-portal.md)
* [Bir grubun ayarlarını yönetme](active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyelerini yönetme](active-directory-groups-members-azure-portal.md)
* [Bir grubun üyeliklerini yönetme](active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](../active-directory-groups-dynamic-membership-azure-portal.md)
