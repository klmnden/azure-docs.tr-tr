---
title: Veri türleri Kılavuzu - Azure SQL Data Warehouse | Microsoft Docs
description: SQL veri ambarı ile uyumlu olan veri türlerini tanımlamak için öneriler sunar.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: ''
ms.assetid: d4a1f0a3-ba9f-44b9-95f6-16a4f30746d6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 03/17/2018
ms.author: barbkess
ms.openlocfilehash: dcdcb6eddf35fe3ec4754353452c68cd3e24f907
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="guidance-for-defining-data-types-for-tables-in-sql-data-warehouse"></a>SQL veri ambarı'nda tabloları veri türlerini tanımlama Kılavuzu
Bu öneriler, SQL Data Warehouse ile uyumlu olan tablo veri türlerini tanımlamak için kullanın. Uyumluluk ek olarak, veri türlerinin boyutu en aza sorgu performansını artırır.

SQL veri ambarı desteklediği veri türleri en yaygın olarak kullanılır. Desteklenen veri türleri listesi için bkz: [veri türleri](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes) CREATE TABLE deyimi içinde. 


## <a name="minimize-row-length"></a>Satır uzunluğu simge durumuna küçült
Veri türlerinin boyutu en aza indirmek için daha iyi sorgu performansını müşteri adayları satır uzunluğu kısaltır. Çalışan en küçük veri türü, verileriniz için kullanın. 

- Büyük varsayılan uzunluk karakter sütunlarla tanımlamamaya özen gösterin. Örneğin, en uzun değer 25 karakterden oluşuyorsa, sütun VARCHAR(25) tanımlayın. 
- Kullanmaktan kaçının [NVARCHAR] [ NVARCHAR] yalnızca gerektiğinde VARCHAR.
- Mümkün olduğunda, NVARCHAR(4000) veya VARCHAR(8000) NVARCHAR(MAX) veya VARCHAR(MAX) yerine kullanın.

Polybase tablolarınızı yüklemek için kullanıyorsanız, tablo satırı tanımlanan uzunluğu 1 MB aşamaz. Değişken uzunluklu veri sahip bir satır 1 MB aşarsa, satır ile BCP, ancak değil PolyBase yükleyebilirsiniz.

## <a name="identify-unsupported-data-types"></a>Desteklenmeyen veri türlerini tanımlayın
Başka bir SQL veritabanı, veritabanınızı geçiriyorsanız, SQL veri ambarı'nda desteklenmeyen veri türleri karşılaşabilirsiniz. Desteklenmeyen veri türleri, var olan SQL şemasında bulmak için bu sorguyu kullanın.

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```


## <a name="unsupported-data-types"></a>Desteklenmeyen veri türleri için kullanım geçici çözümler

Aşağıdaki liste, veri türleri SQL Data Warehouse desteklemediği ve desteklenmeyen veri türleri yerine kullanabileceğiniz sunacaktır gösterir.

| Desteklenmeyen veri türü | Geçici çözüm |
| --- | --- |
| [Geometri][geometry] |[varbinary][varbinary] |
| [coğrafi konum][geography] |[varbinary][varbinary] |
| [hierarchyid][hierarchyid] |[nvarchar][nvarchar](4000) |
| [image][ntext,text,image] |[varbinary][varbinary] |
| [text][ntext,text,image] |[varchar][varchar] |
| [ntext][ntext,text,image] |[nvarchar][nvarchar] |
| [sql_variant][sql_variant] |Sütun birkaç kesin türü belirtilmiş sütuna bölün. |
| [Tablo][table] |Geçici tablolara dönüşür. |
| [timestamp][timestamp] |Kullanmak için kodu rework [datetime2] [ datetime2] ve `CURRENT_TIMESTAMP` işlevi.  Yalnızca sabit değerleri desteklenir varsayılan olarak, bu nedenle current_timestamp varsayılan kısıtlama olarak tanımlanamaz. Zaman damgası yazılan sütundan satır sürümü değerleri geçirmek gerekiyorsa, daha sonra kullanmak [ikili][BINARY](8) veya [VARBINARY][BINARY](8) için NOT NULL veya Satır sürümü değerleri NULL. |
| [xml][xml] |[varchar][varchar] |
| [Kullanıcı tanımlı tür][user defined types] |Geri mümkün olduğunda gibi bir yerel veri türüne dönüştürün. |
| Varsayılan değerler | Varsayılan değerleri değişmez değerleri ve yalnızca sabitleri desteklemez.  Belirleyici olmayan ifadeleri ve İşlevler, gibi `GETDATE()` veya `CURRENT_TIMESTAMP`, desteklenmez. |


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz:

- [SQL veri ambarı en iyi uygulamalar][SQL Data Warehouse Best Practices]
- [Tablo genel bakış][Overview]
- [Bir tablo dağıtma][Distribute]
- [Bir tablo dizin oluşturma][Index]
- [Bir tablo bölümleme][Partition]
- [Tablo istatistikleri koruma][Statistics]
- [Geçici tablolar][Temporary]

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
[create table]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[binary]: https://msdn.microsoft.com/library/ms188362.aspx
[bit]: https://msdn.microsoft.com/library/ms177603.aspx
[char]: https://msdn.microsoft.com/library/ms176089.aspx
[date]: https://msdn.microsoft.com/library/bb630352.aspx
[datetime]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[datetimeoffset]: https://msdn.microsoft.com/library/bb630289.aspx
[decimal]: https://msdn.microsoft.com/library/ms187746.aspx
[float]: https://msdn.microsoft.com/library/ms173773.aspx
[geometry]: https://msdn.microsoft.com/library/cc280487.aspx
[geography]: https://msdn.microsoft.com/library/cc280766.aspx
[hierarchyid]: https://msdn.microsoft.com/library/bb677290.aspx
[int]: https://msdn.microsoft.com/library/ms187745.aspx
[money]: https://msdn.microsoft.com/library/ms179882.aspx
[nchar]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[real]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[table]: https://msdn.microsoft.com/library/ms175010.aspx
[time]: https://msdn.microsoft.com/library/bb677243.aspx
[timestamp]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[uniqueidentifier]: https://msdn.microsoft.com/library/ms187942.aspx
[varbinary]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[xml]: https://msdn.microsoft.com/library/ms187339.aspx
[user defined types]: https://msdn.microsoft.com/library/ms131694.aspx
