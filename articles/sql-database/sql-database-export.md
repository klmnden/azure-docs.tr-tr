---
title: "Bir Azure SQL veritabanı BACPAC dosyaya | Microsoft Docs"
description: "Azure SQL veritabanını Azure Portalı'nı kullanarak bir BACPAC dosyasına dışarı aktarma"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 41d63a97-37db-4e40-b652-77c2fd1c09b7
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.date: 06/15/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: faa567ec615a07da8633629fc98e3454c84a8f5f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="export-an-azure-sql-database-to-a-bacpac-file"></a>Bir Azure SQL veritabanı bir BACPAC dosyasına dışarı aktarma

Bir veritabanı veya başka bir platform için taşıma arşivleme için dışarı aktarmak gerektiğinde, verileri ve veritabanı şeması verebilirsiniz bir [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) dosya. ZIP dosyası meta verileri ve SQL Server veritabanından veri içeren BACPAC uzantılı bir BACPAC dosyasıdır. Bir BACPAC dosyayı Azure blob depolama veya bir şirket içi konumda yerel depolama depolanır ve daha sonra geri Azure SQL Database veya SQL Server içi yükleme içe. 

> [!IMPORTANT] 
> Azure SQL veritabanı otomatik dışarı aktarma 1 Mart 2017 üzerinde devre dışı bırakılan. Kullanabileceğiniz [uzun vadeli yedekleme bekletme](sql-database-long-term-retention.md
) veya [Azure Otomasyonu](https://github.com/Microsoft/azure-docs-pr/blob/2461f706f8fc1150e69312098640c0676206a531/articles/automation/automation-intro.md) düzenli aralıklarla SQL arşivlemek için veritabanları tercih ettiğiniz bir zamanlamaya göre PowerShell kullanarak. Bir örnek için indirme [PowerShell Betiği örnek](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) github'dan.
>

## <a name="considerations-when-exporting-an-azure-sql-database"></a>Azure SQL veritabanını dışa aktarma ilgili önemli noktalar

* Bir verme işlemsel olarak tutarlı olmasını ya da hiçbir yazma emin olmalısınız etkinlik dışa aktarma sırasında gerçekleşen veya dışarı aktarma bir [işlemsel olarak tutarlı kopyalama](sql-database-copy.md) Azure SQL veritabanınızın.
* Blob depolama alanına veriyorsanız, bir BACPAC dosyasının en büyük boyutu 200 GB'tır. Daha büyük bir BACPAC dosyası arşivlemek için yerel depolama alanına verin.
* Bu makalede bahsedilen yöntemler kullanarak Azure premium storage için bir BACPAC dosyayı dışa desteklenmiyor.
* Azure SQL veritabanından dışa aktarma işlemi 20 saat aşarsa, iptal edilebilir. Dışa aktarma sırasında performansı artırmak için şunları yapabilirsiniz:
  * Geçici olarak hizmet düzeyini artırın.
  * Tüm okuma ve dışarı aktarma sırasında etkinlik yazma kesildi.
  * Kullanım bir [kümelenmiş dizin](https://msdn.microsoft.com/library/ms190457.aspx) tüm büyük tablolarda null olmayan değerler ile. 6-12 saatten daha uzun sürerse, kümelenmiş dizinler bir dışarı aktarma başarısız olabilir. Dışarı aktarma hizmeti tüm tabloyu dışarı aktarmak denemek için bir tablo taraması tamamlaması gereken olmasıdır. Tablolarınızı verme çalıştırmak için en iyileştirilmiş varsa belirlemek için en iyi yolu **DBCC SHOW_STATISTICS** emin olun *RANGE_HI_KEY* null olmayan ve iyi dağıtım onun değerine sahiptir. Ayrıntılar için bkz [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [!NOTE]
> BACPACs için yedekleme ve geri yükleme işlemleri amaçlanmamıştır. Azure SQL veritabanı yedeklemeleri her kullanıcı veritabanı için otomatik olarak oluşturur. Ayrıntılar için bkz [iş Sürekliliğine genel bakış](sql-database-business-continuity.md) ve [SQL veritabanı yedeklemeleri](sql-database-automated-backups.md).  
> 

## <a name="export-to-a-bacpac-file-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir BACPAC dosyasına aktarın

Bir veritabanını kullanarak dışarı aktarmak için [Azure portal](https://portal.azure.com), veritabanınız için sayfasını açın ve'ı tıklatın **verme** araç çubuğunda. BACPAC dosya adını belirtin, Azure depolama hesabı ve kapsayıcı dışa aktarma için sağlayabilir ve kaynak veritabanına bağlanmak için kimlik bilgilerini sağlayın.  

![Veritabanı dışarı aktarma](./media/sql-database-export/database-export.png)

Dışarı aktarma işlemin ilerlemesini izlemek için dışarı aktarılan veritabanını içeren mantıksal sunucu için sayfayı açın. Ekranı aşağı kaydırarak **Operations** ve ardından **içeri/dışarı aktarma** geçmişi.

![dışarı aktarma geçmişi](./media/sql-database-export/export-history.png)
![dışa aktarma geçmişi durumu](./media/sql-database-export/export-history2.png)

## <a name="export-to-a-bacpac-file-using-the-sqlpackage-utility"></a>SQLPackage yardımcı programını kullanarak bir BACPAC dosyasına aktarın

SQL veritabanı kullanarak dışarı aktarmak için [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) komut satırı yardımcı programı, bkz: [verme parametreleri ve özellikleri](https://msdn.microsoft.com/library/hh550080.aspx#Export Parameters and Properties). SQLPackage yardımcı programı en son sürümleri ile birlikte gelen [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) ve [SQL Server veri araçları Visual Studio için](https://msdn.microsoft.com/library/mt204009.aspx), veya en son sürümünü indirebilirsiniz [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) doğrudan Microsoft İndirme Merkezi'nden.

Ölçek ve performans çoğu üretim ortamlarında SQLPackage yardımcı programı kullanılmak öneririz. BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Bu örnek, Active Directory Evrensel kimlik doğrulaması ile SqlPackage.exe kullanarak bir veritabanını dışarı aktarma gösterir:

```cmd
SqlPackage.exe /a:Export /tf:testExport.bacpac /scs:"Data Source=apptestserver.database.windows.net;Initial Catalog=MyDB;" /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="export-to-a-bacpac-file-using-sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS) kullanarak bir BACPAC dosyaya dışarı aktarma

SQL Server Management Studio en yeni sürümleri de Azure SQL veritabanı bir BACPAC dosyasına aktarmak için bir sihirbaz sağlar. Bkz: [bir veri katmanı uygulaması verme](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application).

## <a name="export-to-a-bacpac-file-using-powershell"></a>PowerShell kullanarak bir BACPAC dosyasına aktarın

Kullanım [yeni AzureRmSqlDatabaseExport](/powershell/module/azurerm.sql/new-azurermsqldatabaseexport) cmdlet'ini Azure SQL veritabanı hizmetinin bir verme veritabanı isteği gönderin. Veritabanı boyutuna bağlı olarak, dışarı aktarma işlemi tamamlamak için biraz zaman alabilir.

 ```powershell
 $exportRequest = New-AzureRmSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
   -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
   -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password
 ```

Dışarı aktarma isteğinin durumunu denetlemek için kullanın [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet'i. Bu hemen sonra istek döndürür genellikle çalıştığını **durumu: devam ediyor**. Gördüğünüzde **durumu: başarılı** verme tamamlandı.

```powershell
$exportStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
[Console]::Write("Exporting")
while ($exportStatus.Status -eq "InProgress")
{
    $exportStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$exportStatus
```

## <a name="next-steps"></a>Sonraki adımlar

* Arşiv amaçlar için bir veritabanı için bir alternatif dışarı gibi bir Azure SQL Veritabanı yedeğinin uzun vadeli yedekleme bekletme hakkında bilgi edinmek için [uzun vadeli yedekleme bekletme](sql-database-long-term-retention.md).
- BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* Bir SQL Server veritabanı için bir BACPAC alma hakkında bilgi edinmek için [bir SQL Server veritabanına bir BACPCAC alma](https://msdn.microsoft.com/library/hh710052.aspx).
* Bir SQL Server veritabanından bir BACPAC aktarma hakkında bilgi edinmek için [bir veri katmanı uygulaması verme](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) ve [ilk veritabanınızı geçirin](sql-database-migrate-your-sql-server-database.md).
* SQL Server'dan prelude geçiş olarak Azure SQL veritabanına veriyorsanız, bkz: [bir SQL Server veritabanını Azure SQL veritabanına geçirme](sql-database-cloud-migrate.md).
