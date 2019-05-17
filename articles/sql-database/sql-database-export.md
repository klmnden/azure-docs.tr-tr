---
title: Tek bir dışarı aktarma veya bir Azure SQL veritabanını BACPAC dosyasına havuza | Microsoft Docs
description: Azure SQL veritabanını Azure portalını kullanarak BACPAC dosyasına dışarı aktarma
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 03/11/2019
ms.openlocfilehash: c87979760730cbe8f57d8f65463c94d08888aa2b
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65762747"
---
# <a name="export-an-azure-sql-database-to-a-bacpac-file"></a>Azure SQL veritabanını BACPAC dosyasına dışarı aktarma

Arşivleme veya başka bir platformuna geçmek için bir veritabanı dışarı aktarmak, ihtiyacınız olduğunda, veritabanı şemasını ve verilerini dışa aktarabilirsiniz bir [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) dosya. Bir BACPAC dosyasına BACPAC SQL Server veritabanındaki verileri ve meta verileri içeren bir uzantıya sahip bir ZIP dosyasıdır. BACPAC dosyasını Azure Blob Depolama alanında veya bir şirket içi konuma yerel depolama alanında depolanabilir ve daha sonra içeri aktarılan arka Azure SQL veritabanı veya SQL Server şirket yükleme.

## <a name="considerations-when-exporting-an-azure-sql-database"></a>Azure SQL veritabanını dışarı aktarma durumlarda dikkat edilmesi gerekenler

- Bir verme işlemsel olarak tutarlı olmasını ya da yazma olmayan emin olmanız gerekir etkinlik dışarı aktarma sırasında gerçekleşen veya dışarı aktarma bir [işlemsel olarak tutarlı bir kopyası](sql-database-copy.md) Azure SQL veritabanınızın.
- Blob depolama alanına dışa aktarıyorsanız, bir BACPAC dosyasının en büyük boyutu 200 GB'dir. Daha büyük bir BACPAC dosyasına arşivleme için yerel depolama birimine dışarı aktarın.
- Bu makalede bahsedilen yöntemler kullanarak Azure premium depolama için bir BACPAC dosyasına dışarı aktarma desteklenmiyor.
- Bir güvenlik duvarının arkasındaki depolama şu anda desteklenmiyor.
- Azure SQL veritabanı'ndan dışarı aktarma işlemi 20 saat aşarsa, iptal edilebilir. Dışarı aktarma sırasında performansı artırmak için şunları yapabilirsiniz:

  - Geçici olarak, işlem boyutunu artırın.
  - Tüm okuma ve dışarı aktarma sırasında etkinlik yazma kesildi.
  - Kullanım bir [kümelenmiş dizin](https://msdn.microsoft.com/library/ms190457.aspx) boş olmayan değerlerinin tüm büyük tablolarda ile. 6-12 saatten uzun sürerse Kümelenmiş dizinler bir dışarı aktarım başarısız olabilir. Dışarı aktarma hizmeti tüm tabloyu dışarı aktarmak için bir tablo taraması tamamlaması gereken olmasıdır. Tablolarınızı dışarı aktarma çalıştırmak için iyileştirilmiş, belirlemek için en iyi yolu **DBCC SHOW_STATISTICS** emin olun *RANGE_HI_KEY* null değil ve iyi dağıtım değerine sahiptir. Ayrıntılar için bkz [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [!NOTE]
> İşlemlerinde bacpac dosyaları için yedekleme ve geri yükleme işlemleri amaçlanmamıştır. Azure SQL veritabanı, her bir kullanıcı veritabanı için Yedekleme otomatik olarak oluşturur. Ayrıntılar için bkz [iş sürekliliğine genel bakış](sql-database-business-continuity.md) ve [SQL veritabanı yedeklemelerini](sql-database-automated-backups.md).

## <a name="export-to-a-bacpac-file-using-the-azure-portal"></a>Azure portalını kullanarak BACPAC dosyasına dışarı aktarma

> [!NOTE]
> [Yönetilen örnek](sql-database-managed-instance.md) bir veritabanını Azure portalını kullanarak BACPAC dosyasına dışarı aktarma şu anda desteklemiyor. Yönetilen örnek bir BACPAC dosyasına dışarı aktarmak için SQL Server Management Studio veya SQLPackage kullanın.

1. Veritabanını kullanarak dışarı aktarmak için [Azure portalında](https://portal.azure.com), veritabanınız için sayfayı açın ve tıklayın **dışarı** araç.

   ![Veritabanı dışarı aktarma](./media/sql-database-export/database-export1.png)

2. BACPAC dosya adını belirtin, bir var olan Azure depolama hesabı ve kapsayıcı dışa aktarmak için seçin ve ardından kaynak veritabanı erişimi için uygun kimlik bilgilerini sağlayın.

    ![Veritabanı dışarı aktarma](./media/sql-database-export/database-export2.png)

3. **Tamam**'ı tıklatın.

4. Dışarı aktarma işleminin ilerleme durumunu izlemek için dışarı aktarılan veritabanını içeren SQL veritabanı sunucusu için sayfayı açın. Altında için **ayarları** ve ardından **içeri/dışarı aktarma geçmişi**.

   ![dışarı aktarma geçmişi](./media/sql-database-export/export-history.png)

## <a name="export-to-a-bacpac-file-using-the-sqlpackage-utility"></a>SQLPackage yardımcı programını kullanarak BACPAC dosyasına dışarı aktarma

Bir SQL veritabanını kullanarak dışarı aktarmak için [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) komut satırı yardımcı programını bkz [dışarı parametreleri ve özellikleri](https://docs.microsoft.com/sql/tools/sqlpackage#export-parameters-and-properties). SQLPackage yardımcı programı en son sürümleri ile birlikte gelen [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) ve [Visual Studio için SQL Server veri Araçları](https://msdn.microsoft.com/library/mt204009.aspx), veya en son sürümünü indirebilirsiniz [SqlPackage ](https://www.microsoft.com/download/details.aspx?id=53876) doğrudan Microsoft İndirme Merkezi'nden.

SQLPackage yardımcı programının kullanımı ölçek ve performans çoğu üretim ortamlarında öneririz. BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/20../../migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Bu örnek, bir veritabanını SqlPackage.exe Active Directory Evrensel kimlik doğrulaması kullanarak dışarı aktarma işlemini gösterir:

```cmd
SqlPackage.exe /a:Export /tf:testExport.bacpac /scs:"Data Source=apptestserver.database.windows.net;Initial Catalog=MyDB;" /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="export-to-a-bacpac-file-using-sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS) kullanarak bir BACPAC dosyasına aktarma

SQL Server Management Studio en yeni sürümleri, Azure SQL veritabanını BACPAC dosyasına dışarı aktarmak için bir sihirbaz sağlar. Bkz: [veri katmanı uygulaması dışarı aktarma](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application).

## <a name="export-to-a-bacpac-file-using-powershell"></a>PowerShell kullanarak BACPAC dosyasına dışarı aktarma

> [!NOTE]
> [Yönetilen örnek](sql-database-managed-instance.md) bir veritabanı, Azure PowerShell kullanarak BACPAC dosyasına dışarı aktarma şu anda desteklemiyor. Yönetilen örnek bir BACPAC dosyasına dışarı aktarmak için SQL Server Management Studio veya SQLPackage kullanın.

Kullanım [yeni AzSqlDatabaseExport](/powershell/module/az.sql/new-azsqldatabaseexport) cmdlet'i bir Azure SQL veritabanı hizmet verme veritabanı isteğini göndermek için. Veritabanınızın boyutuna bağlı olarak, dışarı aktarma işleminin tamamlanması biraz zaman alabilir.

```powershell
$exportRequest = New-AzSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
  -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
  -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password
```

Dışarı aktarma isteği durumunu denetlemek için kullanmak [Get-AzSqlDatabaseImportExportStatus](/powershell/module/az.sql/get-azsqldatabaseimportexportstatus) cmdlet'i. Bu istekten sonra hemen döndürür genellikle çalıştıran **durumu: Inprogress**. Gördüğünüzde **durumu: Başarılı** verme tamamlandı.

```powershell
$exportStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
[Console]::Write("Exporting")
while ($exportStatus.Status -eq "InProgress")
{
    Start-Sleep -s 10
    $exportStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
    [Console]::Write(".")
}
[Console]::WriteLine("")
$exportStatus
```

## <a name="next-steps"></a>Sonraki adımlar

- Tek uzun süreli yedek saklama hakkında bilgi edinmek için alternatif bir veritabanını arşivleme amacıyla dışarı veritabanları ve havuza alınmış veritabanları görmeniz [uzun süreli yedek saklama](sql-database-long-term-retention.md). SQL Aracısı işleri zamanlamak için kullanabileceğiniz [yalnızca kopya yedekleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server) uzun süreli yedek saklama alternatif olarak.
- BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
- Bir SQL Server veritabanına BACPAC aktarma hakkında bilgi edinmek için [bir SQL Server veritabanına BACPAC aktarma](https://msdn.microsoft.com/library/hh710052.aspx).
- SQL Server veritabanındaki verileri bir BACPAC aktarma hakkında bilgi edinmek için [veri katmanı uygulaması dışarı aktarma](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application)
- Veri geçiş hizmeti kullanarak bir veritabanını geçirme hakkında bilgi edinmek için [Azure SQL veritabanı için SQL Server'ı geçirme DMS kullanarak çevrimdışı](../dms/tutorial-sql-server-to-azure-sql.md).
- Azure SQL veritabanı'na geçiş için bir tanıtımlar olarak SQL Server'dan verdiğiniz olup [bir SQL Server veritabanını Azure SQL veritabanı'na geçirme](sql-database-single-database-migrate.md).
- Depolama anahtarları ve paylaşılan erişim imzaları güvenli bir şekilde, bkz: yönetmek ve paylaşmak hakkında bilgi edinmek için [Azure depolama Güvenlik Kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-security-guide).
