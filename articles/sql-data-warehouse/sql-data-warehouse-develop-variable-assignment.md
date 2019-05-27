---
title: Azure SQL veri ambarı'nda değişkenleri atama | Microsoft Docs
description: T-SQL değişkenleri Azure SQL veri ambarı çözümleri geliştirmek için atama hakkında ipuçları.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 62c4273a02e02aff268a96e1b13483088ba33f87
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65861677"
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
