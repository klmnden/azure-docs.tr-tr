---
title: Azure AD dizin rollerini PIM atama | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM), Azure AD Dizin rolleri atama hakkında bilgi edinin.
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
ms.openlocfilehash: 33bfe28bf612c47c9f42345dabccc017337c3d45
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190165"
---
# <a name="assign-azure-ad-directory-roles-in-pim"></a>Azure AD dizin rollerini PIM atayın

Azure Active Directory'ye (Azure AD), genel yönetici yapabilirsiniz **kalıcı** dizini rol atamaları. Bu rol atamaları kullanılarak oluşturulabilir. [Azure portalında](../users-groups-roles/directory-assign-admin-roles.md) veya bu adı kullanıyor [PowerShell komutlarını](/powershell/module/azuread#directory_roles).

Azure AD Privileged Identity Management (PIM) hizmeti, ayrıcalıklı rol yöneticileri'kalıcı dizini rol atamaları yapmak de sağlar. Ayrıca, ayrıcalıklı rol Yöneticileri kullanıcıların yapabileceğini **uygun** Dizin rolleri için. Uygun yönetici rolü, ihtiyaç duydukları ve bunlar bitirdiğinizde izinlerini süresi dolacak etkinleştirebilirsiniz. PIM kullanarak yönetebileceğiniz rolleri hakkında daha fazla bilgi için bkz: [Azure AD dizin rollerini PIM içinde yönetebileceğiniz](pim-roles.md).

## <a name="make-a-user-eligible-for-a-role"></a>Bir kullanıcı rolü için uygun olarak ayarla

Bir kullanıcı için Azure AD directory rolüne uygun hale getirmek için aşağıdaki adımları izleyin.

1. Oturum [Azure portalında](https://portal.azure.com/) üyesi olan bir kullanıcı ile [ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rol.

    PIM yönetmek için başka bir yönetici erişim verme konusunda daha fazla bilgi için bkz: [PIM yönetmek için diğer yöneticilere erişim ver](pim-how-to-give-access-to-pim.md).

1. Açık **Azure AD Privileged Identity Management**.

    Azure portalında PIM henüz başlamamış, Git [PIM kullanmaya başlamak](pim-getting-started.md).

1. Tıklayın **Azure AD Dizin rolleri**.

1. Tıklayın **rolleri** veya **üyeleri**.

    ![Azure AD dizin rolleri](./media/pim-how-to-add-role-to-user/pim-directory-roles.png)

1. Tıklayın **Üye Ekle** üyeleri Ekle açmak için yönetilen.

1. Tıklayın **bir rol seçin**, yönetmek ve ardından istediğiniz bir rolünü **seçin**.

    ![Rol seçin](./media/pim-how-to-add-role-to-user/pim-select-a-role.png)

1. Tıklayın **üyelerini seçin**, role atamak ve ardından istediğiniz kullanıcıları seçin **seçin**.

    ![Rol seçin](./media/pim-how-to-add-role-to-user/pim-select-members.png)

1. Yönetilen üyeleri Ekle **Tamam** rolüne kullanıcı eklemek için.

1. Rolleri listesinde yalnızca üye listesini görmek için atanan role tıklayın.

     Rol atandığında, seçtiğiniz kullanıcı üye listesi görünür **uygun** rolü için.

    ![Kullanıcı rolü için uygun](./media/pim-how-to-add-role-to-user/pim-directory-role-eligible.png)

1. Kullanıcı rolü için uygundur, bunlar bu yönergeleri göre etkinleştirmelerini bildirmek [Azure AD dizin rollerim PIM etkinleştirme](pim-how-to-activate-role.md).

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

- [PIM'de Azure AD dizini rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
- [PIM Azure kaynak Rolleri Ata](pim-resource-roles-assign-roles.md)
