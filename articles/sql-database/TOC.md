# Genel Bakış
## [SQL Database nedir?](sql-database-technical-overview.md)
## Özellikler
### [Hizmet katmanları](sql-database-service-tiers.md)
### [DTU nedir?](sql-database-what-is-a-dtu.md)
### [DTU kıyaslamaya genel bakış](sql-database-benchmark-overview.md)
### [SQL Veritabanı güvenlik duvarı ve güvenlik duvarı kuralları](sql-database-firewall-configure.md)
### [Yönetim araçları](sql-database-manage-overview.md)
## Önemli noktalar ve sınırlamalar
### [Sanal makine üzerinde SQL Veritabanı ile SQL karşılaştırması](sql-database-paas-vs-sql-server-iaas.md)
### [T-SQL farklılıkları](sql-database-transact-sql-information.md)
### [Kaynak sınırları](sql-database-resource-limits.md)
### [Genel sınırlamalar](sql-database-general-limitations.md)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/)
## [Yenilikler](https://azure.microsoft.com/updates/?service=sql-database)
## [SQL Veritabanı SSS](sql-database-faq.md)

## Avantajlar
### [Öğrenir ve uyum sağlar](sql-database-learn-and-adapt.md)
### [Uygulama çalışırken ölçeklendirir](sql-database-scale-on-the-fly.md)
### [Çok kiracılı uygulamalar oluşturur](sql-database-build-multi-tenant-apps.md)
### [Güvenliği sağlar ve korur](sql-database-helps-secures-and-protects.md)
### [Ortamınızda çalışır](sql-database-works-in-your-environment.md)

## Senaryolar
### Sunucu, havuz, veritabanı ve güvenlik duvarı oluşturma ve yönetme
#### Esnek veritabanı havuzları oluşturma ve yönetme
#### [Esnek veritabanı havuzu ne zaman kullanılır?](sql-database-elastic-pool-guidance.md)
#### [Güvenlik yönergeleri](sql-database-security-guidelines.md)
#### [Esnek veritabanı havuzları](sql-database-elastic-pool.md)
#### [Automation](sql-database-manage-automation.md)
#### Hizmet katmanları ve performans düzeylerini değiştirme
##### [Azure [portal](sql-database-scale-up.md)
##### [PowerShell](sql-database-scale-up-powershell.md)
### Ölçeği artırılmış veritabanları oluşturma ve yönetme
#### [Genel Bakış](sql-database-elastic-scale-introduction.md)
#### [Ölçeklenebilir bulut veritabanları oluşturma](sql-database-elastic-database-client-library.md)
#### [Parça eşleme yöneticisi ile ölçeklendirme](sql-database-elastic-scale-shard-map-management.md)
#### [Ölçeği genişletilen mevcut veritabanlarını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md)
#### [Ölçeği genişletilen bulut veritabanları arasında veri taşıma](sql-database-elastic-scale-overview-split-and-merge.md)
#### [Entity framework kullanma](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
#### [Dapper ile çalışma](sql-database-elastic-scale-working-with-dapper.md)
#### [Parça eşleme yöneticisi için performans sayaçları](sql-database-elastic-database-perf-counters.md)
#### Çapraz veritabanı işleri ve sorguları
##### [Çapraz veritabanı sorgulama](sql-database-elastic-query-overview.md)
##### [Farklı şemalarla çapraz veritabanı sorgulama](sql-database-elastic-query-vertical-partitioning.md)
##### [Çapraz veritabanı raporlama](sql-database-elastic-query-horizontal-partitioning.md)
##### [Çapraz veritabanı işleri](sql-database-elastic-jobs-overview.md)
##### [Verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md)
##### [Çok parçalı sorgulama](sql-database-elastic-scale-multishard-querying.md)
##### [Çok kiracılı satır düzeyi güvenliği](sql-database-elastic-tools-multi-tenant-row-level-security.md)
##### [Bulut veritabanlarında dağıtılmış işlemler](sql-database-elastic-transactions-overview.md)
####[Esnek veritabanı araçları sözlüğü](sql-database-elastic-scale-glossary.md)
#### [SSS](sql-database-elastic-scale-faq.md)
### Erişim ve izin oluşturup yönetme
#### [Genel Bakış](sql-database-security.md)
#### [Güvenlik yönergeleri](sql-database-security-guidelines.md)
#### [Oturum açmaları yönetme](sql-database-manage-logins.md)
#### [Azure SQL Veritabanı için Azure Güvenlik Merkezi](https://azure.microsoft.com/documentation/articles/security-center-sql-database/)
#### [SQL Güvenlik Merkezi](https://msdn.microsoft.com/library/azure/bb510589)
### [Veritabanı ve uygulama geliştirme](sql-database-develop-overview.md)
### Veritabanı geçişi
#### [SQL Server veritabanını geçirme](sql-database-cloud-migrate.md)
### İş sürekliliği sağlama
#### [Genel Bakış](sql-database-business-continuity.md)
#### [Veritabanı yedeklemeleri](sql-database-automated-backups.md) 
#### [Yedeklemeleri kullanarak veritabanı kurtarma](sql-database-recovery-using-backups.md)
#### [Veri merkezi kesintisinden kurtarma](sql-database-disaster-recovery.md)
#### [Olağanüstü durum kurtarma için kimlik doğrulama gereksinimleri](sql-database-geo-replication-security-config.md)
#### [İş sürekliliği tasarım senaryoları](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
#### [Esnek havuzlar ile olağanüstü durum kurtarma stratejileri](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
#### [Toplu yükseltmeler](sql-database-manage-application-rolling-upgrade.md)
### Veritabanlarını izleme ve ayarlama
#### [Tek veritabanları](sql-database-single-database-monitor.md)
#### [Tek veritabanı kılavuzu](sql-database-performance-guidance.md)
#### [Azure portalında iş yükü öngörüleri](sql-database-performance.md)

## Müşteri Uygulamaları
### [Daxko/CSI Yazılımı](sql-database-implementation-daxko.md)
### [GEP](sql-database-implementation-gep.md)
### [SnelStart](sql-database-implementation-snelstart.md)
### [Umbraco](sql-database-implementation-umbraco.md)

# Kullanmaya Başlama
## Sunucu, havuz, veritabanı ve güvenlik duvarı oluşturma
### [Azure Portal](sql-database-get-started.md)
### [PowerShell](sql-database-get-started-powershell.md)
### [C#](sql-database-get-started-csharp.md)
## Verileri sorgulama
### [SQL Server Management Studio](sql-database-connect-query-ssms.md)
## Sunucu, havuz, veritabanı ve güvenlik duvarı yönetme
### [Azure Portal](sql-database-manage-portal.md)
### [SQL Server Management Studio](sql-database-manage-azure-ssms.md)
### [PowerShell](sql-database-manage-powershell.md)
## [Erişim ve izin oluşturup yönetme](sql-database-get-started-security.md)
## Verileri güvenli hale getirme ve koruma
### [Denetim](sql-database-auditing-get-started.md)
### [Tehdit algılama](sql-database-threat-detection-get-started.md)
### Dinamik veri maskeleme
#### [Azure Portal](sql-database-dynamic-data-masking-get-started-portal.md)
## Ölçeği artırılmış veritabanları oluşturma ve yönetme
### [Esnek ölçek](sql-database-elastic-scale-get-started.md)
### [Esnek işler](sql-database-elastic-jobs-getting-started.md)
### Esnek sorgular
### [Çapraz veritabanı raporları](sql-database-elastic-query-getting-started.md)
### [Çapraz veritabanı sorguları](sql-database-elastic-query-getting-started-vertical.md)
## [Bellek içi iyileştirme](sql-database-in-memory.md)
## [Veritabanlarını taşıma](sql-database-troubleshoot-moving-data.md)
## [Veri eşitleme](sql-database-get-started-sql-data-sync.md)
##Veritabanlarını izleme ve ayarlama
### [SQL Veritabanı Danışmanına genel bakış](sql-database-advisor.md)
### [Azure portalında iş yükü öngörüleri](sql-database-performance.md)
## [Çözüm hızlı başlangıçları](sql-database-solution-quick-starts.md)
# Nasıl yapılır?
## [Denetim](sql-database-auditing-get-started.md)
## Oluştur
### Kaynak grubu
#### [PowerShell](sql-database-manage-powershell.md#create-a-resource-group)
### Sunucular
#### [Azure Portal](sql-database-get-started.md)
#### [C#](sql-database-get-started-csharp.md)
#### [PowerShell](sql-database-manage-powershell.md#create-a-sql-database-server)
### Esnek veritabanı havuzları
#### [Azure Portal](sql-database-elastic-pool-create-portal.md)
#### [C#](sql-database-elastic-pool-create-csharp.md)
#### [PowerShell](sql-database-elastic-pool-create-powershell.md)
### Veritabanları
#### Tek veritabanları
##### [Azure Portal](sql-database-get-started.md)
##### [C#](sql-database-get-started-csharp.md)
##### [T-SQL](sql-database-manage-azure-ssms.md#create-and-manage-azure-sql-databases)
##### [PowerShell](sql-database-get-started-powershell.md)
#### [SQL Server Management Studio](sql-database-manage-azure-ssms.md#create-and-manage-azure-sql-databases)
#### Parçalı veritabanları
##### [Parça eşleme yöneticisini kullanma](sql-database-elastic-scale-shard-map-management.md)
##### [Güvenlik yapılandırması birleştir/böl](sql-database-elastic-scale-split-merge-security-configuration.md)
##### [Dapper ile çalışma](sql-database-elastic-scale-working-with-dapper.md)
##### [Entity framework kullanma](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
##### [Verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md)
##### [Çok kiracılı satır düzeyi güvenliği](sql-database-elastic-tools-multi-tenant-row-level-security.md)
### Güvenlik duvarı kuralları
#### Sunucu
##### [Azure Portal](sql-database-configure-firewall-settings.md)
##### [PowerShell](sql-database-configure-firewall-settings-powershell.md)
##### [REST API](sql-database-configure-firewall-settings-rest.md)
##### [T-SQL](sql-database-configure-firewall-settings-tsql.md#server-level-firewall-rules)
#### Veritabanı
##### [T-SQL](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules)
### İşler
#### [Hizmet yükleme](sql-database-elastic-jobs-service-installation.md)
#### [Azure Portal](sql-database-elastic-jobs-create-and-manage.md)
#### [PowerShell](sql-database-elastic-jobs-powershell.md)
### Oturum açma bilgileri
#### [T-SQL](sql-database-manage-azure-ssms.md#create-and-manage-logins.md)
## Geliştirme
### [Genel Bakış](https://msdn.microsoft.com/library/mt763826.aspx)
### Senaryolar
#### [Çok müşterili SaaS uygulamaları](sql-database-design-patterns-multi-tenancy-saas-applications.md)
#### Zamana bağlı tablolar
##### [Zamana bağlı tablolar](sql-database-temporal-tables.md)
##### [Elde tutma ilkeleri](sql-database-temporal-tables-retention-policy.md)
#### [JSON verileri](sql-database-json-features.md)
#### [Bellek içi](sql-database-in-memory.md)
###Başlarken
#### [Bağlantı kitaplıkları](sql-database-libraries.md)
#### Uygulamayı bağlama
##### [.NET](sql-database-develop-dotnet-simple.md)
##### [Java](sql-database-develop-java-simple.md)
##### [Node.js](sql-database-develop-nodejs-simple.md)
##### [PHP](sql-database-develop-php-simple.md)
##### [Python](sql-database-develop-python-simple.md)
##### [Ruby](sql-database-develop-ruby-simple.md)
##### [Excel](sql-database-connect-excel.md)
#### [Visual Studio ile bağlanma](sql-database-connect-query.md)
### Nasıl yapılır?
#### Sunucu oluşturma
##### [PowerShell](sql-database-get-started-powershell.md)
##### [C#](sql-database-get-started-csharp.md)
##### Esnek havuz oluşturma
###### [PowerShell](sql-database-elastic-pool-create-powershell.md)
###### [C#](sql-database-elastic-pool-create-csharp.md)
#### Veritabanları oluşturma
##### [PowerShell](sql-database-get-started-powershell.md)
##### [C#](sql-database-get-started-csharp.md)
##### Güvenlik duvarı kuralları oluşturma
###### [PowerShell](sql-database-configure-firewall-settings-powershell.md)
###### [REST API](sql-database-configure-firewall-settings-rest.md)
#### Sunucu, havuz, veritabanı ve güvenlik duvarı yönetme
##### [PowerShell](sql-database-manage-powershell.md)
##### Esnek havuzları yönetme
###### [PowerShell](sql-database-elastic-pool-manage-powershell.md)
###### [C#](sql-database-elastic-pool-manage-csharp.md)
##### [Hizmet katmanlarını ve performans düzeylerini değiştirme](sql-database-scale-up-powershell.md)
#### Veri taşıma
##### [Veritabanını BACPAC dosyasına aktarma](sql-database-export-powershell.md)
##### [Veritabanını BACPAC dosyasından alma](sql-database-import-powershell.md)
##### [Bir veritabanını başka bir Azure konumuna kopyalama](sql-database-copy-powershell.md)
#### [Uygulamada kimlik doğrulaması için gerekli değerleri alma](sql-database-client-id-keys.md)
#### [Esnek işler](sql-database-elastic-jobs-powershell.md)
#### Veritabanını geri yükleme ve kurtarma
##### Silinen veritabanını geri yükleme
###### [PowerShell](sql-database-restore-deleted-database-powershell.md)
##### Zaman noktasında veritabanını geri yükleme
###### [PowerShell](sql-database-point-in-time-restore-powershell.md)
##### Coğrafi Geri Yükleme
###### [PowerShell](sql-database-geo-restore-powershell.md)
#### Etkin Coğrafi Çoğaltma ile verileri çoğaltma
##### Yapılandırma
###### [PowerShell](sql-database-geo-replication-powershell.md)
##### Yük devretme
###### [PowerShell](sql-database-geo-replication-failover-powershell.md)
#### [ADO.NET 4.5 için 1433’ten sonraki bağlantı noktalarını kullanma](sql-database-develop-direct-route-ports-adonet-v12.md)
#### [Hata iletileriyle çalışma](sql-database-develop-error-messages.md)
#### [Toplu işlem kullanma](sql-database-use-batching-to-improve-performance.md)
### Başvuru
#### [Transact-SQL](https://msdn.microsoft.com/library/azure/bb510741.aspx)
#### [SQL Server için .NET Framework Veri Sağlayıcısı (kavramlar)](https://msdn.microsoft.com/library/kb9s9ks0.aspx)
#### [SQL Server için .NET Framework Veri Sağlayıcısı (API Başvurusu)](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)
#### SQL PowerShell
##### [Azure SQL Veritabanı Cmdlet’leri (Kaynak Yönetimi)](https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
##### [Azure SQL Veritabanı Cmdlet’leri (Hizmet Yönetimi)](https://msdn.microsoft.com/library/azure/dn546723(v=azure.300\).aspx)
##### [SQL Server Cmdlet'leri](https://msdn.microsoft.com/library/mt740629.aspx)
#### SQL Veritabanı REST API'si
##### [REST API’si (Kaynak Yönetimi)](https://msdn.microsoft.com/library/azure/mt420159)
##### [REST API’si (Hizmet Yönetimi)](https://msdn.microsoft.com/library/azure/dn505719.aspx)
#### SQL Veritabanı Yönetim Kitaplığı
##### [SQL Veritabanı Yönetim Kitaplığı Başvurusu](https://msdn.microsoft.com/library/azure/mt349017.aspx)
##### [SQL Veritabanı Yönetim Kitaplığı paketi](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)
#### Entity Framework
##### [Entity Framework paketini alma](https://www.nuget.org/packages/EntityFramework/)
#### [SQL Server Sürücüleri](https://msdn.microsoft.com/library/mt654049.aspx)
##### [ADO.NET](https://msdn.microsoft.com/library/mt657768.aspx)
##### [JDBC](https://msdn.microsoft.com/library/mt484311.aspx)
##### [Node.js](https://msdn.microsoft.com/library/mt652093.aspx)
##### [ODBC](https://msdn.microsoft.com/library/mt654048.aspx)
##### [PHP](https://msdn.microsoft.com/library/dn865013.aspx)
##### [Python](https://msdn.microsoft.com/library/mt652092.aspx)
##### [Ruby](https://msdn.microsoft.com/library/mt691981.aspx)
#### [Azure SDK (indirme)](https://www.visualstudio.com/vs/azure-tools/)
#### [Azure SDK (belgeler)](https://azure.microsoft.com/documentation/articles/dotnet-sdk/)
### Kaynaklar
#### [SQL Server Araçları](https://msdn.microsoft.com/library/mt238365.aspx)
#### [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
#### [SQL Server Veri Araçları (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
#### [BCP](https://msdn.microsoft.com/library/ms162802.aspx)
#### [SQLCMD](https://msdn.microsoft.com/library/ms162773.aspx)
#### [SqlPackage](https://msdn.microsoft.com/hh550080.aspx)
## Sil
### Veritabanı
#### [PowerShell](sql-database-manage-powershell.md#delete-a-sql-database)
### Sunucu
#### [PowerShell](sql-database-manage-powershell.md#delete-a-sql-database-server)
## Tehditleri algılama
### [Tehdit algılama](sql-database-threat-detection-get-started.md)
### [Güvenlik duvarı](sql-database-firewall-configure.md)
## Verileri şifreleme
### Always encrypted
#### [Always encrypted’e genel bakış](sql-database-always-encrypted.md)
#### [Always encrypted Azure anahtar kasası](sql-database-always-encrypted-azure-key-vault.md)
### [Saydam veri şifrelemesi](https://msdn.microsoft.com/library/azure/dn948096)
### [Sütun şifreleme](https://msdn.microsoft.com/library/azure/ms179331)
## Yönet
###  Kimlik Doğrulaması
#### SQL kimlik doğrulaması
#### [Azure Active Directory kimlik doğrulaması](sql-database-aad-authentication.md)
#### [Multi-Factor Authentication](sql-database-ssms-mfa-authentication.md)
### Sunucular
### Esnek havuzlar
#### [Azure Portal](sql-database-elastic-pool-manage-portal.md)
#### [PowerShell](sql-database-elastic-pool-manage-powershell.md)
#### [T-SQL](sql-database-elastic-pool-manage-tsql.md)
#### [C#](sql-database-elastic-pool-manage-csharp.md)
### Veritabanları
#### Tek veritabanları
##### [Azure Portal](sql-database-manage-portal.md)
##### [T-SQL](sql-database-manage-azure-ssms.md#create-and-manage-azure-sql-databases)
##### [PowerShell](sql-database-manage-powershell.md#create-a-sql-database-blank)
#### Hizmet katmanlarını ve performans düzeylerini değiştirme
#### [Azure [portal](sql-database-scale-up.md)
#### [PowerShell](sql-database-manage-powershell.md#change-the-performance-level-of-a-sql-database)
#### Parçalı veritabanları
##### [Ölçeği genişletilen mevcut veritabanlarını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md)
##### [Kimlik bilgilerini yönetme](sql-database-elastic-scale-manage-credentials.md)
##### [Ölçeği genişletilen bulut veritabanları arasında veri taşıma](sql-database-elastic-scale-overview-split-and-merge.md)
##### [Ayırma-birleştirme hizmetini dağıtma](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
##### [Parça ekleme](sql-database-elastic-scale-add-a-shard.md)
##### [RecoveryManager sınıfı ile parça eşleme sorunlarını düzeltme](sql-database-elastic-database-recovery-manager.md)
### Güvenlik duvarı kuralları
#### Sunucu
##### [Azure Portal](sql-database-configure-firewall-settings.md)
##### [PowerShell](sql-database-configure-firewall-settings-powershell.md)
##### [REST API](sql-database-configure-firewall-settings-rest.md)
##### [T-SQL](sql-database-configure-firewall-settings-tsql.md#server-level-firewall-rules)
#### Veritabanı
##### [T-SQL](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules)
### İşler
#### [Hizmet yükleme](sql-database-elastic-jobs-service-installation.md)
#### [Azure Portal](sql-database-elastic-jobs-create-and-manage.md)
#### [PowerShell](sql-database-elastic-jobs-powershell.md)
#### [İstemci kitaplığını yükseltme](sql-database-elastic-scale-upgrade-client-library.md)
### Oturum açma bilgileri
#### [Oturum açmaları yönetme](sql-database-manage-logins.md)
## Verileri maskeleme
### Dinamik veri maskeleme
#### [Azure Portal](sql-database-dynamic-data-masking-get-started-portal.md)
#### [Alt düzey istemciler](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)
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
### Veritabanını BACPAC dosyasına aktarma
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
#### [SQL Paketi yardımcı programı](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
#### [PowerShell](sql-database-export-powershell.md)
### Veritabanını BACPAC dosyasından alma
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
#### [SQL Paketi yardımcı programı](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
#### [Azure Portal](sql-database-import.md)
#### [PowerShell](sql-database-import-powershell.md)

## İzleme ve ayarlama
### [Tek veritabanları](sql-database-performance-guidance.md)
### Esnek havuzlar
#### [Azure Portal](sql-database-elastic-pool-manage-portal.md)
#### [PowerShell](sql-database-elastic-pool-manage-powershell.md)
#### [T-SQL](sql-database-elastic-pool-manage-tsql.md)
#### [C#](sql-database-elastic-pool-manage-csharp.md)
### [Sorgu Performansı Öngörüleri](sql-database-query-performance.md)
### SQL Veritabanı Danışmanı
#### [Azure Portal](sql-database-advisor-portal.md)
### Hizmet katmanlarını ve performans düzeylerini değiştirme
#### [Azure Portal](sql-database-scale-up.md)
#### [PowerShell](sql-database-scale-up-powershell.md)
### [Performans ayarlama ipuçları](sql-database-troubleshoot-performance.md)
### Bellek içi OLTP
#### [Bellek içi OLTP’yi benimseme](sql-database-in-memory-oltp-migration.md)
#### [Bellek içi OLTP Depolama Alanını izleme](sql-database-in-memory-oltp-monitoring.md)
### Sorgu Deposu
#### [Sorgu Deposu kullanarak performans izleme](https://msdn.microsoft.com/library/dn817826.aspx)
#### [Sorgu Deposu kullanım senaryoları](https://msdn.microsoft.com/library/mt614796.aspx)
#### [Sorgu Deposunu çalıştırma](sql-database-operate-query-store.md)
### [Uyumluluk düzeyleri](sql-database-compatibility-level-query-performance-130.md)
### [Olay denetimi](sql-database-auditing-get-started.md)
### [Parça eşleme yöneticisi için performans sayaçları](sql-database-elastic-database-perf-counters.md)
### Genişletilmiş olaylar
#### [Genişletilmiş olaylar](sql-database-xevent-db-diff-from-svr.md)
#### [Olay dosyası hedef kodu](sql-database-xevent-code-event-file.md)
#### [Halka arabelleği hedef kodu](sql-database-xevent-code-ring-buffer.md)
### DMV’ler
#### [DMV’ler](sql-database-monitoring-with-dmvs.md)
#### [DMV’ler](sql-database-manage-azure-ssms#monitor-sql-database-using-dynamic-management-views


## Veri taşıma
### SQL veritabanı kopyalama
#### [Genel Bakış](sql-database-copy.md)
#### [Azure Portal](sql-database-copy-portal.md)
#### [PowerShell](sql-database-copy-powershell.md)
#### [T-SQL](sql-database-copy-transact-sql.md)
### Veritabanını BACPAC dosyasına aktarma
#### [Azure Portal](sql-database-export.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
#### [PowerShell](sql-database-export-powershell.md)
### Veritabanını BACPAC dosyasından alma
#### [Azure Portal](sql-database-import.md)
#### [PowerShell](sql-database-import-powershell.md)
### [Veri eşitleme](sql-database-get-started-sql-data-sync.md)
### [CSV dosyasından BCP kullanarak yükleme](sql-database-load-from-csv-with-bcp.md)

## Sorgu
### [SQL Server Management Studio](sql-database-connect-query-ssms.md)
### [Çok parçalı sorgulama](sql-database-elastic-scale-multishard-querying.md)
### Çapraz veritabanı sorguları
#### [Farklı şemalarla çapraz veritabanı sorgulama](sql-database-elastic-query-vertical-partitioning.md)
#### [Çapraz veritabanı raporlama](sql-database-elastic-query-horizontal-partitioning.md)
#### [Bulut veritabanlarında dağıtılmış işlemler](sql-database-elastic-transactions-overview.md)
#### [İstemci kitaplığını yükseltme](sql-database-elastic-scale-upgrade-client-library.md)
#### [Çok parçalı sorgulama](sql-database-elastic-scale-multishard-querying.md)

## Geri Yükleme
### Silinen veritabanını geri yükleme
### [Azure Portal](sql-database-restore-deleted-database-portal.md)
### [PowerShell](sql-database-restore-deleted-database-powershell.md)
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
### Bağlantı
#### [Bağlantı sorunları](sql-database-troubleshoot-common-connection-issues.md)
#### [Geçici bağlantı hatası](sql-database-troubleshoot-connection.md)
#### [Tanılama ve engelleme](sql-database-connectivity-issues.md)
### [İzinler](sql-database-troubleshoot-permissions.md)

# Başvuru
## [T-SQL](https://msdn.microsoft.com/library/azure/bb510741.aspx)
## SQL PowerShell
### [Azure SQL Veritabanı Cmdlet’leri (Kaynak Yönetimi)](https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
### [SQL Server Cmdlet'leri](https://msdn.microsoft.com/library/mt740629.aspx)
## SQL Veritabanı REST API'si
### [REST API’si (Kaynak Yönetimi)](https://msdn.microsoft.com/library/azure/mt420159)
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
## [SQL Server Araçları](https://msdn.microsoft.com/library/mt238365.aspx)
## [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
## [SQL Server Veri Araçları (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
## [BCP](https://msdn.microsoft.com/library/ms162802.aspx)
## [SQLCMD](https://msdn.microsoft.com/library/ms162773.aspx)
## [SqlPackage](https://msdn.microsoft.com/hh550080.aspx)

<!--HONumber=Nov16_HO2-->


