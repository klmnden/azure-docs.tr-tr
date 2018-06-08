---
title: Kapasite sınırlamaları - Azure SQL Data Warehouse | Microsoft Docs
description: Azure SQL Data Warehouse çeşitli bileşenler için izin verilen maksimum değer.
services: sql-data-warehouse
author: antvgski
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: b79d928f3c1c3d81fbca0b8d676d4a4cbf83369a
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839648"
---
# <a name="sql-data-warehouse-capacity-limits"></a>SQL Data Warehouse kapasite sınırları
Azure SQL Data Warehouse çeşitli bileşenler için izin verilen maksimum değer.

## <a name="workload-management"></a>İş yükü yönetimi
| Kategori | Açıklama | Maksimum |
|:--- |:--- |:--- |
| [Veri ambarı birimi (DWU)](what-is-a-data-warehouse-unit-dwu-cdwu.md) |Tek bir SQL Data Warehouse için en fazla DWU | Gen1: DW6000<br></br>Gen2: DW30000c |
| [Veri ambarı birimi (DWU)](what-is-a-data-warehouse-unit-dwu-cdwu.md) |Sunucu başına DTU varsayılan |54,000<br></br>Varsayılan olarak, her bir SQL server (örneğin, myserver.database.windows.net) kadar DW6000c sağlayan bir DTU kota olarak 54.000, sahiptir. Bu kota yalnızca bir güvenlik sınırıdır. Tarafından kotayı artırabilir [bir destek bileti oluşturma](sql-data-warehouse-get-started-create-support-ticket.md) ve seçerek *kota* istek türü olarak.  DTU hesaplamak için 7.5 DWU gerektiği ve toplam çarpın veya gerekli toplam cDWU tarafından 9.0 Çarp gerekir. Örneğin:<br></br>7.5 = 45,000 x DW6000 Dtu'lar<br></br>9.0 = olarak 54.000 x DW6000c Dtu'lar.<br></br>SQL server seçeneği, geçerli DTU tüketimi Portalı'nda görüntüleyebilirsiniz. DTU kotasında hem duraklatılmış hem de duraklatılmamış veritabanları sayılır. |
| Veritabanı bağlantısı |Eşzamanlı açık oturum |1024<br/><br/>Her 1024 etkin oturumlar aynı anda SQL veri ambarı veritabanına istekleri gönderebilirsiniz. Not, aynı anda yürütebilir sorgu sayısı sınırlamaları vardır. Eşzamanlılık sınırı aşıldığında, istek bir iç sıra burada işlenmeyi bekleyen gider. |
| Veritabanı bağlantısı |Hazırlanmış deyimleri için en fazla belleği |20 MB |
| [İş yükü yönetimi](resource-classes-for-workload-management.md) |En fazla eş zamanlı sorgular |32<br/><br/> Varsayılan olarak, SQL Data Warehouse en fazla 32 eş zamanlı sorgular ve sorguları kalan sıraları yürütebilir.<br/><br/>Kullanıcılar için daha yüksek kaynak sınıfları veya SQL Data Warehouse düşük olduğunda atandığında eşzamanlı olan sorgu sayısını azaltabilirsiniz [veri ambarı birimi](memory-and-concurrency-limits.md) ayarı. DMV sorgu gibi bazı sorgular çalıştırmak için her zaman izin verilir. |
| [tempdb](sql-data-warehouse-tables-temporary.md) |En çok GB |DW100 başına 399 GB. Bu nedenle DWU1000 3,99 TB tempdb boyutlandırılır. |

## <a name="database-objects"></a>Veritabanı nesneleri
| Kategori | Açıklama | Maksimum |
|:--- |:--- |:--- |
| Database |Maksimum boyut | Gen1: 240 disk üzerinde sıkıştırılmış TB. Bu alan, tempdb veya günlük alanının bağımsızdır ve bu nedenle bu alanı kalıcı tablolara ayrılmış olabilir.  Kümelenmiş columnstore sıkıştırma 5 X tahmin.  Bu sıkıştırma yaklaşık 1 büyümeye veritabanı sağlayan tüm tabloları kümelenmiş columnstore (varsayılan tablo türü) olduğunda PB. <br/><br/> Gen2: rowstore ve columnstore tabloları için sınırsız depolama 240TB |
| Tablo |Maksimum boyut |60 disk üzerinde sıkıştırılmış TB |
| Tablo |Her bir veritabanı tabloları |10,000 |
| Tablo |Tablo başına sütun |1024 sütunları |
| Tablo |Sütun başına bayt sayısı |Sütun bağımlı [veri türü](sql-data-warehouse-tables-data-types.md). Karakter veri türleri için 8000 nvarchar 4000 ya da en büyük veri türleri için 2 GB sınırıdır. |
| Tablo |Satır, tanımlanmış boyut başına bayt sayısı |Açıklama 8060 baytlık<br/><br/>Satır başına bayt sayısı, sayfa sıkıştırması ile SQL Server için olduğu gibi aynı şekilde hesaplanır. SQL Server gibi SQL veri ambarı etkinleştirir satır taşma depolama destekleyen **değişken uzunluğu sütununa** satır dışı edilmesini. Değişken uzunlukta satır satır dışı basıldığında yalnızca 24-bayt kök ana kayıtta depolanır. Daha fazla bilgi için bkz: [veri satırı taşma aşan 8 KB'lik](https://msdn.microsoft.com/library/ms186981.aspx). |
| Tablo |Tablo başına bölüm |15,000<br/><br/>Yüksek performans, sayısını en aza indirmenizi öneririz bölümlerini hala iş gereksinimlerinizi destekleyen while. Bölüm sayısı arttıkça, ek yükü veri tanımlama dili (DDL) ve veri işleme dili (DML) işlemleri için büyür ve performans neden olur. |
| Tablo |Karakter başına bölüm sınır değeri. |4000 |
| Dizin oluşturma |Tablo başına olmayan Kümelenmiş dizinler. |50<br/><br/>Yalnızca rowstore tablolar için geçerlidir. |
| Dizin oluşturma |Tablo başına Kümelenmiş dizinler. |1<br><br/>Rowstore ve columnstore tabloları için geçerlidir. |
| Dizin oluşturma |Dizin anahtarı boyutu. |900 bayt sayısı.<br/><br/>Yalnızca rowstore dizinler için geçerlidir.<br/><br/>Dizin oluşturulduğunda sütunlardaki var olan verileri 900 bayt aşmayan, dizinleri birden fazla 900 bayt maksimum boyuta sahip varchar sütunlarda oluşturulabilir. Ancak, daha sonra ekleme veya güncelleştirme eylemleri toplam boyutu 900 baytı aşmasına neden sütunlar üzerinde başarısız olur. |
| Dizin oluşturma |Dizin başına anahtar sütun. |16<br/><br/>Yalnızca rowstore dizinler için geçerlidir. Kümelenmiş columnstore dizinleri tüm sütunları içerir. |
| İstatistikler |Birleşik sütun değerlerini boyutu. |900 bayt sayısı. |
| İstatistikler |Sütun istatistikleri nesne başına. |32 |
| İstatistikler |Tablo başına sütunlar üzerinde oluşturulan istatistikleri. |30,000 |
| Saklı Yordamlar |İç içe geçme en yüksek düzeyde. |8 |
| Görünüm |Görünüm başına sütun |1,024 |

## <a name="loads"></a>Yükler
| Kategori | Açıklama | Maksimum |
|:--- |:--- |:--- |
| Polybase yükler |Satır başına MB |1<br/><br/>1 MB'tan küçük ve VARCHAR(MAX), NVARCHAR(MAX) veya VARBINARY(MAX) yüklenemiyor satırlara Polybase yükler.<br/><br/> |

## <a name="queries"></a>Sorgular
| Kategori | Açıklama | Maksimum |
|:--- |:--- |:--- |
| Sorgu |Kullanıcı tablolar üzerinde sıraya alınan sorgular. |1000 |
| Sorgu |Sistem görünümleri eş zamanlı sorguları. |100 |
| Sorgu |Sıraya alınan sorgularına ilişkin sistem görünümleri |1000 |
| Sorgu |En fazla parametreleri |2098 |
| Batch |En büyük boyutu |65,536*4096 |
| SELECT sonuçları |Satır başına sütun |4096<br/><br/>Bu gibi durumlarda, satır başına birden fazla 4096 sütun hiçbir zaman SELECT sonucu olabilir. 4096 her zaman olabilir garantisi yoktur. Sorgu planı geçici bir tablo gerektiriyorsa, her tabloda en fazla 1024 sütunları uygulanabilir. |
| SEÇ |İç içe alt sorgular |32<br/><br/>Bu gibi durumlarda, 32'den fazla iç içe alt sorgulara hiçbir zaman bir SELECT deyimi içinde olabilir. 32 her zaman olabilir garantisi yoktur. Örneğin, bir birleştirme alt sorgu sorgu planı tanıtabilirsiniz. Sayı, alt sorgular tarafından kullanılabilir bellek sınırlı olabilir. |
| SEÇ |BİRLEŞİM başına sütun |1024 sütunları<br/><br/>Bu gibi durumlarda, 1024'ten fazla sütun hiçbir zaman birleştirme olabilir. 1024 her zaman olabilir garantisi yoktur. BİRLEŞİM planı birleştirme sonucunu çok sütun içeren geçici bir tablo gerektiriyorsa, 1024 sınırı geçici tabloya uygulanır. |
| SEÇ |GROUP BY sütun başına bayt sayısı. |8060<br/><br/>GROUP BY yan tümcesinde sütun en fazla Açıklama 8060 baytlık olabilir. |
| SEÇ |ORDER BY sütun başına bayt sayısı |Açıklama 8060 baytlık<br/><br/>ORDER BY yan tümcesinde sütun en fazla Açıklama 8060 baytlık olabilir |
| Deyimi başına tanımlayıcıları |Başvurulan tanımlayıcıları sayısı |65,535<br/><br/>SQL veri ambarı tek bir sorgu ifadesinde bulunan tanımlayıcıları sayısını sınırlar. SQL Server hatası 8632 numara bu sonuçlarında aşıyor. Daha fazla bilgi için [iç hata: deyim Hizmetleri sınırına ulaşıldı] [iç hata: deyim Hizmetleri sınırına ulaşıldı]. |
| Dize değişmez değerleri | Dize değişmez değerleri bir deyimde sayısı | 20,000 <br/><br/>SQL veri ambarı dize sabitleri sorgu tek bir ifadede sayısını sınırlar. SQL Server hatası 8632 numara bu sonuçlarında aşıyor.|

## <a name="metadata"></a>Meta Veriler
| Sistem Görünümü | En fazla satır |
|:--- |:--- |
| sys.dm_pdw_component_health_alerts |10,000 |
| sys.dm_pdw_dms_cores |100 |
| sys.dm_pdw_dms_workers |SQL istekleri DMS çalışanları için en son 1000 toplam sayısı. |
| sys.dm_pdw_errors |10,000 |
| sys.dm_pdw_exec_requests |10,000 |
| sys.dm_pdw_exec_sessions |10,000 |
| sys.dm_pdw_request_steps |Adımları sys.dm_pdw_exec_requests içinde depolanan en son 1000 SQL istekler için toplam sayısı. |
| sys.dm_pdw_os_event_logs |10,000 |
| sys.dm_pdw_sql_requests |En son 1000 SQL sys.dm_pdw_exec_requests içinde depolanan ister. |

## <a name="next-steps"></a>Sonraki adımlar
SQL veri ambarı kullanma hakkında daha fazla önerileri için bkz: [kopya sayfası](cheat-sheet.md).
