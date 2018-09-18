---
title: Kullanıcılar Azure Active Directory'ye Dizin rolleri atama | Microsoft Docs
description: Kullanıcılar Azure Active Directory'ye Dizin rolleri atama hakkında bilgi edinin.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.openlocfilehash: b73df5ec0381e83c54c8cd9f8c0335448def0c6d
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733051"
---
# <a name="how-to-assign-roles-and-administrators-to-users-with-azure-active-directory"></a>Nasıl yapılır: roller ve yöneticiler kullanıcılara atama Azure Active Directory ile
Kuruluşunuzdaki bir kullanıcı Azure Active Directory (Azure AD) kaynakları yönetme izni gerekiyorsa, kullanıcının uygun bir rol Azure AD'de kullanıcının gerçekleştirme izni gereken eylemler temelinde atamanız gerekir.

Kullanılabilir roller hakkında daha fazla bilgi için bkz: [Azure Active Directory'de yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md). Kullanıcı ekleme hakkında daha fazla bilgi için bkz. [Azure Active Directory'ye yeni kullanıcı ekleme](add-users-azure-active-directory.md).

## <a name="assign-roles"></a>Rol atama
Bir kullanıcıya Azure AD rolleri atamak için yaygın bir yolu açıktır **dizin rolü** sayfasında bir kullanıcı için.

Ayrıca, Privileged Identity Management (PIM) kullanarak rolleri atayabilirsiniz. PIM kullanma hakkında daha ayrıntılı bilgi için bkz: [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management).

### <a name="to-assign-a-role-to-a-user"></a>Bir kullanıcıya rol atamak için
1. Oturum [Azure portalında](https://portal.azure.com/) dizinde genel yönetici hesabını kullanarak.

2. Seçin **Azure Active Directory**seçin **kullanıcılar**, arayın ve rol ataması almak istediğiniz kullanıcıyı seçin. Örneğin, _Alain Charon_.

3. Üzerinde **Alain Charon - profili** sayfasında **dizin rolü**.

    **Alain Charon - dizin rolü** sayfası görüntülenir.

4. Seçin **Rol Ekle**, için Alain atamak için rolü seçin (örneğin, _Uygulama Yöneticisi_) ve ardından **seçin**.

    ![Dizin rolleri sayfasında, seçili rolü gösteriliyor](media/active-directory-users-assign-role-azure-portal/directory-role-select-role.png)

    Alain Charon için uygulama Yönetici rolüne atanır ve göründüğü **Alain Charon - dizin rolü** sayfası.

## <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma
Bir kullanıcıdan rol atamasını kaldırmanız gerekirse, ayrıca, bunu yapabilirsiniz **Alain Charon - dizin rolü** sayfası.

### <a name="to-remove-a-role-assignment-from-a-user"></a>Bir kullanıcıdan bir rol atamasını kaldırmak için

1. Seçin **Azure Active Directory**seçin **kullanıcılar**, arayın ve kaldırılan bir rol ataması alma kullanıcıyı seçin. Örneğin, _Alain Charon_.

2. Seçin **dizin rolü**seçin **Uygulama Yöneticisi**ve ardından **rolü Kaldır**.

    ![Seçili rolü ve Kaldır seçeneğini gösteren Dizin rolleri sayfası](media/active-directory-users-assign-role-azure-portal/directory-role-remove-role.png)

    Uygulama Yöneticisi rolü Alain Charon ' kaldırılır ve artık göründüğü **Alain Charon - dizin rolü** sayfası.

## <a name="next-steps"></a>Sonraki adımlar
- [Ekleme veya kullanıcıları Sil](add-users-azure-active-directory.md)

- [Profil bilgileri ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)

- [Başka bir dizinden konuk kullanıcılar ekleme](../b2b/what-is-b2b.md)

Veya temsilci atayarak, ilkeleri kullanarak ve kullanıcı hesapları paylaşma gibi diğer kullanıcı yönetim görevlerini gerçekleştirebilirsiniz. Diğer kullanılabilir eylemler hakkında daha fazla bilgi için bkz: [Azure Active Directory kullanıcı yönetimi belgeleri](../users-groups-roles/index.yml).


