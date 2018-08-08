---
title: Dizin rolleri Azure AD PIM kullanarak kullanıcılara atama | Microsoft Docs
description: Dizin rolleri Azure Active Directory Privileged Identity Management ve Azure portalını kullanarak kullanıcılara atama konusunda bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 07/23/2018
ms.author: rolyon
ms.openlocfilehash: 1aede38cabba7f9811f2b9320bc1e9a9da857f08
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621822"
---
# <a name="assign-directory-roles-to-users-using-azure-ad-pim"></a>Dizin rolleri Azure AD PIM kullanarak kullanıcılara ata

Azure Active Directory'ye (Azure AD), genel yönetici yapabilirsiniz **kalıcı** dizini rol atamaları. Bu rol atamaları kullanılarak oluşturulabilir. [Azure portalında](../users-groups-roles/directory-assign-admin-roles.md) veya bu adı kullanıyor [PowerShell komutlarını](/powershell/module/azuread#directory_roles).

Azure AD Privileged Identity Management (PIM) hizmeti, ayrıcalıklı rol yöneticileri'kalıcı dizini rol atamaları yapmak de sağlar. Ayrıca, ayrıcalıklı rol Yöneticileri kullanıcıların yapabileceğini **uygun** Dizin rolleri için. Uygun yönetici rolü, ihtiyaç duydukları ve bunlar bitirdiğinizde izinlerini süresi dolacak etkinleştirebilirsiniz. PIM kullanarak yönetebileceğiniz rolleri hakkında daha fazla bilgi için bkz: [dizin rollerini Azure AD PIM kullanarak yönetebileceğiniz](pim-roles.md).

## <a name="make-a-user-eligible-for-a-role"></a>Bir kullanıcı rolü için uygun olarak ayarla

Bir kullanıcı için Azure AD directory rolüne uygun hale getirmek için aşağıdaki adımları izleyin.

1. Oturum [Azure portalında](https://portal.azure.com/) üyesi olan bir kullanıcı ile [ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rol.

    PIM yönetmek için başka bir kullanıcı erişim verme konusunda daha fazla bilgi için bkz: [PIM erişimi verme nasıl](pim-how-to-give-access-to-pim.md).

1. Açık **Azure AD Privileged Identity Management**.

    Git, PIM Azure portalında henüz etkinleştirmediyseniz [Azure AD PIM ile çalışmaya başlama](pim-getting-started.md).

1. Tıklayın **Azure AD Dizin rolleri**.

1. Tıklayın **rol (Önizleme)** veya **üyeleri**.

    ![Azure AD dizin rolleri](./media/pim-how-to-add-role-to-user/pim-directory-roles.png)

1. Tıklayın **Üye Ekle** üyeleri Ekle açmak için yönetilen.

1. Tıklayın **bir rol seçin**, yönetmek ve ardından istediğiniz bir rolünü **seçin**.

    ![Rol seçin](./media/pim-how-to-add-role-to-user/pim-select-a-role.png)

1. Tıklayın **üyelerini seçin**, role atamak ve ardından istediğiniz kullanıcıları seçin **seçin**.

    ![Rol seçin](./media/pim-how-to-add-role-to-user/pim-select-members.png)

1. Yönetilen üyeleri Ekle **Tamam** rolüne kullanıcı eklemek için.

     Rol atandığında, seçtiğiniz kullanıcı üye listesi görünür **uygun** rolü için.

    ![Kullanıcı rolü için uygun](./media/pim-how-to-add-role-to-user/pim-directory-role-eligible.png)

1. Kullanıcı rolü için uygundur, bunlar bu yönergeleri göre etkinleştirmelerini bildirmek [etkinleştirmek veya bir rolü devre dışı bırakma](pim-how-to-activate-role.md).

    Uygun Yöneticiler, etkinleştirme sırasında Azure multi-Factor Authentication (MFA) kaydetmek için istenir. Bir kullanıcı için mfa'yı kaydedilemiyor veya bir Microsoft hesabı kullanarak, (genellikle @outlook.com), kendi rolleri içinde kalıcı yapmanız gerekir.

## <a name="make-a-role-assignment-permanent"></a>Bir rol ataması kalıcı yap

Varsayılan olarak, yeni kullanıcılar yalnızca bir dizin rolüne için uygundur. Bir rol ataması kalıcı hale getirmek isterseniz bu adımları izleyin.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure AD Dizin rolleri**.

1. Tıklayın **üyeleri**.

    ![Üye listesi](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. ' A tıklayın bir **uygun** kalıcı hale getirmek istediğiniz rol.

1. Tıklayın **daha fazla** ve ardından **yapma izni**.

    ![Rol ataması kalıcı yap](./media/pim-how-to-add-role-to-user/pim-make-perm.png)

    Rol olarak listelendiğine **kalıcı**.

    ![Kalıcı değişiklikle üyelerin listesi](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members-permanent.png)

## <a name="remove-a-user-from-a-role"></a>Bir kullanıcıyı rolden Kaldır

Rol ataması kullanıcıları kaldırmak, ancak her zaman kalıcı bir genel yönetici olan en az bir kullanıcı olduğundan emin olun. Hangi kullanıcıların rol atamalarının yine emin değilseniz yapabilecekleriniz [rol için erişim değerlendirmesi başlatma](pim-how-to-start-security-review.md).

Belirli bir kullanıcı bir dizin rolünden kaldırmak için aşağıdaki adımları izleyin.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure AD Dizin rolleri**.

1. Tıklayın **üyeleri**.

    ![Üye listesi](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Kaldırmak istediğiniz bir rol ataması'na tıklayın.

1. Tıklayın **daha fazla** ve ardından **Kaldır**.

    ![Bir rolü Kaldır](./media/pim-how-to-add-role-to-user/pim-remove-role.png)

1. Onaylamanızı ister iletisinde tıklayın **Evet**.

    ![Bir rolü Kaldır](./media/pim-how-to-add-role-to-user/pim-remove-role-confirm.png)

    Rol ataması kaldırıldı.

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]
