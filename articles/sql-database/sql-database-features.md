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
ms.custom: features
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/03/2017
ms.author: carlrab; jognanay
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: a1ede93b5aacf0d8a5bcf83f208f72be328ee72f
ms.lasthandoff: 04/15/2017


---
# <a name="azure-sql-database-features"></a>Azure SQL Veritabanı özellikleri

Aşağıdaki tablolarda Azure SQL Veritabanı’nın başlıca özellikleri ve SQL Server’ın eşdeğer özellikleri listelenmekte ve her bir özelliğin desteklenip desteklenmediğiyle ilgili bilgilerin yanı sıra özelliğin her bir platformdaki durumuyla ilgili daha fazla bilgi veren bir bağlantı verilmiştir. Var olan veritabanı çözümünü geçirirken göz önünde bulundurulması gereken Transact-SQL farklılıkları için bkz. [SQL Veritabanına geçiş sırasında Transact-SQL farklılıklarını çözümleme](sql-database-transact-sql-information.md).

Azure SQL Veritabanı’na özellik eklemeye devam ediyoruz. Bu nedenle Azure için Hizmet Güncelleştirmeleri web sayfamızı ziyaret etmenizi ve oradaki filtreleri kullanmanızı öneriyoruz:

* [SQL Veritabanı hizmeti](https://azure.microsoft.com/updates/?service=sql-database) için filtrelenmiş.
* SQL Veritabanı özellikleri için Genel Kullanılabilirlik [(GA) duyuruları](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability).

| **Özellik** | **SQL Server** | **Azure SQL Veritabanı** | 
| --- | :---: | :---: | 
| Etkin Coğrafi Çoğaltma | Desteklenmiyor - bkz. [AlwaysOn Kullanılabilirlik Grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | [Destekleniyor](sql-database-geo-replication-overview.md)
| Always Encrypted | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Destekleniyor - bkz. [Sertifika deposu](sql-database-always-encrypted.md) ve [Key Vault](sql-database-always-encrypted-azure-key-vault.md)|
| AlwaysOn Kullanılabilirlik Grupları | [Destekleniyor](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | Desteklenmiyor - Bkz. [Etkin Coğrafi Çoğaltma](sql-database-geo-replication-overview.md) |
| Veritabanı ekleme | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Desteklenmiyor |
| Uygulama rolleri | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) |
| Otomatik ölçeklendirme | Desteklenmiyor | Destekleniyor - bkz. [Hizmet katmanları](sql-database-service-tiers.md) |
| Azure Active Directory | Desteklenmiyor | [Destekleniyor](sql-database-aad-authentication.md) |
| Azure Data Factory | [Destekleniyor](../data-factory/data-factory-introduction.md) | [Destekleniyor](../data-factory/data-factory-introduction.md) |
| Denetim | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Destekleniyor](sql-database-auditing.md) |
| BACPAC dosyası (dışarı aktarma) | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | [Destekleniyor](sql-database-export.md) |
| BACPAC dosyası (içeri aktarma) | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | [Destekleniyor](sql-database-import.md) |
| BACKUP | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Desteklenmiyor |
| Yerleşik işlevler | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/functions/functions) | Çoğu - bkz. [ayrı ayrı işlevler] (https://docs.microsoft.com/sql/t-sql/functions/functions) |
| Değişiklik verilerini yakalama | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Desteklenmiyor |
| Değişiklik izleme | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) |
| Harmanlama deyimleri | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/statements/collations) | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/statements/collations) |
| Columnstore dizinleri | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | [Yalnızca Premium sürüm](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |
| Ortak dil çalışma zamanı (CLR) | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Desteklenmiyor |
| Bağımsız veritabanları | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) |
| Bağımsız kullanıcılar | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | [Destekleniyor](sql-database-manage-logins.md#non-administrator-users) |
| Akışı dil anahtar sözcükleri denetimi | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) |
| Veritabanları arası sorgular | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/cross-database-queries) | Kısmi - bkz. [Esnek sorgular](sql-database-elastic-query-overview.md) |
| İmleçler | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | 
| Veri sıkıştırma | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) |
| Veritabanı yedeklemeleri | [Kullanıcı tarafından yönetilir](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases) | [SQL Veritabanı hizmeti tarafından yönetilir](sql-database-automated-backups.md) |
| Veritabanı posta | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Desteklenmiyor |
| Veritabanı yansıtma | [Destekleniyor](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Desteklenmiyor |
| Veritabanı yapılandırma ayarları | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) |
| Data Quality Services (DQS) | [Destekleniyor](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Desteklenmiyor |
| Veritabanı anlık görüntüleri | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Desteklenmiyor |
| Veri türleri | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) |  
| DBCC deyimleri | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | Çoğu - bkz. [ayrı deyimler](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) |
| DDL deyimleri | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/statements/statements) | Çoğu - bkz. [Ayrı deyimler](https://docs.microsoft.com/sql/t-sql/statements/statements)
| DDL tetikleyicileri | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | [Yalnızca veritabanı](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) |
| Dağıtılmış işlemler | [MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Yalnızca sınırlı SQL Veritabanı içi senaryolar |
| DML deyimleri | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/queries/queries) | Çoğu - bkz. [Ayrı deyimler]](https://docs.microsoft.com/sql/t-sql/queries/queries) |
| DML tetikleyicileri | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/triggers/dml-triggers) | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/triggers/dml-triggers) |
| DMV’ler | [Tümü](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Bazıları - bkz. [Ayrı DMV’ler](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) |
| Esnek havuzlar | Desteklenmiyor | [Destekleniyor](sql-database-elastic-pool.md) |
| Esnek işler | Desteklenmiyor - Bkz. [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | [Destekleniyor](sql-database-elastic-jobs-getting-started.md) | 
| Esnek sorgular | Desteklenmiyor - Bkz. [Veritabanları arası sorgular](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/cross-database-queries) | [Destekleniyor](sql-database-elastic-query-overview.md) |
| Olay bildirimleri | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | [Destekleniyor](sql-database-insights-alerts-portal.md) |
| İfadeler | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |
| Genişletilmiş olaylar | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Bazıları - bkz. [Ayrı olaylar](sql-database-xevent-db-diff-from-svr.md) |
| Genişletilmiş saklı yordamlar | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Desteklenmiyor |
| Dosyalar ve dosya grupları | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | [Yalnızca birincil](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) |
| Dosya akışı | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Desteklenmiyor |
| Tam metin arama | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) | [Üçüncü taraf sözcük ayırıcılar desteklenmez](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |
| İşlevler | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/functions/functions) | Çoğu - bkz. [Ayrı işlevler](https://docs.microsoft.com/sql/t-sql/functions/functions) |
| Bellek içi iyileştirme | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | [Yalnızca Premium sürüm](sql-database-in-memory.md) |
| İşler | Bkz. [SQL Server Aracısı](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Bkz. [Esnek işler](sql-database-elastic-jobs-getting-started.md) |
| JSON veri desteği | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | [Destekleniyor](sql-database-json-features.md) |
| Dil öğeleri | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | Çoğu - bkz. [Ayrı öğeler](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) |  
| Bağlı sunucular | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Desteklenmiyor - Bkz. [Elastik sorgu](sql-database-elastic-query-horizontal-partitioning.md) |
| Günlük aktarma | [Destekleniyor](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | Desteklenmiyor - Bkz. [Etkin Coğrafi Çoğaltma](sql-database-geo-replication-overview.md) |
| Ana Veri Hizmetleri (AVH) | [Destekleniyor](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Desteklenmiyor |
| Toplu olarak içeri aktarmada en az günlük | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Desteklenmiyor |
| Sistem verilerini değiştirme | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Desteklenmiyor |
| Çevrimiçi dizin işlemleri | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | [Destekleniyor - işlem boyutu hizmet katmanıyla sınırlıdır](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) |
| İşleçler | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | Çoğu - bkz. [Ayrı işleçler](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) |
| Zaman noktasında veritabanını geri yükleme | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | [Destekleniyor](sql-database-recovery-using-backups.md#point-in-time-restore) |
| Polybase | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Desteklenmiyor
| İlke tabanlı yönetim | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Desteklenmiyor |
| Koşullar | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Çoğu - bkz. [Ayrı doğrulamalar](https://docs.microsoft.com/sql/t-sql/queries/predicates)
| R Hizmetleri | [Destekleniyor](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services)
| Kaynak idarecisi | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Desteklenmiyor |
| RESTORE deyimleri | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Desteklenmiyor | 
| Veritabanını yedekten geri yükleme | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | [Yalnızca yerleşik yedeklerden](sql-database-recovery-using-backups.md) |
| Satır Düzeyi Güvenlik | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) |
| Anlamsal arama | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Desteklenmiyor |
| Sıra numaraları | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) |
| Hizmet Aracısı | [Destekleniyor](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Desteklenmiyor |
| Sunucu yapılandırma ayarları | [Destekleniyor](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Desteklenmiyor - Bkz. [Veritabanı yapılandırma seçenekleri](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) |
| Küme deyimleri | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | Çoğu - Bkz. [Ayrı deyimler](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) 
| Uzamsal | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) |
| SQL Server Agent | [Destekleniyor](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Desteklenmiyor - Bkz. [Elastik işler](sql-database-elastic-jobs-getting-started.md) |
| SQL Server Analysis Services (SSAS) | [Destekleniyor](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Desteklenmiyor - Bkz. [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| SQL Server Integration Services (SSIS) | [Destekleniyor](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Desteklenmiyor - Bkz. [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) |
| SQL Server PowerShell | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) |
| SQL Server Profiler | [Destekleniyor](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Desteklenmiyor - Bkz. [Genişletilmiş olaylar](sql-database-xevent-db-diff-from-svr.md) |
| SQL Server Çoğaltma | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Yalnızca işlem ve anlık görüntü çoğaltma abonesi](sql-database-cloud-migrate.md) |
| SQL Server Reporting Services (SSRS) | [Destekleniyor](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Desteklenmiyor |
| Saklı yordamlar | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) |
| Sistem saklı işlevleri | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Bazıları - Bkz. [Ayrı işlevler](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) |
| Sistem saklı yordamları | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Bazıları - bkz. [Ayrı depolanmış yordamlar](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) |
| Sistem tabloları | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Bazıları - Bkz. [Ayrı tablo](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) |
| Sistem kataloğu görünümleri | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Bazıları - Bkz. [Ayrı görünümler](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql)
| Tablo Bölümleme | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Destekleniyor - [Yalnızca birincil dosya grubu](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) |
| Geçici tablolar | [Yerel ve genel](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#temporary-tables) | [Yalnızca yerel](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#temporary-tables) |
| Zamana bağlı tablolar | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | [Destekleniyor](sql-database-temporal-tables.md) |
| İşlemler | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/language-elements/transactions-transact-sql) | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/language-elements/transactions-transact-sql) |
| Değişkenler | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | 
| Saydam veri şifrelemesi (TDE)  | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | [Destekleniyor](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) |
| Windows Server Yük Devretme Kümelemesi | [Destekleniyor](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | Desteklenmiyor - Bkz. [Etkin Coğrafi Çoğaltma](sql-database-geo-replication-overview.md) |
| XML dizinleri | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | [Destekleniyor](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) |

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL Veritabanı hizmeti hakkında bilgi edinmek için bkz. [SQL Veritabanı nedir?](sql-database-technical-overview.md)
- Transact-SQL desteği ve farklılıkları hakkında bilgi için bkz. [SQL Veritabanına geçiş sırasında Transact-SQL farklılıklarını çözümleme](sql-database-transact-sql-information.md).

