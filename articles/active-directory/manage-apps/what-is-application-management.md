---
title: Azure Active Directory ile Uygulamaları Yönetme | Microsoft Docs
description: Bu makalede, Azure Active Directory’yi şirket içi, bulut ve SaaS uygulamalarınızla tümleştirmenin avantajları açıklanmaktadır.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: overview
ms.workload: identity
ms.date: 10/30/2018
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 657527272ccf8b5f69764052a2385ceec57ddc03
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65826028"
---
# <a name="application-management-with-azure-active-directory"></a>Azure Active Directory ile uygulama yönetimi

Azure Active Directory (Azure AD), bulut ve şirket içi uygulamalara güvenli ve sorunsuz erişim sağlar. Kullanıcılar, Office 365’e ve Microsoft’un diğer iş uygulamalarına, hizmet olarak yazılım (SaaS) uygulamalarına, şirket içi uygulamalara ve iş kolu (LOB) uygulamalarına erişmek için bir kez oturum açabilir. Kullanıcı sağlamayı otomatikleştirerek yönetim maliyetlerini azaltın. Güvenli uygulama erişimi sağlamak için çok faktörlü kimlik doğrulaması ve koşullu erişim ilkelerini kullanın.

![Azure AD yoluyla federasyon oluşturan uygulamalar](media/what-is-application-management/app-management-overview.png)

## <a name="why-manage-applications-with-a-cloud-solution"></a>Neden bir bulut çözümüyle uygulamalar yönetilmelidir?

Kuruluşların çoğu zaman kullanıcıların işlerini yapmak için bağımlı oldukları yüzlerce uygulaması vardır. Kullanıcılar birçok cihaz ve konumdan bu uygulamalara erişir. Her gün yeni uygulamalar eklenir ve geliştirilir. Bu kadar çok uygulama ve erişim noktası olduğunda, tüm uygulamalara kullanıcı erişimini yönetmek için bulut tabanlı bir çözüm kullanmak çok daha kritik hale gelmiştir.

## <a name="manage-risk-with-conditional-access-policies"></a>Koşullu erişim ilkeleriyle riski yönetme
Azure AD çoklu oturum açma (SSO) ile koşullu erişim ilkelerinin eşlenmesiyle, uygulamalara erişmeye yönelik yüksek düzeyde güvenlik sağlanır. Güvenlik özellikleri arasında, bulut ölçeğinde kimlik koruması, riske dayalı erişim denetimi, yerel çok faktörlü kimlik doğrulaması ve koşullu erişim ilkeleri yer alır. Bu özellikler, uygulamalara veya yüksek düzeyde güvenlik gereken gruplara göre ayrıntılı denetim ilkelerine olanak sağlar.

## <a name="improve-productivity-with-single-sign-on"></a>Çoklu oturum açma ile üretkenliği artırma
Uygulamalar ve Office 365 arasında çoklu oturum açma (SSO) etkinleştirildiğinde, oturum açma istemleri azaltılarak veya ortadan kaldırılarak mevcut kullanıcılar için üstün bir oturum açma deneyimi sağlanır. Birden çok istem veya birden çok parolayı yönetme gereksinimi olmadan kullanıcının ortamı çok daha bütünlüklü ve daha az rahatsız edici olur. İş grubu, self servis ve dinamik üyelik aracılığıyla erişimi yönetebilir ve onaylayabilir. İşletmede doğru kişilerin bir uygulamaya erişmesine izin verilmesi, kimlik sisteminin güvenliğini artırır.

SSO, güvenliği artırır. *Çoklu oturum açma olmadan* yöneticilerin her bir bireysel uygulama için kullanıcı hesapları oluşturması ve güncelleştirmesi gerekir; bu da zaman alır. Ayrıca kullanıcıların uygulamalarına erişmek için birden çok kimlik bilgisini izlemesi gerekir. Sonuç olarak kullanıcılar, parolalarını yazma veya başka parola yönetim çözümlerini kullanma eğilimindedir; bu da veri güvenliği riskleri oluşturur. 

## <a name="address-governance-and-compliance"></a>İdare ve uyumu ele alma
Azure AD ile, Güvenlik Bilgileri ve Olay Yönetimi (SIEM) araçlarından yararlanan raporlar aracılığıyla uygulama oturum açma işlemlerini izleyebilirsiniz. Portaldan veya API’lerden raporlara erişebilirsiniz. Uygulamalarınıza kimlerin erişiminin olduğunu programlama yoluyla denetleyin ve erişim gözden geçirmeleri yoluyla etkin olmayan kullanıcılara erişimi kaldırın.

## <a name="manage-costs"></a>Maliyetleri yönetin
Azure AD’ye geçirerek maliyet tasarrufu yapabilir ve şirket içi altyapınızı yönetme zahmetini ortadan kaldırabilirsiniz. Azure AD, uygulamalara self servis erişimi de sağlar; böylece hem yöneticiler hem de kullanıcılar için zaman tasarrufu sağlanır. Çoklu oturum açma, uygulamaya özgü parolaları ortadan kaldırır. Bir kez oturum açma, uygulamalar için parola sıfırlama ve parolalar alınırken oluşan üretkenlik kaybı ile ilgili maliyetlerden tasarruf sağlar.

