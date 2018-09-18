---
title: Azure Active Directory kiracınız için mevcut bir Azure aboneliği ekleme | Microsoft Docs
description: Azure Active Directory kiracınız için mevcut bir Azure aboneliğine eklemeyi öğrenin.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: dd62b22eca40a214c5b08a9bc48815e40fe90e47
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984095"
---
# <a name="how-to-associate-or-add-an-azure-subscription-to-azure-active-directory"></a>Nasıl yapılır: ilişkilendirmek veya Azure Active Directory'ye bir Azure aboneliği ekleme
Azure aboneliğinin Azure Active Directory'ye abonelik kullanıcılar, hizmet ve cihaz kimliğini doğrulamak için Azure AD güvendiği anlamına gelir (Azure AD), bir güven ilişkisi vardır. Birden çok abonelik Azure AD ile aynı dizine güvenebilir ancak her abonelik yalnızca tek bir dizine güvenebilir.

Aboneliğinizin süresi dolarsa abonelikle ilişkili diğer kaynaklara erişimlerini kaybeder. Ancak, Azure AD dizini ilişkilendirmek ve farklı bir Azure aboneliğini kullanarak dizin yönetmenize izin vererek, Azure içinde kalır.

Tüm kullanıcılarınızın kimlik doğrulaması için tek bir "Giriş" dizinine sahip. Bununla birlikte, kullanıcılarınızın diğer dizinlerde Konuk da olabilir. Azure AD'de her kullanıcı için her iki bir ev ve Konuk dizin görebilirsiniz.

>[!Important]
>Tüm [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/role-assignments-portal.md) tüm abonelik yöneticileri birlikte atanan erişimi olan kullanıcıların erişimini kaybedecek sonra Abonelik dizin değişikliklerini. Tüm anahtar kasalarını varsa, ek olarak, ayrıca abonelik taşıma tarafından etkilenmiş. Bu sorunu gidermek için şunları yapmalısınız [anahtar kasası Kiracı Kimliğini değiştirme](../../key-vault/key-vault-subscription-move-fix.md) önce işlem sürdürülüyor.


## <a name="before-you-begin"></a>Başlamadan önce
İlişkilendirme veya aboneliğinize eklemek için önce aşağıdaki görevleri gerçekleştirmeniz gerekir:

- Bir hesap kullanarak oturum açın:
    - Sahip **RBAC sahip** aboneliğe erişim.

    - Abonelikle ilişkili iki geçerli dizindeki ve, bundan sonra abonelik ilişkilendirmek istediğiniz yeni dizine bulunmaktadır. Başka bir dizine erişim sağlama hakkında daha fazla bilgi için bkz. [nasıl Azure Active Directory yöneticileri ekleme B2B işbirliği kullanıcıları?](../b2b/add-users-administrator.md).

- Bir Azure bulut hizmeti sağlayıcıları (CSP) aboneliği (MS-AZR - 0145 P, MS - AZR - 0146 P, MS - AZR - 159 P), bir Microsoft Internal abonelik (MS-AZR - 0015 P) veya Microsoft Imagine aboneliği (MS-AZR - 0144 P) kullanmadığınızdan emin olun.
    
## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>Var olan bir aboneliği Azure AD dizininizle ilişkilendirme
1. Oturum açın ve kullanmak istediğiniz aboneliği seçin [Azure portalındaki abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

2. Seçin **dizinini Değiştir**.

    ![Abonelik sayfasında, dizin seçeneği vurgulanmış olarak değiştir](media/active-directory-how-subscriptions-associated-directory/change-directory-button.png)

3. Görüntülenen ve ardından tüm uyarıları gözden **değişiklik**.

    ![Dizini değiştirmek için gösteren dizin sayfası Değiştir](media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.png)

    Abonelik için dizinin değiştirilir ve bir başarı iletisi alın.

    ![Başarı iletisi](media/active-directory-how-subscriptions-associated-directory/edit-directory-success.png)    

4. Yeni dizininizi gitmek için dizin değiştiricisini kullanın. Her şeyin doğru şekilde gösterilmesi 10 dakikaya kadar sürebilir.

    ![Dizin değiştirici sayfası](media/active-directory-how-subscriptions-associated-directory/directory-switcher.png)

Abonelik faturalandırma sahipliğini etkilemez abonelik dizininin değiştirilmesi bir hizmet düzeyi işlemi olduğundan. Hesap Yöneticisi hala Hizmet Yöneticisi'nden değiştirebilirsiniz [hesap Merkezi](https://account.azure.com/subscriptions). Özgün dizini silmek için abonelik faturalandırma sahipliğini için yeni bir hesap yöneticisine aktarmanız gerekir Faturalandırma sahipliğini aktarma hakkında daha fazla bilgi için bkz. [Bir Azure aboneliğinin sahipliğini başka bir hesaba aktarma](../../billing/billing-subscription-transfer.md). 

## <a name="next-steps"></a>Sonraki adımlar

- Yeni bir Azure AD oluşturmak için Kiracı için bkz: [erişim Azure yeni bir kiracı oluşturmak için Active Directory](active-directory-access-create-new-tenant.md)

- Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](../../role-based-access-control/rbac-and-directory-admin-roles.md)

- Azure AD'de rol atama hakkında daha fazla bilgi için bkz: [kullanıcılar Azure Active Directory'ye Dizin rolleri atama](active-directory-users-assign-role-azure-portal.md)
