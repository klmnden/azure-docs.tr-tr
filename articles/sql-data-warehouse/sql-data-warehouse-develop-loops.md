---
title: Azure SQL veri ambarı'nda T-SQL döngülerini kullanma | Microsoft Docs
description: T-SQL döngülerini kullanma ve çözümleri geliştirmek için Azure SQL veri ambarı'nda imleçler değiştirme için ipuçları.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: c321bc4e493799a50ada4dd91faf2d2ebdee8aba
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65850470"
---
# <a name="using-t-sql-loops-in-sql-data-warehouse"></a>SQL veri ambarı'nda T-SQL döngülerini kullanma
T-SQL döngülerini kullanma ve çözümleri geliştirmek için Azure SQL veri ambarı'nda imleçler değiştirme için ipuçları.

## <a name="purpose-of-while-loops"></a>Amacı, WHILE döngüsü

SQL veri ambarı destekler [sırada](/sql/t-sql/language-elements/while-transact-sql) deyim blokları tekrar tekrar yürütmeye yönelik döngü. Belirtilen koşul true ya da kod kadar özellikle olduğu sürece bu WHILE döngüsü için devam BREAK anahtar sözcüğünü kullanarak döngüyü sonlandırır. Döngüler, SQL kod içinde tanımlanan imleçler değiştirmek için kullanışlıdır. Neyse ki, SQL kod halinde yazılmış neredeyse tüm işaretçiler ileri sarma, salt okunur çeşitli ' dir. Bu nedenle, [döngüler imleçler değiştirmek için harika bir alternatif çalışırken].

## <a name="replacing-cursors-in-sql-data-warehouse"></a>SQL veri ambarı'nda imleçler değiştirme
Ancak, head içinde ilk girmeden önce kendiniz aşağıdaki soru: "Bu imleç kümesi tabanlı işlemler kullanılacak yazılması?." Çoğu durumda, yanıt evet'tir ve genellikle en iyi yaklaşımdır. Bir set tabanlı işlemi genellikle bir yinelemeli, satır satır yaklaşım daha hızlı gerçekleştirir.

İleri Sarma salt okunur imleçler uvozuje konstruktor ile kolayca değiştirilebilir. Basit bir örnek verilmiştir. Bu kod örneği, veritabanındaki her bir tablo istatistiklerini güncelleştirir. Her komut, döngü tablolarında üzerinde yineleme tarafından sırayla yürütür.

İlk olarak, ayrı deyimler tanımlamak için kullanılan bir benzersiz bir satır numarası içeren geçici bir tablo oluşturun:

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

İkinci olarak, döngü gerçekleştirmek için gereken değişkenleri başlatın:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Artık bir yürütme deyimlerinden döngü bir zaman:

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

