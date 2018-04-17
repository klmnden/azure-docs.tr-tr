---
title: Bir Azure SQL veritabanı oluşturmak için bir BACPAC dosyasını içeri | Microsoft Docs
description: Bir BACPAC dosyasını içeri aktararak newAzure SQL veritabanı oluşturun.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: load & move data
ms.date: 04/10/2018
ms.author: carlrab
ms.topic: article
ms.openlocfilehash: 4279630816b6d5f7cf15b7555bf951d3f2a5f95a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="import-a-bacpac-file-to-a-new-azure-sql-database"></a>Yeni bir Azure SQL veritabanı için bir BACPAC dosyasını içeri aktarın

Ne zaman bir veritabanı arşivden içeri aktarmanız gerekir ya da başka bir platformundan geçirirken, verileri ve veritabanı şeması aktarabilirsiniz bir [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) dosya. ZIP dosyası meta verileri ve SQL Server veritabanından veri içeren BACPAC uzantılı bir BACPAC dosyasıdır. Bir BACPAC dosyası (yalnızca standart depolama) Azure blob depolama alanından içeri aktarılabilir veya bir şirket içi konumda yerel depolama. Alma hızı en üst düzeye çıkarmak için daha yüksek Hizmet katmanını ve performans düzeyi, bir P6 gibi belirtin ve ardından içeri aktarma başarılı olduktan sonra uygun şekilde aşağı Ölçekle öneririz. Ayrıca, veritabanı uyumluluk düzeyi içe aktarmadan sonra kaynak veritabanı uyumluluk düzeyini temel alır. 

> [!IMPORTANT] 
> Azure SQL veritabanına veritabanınızı geçirdikten sonra veritabanını geçerli uyumluluk düzeyinde (düzey 100 AdventureWorks2008R2 veritabanı için) veya daha yüksek düzeyde çalışmaya seçebilirsiniz. Veritabanını belirli bir uyumluluk düzeyinde çalıştırmanın etkileri ve buna yönelik seçenekler hakkında daha fazla bilgi için, bkz. [Veritabanı Uyumluluk Düzeyini Değiştirme](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Uyumluluk düzeyleriyle ilgili ek veritabanı düzeyi ayarları hakkında bilgi için, ayrıca bkz. [VERİTABANI KAPSAMLI YAPILANDIRMAYI DEĞİŞTİRME](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql).   >

## <a name="import-from-a-bacpac-file-using-azure-portal"></a>Azure portalını kullanarak bir BACPAC Dosyadan İçeri Aktar

Bu makalede, Azure blob storage kullanarak depolanan bir BACPAC dosyasından bir Azure SQL veritabanı oluşturma için yönergeler sağlanmaktadır [Azure portal](https://portal.azure.com). İçeri aktarma yalnızca Azure portalını kullanarak Azure blob depolama alanından bir BACPAC dosyasını içeri destekler.

Azure Portalı'nı kullanarak bir veritabanını almak için veritabanına ilişkilendirin ve ardından sunucu sayfasını açın **alma** araç çubuğunda. Depolama hesabı ve kapsayıcı belirtin ve içeri aktarmak istediğiniz BACPAC dosyasını seçin. Yeni veritabanı (genellikle aynı kaynak) boyutunu seçin ve hedef SQL Server kimlik bilgilerini sağlayın.  

   ![Veritabanını içeri aktarma](./media/sql-database-import/import.png)

İçeri aktarma işlemin ilerlemesini izlemek için içeri aktarılan veritabanını içeren mantıksal sunucu için sayfayı açın. Ekranı aşağı kaydırarak **Operations** ve ardından **içeri/dışarı aktarma** geçmişi.

> [!NOTE]
> [Yönetilen Azure SQL veritabanı örneği](sql-database-managed-instance.md) bu makalede başka yöntemler kullanarak bir BACPAC dosyasından içe aktarma desteklenir, ancak Azure Portalı'nı kullanarak geçirme şu anda desteklemiyor.

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

## <a name="import-using-other-methods"></a>Başka yöntemler kullanarak içeri aktarma

Bu sihirbazlar de kullanabilirsiniz:

- [Veri katmanı Uygulama Sihirbazı SQL Server Management Studio'da almak](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard).
- [SQL Server içeri ve Dışarı Aktarma Sihirbazı'nı](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard).

## <a name="next-steps"></a>Sonraki adımlar
* Bağlanmak ve içeri aktarılan bir SQL veritabanını sorgulamak öğrenmek için bkz: [SQL Server Management Studio ile SQL veritabanına bağlanma ve örnek T-SQL sorgusu gerçekleştirmeyi](sql-database-connect-query-ssms.md).
* BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* Performans önerileri dahil olmak üzere tüm SQL Server veritabanı geçiş işlemi, bir tartışma için bkz [bir SQL Server veritabanını Azure SQL veritabanına geçirme](sql-database-cloud-migrate.md).



