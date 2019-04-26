---
title: Azure AD Privileged Identity Management (PIM) ile Azure kaynaklarına erişimi yönetme
description: Azure Active Directory Privileged Identity Management (PIM) ve rol tabanlı erişim denetimi (RBAC) kullanarak Azure kaynaklarına erişimi yönetme hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 757068034868744b408c9402b521a0e4c73950f7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60344623"
---
# <a name="manage-access-to-azure-resources-with-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management ile Azure kaynaklarına erişimi yönetme

Kötü amaçlı siber saldırılara karşı ayrıcalıklı hesapları korumak için raporlar ve uyarılar aracılığıyla kullanımlarının görünürlüğünüzü artırın ve ayrıcalıkların tehditlere maruz kalabileceği süreyi azaltmak için Azure Active Directory Privileged Identity Management (PIM) kullanabilirsiniz. PIM bunu yapar yalnızca kendi ayrıcalıklarını "tam zamanında" alma kullanıcıların sınırlayarak (JIT) ya da kısaltılmış bir süre sonra ayrıcalıkları otomatik olarak iptal edilir için ayrıcalıkları atayarak. 

Artık Azure rol tabanlı erişim denetimi (RBAC) ile PIM yönetebilir, denetleyebilir ve Azure kaynaklarına erişimi izlemek için kullanabilirsiniz. PIM yardımcı olması için yerleşik ve özel rol üyeliklerini yönetebilir: 

- İsteğe bağlı, "yalnızca zamanında" Azure kaynaklarına erişimi etkinleştirme
- Otomatik olarak atanan kullanıcılar ve gruplar için kaynak erişim süre sonu
- Hızlı Görevler veya nöbet zamanlamalar için Azure kaynaklarına geçici erişim atama
- Yeni kullanıcılar veya gruplar kaynak erişimi atandığında ve bunlar uygun atamalar'ı etkinleştirdiğinizde uyarılar alın

Daha fazla bilgi için [Azure AD Privileged Identity Management nedir?](../active-directory/privileged-identity-management/pim-configure.md).