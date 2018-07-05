---
title: Privileged Identity Management (PIM) ile Azure kaynaklarına erişimi yönetme
description: Privileged Identity Management (PIM) ve rol tabanlı erişim denetimi (RBAC) kullanarak Azure kaynaklarına erişimi yönetme hakkında bilgi edinin.
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
ms.openlocfilehash: 141cba29f5027ce092775d97c1abe9ecf11badf5
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436054"
---
# <a name="manage-access-to-azure-resources-with-privileged-identity-management"></a>Privileged Identity Management ile Azure kaynaklarına erişimi yönetme

Kötü amaçlı siber saldırılara karşı ayrıcalıklı hesapları korumak için raporlar ve uyarılar aracılığıyla kullanımlarının görünürlüğünüzü artırın ve ayrıcalıkların tehditlere maruz kalabileceği süreyi azaltmak için Azure Active Directory Privileged Identity Management (PIM) kullanabilirsiniz. PIM bunu yapar yalnızca kendi ayrıcalıklarını "tam zamanında" alma kullanıcıların sınırlayarak (JIT) ya da kısaltılmış bir süre sonra ayrıcalıkları otomatik olarak iptal edilir için ayrıcalıkları atayarak. 

Artık Azure rol tabanlı erişim denetimi (RBAC) ile PIM yönetebilir, denetleyebilir ve Azure kaynaklarına erişimi izlemek için kullanabilirsiniz. PIM yardımcı olması için yerleşik ve özel rol üyeliklerini yönetebilir: 

- İsteğe bağlı, "yalnızca zamanında" Azure kaynaklarına erişimi etkinleştirme
- Otomatik olarak atanan kullanıcılar ve gruplar için kaynak erişim süre sonu
- Hızlı Görevler veya nöbet zamanlamalar için Azure kaynaklarına geçici erişim atama
- Yeni kullanıcılar veya gruplar kaynak erişimi atandığında ve bunlar uygun atamalar'ı etkinleştirdiğinizde uyarılar alın

Daha fazla bilgi için [Azure PIM rol tabanlı erişim denetimine genel bakış](../active-directory/privileged-identity-management/azure-pim-resource-rbac.md).