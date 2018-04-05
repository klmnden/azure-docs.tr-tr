---
title: Ayrıcalıklı Kimlik Yönetimi (PIM) ile Azure kaynaklarına erişimi yönetme
description: Azure kaynaklarına erişmek için rol tabanlı erişim yönetimi PIM içinde kullanma hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: skwan
manager: mtillman
editor: bryanla
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: billmath
ms.openlocfilehash: 1e74579ef2f0e18f23a40dfc573177938b9b726f
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="manage-access-to-azure-resources-with-privileged-identity-management"></a>Privileged Identity Management ile Azure kaynaklarına erişimi yönetme

Ayrıcalıklı hesapların siber-saldırılara karşı korumak için ayrıcalıkları Etkilenme süresini azaltmak ve kullanımlarını raporları ve Uyarıları aracılığıyla, görünürlük artırmak için Azure Active Directory ayrıcalıklı Kimlik Yönetimi (PIM) kullanabilirsiniz. PIM olmadığından bu ayrıcalıklarını üzerinde "tam zamanında" yalnızca alma kullanıcıların sınırlayarak (JIT) veya daha sonra ayrıcalıkları iptal otomatik olarak kısaltılmış bir süre için ayrıcalıkları atayarak. 

Artık Azure rol tabanlı erişim denetimi (RBAC) PIM yönetin, denetleyin ve Azure kaynaklarına erişimi izlemek için kullanabilirsiniz. PIM yardımcı olması için yerleşik ve özel rol üyeliğini yönetebilirsiniz: 

- İsteğe bağlı, "tam zamanında" Azure kaynaklarına erişimi etkinleştirme
- Otomatik olarak atanan kullanıcılar ve gruplar için kaynak erişim süresi dolsun
- Hızlı Görevler veya çağrısı zamanlamalar için Azure kaynaklarını geçici erişim atayın
- Kaynak erişim yeni kullanıcılar veya gruplar atandığında ve uygun atamaları etkinleştirdiğinizde uyarıları alma

Daha fazla bilgi için bkz: [Overview of Role-Based erişim denetimini Azure PIM](privileged-identity-management/azure-pim-resource-rbac.md).