---
title: "İş yükü yönetimi - Azure SQL Data Warehouse için kaynak sınıfları | Microsoft Docs"
description: "Eşzamanlılık yönetmek ve işlem kaynaklarını Azure SQL Data Warehouse sorguları için kaynak sınıfları kullanmaya yönelik kılavuz."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/23/2017
ms.author: joeyong;barbkess;kavithaj
ms.openlocfilehash: 122646f73b6e4e7c62eb0e6d4b6672b603d8acb2
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="resource-classes-for-workload-management"></a>İş yükü yönetimi için kaynak sınıfları
Eşzamanlı olarak çalıştıran ve işlem kaynaklarını Azure SQL Data Warehouse sorguları için eş zamanlı sorguları sayısını yönetmek için kaynak sınıfları kullanmaya yönelik kılavuz.
 
## <a name="what-is-workload-management"></a>İş yükü yönetimi nedir?
İş yükü, tüm sorguları genel performansını en iyi duruma getirme olanağı yönetimidir. İyi bizi iş yükü, sorgular ve işlem yoğunluklu veya g/ç yoğunluklu olup olmadıkları bakılmaksızın verimli bir şekilde yük işlemler çalışır. 

SQL veri ambarı çok kullanıcılı ortamlar için iş yükü yönetimi özelliklerini sağlar. Veri ambarı çok Kiracı İş yükleri için tasarlanmamıştır.

## <a name="what-are-resource-classes"></a>Kaynak sınıfları nelerdir?
Kaynak, sorgu yürütme yöneten önceden belirlenen kaynak sınırları sınıflarıdır. SQL veri ambarı işlem kaynakları kaynak sınıfı göre her bir sorgu için sınırlar. 

Kaynak, veri ambarı iş yükü genel performansını yönetmenize yardımcı sınıfları. Kaynak sınıflarını etkili bir şekilde kullanma eşzamanlı olarak çalışan sorguları ve her sorgu için atanan işlem kaynakları sayısı sınırları ayarlayarak İş yükünüzün yönetmenize yardımcı olur. 

- Daha küçük kaynak sınıfları daha az işlem kaynaklarını kullanır ancak büyük genel sorgu eşzamanlılık etkinleştir
- Daha fazla işlem kaynaklarını ancak sorgu eşzamanlılık kısıtlamak büyük kaynak sınıfları sağlar

Kaynak sınıfları veri yönetim ve düzenleme etkinlikleri için tasarlanmıştır. Büyük birleştirmeler ve varken sıralar sistem diskine değişkenlere yerine bellek Sorguyu yürüten böylece bazı çok karmaşık sorgular ayrıca yararlanabilir.

Aşağıdaki işlemleri kaynak sınıfları tarafından yönetilir:

* EKLE-SELECT, UPDATE, DELETE
* (Kullanıcı tablosu sorgulanırken) seçin
* ALTER INDEX - yeniden oluşturma veya yeniden Düzenle
* ALTER TABLE YENİDEN OLUŞTURMA
* DİZİN OLUŞTURMA
* KÜMELENMİŞ COLUMNSTORE DİZİNİ OLUŞTURMA
* TABLE AS SELECT (CTAS) OLUŞTURMA
* Veri yükleme
* Veri Taşıma hizmeti (DMS) tarafından gerçekleştirilen veri taşıma işlemleri

> [!NOTE]  
> Dinamik Yönetim görünümlerini (Dmv'leri) deyimlerinde veya diğer sistem görünümleri herhangi bir eşzamanlılık sınırları tarafından yönetilmeyen seçin. Sistem sorguları üzerinde yürütme sayısından bağımsız olarak izleyebilirsiniz.
> 
> 

## <a name="static-and-dynamic-resource-classes"></a>Statik ve dinamik kaynak sınıfları

İki tür kaynak sınıfı vardır: dinamik ve statik.

- **Statik kaynak sınıflar** bellek ölçülür geçerli hizmet düzeyi bakılmaksızın aynı miktarda tahsis [veri ambarı birimlerini](what-is-a-data-warehouse-unit-dwu-cdwu.md). Bu statik ayırma daha büyük hizmet düzeylerini, her kaynak sınıfında daha fazla sorguları çalıştırabileceğiniz anlamına gelir.  Statik kaynak sınıfları staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70 ve staticrc80 olarak adlandırılır. Bu kaynak sınıfları başka işlem kaynakları almak için kaynak sınıfı artıran çözümleri için uygundur.

- **Dinamik kaynak sınıflar** değişken bir hizmet düzeyine bağlı olarak bellek miktarı ayırın. Daha büyük bir hizmet düzeyine ölçeklendirmek, sorgularınızı daha fazla bellek otomatik olarak alın. Dinamik kaynak sınıfları smallrc, mediumrc, largerc ve xlargerc olarak adlandırılır. Bu kaynak sınıfları hangi artış ek kaynaklar almak için ölçek compute çözümleri için uygundur. 

[Performans katmanı](performance-tiers.md) aynı kaynak sınıf adlarını kullanın, ancak farklı [bellek ve eşzamanlılık belirtimleri](performance-tiers.md). 


## <a name="assigning-resource-classes"></a>Atama Kaynağı sınıfları

Kaynak sınıfları veritabanı rollere kullanıcıları atayarak uygulanır. Bir kullanıcı bir sorgu çalıştırıldığında, sorgu kullanıcının kaynak sınıf ile çalışır. Örneğin, bir kullanıcı smallrc veya staticrc10 veritabanı rolünün bir üyesi olduğunda sorgularını küçük miktarlarda bellek ile çalıştırın. Bir veritabanı kullanıcısı xlargerc veya staticrc80 veritabanı rollerinin bir üyesi olduğunda, sorgularını büyük miktarlarda bellek ile çalıştırın. 

Bir kullanıcının kaynak sınıfı artırmak için saklı yordam kullanın [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql). 

```sql
EXEC sp_addrolemember 'largerc', 'loaduser';
```

Kaynak sınıfı azaltmak için kullanmak [sp_droprolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-droprolemember-transact-sql).  

```sql
EXEC sp_droprolemember 'largerc', 'loaduser';
```

Hizmet Yöneticisi kaynak sınıfının sabittir ve değiştirilemez.  Sağlama işlemi sırasında oluşturulan kullanıcı hizmet yöneticisidir.

### <a name="default-resource-class"></a>Varsayılan kaynak sınıfı
Varsayılan olarak, her kullanıcı küçük kaynak sınıfı üyesidir **smallrc**. 

### <a name="resource-class-precedence"></a>Kaynak sınıf önceliği
Kullanıcılar, birden çok kaynak sınıflarının üyelerini olabilir. Ne zaman bir kullanıcı birden fazla kaynak sınıfına ait:

- Dinamik kaynak sınıfları statik kaynak sınıfları göre önceliklidir. Örneğin, bir kullanıcı mediumrc(dynamic) ve staticrc80 (statik) bir üyesi ise, mediumrc ile sorgular çalıştırın.
- Daha büyük kaynak sınıfları küçük kaynak sınıfları göre önceliklidir. Örneğin, bir kullanıcı mediumrc ve largerc bir üyesi ise, largerc ile sorgular çalıştırın. Benzer şekilde, bir kullanıcı staticrc20 ve statirc80 bir üyesi ise, sorguları staticrc80 kaynak ayırma ile çalışır.

### <a name="queries-exempt-from-resource-classes"></a>Kaynak sınıflardan muafiyet sorgular
Kullanıcı daha büyük bir kaynak sınıfı üyesi olsa bile bazı sorgular smallrc kaynak sınıfında her zaman çalışır. Bu muafiyet sorgular eşzamanlılık sınırında sayılmaz. Eşzamanlılık sınırı 16 ise, örneğin, çok sayıda kullanıcı sistem görünümleri kullanılabilir eşzamanlılık yuvaları etkilemeden seçerek.

Aşağıdaki deyimleri kaynak sınıflardan muaf tutulan ve her zaman içinde smallrc çalıştırın:

* DROP TABLE veya oluşturma
* ALTER TABLE... ANAHTAR, bölme veya bölüm birleştirme
* ALTER INDEX DEVRE DIŞI BIRAK
* DROP INDEX
* OLUŞTURMA, güncelleştirme ya da DROP STATISTICS
* TRUNCATE TABLE
* ALTER YETKİLENDİRME
* OTURUM AÇMA OLUŞTURUN
* CREATE, ALTER ve DROP USER
* OLUŞTURMA, değiştirme veya bırakma yordamı
* OLUŞTURMA veya DROP VIEW
* DEĞER EKLEME
* Sistem görünümleri ve Dmv'leri seçin
* AÇIKLANMAKTADIR
* DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="recommendations"></a>Öneriler
Biz belirli bir sorgu türü çalıştırmaya adanmış bir kullanıcı oluşturulması önerilir ya da işlemleri yükleyin. Ardından o kullanıcı kaynak sınıfı düzenli aralıklarla değiştirme yerine kalıcı kaynak sınıfı verin. İş yükü hakkında daha fazla genel denetim statik kaynak sınıfları göze koşuluyla, dinamik kaynak sınıfları belirlemeden önce bu ilk kullanılarak da öneririz.

### <a name="resource-classes-for-load-users"></a>Yük kullanıcılar için kaynak sınıfları
`CREATE TABLE`Varsayılan olarak kullanan kümelenmiş columnstore dizinleri. Bir columnstore verileri sıkıştırma dizin bellek yoğunluklu bir işlemdir ve bellek baskısı dizin kalitesini düşürebilir. Bu nedenle, daha yüksek bir kaynak sınıfı verileri yüklenirken gerektirecek şekilde olasılığı daha yüksektir. Yükleri yeterli belleğe sahip olmak için yüklerini çalıştırmak için tasarlanmış bir kullanıcı oluşturun ve bu kullanıcıyı daha yüksek bir kaynak sınıfı atayın.

Yüklerini verimli bir şekilde işlemek için gereken bellek yüklenen tablo ve veri boyutu yapısını bağlıdır. Bellek gereksinimleri hakkında daha fazla bilgi için bkz: [satır grubu kimliğinde kalite en üst düzeye](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

Bellek gereksinimi belirledikten sonra bir statik veya dinamik kaynak sınıfına yük kullanıcıya atamak seçin.

- Tablo bellek gereksinimlerini belirli bir aralıkta statik kaynak sınıfı kullanın. Yükleri uygun bellek ile çalıştırın. Veri ambarı ölçeklendirdiğinizde yükleri daha fazla bellek gerekli değildir. Bir statik kaynak sınıfını kullanarak bellek ayırmaları sabit kalır. Bu tutarlılık bellek korur ve daha fazla sorguları aynı anda çalışmasına izin verir. Yeni çözümler statik kaynak sınıfları kullanmanızı öneririz ilk olarak bunlar daha fazla denetim sağlar.
- Tablo bellek gereksinimleri büyük ölçüde farklılık, dinamik kaynak sınıfını kullanın. Yükleri geçerli DWU daha fazla bellek gerektirebilir veya cDWU düzeyi sağlar. Bu nedenle, veri ambarı ölçeklendirme daha fazla bellek yükleme işlemleri için daha hızlı gerçekleştirmek yükleri sağlayan ekler.

### <a name="resource-classes-for-queries"></a>Sorgular için kaynak sınıfları

İşlem yoğunluklu bazı sorgular ve bazı değildir.  

- Sorguları karmaşıktır, ancak yüksek eşzamanlılık gerekmez dinamik kaynak sınıfı seçin.  Örneğin, günlük veya haftalık raporları üretme bir arada sırada kaynaklar için gerekiyor. Raporları büyük miktarlarda veri işleme, veri ambarı ölçeklendirme kullanıcının var olan kaynak sınıfı için daha fazla bellek sağlar.
- Kaynak beklentilerini gün boyunca farklılık statik kaynak sınıfı seçin. Örneğin, iyi veri ambarı birçok kişi tarafından sorgulandığında statik kaynak sınıfı çalışır. Veri ambarı ölçekleme sırasında kullanıcıya ayrılmış bellek miktarı değiştirmez. Sonuç olarak, daha fazla sorguları paralel sistem üzerinde çalıştırılabilir.

Sorgulanan veri miktarı, tablo şemalarını ve çeşitli birleştirme doğası seçin ve koşulları Grup gibi uygun bellek ataması seçerek birçok faktöre bağlıdır. Genel olarak, daha fazla bellek ayırma daha hızlı tamamlamak için sorgular verir, ancak Genel eşzamanlılık azaltır. Eşzamanlılık sorunu değilse, aşırı bellek ayırma verimlilik zarar değil. 

Performansı ayarlamak için farklı bir kaynak sınıflarını kullanın. Sonraki bölüm yardımcı olan bir saklı yordam şekil en iyi kaynak sınıfı sağlar.

## <a name="example-code-for-finding-the-best-resource-class"></a>En iyi kaynak sınıfı bulmak için örnek kod
 
Belirli bir SLO kaynak sınıfı ve bellek yoğun CCI tablosu üzerinde işlem bölümlenmemiş CCI belirtilen kaynak sınıfı en yakın en iyi kaynak sınıfı başına eşzamanlılık ve bellek grant ekleneceğini aşağıdaki depolanan yordamı kullanabilirsiniz:

Bu saklı yordam amacı şöyledir:  
1. Eşzamanlılık ve belirli bir SLO kaynak sınıfı başına vermek bellek görmek için. Kullanıcı Bu örnekte gösterildiği gibi NULL hem şema hem de tablename için sağlaması gerekir.  
2. En yakın iyi kaynak sınıfı için bellek kullanımı yoğun CCI görmek için belirtilen kaynak sınıfı olmayan bölümlenmiş CCI işlemleri (yük, kopyalama tablo yeniden dizin, vb.) tablosu. Saklı yordam tablo şemasını gerekli bellek ataması bulmak için kullanır.

### <a name="dependencies--restrictions"></a>Bağımlılıklar ve sınırlamalar:
- Bu saklı yordam bölümlenmiş cci tablo bellek gereksinimini hesaplamak üzere tasarlanmamıştır.    
- Bu saklı yordam Ekle/CTAS-seçin seçme bölümü bellek gereksinimini dikkate almaz ve bir SELECT olduğunu varsayar.
- Bu saklı yordam Bu saklı yordam oluşturulduğu oturumunda kullanılabilir geçici bir tablo kullanır.    
- Bu değişiklikler, daha sonra bu saklı yordam düzgün çalışmaz ve bu saklı yordam geçerli teklifleri (örneğin, donanım yapılandırması, DMS config) bağlıdır.  
- Değişirse, ardından bu saklı yordam düzgün çalışmayan ve bu saklı yordam varolan sunulan eşzamanlılık sınırı bağlıdır.  
- Değişirse, ardından bu saklı yordam düzgün çalışmayan ve mevcut kaynak sınıfı tekliflerini Bu saklı yordam bağlıdır.  

>  [!NOTE]  
>  Saklı yordam sağlanan parametrelerle yürüttükten sonra çıktı almıyorsanız, ardından olabilir iki durumda. <br />1. Her iki DW parametresi geçersiz bir SLO değer içeriyor <br />2. Ya da tablo CCI işlemi için eşleşen bir kaynak sınıf yok. <br />Örneğin, DW100 en yüksek bellek ataması 400 MB olduğundan ve tablo şemasını 400 MB gereksinim arası için yeterince geniş olduğunda.
      
### <a name="usage-example"></a>Kullanım örneği:
Sözdizimi:  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. @DWU:Ya geçerli DWU DW DB'den ayıklamak veya desteklenen tüm DWU 'DW100' biçiminde sağlamak için bir NULL parametresi sağlayın
2. @SCHEMA_NAME:Tablo şema adını belirtin
3. @TABLE_NAME:İlgilendiğiniz bir tablo adı sağlayın

Bu saklı yordam yürütme örnekler:  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

Aşağıdaki deyim, Yukarıdaki örneklerde kullanılan tablo1 oluşturur.
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

### <a name="stored-procedure-definition"></a>Saklı yordam tanımı

```sql  
-------------------------------------------------------------------------------
-- Dropping prc_workload_management_by_DWU procedure if it exists.
-------------------------------------------------------------------------------
IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'prc_workload_management_by_DWU')
DROP PROCEDURE dbo.prc_workload_management_by_DWU
GO

-------------------------------------------------------------------------------
-- Creating prc_workload_management_by_DWU.
-------------------------------------------------------------------------------
CREATE PROCEDURE dbo.prc_workload_management_by_DWU
(@DWU VARCHAR(7),
 @SCHEMA_NAME VARCHAR(128),
 @TABLE_NAME VARCHAR(128)
)
AS
IF @DWU IS NULL
BEGIN
-- Selecting proper DWU for the current DB if not specified.
SET @DWU = (
  SELECT 'DW'+CAST(COUNT(*)*100 AS VARCHAR(10))
  FROM sys.dm_pdw_nodes
  WHERE type = 'COMPUTE')
END

DECLARE @DWU_NUM INT
SET @DWU_NUM = CAST (SUBSTRING(@DWU, 3, LEN(@DWU)-2) AS INT)

-- Raise error if either schema name or table name is supplied but not both them supplied
--IF ((@SCHEMA_NAME IS NOT NULL AND @TABLE_NAME IS NULL) OR (@TABLE_NAME IS NULL AND @SCHEMA_NAME IS NOT NULL))
--     RAISEERROR('User need to supply either both Schema Name and Table Name or none of them')
       
-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END

-- Creating ref. temp table (CTAS) to hold mapping info.
-- CREATE TABLE #ref
CREATE TABLE #ref
WITH (DISTRIBUTION = ROUND_ROBIN)
AS 
WITH
-- Creating concurrency slots mapping for various DWUs.
alloc
AS
(
  SELECT 'DW100' AS DWU, 4 AS max_queries, 4 AS max_slots, 1 AS slots_used_smallrc, 1 AS slots_used_mediumrc,
        2 AS slots_used_largerc, 4 AS slots_used_xlargerc, 1 AS slots_used_staticrc10, 2 AS slots_used_staticrc20,
        4 AS slots_used_staticrc30, 4 AS slots_used_staticrc40, 4 AS slots_used_staticrc50,
        4 AS slots_used_staticrc60, 4 AS slots_used_staticrc70, 4 AS slots_used_staticrc80
  UNION ALL
    SELECT 'DW200', 8, 8, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW300', 12, 12, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW400', 16, 16, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
     SELECT 'DW500', 20, 20, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW600', 24, 24, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW1000', 32, 40, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1200', 32, 48, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1500', 32, 60, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW2000', 32, 80, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
   SELECT 'DW3000', 32, 120, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW6000', 32, 240, 1, 32, 64, 128, 1, 2, 4, 8, 16, 32, 64, 128
)
-- Creating workload mapping to their corresponding slot consumption and default memory grant.
,map
AS
(
  SELECT 'SloDWGroupC00' AS wg_name,1 AS slots_used,100 AS tgt_mem_grant_MB
  UNION ALL
    SELECT 'SloDWGroupC01',2,200
  UNION ALL
    SELECT 'SloDWGroupC02',4,400
  UNION ALL
    SELECT 'SloDWGroupC03',8,800
  UNION ALL
    SELECT 'SloDWGroupC04',16,1600
  UNION ALL
    SELECT 'SloDWGroupC05',32,3200
  UNION ALL
    SELECT 'SloDWGroupC06',64,6400
  UNION ALL
    SELECT 'SloDWGroupC07',128,12800
)
-- Creating ref based on current / asked DWU.
, ref
AS
(
  SELECT  a1.*
  ,       m1.wg_name          AS wg_name_smallrc
  ,       m1.tgt_mem_grant_MB AS tgt_mem_grant_MB_smallrc
  ,       m2.wg_name          AS wg_name_mediumrc
  ,       m2.tgt_mem_grant_MB AS tgt_mem_grant_MB_mediumrc
  ,       m3.wg_name          AS wg_name_largerc
  ,       m3.tgt_mem_grant_MB AS tgt_mem_grant_MB_largerc
  ,       m4.wg_name          AS wg_name_xlargerc
  ,       m4.tgt_mem_grant_MB AS tgt_mem_grant_MB_xlargerc
  ,       m5.wg_name          AS wg_name_staticrc10
  ,       m5.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc10
  ,       m6.wg_name          AS wg_name_staticrc20
  ,       m6.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc20
  ,       m7.wg_name          AS wg_name_staticrc30
  ,       m7.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc30
  ,       m8.wg_name          AS wg_name_staticrc40
  ,       m8.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc40
  ,       m9.wg_name          AS wg_name_staticrc50
  ,       m9.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc50
  ,       m10.wg_name          AS wg_name_staticrc60
  ,       m10.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc60
  ,       m11.wg_name          AS wg_name_staticrc70
  ,       m11.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc70
  ,       m12.wg_name          AS wg_name_staticrc80
  ,       m12.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc80
  FROM alloc a1
  JOIN map   m1  ON a1.slots_used_smallrc     = m1.slots_used
  JOIN map   m2  ON a1.slots_used_mediumrc    = m2.slots_used
  JOIN map   m3  ON a1.slots_used_largerc     = m3.slots_used
  JOIN map   m4  ON a1.slots_used_xlargerc    = m4.slots_used
  JOIN map   m5  ON a1.slots_used_staticrc10    = m5.slots_used
  JOIN map   m6  ON a1.slots_used_staticrc20    = m6.slots_used
  JOIN map   m7  ON a1.slots_used_staticrc30    = m7.slots_used
  JOIN map   m8  ON a1.slots_used_staticrc40    = m8.slots_used
  JOIN map   m9  ON a1.slots_used_staticrc50    = m9.slots_used
  JOIN map   m10  ON a1.slots_used_staticrc60    = m10.slots_used
  JOIN map   m11  ON a1.slots_used_staticrc70    = m11.slots_used
  JOIN map   m12  ON a1.slots_used_staticrc80    = m12.slots_used
-- WHERE   a1.DWU = @DWU
  WHERE   a1.DWU = UPPER(@DWU)
)
SELECT  DWU
,       max_queries
,       max_slots
,       slots_used
,       wg_name
,       tgt_mem_grant_MB
,       up1 as rc
,       (ROW_NUMBER() OVER(PARTITION BY DWU ORDER BY DWU)) as rc_id
FROM
(
    SELECT  DWU
    ,       max_queries
    ,       max_slots
    ,       slots_used
    ,       wg_name
    ,       tgt_mem_grant_MB
    ,       REVERSE(SUBSTRING(REVERSE(wg_names),1,CHARINDEX('_',REVERSE(wg_names),1)-1)) as up1
    ,       REVERSE(SUBSTRING(REVERSE(tgt_mem_grant_MBs),1,CHARINDEX('_',REVERSE(tgt_mem_grant_MBs),1)-1)) as up2
    ,       REVERSE(SUBSTRING(REVERSE(slots_used_all),1,CHARINDEX('_',REVERSE(slots_used_all),1)-1)) as up3
    FROM    ref AS r1
    UNPIVOT
    (
        wg_name FOR wg_names IN (wg_name_smallrc,wg_name_mediumrc,wg_name_largerc,wg_name_xlargerc,
        wg_name_staticrc10, wg_name_staticrc20, wg_name_staticrc30, wg_name_staticrc40, wg_name_staticrc50,
        wg_name_staticrc60, wg_name_staticrc70, wg_name_staticrc80)
    ) AS r2
    UNPIVOT
    (
        tgt_mem_grant_MB FOR tgt_mem_grant_MBs IN (tgt_mem_grant_MB_smallrc,tgt_mem_grant_MB_mediumrc,
        tgt_mem_grant_MB_largerc,tgt_mem_grant_MB_xlargerc, tgt_mem_grant_MB_staticrc10, tgt_mem_grant_MB_staticrc20,
        tgt_mem_grant_MB_staticrc30, tgt_mem_grant_MB_staticrc40, tgt_mem_grant_MB_staticrc50,
        tgt_mem_grant_MB_staticrc60, tgt_mem_grant_MB_staticrc70, tgt_mem_grant_MB_staticrc80)
    ) AS r3
    UNPIVOT
    (
        slots_used FOR slots_used_all IN (slots_used_smallrc,slots_used_mediumrc,slots_used_largerc,
        slots_used_xlargerc, slots_used_staticrc10, slots_used_staticrc20, slots_used_staticrc30,
        slots_used_staticrc40, slots_used_staticrc50, slots_used_staticrc60, slots_used_staticrc70,
        slots_used_staticrc80)
    ) AS r4
) a
WHERE   up1 = up2
AND     up1 = up3
;
-- Getting current info about workload groups.
WITH  
dmv  
AS  
(
  SELECT
          rp.name                                           AS rp_name
  ,       rp.max_memory_kb*1.0/1048576                      AS rp_max_mem_GB
  ,       (rp.max_memory_kb*1.0/1024)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_MB
  ,       (rp.max_memory_kb*1.0/1048576)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_GB
  ,       wg.name                                           AS wg_name
  ,       wg.importance                                     AS importance
  ,       wg.request_max_memory_grant_percent               AS request_max_memory_grant_percent
  FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
  JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    ON  wg.pdw_node_id  = rp.pdw_node_id
                                                                  AND wg.pool_id      = rp.pool_id
  WHERE   rp.name = 'SloDWPool'
  GROUP BY
          rp.name
  ,       rp.max_memory_kb
  ,       wg.name
  ,       wg.importance
  ,       wg.request_max_memory_grant_percent
)
-- Creating resource class name mapping.
,names
AS
(
  SELECT 'smallrc' as resource_class, 1 as rc_id
  UNION ALL
    SELECT 'mediumrc', 2
  UNION ALL
    SELECT 'largerc', 3
  UNION ALL
    SELECT 'xlargerc', 4
  UNION ALL
    SELECT 'staticrc10', 5
  UNION ALL
    SELECT 'staticrc20', 6
  UNION ALL
    SELECT 'staticrc30', 7
  UNION ALL
    SELECT 'staticrc40', 8
  UNION ALL
    SELECT 'staticrc50', 9
  UNION ALL
    SELECT 'staticrc60', 10
  UNION ALL
    SELECT 'staticrc70', 11
  UNION ALL
    SELECT 'staticrc80', 12
)
,base AS
(   SELECT  schema_name
    ,       table_name
    ,       SUM(column_count)                   AS column_count
    ,       ISNULL(SUM(short_string_column_count),0)   AS short_string_column_count
    ,       ISNULL(SUM(long_string_column_count),0)    AS long_string_column_count
    FROM    (   SELECT  sm.name                                             AS schema_name
                ,       tb.name                                             AS table_name
                ,       COUNT(co.column_id)                                 AS column_count
                           ,       CASE    WHEN co.system_type_id IN (36,43,106,108,165,167,173,175,231,239) 
                                AND  co.max_length <= 32 
                                THEN COUNT(co.column_id) 
                        END                                                 AS short_string_column_count
                ,       CASE    WHEN co.system_type_id IN (165,167,173,175,231,239) 
                                AND  co.max_length > 32 and co.max_length <=8000
                                THEN COUNT(co.column_id) 
                        END                                                 AS long_string_column_count
                FROM    sys.schemas AS sm
                JOIN    sys.tables  AS tb   on sm.[schema_id] = tb.[schema_id]
                JOIN    sys.columns AS co   ON tb.[object_id] = co.[object_id]
                           WHERE tb.name = @TABLE_NAME AND sm.name = @SCHEMA_NAME
                GROUP BY sm.name
                ,        tb.name
                ,        co.system_type_id
                ,        co.max_length            ) a
GROUP BY schema_name
,        table_name
)
, size AS
(
SELECT  schema_name
,       table_name
,       75497472                                            AS table_overhead

,       column_count*1048576*8                              AS column_size
,       short_string_column_count*1048576*32                       AS short_string_size,       (long_string_column_count*16777216) AS long_string_size
FROM    base
UNION
SELECT CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as schema_name
         ,CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as table_name
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as table_overhead
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as column_size
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as short_string_size

,CASE WHEN COUNT(*) = 0 THEN 0 END as long_string_size
FROM   base
)
, load_multiplier as 
(
SELECT  CASE 
                     WHEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) > 0 THEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) 
                     ELSE 1 
              END AS multipliplication_factor
) 
       SELECT  r1.DWU
       , schema_name
       , table_name
       , rc.resource_class as closest_rc_in_increasing_order
       , max_queries_at_this_rc = CASE
             WHEN (r1.max_slots / r1.slots_used > r1.max_queries)
                  THEN r1.max_queries
             ELSE r1.max_slots / r1.slots_used
                  END
       , r1.max_slots as max_concurrency_slots
       , r1.slots_used as required_slots_for_the_rc
       , r1.tgt_mem_grant_MB  as rc_mem_grant_MB
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB       
       FROM    size, load_multiplier, #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```




## <a name="next-steps"></a>Sonraki adımlar
Veritabanı kullanıcıları yönetme ve güvenlik hakkında daha fazla bilgi için bkz: [SQL veri ambarı veritabanında güvenli][Secure a database in SQL Data Warehouse]. Ne kadar büyük kaynak sınıfları hakkında daha fazla bilgi kümelenmiş columnstore dizini kalitesini artırmak, bkz [columnstore sıkıştırma için bellek iyileştirmeleri](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Rebuilding indexes to improve segment quality]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
