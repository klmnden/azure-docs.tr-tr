---
title: Azure Active Directory'de kullanıcılara yönetici rolü atama | Microsoft Docs
description: Azure Active Directory'de kullanıcı yönetim bilgilerini değiştirme
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 06/25/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.openlocfilehash: ec30f1507bfa45c29709a7f4b7dc1e91aa25ca57
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440757"
---
# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory"></a>Azure Active Directory’de kullanıcılara yönetici rolü atama
Bu makalede Azure Active Directory'de (Azure AD) kullanıcılara yönetici rolü atamayı adımları açıklanmaktadır. Kuruluşunuza yeni kullanıcı ekleme hakkında daha fazla bilgi için bkz. [Azure Active Directory'ye yeni kullanıcı ekleme](../add-users-azure-active-directory.md). Eklenen kullanıcılar varsayılan olarak yönetici izinlerine sahip olmaz ancak bu kullanıcılara herhangi bir zamanda roller atayabilirsiniz.

## <a name="assign-a-role-to-a-user"></a>Kullanıcıya rol atama
1. Dizin için genel yönetici veya ayrıcalıklı rol yöneticisi olan bir hesapla [Azure portalında](https://portal.azure.com) oturum açın.

2. **Azure Active Directory**'yi, **Kullanıcılar**'ı ve ardından listeden belirli bir kullanıcıyı seçin.

    ![Kullanıcı yönetimini açma](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)

3. Seçilen kullanıcı için **Dizin rolü**'nü, **Rol ekle**'yi ve ardından **Dizin rolleri** listesinden **Koşullu erişim yöneticisi** gibi uygun yönetici rollerini seçin. Yönetici rolleri hakkında daha fazla bilgi için bkz. [Azure AD'de yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md). 

    ![Bir kullanıcıyı role atama](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)

1. Kaydetmek için **Seç**'e basın.

## <a name="next-steps"></a>Sonraki adımlar
* [Hızlı Başlangıç: Azure Active Directory'de kullanıcı ekleme veya silme](add-users-azure-active-directory.md)
* [Kullanıcı profillerini yönetme](active-directory-users-profile-azure-portal.md)
* [Başka bir dizinden konuk kullanıcılar ekleme](../b2b/what-is-b2b.md) 
* [Azure AD'de bir role kullanıcı atama](active-directory-users-assign-role-azure-portal.md)
* [Silinen bir kullanıcıyı geri yükleme](active-directory-users-restore.md)
