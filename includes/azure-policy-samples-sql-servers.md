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
ms.openlocfilehash: 76e161be1adca4aa2ec7b682a13b0a42e4b79412
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003910"
---
### <a name="sql-servers"></a>SQL Sunucuları

|  |  |
|---------|---------|
| [Azure Active Directory yöneticisi olmadığında denetle](../articles/governance/policy/samples/audit-no-aad-admin.md) | SQL sunucusuna atanmış bir Azure Active Directory yöneticisi olmadığında denetler. |
| [Sunucu düzeyi tehdit algılama ayarını denetle](../articles/governance/policy/samples/audit-sql-ser-threat-det-setting.md) | SQL veritabanı güvenlik uyarısı ilkeleri belirtilen duruma ayarlanmadıysa bu ilkeleri denetler. Tehdit algılamanın etkin mi devre dışı mı olduğunu gösteren bir değer belirtirsiniz.  |
| [SQL Server denetim ayarlarını denetle](../articles/governance/policy/samples/sql-server-audit.md) | Denetim ayarlarının etkin olup olmamasına göre SQL sunucusunu denetler. |
| [SQL Server Düzeyi Denetim Ayarlarını denetle](../articles/governance/policy/samples/audit-sql-ser-leve-audit-setting.md) | Bu ayarlar belirli bir ayarla eşleşmiyorsa SQL sunucusu denetim ayarlarını denetler. Denetim ayarlarının etkin veya devre dışı olması gerektiğini gösteren bir değer belirtirsiniz. |
| [SQL Server sürüm 12.0 iste](../articles/governance/policy/samples/req-sql-12.md) | SQL sunucularının sürüm 12.0’ı kullanmasını gerektirir.  |