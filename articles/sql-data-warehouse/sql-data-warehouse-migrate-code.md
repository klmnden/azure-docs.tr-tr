---
title: SQL veri ambarı'na SQL kodunuzu geçirme | Microsoft Docs
description: SQL kodunuzu çözümleri geliştirmek için Azure SQL veri ambarı'na geçirmek için ipuçları.
services: sql-data-warehouse
author: jrowlandjones
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: jrj
ms.reviewer: igorstan
ms.openlocfilehash: fae3ae16ee0100ad446c0b6c7851553a3376bb4f
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400971"
---
# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a>SQL veri ambarı'na SQL kodunuzu geçirme

Bu makalede, büyük olasılıkla, kodunuzun başka bir veritabanından SQL veri ambarı'na geçiş yapmak için ihtiyacınız olacak kod değişiklikleri açıklar. Dağıtılmış bir şekilde çalışmak için tasarlandığı gibi bazı SQL veri ambarı özellikleri performansını önemli ölçüde artırabilir. Ancak, performansı ve ölçeği sürdürmek istiyorsanız, bazı özellikler de kullanılabilir değil.

## <a name="common-t-sql-limitations"></a>Ortak T-SQL sınırlamaları

Aşağıdaki liste, SQL veri ambarı desteklemiyor en yaygın özellikler özetlenmiştir. Bağlantılar, desteklenmeyen özellikler için geçici çözümler ulaşmanızı sağlar:

* [ANSI birleştirmeler güncelleştirmeleri][ANSI joins on updates]
* [ANSI birleştirmeler üzerinde siler][ANSI joins on deletes]
* [Merge deyimi][merge statement]
* veritabanları arası birleştirmeler
* [İmleçler][cursors]
* [EKLE... EXEC][INSERT..EXEC]
* Output yan tümcesi
* Satır içi kullanıcı tanımlı işlevler
* çok deyimli İşlevler
* Ortak tablo ifadeleri
* [özyinelemeli ortak tablo ifadeleri (CTE)] (#Recursive-common-table-expressions-(CTE)
* CLR işlevleri ve yordamları
* $partition işlevi
* Tablo değişkenleri
* Tablo değer parametreleri
* Dağıtılmış işlemler
* Commit / rollback çalışma
* işlem kaydedin
* yürütme bağlamları (EXECUTE AS)
* [Gruplandırma ölçütü yan tümcesinde toplama / küp / gruplandırma kümeleri seçenekleri][group by clause with rollup / cube / grouping sets options]
* [iç içe geçme düzeyi 8 ötesinde][nesting levels beyond 8]
* [görünümlerini güncelleştirme][updating through views]
* [dinamik SQL dizeleri için hiçbir en fazla veri türü][no MAX data type for dynamic SQL strings]

Neyse ki bu sınırlamaların çoğunu etrafında çalışılabilmesi. Açıklamalar yukarıda anılan ilgili geliştirmesi makalelerine sağlanır.

## <a name="supported-cte-features"></a>Desteklenen CTE özellikleri

Ortak tablo ifadeleri (Cte'lerin), SQL veri ambarı'nda kısmen desteklenir.  Aşağıdaki CTE özellikleri şu anda desteklenmektedir:

* Bir SELECT deyiminde bir CTE belirtilebilir.
* Görünüm Oluştur deyiminde bir CTE belirtilebilir.
* Bir CREATE TABLE AS SELECT (CTAS) deyiminde bir CTE belirtilebilir.
* Bir oluşturma uzak tablo AS seçin (CRTAS) deyiminde bir CTE belirtilebilir.
* Bir oluşturma dış tablo AS seçin (CETAS) deyiminde bir CTE belirtilebilir.
* Uzak tablo bir CTE başvurulabilir.
* Bir dış tablo bir CTE başvurulabilir.
* Birden çok CTE sorgu tanımı bir CTE içinde tanımlanabilir.

## <a name="cte-limitations"></a>CTE sınırlamaları

Ortak tablo ifadelerinde SQL veri ambarı dahil olmak üzere, bazı sınırlamalar mevcuttur:

* Bir CTE tek bir SELECT deyimi tarafından izlenmesi gerekir. INSERT, UPDATE, DELETE ve birleştirme deyimleri desteklenmiyor.
* (Bir özyinelemeli ortak tablo ifadesi) kendine başvuruları içeren bir ortak tablo ifadesi desteklenmiyor (aşağıdaki bölüme bakın).
* Birden çok yan tümcesine sahip bir CTE içinde belirtme izin verilmez. Bir CTE_query_definition alt sorgu içeriyorsa, örneğin, bu alt sorgu iç içe bir başka bir CTE tanımlayan yan tümcesi ile içeremez.
* TOP yan tümcesi belirtildiğinde ORDER BY yan tümcesi dışında CTE_query_definition içinde kullanılamaz.
* Bir toplu işin bir parçası olan bir deyimde bir CTE kullanıldığında, önceki deyimi noktalı virgül gelmelidir.
* Sp_prepare tarafından hazırlanmış deyimleri kullanıldığında, Cte'lerin PDW diğer SELECT deyimlerinde aynı şekilde davranır. Cte'lerin CETAS sp_prepare tarafından hazırlanmış bir parçası olarak kullanılır, ancak davranışı SQL Server ve diğer PDW deyimleri bağlama için sp_prepare uygulanma biçimi nedeniyle yayımlanmalarından sonra. SELECT, başvuruları içinde CTE yok yanlış bir sütun CTE kullanarak sp_prepare hata algılama olmadan geçer, ancak hata sırasında sp_execute yerine oluşturulur.

## <a name="recursive-ctes"></a>Özyinelemeli Cte'lerin

Özyinelemeli Cte'lerin SQL veri ambarı'nda desteklenmez.  Özyinelemeli CTE geçişini biraz karmaşık olabilir ve birden çok adımı ayırmak için en iyi işlemidir. Genellikle döngü kullanma ve geçici bir tablo üzerinde yinelenen geçici sorgular yineleme gibi doldurun. Geçici tablo doldurulduktan sonra için verileri tek bir sonuç kümesi iade edebilirsiniz. Benzer bir yaklaşım çözmek için kullanılan `GROUP BY WITH CUBE` içinde [gruplandırma ölçütü yan tümcesinde toplama / küp / gruplandırma kümeleri seçenekleri] [ group by clause with rollup / cube / grouping sets options] makalesi.

## <a name="unsupported-system-functions"></a>Desteklenmeyen sistem işlevleri

Desteklenmeyen bazı sistem işlevleri vardır. Veri ambarı ' kullanılan genellikle bulabileceğiniz yayılmakta bazıları şunlardır:

* NEWSEQUENTIALID()
* @@NESTLEVEL()
* @@IDENTITY()
* @@ROWCOUNT()
* ROWCOUNT_BIG
* ERROR_LINE()

Bu sorunlardan bazıları geçici çalışılabilmesi.

## <a name="rowcount-workaround"></a>@@ROWCOUNT geçici çözüm

Geçici çözüm desteğinin @@ROWCOUNT, sys.dm_pdw_request_steps son satır sayısı almak ve sonra yürütme bir saklı yordam oluşturma `EXEC LastRowCount` DML deyimi sonra.

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

Desteklenen tüm T-SQL deyimleri tam bir listesi için bkz. [Transact-SQL konuları][Transact-SQL topics].

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
