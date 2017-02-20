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
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/01/2017
ms.author: carlrab; jognanay
translationtype: Human Translation
ms.sourcegitcommit: 21be71a1b4c79ecec8af02d08f65c41128c5ef73
ms.openlocfilehash: 50a465f314909c10bc3c3f95be2d9dc377d433a7


---
# <a name="azure-sql-database-features"></a>Azure SQL Veritabanı özellikleri
Bu konu başlığında, Azure SQL Veritabanı mantıksal sunucuları ve veritabanları için bir genel bakış sunulmakta ve her özellik için bağlantılara yer verilen bir özellik destek tablosuna yer verilmektedir. 

## <a name="what-is-an-azure-sql-database-logical-server"></a>Azure SQL Veritabanı mantıksal sunucusu nedir?
Azure SQL Veritabanı mantıksal sunucusu birden fazla veritabanı için merkezi bir yönetim noktası olarak görev yapar. SQL Veritabanı'nda bir sunucu, şirket içi ortamda alışkın olabileceğiniz bir SQL Server örneğinden farklı olan mantıksal bir yapıdır. SQL Veritabanı hizmeti, veritabanlarının mantıksal sunuculara göre konumuyla ilgili özel bir garanti vermez ve örnek düzeyinde erişim ya da özellik sunmaz. Azure SQL mantıksal sunucuları hakkında daha fazla bilgi edinmek için bkz. [Mantıksal sunucular](sql-database-server-overview.md). 

## <a name="what-is-an-azure-sql-database"></a>Azure SQL veritabanı nedir?
Azure SQL Veritabanı’ndaki her bir veritabanı bir mantıksal sunucuyla ilişkilidir. Veritabanları aşağıdaki biçimlerde olabilir:

- [Kendi kaynak kümesine](sql-database-what-is-a-dtu.md#what-are-database-transaction-units-dtus) (DTU) sahip tek veritabanı
- [Bir kaynak kümesini (eDTU) paylaşan](sql-database-what-is-a-dtu.md#what-are-elastic-database-transaction-units-edtus) bir [veritabanı havuzunun](sql-database-elastic-pool.md) bir parçası
- Tek başına veya havuza eklenmiş veritabanlarından oluşan [parçalı veritabanlarından oluşan ölçeği genişletilmiş kümenin](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) bir parçası
- [Çok kiracılı SaaS tasarım desenine](sql-database-design-patterns-multi-tenancy-saas-applications.md) katılan ve veritabanları tek başına ya da havuza eklenmiş (veya her ikisi de) olabilecek bir veritabanı kümesinin bir parçası 

Azure SQL veritabanları hakkında daha fazla bilgi için bkz. [SQL veritabanları](sql-database-overview.md).

## <a name="what-features-are-supported"></a>Hangi özellikler desteklenir?

Aşağıdaki tablolarda Azure SQL Veritabanı ve SQL Server'ın temel özelliklerinin yanı sıra destek durumu belirtilmekte ve her platformda özellikle ilgili ayrıntılı bilgilerin bulunduğu sayfaya bağlantı verilmektedir. Transact-SQL özellikleri için tablodaki özellik kategorisine ait bağlantıya tıklayın. Belirli türlerdeki özelliklere yönelik destek olmayışı hakkında daha fazla arka plan bilgisi edinmek için ayrıca bkz. [Azure SQL Veritabanı Transact-SQL farkları](sql-database-transact-sql-information.md).

V12'ye özellik eklemeye devam ediyoruz. Bu nedenle Azure için Hizmet Güncelleştirmeleri web sayfamızı ziyaret etmenizi ve oradaki filtreleri kullanmanızı öneriyoruz:

* [SQL Veritabanı hizmeti](https://azure.microsoft.com/updates/?service=sql-database) için filtrelenmiş.
* SQL Veritabanı özellikleri için Genel Kullanılabilirlik [(GA) duyuruları](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability).

> [!TIP]
> Var olan bir veritabanının Azure SQL Veritabanı ile uyumlu olup olmadığını test etmek için bkz. [SQL Veritabanını Azure’a geçirme](sql-database-cloud-migrate.md).
>

| **Özellik** | **SQL Server** | **Azure SQL Veritabanı** | 
| --- | :---: | :---: | 
| Etkin Coğrafi Çoğaltma | Desteklenmiyor - Bkz. [AlwaysOn Kullanılabilirlik Grupları](https://msdn.microsoft.com/library/hh510230.aspx) | [Destekleniyor](sql-database-geo-replication-overview.md)
| Always Encrypted | [Destekleniyor](https://msdn.microsoft.com/library/mt163865.aspx) | [Destekleniyor](sql-database-always-encrypted.md) |
| AlwaysOn Kullanılabilirlik Grupları | [Destekleniyor](https://msdn.microsoft.com/library/hh510230.aspx) | Desteklenmiyor - Bkz. [Etkin Coğrafi Çoğaltma](sql-database-geo-replication-overview.md) |
| Veritabanı ekleme | [Destekleniyor](https://msdn.microsoft.com/library/ms190209.aspx) | Desteklenmiyor |
| Uygulama rolleri | [Destekleniyor](https://msdn.microsoft.com/library/ms190998.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/ms190998.aspx) |
| Otomatik ölçeklendirme | Desteklenmiyor | [Destekleniyor](sql-database-scale-up.md) |
| Azure Active Directory | Desteklenmiyor | [Destekleniyor](sql-database-aad-authentication.md) |
| Azure Data Factory | Desteklenmiyor - Bkz. [SQL Server Integration Services (SSIS)](https://msdn.microsoft.com/library/ms141026.aspx) | [Destekleniyor](https://azure.microsoft.com/services/data-factory/) |
| Denetim | [Destekleniyor](https://msdn.microsoft.com/library/cc280386.aspx) | [Destekleniyor](sql-database-auditing-get-started.md) |
| BACPAC dosyası (dışarı aktarma) | [Destekleniyor](https://msdn.microsoft.com/library/hh213241.aspx) | [Destekleniyor](sql-database-export.md) |
| BACPAC dosyası (içeri aktarma) | [Destekleniyor](https://msdn.microsoft.com/library/hh710052.aspx) | [Destekleniyor](sql-database-import.md) |
| BACKUP ve RESTORE deyimleri | [Destekleniyor](https://msdn.microsoft.com/library/ff848768.aspx) | Desteklenmiyor |
| Yerleşik işlevler | [Destekleniyor](https://msdn.microsoft.com/library/ms174318.aspx) | [Çoğu](https://msdn.microsoft.com/library/ms174318.aspx) |
| Değişiklik verilerini yakalama | [Destekleniyor](https://msdn.microsoft.com/library/cc645937.aspx) | Desteklenmiyor |
| Değişiklik izleme | [Destekleniyor](https://msdn.microsoft.com/library/bb933875.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/bb933875.aspx) |
| Harmanlama deyimleri | [Destekleniyor](https://msdn.microsoft.com/library/ff848763.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/ff848763.aspx) |
| Columnstore dizinleri | [Destekleniyor](https://msdn.microsoft.com/library/gg492088.aspx) | [Yalnızca Premium sürüm](https://msdn.microsoft.com/library/gg492088.aspx) |
| Ortak dil çalışma zamanı (CLR) | [Destekleniyor](https://msdn.microsoft.com/library/ms131102.aspx) | Desteklenmiyor |
| Bağımsız veritabanları | [Destekleniyor](https://msdn.microsoft.com/library/ff929071.aspx) | Yerleşik |
| Bağımsız kullanıcılar | [Destekleniyor](https://msdn.microsoft.com/library/ff929188.aspx) | [Destekleniyor](sql-database-manage-logins.md#non-administrator-users) |
| Akışı dil anahtar sözcükleri denetimi | [Destekleniyor](https://msdn.microsoft.com/library/ms174290.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/ms174290.aspx) |
| Veritabanları arası sorgular | [Destekleniyor](https://msdn.microsoft.com/library/dn584627.aspx) | [Elastik sorgular](sql-database-elastic-query-overview.md) |
| İmleçler | [Destekleniyor](https://msdn.microsoft.com/library/ms181441.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/ms181441.aspx) | 
| Veri sıkıştırma | [Destekleniyor](https://msdn.microsoft.com/library/cc280449.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/cc280449.aspx) |
| Veritabanı yedeklemeleri | [Kullanıcılara görünen](https://msdn.microsoft.com/library/ms187048.aspx) | [Yerleşik](sql-database-automated-backups.md) |
| Veritabanı posta | [Destekleniyor](https://msdn.microsoft.com/library/ms189635.aspx) | Desteklenmiyor |
| Veritabanı yansıtma | [Destekleniyor](https://msdn.microsoft.com/library/ms189852.aspx) | Desteklenmiyor |
| Veritabanı yapılandırma seçenekleri | [Destekleniyor](https://msdn.microsoft.com/library/mt629158.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/mt629158.aspx) |
| Data Quality Services (DQS) | [Destekleniyor](https://msdn.microsoft.com/library/ff877925.aspx) | Desteklenmiyor |
| Veritabanı anlık görüntüleri | [Destekleniyor](https://msdn.microsoft.com/library/ms175158.aspx) | Desteklenmiyor |
| Veri türleri | [Destekleniyor](https://msdn.microsoft.com/library/ms187752.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/ms187752.aspx) |  
| DBCC deyimleri | [Tümü](https://msdn.microsoft.com/library/ms188796.aspx) | [Bazıları](https://msdn.microsoft.com/library/ms188796.aspx) |
| DDL deyimleri | [Destekleniyor](https://msdn.microsoft.com/library/ff848799.aspx) | [Çoğu](https://msdn.microsoft.com/library/ff848799.aspx)
| DDL tetikleyicileri | [Destekleniyor](https://msdn.microsoft.com/library/ms175941.aspx) | [Yalnızca veritabanı](https://msdn.microsoft.com/library/ms175941.aspx) |
| Dağıtılmış işlemler | [MS DTC](https://msdn.microsoft.com/library/ms131665.aspx) | Yalnızca sınırlı SQL Veritabanı içi senaryolar |
| DML deyimleri | [Destekleniyor](https://msdn.microsoft.com/library/ff848766.aspx) | [Çoğu](https://msdn.microsoft.com/library/ff848766.aspx) |
| DML tetikleyicileri | [Destekleniyor](https://msdn.microsoft.com/library/ms178110.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/ms178110.aspx) |
| DMV’ler | [Tümü](https://msdn.microsoft.com/library/ms188754.aspx) | [Bazıları](https://msdn.microsoft.com/library/ms188754.aspx) |
| elastik havuzlar | Desteklenmiyor | [Destekleniyor](sql-database-elastic-pool.md) |
| Esnek işler | Desteklenmiyor - Bkz. [SQL Server Agent](https://msdn.microsoft.com/library/ms189237.aspx) | [Destekleniyor](sql-database-elastic-jobs-getting-started.md) | 
| Esnek sorgular | Desteklenmiyor - Bkz. [Veritabanları arası sorgular](https://msdn.microsoft.com/library/dn584627.aspx) | [Destekleniyor](sql-database-elastic-query-overview.md) |
| Olay bildirimleri | [Destekleniyor](https://msdn.microsoft.com/library/ms186376.aspx) | [Destekleniyor](sql-database-insights-alerts-portal.md) |
| İfadeler | [Destekleniyor](https://msdn.microsoft.com/library/ms190286.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/ms190286.aspx) |
| Genişletilmiş olaylar | [Destekleniyor](https://msdn.microsoft.com/library/bb630282.aspx) | [Bazıları](sql-database-xevent-db-diff-from-svr.md) |
| Genişletilmiş saklı yordamlar | [Destekleniyor](https://msdn.microsoft.com/library/ms164627.aspx) | Desteklenmiyor |
| Dosya grupları | [Destekleniyor](https://msdn.microsoft.com/library/ms189563.aspx#Anchor_2) | [Yalnızca birincil](https://msdn.microsoft.com/library/ms189563.aspx#Anchor_2) |
| Dosya akışı | [Destekleniyor](https://msdn.microsoft.com/library/gg471497.aspx) | Desteklenmiyor |
| Tam metin arama | [Destekleniyor](https://msdn.microsoft.com/library/ms142571.aspx) | [Üçüncü taraf sözcük ayırıcılar desteklenmiyor](https://msdn.microsoft.com/library/ms142571.aspx) |
| İşlevler | [Destekleniyor](https://msdn.microsoft.com/library/ms174318.aspx) | [Çoğu](https://msdn.microsoft.com/library/ms174318.aspx) |
| Bellek içi iyileştirme | [Destekleniyor](https://msdn.microsoft.com/library/dn133186.aspx) | [Yalnızca Premium sürüm](https://msdn.microsoft.com/library/dn133186.aspx) |
| İşler | [SQL Server Agent](https://msdn.microsoft.com/library/ms189237.aspx) | [Destekleniyor](sql-database-elastic-jobs-getting-started.md) |
| JSON veri desteği | [Destekleniyor](https://msdn.microsoft.com/library/dn921897.aspx) | [Destekleniyor](sql-database-json-features.md) |
| Dil öğeleri | [Destekleniyor](https://msdn.microsoft.com/library/ff848807.aspx) | [Çoğu](https://msdn.microsoft.com/library/ff848807.aspx) |  
| Bağlı sunucular | [Destekleniyor](https://msdn.microsoft.com/library/ms188279.aspx) | Desteklenmiyor - Bkz. [Elastik sorgu](sql-database-elastic-query-horizontal-partitioning.md) |
| Günlük aktarma | [Destekleniyor](https://msdn.microsoft.com/library/ms187103.aspx) | Desteklenmiyor - Bkz. [Etkin Coğrafi Çoğaltma](sql-database-geo-replication-overview.md) |
| Yönetim komutları | [Destekleniyor](https://msdn.microsoft.com/library/ms190286.aspx)| [Desteklenmiyor](https://msdn.microsoft.com/library/ms190286.aspx) |
| Ana Veri Hizmetleri (AVH) | [Destekleniyor](https://msdn.microsoft.com/library/ff487003.aspx) | Desteklenmiyor |
| Toplu olarak içeri aktarmada en az günlük | [Destekleniyor](https://msdn.microsoft.com/library/ms190422.aspx) | Desteklenmiyor |
| Sistem verilerini değiştirme | [Destekleniyor](https://msdn.microsoft.com/library/ms178028.aspx) | Desteklenmiyor |
| Çevrimiçi dizin işlemleri | [Destekleniyor](https://msdn.microsoft.com/library/ms177442.aspx) | [İşlem boyutu hizmet katmanıyla sınırlıdır](https://msdn.microsoft.com/library/ms177442.aspx) |
| İşleçler | [Destekleniyor](https://msdn.microsoft.com/library/ms174986.aspx) | [Çoğu](https://msdn.microsoft.com/library/ms174986.aspx) |
| Zaman noktasında veritabanını geri yükleme | [Destekleniyor](https://msdn.microsoft.com/library/ms179451.aspx) | [Destekleniyor](sql-database-recovery-using-backups.md#point-in-time-restore) |
| Polybase | [Destekleniyor](https://msdn.microsoft.com/library/mt143171.aspx) | [Desteklenmiyor]
| İlke tabanlı yönetim | [Destekleniyor](https://msdn.microsoft.com/library/bb510667.aspx) | Desteklenmiyor |
| Koşullar | [Destekleniyor](https://msdn.microsoft.com/library/ms189523.aspx) | [Çoğu](https://msdn.microsoft.com/library/ms189523.aspx)
| Kaynak idarecisi | [Destekleniyor](https://msdn.microsoft.com/library/bb933866.aspx) | [Yerleşik](sql-database-service-tiers.md) |
| Veritabanını yedekten geri yükleme | [Destekleniyor](https://msdn.microsoft.com/library/ms187048.aspx#anchor_6) | [Yalnızca yerleşik yedeklerden](sql-database-recovery-using-backups.md) |
| Satır Düzeyi Güvenlik | [Destekleniyor](https://msdn.microsoft.com/library/dn765131.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/dn765131.aspx) |
| Güvenlik deyimleri | [Destekleniyor](https://msdn.microsoft.com/library/ff848791.aspx) | [Bazıları](https://msdn.microsoft.com/library/ff848791.aspx) |
| Anlamsal arama | [Destekleniyor](https://msdn.microsoft.com/library/gg492075.aspx) | Desteklenmiyor |
| Sıra numaraları | [Destekleniyor](https://msdn.microsoft.com/library/ff878058.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/ff878058.aspx) |
| Hizmet Aracısı | [Destekleniyor](https://msdn.microsoft.com/library/bb522893.aspx) | Desteklenmiyor |
| Sunucu yapılandırma seçenekleri | [Destekleniyor](https://msdn.microsoft.com/library/ms189631.aspx) | Desteklenmiyor - Bkz. [Veritabanı yapılandırma seçenekleri](https://msdn.microsoft.com/library/mt629158.aspx) |
| Küme deyimleri | [Destekleniyor](https://msdn.microsoft.com/library/ms190356.aspx) | [Çoğu](https://msdn.microsoft.com/library/ms190356.aspx) 
| Uzamsal | [Destekleniyor](https://msdn.microsoft.com/library/bb933790.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/bb933790.aspx) |
| SQL Server Agent | [Destekleniyor](https://msdn.microsoft.com/library/ms189237.aspx) | Desteklenmiyor - Bkz. [Elastik işler](sql-database-elastic-jobs-getting-started.md) |
| SQL Server Analysis Services (SSAS) | [Destekleniyor](https://msdn.microsoft.com/library/bb522607.aspx) | Desteklenmiyor - Bkz. [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| SQL Server Integration Services (SSIS) | [Destekleniyor](https://msdn.microsoft.com/library/ms141026.aspx) | Desteklenmiyor - Bkz. [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) |
| SQL Server PowerShell | [Destekleniyor](https://msdn.microsoft.com/library/hh245198.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/hh245198.aspx) |
| SQL Server Profiler | [Destekleniyor](https://msdn.microsoft.com/library/ms181091.aspx) | Desteklenmiyor - Bkz. [Genişletilmiş olaylar](https://msdn.microsoft.com/library/ms181091.aspx) |
| SQL Server Çoğaltma | [Destekleniyor](https://msdn.microsoft.com/library/ms151198.aspx) | [Yalnızca işlem ve anlık görüntü çoğaltma abonesi](sql-database-cloud-migrate-compatible-using-transactional-replication.md) |
| SQL Server Reporting Services (SSRS) | [Destekleniyor](https://msdn.microsoft.com/library/ms159106.aspx) | Desteklenmiyor |
| Saklı yordamlar | [Destekleniyor](https://msdn.microsoft.com/library/ms190782.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/ms190782.aspx) |
| Sistem saklı işlevleri | [Destekleniyor](https://msdn.microsoft.com/library/ff848780.aspx) | [Bazıları](https://msdn.microsoft.com/library/ff848780.aspx) |
| Sistem saklı yordamları | [Destekleniyor](https://msdn.microsoft.com/library/ms187961.aspx) | [Bazıları](https://msdn.microsoft.com/library/ms187961.aspx) |
| Sistem tabloları | [Destekleniyor](https://msdn.microsoft.com/library/ms179932.aspx) | [Bazıları](https://msdn.microsoft.com/library/ms179932.aspx) |
| Sistem görünümleri | [Destekleniyor](https://msdn.microsoft.com/library/ms177862.aspx) | [Bazıları](https://msdn.microsoft.com/library/ms177862.aspx)
| Tablo Bölümleme | [Destekleniyor](https://msdn.microsoft.com/library/ms190787.aspx) | [Yalnızca birincil dosya grubu](https://msdn.microsoft.com/library/ms190787.aspx) |
| Geçici tablolar | [Yerel ve genel](https://msdn.microsoft.com/library/ms174979.aspx#Anchor_4) | [Yalnızca yerel](https://msdn.microsoft.com/library/ms174979.aspx#Anchor_4) |
| Zamana bağlı tablolar | [Destekleniyor](https://msdn.microsoft.com/library/dn935015.aspx) | [Destekleniyor](sql-database-temporal-tables.md) |
| İşlem deyimleri | [Destekleniyor](https://msdn.microsoft.com/library/ms174377.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/ms174377.aspx) |
| Değişkenler | [Destekleniyor](https://msdn.microsoft.com/library/ff848809.aspx) | | [Destekleniyor](https://msdn.microsoft.com/library/ff848809.aspx) | 
| Saydam veri şifrelemesi (TDE)  | [Destekleniyor](https://msdn.microsoft.com/library/bb934049.aspx) | [Destekleniyor](https://msdn.microsoft.com/dn948096.aspx) |
| Windows Server Yük Devretme kümelemesi | [Destekleniyor](https://msdn.microsoft.com/library/hh270278.aspx) | Desteklenmiyor - Bkz. [Etkin Coğrafi Çoğaltma](sql-database-geo-replication-overview.md) |
| XML dizinleri | [Destekleniyor](http://msdn.microsoft.com/library/bb934097.aspx) | [Destekleniyor](http://msdn.microsoft.com/library/bb934097.aspx) |
| XML deyimleri | [Destekleniyor](https://msdn.microsoft.com/library/ff848798.aspx) | [Destekleniyor](https://msdn.microsoft.com/library/ff848798.aspx) |

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL Veritabanı hizmeti hakkında bilgi edinmek için bkz. [SQL Veritabanı nedir?](sql-database-technical-overview.md)
- Azure SQL mantıksal sunucularına yönelik bir genel bakış için bkz. [SQL Veritabanı mantıksal sunucusuna genel bakış](sql-database-server-overview.md)
- Azure SQL veritabanlarına genel bakış için bkz. [SQL Veritabanına genel bakış](sql-database-overview.md)
- Transact-SQL desteği ve farklılıkları hakkında bilgi edinmek için bkz. [Azure SQL Veritabanı Transact-SQL farklılıkları](sql-database-transact-sql-information.md).
- **Hizmet katmanınızı** temel alan özel kaynak kotaları ve sınırlamalar hakkında bilgi edinmek için. Hizmet katmanlarına genel bir bakış için bkz. [SQL Veritabanı hizmet katmanları](sql-database-service-tiers.md).
- Güvenlik özelliklerine genel bakış için bkz. [Azure SQL Veritabanı Güvenliğine Genel Bakış](sql-database-security-overview.md).
- Sürücü kullanılabilirliği ve SQL Veritabanı desteği hakkında bilgi edinmek için bkz. [SQL Veritabanı ve SQL Server için Bağlantı Kitaplıkları](sql-database-libraries.md).



<!--HONumber=Feb17_HO2-->


