---
title: Stretch Database TSQL - Azure için saydam veri şifrelemesini etkinleştirme | Microsoft Docs
description: SQL Server Stretch Database, Azure TSQL şirket için saydam veri şifrelemesi (TDE) etkinleştir
services: sql-server-stretch-database
documentationcenter: ''
ms.assetid: 27753d91-9ca2-4d47-b34d-b5e2c2f029bb
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
author: blazem-msft
ms.author: blazem
ms.reviewer: jroth
manager: jroth
ms.openlocfilehash: 9718db18ea675fa744262f0736aff3c07732e1d1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66002867"
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>(Transact-SQL) azure'da Stretch Database için saydam veri şifrelemesi (TDE) etkinleştirme
> [!div class="op_single_selector"]
> * [Azure portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

Saydam veri şifrelemesi (TDE), kötü amaçlı etkinlik tehditlerine karşı gerçek zamanlı şifreleme ve şifre çözme veritabanını, ilişkili yedeklemeler ve işlem günlük dosyaları bekleme sırasında uygulamada değişiklik gerektirmeden gerçekleştirerek koruma yardımcı olur.

TDE, tüm veritabanı depolama veritabanı şifreleme anahtarı olarak adlandırılan bir simetrik anahtarı'nı kullanarak şifreler. Veritabanı şifreleme anahtarı, bir yerleşik bir sunucu sertifikası tarafından korunur. Azure her sunucu için benzersiz yerleşik sunucu sertifikasıdır. Microsoft, bu sertifikalar otomatik olarak en az 90 günde döndürür. TDE genel bir açıklaması için bkz. [saydam veri şifrelemesi (TDE)].

## <a name="enabling-encryption"></a>Şifrelemeyi etkinleştirme
Azure için TDE'yi etkinleştirmek için Esnetme özellikli SQL Server veritabanından veri depolama veritabanı geçişi, şunları yapın:

1. Bağlanma *ana* Azure sunucusundaki yönetici veya bir üyesi olan bir oturum açma kimliğini kullanarak veritabanını barındıran veritabanı **dbmanager** asıl veritabanı rolü
2. Veritabanı şifrelemek için aşağıdaki deyimi yürütün.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Şifreleme devre dışı bırakma
Azure için TDE devre dışı bırakmak için Esnetme özellikli SQL Server veritabanından veri depolama veritabanı geçişi, şunları yapın:

1. Bağlanma *ana* bir yönetici ya da bir üyesi olan bir oturum açma kimliğini kullanarak veritabanı **dbmanager** asıl veritabanı rolü
2. Veritabanı şifrelemek için aşağıdaki deyimi yürütün.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

## <a name="verifying-encryption"></a>Şifreleme doğrulama
Esnetme özellikli SQL Server veritabanından Azure veritabanı, veri depolama için şifreleme durumu geçişi doğrulamak için şunları yapın:

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

<!--Anchors-->
[Saydam Veri Şifrelemesi (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
