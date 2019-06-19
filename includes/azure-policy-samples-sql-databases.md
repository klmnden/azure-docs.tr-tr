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
ms.openlocfilehash: 7bf885f2ab81e5d86878d2b0b53f4e98b8aedd9a
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188533"
---
### <a name="sql-databases"></a>SQL Veritabanları

|  |  |
|---------|---------|
| [İzin verilen SQL DB SKU’ları](../articles/governance/policy/samples/allowed-sql-db-skus.md) | SQL veritabanlarının onaylı bir SKU kullanmasını gerektirir. İzin verilen bir SKU kimlikleri dizisi veya izin verilen bir SKU adları dizisi belirtirsiniz. |
| [DB düzeyi tehdit algılama ayarını denetle](../articles/governance/policy/samples/audit-db-threat-detection-setting.md) | SQL veritabanı güvenlik uyarısı ilkeleri belirtilen duruma ayarlanmadıysa bu ilkeleri denetler. Tehdit algılamanın etkin mi devre dışı mı olduğunu gösteren bir değer belirtirsiniz.  |
| [SQL Veritabanı şifrelemesini denetleme](../articles/governance/policy/samples/sql-database-encryption-audit.md) | SQL veritabanında saydam veri şifreleme etkin değilse denetler. |
| [SQL DB Düzeyinde Denetim Ayarını denetle](../articles/governance/policy/samples/audit-sql-db-audit-setting.md) | Bu ayarlar belirli bir ayarla eşleşmiyorsa SQL veritabanı denetim ayarlarını denetler. Denetim ayarlarının etkin veya devre dışı olması gerektiğini gösteren bir değer belirtirsiniz.  |