---
title: Azure SQL veritabanını BACPAC dosyasına dışarı aktarma | Microsoft Docs
description: Azure SQL veritabanını Azure portalını kullanarak BACPAC dosyasına dışarı aktarma
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 050da5e71fd804055d0a2ece1150b79b3922170f
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56100593"
---
# <a name="export-an-azure-sql-database-to-a-bacpac-file"></a>Azure SQL veritabanını BACPAC dosyasına dışarı aktarma

Arşivleme veya başka bir platformuna geçmek için bir veritabanı dışarı aktarmak, ihtiyacınız olduğunda, veritabanı şemasını ve verilerini dışa aktarabilirsiniz bir [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) dosya. Bir BACPAC dosyasına BACPAC SQL Server veritabanındaki verileri ve meta verileri içeren bir uzantıya sahip bir ZIP dosyasıdır. BACPAC dosyasını Azure Blob Depolama alanında veya bir şirket içi konuma yerel depolama alanında depolanabilir ve daha sonra içeri aktarılan arka Azure SQL veritabanı veya SQL Server şirket yükleme.

> [!IMPORTANT]
> Azure SQL veritabanı otomatik dışa aktarma, 1 Mart 2017'de devre dışı bırakılan. Kullanabileceğiniz [uzun süreli yedek saklama](sql-database-long-term-retention.md
) veya [Azure Otomasyonu](https://github.com/Microsoft/azure-docs/blob/2461f706f8fc1150e69312098640c0676206a531/articles/automation/automation-intro.md) düzenli aralıklarla SQL arşivlemek için veritabanları PowerShell kullanarak tercih ettiğiniz bir zamanlamaya göre. Bir örnek için indirme [PowerShell betik örneği](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) github'dan.
>

## <a name="considerations-when-exporting-an-azure-sql-database"></a>Azure SQL veritabanını dışarı aktarma durumlarda dikkat edilmesi gerekenler

- Bir verme işlemsel olarak tutarlı olmasını ya da yazma olmayan emin olmanız gerekir etkinlik dışarı aktarma sırasında gerçekleşen veya dışarı aktarma bir [işlemsel olarak tutarlı bir kopyası](sql-database-copy.md) Azure SQL veritabanınızın.
- Blob depolama alanına dışa aktarıyorsanız, bir BACPAC dosyasının en büyük boyutu 200 GB'dir. Daha büyük bir BACPAC dosyasına arşivleme için yerel depolama birimine dışarı aktarın.
- Bu makalede bahsedilen yöntemler kullanarak Azure premium depolama için bir BACPAC dosyasına dışarı aktarma desteklenmiyor.
- Azure SQL veritabanı'ndan dışarı aktarma işlemi 20 saat aşarsa, iptal edilebilir. Dışarı aktarma sırasında performansı artırmak için şunları yapabilirsiniz:
  - Geçici olarak, işlem boyutunu artırın.
  - Tüm okuma ve dışarı aktarma sırasında etkinlik yazma kesildi.
  - Kullanım bir [kümelenmiş dizin](https://msdn.microsoft.com/library/ms190457.aspx) boş olmayan değerlerinin tüm büyük tablolarda ile. 6-12 saatten uzun sürerse Kümelenmiş dizinler bir dışarı aktarım başarısız olabilir. Dışarı aktarma hizmeti tüm tabloyu dışarı aktarmak için bir tablo taraması tamamlaması gereken olmasıdır. Tablolarınızı dışarı aktarma çalıştırmak için iyileştirilmiş, belirlemek için en iyi yolu **DBCC SHOW_STATISTICS** emin olun *RANGE_HI_KEY* null değil ve iyi dağıtım değerine sahiptir. Ayrıntılar için bkz [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [!NOTE]
> İşlemlerinde bacpac dosyaları için yedekleme ve geri yükleme işlemleri amaçlanmamıştır. Azure SQL veritabanı, her bir kullanıcı veritabanı için Yedekleme otomatik olarak oluşturur. Ayrıntılar için bkz [iş Sürekliliğine genel bakış](sql-database-business-continuity.md) ve [SQL veritabanı yedeklemelerini](sql-database-automated-backups.md).

## <a name="export-to-a-bacpac-file-using-the-azure-portal"></a>Azure portalını kullanarak BACPAC dosyasına dışarı aktarma

Veritabanını kullanarak dışarı aktarmak için [Azure portalında](https://portal.azure.com), veritabanınız için sayfayı açın ve tıklayın **dışarı** araç. BACPAC dosya adını belirtin, dışa aktarma için Azure depolama hesabı ve kapsayıcı sağlayabilir ve kaynak veritabanına bağlanmak için kimlik bilgilerini belirtin.

![Veritabanı dışarı aktarma](./media/sql-database-export/database-export.png)

Dışarı aktarma işleminin ilerleme durumunu izlemek için dışarı aktarılan veritabanını içeren SQL veritabanı sunucusu için sayfayı açın. Ekranı aşağı kaydırarak **işlemleri** ve ardından **içeri/dışarı aktarma** geçmişi.

![dışarı aktarma geçmişi](./media/sql-database-export/export-history.png)
![geçmişi durumunu Dışarı Aktar](./media/sql-database-export/export-history2.png)

## <a name="export-to-a-bacpac-file-using-the-sqlpackage-utility"></a>SQLPackage yardımcı programını kullanarak BACPAC dosyasına dışarı aktarma

Bir SQL veritabanını kullanarak dışarı aktarmak için [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) komut satırı yardımcı programını bkz [dışarı parametreleri ve özellikleri](https://docs.microsoft.com/sql/tools/sqlpackage#export-parameters-and-properties). SQLPackage yardımcı programı en son sürümleri ile birlikte gelen [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) ve [Visual Studio için SQL Server veri Araçları](https://msdn.microsoft.com/library/mt204009.aspx), veya en son sürümünü indirebilirsiniz [SqlPackage ](https://www.microsoft.com/download/details.aspx?id=53876) doğrudan Microsoft İndirme Merkezi'nden.

SQLPackage yardımcı programının kullanımı ölçek ve performans çoğu üretim ortamlarında öneririz. BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Bu örnek, bir veritabanını SqlPackage.exe Active Directory Evrensel kimlik doğrulaması kullanarak dışarı aktarma işlemini gösterir:

```cmd
SqlPackage.exe /a:Export /tf:testExport.bacpac /scs:"Data Source=apptestserver.database.windows.net;Initial Catalog=MyDB;" /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="export-to-a-bacpac-file-using-sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS) kullanarak bir BACPAC dosyasına aktarma

SQL Server Management Studio en yeni sürümleri, Azure SQL veritabanını BACPAC dosyasına dışarı aktarmak için sihirbaz da sağlar. Bkz: [veri katmanı uygulaması dışarı aktarma](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application).

## <a name="export-to-a-bacpac-file-using-powershell"></a>PowerShell kullanarak BACPAC dosyasına dışarı aktarma

Kullanım [yeni AzureRmSqlDatabaseExport](/powershell/module/azurerm.sql/new-azurermsqldatabaseexport) cmdlet'i bir Azure SQL veritabanı hizmet verme veritabanı isteğini göndermek için. Veritabanınızın boyutuna bağlı olarak, dışarı aktarma işleminin tamamlanması biraz zaman alabilir.

```powershell
$exportRequest = New-AzureRmSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
  -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
  -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password
```

Dışarı aktarma isteği durumunu denetlemek için kullanmak [Get-Azurermsqldatabaseımportexportstatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet'i. Bu istekten sonra hemen döndürür genellikle çalıştıran **durumu: Inprogress**. Gördüğünüzde **durumu: Başarılı** verme tamamlandı.

```powershell
$exportStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
[Console]::Write("Exporting")
while ($exportStatus.Status -eq "InProgress")
{
    Start-Sleep -s 10
    $exportStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
    [Console]::Write(".")
}
[Console]::WriteLine("")
$exportStatus
```

## <a name="next-steps"></a>Sonraki adımlar

- Arşiv amacıyla bir veritabanını alternatif dışarı gibi bir Azure SQL veritabanı yedeklemesini, uzun süreli yedek saklama hakkında bilgi edinmek için [uzun süreli yedek saklama](sql-database-long-term-retention.md).
- BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
- Bir SQL Server veritabanına BACPAC aktarma hakkında bilgi edinmek için [bir SQL Server veritabanına BACPAC aktarma](https://msdn.microsoft.com/library/hh710052.aspx).
- SQL Server veritabanındaki verileri bir BACPAC aktarma hakkında bilgi edinmek için [veri katmanı uygulaması dışarı aktarma](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application)
- Veri geçiş hizmeti kullanarak bir veritabanını geçirme hakkında bilgi edinmek için [Azure SQL veritabanı için SQL Server'ı geçirme DMS kullanarak çevrimdışı](../dms/tutorial-sql-server-to-azure-sql.md).
- Azure SQL veritabanı'na geçiş için bir tanıtımlar olarak SQL Server'dan verdiğiniz olup [bir SQL Server veritabanını Azure SQL veritabanı'na geçirme](sql-database-single-database-migrate.md).
- Depolama anahtarları ve paylaşılan erişim imzaları güvenli bir şekilde, bkz: yönetmek ve paylaşmak hakkında bilgi edinmek için [Azure depolama Güvenlik Kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-security-guide).
