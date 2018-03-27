---
title: Azure AD dizininize var olan bir Azure aboneliğini ekleme | Microsoft Docs
description: Azure AD dizininize var olan bir aboneliği ekleme
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.assetid: bc4773c2-bc4a-4d21-9264-2267065f0aea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/12/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 2b9ed5b3374c98740923790dac90cb1eb2f065d8
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="how-to-associate-or-add-an-azure-subscription-to-azure-active-directory"></a>Azure Active Directory'ye bir Azure aboneliğini ekleme veya ilişkilendirme

Bu makalede, Azure aboneliğinin Azure Active Directory (Azure AD) ile ilişkisi gibi ilgili konulara yönelik bilgiler ve mevcut bir aboneliği Azure AD dizinine nasıl ekleyeceğiniz ele alınmaktadır. Azure aboneliğinizin Azure AD ile bir güven ilişkisi olması, kullanıcıların, hizmetlerin ve cihazların kimliğini doğrulamak için dizine güvendiği anlamına gelir. Birden çok abonelik aynı dizine güvenebilir ancak her abonelik yalnızca bir dizine güvenir. 

Aboneliğin bir dizinle arasındaki bu güven ilişkisi, aboneliğin Azure'daki diğer kaynaklarla (web siteleri, veritabanları ve benzeri) sahip olduğu ilişkiye benzer nitelikte değildir. Bir aboneliğin süresi dolarsa abonelikle ilişkili diğer kaynaklara erişim de durdurulur. Ancak Azure AD dizini Azure içinde kalır, siz de farklı bir aboneliği bu dizinle ilişkilendirebilir ve dizini yeni aboneliği kullanarak yönetebilirsiniz.

Tüm kullanıcılar kimliklerini doğrulayan tek giriş dizinine sahiptir ancak diğer dizinlerde konuk da olabilmektedir. Azure AD içinde yalnızca, kullanıcı hesabınızın üye veya konuk olduğu dizinleri görebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

* Aboneliğe erişmek için RBAC Sahip erişimi olan hesapla oturum açmanız gerekir.
* Hem aboneliğin ilişkili olduğu geçerli dizinde hem de eklemek istediğiniz dizinde olan bir hesapla oturum açmanız gerekir. Başka bir dizine erişim elde etme hakkında daha fazla bilgi için bkz. [Azure Active Directory yöneticileri B2B işbirliği kullanıcılarını nasıl ekler?](active-directory-b2b-admin-add-users.md)
* Bu özellik, CSP (MS-AZR-0145P, MS-AZR-0146P, MS-AZR-159P) ve Microsoft Imagine (MS-AZR-0144P) abonelikleri için kullanılamaz.

## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>Var olan bir aboneliği Azure AD dizininizle ilişkilendirme

1. Oturum açın ve [Azure portalındaki Abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) bir abonelik seçin.
2. **Dizin değiştir**’e tıklayın.

    ![Dizin değiştir düğmesini gösteren ekran görüntüsü](./media/active-directory-how-subscriptions-associated-directory/edit-directory-button.PNG)
3. Uyarıları gözden geçirin. Abonelik dizini değiştiğinde, erişim atanmış tüm [Rol Tabanlı Erişim Denetimi (RBAC)](role-based-access-control-configure.md) kullanıcıları ve tüm abonelik yöneticileri, erişimini kaybeder.
4. Bir dizin seçin.

    ![Değişiklik dizini kullanıcı arabirimini gösteren ekran görüntüsü](./media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.PNG)
5. **Değiştir**’e tıklayın.
6. Başarılı! Yeni dizine gitmek için dizin değiştiricisini kullanın. Her şeyin doğru şekilde gösterilmesi 10 dakikaya kadar sürebilir.

    ![Dizin değiştir başarı bildirimini gösteren ekran görüntüsü](./media/active-directory-how-subscriptions-associated-directory/edit-directory-success.PNG)

    ![Değiştiriciyi gösteren ekran görüntüsü](./media/active-directory-how-subscriptions-associated-directory/directory-switcher.PNG)


Abonelik dizininin değiştirilmesi, hizmet düzeyinde bir işlemdir. Abonelik faturalandırma sahipliğini etkilemez ve Hesap Yöneticisi hala [Hesap Merkezi](https://account.azure.com/subscriptions)’ni kullanarak Hizmet Yöneticisi’ni değiştirebilir. Özgün dizini silmek istiyorsanız, abonelik faturalandırma sahipliğini yeni bir Hesap Yöneticisine aktarmanız gerekir. Faturalandırma sahipliğini aktarma hakkında daha fazla bilgi için bkz. [Bir Azure aboneliğinin sahipliğini başka bir hesaba aktarma](../billing/billing-subscription-transfer.md). 

## <a name="next-steps"></a>Sonraki adımlar

* Ücretsiz olarak yeni bir Azure AD dizini oluşturma hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory kiracısı alma](develop/active-directory-howto-tenant.md)
* Bir Azure aboneliğinin faturalandırma sahipliğini aktarma hakkında daha fazla bilgi için bkz. [Bir Azure aboneliğinin sahipliğini başka bir hesaba aktarma](../billing/billing-subscription-transfer.md)
* Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](active-directory-understanding-resource-access.md)
* Azure AD'de rol atama hakkında daha fazla bilgi için bkz. [Azure Active Directory'de yönetici rolü atama](active-directory-assign-admin-roles-azure-portal.md)

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG
