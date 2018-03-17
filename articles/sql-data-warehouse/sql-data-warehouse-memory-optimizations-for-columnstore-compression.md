---
title: "Columnstore dizini performansı - Azure SQL Data Warehouse | Microsoft Docs"
description: "Bellek gereksinimlerini azaltın veya her satır grubu kimliğinde bir columnstore dizini sıkıştırır satırların sayısı en üst düzeye çıkarmak için kullanılabilir bellek artırın."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 03/15/2018
ms.author: barbkess
ms.openlocfilehash: 74e641f9da418d678bdbef0c69f9f59ccee32303
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="maximizing-rowgroup-quality-for-columnstore"></a>Satır grubu kimliğinde kalitesi columnstore için en üst düzeye çıkarma

Satır grubu kimliğinde kalite bir satır grubu kimliğinde satır sayısı tarafından belirlenir. Bellek gereksinimlerini azaltın veya her satır grubu kimliğinde bir columnstore dizini sıkıştırır satırların sayısı en üst düzeye çıkarmak için kullanılabilir bellek artırın.  Sıkıştırma oranları artırmak ve performans columnstore dizinleri için sorgulamak için bu yöntemleri kullanın.

## <a name="why-the-rowgroup-size-matters"></a>Satır grubu kimliğinde boyutu neden önemlidir?
Tek tek rowgroups sütun kesimleri tarayarak bir tabloda bir columnstore dizini tarar olduğundan, her satır grubu kimliğinde satır sayısı en üst düzeye çıkarma sorgu performansını geliştirir. Çok sayıda satır RowGroups sahip olduğunuzda, veri sıkıştırma diskten okumak için daha az veri olmadığı anlamına gelir artırır.

Rowgroups hakkında daha fazla bilgi için bkz: [Columnstore dizinleri Kılavuzu](https://msdn.microsoft.com/library/gg492088.aspx).

## <a name="target-size-for-rowgroups"></a>Rowgroups ilişkin hedef boyutu
En iyi sorgu performansını satır grubu kimliğinde bir columnstore dizininde başına satır sayısı en üst düzeye çıkarmak için belirtilir. Bir satır grubu kimliğinde 1.048.576 satır en olabilir. Satır grubu başına satır sayısının üst sınırını olmaması uygundur. Columnstore dizinleri RowGroups 100. 000'en az satır olduğunda iyi performans elde etmek.

## <a name="rowgroups-can-get-trimmed-during-compression"></a>Sıkıştırma sırasında RowGroups kırpılmış

Bir toplu yükleme veya columnstore dizini yeniden oluşturma sırasında bazen yeterli bellek yok her satır grubu için belirlenen tüm satırları sıkıştırmak kullanılabilir. Bellek baskısı olduğunda sıkıştırma columnstore içine başarılı şekilde columnstore dizinleri satır grubu kimliğinde boyutları kesim. 

Her satır grubu 10. 000'en az satır sıkıştırmak için bellek yetersiz olduğunda, SQL Data Warehouse bir hata oluşturur.

Toplu yükleme hakkında daha fazla bilgi için bkz: [toplu yükleme kümelenmiş columnstore dizinine](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).

## <a name="how-to-monitor-rowgroup-quality"></a>Satır grubu kimliğinde kalitesini izleme

Rowgroups ve orada kırpma, kırpma nedenini satır sayısı gibi yararlı bilgileri sunan bir DMV (sys.dm_pdw_nodes_db_column_store_row_group_physical_stats) yoktur. Satır grubu kimliğinde kırpma hakkında bilgi almak için bu DMV sorgusu için kullanışlı bir yöntem olarak, aşağıdaki görünümü oluşturabilirsiniz.

```sql
create view dbo.vCS_rg_physical_stats
as 
with cte
as
(
select   tb.[name]                    AS [logical_table_name]
,        rg.[row_group_id]            AS [row_group_id]
,        rg.[state]                   AS [state]
,        rg.[state_desc]              AS [state_desc]
,        rg.[total_rows]              AS [total_rows]
,        rg.[trim_reason_desc]        AS trim_reason_desc
,        mp.[physical_name]           AS physical_name
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb               ON  sm.[schema_id]          = tb.[schema_id]                             
JOIN    sys.[pdw_table_mappings] mp   ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt     ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[dm_pdw_nodes_db_column_store_row_group_physical_stats] rg      ON  rg.[object_id]     = nt.[object_id]
                                                                            AND rg.[pdw_node_id]   = nt.[pdw_node_id]
                                        AND rg.[distribution_id]    = nt.[distribution_id]                                          
)
select *
from cte;
```

Trim_reason_desc satır grubu kimliğinde aynı olup olmadığını bildirir (trim_reason_desc = NO_TRIM hiçbir kırpma vardı ve satır grubu en iyi kalitesini olduğu anlamına gelir). Kırpma şunlardan satır grubu kimliğinde erken kırpılacağını belirtin:
- BULKLOAD: gelen toplu iş yükü için satır 1 milyondan az satır varken bu kırpma neden kullanılır. Altyapısı varsa (delta deposuna ekleme aksine) eklenmekte 100.000 satırları büyük sıkıştırılmış satır grupları oluşturur, ancak kırpma neden BULKLOAD için ayarlar. Bu senaryoda, daha fazla satır birikmesine toplu iş yükü pencerenizi artırmayı düşünün. Ayrıca, satır gruplarından bölüm sınırları yayılamaz gibi çok parçalı olmadığından emin olmak için bölümleme düzeni yeniden.
- MEMORY_LIMITATION: 1 milyon satır satır grupları oluşturmak için belirli bir çalışma bellek miktarını altyapısı tarafından gereklidir. Kullanılabilir bellek yükleme oturumunun gerekli çalışma belleğinden daha az olduğunda, satır grupları erken kırpılmış. Aşağıdaki bölümlerde, gerekli bellek tahmin etmek ve daha fazla bellek ayırmak açıklanmaktadır.
- DICTIONARY_SIZE: Kırpma bu nedenle, geniş ve/veya yüksek nicelik dizeleri içeren en az bir dize sütunu olduğundan satır grubu kimliğinde kırpma oluştuğunu gösterir. Sözlük boyutu 16 MB bellek sınırlıdır ve satır grubu bu sınıra ulaşıldığında sıkıştırılmış. Bu durumla karşılaşırsanız, sorunlu sütun ayrı bir tabloya izole etmeyi düşünün.

## <a name="how-to-estimate-memory-requirements"></a>Bellek gereksinimlerini tahmin etme

<!--
To view an estimate of the memory requirements to compress a rowgroup of maximum size into a columnstore index, download and run the view [dbo.vCS_mon_mem_grant](). This view shows the size of the memory grant that a rowgroup requires for compression in to the columnstore.
-->

Bir satır grubu kimliğinde sıkıştırmak için gereken en fazla bellek yaklaşık olarak şöyledir

- 72 MB +
- \#Satır \* \#sütunları \* 8 bayt +
- \#Satır \* \#kısa dize sütunlarındaki \* 32 bayt +
- \#uzun dize sütunlarındaki \* sıkıştırma sözlüğü için 16 MB

Burada kısa dize sütunlarındaki dize veri türlerini kullanın < = 32 bayt ve > 32 bayt dizesi veri türlerini uzun dize sütunlarındaki kullanın.

Uzun dizeleri metin sıkıştırma için tasarlanmış bir sıkıştırma yöntemi ile sıkıştırılır. Bu bir sıkıştırma yöntemi kullanan bir *sözlük* metin kalıplarını depolamak için. Bir sözlük maksimum boyutu 16 MB'tır. Satır grubu kimliğinde her uzun bir dize sütunu için yalnızca bir sözlük yoktur.

Video columnstore bellek gereksinimlerini kapsamlı bir açıklama için bkz [Azure SQL Data Warehouse ölçeklendirme: yapılandırma ve Kılavuzu](https://myignite.microsoft.com/videos/14822).

## <a name="ways-to-reduce-memory-requirements"></a>Bellek gereksinimlerini azaltmak için yollar

Columnstore dizinleri rowgroups sıkıştırma için bellek gereksinimlerini azaltmak için aşağıdaki tekniklerini kullanın.

### <a name="use-fewer-columns"></a>Daha az sütun kullanın
Mümkünse, daha az sütun içeren tablo tasarlayın. Columnstore dizinini bir satır grubu kimliğinde columnstore sıkıştırılmış olduğunda, her bir sütun segmenti ayrı olarak sıkıştırır. Bu nedenle bir satır grubu kimliğinde sıkıştırmak için bellek gereksinimleri sütunları artar artırın.


### <a name="use-fewer-string-columns"></a>Daha az dize sütun kullanın
Dize veri türleri sütunlarının daha fazla bellek sayısal ve tarih veri türü gerektirir. Bellek gereksinimlerini azaltmak için dize sütunlarındaki olgu tablolarından kaldırarak ve daha küçük boyut tabloları koyma göz önünde bulundurun.

Dize sıkıştırma için ek bellek gereksinimleri:

- En çok 32 karakter dizesi veri türleriyle 32 ek bayt değeri başına gerektirebilir.
- Birden fazla 32 karakter dizesi veri türleriyle sözlük yöntemleri kullanarak sıkıştırılır.  Her sütunda satır grubu kimliğinde sözlük oluşturmak için ek 16 MB kadar gerektirebilir.

### <a name="avoid-over-partitioning"></a>Aşırı bölümleme kaçının

Columnstore dizinleri bölüm başına bir veya daha fazla rowgroups oluşturun. SQL veri ambarı'nda bölüm sayısı çünkü veri dağıtılır ve her dağıtım bölümlenmiş hızlı bir şekilde artar. Tablonun çok fazla bölümleri varsa rowgroups doldurmak için yeterli satırları olmayabilir. Satır eksikliği sıkıştırma sırasında bellek baskısı oluşturmaz, ancak en iyi columnstore sorgu performansı elde etmeyin rowgroups yol açar.

Aşırı bölümleme önlemek için başka bir nedenle bölümlenmiş bir tablodaki columnstore dizininin içine satırları yükleme için ek bellek olmasıdır. Bir yükleme sırasında her bir bölümü olan sıkıştırılabilmesi için yeterli satırlar kadar bellekte tutulan gelen satırlarını, birçok bölüm alabilir. Çok fazla bölümlemeye sahip olmak için ek bellek baskısı oluşturur.

### <a name="simplify-the-load-query"></a>Yük sorguyu basitleştirin

Veritabanı bellek ataması sorgu yer alan tüm işleçler arasında bir sorgu için paylaşır. Bir yük sorgu karmaşık sıralar ve birleşimler olduğunda, sıkıştırma kullanılabilir bellek azalır.

Yalnızca sorgu yüklenirken üzerinde odaklanmaya yük sorgu tasarlayın. Üzerindeki veri dönüşümleri çalıştırmanız gerekiyorsa, bunları yük sorgudan ayrı çalıştırın. Örneğin, öbek tablodaki verileri aşama, dönüştürmeleri çalıştırabilir ve sonra columnstore dizinini hazırlama tablosunda yükleyin. Ayrıca verileri ilk yükleme ve verileri dönüştürmek için MPP sistemi kullanın.

### <a name="adjust-maxdop"></a>MAXDOP Ayarla

Dağıtım birden çok CPU çekirdeği bulunmadığında her dağıtım rowgroups paralel columnstore içine sıkıştırır. Paralellik bellek baskısı ve satır grubu kimliğinde kırpma neden olabilecek ek bellek kaynaklarının gerektirir.

Bellek baskısı azaltmak için her dağıtım içinde seri modunda çalıştırmak için yükleme işlemi zorlamak için MAXDOP sorgu ipucu kullanabilirsiniz.

```
CREATE TABLE MyFactSalesQuota
WITH (DISTRIBUTION = ROUND_ROBIN)
AS SELECT * FROM FactSalesQUota
OPTION (MAXDOP 1);
```

## <a name="ways-to-allocate-more-memory"></a>Daha fazla bellek yolları

DWU boyutu ve kullanıcı kaynak sınıfı birlikte ne kadar bellek kullanıcı sorgu için uygun olduğunu belirler. Bir yük sorgu için bellek ataması artırmak için Dwu sayısını artırmak veya kaynak sınıfı artırın.

- Dwu artırmak için bkz: [nasıl ı ölçeklendirme performans?](quickstart-scale-compute-portal.md)
- Bir sorgu için kaynak sınıfı değiştirmek için bkz: [kullanıcı kaynak sınıfı örneğini değiştirmek](resource-classes-for-workload-management.md#assigning-resource-classes).

Örneğin, DWU 100 üzerinde smallrc kaynak sınıfında bir kullanıcı her dağıtım için 100 MB bellek kullanabilir. Ayrıntılar için bkz [eşzamanlılık SQL Data warehouse'da](resource-classes-for-workload-management.md).

Yüksek kaliteli satır grubu kimliğinde boyutları almak için 700 MB bellek gerektiğini belirlemek varsayalım. Bu örnekler, yeterli belleğe sahip yük sorgu nasıl çalıştırabileceğiniz gösterir.

- DWU 1000 ve mediumrc kullanarak, 800 MB bellek ataması
- DWU 600 ve largerc kullanarak bellek ataması 800 MB'tır.


## <a name="next-steps"></a>Sonraki adımlar

SQL veri ambarı performansını artırmak için daha fazla yolunu öğrenmek için bkz: [performans genel bakış](sql-data-warehouse-overview-manage-user-queries.md).

<!--Image references-->

<!--Article references-->


<!--MSDN references-->

<!--Other Web references-->
