---
title: "SQL veri ambarı tablolarda dizin oluşturma | Microsoft Azure"
description: "Azure SQL Data Warehouse'da dizin tablo ile çalışmaya başlama."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 03/15/2018
ms.author: barbkess
ms.openlocfilehash: f0e19be5ec7453a76ff4f22ead6d3885e50beca8
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="indexing-tables-in-sql-data-warehouse"></a>SQL veri ambarı tablolarda dizin oluşturma
> [!div class="op_single_selector"]
> * [Genel bakış][Overview]
> * [Veri türleri][Data Types]
> * [Dağıt][Distribute]
> * [Dizin][Index]
> * [Bölüm][Partition]
> * [İstatistikleri][Statistics]
> * [Geçici][Temporary]
> 
> 

SQL Data Warehouse dahil olmak üzere çeşitli dizin oluşturma seçenekleri sunar [kümelenmiş columnstore dizinleri][clustered columnstore indexes], [Kümelenmiş dizinler ve kümelenmemiş dizinler][clustered indexes and nonclustered indexes].  Ayrıca, aynı zamanda herhangi bir dizin seçeneği olarak da bilinen sağlar [yığın][heap].  Bu makalede dizinlerinizi dışında çoğu performans alma ipuçları yanı sıra her dizin türünü yararları kapsar. Bkz: [table söz dizimi oluşturma] [ create table syntax] SQL Data Warehouse'da tablo oluşturma hakkında daha fazla ayrıntı için.

## <a name="clustered-columnstore-indexes"></a>Kümelenmiş columnstore dizinleri
Bir tablo üzerinde hiçbir dizin seçeneği belirtildiğinde varsayılan olarak, SQL Data Warehouse kümelenmiş columnstore dizini oluşturur. Kümelenmiş columnstore tabloları hem en yüksek düzeyde veri sıkıştırma, hem de en iyi genel sorgu performansı sunar.  Kümelenmiş columnstore tabloları kümelenmiş dizin veya yığın tabloları genellikle aşar ve genellikle büyük tablolar için en iyi seçenek olan.  Bu nedenlerle, kümelenmiş columnstore tablonuz dizini oluşturmak nasıl emin değilseniz başlattığınızda için en iyi yerdir.  

Kümelenmiş columnstore tablo oluşturmak için basitçe WITH yan tümcesinde kümelenmiş COLUMNSTORE dizini belirtin veya WITH yan tümcesi devre dışı bırakın:

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

* Columnstore tabloları, varchar(max), nvarchar(max) ve varbinary(max) desteklemez.  Yığın veya kümelenmiş dizin yerine göz önünde bulundurun.
* Columnstore tablolar için geçici verileri daha az verimli olabilir.  Yığın ve hatta belki de geçici tabloları göz önünde bulundurun.
* 100 milyondan az satır içeren küçük tabloları silin.  Yığın tabloları göz önünde bulundurun.

## <a name="heap-tables"></a>Yığın tabloları
SQL Veri Ambarı’na geçici veri yüklemesi yapıyorsanız, yığın tabloları kullanmanın işlem hızını artırdığını fark edeceksiniz.  Yığın yükleri daha hızlı bir şekilde dizin tablolara ve sonraki okuma önbelleğinden yapılabilir bazı durumlarda olmasıdır.  Verileri yalnızca daha fazla dönüştürme gerçekleştirmeden önce hazırlamak için yüklüyorsanız, tabloyu yığın tablosuna yüklemek verileri kümelenmiş bir columnstore tablosuna yüklemekten çok daha hızlı olacaktır. Ayrıca, verileri yüklenirken bir [geçici tablo] [ Temporary] de çok kalıcı depolama birimine bir tablo yüklenirken daha hızlı yükler.  

100 milyondan az satır küçük arama tabloları için genellikle yığın tabloları anlamlı.  100 milyondan fazla satır olduğunda en iyi sıkıştırma elde etmek için küme columnstore tabloları başlayın.

Bir yığın tablo oluşturmak için yığın WITH yan tümcesinde belirtin:

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
Tek bir satır hızlı bir şekilde alınması gerektiğinde Kümelenmiş dizinler kümelenmiş columnstore tabloları daha iyi performans gösterir.  Tek veya çok az sayıda satır arama aşırı hız performansı için gerekli olduğu sorgular için küme dizini ya da ikincil kümelenmemiş dizin göz önünde bulundurun.  Kümelenmiş dizin kullanarak dezavantajı, yüksek oranda seçmeli filtre kümelenmiş dizin sütunu kullanan sorgular faydalanırsınız ' dir.  Diğer sütunlarda filtre artırmak için kümelenmemiş bir dizin için diğer sütunları eklenebilir.  Ancak, bir tabloya eklenen her dizin için yükleri alanı ve işlemci zamanı ekleyeceksiniz.

Bir kümelenmiş dizin tablo oluşturmak için kümelenmiş dizini WITH yan tümcesinde belirtin:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Bir tabloda kümelenmemiş bir dizin eklemek için yalnızca aşağıdaki sözdizimini kullanın:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

## <a name="optimizing-clustered-columnstore-indexes"></a>Kümelenmiş columnstore dizinleri en iyi duruma getirme
Kümelenmiş columnstore tabloları veri kesimler halinde düzenlenir.  Yüksek segment kalite columnstore tabloda en iyi sorgu performansını elde için önemlidir.  Segment kalite sıkıştırılmış satır grubu satır sayısı tarafından ölçülebilir.  Olduğu en az 100 K satır sıkıştırılmış satır başına grup ve satır grubu yaklaşımını başına satır sayısı arttıkça performans 1.048.576 satır, satır grubu içerebilir çoğu satırları olduğu elde segment kalite en uygundur.

Aşağıdaki görünüm oluşturulur ve işlem satır başına ortalama satır grup ve tüm alt uygun küme columnstore dizinleri tanımlamak için sisteminizde kullanılır.  Bu görünümde son sütun dizinleri yeniden oluşturmak için kullanılan SQL ifadesi olarak oluşturur.

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

Görünümü oluşturduğunuza göre daha az 100 K satır ile satır grupları tablolarla tanımlamak için bu sorguyu çalıştırın.  Elbette, daha fazla en iyi segment kalitesini arıyorsanız 100 K eşiğini artırmak isteyebilirsiniz. 

```sql
SELECT    *
FROM    [dbo].[vColumnstoreDensity]
WHERE    COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Veri arama ve sonuçlarınızı çözümlemek için başlayabilirsiniz sorgu çalıştırdıktan sonra. Bu tabloda, satır grubu çözümlemede dikkat edilmesi gerekenler açıklanmaktadır.

| Sütun | Bu verileri kullanma |
| --- | --- |
| [table_partition_count] |Ardından tablo bölümlenmişse daha yüksek açık satır grubu sayar görmeyi beklediğiniz. Her bölüm dağıtımında teorik olarak kendisiyle ilişkili bir açık satır grubu olabilir. Çözümleme öğeli. Bölümlenmiş küçük bir tablo tamamen bölümlendirme kaldırılarak iyileştirilmiş gibi bu sıkıştırma artırmak. |
| [row_count_total] |Tablo için toplam satır sayısı. Örneğin, sıkıştırılmış durumda satırları yüzdesini hesaplamak için bu değeri kullanın. |
| [row_count_per_distribution_MAX] |Tüm satırları olacak şekilde eşit dağıtılır, bu değer, dağıtım başına satır sayısını olacaktır. Bu değeri compressed_rowgroup_count ile karşılaştırın. |
| [COMPRESSED_rowgroup_rows] |Tablo için columnstore biçiminde satırların toplam sayısı. |
| [COMPRESSED_rowgroup_rows_AVG] |Ortalama satır sayısını önemli ölçüde satır satır grubu için maksimum sayısı daha azdır, CTAS veya ALTER INDEX REBUILD verileri sıkıştırmak kullanmayı göz önünde bulundurun |
| [COMPRESSED_rowgroup_count] |Satır gruplarını columnstore biçiminde sayısı. Bu sayı tablosu ile ilgili çok yüksekse, columnstore yoğunluğu düşük bir göstergesidir. |
| [COMPRESSED_rowgroup_rows_DELETED] |Satır columnstore biçiminde mantıksal olarak silinir. Tablo boyutunu göre sayı yüksekse, bölümü yeniden oluşturmak veya bu bunları fiziksel olarak kaldırır dizinini yeniden oluşturmayı düşünün. |
| [COMPRESSED_rowgroup_rows_MIN] |Ortalama ve en büyük sütunlarla birlikte, columnstore satır grupları için değerleri aralığı anlamak için bunu kullanın. En iyi duruma getirme veri yükü olarak kullanılabilir yükleme eşiği (hizalı bölüm dağıtım başına 102,400) üzerinden düşük bir sayı önerir |
| [COMPRESSED_rowgroup_rows_MAX] |Yukarıdaki gibi |
| [OPEN_rowgroup_count] |Açık satır grupları normal. Bir tablo dağıtım (60) başına bir açık satır grubu makul beklenir. Aşırı numaraları veri bölümler yükleme önerin. Çift onay emin olmak için bölümleme stratejisine ses |
| [OPEN_rowgroup_rows] |Her satır grubu 1.048.576 satır en fazla olabilir. Doluluk açık satır grupları şu anda olduğunu görmek için bu değeri kullanın |
| [OPEN_rowgroup_rows_MIN] |Gruplar'ı açın, veri akışla yükleniyor tabloya ya da önceki yükleme bu satır grubuna satırları kalan geçmiş olduğunu olduğunu gösterir. Grupları ne kadar veri Aç sat görmek için ortalama sütunları satır, MIN, MAX kullanın. Küçük tablolar için tüm verilerin % 100 olabilir! ALTER INDEX columnstore için veri zorlamak için REBUILD durumda. |
| [OPEN_rowgroup_rows_MAX] |Yukarıdaki gibi |
| [OPEN_rowgroup_rows_AVG] |Yukarıdaki gibi |
| [CLOSED_rowgroup_rows] |Kapalı satır grubu satırları sağlamlık denetimi olarak arayın. |
| [CLOSED_rowgroup_count] |Kapalı satır grupları sayısı herhangi hiç görülüyorsa düşük olmalıdır. Kapalı satır grupları ALTER INDEX kullanarak sıkıştırılmış satır gruplarına dönüştürülebilir... Komut yeniden düzenleme. Ancak, bu normalde gerekli değildir. Kapalı grupları, otomatik olarak arka plan "tanımlama grubu taşıyıcısı" işlem tarafından columnstore satır gruplarına dönüştürülür. |
| [CLOSED_rowgroup_rows_MIN] |Kapalı satır grupları çok yüksek dolgu oranı olması gerekir. Kapalı satır grubu dolgu oranı düşükse çözümlemeler columnstore gereklidir. |
| [CLOSED_rowgroup_rows_MAX] |Yukarıdaki gibi |
| [CLOSED_rowgroup_rows_AVG] |Yukarıdaki gibi |
| [Rebuild_Index_SQL] |Bir tabloda columnstore dizini yeniden oluşturmak için SQL |

## <a name="causes-of-poor-columnstore-index-quality"></a>Zayıf columnstore dizini kalite nedenleri
Zayıf segment kalitesiyle tabloları belirlediyseniz, kök nedenini belirlemek isteyeceksiniz.  Aşağıda zayıf segment kalitesi bazı diğer yaygın nedenler şunlardır:

1. Dizin yapılandırıldığında bellek baskısı
2. Yüksek hacimli DML işlemleri
3. Küçük veya yük işlemleri trickle
4. Çok fazla bölümleri

Bu etkenler satır grubu başına en iyi 1 milyon satır'den küçük bir columnstore dizini önemli ölçüde olması neden olabilir.  Ayrıca bir sıkıştırılmış satır grubu yerine delta satır grubu gitmek satır neden olabilir. 

### <a name="memory-pressure-when-index-was-built"></a>Dizin yapılandırıldığında bellek baskısı
Sıkıştırılmış satır grubu başına satır sayısı, satır grubu işlemek kullanılabilir bellek miktarına ve satır genişliğini doğrudan ilişkilidir.  Satırlar columnstore tablolarına bellek baskısı altında yazıldığında, segment kalitesi düşebilir.  Bu nedenle, en iyi uygulama olarak, mümkün olduğunca kadar bellek columnstore dizini tabloları erişiminizi yazma oturum vermektir.  Bellek ve eşzamanlılık arasında bir denge olduğundan, tablodaki her satır verileri doğru bellek ayırma hakkında yönergeler bağlıdır, sisteminiz ve eşzamanlılık yuva sayısı için ayrılmış data warehouse birimleri oturuma verebilirsiniz; Veri tablonuza yazma.  En iyi uygulama, DW600 ve mediumrc DW400 DW1000 kullanıyorsanız ve yukarıdaki kullanıyorsanız DW300 kullanıyorsanız xlargerc veya daha az, largerc başlangıç öneririz.

### <a name="high-volume-of-dml-operations"></a>Yüksek hacimli DML işlemleri
Güncelleştirme ve satırları silme DML işlemleri yüksek hacimli Etkisizliği columnstore ortaya çıkarabilir. Bir satır grubu satırları çoğunluğu değiştirildiğinde durumlarda özellikle geçerlidir.

* Sıkıştırılmış satır grubundan yalnızca mantıksal olarak bir satırın silinmesi satır silindi olarak işaretler. Bölüm veya tablo yapılana kadar satır sıkıştırılmış satır grubunda kalır.
* Satır ekleme satır için bir delta satır grubu olarak adlandırılan bir iç rowstore tablosuna ekler. Delta satır grubu dolu olduğunda ve kapatıldı olarak işaretlenen kadar columnstore için eklenen satırın dönüştürülmez. Maksimum kapasite 1.048.576 satır ulaştığınızda satır grupları kapatılır. 
* Columnstore biçiminde bir satırı güncelleştirmek, mantıksal bir silme ve ardından bir ekleme işlenir. Eklenen satırın delta deposunda depolanabilir.

Toplu güncelleştirme ve hizalanmış dağıtım doğrudan columnstore biçimine yazılır bölüm başına 102,400 satır toplu eşiğini aşan işlemlere ekleyin. Ancak, bir bile dağıtım varsayıldığında, bunun gerçekleşmesi tek bir işlemde birden fazla 6.144 milyon satır değiştirme gereksiniminiz olacaktır. Belli bir bölüm için satır sayısını hizalı satırları delta mağazaya gidin ve yeterli satır eklenir veya satır grubu kapatmak için değiştirilmiş veya dizin yeniden kadar orada kalacak dağıtım değerinden 102,400 olur.

### <a name="small-or-trickle-load-operations"></a>Küçük veya yük işlemleri trickle
Küçük SQL Data Warehouse akışına bazen bilinir olduğunu yükleri trickle olarak yükler. Bunlar genellikle sistem tarafından alınan bir veri yakın sabit akışı temsil eder. Ancak, bu akış yakın sürekli olarak birimin satır özellikle büyük değil. Daha sık fazla veri önemli ölçüde columnstore biçimine doğrudan yükleme için gereken eşiğin altında değil.

Bu durumda, verileri Azure blob depolamada ilk güden ve yükleme öncesinde birikmesini çalışmasına izin daha iyidir. Bu teknik genellikle olarak bilinen *mikro toplu işleme*.

### <a name="too-many-partitions"></a>Çok fazla bölümleri
Dikkate alınması gereken başka bir şey, kümelenmiş columnstore tabloları bölümleme etkisidir.  Bölümleme önce SQL veri ambarı zaten verilerinizi 60 veritabanlarına olarak böler.  Bölümlendirme, verilerinizi daha fazla böler.  Verilerinizi bölüm sonra düşünmek isteyebilirsiniz **her** bölüm kümelenmiş columnstore dizinden yararlanmak için en az 1 milyon satır olması gerekir.  Tablonuz 100 bölümlere bölüm sonra tablonuz kümelenmiş columnstore dizinden yararlanmak için en az 6 milyon satır olması gerekir (60 dağıtımları * 100 bölümleri * 1 milyon satır). 100 bölüm tablonuz 6 milyon satır yoksa bölümleri sayısını azaltın veya yığın tablo kullanmayı düşünün.

Tablolarınızı bazı verilerle birlikte yüklenmiş sonra izleyin tanımlamak ve tabloları iyinin küme columnstore dizinleri ile yeniden için aşağıdaki adımları.

## <a name="rebuilding-indexes-to-improve-segment-quality"></a>Segment kalitesini artırmak için dizinleri yeniden oluşturma
### <a name="step-1-identify-or-create-user-which-uses-the-right-resource-class"></a>1. adım: Tanımlamak veya sağ kaynak sınıfı kullanan kullanıcı oluştur
Hemen segment kalitesini artırmak için bir hızlı dizini yeniden oluşturma yoludur.  Yukarıdaki görünümü tarafından döndürülen SQL dizinleri yeniden oluşturmak için kullanılan bir ALTER INDEX REBUILD deyimi döndürür.  Dizinleri yeniden oluştururken dizininizi yeniden oturumu için yeterli bellek ayıramadı emin olun.  Bunu yapmak için önerilen en düşük bu tabloya dizini yeniden oluşturmak için izinlere sahip bir kullanıcı kaynak sınıfının artırın.  Sistemde bir kullanıcı oluşturmadıysanız, önce bunun gerekir, veritabanı sahibi kullanıcı kaynak sınıfının değiştirilemez.  DW600 ve mediumrc DW400 DW1000 kullanıyorsanız ve yukarıdaki kullanıyorsanız öneririz xlargerc DW300 kullanıyorsanız veya daha az, largerc gereksinimdir.

Aşağıda, bir kullanıcıya kendi kaynak sınıfı artırarak daha fazla bellek ayırmayı örneğidir.  Kaynak hakkında daha fazla bilgi için sınıflar ve yeni bir kullanıcı oluşturmak nasıl bulunabilir [eşzamanlılık ve iş yükü yönetimi] [ Concurrency] makalesi.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>2. adım: yüksek kaynak sınıfı kullanıcıyla kümelenmiş columnstore dizinleri yeniden oluştur
Şimdi daha yüksek bir kaynak sınıfı kullanarak ve ALTER INDEX deyimi yürütme olarak oturum açma kullanıcı adım 1 (örneğin LoadUser).  Bu kullanıcı dizini burada yeniden oluşturuluyorsa tablolara ALTER iznine sahip olduğundan emin olun.  Bu, tüm columnstore dizini yeniden oluşturun veya tek bir bölüm yeniden örnekler. Bir kerede tek bir bölüm yeniden oluşturmak için daha fazla pratik dizinler olduğu büyük tablolar üzerinde.

Alternatif olarak, dizini yeniden derlemeyi yerine, tabloyu kullanarak yeni bir tablo için kopyalayabilir [CTAS][CTAS].  Hangi yolla en iyisidir? Büyük veri, birimleri için [CTAS] [ CTAS] genellikle hızlıdır [ALTER INDEX][ALTER INDEX]. Veri, daha küçük birimler için [ALTER INDEX] [ ALTER INDEX] kullanmak daha kolay olur ve Tablo değiştirme gerektirir olmaz.  Bkz: **CTAS ve bölüm değiştirme dizinleri yeniden oluşturma** aşağıda CTAS ile dizinleri yeniden oluştur konusunda daha fazla ayrıntı için.

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

SQL Data warehouse'da bir dizini yeniden oluşturma çevrimdışı bir işlemdir.  ALTER INDEX REBUILD bölümünde dizinleri yeniden oluşturma hakkında daha fazla bilgi için bkz: [Columnstore dizinleri birleştirme][Columnstore Indexes Defragmentation], ve [ALTER INDEX] [ ALTER INDEX].

### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>3. adım: Kümelenmiş columnstore segment kalite geliştirilmiştir doğrulayın
Yeniden çalıştırılan sorgunun düşük tanımlanan hangi tabloyla kalite segment ve segment kalite doğrulayın geliştirilmiştir.  Segment kalitesini artırmak değil ise, tablosundaki satırları çok geniş olabilir.  Dizinleri yeniden oluştururken daha yüksek kaynak sınıfı veya DWU kullanmayı düşünün.

## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>CTAS ve bölüm değiştirme dizinleri yeniden oluşturma
Bu örnekte [CTAS] [ CTAS] ve bölüm tablo bölümünü yeniden oluşturmak için değiştirme. 

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

-- Step 2: Create a SWITCH out table
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT the data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN the rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2;
```

Kullanarak bölümleri yeniden oluşturma hakkında daha fazla ayrıntı için `CTAS`, bkz: [bölüm] [ Partition] makalesi.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek için üzerinde makalelerine bakın [tablo genel bakışı][Overview], [tablo veri türleri][Data Types], [tablo dağıtma][Distribute], [bir tablo bölümleme][Partition], [Bakımı tablo istatistikleri] [ Statistics] ve [geçici tablolar][Temporary].  En iyi uygulamalar hakkında daha fazla bilgi için bkz: [SQL veri ambarı en iyi uygulamalar][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./resource-classes-for-workload-management.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[ALTER INDEX]: https://msdn.microsoft.com/library/ms188388.aspx
[heap]: https://msdn.microsoft.com/library/hh213609.aspx
[clustered indexes and nonclustered indexes]: https://msdn.microsoft.com/library/ms190457.aspx
[create table syntax]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore Indexes Defragmentation]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[clustered columnstore indexes]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
