---
author: MightyPen
ms.service: sql-database
ms.topic: include
ms.date: 01/28/2019
ms.author: genemi
ms.openlocfilehash: edeaa932abe4d1882f957d0ed26aaddd97dabe3f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55198070"
---
<!-- sql-database-connect-query-prerequisites-create-db-includes.md -->

- Azure SQL veritabanı yerleştirildiği [mantıksal sunucu](https://docs.microsoft.com/azure/sql-database/sql-database-single-index) veya [yönetilen örneği](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index). Bir veritabanı oluşturmak için şu tekniklerden birini kullanabilirsiniz:

| Mantıksal sunucu | Yönetilen Örnek |
| --- | --- |
| [Portal](../articles/sql-database/sql-database-get-started-portal.md) | [Portal](../articles/sql-database/sql-database-managed-instance-get-started.md) |
| [CLI](../articles/sql-database/sql-database-get-started-cli.md) | |
| [PowerShell](../articles/sql-database/sql-database-get-started-powershell.md) | |

- **Yalnızca mantıksal sunucu** -bir yapılandırılmış sunucu düzeyinde güvenlik duvarı kuralı, mantıksal sunucunuza bağlamanızı sağlar. Daha fazla bilgi için [sunucu düzeyinde güvenlik duvarı kuralı oluşturma](../articles/sql-database/sql-database-get-started-portal-firewall.md).
- **Yönetilen örneği yalnızca** -yönetilen örnek erişiyor bilgisayardan bağlantısı yapılandırılmış. Aşağıdaki seçenekleri kullanabilirsiniz:
  - [Azure sanal makine](../articles/sql-database/sql-database-managed-instance-configure-vm.md) örneğine erişmek yönetilen örneği ile aynı Azure vnet'teki.
  - [Noktadan siteye bağlantı](../articles/sql-database/sql-database-managed-instance-configure-p2s.md) bilgisayarınızda ağınızdaki diğer SQL Server yönetilen örneği nereye yerleştirilir Vnet'e bilgisayarınızı katılın ve yönetilen örneği'ni kullanın olanak tanıyacaktır.
