---
title: Bir Azure SQL veritabanı oluşturmak için bir BACPAC dosyasını içeri aktarma | Microsoft Docs
description: Bir BACPAC dosyasını içeri aktararak newAzure SQL veritabanı oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: douglaslMS
ms.author: douglasl
ms.reviewer: carlrab
manager: craigg
ms.date: 12/05/2018
ms.openlocfilehash: 9e79aa2315118bcd9ce4328e74d51d7a22ea6247
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53744573"
---
# <a name="quickstart-import-a-bacpac-file-to-a-new-azure-sql-database"></a>Hızlı Başlangıç: Yeni bir Azure SQL veritabanına BACPAC dosyasını içeri aktarma

Bir SQL Server veritabanını kullanarak bir Azure SQL veritabanına geçirebilirsiniz bir [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) dosyası (zip dosyasıyla bir `.bacpac` bir veritabanının meta verilere ve verilere sahip uzantısı). Azure blob depolama alanından (yalnızca standart depolama) bir BACPAC dosyasını içeri aktarabilirsiniz veya yerel depoda bir şirket içi konum. İçeri aktarma hızını en üst düzeye çıkarmak için daha yüksek bir hizmet katmanına belirtin ve boyutu (P6 gibi) işlem kullanabilirsiniz. İçeri aktarma başarılı olduktan sonra ölçeği azaltabilirsiniz. İçeri aktarılan veritabanının uyumluluk düzeyi, kaynak veritabanının uyumluluk düzeyi üzerinde temel alır.

> [!IMPORTANT]
> Veritabanınızı içeri aktardıktan sonra veritabanını, geçerli uyumluluk düzeyinde (düzey 100 AdventureWorks2008R2 veritabanı için) veya daha yüksek bir düzeyde çalışılacak seçebilirsiniz. Veritabanını belirli bir uyumluluk düzeyinde çalıştırmanın etkileri ve buna yönelik seçenekler hakkında daha fazla bilgi için, bkz. [Veritabanı Uyumluluk Düzeyini Değiştirme](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Uyumluluk düzeyleriyle ilgili ek veritabanı düzeyi ayarları hakkında bilgi için, ayrıca bkz. [VERİTABANI KAPSAMLI YAPILANDIRMAYI DEĞİŞTİRME](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql).

## <a name="import-from-a-bacpac-file-in-the-azure-portal"></a>Azure portalında bir BACPAC dosyasından alma

Bu bölümde gösterilmiştir nasıl, [Azure portalında](https://portal.azure.com), Azure blob depolamada depolanan bir BACPAC dosyasından Azure SQL veritabanı oluşturmak için. Portal *yalnızca* blob depolama Azure'dan bir BACPAC dosyasını içeri destekler.

> [!NOTE]
> [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md) bu makaledeki diğer yöntemleri kullanarak BACPAC dosyasından alma destekler, ancak şu anda Azure portalında geçişini desteklemez.

Azure portalında bir veritabanını içeri aktarmak için içeri aktarma barındırmak ve, araç çubuğunda mantıksal sunucu için sayfayı açın **veritabanını içeri aktar**.  

   ![Veritabanı içeri aktarma](./media/sql-database-import/import.png)

Depolama hesabı, kapsayıcı ve BACPAC dosyasını içeri aktarmak istediğiniz seçin. Yeni veritabanı boyutu (genellikle kaynak aynı) belirtin ve hedef SQL Server kimlik bilgilerini sağlayın. 

### <a name="monitor-imports-progress"></a>Alma işleminin ilerleme durumunu izleyin

İçeri aktarılan veritabanı mantıksal sunucusu sayfasında, bir içeri aktarmanın ilerleme durumunu izlemek için ve altında **ayarları**seçin **içeri/dışarı aktarma geçmişi**. Başarılı olduğunda, içeri aktarma sahip bir **tamamlandı** durumu.

Veritabanı sunucusunda Canlı doğrulamak için **SQL veritabanları** ve yeni veritabanı doğrulayın **çevrimiçi**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>SqlPackage kullanarak BACPAC dosyasından alma

Bir SQL veritabanını kullanarak içeri aktarmak için [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) komut satırı yardımcı programını bkz [parametreleri ve özellikleri Al](https://docs.microsoft.com/sql/tools/sqlpackage#import-parameters-and-properties). SqlPackage sahip en son [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) ve [Visual Studio için SQL Server veri Araçları](https://msdn.microsoft.com/library/mt204009.aspx). En son da indirebilirsiniz [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) Microsoft İndirme Merkezi'nden.

Ölçek ve performans için çoğu üretim ortamlarında SqlPackage kullanmanızı öneririz. BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Aşağıdaki komut SqlPackage alır **AdventureWorks2008R2** yerel depolama veritabanından adlı bir Azure SQL veritabanı mantıksal sunucusuna **mynewserver20170403**. Adlı yeni bir veritabanı oluşturur **myMigratedDatabase** ile bir **Premium** hizmet katmanı ve bir **P6** hizmet hedefi. Bu değerleri ortamınız için uygun şekilde değiştirin.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=<your_server_admin_account_user_id>;Password=<your_server_admin_account_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Azure SQL Veritabanı mantıksal sunucusu 1433 numaralı bağlantı noktasında dinler. Kurumsal bir güvenlik duvarının korumasında mantıksal sunucusuna bağlanmak için güvenlik duvarının Bu bağlantı noktası açık olması gerekir.
>

Bu örnek, bir veritabanını Active Directory Evrensel kimlik doğrulaması ile SqlPackage kullanarak içeri aktarma gösterir.

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a>PowerShell kullanarak BACPAC dosyasından alma

Kullanım [New-Azurermsqldatabaseımport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) cmdlet'i bir Azure SQL veritabanı hizmetini alma veritabanı isteğini göndermek için. Veritabanı boyutuna bağlı olarak içeri aktarma tamamlanması biraz zaman alabilir.

 ```powershell
 $importRequest = New-AzureRmSqlDatabaseImport 
    -ResourceGroupName "<your_resource_group>" `
    -ServerName "<your_server>" `
    -DatabaseName "<your_database>" `
    -DatabaseMaxSizeBytes "<database_size_in_bytes>" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzureRmStorageAccountKey -ResourceGroupName "<your_resource_group>" -StorageAccountName "<your_storage_account").Value[0] `
    -StorageUri "https://myStorageAccount.blob.core.windows.net/importsample/sample.bacpac" `
    -Edition "Standard" `
    -ServiceObjectiveName "P6" `
    -AdministratorLogin "<your_server_admin_account_user_id>" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String "<your_server_admin_account_password>" -AsPlainText -Force)

 ```

 Kullanabileceğiniz [Get-Azurermsqldatabaseımportexportstatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) alma işleminin ilerleme durumunu denetlemek için cmdlet'i. Cmdlet hemen sonra istek genellikle döndürür çalıştıran **durumu: Inprogress**. Gördüğünüzde, içeri aktarma tamamlandıktan **durumu: Başarılı**.

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
Başka bir komut dosyası örneği için bkz. [veritabanını BACPAC dosyasından içeri aktarma](scripts/sql-database-import-from-bacpac-powershell.md).

## <a name="limitations"></a>Sınırlamalar

Elastik havuzdaki bir veritabanı için içeri aktarma desteklenmiyor. Verileri tek bir veritabanına içeri aktarabilir ve sonra veritabanını havuza taşıma.

## <a name="import-using-wizards"></a>İçeri aktarma sihirbazları

Bu sihirbazlar de kullanabilirsiniz.

- [SQL Server Management Studio'daki veri katmanı Uygulama Sihirbazı Alma](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard).
- [SQL Server içeri ve Dışarı Aktarma Sihirbazı'nı](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard).

## <a name="next-steps"></a>Sonraki adımlar

- Bağlanmak ve içeri aktarılan bir SQL veritabanını sorgulama hakkında bilgi edinmek için [hızlı başlangıç: Azure SQL veritabanı: Verileri bağlama ve sorgulama için SQL Server Management Studio kullanın](sql-database-connect-query-ssms.md).
- BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
- Performans önerileri de dahil olmak üzere tüm SQL Server veritabanı geçiş işlemi, hakkında ayrıntılı bilgi için bkz. [Azure SQL veritabanı için SQL Server veritabanı geçişi](sql-database-cloud-migrate.md).
- Depolama anahtarları ve paylaşılan erişim imzaları güvenli bir şekilde, bkz: yönetmek ve paylaşmak hakkında bilgi edinmek için [Azure depolama Güvenlik Kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-security-guide).
