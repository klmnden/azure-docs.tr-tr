---
title: Azure SQL veri ambarı'nı (Azure portalı) geri yükleme | Microsoft Docs
description: Azure SQL veri ambarını geri yüklemek için azure portalı görevleri.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 2d59235b067d9571bc8b64c33799431be6489502
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421416"
---
# <a name="restore-azure-sql-data-warehouse-portal"></a>Azure SQL veri ambarı'nı (portal) geri yükleme
> [!div class="op_single_selector"]
> * [Genel bakış][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 
> Bu makalede, Azure portalını kullanarak Azure SQL veri ambarı geri öğreneceksiniz.

## <a name="before-you-begin"></a>Başlamadan önce
**DTU kapasitenizi doğrulayın.** SQL veri ambarı'nın her örneği, varsayılan veri aktarım hızı birimi (DTU) kotası olan bir SQL server tarafından (örn. myserver.database.windows.net) barındırılır. SQL veri ambarı geri yüklemeden önce SQL server'ınızı geri veritabanı için yeterli kalan DTU kotası olduğundan emin olun. DTU kotası hesaplamak için ya da daha fazla Dtu istemek için öğrenmek için bkz: [DTU kota değişiklik isteği][Request a DTU quota change].

## <a name="restore-an-active-or-paused-database"></a>Etkin ya da duraklatılmış bir veritabanını geri yükleme
Bir veritabanını geri yüklemek için:

1. [Azure portalında][Azure portal] oturum açın.
2. Sol bölmede seçin **Gözat**ve ardından **SQL sunucuları**.

    ![Gözat'ı seçin > SQL sunucuları](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Sunucunuzu bulun ve seçin.

    ![Sunucunuzu seçin](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)
4. SQL veri ambarı'ndan geri yüklemek istediğiniz örneğini bulun ve seçin.

    ![Geri yüklemek için SQL veri ambarı örneğini seçin](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Veri ambarı dikey pencerenin en üstünde seçin **geri**.

    ![Geri yükleme seçin](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)
6. Yeni bir belirtin **veritabanı adı**.
7. En son seçin **geri yükleme noktası**.

   En son geri yükleme noktasını seçtiğinizden emin olun. Varsayılan seçenek, Eşgüdümlü Evrensel Saat (UTC) geri yükleme noktaları gösterilir çünkü en son geri yükleme noktası olmayabilir.

      ![Bir geri yükleme noktası seçin](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)
8. **Tamam**’ı seçin.
9. Veritabanı geri yükleme işlemi başlar ve kullanabileceğiniz **bildirimleri** işlemini izleyin.

> [!NOTE]
> Geri yükleme tamamlandıktan sonra takip ederek, kurtarılan veritabanı yapılandırabilirsiniz [kurtarma işleminden sonra veritabanını yapılandırma][Configure your database after recovery].
>
>

## <a name="restore-a-deleted-database"></a>Silinen veritabanını geri yükleme
Silinen bir veritabanını geri yüklemek için:

1. [Azure portalında][Azure portal] oturum açın.
2. Sol bölmede seçin **Gözat**ve ardından **SQL sunucuları**.

    ![Gözat'ı seçin > SQL sunucuları](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Sunucunuzu bulun ve seçin.

    ![Sunucunuzu seçin](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)
4. Ekranı aşağı kaydırarak **işlemleri** sunucunuzun dikey penceresinde bölümü.
5. Seçin **veritabanlarını sildi** Döşe.

    ![Silinen veritabanları kutucuk seçin](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)
6. Geri yüklemek istiyorsanız silinen veritabanını seçin.

    ![Geri yüklemek için bir veritabanı seçin](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)
7. Yeni bir belirtin **veritabanı adı**.

    ![Veritabanı için bir ad ekleyin](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
8. **Tamam**’ı seçin.
9. Veritabanı geri yükleme işlemi başlar ve kullanabileceğiniz **bildirimleri** işlemini izleyin.

> [!NOTE]
> Veritabanınızı geri yükleme tamamlandıktan sonra yapılandırmak için bkz [kurtarma işleminden sonra veritabanını yapılandırma][Configure your database after recovery].
>
>

## <a name="next-steps"></a>Sonraki adımlar
Azure SQL veritabanı sürümlerini iş sürekliliği özellikleri hakkında bilgi edinmek için [Azure SQL Database iş sürekliliğine genel bakış][Azure SQL Database business continuity overview].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure portal]: https://portal.azure.com/
