---
title: İş yükü yönetimi - Azure SQL veri ambarı için kaynak sınıfları | Microsoft Docs
description: Eşzamanlılık yönetmek ve işlem kaynakları için sorgular Azure SQL veri ambarı için kaynak sınıfları kullanma yönergeleri.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload management
ms.date: 05/22/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: 75bd6e8071717ba755b71f51afcd884539049489
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66165973"
---
# <a name="workload-management-with-resource-classes-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'nda kaynak sınıfları ile iş yükü yönetimi

Bellek ve sorgular, Azure SQL veri ambarı için eşzamanlılık yönetmek için kaynak sınıfları kullanmaya yönelik yönergeler.  

## <a name="what-is-workload-management"></a>İş yükü yönetimi nedir

İş yükü yönetimi tüm sorguların genel performansını en iyi duruma getirme olanağı sağlar. İyi ayarlanmış bir iş yükü sorguları çalıştırır ve yoğun işlem gücü kullanımlı veya yoğun g/ç olmalarından işlemleri verimli bir şekilde yükleyin. SQL veri ambarı, çok kullanıcılı ortamlar için iş yükü yönetimi özellikleri sağlar. Veri ambarı, çok kiracılı iş yükleri için tasarlanmamıştır.

Veri ambarı performans kapasite tarafından belirlenir [veri ambarı birimleri](what-is-a-data-warehouse-unit-dwu-cdwu.md).

- Tüm performans profilleri için bellek ve eşzamanlılık sınırları görüntülemek için bkz: [bellek ve eşzamanlılık sınırları](memory-and-concurrency-limits.md).
- Performans kapasitesine ayarlamak için şunları yapabilirsiniz: [ölçeği artırın veya azaltın](quickstart-scale-compute-portal.md).

Bir sorgu performansı kapasitesini sorgunun kaynak sınıfı tarafından belirlenir. Bu makalenin geri kalanında, kaynak sınıfları nedir ve bunları nasıl ayarlayabileceğinizi açıklanmaktadır.

## <a name="what-are-resource-classes"></a>Kaynak sınıfları nelerdir

Sorgu performans kapasitesi, kullanıcının kaynak sınıfı tarafından belirlenir.  Kaynak sınıfları yöneten kaynak sınırları Azure SQL veri ambarı işlem kaynakları ve sorgu yürütme için eşzamanlılık önceden belirler. Kaynak sınıfları, eşzamanlı olarak çalışan sorguları sayısı ve her sorgu için atanan işlem kaynakları üzerinde sınırları ayarlayarak iş yükünüzü yönetmenize yardımcı olabilir.  Bellek ve eşzamanlılık arasında bir ilişki yoktur.

- Daha küçük kaynak sınıfları, sorgu başına en fazla belleği azaltır, ancak eşzamanlılığı.
- Daha büyük kaynak sınıfları, sorgu başına bellek üst sınırını artırın, ancak eşzamanlılık azaltın.

Kaynak sınıfları iki tür vardır:

- Statik kaynaklar sınıflar eşzamanlı sabit bir veri kümesi boyutu için uygundur.
- Dinamik kaynak sınıfı olarak boyutundaki büyüyor ve hizmet düzeyi ölçeği gibi performans artışı gerekiyor. veri kümeleri için uygundur.

Kaynak sınıfları, kaynak tüketimine ölçmek için eşzamanlılık yuvaları kullanın.  [Eşzamanlılık yuvaları](#concurrency-slots) bu makalenin sonraki bölümlerinde açıklanmıştır.

- Kaynak sınıfları için kaynak kullanımını görüntülemek için bkz: [bellek ve eşzamanlılık sınırları](memory-and-concurrency-limits.md#concurrency-maximums).
- Kaynak sınıfı ayarlamak için farklı bir kullanıcı altında sorgu çalıştırabilirsiniz veya [geçerli kullanıcının kaynak sınıfını değiştirin](#change-a-users-resource-class) üyelik.

### <a name="static-resource-classes"></a>Statik kaynak sınıfları

Statik kaynak sınıfları ayırmanız bellek ölçülür geçerli performans düzeyi bakılmaksızın aynı miktarda [veri ambarı birimleri](what-is-a-data-warehouse-unit-dwu-cdwu.md). Sorgu performans düzeyini ne olursa olsun aynı bellek ayırma aldığından [veri ambarı ölçeklendirme](quickstart-scale-compute-portal.md) bir kaynak sınıfı içinde çalıştırmak daha fazla sorguları sağlar.  Statik kaynak sınıfları, veri hacmi biliniyorsa ideal ve sabit.

Statik kaynak sınıfları ile bu önceden tanımlanmış veritabanı rolleri uygulanır:

- staticrc10
- staticrc20
- staticrc30
- staticrc40
- staticrc50
- staticrc60
- staticrc70
- staticrc80

### <a name="dynamic-resource-classes"></a>Dinamik kaynak sınıfları

Bir değişken miktarda bellek geçerli hizmet düzeyine bağlı olarak dinamik kaynak sınıfları ayırın. Statik kaynak sınıfları daha yüksek Eş zamanlılık ve statik veri birimleri için faydalı olsa da, dinamik kaynak sınıfları daha iyi bir değişken veya büyüyen miktarda veriler için uygundur.  Daha büyük bir hizmet düzeyine ölçeklendirmek, sorgularınız otomatik olarak daha fazla bellek alın.  

Dinamik kaynak sınıfları ile bu önceden tanımlanmış veritabanı rolleri uygulanır:

- smallrc
- mediumrc
- largerc
- xlargerc

### <a name="gen2-dynamic-resource-classes-are-truly-dynamic"></a>Gen2 dinamik kaynak sınıfları tamamen dinamik

Dinamik kaynak sınıflarında Gen1 detayına olarak bakıldığında, davranışlarını anlamak için ek karmaşıklığı artıran birkaç ayrıntıya daha vardır:

**Üzerinde Gen1**
- Smallrc kaynaklar sınıfı statik kaynak sınıfı gibi bir sabit bellek modeli ile çalışır.  Smallrc sorguları dinamik olarak hizmet düzeyi arttıkça daha fazla bellek almıyor. 
- Hizmet düzeyleri değiştikçe, kullanılabilir sorgu eşzamanlılık yukarı veya aşağı gidebilirsiniz.
- Hizmet düzeylerini ölçeklendirme aynı kaynak sınıfları için ayrılan bellek orantılı bir değişiklik sağlamaz.

**Gen2 üzerinde**, yukarıda belirtilen noktaları adresleme tamamen dinamik dinamik kaynak sınıfları.  3-10-22-70 küçük-Orta-büyük-xlarge kaynak sınıfları için bellek yüzdesi ayırma için yeni kuralıdır **hizmet düzeyi ne olursa olsun**.  Bellek ayırma yüzdeleri ve en az sayıda çalışan, hizmet düzeyi bağımsız olarak eş zamanlı sorguları birleştirilmiş ayrıntıları aşağıdaki tabloda sahiptir.

| Kaynak Sınıfı | Bellek yüzdesi | Min eş zamanlı sorguları |
|:--------------:|:-----------------:|:----------------------:|
| smallrc        | 3%                | 32                     |
| mediumrc       | %10               | 10                     |
| largerc        | 22%               | 4                      |
| xlargerc       | 70%               | 1                      |

### <a name="default-resource-class"></a>Varsayılan kaynak sınıfı

Varsayılan olarak, her kullanıcı bir dinamik kaynak sınıfı üyesidir **smallrc**.

Hizmet Yöneticisi kaynak sınıfının smallrc sabittir ve değiştirilemez.  Sağlama işlemi sırasında oluşturulan kullanıcı hizmet yöneticisidir.  Bu bağlamda bir Hizmet Yöneticisi, "Sunucu Yöneticisi oturumu" için belirtilen bir oturum açma bilgileri ise yeni bir SQL Data Warehouse örneğine sahip yeni bir sunucu oluşturma.

> [!NOTE]
> Hizmet yöneticileri Ayrıca, kullanıcılar veya gruplar Active Directory Yöneticisi olarak tanımlı değildir.
>
>

## <a name="resource-class-operations"></a>Kaynak sınıf işlemleri

Kaynak sınıfları, veri yönetimi ve işleme etkinlikleri için performansı artırmak için tasarlanmıştır. Karmaşık sorgular ayrıca büyük kaynak sınıfı altında çalışmasını yararlı olabilir. Örneğin, sorgu performansının büyük birleştirmeler için ve kaynak sınıfı bellekte yürütülecek sorgu etkinleştirmek için yeterince büyük olduğunda sıralar iyileştirebilir.

### <a name="operations-governed-by-resource-classes"></a>Kaynak sınıfları tarafından yönetilen işlemleri

Bu işlemler, kaynak sınıfları tarafından yönetilir:

- INSERT-SELECT, UPDATE, DELETE
- (Kullanıcı tablosu sorgulanırken) seçin
- -ALTER INDEX REORGANIZE ya da yeniden oluşturma
- ALTER TABLO YENİDEN OLUŞTURMA
- CREATE INDEX
- KÜMELENMİŞ COLUMNSTORE DİZİNİ OLUŞTURUN
- TABLO AS SELECT (CTAS) OLUŞTURMA
- Veri yükleme
- Veri Taşıma hizmeti (DMS) tarafından gerçekleştirilen veri taşıma işlemleri

> [!NOTE]  
> Dinamik Yönetim görünümlerini (Dmv'ler) deyimlerine veya diğer sistem görünümleri Eş zamanlılık limitlerine biriyle yönetilmeyen seçin. Sistem üzerinde yürütülen sorgular sayısından bağımsız olarak izleyebilirsiniz.

### <a name="operations-not-governed-by-resource-classes"></a>Kaynak sınıfları tarafından yönetilmeyen işlemleri

Kullanıcı daha büyük bir kaynak sınıfının üyesi olsa bile bazı sorgular smallrc kaynak sınıfı, her zaman çalışır. Bu muafiyet sorguları, Eş zamanlılık limitini sayılmaz. Eş zamanlılık limitini 16 ise, örneğin, çok sayıda kullanıcı sistem görünümlerini kullanılabilir eşzamanlılık yuvaları etkilemeden seçerek.

Aşağıdaki deyimleri, kaynak sınıflardan dışındadır ve her zaman smallrc içinde çalıştırın:

- CREATE veya DROP TABLE
- ALTER TABLE... ANAHTAR, bölme ve birleştirme bölüm
- ALTER INDEX DEVRE DIŞI BIRAK
- DROP INDEX
- OLUŞTURMA, güncelleştirme veya DROP STATISTICS
- TRUNCATE TABLE
- ALTER YETKİLENDİRME
- OTURUM AÇMA OLUŞTURMA
- CREATE, ALTER ve DROP USER
- CREATE, ALTER veya bırakma yordamı
- CREATE veya açılan VIEW
- DEĞER EKLE
- Sistem görünümleri ve Dmv'leri seçin
- AÇIKLAYIN
- DBCC

<!--
Removed as these two are not confirmed / supported under SQL DW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="concurrency-slots"></a>Eşzamanlılık yuvaları

Eşzamanlılık yuvaları, sorgu yürütme için kullanılabilir kaynakları izlemek için kullanışlı bir yoludur. Katılımcı sınırlı olduğundan bir konser, lisans ayrılacak satın biletleri gibi değildirler. Eşzamanlılık yuvaları veri ambarı başına toplam sayısı hizmet düzeyi tarafından belirlenir. Bir sorgu yürütme başlamadan önce yeterli eşzamanlı kullanım hakkı ayırmak mümkün olması gerekir. Sorgu tamamladığında, eşzamanlılık yuvaları serbest bırakır.  

- Bir sorgu ile 10 eşzamanlı yuva çalışıyor 2 eşzamanlılık yuvası ile çalışan bir sorgu daha 5 kat daha fazla bilgi işlem kaynaklarına erişim sağlayabilir.
- Her sorgu 10 eşzamanlılık yuvası gerektirir ve 40 eşzamanlılık yuvaları varsa yalnızca 4 sorguları aynı anda çalıştırabilirsiniz.

Yalnızca yönetilen kaynak sorgularını eşzamanlı yuva kullanır. Sistem sorgularını ve bazı basit sorguların yuva kullanmayan. Eşzamanlılık yuvaları tüketilen gerçek sayısı, sorgunun kaynak sınıfı tarafından belirlenir.

## <a name="view-the-resource-classes"></a>Kaynak sınıfları görüntüleyin

Kaynak sınıfları, önceden tanımlanmış veritabanı rolleri olarak uygulanır. Kaynak sınıfları iki tür vardır: dinamik ve statik. Kaynak sınıfları bir listesini görüntülemek için aşağıdaki sorguyu kullanın:

```sql
SELECT name
FROM   sys.database_principals
WHERE  name LIKE '%rc%' AND type_desc = 'DATABASE_ROLE';
```

## <a name="change-a-users-resource-class"></a>Bir kullanıcının kaynak sınıfını değiştirme

Kaynak sınıfları, veritabanı rollerine kullanıcı atama tarafından uygulanır. Bir kullanıcı bir sorgu çalıştırıldığında, sorgu, kullanıcının kaynak sınıfıyla çalıştırılır. Örneğin, bir kullanıcı staticrc10 veritabanı rolünün bir üyesi ise, sorguları küçük miktarlarda bellek ile çalıştırın. Bir veritabanı kullanıcısı xlargerc veya staticrc80 veritabanı rollerinin bir üyesi ise, büyük miktarlarda bellek sorguları çalıştırın.

Bir kullanıcının kaynak sınıfını artırmak için [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql) büyük kaynak sınıfının bir veritabanı rolüne kullanıcı eklemek için.  Aşağıdaki kod, bir kullanıcı largerc veritabanı rolüne ekler.  Her istek %22 sistem belleği alır.

```sql
EXEC sp_addrolemember 'largerc', 'loaduser';
```

Kaynak sınıfı azaltmak için kullanmak [sp_droprolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-droprolemember-transact-sql).  'Loaduser' üyesi veya başka bir kaynak sınıfları değilse, %3 bellek ataması varsayılan smallrc kaynak sınıfıyla uygulamasına gidin.  

```sql
EXEC sp_droprolemember 'largerc', 'loaduser';
```

## <a name="resource-class-precedence"></a>Kaynak sınıfı önceliği

Kullanıcılar birden çok kaynak sınıflarının üyeleri olabilir. Ne zaman bir kullanıcının birden fazla kaynak sınıfına ait:

- Dinamik kaynak sınıf statik kaynak sınıfları daha önceliklidir. Örneğin, bir kullanıcı mediumrc(dynamic) hem staticrc80 (statik) üyesi ise, mediumrc ile sorgular çalıştırın.
- Daha büyük kaynak sınıfları daha küçük kaynak sınıfları daha önceliklidir. Örneğin, bir kullanıcı mediumrc ve largerc üyesi ise, largerc ile sorgular çalıştırın. Benzer şekilde, bir kullanıcı staticrc20 hem statirc80 üyesi ise, staticrc80 kaynak ayırmaları ile sorgular çalıştırın.

## <a name="recommendations"></a>Öneriler

Biz, belirli türde bir sorgu çalıştırmaya ayrılmış bir kullanıcı oluşturmanızı öneririz veya işlemi yükleyin. Kullanıcının kaynak sınıfı düzenli aralıklarla değiştirilmesi yerine bir kalıcı kaynak sınıfı sağlar. Dinamik kaynak sınıfları olduğunu düşünmeden önce statik kaynak sınıflarını kullanmak önerdiğimiz biçimde statik kaynak sınıfları iş yükünün büyük genel denetimde göze.

### <a name="resource-classes-for-load-users"></a>Yük kullanıcılar için kaynak sınıfları

`CREATE TABLE` Varsayılan olarak kullanan kümelenmiş columnstore dizinleri. Verileri sıkıştırma bir columnstore dizini bellek kullanımı yoğun bir işlemdir ve bellek baskısı dizin kalitesini düşürebilir. Bellek baskısı verileri yüklenirken daha yüksek bir kaynak sınıfı gerek neden olabilir. Yükleri yeterli belleğe sahip olmak için yükleri çalıştırmaya ayrılmış bir kullanıcı oluşturun ve bu kullanıcı daha yüksek bir kaynak sınıfına atayın.

Tablonun yüklendi ve veri boyutu doğasını yüklerini verimli bir şekilde işlemek için gereken bellek bağlıdır. Bellek gereksinimleri hakkında daha fazla bilgi için bkz. [satır grubu kalite en üst düzeye](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

Bellek gereksinimi belirledikten sonra Yük kullanıcı için bir statik veya dinamik kaynak sınıfı atamak isteyip istemediğinizi seçin.

- Tablo bellek gereksinimlerini belirli bir aralıkta bir statik kaynak sınıfı kullanın. Yükleri uygun bellek ile çalıştırın. Veri ambarını ölçeklendirmek, daha fazla bellek yükü gerekmez. Statik kaynak sınıfını kullanarak bellek ayırmaları sabit kalır. Bu tutarlılık, bellek koruyan ve daha fazla sorgu eşzamanlı olarak çalıştırılmasını sağlar. Yeni çözümleri statik kaynak sınıflarını kullanmanızı öneririz ilk olarak, bunlar daha fazla denetim sağlar.
- Tablo bellek gereksinimleri büyük ölçüde farklılık dinamik kaynak sınıfı kullanın. Geçerli DWU daha fazla bellek yükü gerektirebilir veya cDWU düzeyi sağlar. Veri ambarı ölçeklendirme daha fazla bellek yükleme işlemleri için daha hızlı bir şekilde gerçekleştirmek yükü sağlayan ekler.

### <a name="resource-classes-for-queries"></a>Sorgular için kaynak sınıfları

Yoğun işlem gücü kullanımlı bazı sorgular ve bazı değil.  

- Sorguları karmaşıktır, ancak yüksek eşzamanlılık gerektirmeyen bir dinamik kaynak sınıfı seçin.  Örneğin, günlük veya haftalık raporlar oluşturarak kaynaklar için bazen bir gerekli değildir. Veri ambarı ölçeklendirme, büyük miktarlarda verinin raporları işliyorsa, kullanıcının mevcut kaynak sınıfı için daha fazla bellek sağlar.
- Gün boyunca kaynak beklentileri farklı bir statik kaynak sınıfı seçin. Örneğin, iyi veri ambarı birçok kişi tarafından sorgulandığında statik kaynak sınıfı çalışır. Kullanıcı için ayrılan bellek miktarını, veri ambarı ölçeklerken değiştirmez. Sonuç olarak, daha fazla sorgu, sistem üzerinde paralel olarak gerçekleştirilebilir.

Uygun bellek verir, sorgulanan, veriler tablo şemalarını yapısını miktarı gibi birçok faktöre bağlıdır ve çeşitli birleşimler seçin ve Grup koşulları. Genel olarak, daha fazla bellek sorguları daha hızlı tamamlamanıza izin verir, ancak Genel eşzamanlılık azaltır. Eşzamanlılık, bir sorun değil, aşırı bellek ayırma aktarım hızı zarar değil.

Performansı ayarlamak için farklı kaynak sınıfları kullanın. Sonraki bölümde yardımcı olan bir saklı yordam şekil en iyi kaynak sınıfını sağlar.

## <a name="example-code-for-finding-the-best-resource-class"></a>En iyi kaynak sınıfı bulmak için örnek kod

Aşağıdaki belirtilen saklı yordam için kullanabileceğiniz [Gen1](#stored-procedure-definition-for-gen1) veya [Gen2](#stored-procedure-definition-for-gen2)-çözmesini eşzamanlılık ve bellek belirtilen bir SLO kaynak sınıfı ve bellek için en iyi kaynak sınıfı başına vermek yoğun CCI Belirtilen kaynak sınıfı bölümlenmemiş CCI tablosu üzerinde işlemler:

Bu saklı yordamı amacı şu şekildedir:

1. Kaynak sınıfı belirli bir SLO başına vermek bellek ve eşzamanlılık görmek için. Kullanıcı Bu örnekte gösterildiği gibi boş hem şema hem de tablename sağlaması gerekir.  
2. Bölümlenmemiş CCI tabloda belirtilen kaynak sınıfı, bellek kullanımı yoğun CCI işlemleri (yükleme, kopyalama tablo, yeniden derleme dizini, vb.) için en iyi kaynak sınıfı görmek için. Saklı yordam tablo şemasını gerekli bellek ataması bulmak için kullanır.

### <a name="dependencies--restrictions"></a>Bağımlılıklar ve sınırlamalar

- Bu saklı yordamı bölümlenmiş ccı tablosu için bellek gereksinimi hesaplamak için tasarlanmamıştır.
- Bu saklı yordam Ekle/CTAS-SELECT seçme bölümü için bellek gereksinimleri dikkate almaz ve SELECT olduğunu varsayar.
- Bu saklı yordamı Bu saklı yordamı oluşturulduğu oturumunda kullanılabilir bir geçici tablo kullanır.
- Bu değişiklikler, daha sonra bu saklı yordam doğru şekilde çalışmaz ve bu saklı yordam geçerli teklifler (örneğin, donanım yapılandırması, DMS config) bağlıdır.  
- Bu saklı yordamı mevcut eşzamanlılık sınırı tekliflerini bağlıdır ve bunlar değiştirirseniz, ardından bu saklı yordamı doğru şekilde çalışmaz.  
- Bu saklı yordamı mevcut kaynak sınıfı tekliflerini bağlıdır ve bunlar değiştirirseniz, ardından bu saklı yordamı doğru şekilde çalışmaz.  

>[!NOTE]  
>Saklı yordam sağlanan parametrelerle yürütüldükten sonra çıktı gitmiyor, ardından olabilir iki durum.
>
>1. SLO değeri geçersiz ya da DW parametresi içerir
>2. Ya da tablo CCI işlemi için eşleşen bir kaynak sınıf yok.
>
>Örneğin, yüksek bellek ataması DW100 400 MB olan ve tablo şemasını 400 MB gereksinim çapraz için yeterince geniş ise.

### <a name="usage-example"></a>Kullanım örneği

Sözdizimi:  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`
  
1. @DWU: Ya da geçerli DWU DW DB'den ayıklamak ya da herhangi bir desteklenen DWU 'DW100' biçiminde sağlamak için bir NULL parametresi sağlayın
2. @SCHEMA_NAME: Tablo şema adını sağlayın
3. @TABLE_NAME: İlgilendiğiniz bir tablo adı sağlayın

Örnekler: Bu saklı yordam yürütme

```sql
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

Aşağıdaki deyim, Yukarıdaki örneklerde kullanılan Table1 oluşturur.
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

### <a name="stored-procedure-definition-for-gen1"></a>Saklı yordam tanımında Gen1 için

```sql  
-------------------------------------------------------------------------------
-- Dropping prc_workload_management_by_DWU procedure if it exists.
-------------------------------------------------------------------------------
IF EXISTS (SELECT -FROM sys.objects WHERE type = 'P' AND name = 'prc_workload_management_by_DWU')
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
                     WHEN FLOOR(8 -(CAST (@DWU_NUM AS FLOAT)/6000)) > 0 THEN FLOOR(8 -(CAST (@DWU_NUM AS FLOAT)/6000))
                     ELSE 1
              END AS multiplication_factor
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
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multiplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB
       FROM    size, load_multiplier, #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multiplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multiplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```

### <a name="stored-procedure-definition-for-gen2"></a>2. nesil için saklı yordam tanımı

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
  SELECT @DWU = 'DW'+CAST(Nodes*CASE WHEN CPUVer>6 THEN 500 ELSE 100 END AS VARCHAR(10))+CASE WHEN CPUVer>6 THEN 'c' ELSE '' END
    FROM (
      SELECT Nodes=count(distinct n.pdw_node_id), CPUVer=max(i.cpu_count)
        FROM sys.dm_pdw_nodes n
        CROSS APPLY sys.dm_pdw_nodes_os_sys_info i
        WHERE type = 'COMPUTE'
         )A
END

-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END;

-- Creating ref. temp table (CTAS) to hold mapping info.
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
  UNION ALL
    SELECT 'DW1000c', 32, 40, 1, 4, 8, 28, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1500c', 32, 60, 1, 6, 13, 42, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW2000c', 48, 80, 2, 8, 17, 56, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW2500c', 48, 100, 3, 10, 22, 70, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW3000c', 64, 120, 3, 12, 26, 84, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW5000c', 64, 200, 6, 20, 44, 140, 1, 2, 4, 8, 16, 32, 64, 128
  UNION ALL
    SELECT 'DW6000c', 128, 240, 7, 24, 52, 168, 1, 2, 4, 8, 16, 32, 64, 128
  UNION ALL
    SELECT 'DW7500c', 128, 300, 9, 30, 66, 210, 1, 2, 4, 8, 16, 32, 64, 128
  UNION ALL
    SELECT 'DW10000c', 128, 400, 12, 40, 88, 280, 1, 2, 4, 8, 16, 32, 64, 128
  UNION ALL
    SELECT 'DW15000c', 128, 600, 18, 60, 132, 420, 1, 2, 4, 8, 16, 32, 64, 128
  UNION ALL
    SELECT 'DW30000c', 128, 1200, 36, 120, 264, 840, 1, 2, 4, 8, 16, 32, 64, 128
)
-- Creating workload mapping to their corresponding slot consumption and default memory grant.
,map
AS
(
  SELECT CONVERT(varchar(20), 'SloDWGroupSmall') AS wg_name, slots_used_smallrc AS slots_used FROM alloc WHERE DWU = @DWU
UNION ALL
  SELECT CONVERT(varchar(20), 'SloDWGroupMedium') AS wg_name, slots_used_mediumrc AS slots_used FROM alloc WHERE DWU = @DWU
UNION ALL
  SELECT CONVERT(varchar(20), 'SloDWGroupLarge') AS wg_name, slots_used_largerc AS slots_used FROM alloc WHERE DWU = @DWU
UNION ALL
  SELECT CONVERT(varchar(20), 'SloDWGroupXLarge') AS wg_name, slots_used_xlargerc AS slots_used FROM alloc WHERE DWU = @DWU
  UNION ALL
  SELECT 'SloDWGroupC00',1
  UNION ALL
    SELECT 'SloDWGroupC01',2
  UNION ALL
    SELECT 'SloDWGroupC02',4
  UNION ALL
    SELECT 'SloDWGroupC03',8
  UNION ALL
    SELECT 'SloDWGroupC04',16
  UNION ALL
    SELECT 'SloDWGroupC05',32
  UNION ALL
    SELECT 'SloDWGroupC06',64
  UNION ALL
    SELECT 'SloDWGroupC07',128
)

-- Creating ref based on current / asked DWU.
, ref
AS
(
  SELECT  a1.*
  ,       m1.wg_name          AS wg_name_smallrc
  ,       m1.slots_used * CASE WHEN RIGHT(@DWU,1)='c' THEN 250 ELSE 200 END AS tgt_mem_grant_MB_smallrc
  ,       m2.wg_name          AS wg_name_mediumrc
  ,       m2.slots_used * CASE WHEN RIGHT(@DWU,1)='c' THEN 250 ELSE 200 END AS tgt_mem_grant_MB_mediumrc
  ,       m3.wg_name          AS wg_name_largerc
  ,       m3.slots_used * CASE WHEN RIGHT(@DWU,1)='c' THEN 250 ELSE 200 END AS tgt_mem_grant_MB_largerc
  ,       m4.wg_name          AS wg_name_xlargerc
  ,       m4.slots_used * CASE WHEN RIGHT(@DWU,1)='c' THEN 250 ELSE 200 END AS tgt_mem_grant_MB_xlargerc
  ,       m5.wg_name          AS wg_name_staticrc10
  ,       m5.slots_used * CASE WHEN RIGHT(@DWU,1)='c' THEN 250 ELSE 200 END AS tgt_mem_grant_MB_staticrc10
  ,       m6.wg_name          AS wg_name_staticrc20
  ,       m6.slots_used * CASE WHEN RIGHT(@DWU,1)='c' THEN 250 ELSE 200 END AS tgt_mem_grant_MB_staticrc20
  ,       m7.wg_name          AS wg_name_staticrc30
  ,       m7.slots_used * CASE WHEN RIGHT(@DWU,1)='c' THEN 250 ELSE 200 END AS tgt_mem_grant_MB_staticrc30
  ,       m8.wg_name          AS wg_name_staticrc40
  ,       m8.slots_used * CASE WHEN RIGHT(@DWU,1)='c' THEN 250 ELSE 200 END AS tgt_mem_grant_MB_staticrc40
  ,       m9.wg_name          AS wg_name_staticrc50
  ,       m9.slots_used * CASE WHEN RIGHT(@DWU,1)='c' THEN 250 ELSE 200 END AS tgt_mem_grant_MB_staticrc50
  ,       m10.wg_name          AS wg_name_staticrc60
  ,       m10.slots_used * CASE WHEN RIGHT(@DWU,1)='c' THEN 250 ELSE 200 END AS tgt_mem_grant_MB_staticrc60
  ,       m11.wg_name          AS wg_name_staticrc70
  ,       m11.slots_used * CASE WHEN RIGHT(@DWU,1)='c' THEN 250 ELSE 200 END AS tgt_mem_grant_MB_staticrc70
  ,       m12.wg_name          AS wg_name_staticrc80
  ,       m12.slots_used * CASE WHEN RIGHT(@DWU,1)='c' THEN 250 ELSE 200 END AS tgt_mem_grant_MB_staticrc80
  FROM alloc a1
  JOIN map   m1  ON a1.slots_used_smallrc     = m1.slots_used and m1.wg_name = 'SloDWGroupSmall'
  JOIN map   m2  ON a1.slots_used_mediumrc    = m2.slots_used and m2.wg_name = 'SloDWGroupMedium'
  JOIN map   m3  ON a1.slots_used_largerc     = m3.slots_used and m3.wg_name = 'SloDWGroupLarge'
  JOIN map   m4  ON a1.slots_used_xlargerc    = m4.slots_used and m4.wg_name = 'SloDWGroupXLarge'
  JOIN map   m5  ON a1.slots_used_staticrc10    = m5.slots_used and m5.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m6  ON a1.slots_used_staticrc20    = m6.slots_used and m6.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m7  ON a1.slots_used_staticrc30    = m7.slots_used and m7.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m8  ON a1.slots_used_staticrc40    = m8.slots_used and m8.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m9  ON a1.slots_used_staticrc50    = m9.slots_used and m9.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m10  ON a1.slots_used_staticrc60    = m10.slots_used and m10.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m11  ON a1.slots_used_staticrc70    = m11.slots_used and m11.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m12  ON a1.slots_used_staticrc80    = m12.slots_used and m12.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  WHERE   a1.DWU = @DWU
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
          WHEN FLOOR(8 * (CAST (CAST(REPLACE(REPLACE(@DWU,'DW',''),'c','') AS INT) AS FLOAT)/6000)) > 0
            AND CHARINDEX(@DWU,'c')=0
          THEN FLOOR(8 * (CAST (CAST(REPLACE(REPLACE(@DWU,'DW',''),'c','') AS INT) AS FLOAT)/6000))
          ELSE 1
        END AS multiplication_factor
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
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multiplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB
       FROM    size
       , load_multiplier
       , #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multiplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multiplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```

## <a name="next-step"></a>Sonraki adım

Veritabanı kullanıcıları yönetme ve güvenlik hakkında daha fazla bilgi için bkz. [güvenli bir veritabanında SQL veri ambarı][Secure a database in SQL Data Warehouse]. Ne kadar büyük kaynak sınıfları hakkında daha fazla bilgi kümelenmiş columnstore dizini kalitesini artırmak, bkz [sıkıştırma columnstore bellek iyileştirmeleri](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Rebuilding indexes to improve segment quality]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
