---
title: include dosyası
description: include dosyası
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 50a6388a95bce14bc9af7e0c9a5387e148f6fa73
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664712"
---
### <a name="sql-databases"></a>SQL Veritabanları

|  |  |
|---------|---------|
| [İzin verilen SQL DB SKU’ları](../articles/azure-policy/scripts/allowed-sql-db-skus.md) | SQL veritabanlarının onaylı bir SKU kullanmasını gerektirir. İzin verilen bir SKU kimlikleri dizisi veya izin verilen bir SKU adları dizisi belirtirsiniz. |
| [DB düzeyi tehdit algılama ayarını denetle](../articles/azure-policy/scripts/audit-db-threat-det-setting.md) | SQL veritabanı güvenlik uyarısı ilkeleri belirtilen duruma ayarlanmadıysa bu ilkeleri denetler. Tehdit algılamanın etkin mi devre dışı mı olduğunu gösteren bir değer belirtirsiniz.  |
| [SQL Veritabanı şifrelemesini denetleme](../articles/azure-policy/scripts/sql-database-encryption-audit.md) | SQL veritabanında saydam veri şifreleme etkin değilse denetler. |
| [SQL DB Düzeyinde Denetim Ayarını denetle](../articles/azure-policy/scripts/audit-sql-db-audit-setting.md) | Bu ayarlar belirli bir ayarla eşleşmiyorsa SQL veritabanı denetim ayarlarını denetler. Denetim ayarlarının etkin veya devre dışı olması gerektiğini gösteren bir değer belirtirsiniz.  |
| [Saydam veri şifreleme durumunu denetle](../articles/azure-policy/scripts/audit-trans-data-enc-status.md) | Etkin değilse SQL veritabanında saydam veri şifrelemeyi denetler.  |