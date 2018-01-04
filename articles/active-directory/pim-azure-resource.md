---
title: "Ayrıcalıklı Kimlik Yönetimi (PIM) ile Azure kaynaklarına erişimi yönetme"
description: "Azure kaynaklarına erişmek için rol tabanlı erişim yönetimi PIM içinde kullanma hakkında bilgi edinin."
services: active-directory
documentationcenter: 
author: skwan
manager: mtillman
editor: bryanla
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/22/2017
ms.author: skwan
ms.openlocfilehash: 1f31d8b76351ac8871f8a5b03d513f7b6704c709
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="manage-access-to-azure-resources-with-privileged-identity-management-preview"></a>Privileged Identity Management (Önizleme) ile Azure kaynaklarına erişimi yönetme

Ayrıcalıklı hesapların siber-saldırılara karşı korumak için ayrıcalıkları Etkilenme süresini azaltmak ve kullanımlarını raporları ve Uyarıları aracılığıyla, görünürlük artırmak için Azure Active Directory ayrıcalıklı Kimlik Yönetimi (PIM) kullanabilirsiniz. PIM olmadığından bu ayrıcalıklarını üzerinde "tam zamanında" yalnızca alma kullanıcıların sınırlayarak (JIT) veya daha sonra ayrıcalıkları iptal otomatik olarak kısaltılmış bir süre için ayrıcalıkları atayarak. 

Artık Azure rol tabanlı erişim denetimi (RBAC) PIM yönetin, denetleyin ve Azure kaynaklarına erişimi izlemek için kullanabilirsiniz. PIM yardımcı olması için yerleşik ve özel rol üyeliğini yönetebilirsiniz: 

- İsteğe bağlı, "tam zamanında" Azure kaynaklarına erişimi etkinleştirme
- Otomatik olarak atanan kullanıcılar ve gruplar için kaynak erişim süresi dolsun
- Hızlı Görevler veya çağrısı zamanlamalar için Azure kaynaklarını geçici erişim atayın
- Kaynak erişim yeni kullanıcılar veya gruplar atandığında ve uygun atamaları etkinleştirdiğinizde uyarıları alma

Daha fazla bilgi için bkz: [Overview of Role-Based erişim denetimini Azure PIM](privileged-identity-management/azure-pim-resource-rbac.md).