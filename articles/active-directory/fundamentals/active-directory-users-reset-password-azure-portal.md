---
title: Parola sıfırlama Azure Active Directory'de | Microsoft Docs
description: Yönetici tarafından başlatılan parola sıfırlama, Azure Active Directory'de bir kullanıcı için
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
editor: ''
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: joflore
ms.reviewer: sahenry
ms.custom: it-pro
ms.openlocfilehash: 689ab264e56bc8559db6237eee19566e9e3c429f
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35648439"
---
# <a name="reset-the-password-for-a-user-in-azure-active-directory"></a>Azure Active Directory'de bir kullanıcının parolasını sıfırlama

Yöneticiler, nerede bunlar unuttum, durumlarda bir kullanıcının parolasını sıfırlamak gerekebilir kilitlenmiş out veya diğer senaryolar. Aşağıdaki adımları bir kullanıcının parolasını sıfırlama yoluyla Kılavuzu.

## <a name="how-to-reset-the-password-for-a-user"></a>Bir kullanıcının parolasını sıfırlama

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com) dizin kullanıcı parolalarını sıfırlama izni olan bir hesap ile.
2. Seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
3. Kullanıcının parolasını sıfırlamak istiyorsanız seçin.
2. Seçilen kullanıcı için seçin **parolayı Sıfırla**.

    ![Azure AD'de kullanıcı profili bir kullanıcının parola sıfırlama](./media/active-directory-users-reset-password-azure-portal/user-password-reset.png)
    
6. Üzerinde **parolayı Sıfırla**seçin **parolayı Sıfırla**.
7. Geçici parolayı kullanıcıya sonra sunabilir görüntülenir. Kullanıcı daha sonra kullanıcılar oturum açtığında parola değiştirmesi istenir. 

   > [!NOTE]
   > Bu geçici bir parola değil sahip sona erme süresini oturum kadar geçerli olacaktır ve olan ardından olan zorunlu değiştirmek için. 

## <a name="next-steps"></a>Sonraki adımlar
* [Kullanıcı ekleme](../add-users-azure-active-directory.md)
* [Bir kullanıcıya yönetici rolü atama](active-directory-users-assign-role-azure-portal.md)
* [Kullanıcı profillerini yönetme](active-directory-users-profile-azure-portal.md)
* [Azure AD'de kullanıcı silme](../add-users-azure-active-directory.md)
