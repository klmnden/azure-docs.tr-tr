---
title: "Grubunuza ait Azure Active Directory içinde grupları yönetme | Microsoft Docs"
description: "Gruplar, diğer grupları Azure Active Directory'de içerebilir. Aşağıda, bu üyeliklerin yönetme verilmiştir."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: e785c2d0-7724-47d4-a56e-c58280c08a14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 251289d2a70f824714789fdf2fb484949745d6d7
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="manage-to-which-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a>Yönetmek için hangi grupların Azure Active Directory kiracınızda bir grup ait
Gruplar, diğer grupları Azure Active Directory'de içerebilir. Aşağıda, bu üyeliklerin yönetme verilmiştir.

## <a name="how-do-i-find-the-groups-of-which-my-group-is-a-member"></a>Grubunuza üye olduğu grupları nasıl bulabilirim?
1. Dizin için genel yönetici olan bir hesapla [Azure AD yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
2. Seçin **kullanıcılar ve gruplar**.

   ![Açılış kullanıcıları ve grupları görüntüsü](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
1. Seçin **tüm grupları**.

   ![Grupları görüntüsü seçin](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
1. Bir grubu seçin.
2. Seçin **grup üyeliklerini**.

   ![Açılış grup üyeliklerini görüntüsü](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
1. Grubunuzun başka bir grubun üyesi olarak eklemek için **Grup - grup üyeliklerini** dikey penceresinde, select **Ekle** komutu.
2. Bir grup seçin **Grup Seç** dikey ve ardından **seçin** dikey pencerenin altındaki düğmesini. Grubunuzun aynı anda yalnızca bir gruba ekleyebilirsiniz. **Kullanıcı** kutusu, bir kullanıcı veya aygıt adı herhangi bir kısmını girişe eşleşmesini temel alan görüntü filtreler. Joker karakterler bu kutuya kabul edilir.

   ![Bir grup üyeliği ekleme](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. Başka bir grubun üyesi olarak grubunuzun kaldırmak için **Grup - grup üyeliklerini** dikey penceresinde, bir grup seçin.
9. Seçin **kaldırmak** komut ve komut isteminde Seçiminizi onaylayın.

   ![Üyelik komutu kaldırın](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. Grubunuz için grup üyeliklerini değiştirme işiniz bittiğinde, seçin **kaydetmek**.

## <a name="additional-information"></a>Ek bilgiler
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Var olan grupları bakın](active-directory-groups-view-azure-portal.md)
* [Yeni bir grup oluşturun ve üye ekleme](active-directory-groups-create-azure-portal.md)
* [Bir grubu ayarlarını yönetme](active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyelerini yönetmek](active-directory-groups-members-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kurallarını yönet](active-directory-groups-dynamic-membership-azure-portal.md)
