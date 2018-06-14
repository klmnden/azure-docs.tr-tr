---
title: Yedek anahtarlar - Azure SQL Data Warehouse oluşturmak için kimlik BİLGİLERİNİZ kullanılarak | Microsoft Docs
description: Öneriler ve Azure SQL Data Warehouse tablolarda yedek anahtarlar oluşturmak için kimlik özelliği kullanımına ilişkin örnekler.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: ab028705f5af7c37017d2e697240b7d3436f5f71
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31526993"
---
# <a name="using-identity-to-create-surrogate-keys-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse'da yedek anahtarlar oluşturmak için kimlik BİLGİLERİNİZ kullanılarak
Öneriler ve Azure SQL Data Warehouse tablolarda yedek anahtarlar oluşturmak için kimlik özelliği kullanımına ilişkin örnekler.

## <a name="what-is-a-surrogate-key"></a>Bir yedek anahtar nedir?
Bir yedek anahtarı bir tablodaki her satır için benzersiz bir tanımlayıcı bir sütundur. Anahtar tablo verileri oluşturulmaz. Veri ambarı modelleri tasarlarken kendi tablolarda yedek anahtarlar oluşturmak veri modelers gibi. IDENTİTY özelliği yükü performansınızı etkilemeden sadece ve etkili bir şekilde bu hedefe ulaşmak için kullanabilirsiniz.  

## <a name="creating-a-table-with-an-identity-column"></a>Tablo bir kimlik sütunu ile oluşturma
KİMLİK özelliği, veri ambarındaki tüm dağıtımlar arasında yük performanslarını etkilemeden genişletmek için tasarlanmıştır. Bu nedenle, kimlik bu hedeflere ulaşmak doğru yönlendirilmiş uygulamasıdır. 

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

Bu bölümde bu geri kalanı daha eksiksiz anlamanıza yardımcı olması için uygulama nüanslarını vurgular.  

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

## <a name="explicitly-inserting-values-into-an-identity-column"></a>Açıkça değerleri bir kimlik sütununa ekleme 
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

## <a name="loading-data"></a>Veri yükleme

IDENTİTY özelliği varlığını veri yükleme kodunuza bazı etkilere sahiptir. Bu bölüm kimliği kullanarak verileri tablolara yüklemek için bazı temel düzenlerden vurgular. 

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
> Tablo AS Seç oluşturmak kullanmak mümkün değil ' şu anda verileri bir kimlik sütunu olan bir tabloya yüklenirken.
> 

Veri yükleme ile ilgili daha fazla bilgi için bkz: [tasarlama ayıklayın, yükleme ve dönüştürme (ELT) Azure SQL Data Warehouse için](design-elt-data-loading.md) ve [en iyi uygulamalar yüklenirken](guidance-for-loading-data.md).


## <a name="system-views"></a>Sistem görünümleri
Kullanabileceğiniz [sys.identity_columns](/sql/relational-databases/system-catalog-views/sys-identity-columns-transact-sql) katalog kimlik özelliğine sahip bir sütunu tanımlamak için görünümü.

Veritabanı şeması daha iyi anlamanıza yardımcı olması için bu örnek sys.identity_column tümleştirmek gösterilmektedir ' diğer sistem Katalog görünümleri ile:

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
- Sütun ayrıca dağıtım anahtarı olduğunda
- Tablo bir dış tablo olduğunda 

SQL veri ambarı'nda aşağıdaki ilgili işlevleri desteklenmez:

- [IDENTITY()](/sql/t-sql/functions/identity-function-transact-sql)
- [@@IDENTITY](/sql/t-sql/functions/identity-transact-sql)
- [SCOPE_IDENTITY](/sql/t-sql/functions/scope-identity-transact-sql)
- [IDENT_CURRENT](/sql/t-sql/functions/ident-current-transact-sql)
- [IDENT_INCR](/sql/t-sql/functions/ident-incr-transact-sql)
- [IDENT_SEED](/sql/t-sql/functions/ident-seed-transact-sql)
- [DBCC CHECK_IDENT()](/sql/t-sql/database-console-commands/dbcc-checkident-transact-sql)

## <a name="common-tasks"></a>Genel görevler

Bu bölümde kimlik sütunu ile çalışırken ortak görevleri gerçekleştirmek için kullanabileceğiniz bazı örnek kodu sağlıyor. 

Sütun C1 tüm aşağıdaki görevler kimliğidir.
 
 
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

* Tabloları geliştirme hakkında daha fazla bilgi için [tablo] bkz: Genel Bakış [genel bakış].  

