---
title: Veri türleri - Azure SQL Data Warehouse tanımlama | Microsoft Docs
description: Azure SQL Data Warehouse'da tablo veri türlerini tanımlamak için öneriler sunar.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 4d8a222a6d4cfa4138fe833fb4e9cc895dbc5f65
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="table-data-types-in-azure-sql-data-warehouse"></a>Azure SQL Data warehouse'da tablo veri türleri
Azure SQL Data Warehouse'da tablo veri türlerini tanımlamak için öneriler sunar. 

## <a name="what-are-the-data-types"></a>Veri türleri nelerdir?

SQL veri ambarı desteklediği veri türleri en yaygın olarak kullanılır. Desteklenen veri türleri listesi için bkz: [veri türleri](/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes) CREATE TABLE deyimi içinde. 

## <a name="minimize-row-length"></a>Satır uzunluğu simge durumuna küçült
Veri türlerinin boyutu en aza indirmek için daha iyi sorgu performansını müşteri adayları satır uzunluğu kısaltır. Çalışan en küçük veri türü, verileriniz için kullanın. 

- Büyük varsayılan uzunluk karakter sütunlarla tanımlamamaya özen gösterin. Örneğin, en uzun değer 25 karakterden oluşuyorsa, sütun VARCHAR(25) tanımlayın. 
- [NVARCHAR] [VARCHAR yalnızca gerektiğinde NVARCHAR] kullanmaktan kaçının.
- Mümkün olduğunda, NVARCHAR(4000) veya VARCHAR(8000) NVARCHAR(MAX) veya VARCHAR(MAX) yerine kullanın.

PolyBase dış tablolara tablolarınızı yüklemek için kullanıyorsanız, tablo satırı tanımlanan uzunluğu 1 MB aşamaz. Değişken uzunluklu veri sahip bir satır 1 MB aşarsa, satır ile BCP, ancak değil PolyBase yükleyebilirsiniz.

## <a name="identify-unsupported-data-types"></a>Desteklenmeyen veri türlerini tanımlayın
Başka bir SQL veritabanı, veritabanınızı geçiriyorsanız, SQL veri ambarı'nda desteklenmeyen veri türleri karşılaşabilirsiniz. Desteklenmeyen veri türleri, var olan SQL şemasında bulmak için bu sorguyu kullanın.

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','xml')
 AND  y.[is_user_defined] = 1;
```


## <a name="unsupported-data-types"></a>Desteklenmeyen veri türleri için geçici çözümler

Aşağıdaki liste, veri türleri SQL Data Warehouse desteklemediği ve desteklenmeyen veri türleri yerine kullanabileceğiniz sunacaktır gösterir.

| Desteklenmeyen veri türü | Geçici çözüm |
| --- | --- |
| [Geometri](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [coğrafi konum](/sql/t-sql/spatial-geography/spatial-types-geography) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [HierarchyId](/sql/t-sql/data-types/hierarchyid-data-type-method-reference) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)(4000) |
| [Görüntü](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [Metin](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql) |
| [ntext](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql) |
| [sql_variant](/sql/t-sql/data-types/sql-variant-transact-sql) |Sütun birkaç kesin türü belirtilmiş sütuna bölün. |
| [Tablo](/sql/t-sql/data-types/table-transact-sql) |Geçici tablolara dönüşür. |
| [zaman damgası](/sql/t-sql/data-types/date-and-time-types) |Kullanmak için kodu rework [datetime2](/sql/t-sql/data-types/datetime2-transact-sql) ve [CURRENT_TIMESTAMP](/sql/t-sql/functions/current-timestamp-transact-sql) işlevi. Yalnızca sabit değerleri desteklenir varsayılan olarak, bu nedenle current_timestamp varsayılan kısıtlama olarak tanımlanamaz. Zaman damgası yazılan sütundan satır sürümü değerleri geçirmek gerekiyorsa, daha sonra kullanmak [ikili](/sql/t-sql/data-types/binary-and-varbinary-transact-sql)(8) veya [VARBINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql)(8) değil NULL veya boş satır sürüm değerleri. |
| [xml](/sql/t-sql/xml/xml-transact-sql) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql) |
| [Kullanıcı tanımlı tür](/sql/relational-databases/native-client/features/using-user-defined-types) |Geri mümkün olduğunda gibi bir yerel veri türüne dönüştürün. |
| Varsayılan değerler | Varsayılan değerleri değişmez değerleri ve yalnızca sabitleri desteklemez. |


## <a name="next-steps"></a>Sonraki adımlar
Tabloları geliştirme hakkında daha fazla bilgi için bkz: [tablo genel bakışı](sql-data-warehouse-tables-overview.md).
