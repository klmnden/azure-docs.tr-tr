---
title: Kimlik Yönetimi - Azure Active Directory | Microsoft Docs
description: Azure Active Directory kimlik yönetimini, güvenlik ve çalışan üretkenliğinin doğru işlemler ve görünürlük kuruluşunuzun gereksinimini Bakiye olanak tanır.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/29/2019
ms.author: rolyon
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: d30bbddd044d1aea70e43825035c94b69a46f1f8
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935824"
---
# <a name="what-is-azure-ad-identity-governance"></a>Azure AD kimlik yönetimi nedir?

Azure Active Directory (Azure AD) kimlik yönetimi, kuruluşunuzun gerekir için güvenlik ve çalışan üretkenliğinin doğru işlemler ve görünürlük sağlar. Doğru kullanıcının doğru kaynaklara doğru erişime sahip ve korumanıza olanak tanır, çalışanların üretkenliğini sağlarken kritik varlıkları için--izleyin ve denetleyin erişim sağlamak için özellikleri sağlar.  

Kimlik Yönetimi kuruluşların çalışanları, iş ortakları ve satıcılar ve Hizmetleri ve uygulamaları üzerinde aşağıdaki görevleri gerçekleştirmek olanağı sağlar:

- Kimlik yaşam döngüsünü yönetme
- Erişim yaşam döngüsünü yönetme
- Güvenli yönetim

Özellikle, bu dört anahtar soruları hızlandırmasına yardımcı olmak için tasarlanmıştır:

- Hangi kullanıcıların hangi kaynaklara erişimine sahip olmalıdır?
- Bu erişim ile bu kullanıcıların ne yaptıklarını?
- Erişimi yönetmek için etkili kuruluş denetimler vardır?
- Denetçiler denetimleri çalıştığını doğrulayabilirsiniz?

## <a name="identity-lifecycle"></a>Kimlik yaşam döngüsü

Kimlik Yönetimi yardımcı olan kuruluşlar arasında bir denge sağlamak *üretkenlik* -ne kadar hızlı bir kişi olabilir erişim için kaynaklar, kuruluşumun ne zaman katılmaları gibi ihtiyaç duydukları? Ve *güvenlik* -nasıl erişimleri değiştirilmelidir zamanla gibi değişiklikler nedeniyle bu kişinin iş durumuna?  Kimlik yaşam döngüsü yönetimi kimlik yönetimi için altyapıdır ve uygun ölçekte etkin idare modernleştirme uygulamalar için kimlik yaşam döngüsü Yönetim Altyapısı gerektirir.

![Kimlik yaşam döngüsü](./media/identity-governance-overview/identity-lifecycle.png)

Çoğu kuruluş için çalışanların kimlik yaşam döngüsü bir HCM (insan sermayesi Yönetimi) sistemde kullanıcı gösterimini bağlıdır.  Azure AD Premium otomatik olarak bulundurur kullanıcı kimliklerini Active Directory hem de Azure Active Directory'de workday'deki temsil kişiler için açıklandığı [Workday gelen (Önizleme) sağlama öğretici](../saas-apps/workday-inbound-tutorial.md).  Azure AD Premium ayrıca içerir [Microsoft Identity Manager](/microsoft-identity-manager/), hangi içeri aktarabilir kayıtları şirket içi HCM sistemlerden gibi SAP, Oracle eBusiness ve Oracle PeopleSoft.

Gittikçe, kuruluşunuz dışındaki kişilerle işbirliği senaryoları gerektirir. [Azure AD B2B](/azure/active-directory/b2b/) işbirliği Konuk kullanıcıları ve dış iş ortaklarının Kurumsal verilerinizin kontrolünü sürdürürken bir kuruluştan kuruluşunuzun uygulamaları ve Hizmetleri güvenli bir şekilde paylaşmanıza olanak sağlar.

## <a name="access-lifecycle"></a>Erişim yaşam döngüsü

Kuruluşların, kullanıcı kimliğinin oluşturulduğu sırada ne bir kullanıcı için başlangıçta sağlanan ötesinde erişimi yönetmek için bir işlem gerekir.  Ayrıca, büyük kuruluşlar için verimli bir şekilde geliştirin ve erişim ilkesi ve düzenli olarak denetimlerini zorlamak mümkün olması için ölçek gerekir.

![Erişim yaşam döngüsü](./media/identity-governance-overview/access-lifecycle.png)

Genellikle, BT temsilciler erişmek için iş karar mekanizmalarına onay kararları.  Ayrıca, BT kullanıcıları içerebilir.  Örneğin, Avrupa pazarlama uygulamada bir şirketin gizli müşteri verilerine erişen kullanıcılar şirketin ilkelerini bilmeniz gerekir. Konuk kullanıcıları bağımsız olarak, bunlar davet edildiniz bir kuruluşta veri işleme gereksinimleri olabilir.

Kuruluşlar otomatik hale getirmenizi teknolojileri aracılığıyla erişim yaşam döngüsü işlemi gibi [dinamik gruplar](../users-groups-roles/groups-dynamic-membership.md), kullanıcı için sağlama ile bağlı [SaaS uygulamaları](../saas-apps/tutorial-list.md) veya [SCIMiletümleşikuygulamaları](../manage-apps/use-scim-to-provision-users-and-groups.md).  Kuruluşlar da denetleyebilirsiniz hangi [konuk kullanıcıların şirket içi uygulamalara erişiminin](../b2b/hybrid-cloud-to-on-premises.md).  Bu erişim hakları can sonra yinelenen kullanarak düzenli olarak gözden geçirilmesi [Azure AD erişim gözden geçirmeleri](access-reviews-overview.md).

Bir kullanıcı, uygulamaları erişmeye çalıştığında Azure AD'ye zorlar [koşullu erişim](/azure/active-directory/conditional-access/) ilkeleri. Örneğin, koşullu erişim ilkelerini görüntüleme dahil edebilirsiniz bir [kullanım koşullarını](../conditional-access/terms-of-use.md) ve [kullanıcı sağlayarak bu koşulları kabul](../conditional-access/require-tou.md) uygulamaya erişebilmeleri için önce.

## <a name="privileged-access-lifecycle"></a>Ayrıcalıklı erişim yaşam döngüsü

Tarihsel olarak, ayrıcalıklı erişimin diğer satıcılar tarafından ayrı bir kimlik yönetimi özelliğinden olarak açıklanan. Ancak, Microsoft'ta ayrıcalıklı erişimi yöneten Kimlik Yönetimi--özellikle olası kötüye kullanım hakları, bir kuruluş için neden olabilir, bu yönetici ile ilişkili belirtilen ilişkin önemli bir parçası olduğunu düşünüyoruz. Çalışanlar, satıcılar ve üzerinde yönetici hakları olması Yükleniciler yönetilmeye gerekir.

![Ayrıcalıklı erişim yaşam döngüsü](./media/identity-governance-overview/privileged-access-lifecycle.png)

Azure AD Privileged Identity Management (PIM) uyarlanmış ek denetimler sağlar erişiminin güvenliğini sağlama kaynaklar için Azure ve diğer Microsoft Çevrimiçi Hizmetler Azure AD genelinde hakları.  Tam zamanında erişim ve uyarı verme özellikleri ek olarak, çok faktörlü kimlik doğrulaması ve koşullu erişim, Azure AD PIM tarafından sağlanan rol değişikliği kapsamlı bir yardımcı olmak için idare güvenli şirketinizin kaynaklarına (dizin, dizi denetimi sağlar. Office 365 ve Azure kaynağı rolleri). Formlarla diğer erişim gibi kuruluşların erişim gözden geçirmeleri tüm kullanıcılar için yinelenen erişim yeniden sertifikalandırılmasını yönetici rolleri yapılandırmak için kullanabilirsiniz.

## <a name="getting-started"></a>Başlarken

Kusursuz bir çözüm ya da her müşteri için öneri olsa da, aşağıdaki yapılandırmaları daha güvenli ve üretken bir iş gücü sağlamak için hangi, Microsoft'un önerdiği temel ilkeleri izleyin bir kılavuz sağlar.

- [Kimlik ve cihaz erişim yapılandırmaları](/microsoft-365/enterprise/microsoft-365-policies-configurations)
- [Ayrıcalıklı erişimin güvenliğini sağlama](../users-groups-roles/directory-admin-roles-secure.md)

Başlarken sekmesinde de göz atabilirsiniz **Kimlik Yönetimi** incelemeleri, Privileged Identity Management ve kullanım koşullarını Hak Yönetimi'ni kullanmaya başlamak için Azure portalında erişim.

![Kimlik yönetimini kullanmaya başlama](./media/identity-governance-overview/getting-started.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD hak yönetimi nedir? (Önizleme)](entitlement-management-overview.md)
- [Nelerdir Azure AD erişim gözden geçirmeleri?](access-reviews-overview.md)
- [Azure AD Privileged Identity Management nedir?](../privileged-identity-management/pim-configure.md)
- [Kullanım koşulları ile ne yapabilirim?](active-directory-tou.md)
