---
title: Kaydolmak için Office 365 ile Azure hesabı | Microsoft Docs
description: Bir Office 365 aboneliği bir Azure hesabı kullanarak oluşturmayı öğrenin
services: ''
documentationcenter: ''
author: JiangChen79
manager: adpick
editor: ''
tags: billing,top-support-issue
ms.assetid: ''
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: banders
ms.openlocfilehash: b67f3c590be290515329af390b4d3d79a9746112
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60369905"
---
# <a name="sign-up-for-an-office-365-subscription-with-your-azure-account"></a>Azure hesabınızı kullanarak Office 365 aboneliği için kaydolun
Azure abonesi iseniz, bir Office 365 aboneliğine kaydolmak için Azure hesabınızı kullanabilirsiniz. Bir Azure aboneliğine sahip bir kuruluş bir parçası kullanıyorsanız, var olan Azure Active Directory'de (Azure AD) kullanıcıları için Office 365 abonelikleri oluşturabilirsiniz. Azure Active Directory kiracınızda genel yönetici veya faturalama yöneticisi izinlerine sahip bir hesap kullanarak Office 365 için kaydolun. Daha fazla bilgi için [hesabı İzinlerim Azure AD'de denetleyin](#RoleInAzureAD) ve [Azure Active Directory'de yönetici rolleri atama](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

Office 365 hesabı hem de Azure aboneliğiniz zaten varsa, [bir Azure aboneliği için bir Office 365 kiracısı ilişkilendirme](billing-add-office-365-tenant-to-azure-subscription.md).

## <a name="get-an-office-365-subscription-by-using-your-azure-account"></a>Azure hesabınızı kullanarak Office 365 aboneliği edinin

1. Git [Office 365 ürün sayfası](https://products.office.com/business)ve bir plan seçin.
2. Tıklayın **oturum** sayfanın sağ üst köşesindeki şirket.

    ![Office 365 deneme sayfasının ekran görüntüsü](./media/billing-use-existing-azure-account-office-365-subscription/12-office-365-trial-page.png)
3. Azure hesabı kimlik bilgilerinizle oturum açın. Kuruluşunuz için bir abonelik oluşturuyorsanız, Azure Active Directory kiracınızın genel yönetici veya faturalama Yöneticisi dizin rolü üyesi olan bir Azure hesabı kullanın.

    ![Ekran görüntüsü, Office 365 oturum açma](./media/billing-use-existing-azure-account-office-365-subscription/13-office-365-sign-in.png)
4. Tıklayın **şimdi deneyin**.

    ![Numaralı Siparişiniz için Office 365 onaylar ekran görüntüsü.](./media/billing-use-existing-azure-account-office-365-subscription/14-office-365-confirm-your-order.png)
5. Sipariş giriş sayfasında tıklayın **devam**.

    ![Office 365 sipariş alındı belgesinde ekran görüntüsü](./media/billing-use-existing-azure-account-office-365-subscription/15-office-365-order-receipt.png)

Artık hazırsınız.
Kuruluşunuz için Office 365 aboneliğiniz varsa Azure AD kullanıcılarınızın artık Office 365'te olduğunu denetlemek için aşağıdaki adımları kullanın.

1. Microsoft 365 Yönetici merkezini açın.
2. Genişletin **kullanıcılar**ve ardından **etkin kullanıcılar**.

    ![Microsoft 365 Yönetici Merkezi kullanıcıların ekran görüntüsü](./media/billing-use-existing-azure-account-office-365-subscription/16-microsoft-365-admin-center-users.png)

Kaydolduktan sonra Office 365 aboneliğine Azure aboneliğinize ait aynı Azure Active Directory örneğine eklenir. Daha fazla bilgi için [Azure ve Office 365 abonelikleri hakkında daha fazla](billing-use-existing-office-365-account-azure-subscription.md#more-about-subs) ve [Azure aboneliklerinin Azure Active Directory ile ilişkisi](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a id="RoleInAzureAD"></a>Azure AD'de hesabı İzinlerim denetleyin
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Tıklayın **tüm hizmetleri**ve ardından arama **Active Directory**.

    ![Ekran görüntüsü, Active Directory'de Azure portalı](./media/billing-use-existing-azure-account-office-365-subscription/billing-more-services-active-directory.png)
3. Tıklayın **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
4. Kullanıcı adı seçin.

    ![Azure Active Directory Kullanıcıları gösteren ekran görüntüsü](./media/billing-use-existing-azure-account-office-365-subscription/billing-users-groups.png)

5. Tıklayın **dizin rolü**.

    ![Azure portal dizin rolü gösteren ekran görüntüsü](./media/billing-use-existing-azure-account-office-365-subscription/billing-user-directory-role.png)
6.  Rol **genel yönetici** veya **sınırlı yönetici** > **Faturalama Yöneticisi** kullanıcılar için bir Office 365 aboneliği oluşturmak için gereklidir var olan Azure Active Directory.

    ![Azure portal dizin rolü Faturalama Yöneticisi gösteren ekran görüntüsü](./media/billing-use-existing-azure-account-office-365-subscription/billing-directoryrole-limited.png)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).
