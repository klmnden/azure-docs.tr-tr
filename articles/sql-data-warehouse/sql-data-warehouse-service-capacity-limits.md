---
title: Kapasite sınırları - Azure SQL veri ambarı | Microsoft Docs
description: Azure SQL veri ambarı çeşitli bileşenler için izin verilen en yüksek değerleri.
services: sql-data-warehouse
author: sachinpMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 11/14/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: b37f16ab914fe4062bc9720ae9cc0139c573fb93
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65154269"
---
# <a name="sql-data-warehouse-capacity-limits"></a>SQL Data Warehouse kapasite sınırları
Azure SQL veri ambarı çeşitli bileşenler için izin verilen en yüksek değerleri.

## <a name="workload-management"></a>İş yükü yönetimi
| Category | Açıklama | Maksimum |
|:--- |:--- |:--- |
| [Veri ambarı birimi (DWU)](what-is-a-data-warehouse-unit-dwu-cdwu.md) |Tek bir SQL veri ambarı için en fazla DWU | Gen1: DW6000<br></br>Gen2: DW30000c |
| [Veri ambarı birimi (DWU)](what-is-a-data-warehouse-unit-dwu-cdwu.md) |Sunucu başına varsayılan DTU |54,000<br></br>Varsayılan olarak, her bir SQL server (örn. myserver.database.windows.net) kadar DW6000c izin veren bir DTU kota olarak 54.000, vardır. Bu kota yalnızca bir güvenlik sınırıdır. Tarafından kotanızı artırabilirsiniz [bir destek bileti oluşturma](sql-data-warehouse-get-started-create-support-ticket.md) seçerek *kota* istek türü olarak.  DTU'yu hesaplama, DWU gereken toplam 7,5 ile çarpın veya 9.0 gereken toplam cDWU tarafından Çarp gerekir. Örneğin:<br></br>7.5 = 45,000 x DW6000 Dtu<br></br>DW6000c 9.0 = olarak 54.000 x dtu'ları.<br></br>SQL server seçeneği geçerli DTU tüketiminizi portalında görüntüleyebilirsiniz. DTU kotasında hem duraklatılmış hem de duraklatılmamış veritabanları sayılır. |
| Veritabanı bağlantısı |En fazla eşzamanlı oturum açın |1024<br/><br/>Eş zamanlı açık oturum sayısı, seçilen DWU göre değişir. DWU500c ve en çok 1024 desteği üzerinde oturum açın. DWU400c ve en fazla eş zamanlı açık oturum sınırı 512, aşağıdaki özellikleri destekler. Unutmayın, eşzamanlı olarak yürütebilir sorguların sayısına yönelik sınırlar vardır. Eşzamanlılık sınırı aşıldığında, istek bir iç kuyruğuna burada işlenmeyi bekleyen gider. |
| Veritabanı bağlantısı |Hazırlanmış deyimleri için en fazla belleği |20 MB |
| [İş yükü yönetimi](resource-classes-for-workload-management.md) |En fazla eş zamanlı sorguları |128<br/><br/> SQL veri ambarı, en fazla 128 eş zamanlı sorguları ve sorguları kalan kuyrukları yürütebilir.<br/><br/>Kullanıcılar daha yüksek kaynak sınıfları veya SQL veri ambarı düşük olduğunda atandığında eş zamanlı sorgu sayısını azaltabilir [veri ambarı birimi](memory-and-concurrency-limits.md) ayarı. DMV sorgu gibi bazı sorgular çalıştırın ve her zaman izin eş zamanlı sorgu sınırı etkisi yok. Eş zamanlı sorgu yürütme hakkında daha fazla bilgi için bkz. [eşzamanlılık sınırları](memory-and-concurrency-limits.md#concurrency-maximums) makalesi. |
| [tempdb](sql-data-warehouse-tables-temporary.md) |En fazla GB |Değeri DW100 399 GB. Bu nedenle DWU1000 sırasında tempdb 3,99 TB boyutlandırılır. |

## <a name="database-objects"></a>Veritabanı nesneleri
| Category | Açıklama | Maksimum |
|:--- |:--- |:--- |
| Database |Maksimum boyut | Gen1: 240 TB disk üzerinde sıkıştırılır. Bu alanı, tempdb veya günlük alanının bağımsızdır ve bu nedenle bu alanı kalıcı tablolara ayrılmış.  Kümelenmiş columnstore sıkıştırması, 5 X tahmin edilir.  Bu sıkıştırma sağlar veritabanına yaklaşık 1 büyütme tüm tablolarda kümelenmiş columnstore (varsayılan tablo türü) olduğunda PB. <br/><br/> Gen2: 240TB rowstore ve columnstore tabloları için sınırsız depolama |
| Tablo |Maksimum boyut |Diskte sıkıştırılmış 60 TB |
| Tablo |Her bir veritabanı tabloları | 100.000 |
| Tablo |Her tablo sütunları |1024 sütunları |
| Tablo |Sütun başına bayt |Sütun bağımlı [veri türü](sql-data-warehouse-tables-data-types.md). Karakter veri türleri için 8000 nvarchar için 4000 veya en fazla veri türleri için 2 GB sınırdır. |
| Tablo |Satır, tanımlanmış boyut başına bayt |Açıklama 8060 baytlık<br/><br/>SQL Server için sayfa sıkıştırmayı ile olduğu gibi aynı şekilde satır başına bayt sayısı hesaplanır. SQL Server gibi SQL veri ambarı sağlayan satır taşma depolama destekler **değişken uzunluğu sütununa** satır dışı gönderilecek. Değişken uzunluklu satır satır dışı itildiğinde yalnızca 24 bayt kök ana kayıt içinde depolanır. Daha fazla bilgi için [veri satırı taşma aşan 8 KB'lık](https://msdn.microsoft.com/library/ms186981.aspx). |
| Tablo |Tablo başına bölüm |15.000<br/><br/>Yüksek performans için sayısını en aza olan öneririz bölümler, yine de iş gereksinimlerinizi destekleyen while. Bölüm sayısı arttıkça, veri tanımlama dili (DDL) ve veri işleme dili (DML) işlemleri için ek yükü artar ve daha yavaş performans neden olur. |
| Tablo |Karakter başına bölüm sınırının değeri. |4000 |
| Dizin oluşturma |Tablo başına olmayan kümelenmiş dizin. |50<br/><br/>Yalnızca rowstore tablolar için geçerlidir. |
| Dizin oluşturma |Tablo başına Kümelenmiş dizinler. |1<br><br/>Rowstore hem columnstore tablolarına uygulanır. |
| Dizin oluşturma |Dizin anahtarı boyutu. |900 baytı.<br/><br/>Yalnızca rowstore dizini için geçerlidir.<br/><br/>Dizin oluştururken sütunlardaki var olan verilerin 900 baytı aşmıyorsa, birden fazla 900 bayt maksimum boyutu ile varchar sütunları dizinleri oluşturulabilir. Ancak, daha sonra Ekle veya toplam boyutu 900 baytı aşmasına neden sütunlarda güncelleştirme eylemleri başarısız olur. |
| Dizin oluşturma |Anahtar sütunları dizin başına. |16<br/><br/>Yalnızca rowstore dizini için geçerlidir. Kümelenmiş columnstore dizinleri, tüm sütunlarını ekleyin. |
| İstatistikler |Birleştirilmiş sütun değerlerini boyutu. |900 baytı. |
| İstatistikler |Sütun istatistikleri nesne başına. |32 |
| İstatistikler |Oluşturulan tablo başına sütunlarda istatistikler. |30,000 |
| Saklı Yordamlar |En fazla iç içe geçme düzeyi. |8 |
| Görünüm |Sütunları görüntüleme başına |1,024 |

## <a name="loads"></a>Yükleri
| Category | Açıklama | Maksimum |
|:--- |:--- |:--- |
| Polybase yükleri |Satır başına MB |1<br/><br/>Polybase, 1 MB'den daha küçük olan satırları yükler.<br/><br/> |

## <a name="queries"></a>Sorgular
| Category | Açıklama | Maksimum |
|:--- |:--- |:--- |
| Sorgu |Sıraya alınan kullanıcı tabloları sorguları. |1000 |
| Sorgu |Sistem görünümleri eş zamanlı sorguları. |100 |
| Sorgu |Sıraya alınan sorguları sistem görünümleri |1000 |
| Sorgu |En fazla parametreleri |2098 |
| Batch |En büyük boyutu |65,536*4096 |
| Seçim sonuçları |Satır başına sütun |4096<br/><br/>Bu gibi durumlarda, satır başına en fazla 4096 sütun hiçbir zaman seçim sonucu olabilir. Her zaman 4096 olabilir bir garanti yoktur. Sorgu planı geçici tablo gerektiriyorsa, her tablo en fazla 1024 sütunları uygulanabilir. |
| SELECT |İç içe geçmiş alt sorgular |32<br/><br/>Bu gibi durumlarda, 32'den fazla iç içe geçmiş alt sorgular hiçbir zaman bir SELECT deyiminde olabilir. Her zaman 32 olabilir bir garanti yoktur. Örneğin, bir birleştirme alt sorgu sorgu planı tanıtabilirsiniz. Alt sorgular sayısı tarafından kullanılabilir bellek sınırlı olabilir. |
| SELECT |Sütun başına birleştirme |1024 sütunları<br/><br/>Hiçbir zaman 1024'ten fazla sütun birleştirme işleminde sahip olabilir. Her zaman 1024 olabilir bir garanti yoktur. Birleştirme planı birleştirme sonucunu çok sütun içeren geçici bir tablo gerektiriyorsa, 1024 sınırı geçici tablo için geçerlidir. |
| SELECT |GROUP BY sütunları başına bayt sayısı. |8060<br/><br/>GROUP BY yan tümcesindeki sütun Açıklama 8060 baytlık en fazla olabilir. |
| SELECT |ORDER BY sütunları başına bayt |Açıklama 8060 baytlık<br/><br/>ORDER BY yan tümcesindeki sütun Açıklama 8060 baytlık en fazla olabilir. |
| İfade başına tanımlayıcıları |Başvurulan tanımlayıcı sayısı |65,535<br/><br/>SQL veri ambarı, bir tek bir sorgu ifadesinde bulunan tanımlayıcı sayısını sınırlar. Bu numara sonuçları SQL Server hatası 8632 aşılıyor. Daha fazla bilgi için [iç hata: Deyim Hizmetleri sınırına ulaşıldı](https://support.microsoft.com/en-us/help/913050/error-message-when-you-run-a-query-in-sql-server-2005-internal-error-a). |
| Dize değişmez değerleri | Dize sabit bir ifade sayısı | 20.000 <br/><br/>SQL veri ambarı, sorgu tek bir ifade içinde dize sabitleri sayısını sınırlar. Bu numara sonuçları SQL Server hatası 8632 aşılıyor.|

## <a name="metadata"></a>Meta Veriler
| Sistem Görünümü | En fazla satır |
|:--- |:--- |
| sys.dm_pdw_component_health_alerts |10,000 |
| sys.dm_pdw_dms_cores |100 |
| sys.dm_pdw_dms_workers |SQL istekleri için en son 1000 DMS çalışanları toplam sayısı. |
| sys.dm_pdw_errors |10,000 |
| sys.dm_pdw_exec_requests |10,000 |
| sys.dm_pdw_exec_sessions |10,000 |
| sys.dm_pdw_request_steps |Sys.dm_pdw_exec_requests içinde depolanan en son 1000 SQL istekler için adımların toplam sayısı. |
| sys.dm_pdw_os_event_logs |10,000 |
| sys.dm_pdw_sql_requests |Sys.dm_pdw_exec_requests içinde depolanan en son 1000 SQL istekler. |

## <a name="next-steps"></a>Sonraki adımlar
SQL veri ambarı'nı kullanma hakkında öneriler için bkz: [kural sayfası](cheat-sheet.md).
