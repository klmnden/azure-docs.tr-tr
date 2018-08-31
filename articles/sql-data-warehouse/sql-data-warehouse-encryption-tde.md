---
title: Saydam veri şifrelemesi SQL veri ambarı (Portal) | Microsoft Docs
description: SQL veri ambarı'nda saydam veri şifrelemesi (TDE)
services: sql-data-warehouse
author: kavithaj
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: e641ddb5360c18b5977fb79de41334d4216c2b90
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306196"
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>İle saydam veri şifrelemesi (TDE) SQL veri ambarı'nda kullanmaya başlayın
> [!div class="op_single_selector"]
> * [Güvenliğe genel bakış](sql-data-warehouse-overview-manage-security.md)
> * [Kimlik doğrulaması](sql-data-warehouse-authentication.md)
> * [Şifreleme (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Şifreleme (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a>Gerekli izinler
Saydam veri şifrelemesi (TDE) etkinleştirmek için bir yönetici veya dbmanager rolünün bir üyesi olması gerekir.

## <a name="enabling-encryption"></a>Şifrelemeyi etkinleştirme
SQL veri ambarı için TDE'yi etkinleştirmek için aşağıdaki adımları izleyin:

1. Veritabanında açın [Azure portalı](https://portal.azure.com)
2. Veritabanı dikey penceresinde **ayarları** düğmesi
3. Seçin **saydam veri şifrelemesi** seçeneği ![][1]
4. Seçin **üzerinde** ayarı ![][2]
5. Seçin **Kaydet**
   ![][3]  

## <a name="disabling-encryption"></a>Şifreleme devre dışı bırakma
SQL veri ambarı için TDE devre dışı bırakmak için aşağıdaki adımları izleyin:

1. Veritabanında açın [Azure portalı](https://portal.azure.com)
2. Veritabanı dikey penceresinde **ayarları** düğmesi
3. Seçin **saydam veri şifrelemesi** seçeneği ![][1]
4. Seçin **kapalı** ayarı ![][4]
5. Seçin **Kaydet**
   ![][5]  

## <a name="encryption-dmvs"></a>Şifreleme Dmv'ler
Şifreleme ile aşağıdaki Dmv'leri onaylanabilir:

* [sys.Databases]
* [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
