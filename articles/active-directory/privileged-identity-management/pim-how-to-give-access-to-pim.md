---
title: PIM - Azure Active Directory yönetmek için diğer yöneticiler için erişim verme | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) yönetmek için diğer yönetimler erişimi vermeyi öğreneceksiniz.
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
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb22e3cc93baebac023c0148812c6a4c6c95be60
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59489219"
---
# <a name="grant-access-to-other-administrators-to-manage-pim"></a>PIM yönetmek için diğer yöneticilere erişim izni ver

Bir kuruluş için Azure Active Directory (Azure AD) Privileged Identity Management (PIM) otomatik olarak sağlayan genel yönetici rolü atamalarını ve PIM erişimi alın. Başka hiç kimse yazma erişimi varsayılan olarak, ancak diğer genel yöneticileri dahil alır. Diğer genel Yöneticiler, güvenlik yöneticileri ve güvenlik okuyucuları PIM salt okunur erişimi vardır. PIM için erişim vermek için ilk kullanıcı başkalarına atayabilirsiniz **ayrıcalıklı Rol Yöneticisi** rol.

> [!NOTE]
> PIM yönetme, Azure mfa'yı gerektirir. Microsoft hesapları için Azure MFA kaydını yapamıyorum olduğundan, bir Microsoft hesabıyla oturum açtığında bir kullanıcının PIM erişemez.

Her zaman en az iki kullanıcı ayrıcalıklı Rol Yöneticisi rolünde, bir kullanıcı kilitli durumda veya bunların hesap silindiğinde emin olun.

## <a name="grant-access-to-manage-pim"></a>PIM yönetme erişimi verme

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure AD rolleri**.

1. Tıklayın **rolleri**.

    ![PIM Azure AD rolleri - roller](./media/pim-how-to-give-access-to-pim/pim-directory-roles-roles.png)

1. Tıklayın **ayrıcalıklı Rol Yöneticisi** rolü üyeleri sayfasını açın.

    ![Ayrıcalıklı Rol Yöneticisi - üyeleri](./media/pim-how-to-give-access-to-pim/pim-pra-members.png)

1. Tıklayın **Üye Ekle** Ekle yönetilen üyeler bölmesi açmak için.

1. Tıklayın **üyelerini seçin** seçim üyeleri bölmesini açmak için.

    ![Ayrıcalıklı Rol Yöneticisi - üyeleri seçin](./media/pim-how-to-give-access-to-pim/pim-pra-select-members.png)

1. Bir üyeyi seçin ve ardından **seçin**.

1. Tıklayın **Tamam** üye için uygun hale getirmek için **ayrıcalıklı Rol Yöneticisi** rol.

    Yeni bir rol, PIM birine atadığınızda, bunlar otomatik olarak yapılandırılır **uygun** rolü etkinleştirmek için.

1. Üye kalıcı hale getirmek için ayrıcalıklı Rol Yöneticisi üye listesinde tıklayın.

1. Tıklayın **daha fazla** ardından **kalıcı hale getirmek** atama kalıcı hale getirmek için.

    ![Ayrıcalıklı Rol Yöneticisi - kalıcı yapma](./media/pim-how-to-give-access-to-pim/pim-pra-make-permanent.png)

1. Kullanıcı bağlantı gönderme [PIM kullanmaya başlamak](pim-getting-started.md).

## <a name="remove-access-to-manage-pim"></a>PIM yönetme erişimi Kaldır

Biri ayrıcalıklı rol yöneticisi rolünden kaldırmadan önce her zaman ayrıca en az iki kullanıcı atanmış hala olacaktır emin olun.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure AD rolleri**.

1. Tıklayın **rolleri**.

1. Tıklayın **ayrıcalıklı Rol Yöneticisi** rolü üyeleri sayfasını açın.

1. Eklemek istediğiniz kaldırın ve ardından kullanıcının yanındaki onay kutusunu **üye kaldırma**.

    ![Ayrıcalıklı Rol Yöneticisi - üye kaldırma](./media/pim-how-to-give-access-to-pim/pim-pra-remove-member.png)

1. Rolden üyeyi kaldırma isterseniz görüntülenen soran iletisinde tıklayın **Evet**.

## <a name="next-steps"></a>Sonraki adımlar

- [PIM kullanmaya başlama](pim-getting-started.md)
