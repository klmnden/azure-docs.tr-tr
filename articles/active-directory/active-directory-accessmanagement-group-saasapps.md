---
title: "SaaS uygulamalarına erişimi yönetmek için bir grup kullanma | Microsoft Docs"
description: "Azure Active Directory Premium veya Basic grupları Azure Active Directory ile tümleşik SaaS uygulamalarına erişimi atamak için nasıl kullanılacağını."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: ab8dee63-bedc-46ca-8852-234f5c16ae98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro
ms.openlocfilehash: 818f4b515926c35078b3118978f3accbf3bbb65b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>SaaS uygulamalarına erişimi yönetmek için grup kullanma
Bir Azure AD Premium veya Azure AD temel lisansı ile Azure Active Directory (Azure AD) kullanarak Azure AD ile tümleşik bir SaaS uygulaması erişim atamak için grupları kullanabilirsiniz. Örneğin, beş farklı SaaS uygulamaları kullanmak pazarlama departmanı için erişim atamak istiyorsanız, pazarlama departmanındaki kullanıcılar içeren bir grup oluşturabilir ve tarafından gerekli olan bu beş SaaS uygulamaları için o grubun atayın Pazarlama departmanı. Bu şekilde, tek bir yerde pazarlama departmanı üyeliğini yöneterek zaman tasarrufu sağlar. Kullanıcılar daha sonra uygulamaya pazarlama grubunun bir üyesi olarak eklenir ve pazarlama gruptan kaldırdığınızda atamalarını uygulamasından kaldırmış atanır. Bu özellik, Azure AD uygulama galerisinde içinde ekleyebilirsiniz uygulamaları yüzlerce kullanılabilir.

> [!IMPORTANT]
> Yalnızca bir Azure AD Premium deneme sürümünü başlatın veya Azure AD Premium veya Azure AD temel lisans satın sonra bu özelliği kullanabilirsiniz.
> Şu anda uygulamalara grup tabanlı atama yapmak için iç içe geçmiş grup üyelikleri desteklenmiyor.

**Bir SaaS uygulaması için bir kullanıcı veya grup için erişim atamak için**

1. İçinde [Azure AD Yönetim Merkezi](https://aad.portal.azure.com)seçin **kurumsal uygulamalar**.
2. Açmak için uygulama Galeriden eklediğiniz uygulama seçin.
3. Seçin **kullanıcılar ve gruplar**ve ardından **Kullanıcı Ekle**.
4. Üzerinde **eklemek atama**seçin **kullanıcılar ve gruplar** açmak için **kullanıcılar ve gruplar** seçim listesi.
6. Sayıda grubu seçin veya kullanıcıların istediğiniz gibi ardından tıklayın veya dokunun **seçin** bunlara eklemek için **eklemek atama** listesi. Ayrıca, bir kullanıcı için bu aşamada rol atayabilirsiniz.
7. Seçin **atamak** kullanıcılar veya gruplar için seçilen Kurumsal Uygulama atama.

### <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Azure Active Directory nedir?](active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
