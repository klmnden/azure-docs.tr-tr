---
title: "Azure SQL Data Warehouse T-SQL Döngülerde yararlanan | Microsoft Docs"
description: "Transact-SQL döngüler ve çözümleri geliştirmek için Azure SQL Data Warehouse değiştirerek imleçler için ipuçları."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f3384b81-b943-431b-bc73-90e47e4c195f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 40a872ff310f48bfd543ac184fe7301b85b50258
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="loops-in-sql-data-warehouse"></a>SQL Data warehouse'da döngüler
SQL veri ambarı destekleyen [sırada][sırada] art arda deyim blokları yürütmek döngü. Belirtilen koşullar doğru olduğunda sürece veya kadar döngü kullanarak kod özellikle sonlandırır için devam eder `BREAK` anahtar sözcüğü. Döngüler SQL kod içinde tanımlanan imleçler değiştirme için özellikle yararlıdır. Neyse ki, SQL kodda yazılır neredeyse tüm imleçler sarma okunur yalnızca çeşitli. Bu nedenle [sırada] döngüler olup mükemmel bir alternatif kendiniz bunlardan değiştirmek zorunda bulun.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Döngüler yararlanan ve SQL Data Warehouse imleçler değiştirme
Ancak, head ilk girmeden önce kendiniz aşağıdaki soru: "Bu imleç göre ayarlama işlemleri kullanacak şekilde yeniden yazılabilir?". Çoğu durumda yanıta Evet olacaktır ve genellikle en iyi yaklaşımdır. Temel ayarlama işlemi genellikle yinelemeli, satır temelinde bir yaklaşım daha önemli ölçüde daha hızlı gerçekleştirir.

İleri Sarma salt okunur imleçler, döngü yapısı ile kolayca değiştirilebilir. Basit bir örnek aşağıda verilmiştir. Bu kod örneği, veritabanındaki her tablo için istatistiklerini güncelleştirir. Döngü tablolarda üzerinde yineleme tarafından biz her komut dizisi sorgulayabilmesi.

İlk olarak, tek tek deyimleri tanımlamak için kullanılan benzersiz bir satır numarası içeren geçici bir tablo oluşturun:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

İkinci olarak, döngü gerçekleştirmek için gereken değişkenlerini Başlat:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Şimdi bir yürütme deyimleri döngü birer birer:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Son olarak ilk adımda oluşturduğunuz geçici tablo bırakma

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[WHILE]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
