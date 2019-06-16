---
title: Azure Active Directory ile Uygulamaları Yönetme | Microsoft Docs
description: Bu makalede, Azure Active Directory, şirket içi, Bulut ve SaaS uygulamalarını tümleştirme avantajları anlatılmaktadır.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: overview
ms.workload: identity
ms.date: 06/05/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9246d7bd48579def171986606e88c09593029aa2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108144"
---
# <a name="application-management-with-azure-active-directory"></a>Azure Active Directory ile uygulama yönetimi

Azure Active Directory (Azure AD), Bulut ve şirket içi uygulamalarınız için tek kimlik sistemi sağlayarak, uygulamalarınızı yönetme kolaylaştırır. Yazılım (SaaS) hizmet olarak ekleyebileceğiniz uygulamaları, şirket içi uygulamaları ve iş kolu (LOB) uygulamaları Azure AD'ye. Ardından kullanıcılar bir kez güvenli ve sorunsuz bir şekilde, Office 365 ve Microsoft'un diğer iş uygulamaları ile birlikte bu uygulamaya erişmek oturum açın. Kullanıcı sağlamayı otomatikleştirerek yönetim maliyetlerini azaltabilirsiniz. Güvenli uygulama erişimi sağlamak için çok faktörlü kimlik doğrulama ve koşullu erişim ilkelerini de kullanabilirsiniz.

![Azure AD yoluyla federasyon oluşturan uygulamalar](media/what-is-application-management/app-management-overview.png)

## <a name="why-manage-applications-with-a-cloud-solution"></a>Neden bir bulut çözümüyle uygulamalar yönetilmelidir?

Kuruluşların çoğu zaman kullanıcıların işlerini yapmak için bağımlı oldukları yüzlerce uygulaması vardır. Kullanıcılar birçok cihaz ve konumdan bu uygulamalara erişir. Her gün yeni uygulamalar eklenir ve geliştirilir. Çok sayıda uygulama ve erişim noktaları ile hiç olmadığı kadar bulut tabanlı bir çözüm tüm uygulamalara kullanıcı erişimini yönetmek için kullanımı çok daha önemlidir.

## <a name="what-types-of-applications-can-i-integrate-with-azure-ad"></a>Hangi tür uygulamaları Azure AD'ye tümleştirebilirim?
Ekleyebileceğiniz uygulamaların dört ana türü vardır, **kurumsal uygulamalar** ve Azure AD ile yönetebilirsiniz:

-   **Azure AD galeri uygulamaları** – Azure AD çoklu oturum açma için Azure AD ile önceden tümleştirilmiş olan uygulamaların binlerce içeren bir galeri sahiptir. Galeride kuruluşunuzun kullandığı uygulamaların bazıları da mevcuttur. [Uygulama tümleştirmenizi planlama hakkında bilgi edinin](plan-an-application-integration.md), ya da tek tek uygulamalar için ayrıntılı tümleştirme adımları alma [SaaS uygulama öğreticileri](https://docs.microsoft.com/azure/active-directory/saas-apps/). 

-   **Şirket içi uygulama ara sunucusu ile uygulamalara** – ile Azure AD uygulama proxy'si, şirket içi web apps ile Azure AD çoklu oturum açmayı destekleyecek şekilde tümleştirilebilir. Daha sonra son kullanıcıların şirket içi web uygulamalarınızı Office 365 ve diğer SaaS uygulamalarına eriştikleri aynı şekilde erişebilirsiniz. [Uygulama Ara sunucusunu kullanacak şekilde neden ve nasıl çalıştığını öğrenin](what-is-application-proxy.md).

-   **Özel olarak geliştirilmiş uygulamaları** – kendi satır iş kolu uygulamaları oluştururken, bunları çoklu oturum açmayı desteklemek için Azure AD ile tümleştirebilirsiniz. Uygulamanızı Azure AD ile kaydederek, uygulama için kimlik doğrulama İlkesi üzerinde denetime sahiptir. Daha fazla bilgi için [geliştiriciler için kılavuz](developer-guidance-for-integrating-applications.md).

-   **Galeri dışı uygulamalar** – kendi uygulamalarınızı getirin! Çoklu oturum açma için diğer uygulamaları Azure AD'ye ekleyerek destekler. İstediğiniz herhangi bir web bağlantısına veya bir kullanıcı adı ve parola alanı işler, SAML veya Openıd Connect protokolleri destekler veya SCIM'yi destekleyen herhangi bir uygulama tümleştirebilirsiniz. Daha fazla bilgi için [galeri dışı uygulamalar için çoklu oturum açmayı yapılandırma](configure-single-sign-on-non-gallery-applications.md).

## <a name="manage-risk-with-conditional-access-policies"></a>Koşullu erişim ilkeleri ile risk yönetme
Azure AD çoklu oturum açma (SSO) ile eşlenmesiyle [koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) uygulamaları erişmek için yüksek düzeyde güvenlik sağlar. Bulut ölçeğinde kimlik koruması, risk tabanlı erişim denetimi, yerel çok faktörlü kimlik doğrulama ve koşullu erişim ilkeleri, güvenlik özellikleri içerir. Bu özellikler, uygulamalara veya yüksek düzeyde güvenlik gereken gruplara göre ayrıntılı denetim ilkelerine olanak sağlar.

## <a name="improve-productivity-with-single-sign-on"></a>Çoklu oturum açma ile üretkenliği artırma
Uygulamalar ve Office 365 arasında çoklu oturum açma (SSO) etkinleştirildiğinde, oturum açma istemleri azaltılarak veya ortadan kaldırılarak mevcut kullanıcılar için üstün bir oturum açma deneyimi sağlanır. Birden çok istem veya birden çok parolayı yönetme gereksinimi olmadan kullanıcının ortamı çok daha bütünlüklü ve daha az rahatsız edici olur. İş grubu, self servis ve dinamik üyelik aracılığıyla erişimi yönetebilir ve onaylayabilir. İşletmede doğru kişilerin bir uygulamaya erişmesine izin verilmesi, kimlik sisteminin güvenliğini artırır.

SSO, güvenliği artırır. *Çoklu oturum açma olmadan* yöneticilerin her bir bireysel uygulama için kullanıcı hesapları oluşturması ve güncelleştirmesi gerekir; bu da zaman alır. Ayrıca kullanıcıların uygulamalarına erişmek için birden çok kimlik bilgisini izlemesi gerekir. Sonuç olarak kullanıcılar, parolalarını yazma veya başka parola yönetim çözümlerini kullanma eğilimindedir; bu da veri güvenliği riskleri oluşturur. [Hakkında daha fazla bilgiyi çoklu oturum açma](what-is-single-sign-on.md).

## <a name="address-governance-and-compliance"></a>İdare ve uyumu ele alma
Azure AD ile, Güvenlik Bilgileri ve Olay Yönetimi (SIEM) araçlarından yararlanan raporlar aracılığıyla uygulama oturum açma işlemlerini izleyebilirsiniz. Portaldan veya API’lerden raporlara erişebilirsiniz. Uygulamalarınıza kimlerin erişiminin olduğunu programlama yoluyla denetleyin ve erişim gözden geçirmeleri yoluyla etkin olmayan kullanıcılara erişimi kaldırın.

## <a name="manage-costs"></a>Maliyetleri yönetme
Azure AD’ye geçirerek maliyet tasarrufu yapabilir ve şirket içi altyapınızı yönetme zahmetini ortadan kaldırabilirsiniz. Azure AD, uygulamalara self servis erişimi de sağlar; böylece hem yöneticiler hem de kullanıcılar için zaman tasarrufu sağlanır. Çoklu oturum açma, uygulamaya özgü parolaları ortadan kaldırır. Bir kez oturum açma, uygulamalar için parola sıfırlama ve parolalar alınırken oluşan üretkenlik kaybı ile ilgili maliyetlerden tasarruf sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama Ara sunucusu nedir?](what-is-application-proxy.md)
- [Hızlı Başlangıç: Azure AD kiracınız için Galeri bir uygulama ekleyin](add-application-portal.md)
