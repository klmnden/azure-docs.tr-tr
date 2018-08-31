---
title: Azure SQL veri ambarı'nda değişkenleri atama | Microsoft Docs
description: T-SQL değişkenleri Azure SQL veri ambarı çözümleri geliştirmek için atama hakkında ipuçları.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: cf6d63c8cf67e42eed2ca52bfd0d0a3f9b0e10b1
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43302093"
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
Değişken ataması için seçin ya da güncelleştirme kullanamazsınız.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).

