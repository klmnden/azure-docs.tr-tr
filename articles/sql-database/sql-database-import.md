---
title: "Bir Azure SQL veritabanı oluşturmak için bir BACPAC dosyasını içeri | Microsoft Docs"
description: "Bir BACPAC dosyasını içeri aktararak newAzure SQL veritabanı oluşturun."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: cf9a9631-56aa-4985-a565-1cacc297871d
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.date: 06/26/2017
ms.author: carlrab
ms.workload: Active
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 34dee9511822acec46ba4854729939b84f3c06c6
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="import-a-bacpac-file-to-a-new-azure-sql-database"></a>Yeni bir Azure SQL veritabanı için bir BACPAC dosyasını içeri aktarın

Ne zaman bir veritabanı arşivden içeri aktarmanız gerekir ya da başka bir platformundan geçirirken, verileri ve veritabanı şeması aktarabilirsiniz bir [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) dosya. ZIP dosyası meta verileri ve SQL Server veritabanından veri içeren BACPAC uzantılı bir BACPAC dosyasıdır. Bir BACPAC dosyası (yalnızca standart depolama) Azure blob depolama alanından içeri aktarılabilir veya bir şirket içi konumda yerel depolama. Alma hızı en üst düzeye çıkarmak için daha yüksek Hizmet katmanını ve performans düzeyi, bir P6 gibi belirtin ve ardından içeri aktarma başarılı olduktan sonra uygun şekilde aşağı Ölçekle öneririz. Ayrıca, veritabanı uyumluluk düzeyi içe aktarmadan sonra kaynak veritabanı uyumluluk düzeyini temel alır. 

> [!IMPORTANT] 
> Azure SQL veritabanına veritabanınızı geçirdikten sonra veritabanını geçerli uyumluluk düzeyinde (düzey 100 AdventureWorks2008R2 veritabanı için) veya daha yüksek düzeyde çalışmaya seçebilirsiniz. Uygulamaları ve belirli bir uyumluluk düzeyinde bir veritabanı işletim seçenekleri hakkında daha fazla bilgi için bkz: [ALTER veritabanı uyumluluk düzeyi](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Ayrıca bkz. [ALTER veritabanı kapsamlı yapılandırma](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) Uyumluluk Düzeyleri ilgili ek veritabanı düzeyi ayarları hakkında bilgi için.   >

> [!NOTE]
> Yeni bir veritabanı için bir BACPAC almak için ilk Azure SQL Database mantıksal sunucusu oluşturmanız gerekir. SQL Server veritabanını Azure SQL veritabanına SQLPackage kullanarak geçirmek nasıl gösteren bir öğretici için bkz: [SQL Server veritabanına geçirme](sql-database-migrate-your-sql-server-database.md)
>

## <a name="import-from-a-bacpac-file-using-azure-portal"></a>Azure portalını kullanarak bir BACPAC Dosyadan İçeri Aktar

Bu makalede, Azure blob storage kullanarak depolanan bir BACPAC dosyasından bir Azure SQL veritabanı oluşturma için yönergeler sağlanmaktadır [Azure portal](https://portal.azure.com). İçeri aktarma yalnızca Azure portalını kullanarak Azure blob depolama alanından bir BACPAC dosyasını içeri destekler.

Azure Portalı'nı kullanarak bir veritabanını almak için veritabanına ilişkilendirin ve ardından sunucu sayfasını açın **alma** araç çubuğunda. Depolama hesabı ve kapsayıcı belirtin ve içeri aktarmak istediğiniz BACPAC dosyasını seçin. Yeni veritabanı (genellikle aynı kaynak) boyutunu seçin ve hedef SQL Server kimlik bilgilerini sağlayın.  

   ![Veritabanını içeri aktarma](./media/sql-database-import/import.png)

İçeri aktarma işlemin ilerlemesini izlemek için içeri aktarılan veritabanını içeren mantıksal sunucu için sayfayı açın. Ekranı aşağı kaydırarak **Operations** ve ardından **içeri/dışarı aktarma** geçmişi.

### <a name="monitor-the-progress-of-an-import-operation"></a>İçeri aktarma işleminin ilerleyişini izleyin

İçe aktarma işleminin ilerlemesini izlemek için alınan veritabanı alınmakta mantıksal sunucu için sayfayı açın. Ekranı aşağı kaydırarak **Operations** ve ardından **içeri/dışarı aktarma** geçmişi.
   
   ![Alma](./media/sql-database-import/import-history.png) ![alma durumu](./media/sql-database-import/import-status.png)

Veritabanı sunucusunda Canlı doğrulamak için tıklatın **SQL veritabanları** ve yeni veritabanı doğrulamanız **çevrimiçi**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>SQLPackage kullanarak bir BACPAC Dosyadan İçeri Aktar

SQL veritabanı kullanarak içe aktarmak için [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) komut satırı yardımcı programı, bkz: [alma parametreleri ve özellikleri](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties). SQLPackage yardımcı programı en son sürümleri ile birlikte gelen [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) ve [SQL Server veri araçları Visual Studio için](https://msdn.microsoft.com/library/mt204009.aspx), veya en son sürümünü indirebilirsiniz [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) doğrudan Microsoft İndirme Merkezi'nden.

Ölçek ve performans çoğu üretim ortamlarında SQLPackage yardımcı programı kullanılmak öneririz. BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Aşağıdaki içeri aktarma SQLPackage komutu bir komut dosyası örneği için bkz: **AdventureWorks2008R2** adlı bir Azure SQL Database mantıksal sunucusu yerel depolama biriminden veritabanına **mynewserver20170403** Bu örnekte. Bu komut dosyası olarak adlandırılan yeni bir veritabanı oluşturulmasını gösterir **myMigratedDatabase**, bir hizmet katmanını ile **Premium**ve bir hizmet amacını **P6**. Bu değerleri ortamınıza uygun şekilde değiştirin.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Azure SQL Veritabanı mantıksal sunucusu 1433 numaralı bağlantı noktasında dinler. Azure SQL Veritabanı mantıksal sunucusuna bir şirket ağından bağlanmaya çalışıyorsanız, bu bağlantı noktası başarıyla bağlanmanız için şirket güvenlik ağında açık olmalıdır.
>

Bu örnek, Active Directory Evrensel kimlik doğrulaması ile SqlPackage.exe kullanarak bir veritabanını içeri aktarma gösterir:

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a>PowerShell kullanarak bir BACPAC Dosyadan İçeri Aktar

Kullanım [yeni AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) cmdlet'ini Azure SQL veritabanı hizmetinin bir alma veritabanı isteği gönderin. Veritabanı boyutuna bağlı olarak, içeri aktarma işlemini tamamlamak için biraz zaman alabilir.

 ```powershell
 $importRequest = New-AzureRmSqlDatabaseImport -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName "MyImportSample" `
    -DatabaseMaxSizeBytes "262144000" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzureRmStorageAccountKey -ResourceGroupName "myResourceGroup" -StorageAccountName $storageaccountname).Value[0] `
    -StorageUri "http://$storageaccountname.blob.core.windows.net/importsample/sample.bacpac" `
    -Edition "Standard" `
    -ServiceObjectiveName "P6" `
    -AdministratorLogin "ServerAdmin" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String "ASecureP@assw0rd" -AsPlainText -Force)

 ```

İçeri aktarma isteği durumunu denetlemek için kullanmak [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet'i. Bu hemen sonra istek döndürür genellikle çalıştığını **durumu: devam ediyor**. Gördüğünüzde **durumu: başarılı** alma işlemi tamamlandı.

```powershell
$importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
[Console]::Write("Importing")
while ($importStatus.Status -eq "InProgress")
{
    $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$importStatus
```

> [!TIP]
Başka bir komut dosyası örneği için bkz: [bir BACPAC dosyadan bir veritabanı](scripts/sql-database-import-from-bacpac-powershell.md).

## <a name="next-steps"></a>Sonraki adımlar
* Bağlanmak ve içeri aktarılan bir SQL veritabanını sorgulamak öğrenmek için bkz: [SQL Server Management Studio ile SQL veritabanına bağlanma ve örnek T-SQL sorgusu gerçekleştirmeyi](sql-database-connect-query-ssms.md).
* BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* Performans önerileri dahil olmak üzere tüm SQL Server veritabanı geçiş işlemi, bir tartışma için bkz [bir SQL Server veritabanını Azure SQL veritabanına geçirme](sql-database-cloud-migrate.md).



