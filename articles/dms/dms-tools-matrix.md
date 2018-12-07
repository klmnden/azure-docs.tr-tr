---
title: Veri geçiş hizmeti ve araçları matris - Azure | Microsoft Docs
description: Veritabanlarını geçirmek ve çeşitli aşamaları geçiş işlemini desteklemek için kullanılabilen araçları ve Hizmetleri hakkında bilgi edinin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 12/07/2018
ms.openlocfilehash: a3580c2939f03e6ede6341e7afb293e7f7c5f885
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53016138"
---
# <a name="service-and-tools-for-data-migration"></a>Hizmet ve veri geçişi için Araçlar

Bu makalede, Microsoft ve üçüncü taraf hizmetleri ve araçları, çeşitli veritabanı ve veri geçişi senaryoları ve özel görevleri ile yardımcı olmak kullanılabilir bir matrisi verilmektedir.

Aşağıdaki tablolarda, hizmet ve veri geçişi için başarıyla planlamak ve kendi çeşitli aşamaları boyunca tamamlamak için kullanabileceğiniz araçları belirleyin.

> [!NOTE]
> Aşağıdaki tablolarda, üçüncü taraf araçları yıldız işareti (*) ile işaretlenen öğeleri temsil eder.

## <a name="business-justification-stage"></a>İş Gerekçesi aşaması

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
| RDS/şirket içi MySQL | MySQL için Azure DB |  |  | [TCO hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| RDS/şirket içi PostgreSQL | PostgreSQL için Azure DB |  |  | [TCO hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| MySQL | Azure SQL DB, mı, VM | [Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) | [Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/) | [TCO hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| DB2 | Azure SQL DB, mı, VM |  |  |  |
| Access | Azure SQL DB, mı, VM |  |  |  |
| Sybase | Azure SQL DB, mı, VM |  |  |  |
| | | | | |

## <a name="pre-migration-stage"></a>Geçiş öncesi aşaması

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
| RDS/şirket içi MySQL | MySQL için Azure DB |  |  |  |
| RDS/şirket içi PostgreSQL | PostgreSQL için Azure DB |  |  |  |
| MySQL | Azure SQL DB, mı, VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/) |  |
| DB2 | Azure SQL DB, mı, VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Access | Azure SQL DB, mı, VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Sybase | Azure SQL DB, mı, VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| | | | | |

## <a name="migration-stage"></a>Geçiş aşaması

| **Kaynak** | **Hedef** | **Şema** | **Veriler**<br/>**(Çevrimdışı)** | **Veriler**<br/>**(Çevrimiçi)** |
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
| RDS/şirket içi MySQL | MySQL için Azure DB | [MySQL döküm *](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) | [DMS](https://azure.microsoft.com/services/database-migration/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| RDS/şirket içi PostgreSQL | PostgreSQL için Azure DB | [PG döküm *](https://www.postgresql.org/docs/11/static/app-pgdump.html) | [DMS](https://azure.microsoft.com/services/database-migration/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| MySQL | Azure SQL DB, mı, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| DB2 | Azure SQL DB, mı, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Access | Azure SQL DB, mı, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |
| Sybase | Azure SQL DB, mı, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Striim *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| | | | | |

## <a name="post-migration-stage"></a>Geçiş sonrası aşaması

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
| RDS/şirket içi MySQL | MySQL için Azure DB |  |
| RDS/şirket içi PostgreSQL | PostgreSQL için Azure DB |  |
| MySQL | Azure SQL DB, mı, VM |  |
| DB2 | Azure SQL DB, mı, VM |  |
| Access | Azure SQL DB, mı, VM |  |
| Sybase | Azure SQL DB, mı, VM |  |
| | | |

## <a name="next-steps"></a>Sonraki adımlar

Azure veritabanı geçiş Hizmeti'nin genel bakış için bkz. [Azure veritabanı geçiş hizmeti önizlemesi nedir](dms-overview.md).
