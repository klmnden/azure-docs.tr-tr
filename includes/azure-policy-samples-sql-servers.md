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
ms.openlocfilehash: 298b92205e73b3623db02fb1dd803edac8379700
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664702"
---
### <a name="sql-servers"></a>SQL Sunucuları

|  |  |
|---------|---------|
| [Azure Active Directory yöneticisi olmadığında denetle](../articles/azure-policy/scripts/audit-no-aad-admin.md) | SQL sunucusuna atanmış bir Azure Active Directory yöneticisi olmadığında denetler. |
| [Sunucu düzeyi tehdit algılama ayarını denetle](../articles/azure-policy/scripts/audit-sql-ser-threat-det-setting.md) | SQL veritabanı güvenlik uyarısı ilkeleri belirtilen duruma ayarlanmadıysa bu ilkeleri denetler. Tehdit algılamanın etkin mi devre dışı mı olduğunu gösteren bir değer belirtirsiniz.  |
| [SQL Server denetim ayarlarını denetle](../articles/azure-policy/scripts/sql-server-audit.md) | Denetim ayarlarının etkin olup olmamasına göre SQL sunucusunu denetler. |
| [SQL Server Düzeyi Denetim Ayarlarını denetle](../articles/azure-policy/scripts/audit-sql-ser-leve-audit-setting.md) | Bu ayarlar belirli bir ayarla eşleşmiyorsa SQL sunucusu denetim ayarlarını denetler. Denetim ayarlarının etkin veya devre dışı olması gerektiğini gösteren bir değer belirtirsiniz. |
| [SQL Server sürüm 12.0 iste](../articles/azure-policy/scripts/req-sql-12.md) | SQL sunucularının sürüm 12.0’ı kullanmasını gerektirir.  |