---
title: include dosyası
description: include dosyası
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: c8453a2ec00a2fca107f85a23a8af1e6313a70b6
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188532"
---
### <a name="sql-servers"></a>SQL Sunucuları

|  |  |
|---------|---------|
| [Azure Active Directory yöneticisi olmadığında denetle](../articles/governance/policy/samples/audit-no-aad-admin.md) | SQL sunucusuna atanmış bir Azure Active Directory yöneticisi olmadığında denetler. |
| [Sunucu düzeyi tehdit algılama ayarını denetle](../articles/governance/policy/samples/audit-sql-server-threat-detection-setting.md) | SQL veritabanı güvenlik uyarısı ilkeleri belirtilen duruma ayarlanmadıysa bu ilkeleri denetler. Tehdit algılamanın etkin mi devre dışı mı olduğunu gösteren bir değer belirtirsiniz.  |
| [SQL Server denetim ayarlarını denetle](../articles/governance/policy/samples/sql-server-audit.md) | Denetim ayarlarının etkin olup olmamasına göre SQL sunucusunu denetler. |
| [SQL Server Düzeyi Denetim Ayarlarını denetle](../articles/governance/policy/samples/audit-sql-server-audit-setting.md) | Bu ayarlar belirli bir ayarla eşleşmiyorsa SQL sunucusu denetim ayarlarını denetler. Denetim ayarlarının etkin veya devre dışı olması gerektiğini gösteren bir değer belirtirsiniz. |
| [SQL Server sürüm 12.0 iste](../articles/governance/policy/samples/require-sql-12.md) | SQL sunucularının sürüm 12.0’ı kullanmasını gerektirir.  |