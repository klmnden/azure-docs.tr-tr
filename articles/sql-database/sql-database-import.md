---
title: Bir Azure SQL veritabanı oluşturmak için bir BACPAC dosyasını içeri aktarma | Microsoft Docs
description: Bir BACPAC dosyasını içeri aktararak newAzure SQL veritabanı oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: 9de7fe9972f1ae0fca1c4e527f718b31fddf4294
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161548"
---
# <a name="import-a-bacpac-file-to-a-new-azure-sql-database"></a>Yeni bir Azure SQL veritabanına BACPAC dosyasını içeri aktarma

Ne zaman bir veritabanı arşivden almanız veya başka bir platformdan diğerine geçirirken, veritabanı şemasını ve verileri içeri aktarabilirsiniz bir [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) dosya. Bir BACPAC dosyasına BACPAC SQL Server veritabanındaki verileri ve meta verileri içeren bir uzantıya sahip bir ZIP dosyasıdır. Bir BACPAC dosyası (yalnızca standart depolama) Azure blob depolama alanından içeri aktarılabilir veya yerel depoda bir şirket içi konum. Alma hızını en üst düzeye çıkarmak için daha yüksek bir hizmet katmanına belirtin ve boyutu, bir P6 gibi işlem ve sonra içeri aktarma başarılı olduktan sonra uygun şekilde aşağı ölçeklendirin öneririz. Ayrıca, içeri aktarma işleminden veritabanı uyumluluk düzeyi, kaynak veritabanının uyumluluk düzeyini temel alır. 

> [!IMPORTANT] 
> Veritabanınızı Azure SQL veritabanı'na geçirdikten sonra veritabanını, geçerli uyumluluk düzeyinde (düzey 100 AdventureWorks2008R2 veritabanı için) veya daha yüksek bir düzeyde çalışılacak seçebilirsiniz. Veritabanını belirli bir uyumluluk düzeyinde çalıştırmanın etkileri ve buna yönelik seçenekler hakkında daha fazla bilgi için, bkz. [Veritabanı Uyumluluk Düzeyini Değiştirme](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Uyumluluk düzeyleriyle ilgili ek veritabanı düzeyi ayarları hakkında bilgi için, ayrıca bkz. [VERİTABANI KAPSAMLI YAPILANDIRMAYI DEĞİŞTİRME](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql).   >

## <a name="import-from-a-bacpac-file-using-azure-portal"></a>Azure portalını kullanarak BACPAC dosyasından alma

Bu makalede, Azure blob depolama kullanarak depolanan bir BACPAC dosyasından Azure SQL veritabanı oluşturma için yönergeler sağlanır. [Azure portalında](https://portal.azure.com). İçeri aktarma yalnızca Azure portalını kullanarak BACPAC dosyasını Azure blob depolama alanından alma destekler.

Azure portalını kullanarak bir veritabanını içeri aktarmak için veritabanı ilişkilendirin ve ardından sunucunun (sayfada değil veritabanı için) sayfasını açın **alma** araç. Depolama hesabı ve kapsayıcı belirtin ve içeri aktarmak istediğiniz BACPAC dosyasını seçin. Yeni veritabanı (genellikle kaynak aynı) boyutunu seçin ve hedef SQL Server kimlik bilgilerini sağlayın.  

   ![Veritabanı içeri aktarma](./media/sql-database-import/import.png)

İçeri aktarma işleminin ilerleme durumunu izlemek için içeri aktarılan veritabanını içeren bir mantıksal sunucu için sayfayı açın. Ekranı aşağı kaydırarak **işlemleri** ve ardından **içeri/dışarı aktarma** geçmişi.

> [!NOTE]
> [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md) bu makaledeki diğer yöntemleri kullanarak BACPAC dosyasından alma desteklenir, ancak şu anda Azure portalını kullanarak geçiş desteklemiyor.

### <a name="monitor-the-progress-of-an-import-operation"></a>Bir içeri aktarma işleminin ilerleme durumunu izleyin

İçeri aktarma işleminin ilerleme durumunu izlemek için içeri aktarılan veritabanını alınmakta mantıksal sunucu için sayfayı açın. Ekranı aşağı kaydırarak **işlemleri** ve ardından **içeri/dışarı aktarma** geçmişi.
   
   ![içeri aktarma](./media/sql-database-import/import-history.png) ![içeri aktarma durumu](./media/sql-database-import/import-status.png)

Veritabanı sunucusunda Canlı doğrulamak için tıklayın **SQL veritabanları** ve yeni veritabanı doğrulayın **çevrimiçi**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>SQLPackage kullanarak BACPAC dosyasından alma

Bir SQL veritabanını kullanarak içeri aktarmak için [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) komut satırı yardımcı programını bkz [parametreleri ve özellikleri Al](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties). SQLPackage yardımcı programı en son sürümleri ile birlikte gelen [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) ve [Visual Studio için SQL Server veri Araçları](https://msdn.microsoft.com/library/mt204009.aspx), veya en son sürümünü indirebilirsiniz [SqlPackage ](https://www.microsoft.com/download/details.aspx?id=53876) doğrudan Microsoft İndirme Merkezi'nden.

SQLPackage yardımcı programının kullanımı ölçek ve performans çoğu üretim ortamlarında öneririz. BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Aşağıdaki içeri aktarma SQLPackage komutu bir komut dosyası örneği için bkz. **AdventureWorks2008R2** yerel depolama veritabanından adlı bir Azure SQL veritabanı mantıksal sunucusuna **mynewserver20170403** Bu örnekte. Bu betik adlı yeni bir veritabanı oluşturulmasını gösterir **myMigratedDatabase**, hizmet katmanı ile **Premium**ve bir hizmet hedefi, **P6**. Bu değerleri ortamınız için uygun şekilde değiştirin.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Azure SQL Veritabanı mantıksal sunucusu 1433 numaralı bağlantı noktasında dinler. Azure SQL Veritabanı mantıksal sunucusuna bir şirket ağından bağlanmaya çalışıyorsanız, bu bağlantı noktası başarıyla bağlanmanız için şirket güvenlik ağında açık olmalıdır.
>

Bu örnek, bir veritabanını SqlPackage.exe Active Directory Evrensel kimlik doğrulaması kullanarak içeri aktarma gösterir:

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a>PowerShell kullanarak BACPAC dosyasından alma

Kullanım [New-Azurermsqldatabaseımport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) cmdlet'i bir Azure SQL veritabanı hizmetini alma veritabanı isteğini göndermek için. Veritabanınızın boyutuna bağlı olarak, içeri aktarma işleminin tamamlanması biraz zaman alabilir.

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

İçeri aktarma isteği durumunu denetlemek için kullanmak [Get-Azurermsqldatabaseımportexportstatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet'i. Bu istekten sonra hemen döndürür genellikle çalıştıran **durumu: Inprogress**. Gördüğünüzde **durumu: başarılı** alma işlemi tamamlandı.

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
- Elastik havuzdaki bir veritabanı için içeri aktarma desteklenmiyor. Verileri tek bir veritabanına içeri aktarabilir ve sonra veritabanını havuza taşıma.

## <a name="import-using-other-methods"></a>Diğer yöntemleri kullanarak içeri aktarma

Bu sihirbazlar de kullanabilirsiniz:

- [SQL Server Management Studio'daki veri katmanı Uygulama Sihirbazı Alma](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard).
- [SQL Server içeri ve Dışarı Aktarma Sihirbazı'nı](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard).

## <a name="next-steps"></a>Sonraki adımlar
* Bağlanmak ve içeri aktarılan bir SQL veritabanını sorgulama hakkında bilgi edinmek için [SQL Server Management Studio ile SQL Database'e bağlanma ve örnek T-SQL sorgusu gerçekleştirme](sql-database-connect-query-ssms.md).
* BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* Performans önerileri de dahil olmak üzere tüm SQL Server veritabanı geçiş işlemi, hakkında ayrıntılı bilgi için bkz. [bir SQL Server veritabanını Azure SQL veritabanı'na geçirme](sql-database-cloud-migrate.md).
* Signitures güvenli bir şekilde, bkz. depolama anahtarları ve paylaşılan erişim yönetmek ve paylaşmak hakkında bilgi edinmek için [Azure depolama Güvenlik Kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-security-guide). 


  

