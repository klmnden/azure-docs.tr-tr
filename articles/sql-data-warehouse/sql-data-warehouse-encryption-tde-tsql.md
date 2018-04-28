---
title: Saydam veri şifreleme SQL Data warehouse'da (T-SQL) | Microsoft Docs
description: Saydam veri şifreleme (TDE) SQL veri ambarı (T-SQL)
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: d10b8f751d782f00cbc58274e4b48c501cea6f70
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Saydam veri şifreleme (TDE) ile çalışmaya başlama
> [!div class="op_single_selector"]
> * [Güvenlik genel bakış](sql-data-warehouse-overview-manage-security.md)
> * [Kimlik doğrulaması](sql-data-warehouse-authentication.md)
> * [Şifreleme (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Şifreleme (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a>Gerekli izinleri
Saydam veri şifreleme (TDE) etkinleştirmek için bir yönetici veya dbmanager rolünün bir üyesi olması gerekir.

## <a name="enabling-encryption"></a>Şifrelemeyi etkinleştirme
Bir SQL Data Warehouse için TDE etkinleştirmek için şu adımları izleyin:

1. Bağlanmak *ana* bir yönetici veya bir üyesi olan bir oturum açma kullanarak veritabanını barındıran sunucuda veritabanı **dbmanager** ana veritabanı rolü
2. Veritabanı şifrelemek için şu deyimi yürütün.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Şifreleme devre dışı bırakma
Bir SQL Data Warehouse için TDE devre dışı bırakmak için aşağıdaki adımları izleyin:

1. Bağlanmak *ana* bir yönetici veya bir üyesi olan bir oturum açma kullanılarak veritabanı **dbmanager** ana veritabanı rolü
2. Veritabanı şifrelemek için şu deyimi yürütün.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> Duraklatılmış SQL Data Warehouse TDE ayarlarına değişiklik yapmadan önce sürdürüldü gerekir.
> 
> 

## <a name="verifying-encryption"></a>Şifreleme doğrulama
Bir SQL Data Warehouse için şifreleme durumunu doğrulamak için aşağıdaki adımları izleyin:

1. Bağlanmak *ana* ya da bir yönetici veya bir üyesi olan bir oturum açma kullanarak örnek veritabanı **dbmanager** ana veritabanı rolü
2. Veritabanı şifrelemek için şu deyimi yürütün.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Sonucunu ```1``` şifrelenmiş bir veritabanı gösterir ```0``` şifrelenmemiş bir veritabanını gösterir.

## <a name="encryption-dmvs"></a>Şifreleme Dmv'leri
* [sys.Databases][sys.databases] 
* [sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
