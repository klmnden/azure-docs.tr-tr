---
title: "Parola sıfırlama Azure Active Directory'de | Microsoft Docs"
description: "Parola sıfırlama Azure Active Directory'de bir kullanıcı için yönetici tarafından başlatılan"
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
editor: 
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: joflore
ms.reviewer: sahenry
ms.custom: it-pro
ms.openlocfilehash: 6d01dff567e49b602e98f717dace4dc75abecb4c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="reset-the-password-for-a-user-in-azure-active-directory"></a>Azure Active Directory'de bir kullanıcı parolasını sıfırlama

Yöneticiler burada bunlar unuttunuz, durumlarda bir kullanıcının parolasını sıfırlamak gerekebilir kilitlenmiştir teslim alma veya diğer senaryolar. Adımları bir kullanıcının parolasını sıfırlama işleminde yol.

## <a name="how-to-reset-the-password-for-a-user"></a>Bir kullanıcının parolasını sıfırlama

1. Oturum [Azure AD Yönetim Merkezi](https://aad.portal.azure.com) kullanıcı parolalarını sıfırlama dizin izinlerine sahip bir hesapla.
2. Seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
3. Parolasını sıfırlamak istediğiniz kullanıcıyı seçin.
2. Seçilen kullanıcı için seçin **parola sıfırlama**.

    ![Azure AD'de bir kullanıcıdan bir kullanıcı profili için bir parola sıfırlama](./media/active-directory-users-reset-password-azure-portal/user-password-reset.png)
    
6. Üzerinde **parola sıfırlama**seçin **parola sıfırlama**.
7. Geçici bir parola kullanıcıya sonra sağlayabilir görüntülenir. Kullanıcı daha sonra kullanıcılar oturum açtığında, parolalarını değiştirip istenir. 

   > [!NOTE]
   > Bu geçici parolayı değil sahip bir sona erme zamanı oturum kadar geçerli olmayacaktır ve olan sonra olan zorla değiştirmek için. 

## <a name="next-steps"></a>Sonraki adımlar
* [Kullanıcı ekleme](active-directory-users-create-azure-portal.md)
* [Bir kullanıcıya yönetici rolleri atama](active-directory-users-assign-role-azure-portal.md)
* [Kullanıcı profillerini yönetme](active-directory-users-profile-azure-portal.md)
* [Azure AD'de kullanıcı silme](active-directory-users-delete-user-azure-portal.md)
