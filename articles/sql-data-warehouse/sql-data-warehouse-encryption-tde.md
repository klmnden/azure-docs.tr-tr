---
title: Saydam veri şifreleme SQL Data warehouse'da (Portal) | Microsoft Docs
description: SQL Data warehouse'da saydam veri şifreleme (TDE)
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 9c4abb0416acc656a4cfae332377c398260191de
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>İle saydam veri şifreleme (TDE) SQL veri ambarı kullanmaya başlama
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
Bir SQL Data Warehouse için TDE etkinleştirmek için aşağıdaki adımları izleyin:

1. Veritabanında açmak [Azure portalı](https://portal.azure.com)
2. Veritabanı dikey penceresinde tıklayın **ayarları** düğmesi
3. Seçin **saydam veri şifreleme** seçeneği ![][1]
4. Seçin **üzerinde** ayarı ![][2]
5. Seçin **Kaydet**
   ![][3]  

## <a name="disabling-encryption"></a>Şifreleme devre dışı bırakma
Bir SQL Data Warehouse için TDE devre dışı bırakmak için aşağıdaki adımları izleyin:

1. Veritabanında açmak [Azure portalı](https://portal.azure.com)
2. Veritabanı dikey penceresinde tıklayın **ayarları** düğmesi
3. Seçin **saydam veri şifreleme** seçeneği ![][1]
4. Seçin **kapalı** ayarı ![][4]
5. Seçin **Kaydet**
   ![][5]  

## <a name="encryption-dmvs"></a>Şifreleme Dmv'leri
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
