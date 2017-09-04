---
title: "Azure aboneliklerinin Azure Active Directory ile ilişkisi | Microsoft Docs"
description: "Microsoft Azure'da oturum açma ve Azure aboneliğinin Azure Active Directory ile ilişkisi gibi ilgili konular."
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
ms.date: 08/22/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: b2f8cc774b6cc245a4000e5c6924a8bb817707bc
ms.contentlocale: tr-tr
ms.lasthandoff: 08/24/2017

---
# <a name="how-azure-subscriptions-are-associated-with-azure-active-directory"></a>Azure aboneliklerinin Azure Active Directory ile ilişkisi
Bu makalede, Azure aboneliğinin Azure Active Directory (Azure AD) ile ilişkisi konusuna yönelik bilgiler ele alınmaktadır.

## <a name="your-azure-subscriptions-relationship-to-azure-ad"></a>Azure aboneliğinizin Azure AD ile ilişkisi
Azure aboneliğinizin Azure AD ile bir güven ilişkisi olması, kullanıcıların, hizmetlerin ve cihazların kimliğini doğrulamak için dizine güvendiği anlamına gelir. Birden çok abonelik aynı dizine güvenebilir ancak her abonelik yalnızca bir dizine güvenir. 

Aboneliğin bir dizinle arasındaki bu güven ilişkisi, aboneliğin Azure'daki diğer kaynaklarla (web siteleri, veritabanları ve benzeri) sahip olduğu ilişkiye benzer nitelikte değildir. Bir aboneliğin süresi dolarsa abonelikle ilişkili diğer kaynaklara erişim de durdurulur. Ancak dizin Azure içinde kalır, siz de farklı bir aboneliği bu dizinle ilişkilendirebilir, dizin kullanıcılarını yönetmeye devam edebilirsiniz.

![abonelik ilişkileri diyagramı](./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png)

Azure AD, Azure aboneliğinizde diğer hizmetler gibi çalışmaz. Diğer Azure Hizmetleri Azure aboneliğinize bağımlıdır. Ancak Azure AD'de gördükleriniz aboneliğe göre farklılık göstermez. Oturum açmış kullanıcıya göre dizinlere erişim sağlanır.

Tüm kullanıcılar kimliklerini doğrulayan tek giriş dizinine sahiptir ancak diğer dizinlerde konuk da olabilmektedir. Azure AD içinde yalnızca, kullanıcı hesabınızın üye olduğu dizinleri görebilirsiniz. Bir dizin aynı zamanda şirket içi Active Directory ile eşitlenebilir.

## <a name="azure-ad-and-cloud-service-subscriptions"></a>Azure AD ve bulut hizmeti abonelikleri
Azure AD, aşağıdakiler dahil olmak üzere çoğu Microsoft bulut hizmetinin dışında çekirdek dizin ve kimlik yönetimi özellikleri sağlar:

* Azure
* Microsoft Office 365
* Microsoft Dynamics CRM Online
* Microsoft Intune

Bu Microsoft bulut hizmetlerinden herhangi birine kaydolduğunuzda, Azure AD hizmetine ücretsiz sahip olursunuz. Azure AD dizini için ek bir Azure aboneliği eklemek istiyorsanız, yalnızca bir Microsoft hesabıyla oturum açtıysanız bunu yapabilirsiniz. Örneğin, bir Microsoft hesabı kullanarak Azure'a kaydolduysanız ve farklı bir Microsoft bulut hizmetine bir iş veya okul hesabı kullanarak kaydolduysanız, iki Azure AD örneğiniz vardır:
1. Azure aboneliğiniz için varsayılan dizin. Azure tarafından kimliğiniz doğrulanabileceğinden, bir Microsoft hesabıyla oturum açtıysanız bu dizine veya oluşturduğunuz diğerlerine başka bir Azure aboneliği ekleyebilirsiniz.
2. İş veya okul hesabınız için giriş dizini. Azure’da bir iş veya okul hesabıyla oturum açarsanız, Azure iş veya okul hesabınızın kimliğini doğrulayamadığından, mevcut bir dizine Azure aboneliği ekleyemezsiniz. 
 
Bir Azure aboneliğinin yöneticilerini değiştirme hakkında daha fazla bilgi için bkz. [Azure aboneliğinin sahipliğini başka bir hesaba devretme](../billing/billing-subscription-transfer.md)

## <a name="suggestions-to-manage-both-a-subscription-and-a-directory"></a>Bir aboneliği ve dizini yönetmek için öneriler
Bir Azure aboneliğine yönelik yönetim rolleri, Azure aboneliği ile bağlantılı kaynakları yönetir. Bu bölümde Azure aboneliği yöneticilerinin ve Azure AD dizini yöneticilerinin arasındaki farklar açıklanmaktadır. Yönetici rolleri ve bunları aboneliğinizi yönetmek için kullanma konusunda diğer öneriler [Azure Active Directory’de yönetici rolü atama](active-directory-assign-admin-roles.md) konusunda ele alınmıştır.

Varsayılan olarak, kaydolduğunuzda size Hizmet Yöneticisi rolü atanır. Başkalarının aynı aboneliği kullanarak oturum açması ve hizmetlere erişmesi gerekiyorsa bu kişileri ortak yönetici olarak ekleyebilirsiniz. 

Azure AD, dizin ve kimlikle ilgili özelliklerin yönetilmesine ilişkin farklı bir yönetim rolleri dizisine sahiptir. Örneğin, bir dizinin genel yöneticisi dizine kullanıcı ve grup ekleyebilir veya kullanıcılar için çok faktörlü kimlik doğrulamasını gerekli kılabilir. Dizin oluşturan bir kullanıcı, genel yönetici rolüne atanır ve diğer kullanıcılara yönetici rolleri atayabilir. Azure AD yönetim rolleri aynı zamanda Office 365 ve Microsoft Intune gibi diğer hizmetler tarafından da kullanılır. 

Azure aboneliği yöneticilerinin ve Azure AD dizini yöneticilerinin iki farklı rol olmasıdır. 
* Azure aboneliği yöneticileri, Azure'daki kaynakları yönetebilir ve (Azure portalı bir Azure kaynağı olduğundan) Azure portalında Azure AD kullanabilir. 
* Dizin yöneticileri yalnızca Azure AD dizinindeki özellikleri yönetebilir.

Bir kişi her iki rolde de olabilir ancak bu gerekli değildir. Bir kullanıcı, dizin genel yöneticisi rolüne atanabilir ancak Hizmet yöneticisi veya bir Azure aboneliğinin ortak yöneticisi olarak atanamaz. Kullanıcılar, yönetici aboneliğin olmadan Azure portalında oturum açabilir, ancak portalda bu abonelik için dizinleri yönetemezler. Bu kullanıcı Azure AD PowerShell veya Office 365 Yönetici Merkezi gibi diğer araçları kullanarak dizinleri yönetebilir.

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure aboneliğinin yöneticilerini değiştirme hakkında daha fazla bilgi için bkz. [Azure aboneliğinin sahipliğini başka bir hesaba devretme](../billing/billing-subscription-transfer.md)
* Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](active-directory-understanding-resource-access.md)
* Azure AD'de rol atama hakkında daha fazla bilgi için bkz. [Azure Active Directory'de yönetici rolü atama](active-directory-assign-admin-roles-azure-portal.md)

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG

