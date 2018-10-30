---
title: include dosyası
description: include dosyası
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 10/29/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 314f29a1135e355597e890b72ef3c0b7372194e6
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227303"
---
### <a name="sql-databases"></a>SQL Veritabanları

|  |  |
|---------|---------|
| [İzin verilen SQL DB SKU’ları](../articles/governance/policy/samples/allowed-sql-db-skus.md) | SQL veritabanlarının onaylı bir SKU kullanmasını gerektirir. İzin verilen bir SKU kimlikleri dizisi veya izin verilen bir SKU adları dizisi belirtirsiniz. |
| [DB düzeyi tehdit algılama ayarını denetle](../articles/governance/policy/samples/audit-db-threat-det-setting.md) | SQL veritabanı güvenlik uyarısı ilkeleri belirtilen duruma ayarlanmadıysa bu ilkeleri denetler. Tehdit algılamanın etkin mi devre dışı mı olduğunu gösteren bir değer belirtirsiniz.  |
| [SQL Veritabanı şifrelemesini denetleme](../articles/governance/policy/samples/sql-database-encryption-audit.md) | SQL veritabanında saydam veri şifreleme etkin değilse denetler. |
| [SQL DB Düzeyinde Denetim Ayarını denetle](../articles/governance/policy/samples/audit-sql-db-audit-setting.md) | Bu ayarlar belirli bir ayarla eşleşmiyorsa SQL veritabanı denetim ayarlarını denetler. Denetim ayarlarının etkin veya devre dışı olması gerektiğini gösteren bir değer belirtirsiniz.  |