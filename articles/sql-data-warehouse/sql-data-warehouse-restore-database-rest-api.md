---
title: "Bir Azure SQL veri ambarı (REST API'si) geri | Microsoft Docs"
description: "Azure SQL Data Warehouse geri yüklemek için REST API görevler."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: fca922c6-b675-49c7-907e-5dcf26d451dd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: 8656607611e7518e42b51b91774f55abec15c228
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Bir Azure SQL veri ambarı (REST API'si) geri yükleme
> [!div class="op_single_selector"]
> * [Genel bakış][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

Bu makalede, REST API kullanarak Azure SQL Data Warehouse geri yükleme öğreneceksiniz.

## <a name="before-you-begin"></a>Başlamadan önce
**DTU kapasitenizi doğrulayın.** Her SQL veri ambarı varsayılan DTU kota olan bir SQL server tarafından (örneğin myserver.database.windows.net) barındırılıyor.  SQL Data Warehouse geri yükleyebilmeniz için önce doğrulayın yeterli kalan DTU kota geri yüklenen veritabanı için SQL server'ınızdaki sahiptir. Gerekli DTU hesaplamak için ya da daha fazla DTU istemek için öğrenmek için bkz: [DTU kota değişiklik isteği][Request a DTU quota change].

## <a name="restore-an-active-or-paused-database"></a>Etkin ya da duraklatılmış bir veritabanını geri yükle
Bir veritabanını geri yüklemek için:

1. Get veritabanı geri yükleme noktaları işlemi kullanarak veritabanını geri yükleme noktaları listesini alın.
2. Geri yükleme kullanarak başlayın [oluşturma veritabanı geri yükleme isteği] [ Create database restore request] işlemi.
3. Kullanarak geri yükleme durumunu izlemek [veritabanı işlemi durumunu] [ Database operation status] işlemi.

> [!NOTE]
> Geri yükleme tamamlandıktan sonra izleyerek, kurtarılan veritabanının yapılandırabilirsiniz [kurtarma işleminden sonra veritabanını yapılandırma][Configure your database after recovery].
> 
> 

## <a name="restore-a-deleted-database"></a>Silinen veritabanını geri yükleme
Silinen bir veritabanını geri yüklemek için:

1. Tüm geri yüklenebilen silinmiş veritabanlarınızın kullanarak liste [listesi geri yüklenebilen bırakılan veritabanları] [ List restorable dropped databases] işlemi.
2. Kullanarak geri yüklemek istediğiniz silinmiş veritabanı için Ayrıntılar elde [Get geri yüklenebilen bırakılan veritabanı] [ Get restorable dropped database] işlemi.
3. Geri yükleme kullanarak başlayın [oluşturma veritabanı geri yükleme isteği] [ Create database restore request] işlemi.
4. Kullanarak geri yükleme durumunu izlemek [veritabanı işlemi durumunu] [ Database operation status] işlemi.

> [!NOTE]
> Geri yükleme tamamlandıktan sonra veritabanını yapılandırmak için bkz: [kurtarma işleminden sonra veritabanını yapılandırma][Configure your database after recovery].
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Azure SQL veritabanı sürümlerini iş sürekliliği özellikleri hakkında bilgi edinmek için lütfen okuyun [Azure SQL Database iş sürekliliğine genel bakış][Azure SQL Database business continuity overview].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Create database restore request]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Database operation status]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Get restorable dropped database]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[List restorable dropped databases]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
