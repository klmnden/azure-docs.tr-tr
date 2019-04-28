---
title: Saydam veri şifrelemesi SQL veri ambarı (T-SQL) | Microsoft Docs
description: Itanium tabanlı sistemler için saydam veri şifrelemesi (TDE) SQL veri ambarı (T-SQL)
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
origin.date: 04/17/2018
ms.date: 03/25/2019
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: 7908e6e0927357446ea45e16b7c44adb83ec1fd3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61439069"
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Saydam veri şifrelemesi (TDE) ile çalışmaya başlama
> [!div class="op_single_selector"]
> * [Güvenliğe genel bakış](sql-data-warehouse-overview-manage-security.md)
> * [Kimlik doğrulaması](sql-data-warehouse-authentication.md)
> * [Şifreleme (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Şifreleme (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permissions"></a>Gerekli izinler
Saydam veri şifrelemesi (TDE) etkinleştirmek için bir yönetici veya dbmanager rolünün bir üyesi olması gerekir.

## <a name="enabling-encryption"></a>Şifrelemeyi etkinleştirme
SQL veri ambarı için TDE'yi etkinleştirmek için aşağıdaki adımları izleyin:

1. Bağlanma *ana* bir yönetici ya da bir üyesi olan bir oturum açma kullanarak veritabanını barındıran sunucuda veritabanı **dbmanager** asıl veritabanı rolü
2. Veritabanı şifrelemek için aşağıdaki deyimi yürütün.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Şifreleme devre dışı bırakma
SQL veri ambarı için TDE devre dışı bırakmak için aşağıdaki adımları izleyin:

1. Bağlanma *ana* bir yönetici ya da bir üyesi olan bir oturum açma kimliğini kullanarak veritabanı **dbmanager** asıl veritabanı rolü
2. Veritabanı şifrelemek için aşağıdaki deyimi yürütün.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> Duraklatılmış bir SQL veri ambarı TDE ayarlarında değişiklik yapmadan önce sürdürüldü gerekir.
> 
> 

## <a name="verifying-encryption"></a>Şifreleme doğrulama
SQL veri ambarı için şifreleme durumunu doğrulamak için aşağıdaki adımları izleyin:

1. Bağlanma *ana* veya bir yönetici ya da bir üyesi olan bir oturum açma kullanarak örnek veritabanına **dbmanager** asıl veritabanı rolü
2. Veritabanı şifrelemek için aşağıdaki deyimi yürütün.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Sonucu ```1``` şifrelenmiş bir veritabanı gösterir ```0``` şifrelenmemiş bir veritabanını gösterir.

## <a name="encryption-dmvs"></a>Şifreleme Dmv'ler
* [sys.databases][sys.databases] 
* [sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->