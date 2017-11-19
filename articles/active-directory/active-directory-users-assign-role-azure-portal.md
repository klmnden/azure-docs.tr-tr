---
title: "Bir kullanıcı Azure Active Directory'de yönetici rolleri atamak | Microsoft Docs"
description: "Azure Active Directory'de kullanıcı yönetim bilgilerini değiştirmek açıklanmaktadır"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a1ca1a53-50d8-4bf0-ae8f-73fa1253e2d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.openlocfilehash: 290f2e837cae9bdb95cf8acb486938632927ca8c
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory"></a>Kullanıcı Azure Active Directory'de yönetici rolleri atama
Bu makalede, Azure Active Directory'de (Azure AD) kullanıcıya bir yönetici rolü atama açıklanmaktadır. Kuruluşunuzdaki yeni kullanıcı ekleme hakkında daha fazla bilgi için bkz: [Azure Active Directory'ye yeni kullanıcı ekleme](active-directory-users-create-azure-portal.md). Eklenen kullanıcılar varsayılan olarak yönetici izinlerine sahip olmaz ancak bu kullanıcılara herhangi bir zamanda roller atayabilirsiniz.

## <a name="assign-a-role-to-a-user"></a>Bir kullanıcıya rol atama
1. Dizin için genel yönetici olan bir hesapla [Azure AD yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
2. Seçin **kullanıcılar ve gruplar**.

   ![Açılış kullanıcı yönetimi](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)
3. Seçin **tüm kullanıcılar**.

   ![Tüm kullanıcılar grubu açma](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)
1. Listeden bir kullanıcı seçin.
2. Seçilen kullanıcı için seçin **dizin rolünü** ve kullanıcının bir rol atayın **dizin rolünü** listesi. Kullanıcı ve yönetici rolleri hakkında daha fazla bilgi için bkz. [Azure AD'de yönetici rolü atama](active-directory-assign-admin-roles-azure-portal.md).

      ![Bir kullanıcı rol atama](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)
6. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar
* [Kullanıcı ekleme](active-directory-users-create-azure-portal.md)
* [Yeni Azure portalında bir kullanıcının parolasını sıfırlama](active-directory-users-reset-password-azure-portal.md)
* [Kullanıcının iş bilgilerini değiştirme](active-directory-users-work-info-azure-portal.md)
* [Kullanıcı profillerini yönetme](active-directory-users-profile-azure-portal.md)
* [Azure AD'de kullanıcı silme](active-directory-users-delete-user-azure-portal.md)
