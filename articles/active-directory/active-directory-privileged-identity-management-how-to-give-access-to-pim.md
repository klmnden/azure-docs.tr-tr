---
title: "Privileged Identity Management - Azure için erişim vermek nasıl | Microsoft Docs"
description: "Azure Active Directory Privileged Identity Management uzantısı ile kullanıcıları için roller PIM yönetilebilir eklemeyi öğrenin."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: d4c53b53-2b37-41e6-813c-96ec08a1c897
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 2fccf7cae61f4d8862640b89d39f9d4480362429
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="giving-access-to-manage-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management yönetmek için erişim verme
Azure AD Privileged Identity Management (PIM) bir kuruluş için otomatik olarak etkinleştirir genel yönetici rol atamalarını ve PIM erişimi alın. Hiç kimsenin yazma erişimi varsayılan olarak, ancak başka genel yöneticiler de dahil olmak üzere alır. Diğer genel yöneticileri, güvenlik yöneticileri ve güvenlik okuyucuları Azure AD PIM salt okunur erişimi vardır. PIM için erişim vermek için ilk kullanıcı başkalarına atayabilirsiniz **ayrıcalıklı Rol Yöneticisi** rol. Bu atama gelen PIM içinde yapılmalıdır ve PowerShell veya diğer portallar değiştirilemez.

> [!NOTE]
> Azure AD PIM yönetme Azure MFA gerektirir. Microsoft hesapları için Azure MFA kaydedilemiyor olduğundan, bir Microsoft hesabıyla oturum açan bir kullanıcı Azure AD PIM erişemiyor.
> 
> 

Her zaman en az iki kullanıcılar ayrıcalıklı Rol Yöneticisi rolünde bir kullanıcı kilitli durumda veya kullanıcıların hesap silindi emin olun.

## <a name="give-another-user-access-to-manage-pim"></a>PIM yönetmek için başka bir kullanıcı erişimi verin
1. Oturum [Azure portal](https://portal.azure.com/) seçip **Azure AD Privileged Identity Management** Panoda uygulama.
2. Seçin **yönetmek ayrıcalıklı rolleri** > **ayrıcalıklı Rol Yöneticisi** > **Ekle**.
   
    ![Ayrıcalıklı rol yöneticileri - ekran görüntüsü ekleme][1]
3. Yönetilen Kullanıcı Ekle dikey, 1. adım zaten tamamlanmış olur. 2. adımı seçin **kullanıcı seçin** eklemek istediğiniz kullanıcıyı arayın.
   
    ![Kullanıcılar - ekran görüntüsü seçin][2]
4. Arama sonuçlarından kullanıcıyı seçin ve'ı tıklatın **Bitti**.
5. Tıklatın **Tamam** seçiminizi kaydetmek için. Seçtiğiniz kullanıcı ayrıcalıklı rol Yöneticiler listesinde görünür.
   
   * Yeni bir rol birine atamak olduğunda, bunlar otomatik olarak uygun rolünü etkinleştirmek için ayarlanır. Roldeki kalıcı hale getirmek istiyorsanız, kullanıcı listesinde'ı tıklatın. Seçin **izin olun** kullanıcı bilgileri menüsünde.
6. Kullanıcı bağlantı gönderme [Azure AD Privileged Identity Management ile çalışmaya başlama](active-directory-privileged-identity-management-getting-started.md).

## <a name="remove-another-users-access-rights-for-managing-pim"></a>PIM yönetmek için başka bir kullanıcının erişim haklarını kaldır
Biri ayrıcalıklı rol yöneticisi rolünden kaldırmadan önce her zaman atanmış iki kullanıcı çıkarılsın emin olun.

1. PIM panosunda rolüne tıklayın **ayrıcalıklı Rol Yöneticisi**.  Şu anda bu roldeki kullanıcılar listesi görüntülenir.
2. Kullanıcı listesinde kullanıcı tıklayın.
3. Tıklayın **kaldırmak**.  Bir onay iletisi ile sunulur.
4. Tıklatın **Evet** kullanıcı rolünden kaldırmak için.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
