---
title: Veri türleri - Azure SQL veri ambarı tanımlama | Microsoft Docs
description: Azure SQL Data Warehouse'da tablo veri türleri tanımlamak için öneriler sunar.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 06a273d3bfd5d416039a992e36bd4b0f72a85f78
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65851558"
---
# <a name="table-data-types-in-azure-sql-data-warehouse"></a>Azure SQL Data warehouse'da tablo veri türleri
Azure SQL Data Warehouse'da tablo veri türleri tanımlamak için öneriler sunar. 

## <a name="what-are-the-data-types"></a>Veri türleri nelerdir?

SQL veri ambarı desteklediği veri türleri en yaygın olarak kullanılır. Desteklenen veri türleri listesi için bkz. [veri türleri](/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes) CREATE TABLE deyiminde. 

## <a name="minimize-row-length"></a>Satır uzunluğu en aza indirin
Veri türlerinin boyutu en aza indirmek için daha iyi sorgu performansını müşteri adayları satır uzunluğu kısaltır. Verileriniz için çalışan en küçük veri türünü kullanın. 

- Karakter sütunları büyük varsayılan uzunluğu tanımlamamaya özen gösterin. Örneğin, en uzun değer 25 karakterse, sütununuzu VARCHAR(25) olarak tanımlayın. 
- [NVARCHAR] [VARCHAR, yalnızca ihtiyacınız olduğunda NVARCHAR] kullanmaktan kaçının.
- Mümkün olduğunda, NVARCHAR(MAX) veya VARCHAR(MAX) yerine NVARCHAR(4000) veya VARCHAR(8000) kullanın.

Tablolara yüklemek için PolyBase dış tablolar kullanıyorsanız, tanımlanan tablo satırı 1 MB uzunluğunda olabilir. Bir değişken uzunluklu veri satırı 1 MB aşarsa, BCP ile ancak PolyBase ile değil satır yükleyebilirsiniz.

## <a name="identify-unsupported-data-types"></a>Desteklenmeyen veri türleri tanımlama
Başka bir SQL veritabanı'ndan veritabanınızı geçiriyorsanız, SQL veri ambarı'nda desteklenmeyen veri türleri karşılaşabilirsiniz. Desteklenmeyen veri türleri, mevcut SQL şemasında bulmak için bu sorguyu kullanın.

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','xml')
 AND  y.[is_user_defined] = 1;
```


## <a name="unsupported-data-types"></a>Desteklenmeyen veri türleri için geçici çözümler

Aşağıdaki liste, SQL veri ambarı desteklemez ve desteklenmeyen veri türleri yerine kullanabileceğiniz sunacaktır veri türlerini gösterir.

| Desteklenmeyen veri türü | Geçici Çözüm |
| --- | --- |
| [Geometri](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [Coğrafi konum](/sql/t-sql/spatial-geography/spatial-types-geography) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [HierarchyId](/sql/t-sql/data-types/hierarchyid-data-type-method-reference) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)(4000) |
| [Görüntü](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [text](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql) |
| [ntext](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql) |
| [sql_variant](/sql/t-sql/data-types/sql-variant-transact-sql) |Kesin türü belirtilmiş olan çeşitli sütunlara sütunu Böl. |
| [Tablo](/sql/t-sql/data-types/table-transact-sql) |Geçici tablolara dönüştürün. |
| [Zaman damgası](/sql/t-sql/data-types/date-and-time-types) |Kullanmak için kodu yeniden [datetime2](/sql/t-sql/data-types/datetime2-transact-sql) ve [CURRENT_TIMESTAMP](/sql/t-sql/functions/current-timestamp-transact-sql) işlevi. Yalnızca sabitler desteklenir varsayılan olarak, bu nedenle current_timestamp varsayılan kısıtlama olarak tanımlanamaz. Zaman damgası türü belirtilmiş bir sütundan satır sürümü değerlerine geçmeniz gerekiyorsa, ardından kullanmak [ikili](/sql/t-sql/data-types/binary-and-varbinary-transact-sql)(8) veya [VARBINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql)(8) değil NULL veya boş satır sürümü değerleri. |
| [xml](/sql/t-sql/xml/xml-transact-sql) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql) |
| [Kullanıcı tanımlı tür](/sql/relational-databases/native-client/features/using-user-defined-types) |Geri mümkün olduğunda gibi bir yerel veri türüne dönüştürün. |
| Varsayılan değerler | Varsayılan değerleri değişmez değerler ve yalnızca sabitleri desteklemez. |


## <a name="next-steps"></a>Sonraki adımlar
Tablo geliştirme hakkında daha fazla bilgi için bkz. [tabloya genel bakış](sql-data-warehouse-tables-overview.md).
