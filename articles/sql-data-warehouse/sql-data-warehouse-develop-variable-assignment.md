---
title: Azure SQL veri ambarı'nda değişkenleri atama | Microsoft Docs
description: T-SQL değişkenleri Azure SQL veri ambarı çözümleri geliştirmek için atama hakkında ipuçları.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: ee97f7e5cda8b954fb697f73746e416d88d38c2d
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401687"
---
# <a name="assigning-variables-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'nda değişkenleri atama

T-SQL değişkenleri Azure SQL veri ambarı çözümleri geliştirmek için atama hakkında ipuçları.

## <a name="setting-variables-with-declare"></a>DECLARE değişkenlerle ayarlama

SQL veri ambarı'nda değişkenleri kullanılarak ayarlanır `DECLARE` deyimi veya `SET` deyimi. DECLARE değişkenlerle başlatma SQL veri ambarı'nda bir değişken değeri ayarlamak için en esnek yollarından biridir.

```sql
DECLARE @v  int = 0
;
```

DECLARE, aynı anda birden fazla değişkenini ayarlamak için de kullanabilirsiniz. Aşağıdakileri yapmak için seçin ya da güncelleştirme kullanamazsınız:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Başlatma ve aynı DECLARE deyimi içinde bir değişkeni kullanın. Anlaşılması için aşağıdaki örnek, **değil** beri izin @p1 hem başlatılır ve aynı DECLARE deyimi içinde kullanılır. Aşağıdaki örnek, bir hata verir.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Ayar değerleri KÜMESİ ile

KÜMESİ, tek bir değişken ayarlamak için yaygın bir yöntemdir.

Aşağıdaki deyimleri KÜMESİYLE bir değişken ayarlamak için geçerli yöntemlerdir:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

KÜMESİ ile aynı anda yalnızca bir değişken ayarlayabilirsiniz. Ancak, bileşik işleçleri verilebilir.

## <a name="limitations"></a>Sınırlamalar

Değişken ataması için güncelleştirme kullanamazsınız.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).
