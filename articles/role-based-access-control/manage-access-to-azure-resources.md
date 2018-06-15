---
title: Azure Active Directory ile Azure kaynaklarına erişimi yönetin
description: Azure Active Directory farklı özelliklerini kullanarak Azure kaynaklarına erişimi yönetme yöntemleri hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: f66abf54-3809-436c-92b6-018e1179780e
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/05/2017
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 55bd18b127fea8a06e58f14873a584f0b5849906
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34203246"
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
| [Azure AD kiracılarıyla ve abonelikleri arasındaki ilişki](rbac-and-directory-admin-roles.md) | Hakkında bilgi edinin Azure AD kullanıcıların ve grupların Azure aboneliği için güvenilen bir kaynak değil. |
| [Rol tabanlı erişim denetimi (RBAC)](overview.md) | Kullanıcıların, grupların veya hizmet asıl adı atanan rolleri aracılığıyla ayrıntılı erişim yönetimi sunar. |
| [RBAC ile ayrıcalıklı Kimlik Yönetimi](pim-azure-resource.md) | Denetim yüksek ayrıcalıklı rolleri tam zamanında atayarak ayrıcalıklı erişimi. |
| [Azure Yönetim için koşullu erişim](conditional-access-azure-management.md) | İzin vermek veya Azure yönetim Uç noktalara erişimi engellemek için koşullu erişim ilkeleri ayarlayabilirsiniz. |
| [Yönetilen hizmet kimliği (MSI)](../active-directory/pp/msi-overview.md) | Böylece, kimlik bilgileri, kodunuzda put gerekmez kendi kimliğini sanal makineleri gibi Azure kaynaklarını verin. |

 