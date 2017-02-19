# Genel Bakış
## [SQL Veritabanı nedir?](sql-database-technical-overview.md)
### [Hizmet katmanları](sql-database-service-tiers.md)
### [DTU ve eDTU](sql-database-what-is-a-dtu.md)
### [DTU kıyaslamaya genel bakış](sql-database-benchmark-overview.md)
### [Kaynak sınırları](sql-database-resource-limits.md)
### [Özellikler](sql-database-features.md)
### [SQL Veritabanı SSS](sql-database-faq.md)
## Karşılaştırmalar
### [Sanal makine üzerinde SQL Veritabanı ile SQL karşılaştırması](sql-database-paas-vs-sql-server-iaas.md)
### [T-SQL farklılıkları](sql-database-transact-sql-information.md)
### [SQL ve NoSQL karşılaştırması](../documentdb/documentdb-nosql-vs-sql.md)
## [SQL Veritabanı araçları](sql-database-manage-overview.md)
## [SQL Veritabanı öğreticileri](sql-database-explore-tutorials.md)
## [Çözüm hızlı başlangıçları](sql-database-solution-quick-starts.md)
## Güvenlik
### [Güvenliğe genel bakış](sql-database-security-overview.md)
### [Azure SQL Veritabanı için Azure Güvenlik Merkezi](https://azure.microsoft.com/documentation/articles/security-center-sql-database/)
### [SQL Güvenlik Merkezi](https://msdn.microsoft.com/library/azure/bb510589)
# Kullanmaya Başlama
## Veritabanları ve sunucular
### Öğrenin
#### [Sunucular](sql-database-server-overview.md)
#### [Tek veritabanları](sql-database-overview.md)
#### [Birden çok veritabanı](sql-database-elastic-scale-introduction.md)
##### Kiracıları eşleme
###### [Elastik istemci kitaplığı](sql-database-elastic-database-client-library.md)
###### [Parça eşleme yöneticisi](sql-database-elastic-scale-shard-map-management.md)
###### [Verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md)
###### [Kimlik bilgilerini yönetme](sql-database-elastic-scale-manage-credentials.md)
###### [Çok parçalı sorgulama](sql-database-elastic-scale-multishard-querying.md)
##### Elastik havuzlar (kaynak havuzları)
###### [Elastik havuz nedir?](sql-database-elastic-pool.md)
###### [Elastik havuz ne zaman kullanılır?](sql-database-elastic-pool-guidance.md)
###### [Esnek havuz fiyatlandırması](sql-database-elastic-pool-price.md)
##### Parçalı veritabanları
###### [Elastik araçlar sözlüğü](sql-database-elastic-scale-glossary.md)
###### [Parçalar arasında veri taşıma](sql-database-elastic-scale-overview-split-and-merge.md)
###### [Esnek araçlar hakkında SSS](sql-database-elastic-scale-faq.md)
##### Elastik sorgu (veritabanları arası sorgular)
###### [Elastik sorgu nedir?](sql-database-elastic-query-overview.md)
##### Elastik işlemler (dağıtılmış işlemler)
###### [Bulut veritabanlarında işlemler](sql-database-elastic-transactions-overview.md)
##### Elastik işler (veritabanları arası işler)
###### [Elastik iş nedir?](sql-database-elastic-jobs-overview.md)
#### [Azure RemoteApp’i kullanarak SQL Veritabanı’na bağlanma](sql-database-ssms-remoteapp.md)
#### [Azure Otomasyonu hizmetini kullanarak SQL Veritabanlarını yönetme](sql-database-manage-automation.md)
### Yapın
#### [Azure portalını kullanarak tek veritabanı oluşturma](sql-database-get-started.md)
#### [PowerShell’i kullanarak tek veritabanı oluşturma](sql-database-get-started-powershell.md)
#### [C# kullanarak tek veritabanı oluşturma](sql-database-get-started-csharp.md)
#### [Parçalı uygulama oluşturma](sql-database-elastic-scale-get-started.md)
#### [Parçalar arasında veri taşıma](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
#### [Elastik işler kullanmaya başlama](sql-database-elastic-jobs-getting-started.md)
#### [Elastik sorgular kullanmaya başlama](sql-database-elastic-query-getting-started-vertical.md)
#### [Elastik sorgu kullanarak rapor oluşturma](sql-database-elastic-query-getting-started.md)
#### [Farklı şemalara sahip veritabanlarında sorgulama yapma](sql-database-elastic-query-vertical-partitioning.md)
#### [Ölçeği genişletilmiş veritabanlarında raporlama](sql-database-elastic-query-horizontal-partitioning.md)
## Verileri geçirme ve taşıma
### Öğrenin
#### [Bir veritabanını geçirme](sql-database-cloud-migrate.md)
#### [İşlem çoğaltması](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
#### [Veri eşitleme](sql-database-get-started-sql-data-sync.md)
#### [SQL veritabanı kopyalama](sql-database-copy.md)
## Güvenlik Duvarı kuralları, kimlik doğrulaması ve yetkilendirme
### Öğrenin
#### [Erişim denetimi](sql-database-control-access.md)
#### [Güvenlik duvarı](sql-database-firewall-configure.md)
#### [Oturum açmaları yönetme](sql-database-manage-logins.md)
### Yapın
#### [SQL kimlik doğrulaması ve yetkilendirme](sql-database-control-access-sql-authentication-get-started.md)
#### [Azure AD kimlik doğrulama ve yetkilendirme](sql-database-control-access-aad-authentication-get-started.md)
## Verileri güvenli hale getirme ve koruma
### Öğrenin
#### Denetim
##### [Denetim](sql-database-auditing-get-started.md)
##### [Alt düzey istemci desteği ve denetim için IP uç noktası değişiklikleri](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)
#### [Tehdit algılama](sql-database-threat-detection-get-started.md)
#### Verileri şifreleme
##### [Azure anahtar kasası](sql-database-always-encrypted-azure-key-vault.md)
##### [Saydam veri şifrelemesi](https://msdn.microsoft.com/library/azure/dn948096)
##### [Sütun şifreleme](https://msdn.microsoft.com/library/azure/ms179331)
#### Verileri maskeleme
##### Dinamik veri maskeleme
###### [Azure portal](sql-database-dynamic-data-masking-get-started.md)
### Yapın
#### [Azure portalını kullanarak dinamik veri maskeleme](sql-database-dynamic-data-masking-get-started.md)
##### [Windows sertifika deposunu kullanan Always Encrypted](sql-database-always-encrypted.md)
## İş sürekliliği
### Öğrenin
#### [Genel Bakış](sql-database-business-continuity.md)
#### [Veritabanı yedeklemeleri](sql-database-automated-backups.md)
#### [Uzun vadeli bekletme](sql-database-long-term-retention.md)
#### [Yedeklemeleri kullanarak veritabanı kurtarma](sql-database-recovery-using-backups.md)
#### [Tek tabloyu kurtarma](sql-database-cloud-migrate-restore-single-table-azure-backup.md)
#### [Veri merkezi kesintisinden kurtarma](sql-database-disaster-recovery.md)
#### [Olağanüstü durum kurtarma için kimlik doğrulama gereksinimleri](sql-database-geo-replication-security-config.md)
#### [İş sürekliliği tasarım senaryoları](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
#### [Esnek havuzlar ile olağanüstü durum kurtarma stratejileri](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
#### [Toplu yükseltmeler](sql-database-manage-application-rolling-upgrade.md)
#### [Olağanüstü durum kurtarma tatbikatı gerçekleştirme](sql-database-disaster-recovery-drills.md)
#### [Etkin Coğrafi Çoğaltmaya genel bakış](sql-database-geo-replication-overview.md)
### Yapın
#### [Azure portalı: Yedekleme ve geri yükleme](sql-database-get-started-backup-recovery.md)
#### [PowerShell: Yedekleme ve geri yükleme](sql-database-get-started-backup-recovery-powershell.md)
## Uygulama geliştirme
### Öğrenin
#### [Veritabanı uygulaması geliştirmeye genel bakış](sql-database-develop-overview.md)
#### [Bağlantı kitaplıkları](sql-database-libraries.md)
#### [Çok müşterili SaaS uygulamaları](sql-database-design-patterns-multi-tenancy-saas-applications.md)
#### [Satır düzeyi güvenlik ile çok kiracılı SaaS uygulamalarını ölçeklendirme](sql-database-elastic-tools-multi-tenant-row-level-security.md)
#### [ADO.NET 4.5 için 1433’ten sonraki bağlantı noktalarını kullanma](sql-database-develop-direct-route-ports-adonet-v12.md)
#### [Uygulamada kimlik doğrulaması için gerekli değerleri alma](sql-database-client-id-keys.md)
### Yapın
#### Uygulamayı bağlama
##### [.NET](sql-database-develop-dotnet-simple.md)
##### [C ve C++](sql-database-develop-cplusplus-simple.md)
##### [Java](sql-database-develop-java-simple.md)
##### [Node.js](sql-database-develop-nodejs-simple.md)
##### [PHP](sql-database-develop-php-simple.md)
##### [Python](sql-database-develop-python-simple.md)
##### [Ruby](sql-database-develop-ruby-simple.md)
##### [Excel](sql-database-connect-excel.md)
#### [Visual Studio ile bağlanma](sql-database-connect-query.md)
#### [İstemci uygulaması oluşturma](https://www.microsoft.com/sql-server/developer-get-started)
#### [Hata iletileriyle çalışma](sql-database-develop-error-messages.md)
#### [Entity framework kullanma](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
#### [Dapper ile istemci kitaplığı kullanma](sql-database-elastic-scale-working-with-dapper.md)
### Müşteri uygulamaları
#### [Daxko/CSI Yazılımı](sql-database-implementation-daxko.md)
#### [GEP](sql-database-implementation-gep.md)
#### [SnelStart](sql-database-implementation-snelstart.md)
#### [Umbraco](sql-database-implementation-umbraco.md)
## Veritabanı geliştirme
### Öğrenin
#### Zamana bağlı tablolar
##### [Zamana bağlı tablolar](sql-database-temporal-tables.md)
##### [Elde tutma ilkeleri](sql-database-temporal-tables-retention-policy.md)
#### [JSON verileri](sql-database-json-features.md)
#### [Bellek içi iyileştirme](sql-database-in-memory.md)
### Yapın
#### [SQL Server geliştirme](https://msdn.microsoft.com/library/ms179422.aspx)
#### [Bellek içi OLTP’yi benimseme](sql-database-in-memory-oltp-migration.md)
## İzleme ve Ayarlama
### Öğrenin
#### [Tek veritabanları](sql-database-single-database-monitor.md)
#### [SQL Veritabanı Danışmanına genel bakış](sql-database-advisor.md)
#### [Tek veritabanı kılavuzu](sql-database-performance-guidance.md)
#### [Performans öngörüleri: Azure portalı](sql-database-performance.md)
#### [Toplu işlem kullanma](sql-database-use-batching-to-improve-performance.md)
#### [Genişletilmiş olaylar](sql-database-xevent-db-diff-from-svr.md)
## SQL Veritabanı V11
### [Web ve işletme sürümlerinin kullanımdan kaldırılması](sql-database-web-business-sunset-faq.md)
### [Service Tier Advisor](sql-database-service-tier-advisor.md)
### [Esnek havuz değerlendirme aracı](sql-database-elastic-pool-database-assessment-powershell.md)
### [V12'ye yükseltme](sql-database-v12-plan-prepare-upgrade.md)
#### [Azure portalını kullanarak yükseltme](sql-database-upgrade-server-portal.md)
#### [PowerShell’i kullanarak yükseltme](sql-database-upgrade-server-powershell.md)
# Nasıl yapılır
## Oluşturma ve yönetme
### [Azure portalını kullanarak SQL Veritabanı’nı yönetme](sql-database-manage-portal.md)
### [PowerShell’i kullanarak SQL Veritabanı'nı yönetme](sql-database-manage-powershell.md)
### [SMSS’yi kullanarak SQL Veritabanı'nı yönetme](sql-database-manage-azure-ssms.md)
### Sunucular
#### [Sunucu oluşturma](sql-database-create-servers.md)
#### [Sunucu ayarlarını görüntüleme veya güncelleştirme](sql-database-view-update-server-settings.md)
### Tek veritabanları
#### [Tek veritabanları oluşturma](sql-database-create-databases.md)
#### [Veritabanı ayarlarını görüntüleme veya güncelleştirme](sql-database-view-update-database-settings.md)
### Güvenlik duvarı kuralları
#### [Azure portalını kullanarak güvenlik duvarı kuralları oluşturma](sql-database-configure-firewall-settings.md)
#### [PowerShell’i kullanarak güvenlik duvarı kuralları oluşturma](sql-database-configure-firewall-settings-powershell.md)
#### [REST API’yi kullanarak güvenlik duvarı kuralları oluşturma](sql-database-configure-firewall-settings-rest.md)
#### [T-SQL’yi kullanarak güvenlik duvarı kuralları oluşturma](sql-database-configure-firewall-settings-tsql.md)
### Birden çok veritabanı
#### [İstemci uygulamalarında istemci kitaplığını yükseltme](sql-database-elastic-scale-upgrade-client-library.md)
#### Parçalı veritabanları
##### [Güvenlik yapılandırması](sql-database-elastic-scale-split-merge-security-configuration.md)
##### [Parça ekleme](sql-database-elastic-scale-add-a-shard.md)
##### [Parça eşleme sorunlarını giderme](sql-database-elastic-database-recovery-manager.md)
##### [Ölçeği genişletilen mevcut veritabanlarını parçalı veritabanlarına geçirme](sql-database-elastic-convert-to-use-elastic-tools.md)
##### [Parça eşleme yöneticisi için performans sayaçları oluşturma](sql-database-elastic-database-perf-counters.md)
#### Esnek işler
##### [Elastik işler hizmetini nasıl yükleyebilirim?](sql-database-elastic-jobs-service-installation.md)
##### [PowerShell’i kullanarak esnek işler oluşturma ve yönetme](sql-database-elastic-jobs-powershell.md) 
##### [Azure portalını kullanarak elastik işler oluşturma ve yönetme](sql-database-elastic-jobs-create-and-manage.md)
##### [Elastik işleri nasıl kaldırabilirim?](sql-database-elastic-jobs-uninstall.md)
#### Esnek havuzlar
##### [Azure portalını kullanarak oluşturma](sql-database-elastic-pool-create-portal.md)
##### [PowerShell kullanarak oluşturma](sql-database-elastic-pool-create-powershell.md)
##### [C# kullanarak oluşturma](sql-database-elastic-pool-create-csharp.md)
##### [Azure portalını kullanarak yönetme](sql-database-elastic-pool-manage-portal.md)
##### [PowerShell’i kullanarak yönetme](sql-database-elastic-pool-manage-powershell.md)
##### [C# kullanarak yönetme](sql-database-elastic-pool-manage-csharp.md)
##### [T-SQL kullanarak yönetme](sql-database-elastic-pool-manage-tsql.md)
##  Kimlik doğrulama ve yetkilendirme
### [Azure AD kimlik doğrulaması](sql-database-aad-authentication.md)
### [Multi-Factor Authentication](sql-database-ssms-mfa-authentication.md)
## Verileri şifreleme
### [Saydam veri şifrelemesi](https://msdn.microsoft.com/library/azure/dn948096)
### [Sütun şifreleme](https://msdn.microsoft.com/library/azure/ms179331)
## Veritabanlarını geçirme
### Uyumluluk belirleme
#### [SQL Paketi yardımcı programını kullanarak uyumluluğu belirleme](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
#### [SSMS‘yi kullanarak uyumluluğu belirleme](sql-database-cloud-migrate-determine-compatibility-ssms.md)
### Uyumluluk sorunlarını giderme
#### [SSDT’yi kullanarak uyumluluk sorunlarını düzeltme](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
#### [SSMS’yi kullanarak uyumluluk sorunlarını düzeltme](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
#### [SMW’yi kullanarak uyumluluk sorunlarını düzeltme](sql-database-cloud-migrate-fix-compatibility-issues.md)
### [SSMS Geçiş Sihirbazı'nı kullanarak geçiş yapma](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
## İzleme ve ayarlama
### [Sorgu Performansı Öngörüleri](sql-database-query-performance.md)
### [SQL Veritabanı Danışmanı](sql-database-advisor-portal.md)
### [DMV’ler](sql-database-monitoring-with-dmvs.md)
### [Uyumluluk düzeyleri](sql-database-compatibility-level-query-performance-130.md)
### [Performans ayarlama ipuçları](sql-database-troubleshoot-performance.md)
### Hizmet katmanlarını ve performans düzeylerini değiştirme
#### [Azure portalını kullanarak hizmet katmanlarını değiştirme](sql-database-scale-up.md)
#### [PowerShell’i kullanarak hizmet katmanlarını değiştirme](sql-database-scale-up-powershell.md)
### [Uyarı oluşturma](sql-database-insights-alerts-portal.md)
#### [Bellek içi OLTP Depolama Alanını izleme](sql-database-in-memory-oltp-monitoring.md)
### Sorgu Deposu
#### [Sorgu Deposu kullanarak performans izleme](https://msdn.microsoft.com/library/dn817826.aspx)
#### [Sorgu Deposu kullanım senaryoları](https://msdn.microsoft.com/library/mt614796.aspx)
#### [Sorgu Deposunu çalıştırma](sql-database-operate-query-store.md)
### Genişletilmiş olaylar
#### [Olay dosyası hedef kodu](sql-database-xevent-code-event-file.md)
#### [Halka arabelleği hedef kodu](sql-database-xevent-code-ring-buffer.md)
## Veri taşıma
### SQL veritabanı kopyalama
#### [Azure portalını kullanarak kopyalama](sql-database-copy-portal.md)
#### [PowerShell’i kullanarak kopyalama](sql-database-copy-powershell.md)
#### [T-SQL kullanarak kopyalama](sql-database-copy-transact-sql.md)
### Veritabanını BACPAC dosyasına aktarma
#### [Azure portalını kullanarak dışarı aktarma](sql-database-export.md)
#### [SSMS‘yi kullanarak dışarı aktarma](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
#### [SQL Paketi yardımcı programını kullanarak dışarı aktarma](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
#### [PowerShell’i kullanarak dışarı aktarma](sql-database-export-powershell.md)
### Veritabanını BACPAC dosyasından alma
#### [Azure portalını kullanarak dışarı aktarma](sql-database-import.md)
#### [PowerShell’i kullanarak içeri aktarma](sql-database-import-powershell.md)
#### [SSMS‘yi kullanarak içeri aktarma](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
#### [SQL Paketi yardımcı programını kullanarak içeri aktarma](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
### [CSV dosyasından BCP kullanarak yükleme](sql-database-load-from-csv-with-bcp.md)
## Sorgu
### [SSMS‘yi kullanarak sorgulama](sql-database-connect-query-ssms.md)
## Yedekleme ve Geri Yükleme
### Uzun süreli yedek saklama
#### [Uzun süreli yedek saklama yapılandırma](sql-database-configure-long-term-retention.md)
#### [Kurtarma Hizmetleri kasasındaki yedekleri görüntüleme](sql-database-view-backups-in-vault.md)
#### [Uzun süreli yedek saklamadan geri yükleme](sql-database-restore-from-long-term-retention.md)
#### [Uzun süreli yedek saklamadan silme](sql-database-long-term-retention-delete.md)
### Silinen veritabanını geri yükleme
#### [Azure portalını kullanarak silinmiş öğeleri geri yükleme](sql-database-restore-deleted-database-portal.md)
#### [PowerShell’i kullanarak silinmiş öğeleri geri yükleme](sql-database-restore-deleted-database-powershell.md)
### Belirli bir noktaya geri yükleme
#### [Zaman içinde bir noktaya geri yükleme](sql-database-point-in-time-restore.md)
#### [En eski geri yükleme noktasını görüntüleme](sql-database-view-oldest-restore-point.md)
### [Coğrafi olarak yedekli bir yedekten geri yükleme](sql-database-geo-restore.md)
## Etkin Coğrafi Çoğaltma
### [Azure portalını kullanarak yapılandırma](sql-database-geo-replication-portal.md)
### [PowerShell’i kullanarak yapılandırma](sql-database-geo-replication-powershell.md)
### [T-SQL’yi kullanarak yapılandırma](sql-database-geo-replication-transact-sql.md)
### [Azure portalını kullanarak yük devretme](sql-database-geo-replication-failover-portal.md)
### [PowerShell'i kullanarak yük devretme](sql-database-geo-replication-failover-powershell.md)
### [T-SQL’yi kullanarak yük devretme](sql-database-geo-replication-failover-transact-sql.md)
## Sorun giderme
### [Bağlantı sorunları](sql-database-troubleshoot-common-connection-issues.md)
### [Geçici bağlantı hatası](sql-database-troubleshoot-connection.md)
### [Tanılama ve engelleme](sql-database-connectivity-issues.md)
### [İzinler](sql-database-troubleshoot-permissions.md)
### [Veritabanlarını taşıma](sql-database-troubleshoot-moving-data.md)
# Başvuru
## [PowerShell](/powershell/resourcemanager/azurerm.sql/v2.3.0/azurerm.sql)
## [PowerShell (Esnek DB)](/powershell/elasticdatabasejobs/v0.8.33/elasticdatabasejobs)
## [.NET](/dotnet/api/microsoft.azure.management.sql.models)
## [Java](/java/api/com.microsoft.azure.management.sql)
## [Node.js](https://msdn.microsoft.com/library/mt652093.aspx)
## [Python](https://msdn.microsoft.com/library/mt652092.aspx)
## [Ruby](https://msdn.microsoft.com/library/mt691981.aspx)
## [PHP](https://msdn.microsoft.com/library/dn865013.aspx)
## [T-SQL](https://msdn.microsoft.com/library/bb510741.aspx)
## [REST](https://msdn.microsoft.com/library/azure/mt163571.aspx)

# İlgili
## SQL Veritabanı Yönetim Kitaplığı
### [SQL Veritabanı Yönetim Kitaplığı paketi](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)
## [SQL Server Sürücüleri](https://msdn.microsoft.com/library/mt654049.aspx)
### [ADO.NET](https://msdn.microsoft.com/library/mt657768.aspx)
### [JDBC](https://msdn.microsoft.com/library/mt484311.aspx)
### [ODBC](https://msdn.microsoft.com/library/mt654048.aspx)

# Kaynaklar
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/)
## [MSDN forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/sql-azure)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=sql-database)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?service=sql-database)
## [SQL Server Araçları](https://msdn.microsoft.com/library/mt238365.aspx)
## [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
## [SQL Server Veri Araçları (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
## [BCP](https://msdn.microsoft.com/library/ms162802.aspx)
## [SQLCMD](https://msdn.microsoft.com/library/ms162773.aspx)
## [SqlPackage](https://msdn.microsoft.com/hh550080.aspx)


<!--HONumber=Feb17_HO1-->


