---
title: "KİMLİĞİ kullanarak yedek anahtarları oluşturma | Microsoft Docs"
description: "KİMLİK tablolarınızı yedek anahtarlar oluşturmak için nasıl kullanılacağını öğrenin."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/13/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 3ab5d159e6eaeb830135fe134e108b0e4de4b7d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-surrogate-keys-by-using-identity"></a>Yedek anahtarları kimliği kullanarak oluşturma
> [!div class="op_single_selector"]
> * [Genel bakış][Overview]
> * [Veri türleri][Data Types]
> * [Dağıt][Distribute]
> * [Dizin][Index]
> * [Bölüm][Partition]
> * [İstatistikleri][Statistics]
> * [Geçici][Temporary]
> * [Kimlik][Identity]
> 
> 

Veri ambarı modelleri tasarlarken kendi tablolarda yedek anahtarlar oluşturmak çok sayıda veri modelers gibi. IDENTİTY özelliği yükü performansınızı etkilemeden sadece ve etkili bir şekilde bu hedefe ulaşmak için kullanabilirsiniz. 

## <a name="get-started-with-identity"></a>KİMLİĞİ ile çalışmaya başlama
Aşağıdaki deyime benzer sözdizimini kullanarak ilk tablonun oluşturduğunuzda kimlik özelliğine sahip olarak bir tablo tanımlayabilirsiniz:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1) NOT NULL
,   C2 INT NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;
```

Daha sonra kullanabilirsiniz `INSERT..SELECT` tabloyu doldurmak için.

## <a name="behavior"></a>Davranışı
KİMLİK özelliği, veri ambarındaki tüm dağıtımlar arasında yük performanslarını etkilemeden genişletmek için tasarlanmıştır. Bu nedenle, kimlik bu hedeflere ulaşmak doğru yönlendirilmiş uygulamasıdır. Bu bölümde daha eksiksiz anlamanıza yardımcı olması için uygulama nüanslarını vurgular.  

### <a name="allocation-of-values"></a>Değerlerin ayırma
KİMLİK özelliği, SQL Server ve Azure SQL veritabanı davranışını yansıtan, yedek değerleri ayrılmış, sipariş garanti etmez. Ancak, Azure SQL veri ambarı'nda bir garanti yokluğu daha belirgin olur. 

Aşağıdaki çizim bir örnektir:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)    NOT NULL
,   C2 VARCHAR(30)              NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

INSERT INTO dbo.T1
VALUES (NULL);

INSERT INTO dbo.T1
VALUES (NULL);

SELECT *
FROM dbo.T1;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

Önceki örnekte, iki satır dağıtım 1 landed. İlk satırı sütun yedek değeri 1 olan `C1`, ve ikinci satırında 61 yedek değerine sahip. Bu değerlerin her ikisi de kimlik özelliği tarafından üretildi. Ancak, değerleri ayırma bitişik değil. Bu davranış tasarım gereğidir.

### <a name="skewed-data"></a>Çarpık veri 
Veri türü için değer aralığını dağıtımlar arasında eşit olarak yayılır. Dağıtılmış bir tablo çarpık verileri varsa, ardından veri türü için kullanılabilir değerleri aralığı erken tükendi. Tüm verileri sona eriyor, tek bir dağıtım Örneğin, ardından etkili bir şekilde tablo yalnızca bir-sixtieth veri türü değerlerinin erişebilir. Bu nedenle, kimlik özelliği sınırlıdır `INT` ve `BIGINT` veri türleri yalnızca.

### <a name="selectinto"></a>SEÇİN... İÇİNE
Varolan bir kimlik sütunu yeni bir tabloya seçildiğinde, yeni bir sütun aşağıdaki koşullardan biri doğru olmadıkça kimlik özelliği alır:
- SELECT deyimi bir birleştirme içerir.
- Birden çok SELECT deyimine birleşim kullanılarak birleştirilmiştir.
- KİMLİK sütunu SELECT listesinde birden fazla kez listelenir.
- KİMLİK sütunu bir ifadenin bir parçasıdır.
    
Bu koşulların herhangi biri doğruysa, sütun kimlik özelliğini devralan yerine NOT NULL oluşturulur.

### <a name="create-table-as-select"></a>TABLE AS SELECT OLUŞTURMA
OLUŞTURMA tablo AS seçin (CTAS) için SELECT belgelenen aynı SQL Server davranışı izleyen... . Ancak, bir kimlik özelliği sütun tanımında belirtemezsiniz `CREATE TABLE` deyim parçası. Ayrıca kimlik işlevinde kullanamazsınız `SELECT` CTAS bir parçası. Bir tabloyu doldurmak için kullanmanız gerekir `CREATE TABLE` arkasından tablosu tanımlamak için `INSERT..SELECT` onu doldurmak için.

## <a name="explicitly-insert-values-into-an-identity-column"></a>Açıkça değerleri bir kimlik sütununa ekleme 
SQL veri ambarı destekleyen `SET IDENTITY_INSERT <your table> ON|OFF` sözdizimi. Açıkça kimlik sütununa değerleri eklemek için şu sözdizimini kullanın.

Birçok veri modelers kendi boyutlarını belirli satırlar için önceden tanımlanmış negatif değerler kullanmak ister. -1 veya "Bilinmeyen bir üyeye" satır örneğidir. 

Sonraki komut dosyası açıkça AYARLAMAK IDENTITY_INSERT kullanarak bu satır eklemek nasıl gösterilmektedir:

```sql
SET IDENTITY_INSERT dbo.T1 ON;

INSERT INTO dbo.T1
(   C1
,   C2
)
VALUES (-1,'UNKNOWN')
;

SET IDENTITY_INSERT dbo.T1 OFF;

SELECT  *
FROM    dbo.T1
;
```    

## <a name="load-data-into-a-table-with-identity"></a>KİMLİĞİNE sahip bir tabloya veri yükleme

IDENTİTY özelliği varlığını veri yükleme kodunuza bazı etkilere sahiptir. Bu bölüm kimliği kullanarak verileri tablolara yüklemek için bazı temel düzenlerden vurgular. 

### <a name="load-data-with-polybase"></a>PolyBase ile veri yükleyin
Verileri bir tabloya yüklemek ve bir yedek anahtar kimliği kullanarak oluşturmak, tablo oluştur ve Ekle kullanmak için... Seç veya Ekle... Yükleme gerçekleştirmek için değerler.

Aşağıdaki örnek temel düzeni vurgular:
 
```sql
--CREATE TABLE with IDENTITY
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)
,   C2 VARCHAR(30)
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

--Use INSERT..SELECT to populate the table from an external table
INSERT INTO dbo.T1
(C2)
SELECT  C2
FROM    ext.T1
;

SELECT  *
FROM    dbo.T1
;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

> [!NOTE] 
> Kullanmak mümkün değil `CREATE TABLE AS SELECT` şu anda verileri bir kimlik sütunu olan bir tabloya yüklenirken.
> 

Toplu kopyalama programı (BCP) aracını kullanarak veri yükleme ile ilgili daha fazla bilgi için aşağıdaki makalelere bakın:

- [PolyBase ile yükleme][]
- [PolyBase en iyi uygulamalar][]

### <a name="load-data-with-bcp"></a>BCP ile veri yükleme
BCP SQL Data Warehouse'a veri yüklemek için kullanabileceğiniz bir komut satırı aracıdır. Parametrelerinden biri (-E) verileri bir kimlik sütunu olan bir tabloya yüklenirken BCP davranışını denetler. 

-E belirtildiğinde, sütun için giriş dosyasındaki KİMLİKLE tutulan değerleri korunur. -E ise *değil* belirtilen sonra da bu sütundaki değerleri yoksayılır. Kimlik sütununun dahil edilmezse, verileri normal olarak yüklenir. Değerleri özelliği artırma ve çekirdek ilkesine göre oluşturulur.

BCP kullanarak veri yükleme ile ilgili daha fazla bilgi için aşağıdaki makalelere bakın:

- [BCP ile yükleme][]
- [BCP MSDN'de][]

## <a name="catalog-views"></a>Katalog görünümleri
SQL veri ambarı destekleyen `sys.identity_columns` Katalog görünümü. Bu görünüm, kimlik özelliğine sahip bir sütunu tanımlamak için kullanılabilir.

Veritabanı şeması daha iyi anlamanıza yardımcı olması için bu örnek nasıl tümleştirileceği gösterir `sys.identity_columns` diğer sistem Katalog görünümleri ile:

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       CASE WHEN ic.column_id IS NOT NULL
             THEN 1
        ELSE 0
        END AS is_identity 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
LEFT JOIN   sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="limitations"></a>Sınırlamalar
KİMLİK özelliği aşağıdaki senaryolarda kullanılamaz:
- Burada sütun veri türü tamsayı veya büyük tamsayı değil
- Sütun dağıtım anahtarı da olduğu
- Tablo bir dış tablo olduğu 

SQL veri ambarı'nda aşağıdaki ilgili işlevleri desteklenmez:

- [IDENTITY()][]
- [@@IDENTITY][]
- [SCOPE_IDENTITY][]
- [IDENT_CURRENT][]
- [IDENT_INCR][]
- [IDENT_SEED][]
- [DBCC CHECK_IDENT()][]

## <a name="tasks"></a>Görevler

Bu bölümde kimlik sütunu ile çalışırken ortak görevleri gerçekleştirmek için kullanabileceğiniz bazı örnek kodu sağlıyor.

> [!NOTE] 
> Sütun C1 tüm aşağıdaki görevler kimliğidir.
> 
 
### <a name="find-the-highest-allocated-value-for-a-table"></a>Bir tablo için en yüksek ayrılmış değeri Bul
Kullanım `MAX()` işlevi dağıtılmış bir tablo için ayrılan en yüksek değeri belirlemek için:

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### <a name="find-the-seed-and-increment-for-the-identity-property"></a>Çekirdek ve Artım kimlik özelliği için Bul
Katalog görünümleri aşağıdaki sorguyu kullanarak bir tablo için kimlik artırma ve çekirdek yapılandırma değerlerini bulmak için kullanabilirsiniz: 

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       ic.seed_value
,       ic.increment_value 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
JOIN        sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="next-steps"></a>Sonraki adımlar

* Tabloları geliştirme hakkında daha fazla bilgi için bkz: [tablo genel bakış][Overview], [tablo veri türleri][Data Types], [bir tabloDağıt] [ Distribute], [Tablo dizin][Index], [tablo bölüm][Partition], ve [ Geçici tablolara][Temporary]. 
* En iyi uygulamalar hakkında daha fazla bilgi için bkz: [SQL veri ambarı en iyi yöntemler][SQL Data Warehouse Best Practices].  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Identity]: ./sql-data-warehouse-tables-identity.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

[BCP ile yükleme]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp/
[PolyBase ile yükleme]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase/
[PolyBase en iyi uygulamalar]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide/


<!--MSDN references-->
[Identity property]: https://msdn.microsoft.com/library/ms186775.aspx
[sys.identity_columns]: https://msdn.microsoft.com/library/ms187334.aspx
[IDENTITY()]: https://msdn.microsoft.com/library/ms189838.aspx
[@@IDENTITY]: https://msdn.microsoft.com/library/ms187342.aspx
[SCOPE_IDENTITY]: https://msdn.microsoft.com/library/ms190315.aspx
[IDENT_CURRENT]: https://msdn.microsoft.com/library/ms175098.aspx
[IDENT_INCR]: https://msdn.microsoft.com/library/ms189795.aspx
[IDENT_SEED]: https://msdn.microsoft.com/library/ms189834.aspx
[DBCC CHECK_IDENT()]: https://msdn.microsoft.com/library/ms176057.aspx

[BCP MSDN'de]: https://msdn.microsoft.com/library/ms162802.aspx
  

<!--Other Web references-->  
