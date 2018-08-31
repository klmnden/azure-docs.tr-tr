---
title: Azure PIM - yönetmek için diğer yöneticiler için erişim verme | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) yönetmek için diğer yönetimler erişimi vermeyi öğreneceksiniz.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: d6b2d9f43ce9bb86f4557c92887689c83beb49fa
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189564"
---
# <a name="grant-access-to-other-administrators-to-manage-pim"></a>PIM yönetmek için diğer yöneticilere erişim izni ver
Azure AD Privileged Identity Management (PIM) bir kuruluş için otomatik olarak sağlayan bir genel yönetici rolü atamalarını ve PIM erişimi alın. Başka hiç kimse yazma erişimi varsayılan olarak, ancak diğer genel yöneticileri dahil alır. Diğer genel Yöneticiler, güvenlik yöneticileri ve güvenlik okuyucuları Azure AD PIM salt okunur erişimi vardır. PIM erişimi verme, ilk kullanıcı başkalarına atayabilirsiniz **ayrıcalıklı Rol Yöneticisi** rol.

> [!NOTE]
> Azure MFA, Azure AD PIM yönetme gerektirir. Microsoft hesapları için Azure MFA kaydını yapamıyorum olduğundan, bir Microsoft hesabıyla oturum açtığında bir kullanıcıyı Azure AD PIM erişemez.
> 
> 

Her zaman en az iki kullanıcı bir ayrıcalıklı Rol Yöneticisi rolünde bir kullanıcı kilitli durumda veya bunların hesap silindiğinde emin olun.

## <a name="give-another-user-access-to-manage-pim"></a>PIM yönetmek için başka bir kullanıcı erişimi verme
1. Oturum [Azure portalında](https://portal.azure.com/) seçip **Azure AD Privileged Identity Management** Panoda uygulama.
2. Seçin **ayrıcalıklı rollerimi Yönet** > **ayrıcalıklı Rol Yöneticisi** > **Ekle**.
   
    ![Ayrıcalıklı rol yöneticileri - ekran görüntüsü Ekle](./media/pim-how-to-give-access-to-pim/PIM_add_PRA.png)
3. Yönetilen Kullanıcı Ekle dikey, 1. adım zaten tamamlandı. 2. adımı seçin **Seçili kullanıcılar** eklemek istediğiniz kullanıcıyı arayın.
   
    ![Kullanıcılar - ekran görüntüsü](./media/pim-how-to-give-access-to-pim/PIM_select_users.png)
4. Arama sonuçlarından kullanıcı seçin ve tıklayın **Bitti**.
5. Tıklayın **Tamam** seçiminizi kaydetmek için. Seçtiğiniz kullanıcı ayrıcalıklı rol yöneticileri listesinde görünür.
   
   * Birine yeni rol atamanız olduğunda, bunlar otomatik uygun rolü etkinleştirmek için ayarlanır. Bunları rolde kalıcı hale getirmek isterseniz, kullanıcı listesinde'e tıklayın. Seçin **kalıcı yap** kullanıcı bilgileri menüsünde.
6. Kullanıcı bağlantı gönderme [Azure AD Privileged Identity Management ile çalışmaya başlama](pim-getting-started.md).

## <a name="remove-another-users-access-rights-for-managing-pim"></a>PIM yönetmek için başka bir kullanıcının erişim haklarını kaldır
Biri ayrıcalıklı rol yönetici rolünden kaldırmadan önce her zaman Ayrıca iki kullanıcı atanmış hala olacaktır emin olun.

1. PIM Panoda role tıklayın **ayrıcalıklı Rol Yöneticisi**.  Bu roldeki kullanıcılar listesi görüntülenir.
2. Kullanıcı listesinde kullanıcı tıklayın.
3. Tıklayarak **Kaldır**.  Size bir onay iletisi gösterilir.
4. Tıklayın **Evet** kullanıcı rolünden kaldırmak için.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar

- [Kiracınızdaki abonelik yönetimini etkinleştirme](pim-resource-roles-enable-subscription-management.md)