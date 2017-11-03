---
title: "SQL veri ambarı değişkenlerde Ata | Microsoft Docs"
description: "Çözümleri geliştirme için Azure SQL Data Warehouse Transact-SQL değişkenleri atamak için ipuçları."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 81ddc7cf-a6ba-4585-91a3-b6ea50f49227
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 045d5148cd3f12dac63c961ccf7c953d355ed725
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="assign-variables-in-sql-data-warehouse"></a>SQL veri ambarı değişkenlerde atayın
SQL veri ambarı değişkenleri kullanılarak ayarlanır `DECLARE` deyimi veya `SET` deyimi.

Aşağıdakilerin tümü bir değişken değerini ayarlamak için mükemmel geçerli yöntemlerdir:

## <a name="setting-variables-with-declare"></a>DECLARE değişkenlerle ayarlama
DECLARE değişkenlerle başlatılıyor SQL veri ambarı'nda bir değişken değerini ayarlamak için en esnek yollardan biridir.

```sql
DECLARE @v  int = 0
;
```

DECLARE, aynı anda birden fazla değişken ayarlamak üzere de kullanabilirsiniz. Kullanamazsınız `SELECT` veya `UPDATE` Bunu yapmak için:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

İnitialise ve bir değişkeni aynı DECLARE deyimi kullanın. Aşağıdaki örnekte olduğu noktası göstermeye **değil** izin @p1 hem başlatılır ve aynı DECLARE deyiminde kullanılan. Bu bir hatayla sonuçlanır.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>SET değerleri ayarlama
Kümesi, tek bir değişken ayarlamak için yaygın bir yöntemdir.

Tüm örnekler kümesine sahip bir değişken ayarı geçerli yöntemler şunlardır:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

KÜMESİYLE aynı anda yalnızca bir değişken ayarlayabilirsiniz. Ancak, yukarıda görüldüğü gibi bileşik işleçler kabul edilebilir değildir.

## <a name="limitations"></a>Sınırlamalar
Değişken ataması için SELECT veya UPDATE kullanamazsınız.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
