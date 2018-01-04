---
title: "Saydam veri şifreleme SQL Data warehouse'da (Portal) | Microsoft Docs"
description: "SQL Data warehouse'da saydam veri şifreleme (TDE)"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: fabf75d3-9bbf-4e0d-9b31-8b5a8713f08d
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: b1db3bdfdfb54bda325c9b971cfcb4dd5efa333a
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
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
3. Seçin **saydam veri şifreleme** seçeneği![][1]
4. Seçin **üzerinde** ayarı![][2]
5. Seçin **Kaydet**
   ![][3]  

## <a name="disabling-encryption"></a>Şifreleme devre dışı bırakma
Bir SQL Data Warehouse için TDE devre dışı bırakmak için aşağıdaki adımları izleyin:

1. Veritabanında açmak [Azure portalı](https://portal.azure.com)
2. Veritabanı dikey penceresinde tıklayın **ayarları** düğmesi
3. Seçin **saydam veri şifreleme** seçeneği![][1]
4. Seçin **kapalı** ayarı![][4]
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
