---
title: "Azure Active Directory ile Azure kaynaklarına erişimi yönetin"
description: "Azure Active Directory farklı özelliklerini kullanarak Azure kaynaklarına erişimi yönetme yöntemleri hakkında bilgi edinin."
services: active-directory
documentationcenter: 
author: skwan
manager: mtillman
editor: bryanla
ms.assetid: f66abf54-3809-436c-92b6-018e1179780e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/05/2017
ms.author: skwan
ms.openlocfilehash: eee4353c183aeec19f72f8e1dec6c20b6c5bb226
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="manage-access-to-azure-resources-with-azure-active-directory"></a>Azure Active Directory ile Azure kaynaklarına erişimi yönetin

Bulut kaynağı için kimlik ve erişim yönetimi bulut kullanarak her kuruluş için kritik bir işlevdir. Azure Active Directory (Azure AD), Microsoft Azure için kimlik ve erişim sistemidir.  

Destekleyici keşfetme önce Özelliği Azure ad alanları, aşağıdaki video çıkış denetleyin: "SSO, rol tabanlı erişim denetimi ve koşullu kullanarak Azure bulut erişimi kilitleme." İçinde hakkında bilgi edinin:

- Çoklu oturum açma Azure portalına şirket içi Active Directory ile yapılandırmak için en iyi yöntemler.
- Kaynakları abonelikler için ayrıntılı erişim denetimi için Azure RBAC kullanarak.
- Azure AD koşullu erişim kullanarak güçlü kimlik doğrulama kural zorlama.
- Yönetilen hizmet burada Azure kaynakları otomatik olarak Azure hizmetlerine doğrulayabilir ve geliştiriciler API anahtar veya gizli işlemek için gerek yoktur kimliği kavramı.

> [!VIDEO https://www.youtube.com/embed/FKBoWWKRnvI]

## <a name="feature-areas"></a>Özellik alanları
Azure AD Azure kaynaklarına erişimi yönetmek için aşağıdaki özellikleri sağlar:

|||
|---|---|
| [Azure AD kiracılarıyla ve abonelikleri arasındaki ilişki](active-directory-understanding-resource-access.md) | Hakkında bilgi edinin Azure AD kullanıcıların ve grupların Azure aboneliği için güvenilen bir kaynak değil. |
| [Rol tabanlı erişim denetimi (RBAC)](role-based-access-control-what-is.md) | Kullanıcıların, grupların veya hizmet asıl adı atanan rolleri aracılığıyla ayrıntılı erişim yönetimi sunar. |
| [RBAC ile ayrıcalıklı Kimlik Yönetimi](pim-azure-resource.md) | Denetim yüksek ayrıcalıklı rolleri tam zamanında atayarak ayrıcalıklı erişimi. |
| [Azure Yönetim için koşullu erişim](conditional-access-azure-management.md) | İzin vermek veya Azure yönetim Uç noktalara erişimi engellemek için koşullu erişim ilkeleri ayarlayabilirsiniz. |
| [Yönetilen hizmet kimliği (MSI)](msi-overview.md) | Böylece, kimlik bilgileri, kodunuzda put gerekmez kendi kimliğini sanal makineleri gibi Azure kaynaklarını verin. |

 