---
title: Bir kullanıcının parola - Azure Active Directory sıfırlama | Microsoft Docs
description: Azure Active Directory'yi kullanarak bir kullanıcının parolasını sıfırlama konusunda yönergeler.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4db6554e86cef61f2fc8e7a466919d2ce723f0e5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60248018"
---
# <a name="reset-a-users-password-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak bir kullanıcının parolasını sıfırlama

Yönetici olarak, kullanıcı bir cihaz dışında kilitli, parolasını unutursa, ya da kullanıcı hiç parola almadıysanız, bir kullanıcının parolasını sıfırlayabilirsiniz.

>[!Note]
>Azure AD kiracınıza kullanıcı için giriş dizini olmadıkça mümkün olmayacaktır kullanarak parolalarını sıfırlayabilir. Bu, başka bir kuruluşa, bir Microsoft hesabı ya da bir Google hesabı bir hesap kullanarak kuruluşunuz için kullanıcı oturum açarsa, kullanıcının parolasını sıfırlamasını belirleyemeyeceğinizi anlamına gelir.<br><br>Kullanıcı bir Windows Server Active Directory olarak yetki kaynağı varsa, yalnızca üzerinde parola geri yazma özelliğini etkinleştirdiyseniz parolayı sıfırlamak mümkün olacaktır.<br><br>Bir kaynak olarak dış Azure AD yetkilisi kullanıcınız varsa parolayı sıfırlamak mümkün olmayacaktır. Yalnızca kullanıcı veya bir Yönetici Kılavuzu dış Azure AD'de, parolayı sıfırlayabilirsiniz.

>[!Note]
>Bir yönetici değilseniz ve bunun yerine kendi iş veya Okul parolanızı sıfırlamaya ilişkin yönergeler arıyorsanız bkz [iş veya Okul parolanızı sıfırlama](../user-help/active-directory-passwords-update-your-own-password.md).

## <a name="to-reset-a-password"></a>Parola sıfırlama

1. Oturum [Azure portalında](https://portal.azure.com/) parola Yöneticisi veya Kullanıcı Yöneticisi. Kullanılabilir roller hakkında daha fazla bilgi için bkz: [Azure Active Directory'de yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md#available-roles)

2. Seçin **Azure Active Directory**seçin **kullanıcılar**arayın ve sıfırlama ihtiyacı olan kullanıcıyı seçin ve ardından **parola sıfırlama**.

    **Alain Charon - profili** sayfası görünür ile **parolayı Sıfırla** seçeneği.

    ![Parola sıfırlama seçeneği vurgulanmış olan kullanıcının profil sayfası](media/active-directory-users-reset-password-azure-portal/user-profile-reset-password-link.png)

3. İçinde **parolayı Sıfırla** sayfasında **parolayı Sıfırla**.

    Kullanıcı için otomatik olarak oluşturulan geçici bir parola.

4. Parolayı kopyalayın ve kullanıcıya verin. Kullanıcı, sonraki oturum açma işlemi sırasında parolasını değiştirmeye gerekir.

    >[!Note]
    >Geçici parola her zaman geçerli olsun. Kullanıcı, sonraki oturum açışında parolasını çalışmaya devam edecek, bakılmaksızın geçici parolayı oluşturulmasının üzerinden ne kadar zaman geçti.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcı parola sıfırlama sonra aşağıdaki temel işlemleri gerçekleştirebilirsiniz:

- [Ekleme veya kullanıcıları Sil](add-users-azure-active-directory.md)

- [Kullanıcılara rol atama](active-directory-users-assign-role-azure-portal.md)

- [Profil bilgileri ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

Veya temsilci atayarak, ilkeleri kullanarak ve kullanıcı hesapları paylaşma gibi daha karmaşık kullanıcı senaryoları gerçekleştirebilirsiniz. Diğer kullanılabilir eylemler hakkında daha fazla bilgi için bkz: [Azure Active Directory kullanıcı yönetimi belgeleri](../users-groups-roles/index.yml).
