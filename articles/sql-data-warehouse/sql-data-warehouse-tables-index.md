---
title: Azure SQL veri ambarı tabloları dizinleme | Microsoft Azure
description: Öneriler ve Azure SQL veri ambarı tabloları dizinleme örnekler.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 03/18/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seoapril2019
ms.openlocfilehash: 158b229c2c45a14ed0fd5433d1903eca92f32401
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65851660"
---
# <a name="indexing-tables-in-sql-data-warehouse"></a>SQL veri ambarı'nda dizin tabloları

Öneriler ve Azure SQL veri ambarı tabloları dizinleme örnekler.

## <a name="index-types"></a>Dizin türleri

SQL veri ambarı dahil olmak üzere çeşitli dizin oluşturma seçeneği sunar [kümelenmiş columnstore dizinleri](/sql/relational-databases/indexes/columnstore-indexes-overview), [Kümelenmiş dizinler ve kümelenmemiş dizinler](/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described), ve dizin olmayan bir seçenek olarak da bilinen [yığın ](/sql/relational-databases/indexes/heaps-tables-without-clustered-indexes).  

Bir tablo olan bir dizin oluşturmak için bkz: [CREATE TABLE (Azure SQL veri ambarı)](/sql/t-sql/statements/create-table-azure-sql-data-warehouse) belgeleri.

## <a name="clustered-columnstore-indexes"></a>Kümelenmiş columnstore dizinleri

Bir tabloda dizin seçeneği belirtildiğinde varsayılan olarak, SQL veri ambarı kümelenmiş bir columnstore dizini oluşturur. Kümelenmiş columnstore tablolarının hem en yüksek düzeyde veri sıkıştırma, hem de genel en iyi sorgu performansını sağlar.  Kümelenmiş columnstore tabloları genelde kümelenmiş dizin veya yığın tablo aşar ve genellikle büyük tablolar için en iyi seçenek olan.  Bu nedenlerle, kümelenmiş columnstore tablonuzu dizinleme nasıl gerçekleştireceğinizden emin olduğunuzda başlatmak için en iyi yerdir.  

Kümelenmiş bir columnstore tablosuna oluşturmak için basitçe WITH yan tümcesinde kümelenmiş COLUMNSTORE dizini belirtin veya WITH yan tümcesiyle bırakın:

```SQL
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Burada kümelenmiş columnstore iyi bir seçenek olmayabilir birkaç senaryo vardır:

- Columnstore tabloları, varchar(max), nvarchar(max) ve varbinary(max) desteklemez. Yığın veya kümelenmiş dizin yerine göz önünde bulundurun.
- Columnstore tabloları, geçici veriler için daha az verimli olabilir. Yığın ve hatta belki de geçici tablolara göz önünde bulundurun.
- 60 milyondan az satır içeren küçük tablolar. Yığın tabloları göz önünde bulundurun.

## <a name="heap-tables"></a>Yığın tabloları

Verileri SQL veri ambarı'nda geçici olarak giriş, yığın tabloları kullanmanın işlem yapar bulabilirsiniz. Yığınlar yüklemeler daha hızlı dizin tabloları ve bazı durumlarda, sonraki okuma önbelleğinden yapılabilir olmasıdır.  Verileri yalnızca daha fazla dönüştürme gerçekleştirmeden önce hazırlamak için yüklüyorsanız, tabloyu yığın tablosuna yüklemek verileri kümelenmiş columnstore tablosuna yüklemekten çok daha hızlı. Ayrıca, verileri yüklenirken bir [geçici tablo](sql-data-warehouse-tables-temporary.md) kalıcı depolama için bir tablo yüklenirken daha hızlı yükler.  

60 milyondan az satır küçük arama tablolar için sık sık yığın tabloları mantıklı.  60 milyondan fazla satır olduğunda en iyi sıkıştırma elde etmek küme columnstore tabloları başlayın.

Yığın tablo oluşturmak için yığın WITH yan tümcesinde belirtin:

```SQL
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Kümelenmiş ve kümelenmemiş dizinleri

Tek bir satır hızlı bir şekilde alınması gerektiğinde Kümelenmiş dizinler kümelenmiş columnstore tablolarının daha iyi performans gösterir. Sorgular tek veya çok az sayıda satır arama olağanüstü hız performansı için gerekli olduğu için küme dizini ya da ikincil kümelenmemiş dizin göz önünde bulundurun. Kümelenmiş bir dizin kullanılarak dezavantajı yararlanan sorguları yüksek oranda seçmeli bir filtre kullanan kümelenmiş dizin sütunu olanların olmasıdır. Diğer sütunlarda filtre geliştirmek için diğer sütunları kümelenmemiş bir dizin eklenebilir. Ancak, tabloya eklenen her dizin alanı hem de işleme süresini yükler ile ekler.

Bir kümelenmiş dizin tablosu oluşturmak için kümelenmiş dizin WITH yan tümcesinde belirtin:

```SQL
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Bir tabloda kümelenmemiş bir dizin eklemek için aşağıdaki sözdizimini kullanın:

```SQL
CREATE INDEX zipCodeIndex ON myTable (zipCode);
```

## <a name="optimizing-clustered-columnstore-indexes"></a>Kümelenmiş columnstore dizinleri en iyi duruma getirme

Kümelenmiş columnstore tablolarının verilerinde kesimler halinde düzenlenir.  Columnstore tablosunda optimum sorgu performansı elde etmek için yüksek segment kalitesinin yüksek olması önemlidir.  Segment kalitesi, sıkıştırılmış satır grubu içindeki satır sayısıyla ölçülebilir.  Segment kalitesi olduğu en az sıkıştırılmış satır başına 100 bin satır grubunda ve satır grubu yaklaşımını başına satır sayısı arttıkça performans 1.048.576 satır, satır grubu içerebilir çoğu satırları olduğu geçirmesine en uygunudur.

Aşağıdaki görünüm oluşturulabilir ve sisteminizde işlem satır başına ortalama satır grubunda ve herhangi bir düzeyin cluster columnstore dizinlerine tanımlamak için kullanılır.  Bu görünüm son sütunu yeniden kullanılabilir bir SQL deyimi oluşturur.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,    COUNT(DISTINCT rg.[partition_number])                    AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,    CEILING    ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Görünüm oluşturduğunuza göre 100 bin satır miktarından daha azıyla çalışabilse satır grupları tablolarla tanımlamak için bu sorguyu çalıştırın. Elbette, daha fazla iyi segment kalitesini arıyorsanız 100 bin cinsinden eşiğini artırmak isteyebilirsiniz.

```sql
SELECT    *
FROM    [dbo].[vColumnstoreDensity]
WHERE    COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Verilere bakmak ve analiz sonuçlarınızı başlamadan sorgu çalıştırıldıktan sonra. Bu tabloda, satır grubu analizinizi dikkat edilmesi gerekenler açıklanmaktadır.

| Sütun | Bu verileri kullanma |
| --- | --- |
| [table_partition_count] |Tablo bölümlenmişse, daha yüksek açık satır grubu sayıları görmek bekleyebilir. Her bölümde dağıtım teorik olarak kendisiyle ilişkilendirilmiş bir açık satır grubu olabilir. Bu analiz faktörü. Bölümlenmiş küçük bir tablo bölümleme tamamen ortadan kaldırarak iyileştirilmiş bu sıkıştırma iyileştirecek şekilde. |
| [row_count_total] |Tablo için toplam satır sayısı. Örneğin, bu değer, sıkıştırılmış satır yüzdesini hesaplamak için kullanabilirsiniz. |
| [row_count_per_distribution_MAX] |Tüm satırları eşit olarak dağıtılmış, hedef dağıtım başına satır sayısı bu değeri olur. Bu değer compressed_rowgroup_count ile karşılaştırın. |
| [COMPRESSED_rowgroup_rows] |Satırlar columnstore biçiminde tablosu için toplam sayısı. |
| [COMPRESSED_rowgroup_rows_AVG] |Ortalama satır sayısını önemli ölçüde satır satır grubu için maksimum sayısı daha azdır, verileri sıkıştırmak için CTAS veya ALTER INDEX REBUILD kullanarak göz önünde bulundurun |
| [COMPRESSED_rowgroup_count] |Satır grupları columnstore biçiminde sayısı. Bu sayı tablosu ile ilgili çok yüksekse, columnstore yoğunluklu düşükse bir göstergesidir. |
| [COMPRESSED_rowgroup_rows_DELETED] |Satırlar columnstore biçiminde mantıksal olarak silinir. Tablo boyutuna göre sayı yüksekse, bölümü yeniden oluşturmak veya bu bunları fiziksel olarak kaldırır dizinini yeniden oluşturmayı düşünün. |
| [COMPRESSED_rowgroup_rows_MIN] |Bu ortalama ve en fazla sütunlarını birlikte, columnstore satır grupları için değer aralığını anlamak için kullanın. En iyi duruma getirme veri yükü olarak kullanılabilir yükleme eşiği (102,400 hizalı bölüm dağıtım başına) üzerinden düşük bir sayı önerir. |
| [COMPRESSED_rowgroup_rows_MAX] |Yukarıdaki gibi |
| [OPEN_rowgroup_count] |Açık satır grupları normal. Bir makul bir açık satır grubu başına Tablo dağıtımı (60) bekleyebilirsiniz. Aşırı numaraları bölümler arasında veri yükleme önerin. Çifte denetim emin olmak için bölümleme stratejisi ses |
| [OPEN_rowgroup_rows] |Her satır grubu 1.048.576 satır içinde en fazla olabilir. Nasıl tam açık satır grupları şu anda olduğunu görmek için bu değeri kullanın. |
| [OPEN_rowgroup_rows_MIN] |Gruplar'ı açın, veriler akışla yüklenme tabloya veya önceki yük satırları bu satır grubuna kalan geçmiş olduğunu olduğunu gösterir. MIN MAX kullanın, ne kadar veri, açma sat görmek için ortalama sütunların satır grupları. Küçük tablolar için tüm verilerin %100 olabilir! ALTER INDEX columnstore için veri zorlamak için REBUILD durumda. |
| [OPEN_rowgroup_rows_MAX] |Yukarıdaki gibi |
| [OPEN_rowgroup_rows_AVG] |Yukarıdaki gibi |
| [CLOSED_rowgroup_rows] |Kapalı satır grubu satırları sağlamlık denetimi olarak görünür. |
| [CLOSED_rowgroup_count] |Kapalı satır grubu herhangi hiç görülürse düşük olmalıdır. ALTER INDEX kullanarak sıkıştırılmış satır grupları için Kapalı satır grupları dönüştürülebilir... Komut yeniden düzenleyin. Ancak, bu genellikle gerekli değildir. Kapalı grupları columnstore satır grupları için arka plan "tanımlama grubu taşıyıcısı" işlemi tarafından otomatik olarak geri dönüştürülür. |
| [CLOSED_rowgroup_rows_MIN] |Kapalı satır grupları, çok yüksek doldurma oranı olmalıdır. Kapalı satır grubu doldurma oranı düşükse, columnstore çözümlemeler gereklidir. |
| [CLOSED_rowgroup_rows_MAX] |Yukarıdaki gibi |
| [CLOSED_rowgroup_rows_AVG] |Yukarıdaki gibi |
| [Rebuild_Index_SQL] |Bir tabloda columnstore dizini yeniden oluşturmak için SQL |

## <a name="causes-of-poor-columnstore-index-quality"></a>Zayıf columnstore dizin kalitesinin nedenleri

Tablolar ile düşük segment kalitesi belirlediyseniz, kök nedeni belirlemek istersiniz.  Zayıf segment kalitesi, bazı diğer yaygın nedenleri aşağıda verilmiştir:

1. Dizin oluşturulduğunda bellek baskısı
2. Yüksek hacimli DML işlemleri
3. Küçük veya yükleme işlemlerini trickle
4. Çok fazla

Bu etkenler satır grubu başına en iyi 1 milyon satır değerinden küçük bir columnstore dizini, önemli ölçüde olması neden olabilir. Bunlar ayrıca bir sıkıştırılmış satır grubu yerine delta satır grubu gitmek satır neden olabilir.

### <a name="memory-pressure-when-index-was-built"></a>Dizin oluşturulduğunda bellek baskısı

Sıkıştırılmış satır grubu başına satır sayısı, satır satır grubu işlemek kullanılabilir bellek miktarı ve genişliğine doğrudan ilişkilidir.  Satırlar columnstore tablolarına bellek baskısı altında yazıldığında, segment kalitesi düşebilir.  Bu nedenle, en iyi kadar bellek mümkün olduğunca columnstore dizin tabloları erişiminizi yazma oturum vermektir.  Bellek ve eşzamanlılık arasında bir denge olduğundan, doğru bellek ayırma kılavuzuna tablonuzun her bir satırdaki verileri bağlıdır, sisteminiz ve eşzamanlılık yuva sayısı için ayrılmış veri ambarı birimleri oturuma verebilirsiniz, tablonuza veri yazıyor demektir.

### <a name="high-volume-of-dml-operations"></a>Yüksek hacimli DML işlemleri

Güncelleştirme ve silme satırları DML işlemleri yüksek hacimli Etkisizliği columnstore ortaya çıkarabilir. Bir satır grubunda satırları çoğunu değiştirildiğinde, bu özellikle doğrudur.

- Bir sıkıştırılmış satır grubu, mantıksal olarak yalnızca bir satırın silinmesi satır silinmiş olarak işaretler. Bölüm veya tablo yeniden oluşturulana kadar sıkıştırılmış satır grubu içindeki satır kalır.
- Satır ekleme, bir delta satır grubu olarak adlandırılan bir iç rowstore tabloya satır ekler. Delta satır grubu dolu ve kapalı olarak işaretlenene kadar columnstore için eklenen satır dönüştürülemez. Hizmetin maksimum kapasitesi 1.048.576 satırı sıkıştırması ulaşmadan sonra satır grupları kapatılır.
- Columnstore biçiminde bir satırı güncelleştirmek, mantıksal bir silme ve ardından bir ekleme işlenir. Eklenen satırın delta deposunda depolanabilir.

Toplu güncelleştirme ve bölüme göre hizalı dağıtım başına 102. 400 satır toplu eşiğini aşan ekleme işlemleri doğrudan columnstore biçimine gidin. Ancak, bir dağılımı varsayıldığında, bunun gerçekleşmesi tek bir işlemde 6.144 milyondan fazla satır değiştirme gerekecektir. Belirli bir bölüme göre hizalı dağıtım için satır sayısı değerinden 102,400 ise satır delta mağazaya gidin ve yeterli satır eklendi veya satır grubu kapatmak için değiştirilmiş veya dizini yeniden oluşturulana kadar orada kalır.

### <a name="small-or-trickle-load-operations"></a>Küçük veya yükleme işlemlerini trickle

SQL veri ambarı'na akış bazen bilinir, yükleri trickle gibi küçük yükler. Bunlar genellikle sistem tarafından alınıyor veri yakın sabit akışını temsil eder. Ancak, bu akış yakın sürekli olarak toplu satır özellikle büyük değil. Genellikle önemli ölçüde columnstore biçiminde doğrudan bir yükleme için gereken eşiğin altında verilerdir.

Bu durumda, verileri Azure blob depolama alanında ilk kavuşmak ve yükleme öncesinde accumulate çalışmasına izin daha iyidir. Bu teknik genellikle olarak bilinen *mikro toplu işleme*.

### <a name="too-many-partitions"></a>Çok fazla

Dikkate alınması gereken başka bir şey üzerinde kümelenmiş columnstore tablolarının bölümleme etkisidir.  Bölümleme önce SQL veri ambarı zaten verilerinizi 60 veritabanına olarak böler.  Bölümleme, verilerinizi daha fazla böler.  Ardından, verilerinizi bölümlemeniz halinde düşünün **her** bölümü, bir kümelenmiş columnstore dizini en az 1 milyon satır gerekiyor.  100 bölüme, tablo bölümleme sonra bir kümelenmiş columnstore dizini için en az 6 milyar satıra tablonuzun gerekir (60 dağıtım *100 bölüme* 1 milyon satır). 100 bölüm tablonuzun 6 milyar satıra sahip değilse, bölüm sayısını azaltabilir veya yığın tablo kullanmayı düşünün.

Bazı veriler tablolarınızı veriler yüklendikten sonra takip tanımlamak ve iyinin tablolarla yeniden oluşturmak için aşağıdaki adımları columnstore dizinleri kümelenmiş.

## <a name="rebuilding-indexes-to-improve-segment-quality"></a>Segment kalitesini artırmak için dizinlerini yeniden oluşturma

### <a name="step-1-identify-or-create-user-which-uses-the-right-resource-class"></a>1\. adım: Kimliğinizi belirlemek veya doğru kaynak sınıfı kullanan kullanıcı oluşturma

Hemen segment kalitesini artırmak için bir hızlı yol, dizini yeniden sağlamaktır.  Yukarıdaki görünümü tarafından döndürülen SQL dizinlerinizi yeniden oluşturmak için kullanılabilecek bir ALTER INDEX REBUILD deyimi döndürür. Dizinlerinizi yeniden oluştururken dizininizi oluşturur oturum yeterli bellek tahsis emin olun.  Bunu yapmak için bu tablodaki önerilen minimum dizini yeniden oluşturma izni olan bir kullanıcının kaynak sınıfı artırın.

Daha fazla kullanıcıya kendi kaynak sınıfı artırarak bellek ilişkin bir örnek aşağıdadır. Kaynak sınıfları ile çalışmak için bkz [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md).

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>2\. adım: Daha yüksek kaynak sınıfı kullanıcıyla kümelenmiş columnstore dizinleri yeniden oluştur

Daha yüksek bir kaynak sınıfı kullanarak artık olan kullanıcı olarak 1. adımdaki (örneğin LoadUser) oturum açın ve ALTER INDEX deyimi yürütün. Bu kullanıcı dizini burada yeniden oluşturuluyorsa tablolara ALTER iznine sahip olduğundan emin olun. Bu örnekler, tüm columnstore dizinini yeniden oluşturmak nasıl ya da tek bir bölüm yeniden oluşturmak nasıl gösterir. Yeniden oluşturmak için daha fazla pratik bir kerede tek bir bölüm dizinleri olduğu büyük tablolar üzerinde.

Alternatif olarak, dizini yeniden derlemeyi yerine, tabloyu yeni bir tabloya kopyalanamadı [CTAS kullanarak](sql-data-warehouse-develop-ctas.md). Hangi yolla en iyisidir? Büyük veri birimleri için CTAS genellikle daha hızlıdır [ALTER INDEX](/sql/t-sql/statements/alter-index-transact-sql). Daha küçük veri hacimleri, ALTER INDEX kullanımı daha kolay ve tablo takas etmenizi yapılması gerekmez.

```sql
-- Rebuild the entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

SQL veri ambarı'nda bir dizini yeniden oluşturma çevrimdışı bir işlemdir.  ALTER INDEX REBUILD bölümünde dizinlerini yeniden oluşturma hakkında daha fazla bilgi için bkz. [Columnstore dizinleri birleştirme](/sql/relational-databases/indexes/columnstore-indexes-defragmentation), ve [ALTER INDEX](/sql/t-sql/statements/alter-index-transact-sql).

### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>3\. adım: Kümelenmiş columnstore segment kalitesini geliştirdi doğrulayın

Yeniden çalıştırılan sorgunun düşük ile tanımlanan hangi tabloya segment kalitesi ve segment kalitesi doğrulayın geliştirdi.  Segment kalitesini artırmak değil ise, tablosundaki satırları çok geniş olması olabilir.  Daha yüksek kaynak sınıfı ya da DWU dizinlerinizi yeniden oluştururken kullanmayı düşünün.

## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>CTAS ve bölüm değiştirme ile dizinlerini yeniden oluşturma

Bu örnekte [CREATE TABLE AS SELECT (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) deyimi ve bölüm geçiş tablosu bölümü yeniden oluşturun.

```sql
-- Step 1: Select the partition of data and write it out to a new table using CTAS
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Switch IN the rebuilt data with TRUNCATE_TARGET option
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2 WITH (TRUNCATE_TARGET = ON);
```

CTAS kullanarak bölümleri yeniden oluşturma hakkında daha fazla ayrıntı için bkz. [bölümleri kullanarak SQL veri ambarı'nda](sql-data-warehouse-tables-partition.md).

## <a name="next-steps"></a>Sonraki adımlar

Tablo geliştirme hakkında daha fazla bilgi için bkz. [tabloları geliştirme](sql-data-warehouse-tables-overview.md).
