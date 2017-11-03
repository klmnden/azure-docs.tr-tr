---
title: "Saydam veri şifreleme Esnetme veritabanı TSQL - Azure için etkinleştirme | Microsoft Docs"
description: "Saydam veri şifreleme (TDE) Azure TSQL üzerinde SQL Server Esnetme veritabanı için etkinleştirin"
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: 27753d91-9ca2-4d47-b34d-b5e2c2f029bb
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: anvang
ms.openlocfilehash: ed26c2b386e08b76f78b4a05e12c46d2b97c20f2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Saydam veri şifreleme (TDE) Azure (Transact-SQL) üzerinde Esnetme veritabanı için etkinleştirin
> [!div class="op_single_selector"]
> * [Azure portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

Saydam veri şifreleme (TDE) gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemelerinizi ve geri kalan işlem günlüğü dosyalarını uygulamasında yapılacak değişiklikler gerek kalmadan gerçekleştirerek kötü amaçlı etkinliği tehdide karşı korunmasına yardımcı olur.

TDE, veritabanının tamamını Depolama veritabanı şifreleme anahtarını adlı bir simetrik anahtar kullanarak şifreler. Veritabanı şifreleme anahtarını bir yerleşik bir sunucu sertifikası tarafından korunur. Yerleşik bir sunucu sertifikası Azure her sunucu için benzersizdir. Microsoft, bu sertifikalar otomatik olarak en az 90 günde döndürür. TDE genel bir açıklaması için bkz [saydam veri şifreleme (TDE)].

## <a name="enabling-encryption"></a>Şifrelemeyi etkinleştirme
TDE Azure etkinleştirmek için bir SQL Server Esnetme etkinleştirilmiş veritabanından veri depolama veritabanı geçişi, şunları yapın:

1. Bağlanmak *ana* veritabanından bir yönetici veya bir üyesi olan bir oturum açma kullanarak veritabanını barındıran Azure sunucusuna **dbmanager** ana veritabanı rolü
2. Veritabanı şifrelemek için şu deyimi yürütün.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Şifreleme devre dışı bırakma
Azure TDE devre dışı bırakmak için bir SQL Server Esnetme etkinleştirilmiş veritabanından veri depolama veritabanı geçişi, şunları yapın:

1. Bağlanmak *ana* bir yönetici veya bir üyesi olan bir oturum açma kullanılarak veritabanı **dbmanager** ana veritabanı rolü
2. Veritabanı şifrelemek için şu deyimi yürütün.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

## <a name="verifying-encryption"></a>Şifreleme doğrulama
Bir SQL Server Esnetme etkinleştirilmiş veritabanından verileri depolarken bir Azure veritabanı için şifreleme durum geçişi doğrulamak için şunları yapın:

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

<!--Anchors-->
[saydam veri şifreleme (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
