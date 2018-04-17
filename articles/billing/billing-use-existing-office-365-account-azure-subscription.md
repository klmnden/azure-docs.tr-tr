---
title: Kaydolmak için Azure ile Office 365 hesabı | Microsoft Docs
description: Office 365 hesabı kullanarak bir Azure aboneliği oluşturma hakkında bilgi edinin
services: ''
documentationcenter: ''
author: JiangChen79
manager: adpick
editor: ''
tags: billing,top-support-issue
ms.assetid: 129cdf7a-2165-483d-83e4-8f11f0fa7f8b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: cjiang
ms.openlocfilehash: e9f90127bce0502147572c5ac6bd65e47dbe8c35
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="sign-up-for-an-azure-subscription-with-your-office-365-account"></a>Office 365 hesabınıza bir Azure aboneliği için kaydolun
Bir Office 365 aboneliğiniz varsa, bir Azure aboneliği oluşturmak için Office 365 hesabınızı kullanabilirsiniz. Oturum [Azure portal](https://portal.azure.com/) Office 365 kullanıcı adınızı ve parolanızı kullanarak. Sanal makineleri ayarlamanız veya diğer Azure hizmetleriyle kullanmak istiyorsanız, Azure aboneliği için kaydolmanız gerekir. Azure aboneliğiniz başkalarıyla paylaşabilir ve [Azure aboneliği ve kaynaklarınıza erişimi yönetmek için rol tabanlı erişim denetimi kullanın](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

Office 365 hesabı ve bir Azure aboneliğiniz zaten varsa, bkz: [bir Azure aboneliği için bir Office 365 Kiracı ilişkilendirmek](billing-add-office-365-tenant-to-azure-subscription.md).

## <a name="get-an-azure-subscription-using-your-office-365-account"></a>Office 365 hesabınızı kullanarak bir Azure aboneliği edinme

Zaman kazanmak ve Office 365 kullanıcı adınızı ve parolanızı kullanarak Azure için kaydolarak hesap artışı kaçının. 

1. Oturum açın [Azure.com](https://account.azure.com/signup?offer=MS-AZR-0044p&appId=docs). 
2. Office 365 kullanıcı adınızı ve parolanızı kullanarak oturum açın. Kullandığınız hesap, yönetici izinlerine sahip olmanız gerekmez. Birden fazla Office 365 hesabı varsa, Azure aboneliğinizle ilişkilendirmek istediğiniz Office 365 hesabı için kimlik bilgilerini kullandığınızdan emin olun. 

   ![Oturum açma sayfasını gösteren ekran görüntüsü.](./media/billing-use-existing-office-365-account-azure-subscription/billing-sign-in-with-office-365-account.png)

3. Gerekli bilgileri girin ve kayıt işlemini tamamlayın. Office 365 hesabınız zaten varsa bazı bilgiler gerekli olmayabilir.

    ![Kayıt formunu gösteren ekran görüntüsü.](./media/billing-use-existing-office-365-account-azure-subscription/billing-azure-sign-up-fill-information.png)

- Azure aboneliği için kuruluşunuzdaki diğer kişiler eklemeniz gerekiyorsa, bkz: [Azure portalında erişim yönetimini kullanmaya başlama](../role-based-access-control/overview.md). 

## <a id="more-about-subs">Azure ve Office 365 abonelikleri hakkında daha fazla bilgi</a>
Office 365 ve Azure kullanıcılar ve Aboneliklerini yönetmek için Azure AD hizmeti kullanın. Azure directory kullanıcıları ve abonelikleri gruplandırabilirsiniz bir gibi kapsayıcıdır. Azure ve Office 365 abonelikleri için aynı kullanıcı hesaplarını kullanmak için Azure abonelikleri Office 365 abonelikleri ile aynı dizinde oluşturulduğundan emin olmanız gerekir. Aşağıdaki noktaları göz önünde bulundurun:

* Bir abonelik altında bir dizin oluşturulmuş
* Kullanıcıların dizinlerine ait
* Bir abonelik abonelik oluşturan kullanıcının dizinde adlandırıldığını. Bu nedenle Office 365 aboneliğinize Azure aboneliğinize aynı hesapta bağlıdır.
* Azure abonelikleri dizinde tek tek kullanıcılar tarafından sahip olunan
* Office 365 abonelikleri directory tarafından sahibi olur. Dizin içindeki doğru izinlere sahip kullanıcılar bu abonelikleri yönetebilirsiniz.

![Dizin, kullanıcılar ve abonelikleri ilişkiyi gösteren ekran görüntüsü.](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

Daha fazla bilgi için bkz: [Azure aboneliklerinin Azure Active Directory ile ilişkili](../active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.
Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için. 
