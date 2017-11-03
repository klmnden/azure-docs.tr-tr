---
title: "Kaydolmak için Office 365 ile Azure hesabı | Microsoft Docs"
description: "Bir Azure hesabı kullanarak bir Office 365 aboneliği oluşturma hakkında bilgi edinin"
services: 
documentationcenter: 
author: JiangChen79
manager: adpick
editor: 
tags: billing,top-support-issue
ms.assetid: 
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: troubleshooting
ms.date: 08/03/2017
ms.author: cjiang
ms.openlocfilehash: 6965765757d9a20dfb542bbb3cfae1c30276c238
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="sign-up-for-an-office-365-subscription-with-your-azure-account"></a>Bir Office 365 aboneliğine Azure hesabınızla kaydolun
Azure abone değilseniz, bir Office 365 aboneliği için kaydolmak için Azure hesabınızı kullanabilirsiniz. Bir Azure aboneliğine sahip kuruluş parçası sizseniz, mevcut Azure Active Directory'de (Azure AD) kullanıcıları için Office 365 abonelikleri oluşturabilirsiniz. Azure Active Directory kiracınızda genel yönetici veya faturalama yönetici izinlerine sahip bir hesabı kullanarak Office 365 için kaydolun. Daha fazla bilgi için bkz: [Azure AD'de my hesap izinlerini denetlemek](#RoleInAzureAD) ve [Azure Active Directory'de yönetici rolleri atama](../active-directory/active-directory-assign-admin-roles.md).

Office 365 hesabı ve bir Azure aboneliğiniz zaten varsa, [bir Azure aboneliği için bir Office 365 Kiracı ilişkilendirmek](billing-add-office-365-tenant-to-azure-subscription.md).

## <a name="get-an-office-365-subscription-by-using-your-azure-account"></a>Azure hesabınızı kullanarak bir Office 365 aboneliği edinme

1. Git [Office 365 ürün sayfası](https://products.office.com/business)ve bir planı seçin.
2. Tıklatın **oturum** sayfanın sağ üst köşesinde üzerinde.

    ![Office 365 deneme sayfasının ekran görüntüsü](./media/billing-use-existing-azure-account-office-365-subscription/12-office-365-trial-page.png)
3. Azure hesabı kimlik bilgilerinizle oturum. Kuruluşunuz için bir abonelik oluşturuyorsanız, Azure Active Directory kiracınızda genel yönetici veya faturalama Yöneticisi dizin rolünün bir üyesi olan bir Azure hesabı kullanın.

    ![Office 365 ekran oturum açma](./media/billing-use-existing-azure-account-office-365-subscription/13-office-365-sign-in.png)
4. Tıklatın **şimdi deneyin**.

    ![Office 365 için sipariş onaylar ekran görüntüsü.](./media/billing-use-existing-azure-account-office-365-subscription/14-office-365-confirm-your-order.png)
5. Sipariş giriş sayfasında, tıklatın **devam**.

    ![Office 365 sipariş girişi ekran görüntüsü](./media/billing-use-existing-azure-account-office-365-subscription/15-office-365-order-receipt.png)

Artık başlamaya hazırsınız. Kuruluşunuz için Office 365 aboneliği oluşturduysanız, Azure AD kullanıcılarınız artık Office 365'te olduğunu denetlemek için aşağıdaki adımları kullanın.

1. Office 365 Yönetici merkezini açın.
2. Genişletme **kullanıcılar**ve ardından **etkin kullanıcılar**.

    ![Office 365 Yönetici merkezi kullanıcılar ekran görüntüsü](./media/billing-use-existing-azure-account-office-365-subscription/16-office-365-admin-center-users.png)

Kaydolduktan sonra Office 365 aboneliğine Azure aboneliğinize ait aynı Azure Active Directory örneğini eklenir. Daha fazla bilgi için bkz: [Azure ve Office 365 abonelikleri hakkında daha fazla](billing-use-existing-office-365-account-azure-subscription.md#more-about-subs) ve [Azure aboneliklerinin Azure Active Directory ile ilişkili](../active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a id="RoleInAzureAD"></a>Azure AD'de hesabı izinlerimi denetleyin
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Tıklatın **daha fazla hizmet**, arayın ve sonra **Active Directory**.

    ![Ekran görüntüsü, Active Directory'de Azure portalı](./media/billing-use-existing-azure-account-office-365-subscription/billing-more-services-active-directory.png)
3. Tıklatın **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
4. Kullanıcı adı seçin. 

    ![Azure Active Directory Kullanıcıları gösteren ekran görüntüsü](./media/billing-use-existing-azure-account-office-365-subscription/billing-users-groups.png)

5. Tıklatın **dizin rolünü**.
  
    ![Azure portal dizin rolünü gösteren ekran görüntüsü](./media/billing-use-existing-azure-account-office-365-subscription/billing-user-directory-role.png)
6.  Rol **genel yönetici** veya **sınırlı yönetici** > **Faturalama Yöneticisi** kullanıcılar için bir Office 365 aboneliği oluşturmak için gereklidir var olan Azure Active Directory'yi.

    ![Azure portal dizin rolünü Faturalama Yöneticisi gösteren ekran görüntüsü](./media/billing-use-existing-azure-account-office-365-subscription/billing-directoryrole-limited.png)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.
Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için. 
