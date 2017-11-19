---
title: SQL Data Warehouse kapasite limitlerini | Microsoft Docs
description: "Bağlantılar, veritabanları, tablolar ve SQL Data Warehouse için sorguları için en yüksek değerleri."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: e1eac122-baee-4200-a2ed-f38bfa0f67ce
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: reference
ms.date: 11/10/2017
ms.author: kevin;barbkess
ms.openlocfilehash: d10d06edfc75594854d8f4da5cf29d6c2fd5ed24
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="sql-data-warehouse-capacity-limits"></a>SQL Data Warehouse kapasite sınırları
Aşağıdaki tablolarda, Azure SQL Data Warehouse çeşitli bileşenler için izin verilen en yüksek değerleri içerir.

## <a name="workload-management"></a>İş yükü yönetimi
| Kategori | Açıklama | Maksimum |
|:--- |:--- |:--- |
| [Veri ambarı birimi (DWU)][Data Warehouse Units (DWU)] |Tek bir SQL Data Warehouse için en fazla DWU | Esneklik için en iyi duruma getirilmiş [performans katmanı](performance-tiers.md): DW6000<br></br>İşlem için en iyi duruma getirilmiş [performans katmanı](performance-tiers.md): DW30000c |
| [Veri ambarı birimi (DWU)][Data Warehouse Units (DWU)] |Sunucu başına DTU varsayılan |54,000<br></br>Varsayılan olarak, her bir SQL server (örneğin, myserver.database.windows.net) kadar DW6000c sağlayan bir DTU kota olarak 54.000, sahiptir. Bu kota yalnızca bir güvenlik sınırıdır. Tarafından kotayı artırabilir [bir destek bileti oluşturma] [ creating a support ticket] ve seçerek *kota* istek türü olarak.  DTU hesaplamak için 7.5 DWU gerektiği ve toplam çarpın veya gerekli toplam cDWU tarafından 9.0 Çarp gerekir. Örneğin:<br></br>7.5 = 45,000 x DW6000 Dtu'lar<br></br>9.0 = olarak 54.000 x DW600c Dtu'lar.<br></br>SQL server seçeneği, geçerli DTU tüketimi Portalı'nda görüntüleyebilirsiniz. Duraklatıldı ve gezinti veritabanları doğru DTU kota sayısı. |
| Veritabanı bağlantısı |Eşzamanlı açık oturum |1024<br/><br/>Her 1024 etkin oturumlar aynı anda SQL veri ambarı veritabanına istekleri gönderebilirsiniz. Not, aynı anda yürütebilir sorgu sayısı sınırlamaları vardır. Eşzamanlılık sınırı aşıldığında, istek bir iç sıra burada işlenmeyi bekleyen gider. |
| Veritabanı bağlantısı |Hazırlanmış deyimleri için en fazla belleği |20 MB |
| [İş yükü yönetimi][Workload management] |En fazla eş zamanlı sorgular |32<br/><br/> Varsayılan olarak, SQL Data Warehouse en fazla 32 eş zamanlı sorgular ve sorguları kalan sıraları yürütebilir.<br/><br/>Kullanıcılar daha yüksek kaynak sınıfları veya SQL Data Warehouse düşük olduğunda atanmış descrease eşzamanlı sorgu sayısı yapabilirsiniz [hizmet düzeyi](performance-tiers.md#service-levels). DMV sorgu gibi bazı sorgular çalıştırmak için her zaman izin verilir. |
| [tempdb][Tempdb] |En çok GB |DW100 başına 399 GB. Bu nedenle DWU1000 tempdb 3,99 TB boyuta sahip olmadığından |

## <a name="database-objects"></a>Veritabanı nesneleri
| Kategori | Açıklama | Maksimum |
|:--- |:--- |:--- |
| Veritabanı |En büyük boyutu |Disk üzerinde sıkıştırılmış 240 TB<br/><br/>Bu alan, tempdb veya günlük alanının bağımsızdır ve bu nedenle bu alanı kalıcı tablolara ayrılmış olabilir.  Kümelenmiş columnstore sıkıştırma 5 X tahmin.  Bu sıkıştırma yaklaşık 1 büyümeye veritabanı sağlayan tüm tabloları kümelenmiş columnstore (varsayılan tablo türü) olduğunda PB. |
| Tablo |En büyük boyutu |60 disk üzerinde sıkıştırılmış TB |
| Tablo |Her bir veritabanı tabloları |2 milyara |
| Tablo |Tablo başına sütun |1024 sütunları |
| Tablo |Sütun başına bayt sayısı |Sütun bağımlı [veri türü][data type].  Karakter veri türleri için 8000 nvarchar 4000 ya da en büyük veri türleri için 2 GB sınırıdır. |
| Tablo |Satır, tanımlanmış boyut başına bayt sayısı |Açıklama 8060 baytlık<br/><br/>Satır başına bayt sayısı, sayfa sıkıştırması ile SQL Server için olduğu gibi aynı şekilde hesaplanır. SQL Server gibi SQL veri ambarı etkinleştirir satır taşma depolama destekleyen **değişken uzunluğu sütununa** satır dışı edilmesini. Değişken uzunlukta satır satır dışı basıldığında yalnızca 24-bayt kök ana kayıtta depolanır. Daha fazla bilgi için bkz: [veri satırı taşma aşan 8 KB'lik][Row-Overflow Data Exceeding 8 KB]. |
| Tablo |Tablo başına bölüm |15,000<br/><br/>Yüksek performans, sayısını en aza indirmenizi öneririz bölümlerini hala iş gereksinimlerinizi destekleyen while. Bölüm sayısı arttıkça, ek yükü veri tanımlama dili (DDL) ve veri işleme dili (DML) işlemleri için büyür ve performans neden olur. |
| Tablo |Karakter başına bölüm sınır değeri. |4000 |
| Dizin |Tablo başına olmayan Kümelenmiş dizinler. |999<br/><br/>Yalnızca rowstore tablolar için geçerlidir. |
| Dizin |Tablo başına Kümelenmiş dizinler. |1<br><br/>Rowstore ve columnstore tabloları için geçerlidir. |
| Dizin |Dizin anahtarı boyutu. |900 bayt sayısı.<br/><br/>Yalnızca rowstore dizinler için geçerlidir.<br/><br/>Dizin oluşturulduğunda sütunlardaki var olan verileri 900 bayt aşmayan, dizinleri birden fazla 900 bayt maksimum boyuta sahip varchar sütunlarda oluşturulabilir. Ancak, daha sonra ekleme veya güncelleştirme eylemleri toplam boyutu 900 baytı aşmasına neden sütunlar üzerinde başarısız olur. |
| Dizin |Dizin başına anahtar sütun. |16<br/><br/>Yalnızca rowstore dizinler için geçerlidir. Kümelenmiş columnstore dizinleri tüm sütunları içerir. |
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
| SEÇİN |İç içe alt sorgular |32<br/><br/>Bu gibi durumlarda, 32'den fazla iç içe alt sorgulara hiçbir zaman bir SELECT deyimi içinde olabilir. 32 her zaman olabilir garantisi yoktur. Örneğin, bir birleştirme alt sorgu sorgu planı tanıtabilirsiniz. Sayı, alt sorgular tarafından kullanılabilir bellek sınırlı olabilir. |
| SEÇİN |BİRLEŞİM başına sütun |1024 sütunları<br/><br/>Bu gibi durumlarda, 1024'ten fazla sütun hiçbir zaman birleştirme olabilir. 1024 her zaman olabilir garantisi yoktur. BİRLEŞİM planı birleştirme sonucunu çok sütun içeren geçici bir tablo gerektiriyorsa, 1024 sınırı geçici tabloya uygulanır. |
| SEÇİN |GROUP BY sütun başına bayt sayısı. |8060<br/><br/>GROUP BY yan tümcesinde sütun en fazla Açıklama 8060 baytlık olabilir. |
| SEÇİN |ORDER BY sütun başına bayt sayısı |Açıklama 8060 baytlık.<br/><br/>ORDER BY yan tümcesinde sütun en fazla Açıklama 8060 baytlık olabilir. |
| Tanımlayıcıları ve deyimi başına sabitleri |Başvurulan tanımlayıcıları ve sabitleri sayısı. |65,535<br/><br/>SQL veri ambarı tanımlayıcıları ve tek bir sorgu ifadesinde bulunan sabitler sayısını sınırlar. Bu sınırı 65. 535'dir. SQL Server hatası 8632 numara bu sonuçlarında aşıyor. Daha fazla bilgi için bkz: [iç hata: deyim Hizmetleri sınırına ulaşıldı][Internal error: An expression services limit has been reached]. |

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
Daha fazla başvuru bilgileri için bkz: [SQL Data Warehouse başvuru genel bakış][SQL Data Warehouse reference overview].

<!--Image references-->

<!--Article references-->
[Data Warehouse Units (DWU)]: ./sql-data-warehouse-overview-what-is.md
[SQL Data Warehouse reference overview]: ./sql-data-warehouse-overview-reference.md
[Workload management]: ./sql-data-warehouse-develop-concurrency.md
[Tempdb]: ./sql-data-warehouse-tables-temporary.md
[data type]: ./sql-data-warehouse-tables-data-types.md
[creating a support ticket]: /sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->
[Row-Overflow Data Exceeding 8 KB]: https://msdn.microsoft.com/library/ms186981.aspx
[Internal error: An expression services limit has been reached]: https://support.microsoft.com/kb/913050
