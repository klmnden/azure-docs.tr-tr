---
title: SaaS uygulamalarına erişimi yönetmek için bir grup kullanmanızı | Microsoft Docs
description: Azure Active Directory ile tümleştirilmiş SaaS uygulamalarına erişimi atamak için Azure Active Directory Premium veya Basic grupları kullanma
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 03/14/2017
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 45882e4d107ffd9ece2bfa6caac43b3bb2f60add
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720251"
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>SaaS uygulamalarına erişimi yönetmek için grup kullanma
Azure Active Directory (Azure AD) bir Azure AD Premium veya Azure AD temel lisansı ile birlikte kullanarak, Azure AD ile tümleştirilmiş bir SaaS uygulamasına erişim atamak için grupları kullanabilirsiniz. Örneğin, beş farklı SaaS uygulamasında kullanmak pazarlama bölümü için erişimi atamak istiyorsanız, pazarlama departmanındaki kullanıcılar içeren bir grup oluşturabilir ve ardından tarafından gerekli olan bu beş SaaS uygulamaları için bu gruba atayın Pazarlama departmanındaki. Bu şekilde üyelik pazarlama departmanı tek bir yerden yöneterek zaman kazanabilirsiniz. Pazarlama grubunun bir üyesi eklenir ve pazarlama gruptan kaldırdığınızda atamaları uygulamadan kaldırdıysanız, kullanıcılar uygulamaya sonra atanır. Bu özellik, Azure AD uygulama Galerisi içinde ekleyebilirsiniz uygulamaları yüzlerce ile kullanılabilir.

> [!IMPORTANT]
> Yalnızca bir Azure AD Premium denemesi başlatın veya Azure AD Premium veya Azure AD temel lisansı satın sonra bu özelliği kullanabilirsiniz. Grup tabanlı atama, yalnızca güvenlik grupları için desteklenir. Şu anda uygulamalara grup tabanlı atama yapmak için iç içe geçmiş grup üyelikleri desteklenmiyor.

**Bir SaaS uygulamasına erişimi bir kullanıcı veya grup için atama**

1. İçinde [Azure AD yönetim merkezini](https://aad.portal.azure.com)seçin **kurumsal uygulamalar**.
2. Açmak için uygulama galerisinden eklediğiniz uygulama seçin.
3. Seçin **kullanıcılar ve gruplar**ve ardından **Kullanıcı Ekle**.
4. Üzerinde **atama Ekle**seçin **kullanıcılar ve gruplar** açmak için **kullanıcılar ve gruplar** seçim listesi.
6. Sayıda grubu seçin veya kullanıcıların, istediğiniz tıklayın veya dokunun **seçin** ekleneceği **atama Ekle** listesi. Ayrıca bu aşamada bir kullanıcıya bir rol atayabilirsiniz.
7. Seçin **atama** kullanıcılar veya gruplar için seçilen Kurumsal uygulama atamak için.

### <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md)
* [Azure Active Directory'de uygulama yönetimi](../manage-apps/what-is-application-management.md)
* [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](groups-settings-cmdlets.md)
* [Azure Active Directory nedir?](../fundamentals/active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../connect/active-directory-aadconnect.md)
