---
title: Mevcut bir Azure aboneliği, kiracınıza - Azure Active Directory Ekle | Microsoft Docs
description: Azure Active Directory kiracınız için mevcut bir Azure aboneliği ekleme hakkında yönergeler.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3322e49c6fdc590b785806f67b5081700bf8b37b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59788642"
---
# <a name="associate-or-add-an-azure-subscription-to-your-azure-active-directory-tenant"></a>Azure Active Directory kiracınız için Azure aboneliği ekleme veya ilişkilendirme

Azure aboneliğinin Azure Active Directory'ye abonelik kullanıcılar, hizmet ve cihaz kimliğini doğrulamak için Azure AD güvendiği anlamına gelir (Azure AD), bir güven ilişkisi vardır. Birden çok abonelik Azure AD ile aynı dizine güvenebilir ancak her abonelik yalnızca tek bir dizine güvenebilir.

Aboneliğinizin süresi dolarsa abonelikle ilişkili diğer kaynaklara erişimlerini kaybeder. Ancak, Azure AD dizini ilişkilendirmek ve farklı bir Azure aboneliğini kullanarak dizin yönetmenize izin vererek, Azure içinde kalır.

Tüm kullanıcılarınızın sahip tek bir *giriş* kimlik doğrulaması için dizin. Bununla birlikte, kullanıcılarınızın diğer dizinlerde Konuk da olabilir. Azure AD'de her kullanıcı için her iki bir ev ve Konuk dizin görebilirsiniz.

> [!Important]
> Farklı bir dizin rolleri kullanarak atanmış kullanıcılar için bir abonelik ilişkilendirdiğinizde [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/role-assignments-portal.md) erişimlerini kaybedeceklerdir. Klasik abonelik yöneticileri (Hizmet Yöneticisi ve ortak Yöneticiler) da erişimi kaybedersiniz.
> 
> Ayrıca, Azure Kubernetes Service (AKS) kümenizi farklı bir aboneliğe taşınmasını ya da yeni bir kiracı için küme sahip olan abonelik taşıma işlevselliği kayıp rol atamaları ve hizmet sorumluları hakları nedeniyle kümenin neden olur. AKS hakkında daha fazla bilgi için bkz: [Azure Kubernetes Service (AKS)](https://docs.microsoft.com/en-us/azure/aks/).

## <a name="before-you-begin"></a>Başlamadan önce

İlişkilendirme veya aboneliğinize eklemek için önce aşağıdaki görevleri gerçekleştirmeniz gerekir:

1. Aşağıdaki değişiklikler ve size nasıl etkilenebilecek listesini gözden geçirin:

    - RBAC kullanarak rolleri atanmış kullanıcılar erişimlerini kaybedeceklerdir
    - Hizmet Yöneticisi ve ortak yöneticilerin erişimi kaybedersiniz.
    - Tüm anahtar kasalarını varsa, bunlar erişilemez olacaktır ve bunları ilişkilendirmeden sonra düzeltmek zorunda kalırsınız
    - Kayıtlı bir Azure Stack varsa ilişkilendirmeden sonra yeniden kaydetmeniz gerekir

1. Bir hesap kullanarak oturum açın:
    - Sahip bir [sahibi](../../role-based-access-control/built-in-roles.md#owner) abonelik için rol ataması. Sahip rolü atama hakkında daha fazla bilgi için bkz: [RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetme](../../role-based-access-control/role-assignments-portal.md).
    - Abonelikle ilişkili iki geçerli dizindeki ve, bundan sonra abonelik ilişkilendirmek istediğiniz yeni dizine bulunmaktadır. Başka bir dizine erişim sağlama hakkında daha fazla bilgi için bkz. [nasıl Azure Active Directory yöneticileri ekleme B2B işbirliği kullanıcıları?](../b2b/add-users-administrator.md).

1. Bir Azure bulut hizmeti sağlayıcıları (CSP) aboneliği (MS-AZR - 0145 P, MS - AZR - 0146 P, MS - AZR - 159 P), bir Microsoft Internal abonelik (MS-AZR - 0015 P) veya Microsoft Imagine aboneliği (MS-AZR - 0144 P) kullanmadığınızdan emin olun.
    
## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>Var olan bir aboneliği Azure AD dizininizle ilişkilendirme

1. Oturum açın ve kullanmak istediğiniz aboneliği seçin [Azure portalındaki abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

2. Seçin **dizinini Değiştir**.

    ![Abonelik sayfasında, dizin seçeneği vurgulanmış olarak değiştir](media/active-directory-how-subscriptions-associated-directory/change-directory-button.png)

3. Görüntülenen ve ardından tüm uyarıları gözden **değişiklik**.

    ![Dizini değiştirmek için gösteren dizin sayfası Değiştir](media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.png)

    Abonelik için dizinin değiştirilir ve bir başarı iletisi alın.

    ![Başarılı iletisi dizin değişikliği hakkında](media/active-directory-how-subscriptions-associated-directory/edit-directory-success.png)    
4. Kullanım **dizin değiştiricisini** yeni dizininize gidin. Her şeyin doğru şekilde gösterilmesi 10 dakikaya kadar sürebilir.

    ![Örnek bilgileri içeren dizin değiştirici sayfası](media/active-directory-how-subscriptions-associated-directory/directory-switcher.png)

Abonelik faturalandırma sahipliğini etkilemez abonelik dizininin değiştirilmesi bir hizmet düzeyi işlemi olduğundan. Hesap Yöneticisi hala Hizmet Yöneticisi'nden değiştirebilirsiniz [hesap Merkezi](https://account.azure.com/subscriptions). Özgün dizini silmek için abonelik faturalandırma sahipliğini için yeni bir hesap yöneticisine aktarmanız gerekir Faturalandırma sahipliğini aktarma hakkında daha fazla bilgi için bkz. [Bir Azure aboneliğinin sahipliğini başka bir hesaba aktarma](../../billing/billing-subscription-transfer.md).

## <a name="post-association-steps"></a>İlişkilendirme sonrası yönergeleri
Abonelik farklı bir dizine ilişkilendirdikten sonra işlemlerini sürdürmek için gerçekleştirmesi gereken ek adımlar olabilir.

1. Tüm anahtar kasalarını varsa, anahtar kasası Kiracı kimliğini değiştirme Daha fazla bilgi için [abonelik taşıma işlemi sonrasında anahtar kasası Kiracı Kimliğini değiştirme](../../key-vault/key-vault-subscription-move-fix.md).

2. Bu aboneliği kullanarak bir Azure Stack kayıtlıysanız, yeniden kaydetmeniz gerekir. Daha fazla bilgi için [kaydetme Azure Stack Azure ile](../../azure-stack/azure-stack-registration.md).



## <a name="next-steps"></a>Sonraki adımlar

- Yeni bir Azure AD oluşturmak için Kiracı için bkz: [erişim Azure yeni bir kiracı oluşturmak için Active Directory](active-directory-access-create-new-tenant.md)

- Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](../../role-based-access-control/rbac-and-directory-admin-roles.md)

- Azure AD'de rol atama hakkında daha fazla bilgi için bkz: [kullanıcılar Azure Active Directory'ye Dizin rolleri atama](active-directory-users-assign-role-azure-portal.md)
