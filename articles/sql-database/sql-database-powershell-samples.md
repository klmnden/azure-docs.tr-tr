---
title: SQL Veritabanı için Azure PowerShell betik örnekleri | Microsoft Docs
description: Azure SQL Veritabanı sunucuları, elastik havuzlar, veritabanları ve güvenlik duvarları oluşturmanıza ve yönetmenize yardımcı olacak Azure PowerShell betik örnekleri.
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: craigg
editor: tysonn
tags: azure-service-management
ms.service: sql-database
ms.custom: overview-samples, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.date: 07/16/2018
ms.author: carlrab
ms.openlocfilehash: 81f39c2ecc014ee53dcff2fab88ff0f890e1a610
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091907"
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>Azure SQL Veritabanı için Azure PowerShell örnekleri

Aşağıdaki tablo, Azure SQL Veritabanı’na yönelik örnek Azure PowerShell betiklerinin bağlantılarını içerir.

| |  |
|---|---|
|**Tek bir veritabanı ve elastik havuz oluşturma**||
| [Tek bir veritabanı oluşturma ve güvenlik duvarı kuralını yapılandırma](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, tek bir Azure SQL veritabanı oluşturur ve sunucu düzeyinde güvenlik duvarı kuralı yapılandırır. |
| [Elastik havuzlar oluşturma ve havuza alınmış veritabanlarını taşıma](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Azure SQL Veritabanı elastik havuzları oluşturur, havuza alınmış veritabanlarını taşır ve performans düzeylerini değiştirir.|
|**Coğrafi çoğaltmayı ve yük devretmeyi yapılandırma**||
| [Etkin coğrafi çoğaltmayı kullanarak tek bir veritabanını yapılandırma ve tek bir veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği, tek bir Azure SQL veritabanı için etkin coğrafi çoğaltmayı yapılandırır ve ikincil bir çoğaltmaya yük devreder. |
| [Etkin coğrafi çoğaltmayı kullanarak havuza alınmış veritabanını yapılandırma ve havuza alınmış veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği, SQL elastik havuzundaki bir Azure SQL veritabanı için etkin coğrafi çoğaltmayı yapılandırır ve ikincil bir çoğaltmaya yük devreder. |
| [Tek bir veritabanı için bir yük devretme grubunu yapılandırma ve yük devretme grubunun yükünü devretme](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Azure SQL Veritabanı sunucu örneği için bir yük devretme grubu yapılandırır, yük devretme grubuna bir veritabanı ekler ve ikincil sunucuya yük devreder |
|**Tek bir veritabanını ve elastik havuzu ölçekleme**||
| [Tek bir veritabanını ölçekleme](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Azure SQL veritabanının performans ölçümlerini izler, veritabanını daha yüksek bir performans düzeyine ölçekler ve performans ölçümlerinden birinde bir uyarı kuralı oluşturur. |
| [Elastik havuzu ölçekleme](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Azure SQL Veritabanı elastik havuzunun performans ölçümlerini izler, veritabanını daha yüksek bir performans düzeyine ölçekler ve performans ölçümlerinden birinde bir uyarı kuralı oluşturur.  |
| **Denetim ve tehdit algılama** |
| [Denetim ve tehdit algılamayı yapılandırma](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği, Azure SQL veritabanı için denetim ve tehdit algılama ilkelerini yapılandırır. |
| **Veritabanını geri yükleme, kopyalama ve içeri aktarma**||
| [Veritabanını geri yükleme](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği, coğrafi olarak yedekli bir yedeklemeden Azure SQL veritabanını geri yükler ve silinmiş bir Azure SQL veritabanını en son yedeklemeye geri yükler. |
| [Bir veritabanını yeni bir sunucuya kopyalama](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği, yeni Azure SQL sunucusunda mevcut bir Azure SQL veritabanının kopyasını oluşturur. |
| [Bacpac dosyasından veritabanını içeri aktarma](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği bir veritabanını bacpac dosyasından Azure SQL sunucusuna içeri aktarır. |
| **Veritabanları arasında verileri eşitleme**||
| [SQL veritabanları arasında verileri eşitleme](scripts/sql-database-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Veri Eşitleme’yi birden çok Azure SQL veritabanı arasında eşitlenecek şekilde yapılandırır. |
| [SQL Veritabanı ile şirket içi SQL Server arasında verileri eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, bir Azure SQL veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme yapmak için Veri Eşitleme’yi yapılandırır. |
| [SQL Data Sync için eşitleme şemasını güncelleştirme](scripts/sql-database-sync-update-schema.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Veri Eşitleme için eşitleme şemasından öğeler ekler veya öğeleri kaldırır. |
|||
