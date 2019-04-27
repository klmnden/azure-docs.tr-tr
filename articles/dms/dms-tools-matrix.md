---
title: Veri geçiş hizmeti ve araçları matris - Azure | Microsoft Docs
description: Veritabanlarını geçirmek ve çeşitli aşamaları geçiş işlemini desteklemek için kullanılabilen araçları ve Hizmetleri hakkında bilgi edinin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/12/2019
ms.openlocfilehash: 3b3bbe45c4850d1bb37a4d991e323d5f6d9a8a0a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60532409"
---
# <a name="services-and-tools-available-for-data-migration-scenarios"></a>Hizmetleri ve veri geçişi senaryoları için kullanılabilen Araçlar

Bu makalede, Microsoft ve üçüncü taraf hizmetleri ve araçları, çeşitli veritabanı ve veri geçişi senaryoları ve özel görevleri ile yardımcı olmak kullanılabilir bir matrisi verilmektedir.

Aşağıdaki tablolarda, hizmet ve veri geçişi için başarıyla planlamak ve kendi çeşitli aşamaları tamamlamak için kullanabileceğiniz araçları belirleyin.

> [!NOTE]
> Aşağıdaki tablolarda, üçüncü taraf araçları yıldız işareti (*) ile işaretlenen öğeleri temsil eder.

## <a name="business-justification-phase"></a>İş kolu düzeltme aşaması

| **Kaynak** | **Hedef** | **Bul /**<br/>**Envanteri** | **Hedef ve SKU**<br/>**Öneri** | **TCO/ROI ve**<br/>**İş durumu** |
| --- | --- | --- | --- | --- |
| SQL Server | Azure SQL DB | [MAP Araç Kiti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [TCO hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| SQL Server | Azure SQL DB mı | [MAP Araç Kiti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [TCO hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| SQL Server | Azure SQL VM | [MAP Araç Kiti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [TCO hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| SQL Server | SQL DW |  |  | [TCO hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| RDS SQL | Azure SQL DB, mı, VM |  | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [TCO hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| Oracle | Azure SQL DB, mı, VM | [MAP Araç Kiti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) | [MigVisor *](https://www.migvisor.com/) |  |
| Oracle | SQL DW | [MAP Araç Kiti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) |  |  |
| Oracle | PostgreSQL için Azure DB |  |  |  |
| MongoDB | Cosmos DB | [Cloudamize *](https://www.cloudamize.com/) | [Cloudamize *](https://www.cloudamize.com/) |  |
| Cassandra | Cosmos DB |  |  |  |
| MySQL | Azure SQL DB, mı, VM | [Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) | [Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/) | [TCO hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| MySQL | MySQL için Azure DB | [Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) |  | [TCO hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| RDS MySQL | MySQL için Azure DB |  |  | [TCO hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| PostgreSQL | PostgreSQL için Azure DB | [Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) |  | [TCO hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| RDS PostgreSQL | PostgreSQL için Azure DB |  |  | [TCO hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| DB2 | Azure SQL DB, mı, VM |  |  |  |
| Access | Azure SQL DB, mı, VM |  |  |  |
| Sybase | Azure SQL DB, mı, VM |  |  |  |
| | | | | |

## <a name="pre-migration-phase"></a>Geçiş öncesi aşaması

| **Kaynak** | **Hedef** | **Uygulama veri erişimi**<br/>**Katman değerlendirmesi** | **Veritabanı**<br/>**Değerlendirme** | **Performans**<br/>**Değerlendirme** |
| --- | --- | --- | --- | --- |
| SQL Server | Azure SQL DB |  | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [KULLANI](https://www.microsoft.com/download/details.aspx?id=54090)<br/>[Cloudamize *](https://www.cloudamize.com/) |
| SQL Server | Azure SQL DB mı |  | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [KULLANI](https://www.microsoft.com/download/details.aspx?id=54090)<br/>[Cloudamize *](https://www.cloudamize.com/) |
| SQL Server | Azure SQL VM |  | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [KULLANI](https://www.microsoft.com/download/details.aspx?id=54090)<br/>[Cloudamize *](https://www.cloudamize.com/) |
| SQL Server | SQL DW |  |  |  |
| RDS SQL | Azure SQL DB, mı, VM |  | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [KULLANI](https://www.microsoft.com/download/details.aspx?id=54090) |
| Oracle | Azure SQL DB, mı, VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [Simora *](http://www.simora.co.uk/) |
| Oracle | SQL DW |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [Simora *](http://www.simora.co.uk/) |
| Oracle | PostgreSQL için Azure DB |  |  |  |
| MongoDB | Cosmos DB |  | [Cloudamize *](https://www.cloudamize.com/) | [Cloudamize *](https://www.cloudamize.com/) |
| Cassandra | Cosmos DB |  |  |  |
| MySQL | Azure SQL DB, mı, VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/) |  |
| MySQL | MySQL için Azure DB |  |  |  |
| RDS MySQL | MySQL için Azure DB |  |  |  |
| PostgreSQL | PostgreSQL için Azure DB |  |  |  |
| RDS PostgreSQL | PostgreSQL için Azure DB |  |  |  |
| DB2 | Azure SQL DB, mı, VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Access | Azure SQL DB, mı, VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Sybase | Azure SQL DB, mı, VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| | | | | |

## <a name="migration-phase"></a>Geçiş aşaması

| **Kaynak** | **Hedef** | **Şema** | **Veriler**<br/>**(Çevrimdışı)** | **Veriler**<br/>**(Online)** |
| --- | --- | --- | --- | --- |
| SQL Server | Azure SQL DB | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| SQL Server | Azure SQL DB mı | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| SQL Server | Azure SQL VM | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| SQL Server | SQL DW |  |  |  |
| RDS SQL | Azure SQL DB, mı, VM | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [DMS](https://azure.microsoft.com/services/database-migration/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Oracle | Azure SQL DB, mı, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[SharePlex *](https://www.quest.com/products/shareplex/) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[SharePlex *](https://www.quest.com/products/shareplex/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[SharePlex *](https://www.quest.com/products/shareplex/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Oracle | SQL DW | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |
| Oracle | PostgreSQL için Azure DB |  |  |  |
| MongoDB | Cosmos DB | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/)<br/>[Imanis veri *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/)<br/>[Imanis veri *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [Cloudamize *](https://www.cloudamize.com/)<br/>[Imanis veri *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Cassandra | Cosmos DB | [Imanis veri *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [Imanis veri *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [Imanis veri *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) |
| MySQL | Azure SQL DB, mı, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| MySQL | MySQL için Azure DB | [MySQL döküm *](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) | [DMS](https://azure.microsoft.com/services/database-migration/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| RDS MySQL | MySQL için Azure DB | [MySQL döküm *](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) | [DMS](https://azure.microsoft.com/services/database-migration/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| PostgreSQL | PostgreSQL için Azure DB | [PG döküm *](https://www.postgresql.org/docs/11/static/app-pgdump.html) | [DMS](https://azure.microsoft.com/services/database-migration/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| RDS PostgreSQL | PostgreSQL için Azure DB | [PG döküm *](https://www.postgresql.org/docs/11/static/app-pgdump.html) | [DMS](https://azure.microsoft.com/services/database-migration/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| DB2 | Azure SQL DB, mı, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Access | Azure SQL DB, mı, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |
| Sybase | Azure SQL DB, mı, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| | | | | |

## <a name="post-migration-phase"></a>Geçiş sonrası aşaması

| **Kaynak** | **Hedef** | **İyileştir** |
| --- | --- | --- |
| SQL Server | Azure SQL DB | [Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) |
| SQL Server | Azure SQL DB mı | [Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) |
| SQL Server | Azure SQL VM | [Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) |
| SQL Server | SQL DW |  |
| RDS SQL | Azure SQL DB, mı, VM |  |
| Oracle | Azure SQL DB, mı, VM |  |
| Oracle | SQL DW |  |
| Oracle | PostgreSQL için Azure DB |  |
| MongoDB | Cosmos DB | [Cloudamize *](https://www.cloudamize.com/) |
| Cassandra | Cosmos DB |  |
| MySQL | Azure SQL DB, mı, VM |  |
| MySQL | MySQL için Azure DB |  |
| RDS MySQL | MySQL için Azure DB |  |
| PostgreSQL | PostgreSQL için Azure DB |  |
| RDS PostgreSQL | PostgreSQL için Azure DB |  |
| DB2 | Azure SQL DB, mı, VM |  |
| Access | Azure SQL DB, mı, VM |  |
| Sybase | Azure SQL DB, mı, VM |  |
| | | |

## <a name="next-steps"></a>Sonraki adımlar

Azure veritabanı geçiş Hizmeti'nin genel bakış için bkz [Azure veritabanı geçiş hizmeti nedir](dms-overview.md).
