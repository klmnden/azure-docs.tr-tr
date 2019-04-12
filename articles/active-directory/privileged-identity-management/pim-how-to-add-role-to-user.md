---
title: Azure AD PIM - Azure Active Directory rolleri atama | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure AD rolleri atama hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 07259d90c7119dec4ca9139e10af2fb20a439425
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59492419"
---
# <a name="assign-azure-ad-roles-in-pim"></a>Azure AD PIM Rolleri Ata

Azure Active Directory'ye (Azure AD), genel yönetici yapabilirsiniz **kalıcı** Azure AD yönetici rol atamaları. Bu rol atamaları kullanılarak oluşturulabilir. [Azure portalında](../users-groups-roles/directory-assign-admin-roles.md) veya bu adı kullanıyor [PowerShell komutlarını](/powershell/module/azuread#directory_roles).

Azure AD Privileged Identity Management (PIM) hizmeti, rol atamalarını kalıcı yönetici yap ayrıcalıklı rol yöneticileri de sağlar. Ayrıca, ayrıcalıklı rol Yöneticileri kullanıcıların yapabileceğini **uygun** Azure AD yönetim rolleri için. Uygun yönetici rolü, ihtiyaç duydukları ve bunlar bitirdiğinizde izinlerini süresi dolacak etkinleştirebilirsiniz.

## <a name="make-a-user-eligible-for-a-role"></a>Bir kullanıcı rolü için uygun olarak ayarla

Bir kullanıcı için Azure AD Yönetici rolüne uygun hale getirmek için aşağıdaki adımları izleyin.

1. Oturum [Azure portalında](https://portal.azure.com/) üyesi olan bir kullanıcı ile [ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rol.

    PIM yönetmek için başka bir yönetici erişim verme konusunda daha fazla bilgi için bkz: [PIM yönetmek için diğer yöneticilere erişim ver](pim-how-to-give-access-to-pim.md).

1. Açık **Azure AD Privileged Identity Management**.

    Azure portalında PIM henüz başlamamış, Git [PIM kullanmaya başlamak](pim-getting-started.md).

1. Tıklayın **Azure AD rolleri**.

1. Tıklayın **rolleri** veya **üyeleri**.

    ![Azure AD rolleri](./media/pim-how-to-add-role-to-user/pim-directory-roles.png)

1. Tıklayın **Üye Ekle** üyeleri Ekle açmak için yönetilen.

1. Tıklayın **bir rol seçin**, yönetmek ve ardından istediğiniz bir rolünü **seçin**.

    ![Rol seçin](./media/pim-how-to-add-role-to-user/pim-select-a-role.png)

1. Tıklayın **üyelerini seçin**, role atamak ve ardından istediğiniz kullanıcıları seçin **seçin**.

    ![Rol seçin](./media/pim-how-to-add-role-to-user/pim-select-members.png)

1. Yönetilen üyeleri Ekle **Tamam** rolüne kullanıcı eklemek için.

1. Rolleri listesinde yalnızca üye listesini görmek için atanan role tıklayın.

     Rol atandığında, seçtiğiniz kullanıcı üye listesi görünür **uygun** rolü için.

    ![Kullanıcı rolü için uygun](./media/pim-how-to-add-role-to-user/pim-directory-role-eligible.png)

1. Kullanıcı rolü için uygundur, bunlar bu yönergeleri göre etkinleştirmelerini bildirmek [PIM Azure AD'ye rollerimi etkinleştir](pim-how-to-activate-role.md).

    Uygun Yöneticiler, etkinleştirme sırasında Azure multi-Factor Authentication (MFA) kaydetmek için istenir. Bir kullanıcı için mfa'yı kaydedilemiyor veya bir Microsoft hesabı kullanarak, (genellikle @outlook.com), kendi rolleri içinde kalıcı yapmanız gerekir.

## <a name="make-a-role-assignment-permanent"></a>Bir rol ataması kalıcı yap

Varsayılan olarak, yeni kullanıcılar yalnızca Azure AD Yönetici rolüne için uygundur. Bir rol ataması kalıcı hale getirmek isterseniz bu adımları izleyin.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure AD rolleri**.

1. Tıklayın **üyeleri**.

    ![Üye listesi](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. ' A tıklayın bir **uygun** kalıcı hale getirmek istediğiniz rol.

1. Tıklayın **daha fazla** ve ardından **yapma izni**.

    ![Rol ataması kalıcı yap](./media/pim-how-to-add-role-to-user/pim-make-perm.png)

    Rol olarak listelendiğine **kalıcı**.

    ![Kalıcı değişiklikle üyelerin listesi](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members-permanent.png)

## <a name="remove-a-user-from-a-role"></a>Bir kullanıcıyı rolden Kaldır

Rol ataması kullanıcıları kaldırmak, ancak her zaman kalıcı bir genel yönetici olan en az bir kullanıcı olduğundan emin olun. Hangi kullanıcıların rol atamalarının yine emin değilseniz yapabilecekleriniz [rol için erişim değerlendirmesi başlatma](pim-how-to-start-security-review.md).

Belirli bir kullanıcının bir Azure AD yönetici rolünden kaldırmak için aşağıdaki adımları izleyin.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure AD rolleri**.

1. Tıklayın **üyeleri**.

    ![Üye listesi](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Kaldırmak istediğiniz bir rol ataması'na tıklayın.

1. Tıklayın **daha fazla** ve ardından **Kaldır**.

    ![Bir rolü Kaldır](./media/pim-how-to-add-role-to-user/pim-remove-role.png)

1. Onaylamanızı ister iletisinde tıklayın **Evet**.

    ![Bir rolü Kaldır](./media/pim-how-to-add-role-to-user/pim-remove-role-confirm.png)

    Rol ataması kaldırıldı.

## <a name="authorization-error-when-assigning-roles"></a>Rol atamasını yaparken Yetkilendirme hatası

MS-PIM hizmet ilkesi henüz uygun izinlere sahip olmadığından bir abonelik için en son PIM etkin ve bir kullanıcı için Azure AD Yönetici rolüne uygun yapmayı denediğinizde bir Yetkilendirme hatası alırsanız olabilir. MS-PIM hizmet ilkesi olmalıdır [kullanıcı erişimi Yöneticisi](../../role-based-access-control/built-in-roles.md#user-access-administrator) başkalarına rolleri atamak için rol. PIM MS kullanıcı erişimi yöneticisi rolü atanmış kadar beklemek yerine uygulamayı el ile atayabilirsiniz.

MS-PIM hizmet sorumlusu bir abonelik için kullanıcı erişimi yöneticisi rolü atamak için aşağıdaki adımları izleyin.

1. Azure portalında genel yönetici olarak oturum açın.

1. Seçin **tüm hizmetleri** ardından **abonelikleri**.

1. Aboneliğinizi seçin.

1. **Erişim denetimi (IAM)** öğesini seçin.

1. Seçin **rol atamaları** abonelik kapsamında rol atamaları geçerli listesini görmek için.

   ![Erişim denetimi (IAM) dikey penceresinde bir abonelik için](./media/pim-how-to-add-role-to-user/ms-pim-access-control.png)

1. Denetleme olmadığını **MS PIM** hizmet sorumlusu atandığı **kullanıcı erişimi Yöneticisi** rol.

1. Aksi takdirde, seçin **rol ataması Ekle** açmak için **rol ataması Ekle** bölmesi.

1. İçinde **rol** aşağı açılan listesinden **kullanıcı erişimi Yöneticisi** rol.

1. İçinde **seçin** listesinde, bulmak ve seçmek **MS PIM** hizmet sorumlusu.

   ![MS-PIM için izinler ekleme](./media/pim-how-to-add-role-to-user/ms-pim-add-permissions.png)

1. Seçin **Kaydet** rol atamak için.

   Birkaç dakika sonra MS-PIM hizmet sorumlusu abonelik kapsamında kullanıcı erişimi yöneticisi rolü atanır.

   ![MS-PIM için kullanıcı erişimi yöneticisi rolü](./media/pim-how-to-add-role-to-user/ms-pim-user-access-administrator.png)


## <a name="next-steps"></a>Sonraki adımlar

- [PIM'de Azure AD yönetici rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
- [PIM Azure kaynak Rolleri Ata](pim-resource-roles-assign-roles.md)
