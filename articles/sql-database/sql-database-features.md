---
title: "Azure SQL veritabanı özellik karşılaştırması | Microsoft Docs"
description: "Bu makalede, Azure SQL veritabanı ve özelliklerini yönetilen örnekleri birbirleriyle ve SQL Server ile karşılaştırır."
services: sql-database
documentationcenter: 
author: jovanpop-msft
ms.reviewer: bonova, carlrab
ms.service: sql-database
ms.topic: article
ms.date: 02/28/2018
ms.author: jovanpop
manager: cguyer
ms.openlocfilehash: 34aafdc377acf0b67674dbac2e67237440ed1420
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="feature-comparison-azure-sql-database-versus-sql-server"></a>Özellik karşılaştırması: SQL Server ile Azure SQL veritabanı 

Azure SQL veritabanı, SQL Server ile temel bir ortak kodun paylaşır. Azure SQL veritabanı tarafından desteklenen bir SQL Server özelliklerini oluşturduğunuz Azure SQL veritabanı türüne bağlıdır. Azure SQL veritabanı ile ya da bir parçası olarak bir veritabanı oluşturabilirsiniz bir [yönetilen örneği](sql-database-managed-instance.md) (şu anda önizlemede genel) veya tek bir veritabanı veya bir esnek havuz parçası olan bir veritabanı bir veritabanı oluşturabilirsiniz. 

Microsoft Azure SQL veritabanına özellikleri eklemek devam eder. Hizmet güncelleştirmeleri Web sayfası için Azure bu filtreleri kullanarak en son güncelleştirmeler için ziyaret edin:

* [SQL Veritabanı hizmeti](https://azure.microsoft.com/updates/?service=sql-database) için filtrelenmiş.
* SQL Veritabanı özellikleri için Genel Kullanılabilirlik [(GA) duyuruları](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability).

## <a name="sql-server-and-sql-database-feature-support"></a>SQL Server ve SQL veritabanı özellik desteği

Aşağıdaki tabloda, SQL Server'ın önemli olan özellikler listelenmekte ve özellik kısmen veya tamamen desteklenir ve özelliği hakkında daha fazla bilgi için bağlantı hakkında bilgi sağlar. 

| **SQL özelliği** | **Azure SQL veritabanı'nda desteklenen** | **Yönetilen örneği (Önizleme)** |
| --- | --- | --- |
| [Her zaman şifreli](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Evet - bkz [sertifika deposu](sql-database-always-encrypted.md) ve [anahtar kasası](sql-database-always-encrypted-azure-key-vault.md) | Evet - bkz [sertifika deposu](sql-database-always-encrypted.md) ve [anahtar kasası](sql-database-always-encrypted-azure-key-vault.md) |
| [AlwaysOn Kullanılabilirlik grupları:](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | [Yüksek kullanılabilirlik](sql-database-high-availability.md) her veritabanı ile eklenmiştir. Olağanüstü durum kurtarma ele alınmıştır [iş sürekliliği bir bakış ile Azure SQL veritabanı](sql-database-business-continuity.md) | [Yüksek kullanılabilirlik](sql-database-high-availability.md) her veritabanı ile eklenmiştir. Olağanüstü durum kurtarma ele alınmıştır [iş sürekliliği bir bakış ile Azure SQL veritabanı](sql-database-business-continuity.md) |
| [Bir veritabanı ekleme](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Hayır | Hayır |
| [Uygulama rolleri](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Evet | Evet |
|[Denetim](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Evet](sql-database-auditing.md)| Evet - bkz [farklar denetleme](sql-database-managed-instance-transact-sql-information.md#auditing) |
| [Otomatik yedekleme](sql-database-automated-backups.md) | Evet | Evet |
| [Otomatik (plan zorlama) ayarlama](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Evet](sql-database-automatic-tuning.md)| [Evet](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) |
| [Otomatik (dizinler) ayarlama](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Evet](sql-database-automatic-tuning.md)| Hayır |
| [BACPAC dosyası (verme)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Evet - bkz [SQL veritabanı dışarı aktarma](sql-database-export.md) | Evet |
| [BACPAC dosyası (içe aktarma)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Evet - bkz [SQL veritabanı alma](sql-database-import.md) | Evet |
| [Yedekleme komutu](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Hayır, yalnızca sistem tarafından başlatılan otomatik yedeklemeler - bkz [yedeklemeleri otomatik](sql-database-automated-backups.md) | Sistem tarafından başlatılan otomatik yedeklemeler ve kullanıcı tarafından başlatılan yalnızca kopya yedekleri - bkz [yedekleme farklar](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Yerleşik işlevler](https://docs.microsoft.com/sql/t-sql/functions/functions) | Çoğu - tekil işlevler bakın | Evet - bkz [saklı yordamlar, İşlevler, farklar tetikler](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Değişiklik verilerini yakalama](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Hayır | Evet |
| [Değişiklik izleme](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Evet |Evet |
| [Harmanlama deyimleri](https://docs.microsoft.com/sql/t-sql/statements/collations) | Evet | Evet |
| [Columnstore dizinleri](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Evet - [yalnızca Premium edition](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |Evet |
| [Ortak dil çalışma zamanı (CLR)](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Hayır | Evet - bkz [CLR farklar](sql-database-managed-instance-transact-sql-information.md#clr) |
| [Kapsanan veritabanları](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Evet | Evet |
| [Kapsanan kullanıcılar](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Evet | Evet |
| [Denetim akışı dil anahtar sözcükleri](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Evet | Evet |
| [Veritabanları arası sorguları](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Hayır - bkz [esnek sorgular](sql-database-elastic-query-overview.md) | Evet, artı [esnek sorgular](sql-database-elastic-query-overview.md) |
| [Veritabanları arası işlemler]((https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine)) | Hayır | Evet |
| [İmleçler](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Evet |Evet | 
| [Veri sıkıştırma](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Evet |Evet |
| [Veritabanı posta](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Hayır | Evet |
| [Veri Taşıma hizmeti (DMS)](https://docs.microsoft.com/sql/dma/dma-overview) | Evet | Evet |
| [Veritabanı yansıtma](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Hayır | Hayır |
| [Veritabanı yapılandırma ayarları](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Evet | Evet |
| [Veri Kalitesi Hizmetleri (DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Hayır | Hayır |
| [Veritabanı anlık görüntüleri](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Hayır | Hayır |
| [Veri türleri](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Evet |Evet |
| [DBCC deyimleri](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | Çoğu - tek tek deyimleri bakın | Evet - bkz [DBCC farklar](sql-database-managed-instance-transact-sql-information.md#dbcc) |
| [DDL deyimleri](https://docs.microsoft.com/sql/t-sql/statements/statements) | Çoğu - tek tek deyimleri bakın | Evet - bkz [T-SQL farkları](sql-database-managed-instance-transact-sql-information.md) |
| [DDL Tetikleyicileri](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Yalnızca veritabanı |  Evet |
| [Dağıtılmış bölüm görünümleri](https://docs.microsoft.com/sql/t-sql/statements/create-view-transact-sql#partitioned-views) | Hayır | Evet |
| [Dağıtılmış işlemler - MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Hayır - bkz [esnek işlemleri](sql-database-elastic-transactions-overview.md) |  Hayır - bkz [esnek işlemleri](sql-database-elastic-transactions-overview.md) |
| [DML deyimleri](https://docs.microsoft.com/sql/t-sql/queries/queries) | Evet | Evet |
| [DML Tetikleyicileri](https://docs.microsoft.com/sql/relational-databases/triggers/create-dml-triggers) | Çoğu - tek tek deyimleri bakın |  Evet |
| [DMV’ler](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | En çok - tek tek Dmv'leri bakın |  Evet - bkz [T-SQL farkları](sql-database-managed-instance-transact-sql-information.md) |
|[Dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)|[Evet](sql-database-dynamic-data-masking-get-started.md)| Evet |
| [Elastik havuzlar](sql-database-elastic-pool.md) | Evet | tek bir yönetilen örneği aynı kaynak havuzu paylaşan birden çok veritabanı olabilir |
| [Olay bildirimleri](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | Hayır - bkz [uyarıları](sql-database-insights-alerts-portal.md) | Evet |
| [İfadeler](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Evet | Evet |
| [Genişletilmiş olaylar](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Bazı - bkz [SQL veritabanında olaylar genişletilmiş](sql-database-xevent-db-diff-from-svr.md) | Evet - bkz [olayları farklar genişletilmiş ](sql-database-managed-instance-transact-sql-information.md#extended-events) |
| [genişletilmiş saklı yordamlar](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Hayır | Hayır |
[Dosyalar ve dosya grupları](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Yalnızca birincil dosya grubu | Evet |
| [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Hayır | Hayır |
| [Tam metin araması](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |  Üçüncü taraf sözcük ayırıcı desteklenmez. |Üçüncü taraf sözcük ayırıcı desteklenmez. |
| [İşlevler](https://docs.microsoft.com/sql/t-sql/functions/functions) | Çoğu - tekil işlevler bakın | Evet - bkz [saklı yordamlar, İşlevler, farklar tetikler](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Geo-restore](sql-database-recovery-using-backups.md#geo-restore) | Evet | COPY_ONLY Hayır – geri düzenli aralıklarla - ele tam yedeklemeler bkz [yedekleme farklar](sql-database-managed-instance-transact-sql-information.md#backup) ve [geri farklar](sql-database-managed-instance-transact-sql-information.md#restore-statement). |
| [Coğrafi çoğaltma](sql-database-geo-replication-overview.md) | Evet | Hayır |
| [Grafik işleme](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview) | Evet | Evet |
| [Bellek içi iyileştirme](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Evet - [yalnızca Premium edition](sql-database-in-memory.md) | Hayır |
| [JSON veri desteği](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | Evet | Evet |
| [Dil öğeleri](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | Çoğu - ayrı ayrı öğeler bakın |  Evet - bkz [T-SQL farkları](sql-database-managed-instance-transact-sql-information.md) |
| [Bağlantılı sunucular](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Hayır - bkz [esnek sorgu](sql-database-elastic-query-horizontal-partitioning.md) | Yalnızca SQL Server |
| [Günlük aktarma](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | [Yüksek kullanılabilirlik](sql-database-high-availability.md) her veritabanı ile eklenmiştir. Olağanüstü durum kurtarma ele alınmıştır [iş sürekliliği bir bakış ile Azure SQL veritabanı](sql-database-business-continuity.md) |[Yüksek kullanılabilirlik](sql-database-high-availability.md) her veritabanı ile eklenmiştir. Olağanüstü durum kurtarma ele alınmıştır [iş sürekliliği bir bakış ile Azure SQL veritabanı](sql-database-business-continuity.md) |
| [Ana Veri Hizmetleri (MDS)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Hayır | Hayır |
| [En düşük toplu içeri aktarma günlüğü](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Hayır | Hayır |
| [Sistem verileri değiştirme](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Hayır | Evet |
| [Çevrimiçi dizin işlemleri](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Evet | Evet |
| [İşleçler](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | Çok - tek tek işleçleri konusuna bakın |Evet - bkz [T-SQL farkları](sql-database-managed-instance-transact-sql-information.md) |
| [Bölümlendirme](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Evet | Evet |
| [Veritabanı geri yükleme noktası](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Evet - bkz [SQL veritabanını kurtarma](sql-database-recovery-using-backups.md#point-in-time-restore) | Evet - bkz [SQL veritabanını kurtarma](sql-database-recovery-using-backups.md#point-in-time-restore) |
| [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Hayır | Hayır |
| [İlke tabanlı yönetim](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Hayır | Hayır |
| [Koşulları](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Evet | Evet |
| [R Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Önizleme sürümü; bkz: [machine learning'de yenilikler nelerdir?](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services)  | Hayır |
| [Kaynak İdarecisi](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Hayır | Hayır |
| [Deyimleri geri yükleme](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Hayır | Evet - bkz [geri farklar](sql-database-managed-instance-transact-sql-information.md#restore-statement) |
| [Veritabanını yedekten geri yükleyin](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | Bkz: yalnızca - otomatik yedeklemeler [SQL veritabanını kurtarma](sql-database-recovery-using-backups.md) | Otomatik yedeklemelerden - bkz [SQL veritabanı kurtarma](sql-database-recovery-using-backups.md) ve tam yedeklerden - [yedekleme farklar](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Evet | Evet |
| [Anlam arama](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Hayır | Hayır |
| [Sıra numaraları](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Evet | Evet |
| [Service Broker](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Hayır | Evet - bkz [Service Broker farklar](sql-database-managed-instance-transact-sql-information.md#service-broker) |
| [Sunucu Yapılandırma ayarları](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Hayır | Evet - bkz [T-SQL farkları](sql-database-managed-instance-transact-sql-information.md) |
| [Set deyimleri](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | Çoğu - tek tek deyimleri bakın | Evet - bkz [T-SQL farkları](sql-database-managed-instance-transact-sql-information.md)|
| [SMO](https://docs.microsoft.com/sql/relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide) | Evet | Evet |
| [Uzamsal](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Evet | Evet |
| [SQL veri eşitleme](sql-database-get-started-sql-data-sync.md) | Evet | Evet |
| [SQL Operations Studio](https://docs.microsoft.com/sql/sql-operations-studio/what-is) | Evet | Evet |
| [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Hayır - bkz [esnek işler](sql-database-elastic-jobs-getting-started.md) | Evet - bkz [SQL Server Agent farklar](sql-database-managed-instance-transact-sql-information.md#sql-server-agent) |
| [SQL Server Analysis Services (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Hayır - bkz [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) | Hayır - bkz [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| [SQL Server denetimi](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | Hayır - bkz [SQL veritabanı denetimi](sql-database-auditing.md) | Evet - bkz [farklar denetleme](sql-database-managed-instance-transact-sql-information.md#auditing) |
| [SQL Server veri Araçları (SSDT)] (https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) | Evet | Evet |
| [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Kısmi – SQL Server veri araçları paketini geliştirme desteklenmiyor. | Genel Önizleme'de Hayır |
| [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) | Evet | Evet |
| [SQL Server PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Evet | Evet |
| [SQL Server Profiler](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Hayır - bkz [olaylar genişletilmiş](sql-database-xevent-db-diff-from-svr.md) | Evet |
| [SQL Server çoğaltması](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Yalnızca işlem ve anlık görüntü çoğaltma abonesi](sql-database-cloud-migrate.md) | Hayır |
| [SQL Server Raporlama Hizmetleri (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Hayır - [Power BI bakın](https://docs.microsoft.com/power-bi/) | Hayır - [Power BI bakın](https://docs.microsoft.com/power-bi/) |
| [Saklı yordamlar](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Evet | Evet |
| [Sistem saklı işlevleri](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Çoğu - tekil işlevler bakın | Evet - bkz [saklı yordamlar, İşlevler, farklar tetikler](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Sistem saklı yordamları](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Bazı - tek tek saklı yordamlara bakın | Evet - bkz [saklı yordamlar, İşlevler, farklar tetikler](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Sistem tabloları](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Bazı - bkz: tek tek tablolar | Evet - bkz [T-SQL farkları](sql-database-managed-instance-transact-sql-information.md) |
| [Sistem Katalog görünümleri](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Bazı - bkz: tek bir görünüm | Evet - bkz [T-SQL farkları](sql-database-managed-instance-transact-sql-information.md) |
| [Geçici tablolar](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#database-scoped-global-temporary-tables-azure-sql-database) | Yerel ve veritabanı kapsamlı genel geçici tabloları | Yerel ve örnek kapsamlı genel geçici tabloları |
| [Zamana bağlı tablolar](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | Evet | Evet |
| [İzleme Bayrakları](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql) | Hayır | Hayır |
| [Değişkenler](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Evet | Evet |
| [Saydam veri şifreleme (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Evet | Genel Önizleme'de Hayır |
[VNet](../virtual-network/virtual-networks-overview.md) | Kısmi - bkz [VNET uç noktaları](sql-database-vnet-service-endpoint-rule-overview.md) | Evet, yalnızca Resource Manager modeli |
| [Windows Server Yük Devretme Kümelemesi](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | [Yüksek kullanılabilirlik](sql-database-high-availability.md) her veritabanı ile eklenmiştir. Olağanüstü durum kurtarma ele alınmıştır [iş sürekliliği bir bakış ile Azure SQL veritabanı](sql-database-business-continuity.md) | [Yüksek kullanılabilirlik](sql-database-high-availability.md) her veritabanı ile eklenmiştir. Olağanüstü durum kurtarma ele alınmıştır [iş sürekliliği bir bakış ile Azure SQL veritabanı](sql-database-business-continuity.md) |
| [XML dizinler](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Evet | Evet |

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL Veritabanı hizmeti hakkında bilgi edinmek için bkz. [SQL Veritabanı nedir?](sql-database-technical-overview.md)
- Yönetilen bir örneği hakkında daha fazla bilgi için bkz: [yönetilen örneği nedir?](sql-database-managed-instance.md).
