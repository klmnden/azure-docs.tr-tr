---
title: Azure Active Directory'deki bir gruba üye yönetme | Microsoft Docs
description: Ekleme veya Azure Active Directory'deki bir gruba kullanıcı ve cihazları kaldırma
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: a35e85feb10997f458c7f3764920891e7932de59
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a>Azure Active Directory kiracınızdaki kullanıcıların Grup üyeliğini yönetme
Bu makalede, Azure Active Directory'de (Azure AD) bir grup üyelerini yönetmek açıklanmaktadır.

## <a name="how-do-i-find-the-members-and-manage-them"></a>Nasıl üyeleri bulmak ve onları yönetmeyi?
1. Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2. Seçin **tüm hizmetleri**, girin **kullanıcılar ve gruplar** metin kutusuna ve ardından **Enter**.

   ![Açılış kullanıcı yönetimi](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. Üzerinde **kullanıcılar ve gruplar** dikey penceresinde, select **tüm grupları**.

   ![Grupları dikey penceresini açma](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. Üzerinde **kullanıcılar ve gruplar - tüm grupları** dikey penceresinde, bir grup seçin.
5. Üzerinde **grup - *groupname***  dikey penceresinde, select **üyeleri**.

   ![Üyeleri dikey penceresini açma](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. Üzerinde grubuna üye eklemek için **Grup - üyeleri** dikey penceresinde, select **Add Members**.

   ![Üyeleri komut ekleme](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. Üzerinde **üyeleri** dikey penceresinde, select bir veya daha fazla kullanıcılara veya cihazlara grubuna eklemeniz ve seçin **seçin** grubuna eklemek için dikey pencerenin altındaki düğmesini. **Kullanıcı** kutusu, bir kullanıcı veya aygıt adı herhangi bir kısmını girişe eşleşmesini temel alan görüntü filtreler. Joker karakterler bu kutuya kabul edilir.
8. Üzerinde grubundan üye kaldırma **Grup - üyeleri** dikey penceresinde, bir üye seçin.
9. Üzerinde ***membername*** dikey penceresinde, select **kaldırmak** komut ve komut isteminde Seçiminizi onaylayın.

   ![Üyeleri komutu kaldırın](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. Grubunun üyelerini değiştirmeyi tamamladığınızda seçin **kaydetmek**.

## <a name="additional-information"></a>Ek bilgiler
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Var olan grupları bakın](active-directory-groups-view-azure-portal.md)
* [Yeni bir grup oluşturun ve üye ekleme](active-directory-groups-create-azure-portal.md)
* [Bir grubu ayarlarını yönetme](active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyeliğini yönetme](active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kurallarını yönet](active-directory-groups-dynamic-membership-azure-portal.md)
