# Genel Bakış
## [SQL Veritabanı nedir?](sql-database-technical-overview.md)
## [SQL Veritabanı SSS](sql-database-faq.md)
## Özellikler
### [Hizmet katmanları](sql-database-service-tiers.md)
### [Veritabanı İşlem Birimleri](sql-database-what-is-a-dtu.md)
### [DTU kıyaslamaya genel bakış](sql-database-benchmark-overview.md)
### [Yönetim araçları](sql-database-manage-overview.md)
## Önemli noktalar ve sınırlamalar
### [Sanal makine üzerinde SQL Veritabanı ile SQL karşılaştırması](sql-database-paas-vs-sql-server-iaas.md)
### [T-SQL farklılıkları](sql-database-transact-sql-information.md)
### [Kaynak sınırları](sql-database-resource-limits.md)
### [Genel sınırlamalar](sql-database-general-limitations.md)
### [Güvenlik yönergeleri](sql-database-security-guidelines.md)

## Avantajlar
### [Öğrenir ve uyum sağlar](sql-database-learn-and-adapt.md)
### [Uygulama çalışırken ölçeklendirir](sql-database-scale-on-the-fly.md)
### [Çok kiracılı uygulamalar oluşturur](sql-database-build-multi-tenant-apps.md)
### [Güvenliği sağlar ve korur](sql-database-helps-secures-and-protects.md)
### [Ortamınızda çalışır](sql-database-works-in-your-environment.md)

## Senaryolar

### Sunucular, havuzlar, veritabanları ve güvenlik duvarları
#### [Esnek veritabanı havuzu ne zaman kullanılır?](sql-database-elastic-pool-guidance.md)
#### [Esnek veritabanı havuzları](sql-database-elastic-pool.md)
#### [Otomasyon](sql-database-manage-automation.md)

### Ölçeklendirilen veritabanları
#### [Genel Bakış](sql-database-elastic-scale-introduction.md)
#### [Ölçeklenebilir bulut veritabanları oluşturma](sql-database-elastic-database-client-library.md)
#### [Veritabanları arası işler](sql-database-elastic-jobs-overview.md)
#### [Esnek veritabanı araçları sözlüğü](sql-database-elastic-scale-glossary.md)
#### [SSS](sql-database-elastic-scale-faq.md)

### Erişim ve izinler
#### [Genel Bakış](sql-database-security.md)
#### [Azure SQL Veritabanı için Azure Güvenlik Merkezi](../security-center/security-center-sql-database.md?toc=%2fazure%2fsql-database%2ftoc.json)
#### [SQL Güvenlik Merkezi](https://msdn.microsoft.com/library/azure/bb510589)

### İş sürekliliği
#### [Genel Bakış](sql-database-business-continuity.md)
#### [Veritabanı yedeklemeleri](sql-database-automated-backups.md)
#### [Yedeklemeleri kullanarak veritabanı kurtarma](sql-database-recovery-using-backups.md)
#### [Olağanüstü durum kurtarma için kimlik doğrulama gereksinimleri](sql-database-geo-replication-security-config.md)
#### [İş sürekliliği tasarım senaryoları](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
#### [Esnek havuzlar ile olağanüstü durum kurtarma stratejileri](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
#### [Toplu yükseltmeler](sql-database-manage-application-rolling-upgrade.md)

### [Veritabanı geliştirme](sql-database-develop-overview.md)
### [SQL Server veritabanlarını geçirme](sql-database-cloud-migrate.md)

## Müşteri Uygulamaları
### [Daxko/CSI Yazılımı](sql-database-implementation-daxko.md)
### [GEP](sql-database-implementation-gep.md)
### [SnelStart](sql-database-implementation-snelstart.md)
### [Umbraco](sql-database-implementation-umbraco.md)


# Kullanmaya Başlama
## [Çözüm hızlı başlangıçları](sql-database-solution-quick-starts.md)
## SQL Veritabanı oluşturma
### [Azure portal](sql-database-get-started.md)
### [C#](sql-database-get-started-csharp.md)
### [PowerShell](sql-database-get-started-powershell.md)
## Ölçeği artırılmış veritabanları oluşturma ve yönetme
### [Esnek ölçek](sql-database-elastic-scale-get-started.md)
### [Esnek işler](sql-database-elastic-jobs-getting-started.md)
### [Çapraz veritabanı raporları](sql-database-elastic-query-getting-started.md)
### [Çapraz veritabanı sorguları](sql-database-elastic-query-getting-started-vertical.md)
## Veritabanlarını izleme ve ayarlama
### [SQL Veritabanı Danışmanına genel bakış](sql-database-advisor.md)
### [Azure portalında iş yükü öngörüleri](sql-database-performance.md)
## [Erişim ve izin oluşturup yönetme](sql-database-get-started-security.md)
## [Bellek içi iyileştirme](sql-database-in-memory.md)
## [Veri eşitleme](sql-database-get-started-sql-data-sync.md)

# Nasıl yapılır?

## Oluşturma ve yönetme
### Sunucular ve veritabanları
#### [Tek veritabanları](sql-database-manage-portal.md)
#### [Azure Portal](sql-database-get-started.md)
#### [C#](sql-database-get-started-csharp.md)
#### [PowerShell](sql-database-manage-powershell.md)
#### [SQL Server Management Studio](sql-database-manage-azure-ssms.md)
### Esnek veritabanı havuzları
#### [Azure Portal](sql-database-elastic-pool-create-portal.md)
#### [PowerShell](sql-database-elastic-pool-create-powershell.md)
#### [C#](sql-database-elastic-pool-create-csharp.md)
#### [T-SQL](sql-database-elastic-pool-manage-tsql.md)
### Parçalı veritabanları
#### [Parça eşleme yöneticisini kullanma](sql-database-elastic-scale-shard-map-management.md)
#### [Güvenlik yapılandırması birleştir/böl](sql-database-elastic-scale-split-merge-security-configuration.md)
#### [Dapper ile çalışma](sql-database-elastic-scale-working-with-dapper.md)
#### [Entity framework kullanma](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
#### [Verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md)
#### [Çok kiracılı satır düzeyi güvenliği](sql-database-elastic-tools-multi-tenant-row-level-security.md)
#### [Kimlik bilgilerini yönetme](sql-database-elastic-scale-manage-credentials.md)
#### [Ayırma-birleştirme hizmetini dağıtma](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
#### [Parça ekleme](sql-database-elastic-scale-add-a-shard.md)
#### [RecoveryManager sınıfı ile parça eşleme sorunlarını düzeltme](sql-database-elastic-database-recovery-manager.md)
###  Kimlik Doğrulaması
#### [Azure AD kimlik doğrulaması](sql-database-aad-authentication.md)
#### [Multi-Factor Authentication](sql-database-ssms-mfa-authentication.md)
### Güvenlik duvarı kuralları
#### [Azure Portal](sql-database-configure-firewall-settings.md)
#### [PowerShell](sql-database-configure-firewall-settings-powershell.md)
#### [REST API](sql-database-configure-firewall-settings-rest.md)
#### [T-SQL](sql-database-configure-firewall-settings-tsql.md)
### İşler
#### [Hizmet yükleme](sql-database-elastic-jobs-service-installation.md)
#### [Azure Portal](sql-database-elastic-jobs-create-and-manage.md)
#### [PowerShell](sql-database-elastic-jobs-powershell.md)
#### [İstemci kitaplığını yükseltme](sql-database-elastic-scale-upgrade-client-library.md)
### [Oturum açma bilgileri](sql-database-manage-logins.md)

## Geliştirme
### [Genel Bakış](https://msdn.microsoft.com/library/mt763826.aspx)
### [Bağlantı kitaplıkları](sql-database-libraries.md)
### Senaryolar
#### [Çok müşterili SaaS uygulamaları](sql-database-design-patterns-multi-tenancy-saas-applications.md)
#### [JSON verileri](sql-database-json-features.md)
#### [Zamana bağlı tablolar](sql-database-temporal-tables.md)
#### [Elde tutma ilkeleri](sql-database-temporal-tables-retention-policy.md)
### Uygulamayı bağlama
#### [.NET](sql-database-develop-dotnet-simple.md)
#### [Java](sql-database-develop-java-simple.md)
#### [Node.js](sql-database-develop-nodejs-simple.md)
#### [PHP](sql-database-develop-php-simple.md)
#### [Python](sql-database-develop-python-simple.md)
#### [Ruby](sql-database-develop-ruby-simple.md)
#### [Excel](sql-database-connect-excel.md)
#### [Visual Studio](sql-database-connect-query.md)

### [Veritabanları oluşturma](sql-database-get-started-powershell.md)
### Esnek havuzları yönetme
#### [PowerShell](sql-database-elastic-pool-manage-powershell.md)
#### [C#](sql-database-elastic-pool-manage-csharp.md)
### [Uygulamada kimlik doğrulaması için gerekli değerleri alma](sql-database-client-id-keys.md)
### [ADO.NET 4.5 için 1433’ten sonraki bağlantı noktalarını kullanma](sql-database-develop-direct-route-ports-adonet-v12.md)
### [Hata iletileriyle çalışma](sql-database-develop-error-messages.md)
### [Toplu işlem kullanma](sql-database-use-batching-to-improve-performance.md)

### Başvuru
#### [.NET (kavramlar)](https://msdn.microsoft.com/library/kb9s9ks0.aspx)
#### [.NET (API Başvurusu)](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)
#### [Entity Framework](https://www.nuget.org/packages/EntityFramework/)
#### [Azure SDK (indirme)](https://www.visualstudio.com/vs/azure-tools/)
#### [Azure SDK (belgeler)](../dotnet-sdk.md)
#### [PowerShell Hizmet Yönetimi cmdlet’leri](https://msdn.microsoft.com/library/azure/dn546723.aspx)
#### REST
##### [REST (Kaynak Yönetimi)](https://msdn.microsoft.com/library/azure/mt420159)
##### [REST (Hizmet Yönetimi)](https://msdn.microsoft.com/library/azure/dn505719.aspx)

## Tehditleri algılama
### [Tehdit algılama](sql-database-threat-detection-get-started.md)
### [Güvenlik duvarı](sql-database-firewall-configure.md)

## Verileri şifreleme
### [Her zaman şifreli genel bakışı](sql-database-always-encrypted.md)
### [Her zaman şifreli Azure Anahtar Kasası](sql-database-always-encrypted-azure-key-vault.md)
### [Saydam veri şifrelemesi](https://msdn.microsoft.com/library/azure/dn948096)
### [Sütun şifreleme](https://msdn.microsoft.com/library/azure/ms179331)

## Verileri maskeleme
### [Dinamik veri maskeleme](sql-database-dynamic-data-masking-get-started-portal.md)
### [Alt düzey istemciler](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)

## Geçiş
### Uyumluluk belirleme
#### [SQL Paketi yardımcı programı](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md)
### Uyumluluk sorunlarını giderme
#### [SQL Server Veri Araçları](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
#### [SQL Azure Geçiş Sihirbazı](sql-database-cloud-migrate-fix-compatibility-issues.md)
### [SQL Server Management Studio Geçiş Sihirbazı'nı kullanma](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
### [İşlem çoğaltma kullanma](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
### [Ölçeği genişletilen mevcut veritabanlarını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md)

## İzleme ve ayarlama
### [Tek veritabanları](sql-database-performance-guidance.md)
### [Sorgu Performansı Öngörüleri](sql-database-query-performance.md)
### [SQL Veritabanı Danışmanı](sql-database-advisor-portal.md)
### [Veritabanı performansı](sql-database-single-database-monitor.md)
### [DMV’ler](sql-database-monitoring-with-dmvs.md)
### [Uyumluluk düzeyleri](sql-database-compatibility-level-query-performance-130.md)
### [Olay denetimi](sql-database-auditing-get-started.md)
### [Parça eşleme yöneticisi için performans sayaçları](sql-database-elastic-database-perf-counters.md)
### [Performans ayarlama ipuçları](sql-database-troubleshoot-performance.md)
### Hizmet katmanlarını ve performans düzeylerini değiştirme
#### [Azure Portal](sql-database-scale-up.md)
#### [PowerShell](sql-database-scale-up-powershell.md)
### Bellek içi OLTP
#### [Bellek içi OLTP’yi benimseme](sql-database-in-memory-oltp-migration.md)
#### [Bellek içi OLTP Depolama Alanını izleme](sql-database-in-memory-oltp-monitoring.md)
### Sorgu Deposu
#### [Sorgu Deposu kullanarak performans izleme](https://msdn.microsoft.com/library/dn817826.aspx)
#### [Sorgu Deposu kullanım senaryoları](https://msdn.microsoft.com/library/mt614796.aspx)
#### [Sorgu Deposunu çalıştırma](sql-database-operate-query-store.md)
### Genişletilmiş olaylar
#### [Genişletilmiş olaylar](sql-database-xevent-db-diff-from-svr.md)
#### [Olay dosyası hedef kodu](sql-database-xevent-code-event-file.md)
#### [Halka arabelleği hedef kodu](sql-database-xevent-code-ring-buffer.md)

## Veri taşıma
### [SQL veritabanı kopyalama](sql-database-copy.md)
#### [Azure portal](sql-database-copy-portal.md)
#### [PowerShell](sql-database-copy-powershell.md)
#### [T-SQL](sql-database-copy-transact-sql.md)
### Veritabanını BACPAC dosyasına aktarma
#### [Azure Portal](sql-database-export.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
#### [SQL Paketi yardımcı programı](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
#### [PowerShell](sql-database-export-powershell.md)
### Veritabanını BACPAC dosyasından alma
#### [Azure Portal](sql-database-import.md)
#### [PowerShell](sql-database-import-powershell.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
#### [SQL Paketi yardımcı programı](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
### [CSV dosyasından BCP kullanarak yükleme](sql-database-load-from-csv-with-bcp.md)
### [Ölçeği genişletilen bulut veritabanları arasında veri taşıma](sql-database-elastic-scale-overview-split-and-merge.md)

## Sorgu
### [SQL Server Management Studio](sql-database-connect-query-ssms.md)
### [Çok parçalı sorgulama](sql-database-elastic-scale-multishard-querying.md)
### Veritabanları arası sorgular
#### [Genel Bakış](sql-database-elastic-query-overview.md)
#### [Farklı şemalarla çapraz veritabanı sorgulama](sql-database-elastic-query-vertical-partitioning.md)
#### [Çapraz veritabanı raporlama](sql-database-elastic-query-horizontal-partitioning.md)
#### [Bulut veritabanlarında dağıtılmış işlemler](sql-database-elastic-transactions-overview.md)

## Geri Yükleme
### Silinen veritabanını geri yükleme
#### [Azure Portal](sql-database-restore-deleted-database-portal.md)
#### [PowerShell](sql-database-restore-deleted-database-powershell.md)
### Belirli bir noktaya geri yükleme
#### [Azure Portal](sql-database-point-in-time-restore-portal.md)
#### [PowerShell](sql-database-point-in-time-restore-powershell.md)
### Coğrafi Geri Yükleme
#### [Azure Portal](sql-database-geo-restore-portal.md)
#### [PowerShell](sql-database-geo-restore-powershell.md)
#### [Tek tablo](sql-database-cloud-migrate-restore-single-table-azure-backup.md)
### [Veri merkezi kesintisinden kurtarma](sql-database-disaster-recovery.md)
### [Olağanüstü durum kurtarma tatbikatı gerçekleştirme](sql-database-disaster-recovery-drills.md)

## Çoğaltma
### [Etkin Coğrafi Çoğaltmaya genel bakış](sql-database-geo-replication-overview.md)
### Yapılandırma
#### [PowerShell](sql-database-geo-replication-powershell.md)
#### [T-SQL](sql-database-geo-replication-transact-sql.md)
### Yük devretme
#### [Azure Portal](sql-database-geo-replication-failover-portal.md)
#### [PowerShell](sql-database-geo-replication-failover-powershell.md)
#### [T-SQL](sql-database-geo-replication-failover-transact-sql.md)

## Sorun giderme
### [Bağlantı sorunları](sql-database-troubleshoot-common-connection-issues.md)
### [Geçici bağlantı hatası](sql-database-troubleshoot-connection.md)
### [Tanılama ve engelleme](sql-database-connectivity-issues.md)
### [İzinler](sql-database-troubleshoot-permissions.md)
### [Veritabanlarını taşıma](sql-database-troubleshoot-moving-data.md)

# Başvuru
## [PowerShell](/powershell/azureps-cmdlets-docs/)
## [PowerShell klasik](/powershell/servicemanagement/)
## [Java](/java/api/)
## [.NET](/dotnet/api/)
## [T-SQL](https://msdn.microsoft.com/library/azure/bb510741.aspx)
## [Azure SQL Veritabanı Cmdlet’leri](https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
## [SQL Server Cmdlet'leri](https://msdn.microsoft.com/library/mt740629.aspx)
## [REST](/rest/api/sql/)

## SQL Veritabanı Yönetim Kitaplığı
### [SQL Veritabanı Yönetim Kitaplığı Başvurusu](https://msdn.microsoft.com/library/azure/mt349017.aspx)
### [SQL Veritabanı Yönetim Kitaplığı paketi](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)
## [SQL Server Sürücüleri](https://msdn.microsoft.com/library/mt654049.aspx)
### [ADO.NET](https://msdn.microsoft.com/library/mt657768.aspx)
### [JDBC](https://msdn.microsoft.com/library/mt484311.aspx)
### [Node.js](https://msdn.microsoft.com/library/mt652093.aspx)
### [ODBC](https://msdn.microsoft.com/library/mt654048.aspx)
### [PHP](https://msdn.microsoft.com/library/dn865013.aspx)
### [Python](https://msdn.microsoft.com/library/mt652092.aspx)
### [Ruby](https://msdn.microsoft.com/library/mt691981.aspx)

# Kaynaklar
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?service=sql-database)
## [SQL Server Araçları](https://msdn.microsoft.com/library/mt238365.aspx)
## [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
## [SQL Server Veri Araçları (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
## [BCP](https://msdn.microsoft.com/library/ms162802.aspx)
## [SQLCMD](https://msdn.microsoft.com/library/ms162773.aspx)
## [SqlPackage](https://msdn.microsoft.com/hh550080.aspx)
## [Forum](https://social.msdn.microsoft.com/Forums/home?forum=ssdsgetstarted)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=sql-database)


<!--HONumber=Nov16_HO2-->


