---
title: Columnstore dizini performansını - Azure SQL veri ambarı | Microsoft Docs
description: Bellek gereksinimlerini azaltın veya columnstore dizininin her satır sıkıştırır satır sayısını en üst düzeye çıkarmak için kullanılabilir belleğini artırma.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: load data
ms.date: 03/22/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 0b9a4ce84544beb09431e494385f3b9d8507c418
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873548"
---
# <a name="maximizing-rowgroup-quality-for-columnstore"></a>Satır grubu kaliteli columnstore için en üst düzeye çıkarma

Satır grubu kalite bir satır satır sayısı tarafından belirlenir. Kullanılabilir belleğin artırılması, columnstore dizininin her satır sıkıştırır satır sayısını en üst düzeye çıkarabilirsiniz.  Sıkıştırma oranları artırmak ve performans columnstore dizinleri için sorgulamak için bu yöntemleri kullanın.

## <a name="why-the-rowgroup-size-matters"></a>Satır grubu boyutu neden önemlidir
Tek tek rowgroups olan sütun segmentleri tarayarak bir tabloda bir columnstore dizini tarar olduğundan, her satır satır sayısı en üst düzeye sorgu performansını geliştirir. RowGroups çok sayıda satır varsa, veri sıkıştırma diskten okunan için daha az veri yani artırır.

Satır grupları hakkında daha fazla bilgi için bkz: [Columnstore dizinleri Kılavuzu](https://msdn.microsoft.com/library/gg492088.aspx).

## <a name="target-size-for-rowgroups"></a>Satır grupları için hedef boyutu
En iyi sorgu performansı için hedef bir columnstore dizini, satır grubu başına satır sayısı en üst düzeye çıkarmaktır. Bir satır grubu en fazla 1.048.576 satır olabilir. Satır grubu başına satır sayısı olmaması uygundur. Columnstore dizinleri RowGroups en az 100.000 satır olduğunda iyi performans elde edin.

## <a name="rowgroups-can-get-trimmed-during-compression"></a>Sıkıştırma sırasında RowGroups kırpılmış

Toplu yük veya columnstore dizini yeniden oluşturma sırasında bazen hiç her satır için belirlenen tüm satırları sıkıştırma için yeterli bellek yok. Bellek baskısı olduğunda, columnstore dizinleri halinde columnstore sıkıştırması başarılı satır grubu boyutları kesim. 

10. 000'en az bir satır her satır sıkıştırmak için bellek yetersiz olduğunda, SQL veri ambarı, bir hata oluşturur.

Toplu yükleme hakkında daha fazla bilgi için bkz. [toplu yükleme kümelenmiş columnstore dizinine](https://msdn.microsoft.com/library/dn935008.aspx#Bulk ).

## <a name="how-to-monitor-rowgroup-quality"></a>Satır grubu kalitesini izleme

DMV sys.dm_pdw_nodes_db_column_store_row_group_physical_stats ([sys.dm_db_column_store_row_group_physical_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql) SQL veri ambarı SQL DB eşleşen görünüm tanımını içeren), yararlı bilgiler de kullanıma sunar. rowgroups ve kırpma vardır, kırpma nedenini satır sayısı gibi. Satır grubu kırpmayı hakkında bilgi edinmek için bu DMV sorgulamak için kullanışlı bir yöntem olarak, aşağıdaki görünümü oluşturabilirsiniz.

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

Trim_reason_desc satır grubu kırpılmış olup olmadığını belirtir (trim_reason_desc = NO_TRIM hiçbir kesme oluştu ve satır grubu en iyi kalitesini anlamına gelir). Kırpma nedeniyle erken erişim satır grubu belirtin:
- BULKLOAD: Gelen toplu iş yükü için satır 1 milyondan az satır başlattıklarında kırpma bu nedenle kullanılır. Altyapısı varsa (delta deposuna ekleme aksine) eklenen 100.000 satır büyük sıkıştırılmış satır grupları oluşturur ancak BULKLOAD için kesim nedeni ayarlar. Bu senaryoda, daha fazla satır eklemek için toplu yükleme artırmayı deneyin. Ayrıca, satır grupları bölüm sınırlarını yayılamaz, çok parçalı değil emin olmak için bölümleme düzeninizi yeniden değerlendir.
- MEMORY_LIMITATION: 1 milyon satır satır grupları oluşturmak için belirli bir miktarda bellek çalışma altyapısı tarafından gereklidir. Kullanılabilir bellek yükleme oturumu gerekli çalışma belleğinden daha az olduğunda, satır grupları erken kırpılmış. Aşağıdaki bölümlerde, gerekli bellek tahmin edin ve daha fazla bellek açıklanmaktadır.
- DICTIONARY_SIZE: Bu kesim nedeni, geniş ve/veya yüksek bir kardinalite dizeleri içeren en az bir dize sütunu olduğundan satır kesme oluştuğunu gösterir. Sözlük boyutu bellekte 16 MB ile sınırlıdır ve satır grubu bu sınıra ulaştığınızda sıkıştırılır. Bu durumla karşılaşırsanız, ayrı bir tabloya sorunlu sütuna yalıtarak göz önünde bulundurun.

## <a name="how-to-estimate-memory-requirements"></a>Bellek gereksinimlerini tahmin etme

<!--
To view an estimate of the memory requirements to compress a rowgroup of maximum size into a columnstore index, download and run the view [dbo.vCS_mon_mem_grant](). This view shows the size of the memory grant that a rowgroup requires for compression in to the columnstore.
-->

Yaklaşık bir satır grubu sıkıştırmak için gereken en fazla belleği olan

- 72 MB +
- \#satırları \* \#sütunları \* 8 bayt +
- \#satırları \* \#kısa dize sütunlarındaki \* 32 bayt +
- \#uzun dize sütunlarındaki \* sıkıştırma sözlüğü için 16 MB

Burada kısa dize sütunlarındaki dize veri türlerini kullanın < = 32 bayt ve dize veri türlerini > 32 bayt uzun dize sütunlarındaki kullanın.

Uzun dizeler metin sıkıştırmak için tasarlanmış bir sıkıştırma yöntemi ile sıkıştırılır. Bu sıkıştırma yöntemi kullanan bir *sözlük* metin desenleri depolamak için. Bir sözlük maksimum boyutu 16 MB'dir. Satır grubu içindeki her uzun dize sütunu için yalnızca bir sözlük var.

Video columnstore bellek gereksinimleri ilgili ayrıntılı bir tartışma için bkz [Azure SQL veri ambarı ölçeklendirme: yapılandırma ve rehberlik](https://channel9.msdn.com/Events/Ignite/2016/BRK3291).

## <a name="ways-to-reduce-memory-requirements"></a>Bellek gereksinimlerini azaltın yolları

Rowgroups columnstore dizinlerine sıkıştırıyorsunuz bellek gereksinimlerini azaltmak için aşağıdaki teknikleri kullanır.

### <a name="use-fewer-columns"></a>Daha az sütun kullanın
Mümkünse, daha az sütun içeren tablo tasarlayın. Columnstore dizinini bir satır columnstore sıkıştırılmış, her sütun segmenti ayrı olarak sıkıştırır. Bu nedenle bir satır grubu sıkıştırmak için bellek gereksinimleri sayısı arttıkça sütunları artırın.


### <a name="use-fewer-string-columns"></a>Daha az dize sütun kullanın
Dize veri türleri sütunlarının daha fazla bellek sayısal ve tarih veri türleri gerektirir. Bellek gereksinimlerini azaltmak için dize sütunlarındaki olgu tabloları kaldırma ve bunları daha küçük boyut tabloları koyarak göz önünde bulundurun.

Dize sıkıştırma için ek bellek gereksinimleri:

- En çok 32 karakter dizesi veri türleriyle değeri her ek 32 bayt gerektirebilir.
- 32'den fazla karakter dizesi veri türleriyle sözlük yöntemlerle sıkıştırılır.  Her sütunda bir satır grubu sözlüğü oluşturmak için en fazla ek olarak 16 MB gerektirebilir.

### <a name="avoid-over-partitioning"></a>Aşırı bölümleme kaçının

Columnstore dizinleri, bölüm başına bir veya daha fazla satır grupları oluşturun. SQL veri ambarı'nda bölüm sayısı hızla çünkü veriler dağıtılır ve her dağıtım bölümlenmiş büyür. Çok fazla tablo varsa rowgroups doldurmak için yeterli satır olmayabilir. Sıkıştırma sırasında satır eksikliği bellek baskısı oluşturmaz, ancak columnstore sorgularınızdan en iyi performansı elde etmeyin rowgroups için yönlendirir.

Aşırı bölümleme önlemek için başka bir nedeni, bir bellek yükü bölümlenmiş bir tablodaki columnstore dizininin satır yükleme için olmasıdır. Bir yük birçok bölümler her bölüm sıkıştırılmasının için yeterli satır olana kadar bellekte tutulur gelen satır alabilir. Çok fazla olması, ek bellek baskısı oluşturur.

### <a name="simplify-the-load-query"></a>Yük sorguyu basitleştirin

Veritabanı sorgusu içindeki tüm işleçleri arasında bir sorgu için bellek ataması paylaşır. Sıkıştırma için kullanılabilir bellek yükü sorgu karmaşık sıralar ve birleşimler olduğunda azaltılır.

Yalnızca sorgu yüklenirken üzerinde odaklanmak için yük sorgu tasarlayın. Veriler üzerinde dönüştürmeler çalıştırmanız gerekiyorsa, bunları ayrı yük sorgu çalıştırın. Örneğin, bir yığın tablodaki verileri hazırlama, dönüştürmeleri çalıştırın ve sonra columnstore dizinine hazırlama tablosuna yükleyin. Ayrıca verileri ilk yüklemek ve ardından verileri dönüştürmek için MPP sistemi kullanın.

### <a name="adjust-maxdop"></a>MAXDOP Ayarla

Her dağıtım, birden fazla CPU çekirdeği dağıtım kullanılabilir rowgroups columnstore paralel olarak sıkıştırır. Bellek baskısı ve satır kesme açabilir ek bellek kaynaklarının, paralellik gerektirir.

Bellek baskısı azaltmak için yükleme işlemi her dağıtım içinde seri modunda çalışmaya zorlamak için MAXDOP sorgu ipucunu kullanabilirsiniz.

```sql
CREATE TABLE MyFactSalesQuota
WITH (DISTRIBUTION = ROUND_ROBIN)
AS SELECT * FROM FactSalesQuota
OPTION (MAXDOP 1);
```

## <a name="ways-to-allocate-more-memory"></a>Daha fazla bellek yolları

DWU boyutu ve birlikte kullanıcı kaynak sınıfı için bir kullanıcı sorgu kullanılabilir ne kadar bellek belirleyin. Bellek ataması için bir yük sorgu artırmak için Dwu sayısını artırmak veya kaynak sınıfı artırın.

- Dwu'lar artırmak için bkz: [nasıl ı ölçeklendirme performans?](quickstart-scale-compute-portal.md)
- Kaynak sınıfı için bir sorgu değiştirmek için bkz [kullanıcı kaynak sınıfı örneğini değiştirmek](resource-classes-for-workload-management.md#change-a-users-resource-class).

## <a name="next-steps"></a>Sonraki adımlar

SQL veri ambarı performansını artırmak için daha fazla yolunu öğrenmek için bkz: [performansa genel bakış](sql-data-warehouse-overview-manage-user-queries.md).

