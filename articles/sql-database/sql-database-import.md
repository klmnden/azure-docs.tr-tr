---
title: Bir Azure SQL veritabanı oluşturmak için bir BACPAC dosyasını içeri aktarma | Microsoft Docs
description: Bir BACPAC dosyasını içeri aktararak newAzure SQL veritabanı oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 98b316f8a9c1c8ceba91870af4ff67b1aa854a9b
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65785331"
---
# <a name="quickstart-import-a-bacpac-file-to-a-database-in-azure-sql-database"></a>Hızlı Başlangıç: BACPAC dosyasını Azure SQL veritabanında bir veritabanı için içeri aktarma

Bir SQL Server veritabanını Azure SQL veritabanı'nı kullanarak bir veritabanına alabileceğiniz bir [BACPAC](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications#bacpac) dosya. Verileri içeri aktarabilirsiniz bir `BACPAC` (yalnızca standart depolama) Azure Blob depolamada depolanan dosya veya bir şirket içi konuma yerel depodan. Alma hızı daha hızlı ve daha fazla kaynak sağlayarak en üst düzeye çıkarmak, veritabanınız daha yüksek bir hizmet katmanına ve içeri aktarma işlemi sırasında boyutu bilgi işlem. İçeri aktarma başarılı olduktan sonra ölçeği azaltabilirsiniz.

> [!NOTE]
> İçeri aktarılan veritabanının uyumluluk düzeyi, kaynak veritabanının uyumluluk düzeyi üzerinde temel alır.
> [!IMPORTANT]
> Veritabanınızı içeri aktardıktan sonra veritabanını, geçerli uyumluluk düzeyinde (düzey 100 AdventureWorks2008R2 veritabanı için) veya daha yüksek bir düzeyde çalışılacak seçebilirsiniz. Veritabanını belirli bir uyumluluk düzeyinde çalıştırmanın etkileri ve buna yönelik seçenekler hakkında daha fazla bilgi için, bkz. [Veritabanı Uyumluluk Düzeyini Değiştirme](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Uyumluluk düzeyleriyle ilgili ek veritabanı düzeyi ayarları hakkında bilgi için, ayrıca bkz. [VERİTABANI KAPSAMLI YAPILANDIRMAYI DEĞİŞTİRME](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql).

## <a name="import-from-a-bacpac-file-in-the-azure-portal"></a>Azure portalında bir BACPAC dosyasından alma

[Azure portalında](https://portal.azure.com) *yalnızca* destekleyen Azure SQL veritabanı'nda tek bir veritabanı oluşturma ve *yalnızca* bir BACPAC dosyasından Azure Blob Depolama alanında depolanır.

> [!NOTE]
> [Yönetilen örnek](sql-database-managed-instance.md) bir veritabanını Azure portalını kullanarak BACPAC dosyasından bir örneği veritabanına geçirme şu anda desteklemiyor. Yönetilen örneğine almak için SQL Server Management Studio veya SQLPackage kullanın.

1. Azure portalını kullanarak yeni tek veritabanına BACPAC dosyasından içeri aktarmak için uygun veritabanı sunucu sayfasına açın ve ardından, araç çubuğunda **veritabanını içeri aktar**.  

   ![Veritabanı import1](./media/sql-database-import/import1.png)

2. Depolama hesabı ve kapsayıcı BACPAC dosyası seçin ve ardından alınacağı BACPAC dosyasını seçin.
3. Yeni veritabanı boyutu (genellikle kaynak aynı) belirtin ve hedef SQL Server kimlik bilgilerini sağlayın. Yeni bir Azure SQL veritabanı için olası değerler listesi için bkz. [Create Database](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current).

   ![Veritabanı import2](./media/sql-database-import/import2.png)

4. **Tamam** düğmesine tıklayın.

5. Veritabanı sunucusu sayfasında, bir içeri aktarmanın ilerleme durumunu izlemek için ve altında **ayarları**seçin **içeri/dışarı aktarma geçmişi**. Başarılı olduğunda, içeri aktarma sahip bir **tamamlandı** durumu.

   ![Veritabanı içeri aktarma durumu](./media/sql-database-import/import-status.png)

6. Veritabanı veritabanı sunucusunda Canlı doğrulamak için **SQL veritabanları** ve yeni veritabanı doğrulayın **çevrimiçi**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>SqlPackage kullanarak BACPAC dosyasından alma

SQL Server veritabanını kullanarak içeri aktarmak için [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) komut satırı yardımcı programını bkz [parametreleri ve özellikleri Al](https://docs.microsoft.com/sql/tools/sqlpackage#import-parameters-and-properties). SqlPackage sahip en son [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve [Visual Studio için SQL Server veri Araçları](https://msdn.microsoft.com/library/mt204009.aspx). En son da indirebilirsiniz [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) Microsoft İndirme Merkezi'nden.

Ölçek ve performans için Azure portalını kullanarak yerine çoğu üretim ortamlarında SqlPackage kullanarak öneririz. Kullanarak geçirme hakkında bir SQL Server Müşteri danışmanlık ekibi blogu için `BACPAC` dosyaları görmek [SQL Server'dan Azure SQL veritabanına BACPAC dosyalarını kullanarak geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Ölçek ve performans için çoğu üretim ortamlarında SqlPackage kullanmanızı öneririz. BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/20../../migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Aşağıdaki komut SqlPackage alır **AdventureWorks2008R2** adlı bir Azure SQL veritabanı sunucusu yerel depodan veritabanına **mynewserver20170403**. Adlı yeni bir veritabanı oluşturur **myMigratedDatabase** ile bir **Premium** hizmet katmanı ve bir **P6** hizmet hedefi. Bu değerleri ortamınız için uygun şekilde değiştirin.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=<your_server_admin_account_user_id>;Password=<your_server_admin_account_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Kurumsal bir güvenlik duvarının korumasında tek bir veritabanını yönetmek SQL veritabanı sunucusuna bağlanmak için güvenlik duvarı bağlantı noktası 1433'ü açık olması gerekir. Yönetilen bir örneğine bağlanmak için olmalıdır bir [noktadan siteye bağlantı](sql-database-managed-instance-configure-p2s.md) veya express route bağlantı.
>

Bu örnek, bir veritabanını Active Directory Evrensel kimlik doğrulaması ile SqlPackage kullanarak içeri aktarma gösterir.

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-into-a-single-database-from-a-bacpac-file-using-powershell"></a>Tek bir veritabanını PowerShell kullanarak BACPAC dosyasından alma

> [!NOTE]
> [Yönetilen örnek](sql-database-managed-instance.md) bir örneği veritabanına Azure PowerShell kullanarak BACPAC dosyasından bir veritabanı geçişi şu anda desteklemiyor. Yönetilen örneğine almak için SQL Server Management Studio veya SQLPackage kullanın.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

Kullanım [yeni AzSqlDatabaseImport](/powershell/module/az.sql/new-azsqldatabaseimport) cmdlet'i bir Azure SQL veritabanı hizmetini alma veritabanı isteğini göndermek için. Veritabanı boyutuna bağlı olarak içeri aktarma tamamlanması biraz zaman alabilir.

 ```powershell
 $importRequest = New-AzSqlDatabaseImport 
    -ResourceGroupName "<your_resource_group>" `
    -ServerName "<your_server>" `
    -DatabaseName "<your_database>" `
    -DatabaseMaxSizeBytes "<database_size_in_bytes>" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzStorageAccountKey -ResourceGroupName "<your_resource_group>" -StorageAccountName "<your_storage_account").Value[0] `
    -StorageUri "https://myStorageAccount.blob.core.windows.net/importsample/sample.bacpac" `
    -Edition "Standard" `
    -ServiceObjectiveName "P6" `
    -AdministratorLogin "<your_server_admin_account_user_id>" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String "<your_server_admin_account_password>" -AsPlainText -Force)

 ```

 Kullanabileceğiniz [Get-AzSqlDatabaseImportExportStatus](/powershell/module/az.sql/get-azsqldatabaseimportexportstatus) alma işleminin ilerleme durumunu denetlemek için cmdlet'i. Cmdlet hemen sonra istek genellikle döndürür çalıştıran **durumu: Inprogress**. Gördüğünüzde, içeri aktarma tamamlandıktan **durumu: Başarılı**.

```powershell
$importStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
[Console]::Write("Importing")
while ($importStatus.Status -eq "InProgress")
{
    $importStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$importStatus
```

> [!TIP]
> Başka bir komut dosyası örneği için bkz. [veritabanını BACPAC dosyasından içeri aktarma](scripts/sql-database-import-from-bacpac-powershell.md).

## <a name="limitations"></a>Sınırlamalar

Elastik havuzdaki bir veritabanı için içeri aktarma desteklenmiyor. Bir tek veritabanı'na veri aktarma ve ardından veritabanını bir elastik havuz için taşıma.

## <a name="import-using-wizards"></a>İçeri aktarma sihirbazları

Bu sihirbazlar de kullanabilirsiniz.

- [SQL Server Management Studio'daki veri katmanı Uygulama Sihirbazı Alma](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard).
- [SQL Server içeri ve Dışarı Aktarma Sihirbazı'nı](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard).

## <a name="next-steps"></a>Sonraki adımlar

- Bağlanmak ve içeri aktarılan bir SQL veritabanını sorgulama hakkında bilgi edinmek için [hızlı başlangıç: Azure SQL Veritabanı: Verileri bağlama ve sorgulama için SQL Server Management Studio kullanın](sql-database-connect-query-ssms.md).
- BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://techcommunity.microsoft.com/t5/DataCAT/Migrating-from-SQL-Server-to-Azure-SQL-Database-using-Bacpac/ba-p/305407).
- Performans önerileri de dahil olmak üzere tüm SQL Server veritabanı geçiş işlemi, hakkında ayrıntılı bilgi için bkz. [Azure SQL veritabanı için SQL Server veritabanı geçişi](sql-database-single-database-migrate.md).
- Depolama anahtarları ve paylaşılan erişim imzaları güvenli bir şekilde, bkz: yönetmek ve paylaşmak hakkında bilgi edinmek için [Azure depolama Güvenlik Kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-security-guide).
