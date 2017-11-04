---
title: "Azure SQL Veritabanı Özelliklerine Genel Bakış | Microsoft Belgeleri"
description: "Bu sayfada Azure SQL Veritabanı mantıksal sunucuları ve veritabanları için bir genel bakış sunulmakta ve her özellik için bağlantılara yer verilen bir özellik destek tablosuna yer verilmektedir."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d1a46fa4-53d2-4d25-a0a7-92e8f9d70828
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 10/25/2017
ms.author: carlrab
ms.openlocfilehash: 1505b85a8828cd64045a982ece48e49d85ea3314
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="azure-sql-database-features"></a>Azure SQL Veritabanı özellikleri

Azure SQL veritabanı, SQL Server ile temel bir ortak kodun paylaşır ve veritabanı düzeyinde aynı özelliklerin çoğunu destekler. Azure SQL Database ve SQL Server önemli özellik farklarını kaynağı örneği düzeyindedir. 

Azure SQL Veritabanı’na özellik eklemeye devam ediyoruz. Bu nedenle Azure için Hizmet Güncelleştirmeleri web sayfamızı ziyaret etmenizi ve oradaki filtreleri kullanmanızı öneriyoruz:

* [SQL Veritabanı hizmeti](https://azure.microsoft.com/updates/?service=sql-database) için filtrelenmiş.
* SQL Veritabanı özellikleri için Genel Kullanılabilirlik [(GA) duyuruları](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability).

## <a name="sql-server-and-sql-database-feature-support"></a>SQL Server ve SQL veritabanı özellik desteği

Aşağıdaki tabloda, SQL Server'ın önemli özellikleri listeler ve her belirli bir özellik desteklenip ve özelliği hakkında daha fazla bilgi için bağlantı hakkında bilgi sağlar. Var olan veritabanı çözümünü geçirirken göz önünde bulundurulması gereken Transact-SQL farklılıkları için bkz. [SQL Veritabanına geçiş sırasında Transact-SQL farklılıklarını çözümleme](sql-database-transact-sql-information.md).


| **SQL Server özelliği** | **Azure SQL veritabanı'nda desteklenen** | 
| --- | --- |  
| [Her zaman şifreli](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Evet - bkz [sertifika deposu](sql-database-always-encrypted.md) ve [anahtar kasası](sql-database-always-encrypted-azure-key-vault.md)|
| [AlwaysOn Kullanılabilirlik grupları:](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | Yüksek kullanılabilirlik ile her veritabanı dahil edilir. Olağanüstü durum kurtarma ele alınmıştır [iş sürekliliği bir bakış ile Azure SQL veritabanı](sql-database-geo-replication-overview.md) |
| [Bir veritabanı ekleme](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Hayır |
| [Uygulama rolleri](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Evet |
|[Denetim](/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Evet](sql-database-auditing.md)|
| [Otomatik ayarlama](/sql/relational-databases/automatic-tuning/automatic-tuning)| [Evet](sql-database-automatic-tuning.md)|
| [BACPAC dosyası (verme)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Evet - bkz [SQL veritabanı dışarı aktarma](sql-database-export.md) |
| [BACPAC dosyası (içe aktarma)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Evet - bkz [SQL veritabanı alma](sql-database-import.md) |
| [Yedekleme komutu](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Hayır - bkz [yedeklemeleri otomatik](sql-database-automated-backups.md) |
| [Yerleşik işlevler](https://docs.microsoft.com/sql/t-sql/functions/functions) | Çoğu - tekil işlevler bakın |
| [Değişiklik verilerini yakalama](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Hayır |
| [Değişiklik izleme](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Evet |
| [Harmanlama deyimleri](https://docs.microsoft.com/sql/t-sql/statements/collations) | Evet |
| [Columnstore dizinleri](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Evet - [yalnızca Premium edition](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |
| [Ortak dil çalışma zamanı (CLR)](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Hayır |
| [Kapsanan veritabanları](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Evet |
| [Kapsanan kullanıcılar](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Evet |
| [Denetim akışı dil anahtar sözcükleri](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Evet |
| [Veritabanları arası sorguları](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/cross-database-queries) | Kısmi - bkz. [Esnek sorgular](sql-database-elastic-query-overview.md) |
| [İmleçler](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Evet | 
| [Veri sıkıştırma](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Evet |
| [Veritabanı posta](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Hayır |
| [Veritabanı yansıtma](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Hayır |
| [Veritabanı yapılandırma ayarları](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Evet |
| [Veri Kalitesi Hizmetleri (DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Hayır |
| [Veritabanı anlık görüntüleri](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Hayır |
| [Veri türleri](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Evet |  
| [DBCC deyimleri](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | Çoğu - tek tek deyimleri bakın |
| [DDL deyimleri](https://docs.microsoft.com/sql/t-sql/statements/statements) | Evet |
| [DDL Tetikleyicileri](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Yalnızca veritabanı |
| [Dağıtılmış işlemler - MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Hayır - bkz [esnek işlemleri](sql-database-elastic-transactions-overview.md) |
| [DML deyimleri](https://docs.microsoft.com/sql/t-sql/queries/queries) | Evet |
| [DML Tetikleyicileri](https://docs.microsoft.com/en-us/sql/relational-databases/triggers/create-dml-triggers) | Çoğu - tek tek deyimleri bakın | 
| [DMV’ler](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Bazı - tek tek Dmv'leri bakın |
|[Dinamik veri maskeleme](/sql/relational-databases/security/dynamic-data-masking)|[Evet](sql-database-dynamic-data-masking-get-started.md)|
| [Olay bildirimleri](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | Hayır - bkz [uyarıları](sql-database-insights-alerts-portal.md) |
| [İfadeler](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Evet |
| [Genişletilmiş olaylar](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Bazı - bkz [SQL veritabanında olaylar genişletilmiş](sql-database-xevent-db-diff-from-svr.md) |
| [Genişletilmiş saklı yordamlar](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Hayır |
| [Dosyalar ve dosya grupları](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Yalnızca birincil dosya grubu |
| [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Hayır |
| [Tam metin araması](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) | Üçüncü taraf sözcük ayırıcı desteklenmez. |
| [İşlevler](https://docs.microsoft.com/sql/t-sql/functions/functions) | Çoğu - tekil işlevler bakın |
| [Grafik işleme](/sql/relational-databases/graphs/sql-graph-overview) | Evet |
| [Bellek içi iyileştirme](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Evet - [yalnızca Premium edition](sql-database-in-memory.md) |
| [JSON veri desteği](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | Evet |
| [Dil öğeleri](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | Çoğu - ayrı ayrı öğeler bakın |  
| [Bağlantılı sunucular](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Hayır - bkz [esnek sorgu](sql-database-elastic-query-horizontal-partitioning.md) |
| [Günlük aktarma](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | Yüksek kullanılabilirlik ile her veritabanı dahil edilir. Olağanüstü durum kurtarma ele alınmıştır [iş sürekliliği bir bakış ile Azure SQL veritabanı](sql-database-geo-replication-overview.md) |
| [Ana Veri Hizmetleri (MDS)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Hayır |
| [En düşük toplu içeri aktarma günlüğü](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Hayır |
| [Sistem verileri değiştirme](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Hayır |
| [Çevrimiçi dizin işlemleri](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Evet |
| [İşleçler](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | Çok - tek tek işleçleri konusuna bakın |
| [Veritabanı geri yükleme noktası](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Evet - bkz [SQL veritabanını kurtarma](sql-database-recovery-using-backups.md#point-in-time-restore) |
| [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Hayır |
| [İlke tabanlı yönetim](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Hayır |
| [Koşulları](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Evet |
| [R Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Önizleme sürümü; bkz: [machine learning'de yenilikler nelerdir?](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services)  |
| [Kaynak İdarecisi](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Hayır |
| [Deyimleri geri yükleme](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Hayır | 
| [Veritabanını yedekten geri yükleyin](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | Yalnızca - yerleşik yedeklemelerden bkz [SQL veritabanını kurtarma](sql-database-recovery-using-backups.md) |
| [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Evet |
| [Anlam arama](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Hayır |
| [Sıra numaraları](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Evet |
| [Hizmet Aracısı](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Hayır |
| [Sunucu Yapılandırma ayarları](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Hayır |
| [Set deyimleri](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | Çoğu - tek tek deyimleri bakın 
| [Uzamsal](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Evet |
| [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Hayır - bkz [esnek işler](sql-database-elastic-jobs-getting-started.md) |
| [SQL Server Analysis Services (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Bkz: [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| [SQL Server denetimi](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | Hayır - bkz [SQL veritabanı denetimi](sql-database-auditing.md) |
| [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Bkz: [Azure veri fabrikası](https://azure.microsoft.com/services/data-factory/) |
| [SQL Server PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Evet |
| [SQL Server Profiler](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Hayır - bkz [olaylar genişletilmiş](sql-database-xevent-db-diff-from-svr.md) |
| [SQL Server çoğaltması](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Yalnızca işlem ve anlık görüntü çoğaltma abonesi](sql-database-cloud-migrate.md) |
| [SQL Server Raporlama Hizmetleri (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Hayır |
| [Saklı yordamlar](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Evet |
| [Sistem saklı işlevleri](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Bazı - bakın tekil İşlevler |
| [Sistem saklı yordamları](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Bazı - tek tek saklı yordamlara bakın |
| [Sistem tabloları](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Bazı - bkz: tek tek tablolar |
| [Sistem Katalog görünümleri](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Bazı - bkz: tek bir görünüm |
| [Tablo bölümleme](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Evet - yalnızca birincil dosya grubu |
| [Geçici tablolar](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#temporary-tables) | Yerel ve veritabanı kapsamlı genel geçici tabloları yalnızca |
| [Zamana bağlı tablolar](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | Evet |
| [Değişkenler](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Evet | 
| [Saydam veri şifreleme (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Evet |
| [Windows Server Yük Devretme Kümelemesi](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | Yüksek kullanılabilirlik ile her veritabanı dahil edilir. Olağanüstü durum kurtarma ele alınmıştır [iş sürekliliği bir bakış ile Azure SQL veritabanı](sql-database-geo-replication-overview.md) |
| [XML dizinler](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Evet |

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL Veritabanı hizmeti hakkında bilgi edinmek için bkz. [SQL Veritabanı nedir?](sql-database-technical-overview.md)
- Transact-SQL desteği ve farklılıkları hakkında bilgi için bkz. [SQL Veritabanına geçiş sırasında Transact-SQL farklılıklarını çözümleme](sql-database-transact-sql-information.md).
