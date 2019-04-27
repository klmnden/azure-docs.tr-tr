---
title: Mayıs 2018'den Azure SQL veri ambarı sürüm notları | Microsoft Docs
description: Azure SQL veri ambarı için sürüm notları.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 07/23/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: c17cb13bff0ea9eb3b0bb2caf5bb527fa3958428
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60402709"
---
# <a name="whats-new-in-azure-sql-data-warehouse-may-2018"></a>Azure SQL veri ambarı'nda yenilikler nelerdir? Mayıs 2018 
Azure SQL veri ambarı, sürekli olarak iyileştirmeler alır. Bu makalede, Mayıs 2018'de sunulan değişiklikler ve yeni özellikleri açıklar. 

## <a name="gen-2-instances"></a>Gen 2 örnekleri
![alt](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/2528b41b-f09f-45b1-aa65-fc60d562d3bd.png) Azure SQL veri ambarı işlem için iyileştirilmiş 2. nesil katmanı bulut veri ambarı işlemleri için yeni performans standartları ayarlar. Müşteriler artık daha iyi sorgu performansını en fazla beş kez alın, daha fazla eşzamanlılık ve beş kat daha yüksek bilgi işlem gücü için geçerli nesil karşılaştırıldığında dört kez. Artık 128 eş zamanlı sorgu, herhangi bir bulut veri ambarı hizmeti en yüksek tek bir kümeden hizmet verebilir.

Bkz: [yükseltebileceğinizi bulut analizi, Azure SQL veri ambarı ile](https://azure.microsoft.com/blog/turbocharge-cloud-analytics-with-azure-sql-data-warehouse/) blog duyurusuna Rohan Kumar, Kurumsal Başkan Yardımcısı, Azure veri.

## <a name="auto-statistics"></a>Otomatik İstatistikler
İstatistikleri sorgu planı oluşturma, SQL veri ambarı altyapısında gibi modern maliyet tabanlı iyileştiricileri en iyi duruma getirmek için kritik öneme sahiptir. Tüm sorguları önceden bilindiğinde ise istatistikleri nesne oluşturulması gerektiğini belirleme ulaşılabilir bir görevdir. Ancak, sistem ile geçici karşılaştığı ve veri ambarı iş yükleriniz için tipik olan rastgele sorguları tahmin etmek sistem yöneticileri uğraşıyor, hangi istatistikleri büyük olasılıkla yetersiz sorgu yürütme planları için önde gelen ve uzun oluşturulması gerekir Sorgu yanıt süreleri. Bu sorunu çözmek için bir yol istatistiklerini nesneler üzerinde tüm tablo sütunları önceden oluşturmaktır. Ancak, işlem istatistikleri nesneleri sırasında yükleme işlemi, daha uzun yükleme süreleri neden tablo saklanması gerektiği bir ceza ile birlikte gelir.

SQL veri ambarı artık daha fazla esneklik, üretkenlik ve kullanım kolaylığı, sistem yöneticileri ve geliştiriciler için sistem kalitesini yürütme planlarını ve en iyi sunmaya devam etmesini sağlar sağlama istatistikleri nesneleri otomatik olarak oluşturulmasını destekler. yanıt süreleri.

Etkinleştirmek veya SQL veri ambarı oluşturmayı otomatik istatistikleri devre dışı bırakmak için aşağıdaki deyimi yürütün:
```sql
ALTER DATABASE { database_name } SET { AUTO_CREATE_STATISTICS { OFF | ON } } [;]
```

Bir en iyi yöntem ve kılavuz olarak ayarı öneririz `AUTO_CREATE_STATISTICS` seçeneğini `ON`.

> [!NOTE]
> Otomatik istatistik oluşturma *varsayılan olarak etkin* tüm yeni veri ambarları için.
>  

Bkz: [ALTER DATABASE SET seçenekleri](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options) makalesine bakın.

## <a name="rejected-row-support"></a>Satır destek reddetti
Müşterilerin kullandığı [PolyBase veri yükleme için (Dış tablolar)](design-elt-data-loading.md) nedeniyle yüksek performanslı SQL veri ambarı'na veri yükleme doğasını paralel. PolyBase olan varsayılan yükleme modeli aracılığıyla veri yüklenirken [Azure Data Factory](https://azure.com/adf) de. 

SQL veri ambarı ekler özelliği aracılığıyla bir reddedilen satır konumunu tanımlayın `REJECTED_ROW_LOCATION` parametresiyle [CREATE EXTERNAL TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql) deyimi. Yürütme sonrasında bir [CREATE TABLE AS SELECT (CTAS)](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) yüklenemedi herhangi bir satır dış tablodaki varlıkları daha fazla araştırma için kaynak yakın bir dosyada depolanır. 

Bkz: [yük güvenle SQL veri ambarı PolyBase reddedilen satır konumu ile](https://azure.microsoft.com/blog/load-confidently-with-sql-data-warehouse-polybase-rejected-row-location/) reddedilen satır davranışı hakkında daha fazla ayrıntı için blogu.

Aşağıdaki örnek, reddedilen satır belirtmek için yeni söz dizimini gösterir.

```sql
CREATE EXTERNAL TABLE [dbo].[Reject_Example]
(
    ...
)
WITH
(
    ...
    ,REJECTED_ROW_LOCATION=‘/Reject_Directory'
)
```

## <a name="alter-view"></a>ALTER VIEW
[ALTER VIEW](https://docs.microsoft.com/sql/t-sql/statements/alter-view-transact-sql) silme/CREATE görünümü gerek kalmadan daha önce oluşturulan görünümü değiştirme ve izinleri yeniden açmasına olanak sağlar. 

Aşağıdaki örnek, daha önce oluşturulan görünümü değiştirir.
```sql
ALTER VIEW test_view AS SELECT 1 [data];
```

## <a name="concatws"></a>CONCAT_WS
[CONCAT_WS()](https://docs.microsoft.com/sql/t-sql/functions/concat-ws-transact-sql) işlevi bir uçtan uca şekilde iki veya daha fazla değer birleşimi kaynaklanan bir dize döndürür. Bitişik değer ile ilk bağımsız değişkende belirtilen sınırlayıcıyı ayırır. `CONCAT_WS` İşlevi virgül ile ayrılmış değer (CSV) çıktı oluşturmak için yararlıdır.

Aşağıdaki örnek, bir dizi int değerleri virgül ile birleştirerek gösterir.
```sql
SELECT CONCAT_WS(',', 1, 2, 3) [result];
```
Deyimi aşağıdaki sonucu verir:
```
result
---------
1,2,3
```
Aşağıdaki örnek, bir dizi karma veri türü değerleri virgül ile birleştirerek gösterir.
```sql
SELECT CONCAT_WS(',', 1, 2, 'String', NEWID()) [result]
```
Deyimi aşağıdaki sonucu verir:
```
result
---------
1,2,String,26E1F74D-5746-44DC-B47F-2FC1DA1B6E49
```

## <a name="spdatatypeinfo"></a>SP_DATATYPE_INFO
[Sp_datatype_info](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-datatype-info-transact-sql) sistem saklı yordam geçerli ortam tarafından desteklenen veri türleri hakkında bilgi döndürür. ODBC bağlantıları için veri türü araştırma bağlantı araçları tarafından yaygın olarak kullanılır.

Aşağıdaki örnek, SQL veri ambarı tarafından desteklenen tüm veri türleri için ayrıntılarını alır.

```sql
EXEC sp_datatype_info
```

## <a name="select-into-with-order-by-behavior-change"></a>ORDER BY davranış değişikliği ile INTO seçin
SQL veri ambarı artık engeller `SELECT INTO` içeren sorgular bir `ORDER BY` yan tümcesi. Daha önce bu işlem, ilk veri bellek ve daha sonra veri tablosu şekli eşleşecek şekilde yeniden sıralama hedef tablosuna ekleme sıralaması başarılı olabilir.

### <a name="previous-behavior"></a>Önceki davranışı
Aşağıdaki deyim, ek işlem ek yükü ile başarılı olabilir.
```sql
SELECT * INTO table2 FROM table1 ORDER BY 1;
```

### <a name="current-behavior"></a>Şu anki davranışı
Aşağıdaki deyim belirten bir hata oluşturmaz `ORDER BY` yan tümcesi içinde desteklenmez bir `SELECT INTO` deyimi.
```sql
SELECT * INTO table2 FROM table1 ORDER BY 1;
```
Error deyimi döndürdü:
```
Msg 104381, Level 16, State 1, Line 1
The ORDER BY clause is invalid in views, CREATE TABLE AS SELECT, INSERT SELECT, SELECT INTO, inline functions, derived tables, subqueries, and common table expressions, unless TOP or FOR XML is also specified.
```

## <a name="set-parseonly-on-query-status-behavior-change"></a>Sorgu durumu (davranış değişikliği) üzerinde PARSEONLY ayarlayın
Kullanarak `SET PARSEONLY ON` sözdizimi SQL veri ambarı altyapısının her T-SQL deyimi sözdizimi inceleyin ve derleme veya deyimi yürütüp herhangi bir hata iletisi döndürmek bir kullanıcı sağlar. Önceden, [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) sistem görünümü, bu deyimler durumu kalabilir `Running` durumu. `sys.dm_pdw_exec_requests` Görünüm durumu olarak artık döndürecektir `Complete`.

## <a name="next-steps"></a>Sonraki adımlar
SQL veri ambarı hakkında biraz bilmek, bilgi nasıl hızlı bir şekilde [SQL veri ambarı oluşturma][create a SQL Data Warehouse]. Azure'da yeniyseniz yeni terimlerle karşılaşabileceğinizi için [Azure sözlüğünü][Azure glossary] yararlı bulabilirsiniz. Alternatif olarak, aşağıdaki diğer SQL Veri Ambarı Kaynakları’na göz atın.  

* [Müşteri başarı hikayeleri]
* [Bloglar]
* [Özellik istekleri]
* [Videolar]
* [Müşteri Danışma Ekibi blogları]
* [Stack Overflow forumu]
* [Twitter]


[Bloglar]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Müşteri Danışma Ekibi blogları]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Müşteri başarı hikayeleri]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Özellik istekleri]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow forumu]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videolar]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
