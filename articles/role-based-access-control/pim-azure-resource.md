---
title: Ayrıcalıklı Kimlik Yönetimi (PIM) ile Azure kaynaklarına erişimi yönetme
description: Ayrıcalıklı Kimlik Yönetimi (PIM) ve rol tabanlı erişim denetimi (RBAC) kullanarak Azure kaynaklarına erişimi yönetme hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 838c889f2dc099b4a4c5d84521871c64eb989163
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36293759"
---
# <a name="manage-access-to-azure-resources-with-privileged-identity-management"></a>Privileged Identity Management ile Azure kaynaklarına erişimi yönetme

Ayrıcalıklı hesapların siber-saldırılara karşı korumak için ayrıcalıkları Etkilenme süresini azaltmak ve kullanımlarını raporları ve Uyarıları aracılığıyla, görünürlük artırmak için Azure Active Directory ayrıcalıklı Kimlik Yönetimi (PIM) kullanabilirsiniz. PIM olmadığından bu ayrıcalıklarını üzerinde "tam zamanında" yalnızca alma kullanıcıların sınırlayarak (JIT) veya daha sonra ayrıcalıkları iptal otomatik olarak kısaltılmış bir süre için ayrıcalıkları atayarak. 

Artık Azure rol tabanlı erişim denetimi (RBAC) PIM yönetin, denetleyin ve Azure kaynaklarına erişimi izlemek için kullanabilirsiniz. PIM yardımcı olması için yerleşik ve özel rol üyeliğini yönetebilirsiniz: 

- İsteğe bağlı, "tam zamanında" Azure kaynaklarına erişimi etkinleştirme
- Otomatik olarak atanan kullanıcılar ve gruplar için kaynak erişim süresi dolsun
- Hızlı Görevler veya çağrısı zamanlamalar için Azure kaynaklarını geçici erişim atayın
- Kaynak erişim yeni kullanıcılar veya gruplar atandığında ve uygun atamaları etkinleştirdiğinizde uyarıları alma

Daha fazla bilgi için bkz: [Azure PIM rol tabanlı erişim denetimine genel bakış](../active-directory/privileged-identity-management/azure-pim-resource-rbac.md).