---
title: Azure SQL Data Warehouse'da T-SQL döngüleri kullanma | Microsoft Docs
description: T-SQL döngüleri kullanma ve çözümleri geliştirmek için Azure SQL Data Warehouse imleçler değiştirme ipuçları.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: ''
ms.assetid: f3384b81-b943-431b-bc73-90e47e4c195f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 2e49a0de0e4a6aba6639f7f3100f41c8db254220
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="using-t-sql-loops-in-sql-data-warehouse"></a>SQL veri ambarı'nda T-SQL döngüleri kullanma
T-SQL döngüleri kullanma ve çözümleri geliştirmek için Azure SQL Data Warehouse imleçler değiştirme ipuçları.

## <a name="purpose-of-while-loops"></a>Döngüler sırasında amacı

SQL veri ambarı destekleyen [sırada](/sql/t-sql/language-elements/while-transact-sql) art arda deyim blokları yürütmek döngü. İçin belirtilen koşullar doğru veya kod kadar özellikle olduğu sürece bu WHILE döngünün devam BREAK anahtar sözcüğü kullanılarak döngü sonlandırır. Döngüler, SQL kod içinde tanımlanan imleçler değiştirme için kullanışlıdır. Neyse ki, SQL kodda yazılır neredeyse tüm imleçler ileri sarma, salt okunur çeşitli ' dir. Bu nedenle, [döngüler imleçler değiştirme için mükemmel bir alternatif çalışırken].

## <a name="replacing-cursors-in-sql-data-warehouse"></a>SQL veri ambarı imleçler değiştirme
Ancak, head ilk girmeden önce kendiniz aşağıdaki soru: "Bu imleç kümesi tabanlı işlemlerini kullanmak için yazılması?." Çoğu durumda, yanıt Evet ve genellikle en iyi yaklaşımdır. Set-based işlemi genellikle yinelemeli, satır temelinde bir yaklaşım daha hızlı gerçekleştirir.

İleri Sarma salt okunur imleçler, döngü yapısı ile kolayca değiştirilebilir. Basit bir örnek verilmiştir. Bu kod örneği, veritabanındaki her tablo için istatistiklerini güncelleştirir. Döngü tablolarda üzerinde yineleme tarafından her komutun sırayla yürütür.

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

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).

