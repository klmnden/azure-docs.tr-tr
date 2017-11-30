---
title: "Azure AD dizininize var olan bir Azure aboneliğini ekleme | Microsoft Docs"
description: "Azure AD dizininize var olan bir aboneliği ekleme"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: bc4773c2-bc4a-4d21-9264-2267065f0aea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/17/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 67df6d893c0770b9210fc73e96865d5c6396796c
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-add-an-azure-subscription-to-azure-active-directory"></a>Azure Active Directory'ye bir Azure aboneliğini ekleme
Bu makalede, Azure aboneliğinin Azure Active Directory (Azure AD) ile ilişkisi gibi ilgili konulara yönelik bilgiler ve mevcut bir aboneliği Azure AD dizinine nasıl ekleyeceğiniz ele alınmaktadır. Azure aboneliğinizin Azure AD ile bir güven ilişkisi olması, kullanıcıların, hizmetlerin ve cihazların kimliğini doğrulamak için dizine güvendiği anlamına gelir. Birden çok abonelik aynı dizine güvenebilir ancak her abonelik yalnızca bir dizine güvenir. 

Aboneliğin bir dizinle arasındaki bu güven ilişkisi, aboneliğin Azure'daki diğer kaynaklarla (web siteleri, veritabanları ve benzeri) sahip olduğu ilişkiye benzer nitelikte değildir. Bir aboneliğin süresi dolarsa abonelikle ilişkili diğer kaynaklara erişim de durdurulur. Ancak Azure AD dizini Azure içinde kalır, siz de farklı bir aboneliği bu dizinle ilişkilendirebilir ve dizini yeni aboneliği kullanarak yönetebilirsiniz.

Tüm kullanıcılar kimliklerini doğrulayan tek giriş dizinine sahiptir ancak diğer dizinlerde konuk da olabilmektedir. Azure AD içinde yalnızca, kullanıcı hesabınızın üye veya konuk olduğu dizinleri görebilirsiniz.

## <a name="to-add-an-existing-subscription-to-your-azure-ad-directory"></a>Azure AD dizininize mevcut bir abonelik eklemek için
Hem aboneliğin ilişkili olduğu geçerli dizinde hem de eklemek istediğiniz dizinde olan bir hesapla oturum açmanız gerekir. 

1. Sahipliğini devretmek istediğiniz aboneliğin Hesap Yöneticisi olan bir hesapla [Azure Hesap Merkezi](https://account.azure.com/Subscriptions)’nde oturum açın.
2. Aboneliğin sahibi olmasını istediğiniz kullanıcının hedeflenen dizininde olduğundan emin olun.
3. **Aboneliği devret**'e tıklayın.
4. Alıcıyı belirtin. Alıcı otomatik olarak, kabul bağlantısı içeren bir e-posta alır.
5. Alıcı bağlantıya tıklar ve yönergeleri izler; bu arada ödeme bilgilerini de girer. Alıcı başarılı olduğunda, abonelik devredilir. 
6. Aboneliğin varsayılan dizini ilgili kullanıcının bulunduğu dizin olarak değiştirilir.


## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure aboneliğinin yöneticilerini değiştirme hakkında daha fazla bilgi için bkz. [Azure aboneliğinin sahipliğini başka bir hesaba devretme](../billing/billing-subscription-transfer.md)
* Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](active-directory-understanding-resource-access.md)
* Azure AD'de rol atama hakkında daha fazla bilgi için bkz. [Azure Active Directory'de yönetici rolü atama](active-directory-assign-admin-roles-azure-portal.md)

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG
