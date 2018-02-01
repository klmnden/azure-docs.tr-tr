# Genel Bakış

## [SQL Veri Ambarı Hakkında](sql-data-warehouse-overview-what-is.md)
## [Kopya kağıdı](cheat-sheet.md)

# Hızlı Başlangıçlar

## [Oluşturun ve bağlanın - portal](create-data-warehouse-portal.md)
## İşlemi duraklatma ve sürdürme
### [Portal](pause-and-resume-compute-portal.md)
### [PowerShell](pause-and-resume-compute-powershell.md)


# Öğreticiler
## [1 - Blobdan veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)

# Kavramlar
## Hizmet özellikleri
### [MPP mimarisi](massively-parallel-processing-mpp-architecture.md)
### [Performans katmanları](performance-tiers.md)
### [Veri ambarı birimleri](what-is-a-data-warehouse-unit-dwu-cdwu.md)
### [Veri ambarı yedekleri](sql-data-warehouse-backups.md)
### [Denetim](sql-data-warehouse-auditing-overview.md)
### [Kapasite sınırları](sql-data-warehouse-service-capacity-limits.md)
### [SSS](sql-data-warehouse-overview-faq.md)

## Güvenlik
### [Genel Bakış](sql-data-warehouse-overview-manage-security.md)
### [Kimlik doğrulaması](sql-data-warehouse-authentication.md)


## SQL Veri Ambarı’na geçiş
### [Genel Bakış](sql-data-warehouse-overview-migrate.md)
### [Geçiş şeması](sql-data-warehouse-migrate-schema.md)
### [Geçiş kodu](sql-data-warehouse-migrate-code.md)
### [Geçiş verileri](sql-data-warehouse-migrate-data.md)

## Verileri yükleme ve taşıma
### [Genel Bakış](design-elt-data-loading.md)
### [En iyi uygulamalar](guidance-for-loading-data.md)


## Tümleştirme
### [Genel Bakış](sql-data-warehouse-overview-integrate.md)
### [SQL Veritabanı esnek sorgu](how-to-use-elastic-query-with-sql-data-warehouse.md)


## İzleme ve ayarlama
### [Yönergeler](resource-classes-for-workload-management.md)
### [Columnstore sıkıştırması](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md)
### [İzleme](sql-data-warehouse-manage-monitor.md)
### [Sorun giderme](sql-data-warehouse-troubleshoot.md)

## Veri ambarlarını geliştirme
### [Genel Bakış](sql-data-warehouse-overview-develop.md)
### [Veri ambarı bileşenleri](sql-data-warehouse-overview-workload.md)

### Tablolar
#### [Genel Bakış](sql-data-warehouse-tables-overview.md)
#### [CTAS](sql-data-warehouse-develop-ctas.md)
#### [Veri türleri](sql-data-warehouse-tables-data-types.md)
#### [Dağıtılmış tablolar](sql-data-warehouse-tables-distribute.md)
#### [Dizinler](sql-data-warehouse-tables-index.md)
#### [Kimlik](sql-data-warehouse-tables-identity.md)
#### [Bölümler](sql-data-warehouse-tables-partition.md)
#### [Çoğaltılmış tablolar](design-guidance-for-replicated-tables.md)
#### [İstatistikler](sql-data-warehouse-tables-statistics.md)
#### [Geçici](sql-data-warehouse-tables-temporary.md)

### Sorgular
#### [Dinamik SQL](sql-data-warehouse-develop-dynamic-sql.md)
#### [Seçeneklere göre gruplandırma](sql-data-warehouse-develop-group-by-options.md)
#### [Etiketler](sql-data-warehouse-develop-label.md)

### T-SQL dil öğeleri
#### [Döngüler](sql-data-warehouse-develop-loops.md)
#### [Saklı yordamlar](sql-data-warehouse-develop-stored-procedures.md)
#### [İşlemler](sql-data-warehouse-develop-transactions.md)
#### [En iyi İşlem Uygulamaları](sql-data-warehouse-develop-best-practices-transactions.md)
#### [Kullanıcı tanımlı şemalar](sql-data-warehouse-develop-user-defined-schemas.md)
#### [Değişken ataması](sql-data-warehouse-develop-variable-assignment.md)
#### [Görünümler](sql-data-warehouse-develop-views.md)

# Nasıl yapılır kılavuzları
## Hizmet özellikleri
### [Bir veri ambarını geri yükleme - portal](sql-data-warehouse-restore-database-portal.md)
### [Bir veri ambarını geri yükleme - PowerShell](sql-data-warehouse-restore-database-powershell.md)
### [Bir veri ambarını geri yükleme - REST API’si](sql-data-warehouse-restore-database-rest-api.md)

## Güvenlik
### [Şifrelemeyi etkinleştirme - portal](sql-data-warehouse-encryption-tde.md)
### [Şifrelemeyi etkinleştirme - T-SQL](sql-data-warehouse-encryption-tde-tsql.md)
### [Tehdit algılama](sql-data-warehouse-security-threat-detection.md)


## Verileri yükleme ve taşıma
### [Contoso genel verileri](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
### [Azure Data Lake Store](sql-data-warehouse-load-from-azure-data-lake-store.md)
### [BCP](sql-data-warehouse-load-with-bcp.md)
### [Data Factory](sql-data-warehouse-load-with-data-factory.md)
### [AzCopy](sql-data-warehouse-load-from-sql-server-with-polybase.md)
### [RedGate](sql-data-warehouse-load-with-redgate.md)
### [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)


## Tümleştirme
### [SQL Veritabanı esnek sorgu yapılandırma](tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md)
### [Azure Stream Analytics işi ekleme](sql-data-warehouse-integrate-azure-stream-analytics.md)
### [Makine öğrenmesini kullanma](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
### [Power BI ile görselleştirme](sql-data-warehouse-get-started-visualize-with-power-bi.md)

## İzleme ve ayarlama
### [İş yükünüzü çözümleme](analyze-your-workload.md)

## Ölçeği genişletme
### [İşlemleri yönetme - portal](sql-data-warehouse-manage-compute-portal.md)
### [İşlemleri yönetme - PowerShell](sql-data-warehouse-manage-compute-powershell.md)
### [İşlemleri yönetme - REST API’si](sql-data-warehouse-manage-compute-rest-api.md)
### [İşlem düzeylerini otomatik hale getirme](manage-compute-with-azure-functions.md)


# Başvuru


## T-SQL
### [Tam başvuru](https://docs.microsoft.com/sql/t-sql/language-reference/)
### [SQL DW dil öğeleri](sql-data-warehouse-reference-tsql-language-elements.md)
### [SQL DW deyimleri](sql-data-warehouse-reference-tsql-statements.md)
## [Sistem görünümleri](sql-data-warehouse-reference-tsql-system-views.md)
## [PowerShell cmdlet'leri](sql-data-warehouse-reference-powershell-cmdlets.md)

# Kaynaklar
## [Azure Yol Haritası](https://azure.microsoft.com/roadmap/?category=databases)
## [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSQLDataWarehouse)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-data-warehouse/)
## [Fiyatlandırma hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)
## [Özellik istekleri](https://feedback.azure.com/forums/307516-sql-data-warehouse/)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=sql-data-warehouse)
## [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw/)
## [Destek](sql-data-warehouse-get-started-create-support-ticket.md)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)

## İş Ortakları
### [İş zekası](sql-data-warehouse-partner-business-intelligence.md)
### [Veri tümleştirmesi](sql-data-warehouse-partner-data-integration.md)
### [Veri yönetimi](sql-data-warehouse-partner-data-management.md)
