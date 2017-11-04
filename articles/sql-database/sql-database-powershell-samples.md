---
title: "SQL veritabanı için Azure PowerShell komut dosyası örnekleri | Microsoft Docs"
description: "Azure PowerShell komut dosyası oluşturma ve Azure SQL veritabanı sunucularının, esnek havuzlar, veritabanları ve güvenlik duvarları yönetmenize yardımcı olmak için örnekler."
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: jhubbard
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: overview-samples
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: sql-database
ms.workload: On Demand
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 1d1692cc43a7a5ec50c0689706d93a784a5fed88
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>Azure SQL veritabanı için Azure PowerShell örnekleri

Aşağıdaki tabloda, Azure SQL veritabanı için örnek Azure PowerShell betikleri bağlantılarını içerir.

| |  |
|---|---|
|**Tek bir veritabanı ve esnek havuz oluşturma**||
| [Tek bir veritabanı oluşturun ve bir güvenlik duvarı kuralı yapılandırma](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell Betiği tek bir Azure SQL veritabanı oluşturur ve bir sunucu düzeyinde güvenlik duvarı kuralı yapılandırır. |
| [Esnek havuzları oluşturma ve havuza alınmış veritabanlarını taşıma](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell Betiği Azure SQL Database esnek havuzları, oluşturur ve havuza alınmış veritabanları taşır ve performans düzeyleri değiştirir.|
|**Coğrafi çoğaltma ve yük devretme yapılandırma**||
| [Yapılandırma ve yük devretme aktif coğrafi çoğaltma kullanarak tek bir veritabanı](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell komut dosyası etkin coğrafi çoğaltma tek bir Azure SQL veritabanı için yapılandırır ve ikincil çoğaltma üzerinden başarısız. |
| [Yapılandırma ve yük devretme aktif coğrafi çoğaltma kullanarak havuza alınmış bir veritabanı](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell Betiği SQL esnek havuzu içinde bir Azure SQL veritabanı için etkin coğrafi çoğaltma yapılandırır ve ikincil çoğaltma üzerinden başarısız. |
| [Yapılandırma ve yük devretme yük devretme bir grup için tek bir veritabanına (Önizleme)](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell komut dosyasını bir Azure SQL veritabanı sunucusu örneği için bir yük devretme grubu yapılandırır, bir veritabanı yük devretme grubuna ekler ve ikincil sunucusu üzerinden başarısız |
|**Tek veritabanları ve esnek havuz ölçekleme**||
| [Tek bir veritabanı ölçekleme](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell komut dosyası izleyicileri bir Azure SQL veritabanı performans ölçümleri daha yüksek bir performans düzeyine ölçeklendirir ve performans ölçümleri birinde bir uyarı kuralı oluşturur. |
| [Ölçek bir esnek havuz](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell komut dosyası izleyicileri bir Azure SQL Database esnek havuzunu performans ölçümlerini daha yüksek bir performans düzeyine ölçeklendirir ve performans ölçümleri birinde bir uyarı kuralı oluşturur.  |
| **Denetim ve tehdit algılama** |
| [Denetim ve tehdit algılama yapılandırın](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell komut dosyasını bir Azure SQL veritabanı için Denetim ve tehdit algılama ilkeleri yapılandırır. |
| **Geri yükleme, kopyalama ve veritabanı alma**||
| [Bir veritabanını geri yükle](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell komut dosyasını bir Azure SQL veritabanı coğrafi olarak yedekli bir yedekten geri yükler ve son yedekleme silinen bir Azure SQL veritabanını geri yükler. |
| [Bir veritabanını yeni bir sunucuya kopyalayın](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell Betiği yeni bir Azure SQL Server'da mevcut bir Azure SQL veritabanının bir kopyasını oluşturur. |
| [Bir veritabanı bir bacpac Dosyadan İçeri Aktar](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell komut dosyasını bir veritabanı bir bacpac dosyasından bir Azure SQL sunucusuna aktarır. |
| **Veritabanları arasında eşitleme verileri**||
| [SQL veritabanları arasında eşitleme verileri](scripts/sql-database-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell Betiği veri birden çok Azure SQL veritabanları arasında eşitlemek için eşitleme yapılandırır. |
| [Şirket içi SQL Database ve SQL Server arasında veri eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell Betiği veri bir Azure SQL veritabanı ile bir SQL Server içi veritabanı arasında eşitlemek için eşitleme yapılandırır. |
|||
|||
