---
title: Yedeklemeyi Azure SQL Veritabanı Yönetilen Örneği’ne geri yükleme| Microsoft Docs
description: SSMS kullanarak veritabanı yedeklemesini Azure SQL Veritabanı Yönetilen Örneğine geri yükleyin.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab, bonova
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: fa9686e7f9ca7f14a51ea2b9c313dd69a2e40cec
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49362263"
---
# <a name="restore-a-database-backup-to-an-azure-sql-database-managed-instance"></a>Veritabanı yedeklemesini Azure SQL Veritabanı Yönetilen Örneğine geri yükleme

Bu hızlı başlangıçta Wide World Importers - Standart yedekleme dosyasını kullanarak Azure blob depolama’da depolanan bir veritabanı yedeklemesinin Yönetilen Örneğe nasıl geri yükleneceği gösterilmektedir. Bu yöntem miktar biraz kesinti süresine neden olur. 

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

Azure Veritabanı Geçiş Hizmeti’ni (DMS) geçiş için kullanmaya ilişkin bir öğretici için bkz. [DMS kullanarak Yönetilen Örnek geçişi](../dms/tutorial-sql-server-to-managed-instance.md). Çeşitli geçiş yöntemleriyle ilgili bir tartışma için bkz. [SQL Server örneğinin Azure SQL Veritabanı Yönetilen Örneği'ne geçişi](sql-database-managed-instance-migrate.md).

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç:
- [Yönetilen Örnek Oluşturma](sql-database-managed-instance-get-started.md) hızlı başlangıcında oluşturulan kaynakları başlangıç noktası olarak kullanır.
- Şirket içi istemci bilgisayarınızda [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms)'nun en yeni sürümünün olmasını gerektirir
- Yönetilen Örneğinize SQL Server Management Studio ile bağlanabilmeyi gerektirir. Bağlantı seçenekleri için şu hızlı başlangıçlara bakın:
  - [Bir Azure VM'den bir Azure SQL Veritabanı Yönetilen Örneğine bağlanma](sql-database-managed-instance-configure-vm.md)
  - [Noktadan siteye bağlantı ile şirket içinden Azure SQL Veritabanı Yönetilen Örneğine bağlanma](sql-database-managed-instance-configure-p2s.md).
- Wide World Importers - Standart yedekleme dosyasını (indirme konumu: https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Standard.bak) içeren önceden yapılandırılmış bir Azure blob depolama hesabı kullanır.

> [!NOTE]
> Bir SQL Server veritabanını Azure blob depolama ve Paylaşılan Erişim İmzası (SAS) kullanarak yedekleme hakkında daha fazla bilgi için bkz. [URL'ye SQL Server Yedekleme](sql-database-managed-instance-get-started-restore.md).

## <a name="restore-the-wide-world-importers-database-from-a-backup-file"></a>Wide World Importers veritabanını bir yedekleme dosyasından geri yükleme

SSMS ile Wide World Importers veritabanını yedekleme dosyasından Yönetilen Örneğinize geri yüklemek için aşağıdaki adımları kullanın.

1. SQL Server Management Studio'yu (SSMS) açın ve Yönetilen Örneğinize bağlanın.
2. SSMS içinde yeni bir sorgu penceresi açın.
3. Yönetilen Örnekte önceden yapılandırılmış depolama hesabını ve SAS anahtarını kullanarak bir kimlik bilgisi oluşturmak için aşağıdaki betiği kullanın.

   ```sql
   CREATE CREDENTIAL [https://mitutorials.blob.core.windows.net/databases] 
   WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
   , SECRET = 'sv=2017-11-09&ss=bfqt&srt=sco&sp=rwdlacup&se=2028-09-06T02:52:55Z&st=2018-09-04T18:52:55Z&spr=https&sig=WOTiM%2FS4GVF%2FEEs9DGQR9Im0W%2BwndxW2CQ7%2B5fHd7Is%3D' 
   ```

    ![kimlik bilgisi oluşturma](./media/sql-database-managed-instance-get-started-restore/credential.png)

    > [!NOTE]
    > Oluşturulan SAS anahtarındaki baştaki **?** işaretini her zaman kaldırın.
  
3. SAS kimlik bilgisini ve yedekleme geçerliliğini denetlemek için, yedekleme dosyasını içeren kapsayıcının URL'sini sağlayarak şu betiği kullanın:

   ```sql
   RESTORE FILELISTONLY FROM URL = 
      'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![dosya listesi](./media/sql-database-managed-instance-get-started-restore/file-list.png)

4. Aşağıdaki betiği kullanarak Wide World Importers veritabanını bir yedekleme dosyasından geri yükleyin; yedekleme dosyasını içeren kapsayıcının URL’sini sağlayın:

   ```sql
   RESTORE DATABASE [Wide World Importers] FROM URL =
     'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![geri yükleme](./media/sql-database-managed-instance-get-started-restore/restore.png)

5. Geri yükleme işleminizin durumunu izlemek için aşağıdaki sorguyu yeni bir sorgu oturumunda çalıştırın:

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time 
   FROM sys.dm_exec_requests r 
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a 
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')`
   ```

6. Geri yükleme tamamlandığında Nesne Gezgini içinde görüntüleyin. 

## <a name="next-steps"></a>Sonraki adımlar

- URL'ye yedekleme sorunlarını giderme için bkz: [URL'ye SQL Server Yedekleme En İyi Yöntemleri ve Sorun Giderme](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting).
- Uygulamaların bağlantı seçeneklerine genel bir bakış için bkz: [Uygulamalarınızı Yönetilen Örneğe bağlama](sql-database-managed-instance-connect-app.md).
- Tercih ettiğiniz araçlardan veya dillerden birini kullanarak sorgulama için bkz: [bağlanma ve sorgulama](sql-database-connect-query.md).
