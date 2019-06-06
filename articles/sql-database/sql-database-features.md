---
title: Azure SQL veritabanı özellik karşılaştırması | Microsoft Docs
description: Bu makalede, Azure SQL veritabanı farklı türdeki kullanılabilir olan özelliklerin SQL Server karşılaştırılmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: bonova, sstein
manager: craigg
ms.date: 05/10/2019
ms.openlocfilehash: 4d8d2fd9a7408bb77939c9a1c8fdd67251282f49
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479209"
---
# <a name="feature-comparison-azure-sql-database-versus-sql-server"></a>Özellik karşılaştırması: Azure SQL veritabanı SQL Server ile karşılaştırması

Azure SQL veritabanı ile SQL Server ortak bir kod temeli paylaşır. Azure SQL veritabanı tarafından desteklenen bir SQL Server özelliklerinin oluşturduğunuz Azure SQL veritabanı türüne bağlıdır. Azure SQL veritabanı ile bir parçası olarak bir veritabanı oluşturabilirsiniz bir [yönetilen örnek](sql-database-managed-instance.md), tek bir veritabanı veya elastik bir havuzun parçası olarak.

Microsoft Azure SQL veritabanı'na özellik eklemeye devam eder. Hizmet güncelleştirmeleri Web sayfası için Azure bu filtreleri kullanarak en son güncelleştirmeler için ziyaret edin:

- [SQL Veritabanı hizmeti](https://azure.microsoft.com/updates/?service=sql-database) için filtrelenmiş.
- SQL Veritabanı özellikleri için Genel Kullanılabilirlik [(GA) duyuruları](https://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability).

## <a name="sql-server-feature-support-in-azure-sql-database"></a>Azure SQL veritabanı'nda SQL Server özellik desteği

Aşağıdaki tabloda, SQL Server'ın temel özelliklerinin listeler ve bağlantı özelliği hakkında daha fazla bilgi için bu özellik kısmen veya tamamen desteklenir ve hakkında bilgi sağlar.

| **SQL özellik** | **Tek veritabanları ve elastik havuzlar tarafından desteklenmiyor** | **Yönetilen örnekleri tarafından desteklenir** |
| --- | --- | --- |
| [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) | Evet - tüm hiper ölçekli dışında katmanları hizmet | Hayır, bkz: [otomatik yük devretme groups(preview)](sql-database-auto-failover-group.md) alternatif olarak |
| [Otomatik yük devretme grupları](sql-database-auto-failover-group.md) | Evet - tüm hiper ölçekli dışında katmanları hizmet | Evet, içinde [genel önizlemeye sunuldu](sql-database-auto-failover-group.md)|
| [Her zaman şifreli](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Evet - bkz [sertifika deposu](sql-database-always-encrypted.md) ve [anahtar kasası](sql-database-always-encrypted-azure-key-vault.md) | Evet - bkz [sertifika deposu](sql-database-always-encrypted.md) ve [anahtar kasası](sql-database-always-encrypted-azure-key-vault.md) |
| [Always On kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | [Yüksek kullanılabilirlik](sql-database-high-availability.md) ile her veritabanı bulunur. Olağanüstü durum kurtarma ele alınmıştır [Azure SQL veritabanı ile iş sürekliliğine genel bakış](sql-database-business-continuity.md) | [Yüksek kullanılabilirlik](sql-database-high-availability.md) ile her veritabanı dahil edilir ve [kullanıcı tarafından yönetilen](sql-database-managed-instance-transact-sql-information.md#always-on-availability). Olağanüstü durum kurtarma ele alınmıştır [Azure SQL veritabanı ile iş sürekliliğine genel bakış](sql-database-business-continuity.md) |
| [Veritabanı ekleme](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Hayır | Hayır |
| [Uygulama rolleri](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Evet | Evet |
| [Denetim](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Evet](sql-database-auditing.md)| [Evet](sql-database-managed-instance-auditing.md), bazı [farkları](sql-database-managed-instance-transact-sql-information.md#auditing) |
| [Otomatik yedekleme](sql-database-automated-backups.md) | Evet. Tam yedeklemeler her 7 gün, fark 12 saat alınır ve her 5-10 dakikada bir günlük yedeklemelerine. | Evet. Tam yedeklemeler her 7 gün, fark 12 saat alınır ve her 5-10 dakikada bir günlük yedeklemelerine. |
| [Otomatik ayarlama (plan zorlama)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Evet](sql-database-automatic-tuning.md)| [Evet](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) |
| [Otomatik ayarlama (dizin)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Evet](sql-database-automatic-tuning.md)| Hayır |
| [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) | Evet | Evet |
| [BACPAC dosyası (dışarı aktarma)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Evet - bkz [SQL veritabanını dışarı aktarma](sql-database-export.md) | Evet - bkz [SQL veritabanını dışarı aktarma](sql-database-export.md) |
| [BACPAC dosyası (içeri aktarma)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Evet - bkz [SQL veritabanı içeri aktarma](sql-database-import.md) | Evet - bkz [SQL veritabanı içeri aktarma](sql-database-import.md) |
| [Yedekleme komutu](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Hayır, yalnızca sistem tarafından başlatılan otomatik yedeklemeler - bkz [otomatik yedeklemeler](sql-database-automated-backups.md) | Evet, Azure Blob depolama alanına (Otomatik Sistem yedekleri kullanıcı tarafından başlatılamaz) - yalnızca kopya yedekleri kullanıcı bakın başlatılan [fark yedekleme](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Yerleşik işlevler](https://docs.microsoft.com/sql/t-sql/functions/functions) | Çoğu - bkz ayrı İşlevler | Evet - bkz [saklı yordamlar, İşlevler, Tetikleyiciler farkları](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-and-triggers) | 
| [TOPLU ekleme deyimi](https://docs.microsoft.com/sql/relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server) | Evet, ancak henüz bir kaynak olarak Azure Blob depolamadan. | Evet, ancak yalnızca - kaynağı olarak Azure Blob depolama alanından bkz [farklar](sql-database-managed-instance-transact-sql-information.md#bulk-insert--openrowset). |
| [Sertifikalar ve asimetrik anahtarlar](https://docs.microsoft.com/sql/relational-databases/security/sql-server-certificates-and-asymmetric-keys) | Evet, dosya sistemi erişimi olmadan `BACKUP` ve `CREATE` operations. | Evet, dosya sistemi erişimi olmadan `BACKUP` ve `CREATE` işlemleri - görmek [sertifika farklar](sql-database-managed-instance-transact-sql-information.md#certificates). | 
| [Değişiklik verilerini yakalama](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Hayır | Evet |
| [Değişiklik izleme](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Evet |Evet |
| [Harmanlama - veritabanı](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation) | Evet | Evet |
| [Harmanlama - server/örneği](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation) | Hayır, varsayılan mantıksal sunucu harmanlama `SQL_Latin1_General_CP1_CI_AS` her zaman kullanılır. | Evet, ne zaman ayarlanabilir [örneği oluşturulduğu](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md) ve daha sonra güncelleştirilemiyor. |
| [Columnstore dizinleri](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Evet - [Premium katman, standart katman - S3 ve üstü, genel amaçlı katmanı ve iş açısından kritik katmanları](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |Evet |
| [Ortak dil çalışma zamanı (CLR)](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Hayır | Evet, ancak dosya sistemine erişim `CREATE ASSEMBLY` bildirimi - bkz [CLR farkları](sql-database-managed-instance-transact-sql-information.md#clr) |
| [Kapsanan veritabanları](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Evet | Şu anda yok [geri yükleme-belirli bir noktaya geri yükleme gibi hata nedeniyle](sql-database-managed-instance-transact-sql-information.md#cant-restore-contained-database). Yakında çözülecektir bir kusur budur. |
| [Bağımsız kullanıcılar](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Evet | Evet |
| [Denetim akışı dil anahtar sözcükleri](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Evet | Evet |
| [Kimlik Bilgileri](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/credentials-database-engine) | Evet, ancak yalnızca [veritabanı kapsamlı kimlik bilgilerini](https://docs.microsoft.com/sql/t-sql/statements/create-database-scoped-credential-transact-sql). | Evet, ancak yalnızca **Azure anahtar kasası** ve `SHARED ACCESS SIGNATURE` desteklenen bkz [ayrıntıları](sql-database-managed-instance-transact-sql-information.md#credential) |
| [Veritabanları arası sorgular](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Hayır - bkz [esnek sorgular](sql-database-elastic-query-overview.md) | Evet, artı [esnek sorgular](sql-database-elastic-query-overview.md) |
| [Veritabanları arası işlemler](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Hayır | Evet, örneği içinde. Bkz: [bağlı sunucu farklar](sql-database-managed-instance-transact-sql-information.md#linked-servers) arası örnek sorgular için. |
| [İmleçler](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Evet |Evet |
| [Veri sıkıştırma](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Evet |Evet |
| [Veritabanı posta](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Hayır | Evet |
| [Veri geçiş hizmeti (DMS)](https://docs.microsoft.com/sql/dma/dma-overview) | Evet | Evet |
| [Veritabanı yansıtma](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Hayır | [Hayır](sql-database-managed-instance-transact-sql-information.md#database-mirroring) |
| [Veritabanı yapılandırma ayarları](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Evet | Evet |
| [Veri Kalitesi Hizmetleri (DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Hayır | Hayır |
| [Veritabanı anlık görüntüleri](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Hayır | Hayır |
| [Veri türleri](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Evet |Evet |
| [DBCC deyimleri](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | Çoğu - bkz ayrı deyimler | Evet - bkz [DBCC farkları](sql-database-managed-instance-transact-sql-information.md#dbcc) |
| [DDL deyimleri](https://docs.microsoft.com/sql/t-sql/statements/statements) | Çoğu - bkz ayrı deyimler | Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md) |
| [DDL Tetikleyicileri](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Yalnızca veritabanı |  Evet |
| [Bölüm dağıtılmış görünümleri](https://docs.microsoft.com/sql/t-sql/statements/create-view-transact-sql#partitioned-views) | Hayır | Evet |
| [Dağıtılmış işlemler - MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Hayır - bkz [elastik işlemler](sql-database-elastic-transactions-overview.md) |  Hayır - bkz [bağlı sunucu farkları](sql-database-managed-instance-transact-sql-information.md#linked-servers) |
| [DML deyimleri](https://docs.microsoft.com/sql/t-sql/queries/queries) | Evet | Evet |
| [DML Tetikleyicileri](https://docs.microsoft.com/sql/relational-databases/triggers/create-dml-triggers) | Çoğu - bkz ayrı deyimler |  Evet |
| [DMV’ler](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Çoğu - bkz ayrı Dmv'ler |  Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md) |
| [Dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)|[Evet](sql-database-dynamic-data-masking-get-started.md)| [Evet](sql-database-dynamic-data-masking-get-started.md) |
| [Elastik havuzlar](sql-database-elastic-pool.md) | Evet | Yerleşik-tek bir yönetilen örnek aynı kaynak havuzu paylaşan birden çok veritabanına sahip olabilir |
| [Olay bildirimleri](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | Hayır - bkz [uyarıları](sql-database-insights-alerts-portal.md) | Hayır |
| [İfadeler](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Evet | Evet |
| [Genişletilmiş olaylar](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Bazıları - bkz [SQL veritabanı'nda olaylar genişletilmiş](sql-database-xevent-db-diff-from-svr.md) | Evet - bkz [genişletilmiş olaylar farkları](sql-database-managed-instance-transact-sql-information.md#extended-events) |
| [Genişletilmiş saklı yordamlar](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Hayır | Hayır |
| [Dosyalar ve dosya grupları](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Yalnızca birincil dosya grubu | Evet. Dosya yolları otomatik olarak atanır ve dosya konumunu belirtilemez `ALTER DATABASE ADD FILE` [deyimi](sql-database-managed-instance-transact-sql-information.md#alter-database-statement).  |
| [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Hayır | [Hayır](sql-database-managed-instance-transact-sql-information.md#filestream-and-filetable) |
| [Tam metin araması](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |  Evet, ancak üçüncü taraf sözcük ayırıcılar desteklenmez | Evet, ancak [üçüncü taraf sözcük ayırıcılar desteklenmez](sql-database-managed-instance-transact-sql-information.md#full-text-semantic-search) |
| [İşlevler](https://docs.microsoft.com/sql/t-sql/functions/functions) | Çoğu - bkz ayrı İşlevler | Evet - bkz [saklı yordamlar, İşlevler, Tetikleyiciler farkları](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-and-triggers) |
| [Coğrafi geri yükleme](sql-database-recovery-using-backups.md#geo-restore) | Evet - tüm hiper ölçekli dışında katmanları hizmet | Evet - kullanarak [Azure PowerShell](https://medium.com/azure-sqldb-managed-instance/geo-restore-your-databases-on-azure-sql-instances-1451480e90fa). |
| [Grafik işleme](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview) | Evet | Evet |
| [Bellek içi iyileştirme](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Evet - [yalnızca Premium ve iş açısından kritik katmanları](sql-database-in-memory.md) | Evet - [iş yalnızca kritik katmanı](sql-database-managed-instance.md) |
| [JSON veri desteği](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | [Evet](sql-database-json-features.md) | [Evet](sql-database-json-features.md) |
| [Dil öğeleri](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | Çoğu - bkz. ayrı ayrı öğeler |  Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md) |
| [Bağlı sunucular](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Hayır - bkz [esnek sorgu](sql-database-elastic-query-horizontal-partitioning.md) | Yalnızca [SQL Server ve SQL veritabanı](sql-database-managed-instance-transact-sql-information.md#linked-servers) |
| [Günlük aktarma](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | [Yüksek kullanılabilirlik](sql-database-high-availability.md) ile her veritabanı bulunur. Olağanüstü durum kurtarma ele alınmıştır [Azure SQL veritabanı ile iş sürekliliğine genel bakış](sql-database-business-continuity.md) | Yerel olarak yerleşik DMS geçiş işleminin bir parçası olarak. [Yüksek kullanılabilirlik](sql-database-high-availability.md) ile her veritabanı dahil edilir ve günlük aktarma HA alternatif olarak kullanmak için önerilmez. Olağanüstü durum kurtarma ele alınmıştır [Azure SQL veritabanı ile iş sürekliliğine genel bakış](sql-database-business-continuity.md) |
| [Oturum açma bilgileri ve kullanıcılar](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/principals-database-engine) | Evet, ancak `CREATE` ve `ALTER` oturum açma deyimleri tüm seçenekleri (hiçbir Windows ve sunucu düzeyinde Azure Active Directory oturum açma bilgileri) teklif değil. `EXECUTE AS LOGIN` olduğundan desteklenmiyor - kullanın `EXECUTE AS USER` yerine.  | Evet, bazı ile [farklar](sql-database-managed-instance-transact-sql-information.md#logins-and-users). Windows oturum açma bilgileri desteklenmez ve Azure Active Directory oturum açma bilgileri ile değiştirilmelidir. |
| [Uzun süreli yedek saklama - LTR](sql-database-long-term-retention.md) | Otomatik olarak alınan yedeklemeler 10 yıla Evet, takip edin. | Henüz değil. Kullanım `COPY_ONLY` [el ile yedeklemeler](sql-database-managed-instance-transact-sql-information.md#backup) geçici bir çözüm olarak. |
| [Ana Veri Hizmetleri (AVH)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Hayır | Hayır |
| [En az günlük toplu içeri aktarma](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Hayır | Hayır |
| [Sistem verilerini değiştirme](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Hayır | Evet |
| [OLE Otomasyonu nesnesi etkin](https://docs.microsoft.com/sql/database-engine/configure-windows/ole-automation-procedures-server-configuration-option) | Hayır | Hayır |
| [Çevrimiçi dizin işlemleri](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Evet | Evet |
| [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql)|Hayır|Evet, yalnızca diğer Azure SQL veritabanları ve SQL sunucuları için. Bkz: [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md)|
| [OPENJSON](https://docs.microsoft.com/sql/t-sql/functions/openjson-transact-sql)|Evet|Evet|
| [OPENQUERY](https://docs.microsoft.com/sql/t-sql/functions/openquery-transact-sql)|Hayır|Evet, yalnızca diğer Azure SQL veritabanları ve SQL sunucuları için. Bkz: [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md)|
| [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql)|Evet, yalnızca Azure Blob depolama alanından içeri aktarmak için kullanılır. |Evet, yalnızca diğer Azure SQL veritabanları ve SQL Server'lar ve Azure Blob depolama alanından içeri aktarın. Bkz: [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md)|
| [OPENXML](https://docs.microsoft.com/sql/t-sql/functions/openxml-transact-sql)|Evet|Evet|
| [İşleçler](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | Çoğu - bkz. tek tek işleçler |Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md) |
| [Bölümleme](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Evet | Evet |
| Genel IP adresi | Evet. Erişim, güvenlik duvarı ya da hizmet uç noktalarını kullanarak kısıtlanmış olabilir.  | Evet. Açıkça olması gereken etkin ve bağlantı noktası 3342 NSG kuralları etkinleştirilmiş olmalıdır. Genel IP gerekirse devre dışı bırakılabilir. Bkz: [genel uç nokta](sql-database-managed-instance-public-endpoint-securely.md) daha fazla ayrıntı için. | 
| [Zaman veritabanını geri yükleme noktası](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Evet - bkz. hiper ölçekli - dışındaki tüm hizmet katmanları [SQL veritabanını kurtarma](sql-database-recovery-using-backups.md#point-in-time-restore) | Evet - bkz [SQL veritabanını kurtarma](sql-database-recovery-using-backups.md#point-in-time-restore) |
| [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Hayır | Hayır |
| [İlke tabanlı yönetim](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Hayır | Hayır |
| [Doğrulamaları](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Evet | Evet |
| [Sorgu bildirimleri](https://docs.microsoft.com/sql/relational-databases/native-client/features/working-with-query-notifications) | Hayır | Evet |
| [Sorgu Performansı İçgörüleri](sql-database-query-performance.md) | Evet | Hayır |
| [R Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Evet, içinde [genel önizlemeye sunuldu](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services)  | Hayır |
| [Kaynak İdarecisi](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Hayır | Evet |
| [RESTORE deyimleri](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Hayır | Evet, zorunlu ile `FROM URL` yedekleme dosyaları için seçenekleri, Azure Blob Depolama alanında yerleştirilir. Bkz: [farklar geri yükleme](sql-database-managed-instance-transact-sql-information.md#restore-statement) |
| [Veritabanını yedekten geri yükleyin](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | Yalnızca - otomatik yedeklerden bkz [SQL veritabanını kurtarma](sql-database-recovery-using-backups.md) | Otomatik yedeklerden - bkz [SQL veritabanı kurtarma](sql-database-recovery-using-backups.md) ve Azure Blob Depolama alanında - yerleştirilen tam yedeklerden bkz [fark yedekleme](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Evet | Evet |
| [Anlamsal arama](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Hayır | Hayır |
| [Sıra numaraları](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Evet | Evet |
| [Hizmet Aracısı](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Hayır | Evet, ancak yalnızca içinde örneği. Bkz: [hizmet Aracısı farkları](sql-database-managed-instance-transact-sql-information.md#service-broker) |
| [Sunucu Yapılandırma ayarları](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Hayır | Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md) |
| [Küme deyimleri](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | Çoğu - bkz ayrı deyimler | Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md)|
| [SMO](https://docs.microsoft.com/sql/relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide) | [Evet](https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects) | Evet [sürümü 150](https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects) |
| [Uzamsal](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Evet | Evet |
| [SQL analizi](https://docs.microsoft.com/azure/azure-monitor/insights/azure-sql) | Evet | Evet |
| [SQL Data Sync](sql-database-get-started-sql-data-sync.md) | Evet | Hayır |
| [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Hayır - bkz [esnek işler](elastic-jobs-overview.md) | Evet - bkz [SQL Server Agent farkları](sql-database-managed-instance-transact-sql-information.md#sql-server-agent) |
| [SQL Server Analysis Services (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Hayır, [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) ayrı Azure bulut hizmetidir. | Hayır, [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) ayrı Azure bulut hizmetidir. |
| [SQL Server denetimi](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | Hayır - bkz [SQL veritabanı denetimi](sql-database-auditing.md) | Evet - bkz [farklar denetleme](sql-database-managed-instance-transact-sql-information.md#auditing) |
| [SQL Server Veri Araçları (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) | Evet | Evet |
| [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Evet, bir yönetilen SSIS paketleri depolandığı Azure SQL veritabanı tarafından barındırılan ve Azure SSIS tümleştirme çalışma zamanı (IR) yürütülen SSISDB içinde Azure Data Factory (ADF) ortamında görmek [ADF içinde Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>SQL veritabanı sunucusu ve yönetilen örneği SSIS özellikleri karşılaştırmak için bkz [karşılaştırma Azure SQL veritabanı tek veritabanlarını elastik havuzları ve yönetilen örneği](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). | Evet, bir yönetilen SSIS paketleri depolandığı yönetilen örneği tarafından barındırılan ve Azure SSIS tümleştirme çalışma zamanı (IR) yürütülen SSISDB içinde Azure Data Factory (ADF) ortamında görmek [ADF içinde Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>SQL veritabanı yönetilen örneği, SSIS özellikleri karşılaştırmak için bkz [karşılaştırma Azure SQL veritabanı tek veritabanlarını elastik havuzları ve yönetilen örneği](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). |
| [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) | Evet | Evet [18,0 ve daha yüksek sürüm](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) |
| [SQL Server PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Evet | Evet |
| [SQL Server Profiler](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Hayır - bkz [genişletilmiş olaylar](sql-database-xevent-db-diff-from-svr.md) | Evet |
| [SQL Server çoğaltma](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Yalnızca işlem ve anlık görüntü çoğaltma abonesi](sql-database-single-database-migrate.md) | Evet, içinde [genel önizlemeye sunuldu](https://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance) |
| [SQL Server Raporlama Hizmetleri (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Hayır - [Power BI bakın](https://docs.microsoft.com/power-bi/) | Hayır - [Power BI bakın](https://docs.microsoft.com/power-bi/) |
| [Saklı yordamlar](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Evet | Evet |
| [Sistem saklı işlevleri](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Çoğu - bkz ayrı İşlevler | Evet - bkz [saklı yordamlar, İşlevler, Tetikleyiciler farkları](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-and-triggers) |
| [Sistem saklı yordamları](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Bazıları - bkz bireysel saklı yordamlar | Evet - bkz [saklı yordamlar, İşlevler, Tetikleyiciler farkları](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-and-triggers) |
| [Sistem tabloları](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Bazıları - bkz tek tek tablolar | Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md) |
| [Sistem kataloğu görünümleri](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Bazıları - bkz. tek bir görünüm | Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md) |
| [Geçici tablolar](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#database-scoped-global-temporary-tables-azure-sql-database) | Yerel ve veritabanı kapsamlı genel geçici tablolar | Yerel ve örnek kapsamlı genel geçici tablolar |
| [Zamana bağlı tablolar](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | [Evet](sql-database-temporal-tables.md) | [Evet](sql-database-temporal-tables.md) |
| Saat dilimi seçim | Hayır | [Yes(Preview)](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-timezone) |
| Tehdit algılama|  [Evet](sql-database-threat-detection.md)|[Evet](sql-database-managed-instance-threat-detection.md)|
| [İzleme Bayrakları](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql) | Hayır | Hayır |
| [Değişkenler](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Evet | Evet |
| [Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Evet - yalnızca katmanları genel amaçlı ve iş açısından kritik hizmet| [Evet](transparent-data-encryption-azure-sql.md) |
| [VNet](../virtual-network/virtual-networks-overview.md) | Kısmi - bkz [VNet uç noktaları](sql-database-vnet-service-endpoint-rule-overview.md) | Evet, yalnızca Resource Manager modeli |
| [Windows Server Yük Devretme Kümelemesi](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | [Yüksek kullanılabilirlik](sql-database-high-availability.md) ile her veritabanı bulunur. Olağanüstü durum kurtarma ele alınmıştır [Azure SQL veritabanı ile iş sürekliliğine genel bakış](sql-database-business-continuity.md) | [Yüksek kullanılabilirlik](sql-database-high-availability.md) ile her veritabanı bulunur. Olağanüstü durum kurtarma ele alınmıştır [Azure SQL veritabanı ile iş sürekliliğine genel bakış](sql-database-business-continuity.md) |
| [XML dizinleri](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Evet | Evet |

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL Veritabanı hizmeti hakkında bilgi edinmek için bkz. [SQL Veritabanı nedir?](sql-database-technical-overview.md)
- Bir yönetilen örneği hakkında daha fazla bilgi için bkz: [yönetilen örnek nedir?](sql-database-managed-instance.md).
