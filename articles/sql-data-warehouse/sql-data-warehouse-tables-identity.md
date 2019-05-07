---
title: Vekil anahtarlar - Azure SQL veri ambarı oluşturmak için kimlik BİLGİLERİNİZ kullanılarak | Microsoft Docs
description: Öneriler ve tablolarda Azure SQL veri ambarı'nda vekil anahtarlar oluşturmak için kimlik özelliği kullanımına ilişkin örnekler.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/30/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: be7d1f7d04b759a5d60ba26d391d19d1bc57f325
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65154307"
---
# <a name="using-identity-to-create-surrogate-keys-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'nda vekil anahtarlar oluşturmak için kimlik BİLGİLERİNİZ kullanılarak

Öneriler ve tablolarda Azure SQL veri ambarı'nda vekil anahtarlar oluşturmak için kimlik özelliği kullanımına ilişkin örnekler.

## <a name="what-is-a-surrogate-key"></a>Vekil anahtar nedir

Bir yedek anahtarı bir tablodaki her satır için benzersiz bir tanımlayıcı bir sütundur. Anahtarı, tablo verilerinden oluşturulmaz. Bunlar veri ambarı modelleri tasarlarken, tablolarda vekil anahtarlar oluşturmak veri modelleyen gibi. KİMLİK özelliği, yük performansını etkilemeden basit ve etkili bir şekilde bu hedefe ulaşmak için kullanabilirsiniz.  

## <a name="creating-a-table-with-an-identity-column"></a>Bir kimlik sütunu ile tablo oluşturma

KİMLİK özelliği, veri ambarındaki tüm dağıtımları arasında yük performansını etkilemeden ölçeğini genişletmek için tasarlanmıştır. Bu nedenle, kimlik bu hedeflere ulaşmak için doğru yönlendirilmiş uygulamasıdır.

Aşağıdaki deyime benzer bir sözdizimi kullanarak ilk tablo oluşturduğunuzda kimlik özelliğine sahip olarak bir tablo tanımlayabilirsiniz:

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

Ardından `INSERT..SELECT` tabloyu doldurmak için.

Bu bölümde bu geri kalanında daha eksiksiz anlamanıza yardımcı olması için uygulama farklılıklarına vurgular.  

### <a name="allocation-of-values"></a>Değerlerin ayırma

KİMLİK özelliği, SQL Server ve Azure SQL veritabanı davranışını yansıtan vekil değerler, ayrılmış, sipariş garanti etmez. Ancak, Azure SQL veri ambarı'nda bir garanti olmaması daha belirgin olur.

Aşağıdaki örnek bir örnektir:

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

Önceki örnekte, iki satır 1 dağıtımlarında yerleştirildi. İlk satır, sütun vekil değeri 1 olan `C1`, ve ikinci satırın 61 vekil değerine sahiptir. Bu değerleri her ikisi kimlik özelliği tarafından üretildi. Ancak, değerler ayırma bitişik değil. Bu davranış tasarım gereğidir.

### <a name="skewed-data"></a>Dengesiz veri

Veri türü için değer aralığını dağıtımlar arasında eşit olarak yayılır. Dağıtılmış bir tablo dengesiz verilerinden alternatife, ardından veri türü için kullanılabilir değerleri aralığı erken tükenmiş. Tüm verileri sona eriyor tek bir dağıtım, örneğin, etkili bir şekilde tabloda yalnızca bir-sixtieth veri türü değerlerinin erişimi vardır. Bu nedenle, kimlik özelliği sınırlı olan `INT` ve `BIGINT` veri türleri yalnızca.

### <a name="selectinto"></a>SEÇİN... İÇİNE

Mevcut bir kimlik sütunu yeni bir tabloya seçildiğinde, aşağıdaki koşullardan biri doğru olmadığı sürece yeni bir sütun kimlik özelliği alır:

- SELECT deyimi bir JOIN içeriyor.
- Birden çok SELECT deyimine birleşim ile birleştirilir.
- KİMLİK sütunu SELECT listesinde bulunan birden fazla kez listeleniyor.
- KİMLİK sütunu bir ifade bir parçasıdır.

Bu koşullardan herhangi biri true ise, kimlik özelliğini devralan yerine NULL olmayan sütun oluşturulur.

### <a name="create-table-as-select"></a>TABLO AS SELECT OLUŞTURMA

CREATE TABLE AS SELECT (CTAS) için seçin'de belgelenen aynı SQL Server davranışı aşağıdaki... . Ancak, sütun tanımında bir kimlik özelliği belirtilemez. `CREATE TABLE` ifadesinin parçası. Ayrıca kimlik işlevinde kullanamazsınız `SELECT` CTAS bir parçası. Bir tabloyu doldurmak için kullanmanız gerekir `CREATE TABLE` ardından tablo tanımlamak için `INSERT..SELECT` bunu doldurmak üzere.

## <a name="explicitly-inserting-values-into-an-identity-column"></a>Açıkça değerleri bir kimlik sütununa ekleme

SQL veri ambarı destekler `SET IDENTITY_INSERT <your table> ON|OFF` söz dizimi. Açıkça kimlik sütununa değerleri eklemek için bu sözdizimini kullanabilirsiniz.

Birçok veri modelleyen kendi boyutlarını belirli satırlar için önceden tanımlanmış negatif değerleri kullanmak ister. Bir örnek, "Bilinmeyen üye" satır veya -1 ' dir.

Sonraki betik, açıkça AYARLAMAK IDENTITY_INSERT kullanarak bu satır ekleme işlemi gösterilmektedir:

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

## <a name="loading-data"></a>Veri yükleme

IDENTİTY özelliği bulunması, veri yükleme kodunuzda bazı etkilere sahiptir. Bu bölüm kimliği'ni kullanarak verileri tablolara yüklemek için bazı temel düzenlerden vurgular.

Verileri bir tabloya yüklemek ve kimlik kullanarak bir yedek anahtar oluşturmak için tablo oluşturun ve INSERT kullanın... Seç veya Ekle... Yükleme gerçekleştirmek için değerler.

Aşağıdaki örnek, temel düzeni vurgular:

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

SELECT *
FROM   dbo.T1
;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

> [!NOTE]
> Kullanmak mümkün değil `CREATE TABLE AS SELECT` şu anda bir kimlik sütunu içeren bir tabloya veri yükleme zaman.
>

Veri yükleme ile ilgili daha fazla bilgi için bkz: [tasarlama ayıklama, yükleme ve dönüştürme (ELT) için Azure SQL veri ambarı](design-elt-data-loading.md) ve [en iyi uygulamalar yüklenirken](guidance-for-loading-data.md).

## <a name="system-views"></a>Sistem görünümleri

Kullanabileceğiniz [sys.identity_columns](/sql/relational-databases/system-catalog-views/sys-identity-columns-transact-sql) katalog görünümünü kimlik özelliğine sahip bir sütun tanımlayın.

Veritabanı şeması daha iyi anlamanıza yardımcı olmak için bu örnek nasıl tümleştireceğinizi sys.identity_column gösterir ' diğer sistem Kataloğu görünümleri ile:

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

IDENTİTY özelliği kullanılamaz:

- Ne zaman sütununun veri türü tamsayı veya büyük tamsayı değil
- Sütun ayrıca dağıtım anahtarının olduğunda
- Tablo, bir dış tablo olduğunda

SQL veri ambarı'nda aşağıdaki ilgili işlevleri desteklenmez:

- [IDENTITY()](/sql/t-sql/functions/identity-function-transact-sql)
- [@@IDENTITY](/sql/t-sql/functions/identity-transact-sql)
- [SCOPE_IDENTITY](/sql/t-sql/functions/scope-identity-transact-sql)
- [IDENT_CURRENT](/sql/t-sql/functions/ident-current-transact-sql)
- [IDENT_INCR](/sql/t-sql/functions/ident-incr-transact-sql)
- [IDENT_SEED](/sql/t-sql/functions/ident-seed-transact-sql)

## <a name="common-tasks"></a>Genel görevler

Bu bölümde Kimlik sütunları ile çalışırken yaygın görevleri gerçekleştirmek için kullanabileceğiniz bazı örnek kodlar sağlanmaktadır.

Sütun C1 aşağıdaki görevlerin tümü kimliktir.

### <a name="find-the-highest-allocated-value-for-a-table"></a>Bir tablo için ayrılan en yüksek değeri bulma

Kullanım `MAX()` işlevi dağıtılan bir tablo için ayrılan en yüksek değeri belirlemek için:

```sql
SELECT MAX(C1)
FROM dbo.T1
```

### <a name="find-the-seed-and-increment-for-the-identity-property"></a>Çekirdek ve Artım için kimlik özelliği bulunamadı

Katalog görünümleri aşağıdaki sorguyu kullanarak kimlik artırma ve çekirdek yapılandırma değerlerini bir tablo için keşfetmek için kullanabilirsiniz:

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

- [Tabloya genel bakış](/azure/sql-data-warehouse/sql-data-warehouse-tables-overview)
- [Tablo (Transact-SQL) kimliği (özellik) oluşturma](/sql/t-sql/statements/create-table-transact-sql-identity-property?view=azure-sqldw-latest)
- [DBCC CHECKINDENT](/sql/t-sql/database-console-commands/dbcc-checkident-transact-sql)
