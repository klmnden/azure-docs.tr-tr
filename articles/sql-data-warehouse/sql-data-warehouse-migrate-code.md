---
title: "SQL veri ambarına SQL kodunuzu geçirme | Microsoft Docs"
description: "Çözümleri geliştirme için Azure SQL Data Warehouse SQL kodunuzu geçirmek için ipuçları."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 19c252a3-0e41-4eec-9d3e-09a68c7e7add
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/23/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: c6e6b890f5e2d0e31b10bbb6803adad02bf60248
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a>SQL kodunuzu SQL veri ambarına geçirme
Bu makalede, büyük olasılıkla kodunuzun başka bir veritabanından SQL veri ambarı'na geçirirken yapmanız gerekecektir kod değişiklikleri açıklanmaktadır. Dağıtılmış bir şekilde çalışması için tasarlanmış gibi bazı SQL veri ambarı özellikleri önemli ölçüde performansını iyileştirebilir. Ancak, performansı ve ölçeği korumak için bazı özellikler de kullanılamaz.

## <a name="common-t-sql-limitations"></a>Ortak T-SQL sınırlamaları
Aşağıdaki listede, SQL Data Warehouse desteklemediği en yaygın özellikler özetlenmektedir. Bağlantılar için desteklenmeyen özellikler geçici çözümler için gerçekleştirin:

* [ANSI birleştirmeler güncelleştirmeleri][ANSI joins on updates]
* [ANSI birleştirmeler üzerinde siler][ANSI joins on deletes]
* [Merge deyimi][merge statement]
* veritabanları arası birleşimler
* [imleçler][cursors]
* [EKLE... EXEC][INSERT..EXEC]
* Output yan tümcesi
* Satır içi kullanıcı tanımlı işlevler
* birden fazla deyim işlevleri
* [Ortak tablo ifadeleri](#Common-table-expressions)
* [özyinelemeli ortak tablo ifadeleri (CTE)] (#Recursive-common-table-expressions-(CTE)
* CLR işlevleri ve yordamları
* $partition işlevi
* Tablo değişkenleri
* Tablo değeri parametreleri
* Dağıtılmış işlemler
* Commit / rollback çalışma
* işlem Kaydet
* yürütme bağlamı (EXECUTE AS)
* [Grup by tümcesi paketi / küp / gruplandırma kümeleri seçenekleri][group by clause with rollup / cube / grouping sets options]
* [iç içe geçme düzeyi 8 ötesinde][nesting levels beyond 8]
* [görünümleri aracılığıyla güncelleştirme][updating through views]
* [değişken ataması için seçimi kullanın][use of select for variable assignment]
* [dinamik SQL dizeleri için hiçbir en büyük veri türü][no MAX data type for dynamic SQL strings]

Neyse ki bu sınırlamalara çoğunu geçici çalışılan. Açıklamalar, yukarıda başvurulan ilgili geliştirme makalelerinde sağlanır.

## <a name="supported-cte-features"></a>Desteklenen CTE Özellikler
Ortak tablo ifadelerinde (Cte'lerin) SQL veri ambarı'nda kısmen desteklenir.  Aşağıdaki CTE özellikleri şu anda desteklenir:

* SELECT deyiminde bir CTE belirtilebilir.
* Bir CTE CREATE VIEW deyiminde belirtilebilir.
* Bir CTE oluşturmak tablo AS seçin (CTAS) deyiminde belirtilebilir.
* Bir CTE oluşturma uzak tablo AS seçin (CRTAS) deyiminde belirtilebilir.
* Bir CTE oluşturmak dış tablo AS seçin (CETAS) deyiminde belirtilebilir.
* Bir uzak tablo CTE başvurulabilir.
* Bir dış tablo CTE başvurulabilir.
* Birden fazla CTE sorgu tanımı bir CTE tanımlanabilir.

## <a name="cte-limitations"></a>CTE sınırlamaları
Ortak tablo ifadelerinde SQL Data Warehouse dahil, bazı sınırlamalar vardır:

* Bir CTE tek bir SELECT deyimi tarafından izlenmesi gerekir. INSERT, UPDATE, DELETE ve MERGE deyimleri desteklenmiyor.
* Başvurular kendisi (bir özyinelemeli ortak tablo ifadesi) içeren bir ortak tablo ifadesi desteklenmiyor (bölüme bakın).
* Birden çok yan tümcesi ile birlikte bir CTE belirtme izin verilmiyor. Bir CTE_query_definition bir alt sorgu içeriyorsa, örneğin, bu alt sorgu iç içe bir başka bir CTE tanımlayan yan tümcesiyle birlikte içeremez.
* TOP yan tümcesi belirtildiğinde ORDER BY yan tümcesi dışında CTE_query_definition içinde kullanılamaz.
* Bir CTE bir toplu iş parçası olan bir deyimde kullanıldığında, önceki deyimi noktalı virgülle gelmelidir.
* Sp_prepare tarafından hazırlanan deyimlerinde kullanıldığında, Cte'lerin PDW diğer SELECT deyimlerinde aynı şekilde davranır. Cte'lerin CETAS sp_prepare tarafından hazırlanan bir parçası olarak kullanılıyorsa, ancak davranışı SQL Server ve diğer PDW deyimleri bağlama için sp_prepare uygulandığı yol nedeniyle erteleneceği. SEÇİN, başvuruları CTE içinde yok yanlış bir sütun CTE kullanarak sp_prepare hata algılama olmadan geçirir, ancak hata sırasında sp_execute yerine oluşturulur.

## <a name="recursive-ctes"></a>Özyinelemeli Cte'lerin
Özyinelemeli Cte'lerin SQL veri ambarı'nda desteklenmez.  Özyinelemeli CTE geçişini biraz karmaşık olabilir ve içine birden çok adımı ayırmak için en iyi işlemidir. Genellikle, bir döngü kullanın ve geçici bir tablo özyinelemeli geçici sorguları yinelemek gibi doldurun. Geçici tablo doldurulduktan sonra için verileri tek bir sonuç kümesi olarak geri dönebilirsiniz. Benzer bir yaklaşım çözmek için kullanılan `GROUP BY WITH CUBE` içinde [Grup by tümcesi paketi / küp / gruplandırma kümeleri seçenekleri] [ group by clause with rollup / cube / grouping sets options] makalesi.

## <a name="unsupported-system-functions"></a>Desteklenmeyen sistem işlevleri
Desteklenmeyen bazı sistem işlevleri de vardır. Veri ambarı genellikle bulabileceğiniz Ana Kullanılanlar bazıları şunlardır:

* NEWSEQUENTIALID()
* @@NESTLEVEL()
* @@IDENTITY()
* @@ROWCOUNT()
* ROWCOUNT_BIG
* ERROR_LINE()

Bu sorunlardan bazıları geçici çalışılan.

## <a name="rowcount-workaround"></a>@@ROWCOUNT geçici çözüm
Desteğinin geçici olarak çözmek için@ROWCOUNT, son satır sayısı sys.dm_pdw_request_steps almak ve sonra yürütün bir saklı yordam oluşturmak `EXEC LastRowCount` DML deyimi sonra.

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a>Sonraki adımlar
Tüm desteklenen T-SQL deyimlerini tam bir listesi için bkz: [Transact-SQL konuları][Transact-SQL topics].

<!--Image references-->

<!--Article references-->
[ANSI joins on updates]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[ANSI joins on deletes]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[merge statement]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[INSERT..EXEC]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Transact-SQL topics]: ./sql-data-warehouse-reference-tsql-statements.md

[cursors]: ./sql-data-warehouse-develop-loops.md
[group by clause with rollup / cube / grouping sets options]: ./sql-data-warehouse-develop-group-by-options.md
[nesting levels beyond 8]: ./sql-data-warehouse-develop-transactions.md
[updating through views]: ./sql-data-warehouse-develop-views.md
[use of select for variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[no MAX data type for dynamic SQL strings]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->
