---
title: Azure SQL Data Warehouse değişkenlerde Ata | Microsoft Docs
description: Çözümleri geliştirme için Azure SQL Data Warehouse T-SQL değişkenleri atamak için ipuçları.
services: sql-data-warehouse
author: ckarst
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 09b0ee336ce00eb20ea501cd97833dfdd6540b30
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="assigning-variables-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse değişkenlerde atama
Çözümleri geliştirme için Azure SQL Data Warehouse T-SQL değişkenleri atamak için ipuçları.

## <a name="setting-variables-with-declare"></a>DECLARE değişkenlerle ayarlama
SQL veri ambarı değişkenleri kullanılarak ayarlanır `DECLARE` deyimi veya `SET` deyimi. DECLARE değişkenlerle başlatılıyor SQL veri ambarı'nda bir değişken değerini ayarlamak için en esnek yollardan biridir.

```sql
DECLARE @v  int = 0
;
```

DECLARE, aynı anda birden fazla değişken ayarlamak üzere de kullanabilirsiniz. Aşağıdakileri yapmak için SELECT veya UPDATE kullanamazsınız:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Başlatma ve bir değişkeni aynı DECLARE deyimi kullanın. Noktayı anlamak için aşağıdaki örnekte olduğu **değil** bu yana izin @p1 hem başlatılır ve aynı DECLARE deyiminde kullanılan. Aşağıdaki örnek, bir hata verir.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>SET değerleri ayarlama
KÜMESİ, tek bir değişken ayarlamak için yaygın bir yöntemdir.

Aşağıdaki deyimleri kümesine sahip bir değişken ayarlamak üzere tüm geçerli yöntemlerdir:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

KÜMESİYLE aynı anda yalnızca bir değişken ayarlayabilirsiniz. Ancak, bileşik işleçler verilebilir.

## <a name="limitations"></a>Sınırlamalar
Değişken ataması için SELECT veya UPDATE kullanamazsınız.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).

