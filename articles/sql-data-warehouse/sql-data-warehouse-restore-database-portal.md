---
title: "Azure SQL Data Warehouse'a (Azure portalı) geri | Microsoft Docs"
description: "Azure SQL Data Warehouse geri yüklemek için azure portal görevler."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: barbkess
editor: 
ms.assetid: b0aef539-7657-4b0e-9899-74098f5c21bc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 09/21/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: f6bc8671410dc7015a8d2a4bea1ba11f9ae526c3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="restore-azure-sql-data-warehouse-portal"></a>Azure SQL Data Warehouse (portal) geri yükleme
> [!div class="op_single_selector"]
> * [Genel bakış][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
>
>
Bu makalede, Azure portalını kullanarak Azure SQL Data Warehouse geri öğreneceksiniz.

## <a name="before-you-begin"></a>Başlamadan önce
**DTU kapasitenizi doğrulayın.** SQL veri ambarı her bir örneğini varsayılan veri işleme birimi (DTU) kota olan bir SQL server tarafından (örneğin, myserver.database.windows.net) barındırılıyor. SQL veri ambarı geri yüklemeden önce SQL server'ınızdaki geri yüklemekte veritabanı için yeterli kalan DTU kota sahip olduğunu doğrulayın. DTU kota hesaplamak için ya da daha fazla Dtu'lar istemek için öğrenmek için bkz: [DTU kota değişiklik isteği][Request a DTU quota change].

## <a name="restore-an-active-or-paused-database"></a>Etkin ya da duraklatılmış bir veritabanını geri yükle
Bir veritabanını geri yüklemek için:

1. [Azure portalında][Azure portal] oturum açın.
2. Sol bölmede seçin **Gözat**ve ardından **SQL sunucuları**.

    ![Gözat seçin > SQL Server'lar](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Sunucunuz bulun ve seçin.

    ![Sunucunuzu seçin](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)
4. Geri yüklemek istediğiniz bir SQL Data Warehouse örneğini bulun ve seçin.

    ![Geri yüklemek için SQL veri ambarı örneği seçin](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Veri ambarı dikey pencerenin en üstünde seçin **geri**.

    ![Geri yükleme seçin](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)
6. Yeni bir belirtin **veritabanı adı**.
7. En son seçin **geri yükleme noktası**.

   En son geri yükleme noktası seçtiğinizden emin olun. Geri yükleme noktaları Eşgüdümlü Evrensel Saat (UTC) gösterilir çünkü varsayılan seçeneği en son geri yükleme noktası olmayabilir.

      ![Bir geri yükleme noktası seçin](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)
8. **Tamam**’ı seçin.
9. Veritabanı geri yükleme işlemi başlar ve kullanabileceğiniz **bildirimleri** işlemi izlemek üzere.

> [!NOTE]
> Geri yükleme tamamlandıktan sonra kurtarılan veritabanının izleyerek yapılandırabileceğiniz [kurtarma işleminden sonra veritabanını yapılandırma][Configure your database after recovery].
>
>

## <a name="restore-a-deleted-database"></a>Silinen veritabanını geri yükleme
Silinen bir veritabanını geri yüklemek için:

1. [Azure portalında][Azure portal] oturum açın.
2. Sol bölmede seçin **Gözat**ve ardından **SQL sunucuları**.

    ![Gözat seçin > SQL Server'lar](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Sunucunuz bulun ve seçin.

    ![Sunucunuzu seçin](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)
4. Ekranı aşağı kaydırarak **Operations** sunucunuzun dikey bölüm.
5. Seçin **veritabanlarını sildi** döşeme.

    ![Silinen veritabanları kutucuk seçin](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)
6. Geri yüklemek istediğiniz silinen bir veritabanını seçin.

    ![Geri yüklemek için bir veritabanı seçin](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)
7. Yeni bir belirtin **veritabanı adı**.

    ![Veritabanı için bir ad ekleyin](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
8. **Tamam**’ı seçin.
9. Veritabanı geri yükleme işlemi başlar ve kullanabileceğiniz **bildirimleri** işlemi izlemek üzere.

> [!NOTE]
> Geri yükleme tamamlandıktan sonra veritabanını yapılandırmak için bkz: [kurtarma işleminden sonra veritabanını yapılandırma][Configure your database after recovery].
>
>

## <a name="next-steps"></a>Sonraki adımlar
Azure SQL veritabanı sürümlerini iş sürekliliği özellikleri hakkında bilgi edinmek için okuma [Azure SQL Database iş sürekliliğine genel bakış][Azure SQL Database business continuity overview].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure portal]: https://portal.azure.com/
