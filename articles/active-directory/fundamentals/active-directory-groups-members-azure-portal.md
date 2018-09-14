---
title: Azure Active Directory'yi kullanarak grubuna üye ekleme veya kaldırma nasıl | Microsoft Docs
description: Ekleme veya kullanıcıları ve cihazları Azure Active Directory'yi kullanarak bir gruptan kaldırmak nasıl.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/23/2018
ms.author: lizross
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: f9244e1285396a2d5de40b596d47e311efa50b83
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574242"
---
# <a name="how-to-add-or-remove-group-members-using-azure-active-directory"></a>Nasıl yapılır: Azure Active Directory'yi kullanarak grubuna üye ekleme veya kaldırma
Azure Active Directory'yi kullanarak, ekleme ve grubu üyelerini kaldırma devam edebilirsiniz.

## <a name="to-add-group-members"></a>Grup üyeleri eklemek için

1. Oturum [Azure portalında](https://portal.azure.com) dizinde genel yönetici hesabını kullanarak.

2. Seçin **Azure Active Directory**ve ardından **grupları**.

3. Gelen **gruplar - tüm gruplar** sayfasında arayın ve üye eklemek istediğiniz grubu seçin. Bu durumda, önceden oluşturulmuş grubumuz kullanın **MDM İlkesi - Batı**.

    ![Tüm grupları grupları sayfasında grubu adı vurgulanmış](media/active-directory-groups-members-azure-portal/group-all-groups-screen.png)

4. Gelen **MDM İlkesi - Batı genel bakış** sayfasında **üyeleri** gelen **Yönet** alan.

    ![MDM İlkesi - üyeleri seçeneğinin vurgulandığı Batı genel bakış sayfası](media/active-directory-groups-members-azure-portal/group-overview-blade.png)

5. Seçin **üye ekleme**, her gruba eklemeniz ve ardından istediğiniz üyeleri seçin ve ardından arama **seçin**.

    Üye başarıyla eklendi belirten bir ileti alırsınız.

    ![Üyeleri sayfa ekleme ile gösterilen üyesi için aranır.](media/active-directory-groups-members-azure-portal/update-members.png)

6. Gruba üye adlarını görmek için ekranı yenileyin.

## <a name="to-remove-group-members"></a>Grup üyeleri kaldırmak için

1. Gelen **gruplar - tüm gruplar** sayfasında arayın ve üye kaldırmak istediğiniz grubu seçin. Tekrar kullanacağız, **MDM İlkesi - Batı**.

2. Seçin **üyeleri** gelen **Yönet** alanında, arama ve kaldırın ve ardından üyenin adını işaretleyin **Kaldır**.

    ![Üye bilgileri sayfası, Kaldır seçeneği](media/active-directory-groups-members-azure-portal/remove-members-from-group.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Gruplar ve üyeler görüntüleyin](active-directory-groups-view-azure-portal.md)

- [Grup ayarlarınızı düzenleyin](active-directory-groups-settings-azure-portal.md)

- [Grupları kullanarak kaynaklara erişimi yönetme](active-directory-manage-groups.md)

- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](../users-groups-roles/groups-create-rule.md)

- [Azure Active Directory'ye bir Azure aboneliği ekleme veya ilişkilendirme](active-directory-how-subscriptions-associated-directory.md)
