---
title: Azure AD Privileged Identity Management Güvenlik Sihirbazı
description: Azure Active Directory Privileged Identity Management uzantısı ilk kullanışınızda bir güvenlik Sihirbazı ile sunulur. Bu makalede Sihirbazı'nı kullanmaya yönelik adımlar açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: users-groups-roles
ms.date: 02/27/2017
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 4b3856d74b1109b20a1ff9f93b76ee36b66ee312
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="using-the-security-wizard-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management Güvenlik Sihirbazı'nı kullanma 
Kuruluşunuz için Azure Privileged Identity Management (PIM) çalıştırmak için ilk kişi sizseniz, bir sihirbaz ile sunulur. Sihirbaz, ayrıcalıklı kimliklerinizi ve PIM bu riskleri azaltmak için nasıl kullanılacağını güvenlik risklerini anlamanıza yardımcı olur. Daha sonra yapmayı tercih ediyorsanız Sihirbazı'nda, var olan rol atamalarını herhangi bir değişiklik yapmak gerekmez.

## <a name="what-to-expect"></a>Sizi neler bekliyor
PIM kullanarak kuruluşunuz başlamadan önce tüm rol atamalarını kalıcı: şu anda ayrıcalıklarını gerekmez olsa bile her zaman bu rollerde kullanıcılardır.  Sihirbazın ilk adımı, yüksek ayrıcalıklı rolleri listesini ve kaç kullanıcı şu anda bu rollerden gösterir. Varsa kullanıcılar hakkında daha fazla bilgi edinmek için belirli bir rol için ayrıntıya inebilir veya daha fazlası tanımıyorsanız.

Sihirbazın ikinci yönetici rol atamalarını değiştirme olanağı sağlar.  

> [!WARNING]
> En az bir genel yönetici ve bir kurumsal hesap (Microsoft hesabı değil) ile birden fazla ayrıcalıklı Rol Yöneticisi olması da önemlidir. Kuruluş, tek bir ayrıcalıklı rol yöneticisi ise, bu hesabı silinirse PIM yönetebilmek için olmaz.
> Ayrıca, rol atamaları bir kullanıcının bir Microsoft hesabı (Skype ve Outlook.com gibi Microsoft hizmetlerinde oturum açarken kullandıkları hesabı) varsa kalıcı tutun. Bu rol için etkinleştirme için MFA gerektirecek şekilde planlıyorsanız, bu kullanıcının kilitlenir.
> 
> 

Değişiklikleri yaptıktan sonra sihirbazın artık görünecektir. Siz veya başka bir ayrıcalıklı Rol Yöneticisi PIM, bir sonraki çalıştırmanızda PIM Pano görürsünüz.  

* Eklemek veya kullanıcıların rollerini kaldırın veya atamaları kalıcı için uygun değiştirmek istiyorsanız, hakkında daha fazla okuma [kullanıcı rolü ekleme veya kaldırma nasıl](active-directory-privileged-identity-management-how-to-add-role-to-user.md).
* Daha fazla kullanıcı PIM yönetmek için erişim vermek istiyorsanız, hakkında daha fazla okuma [içinde PIM yönetmek için erişim vermek nasıl](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

