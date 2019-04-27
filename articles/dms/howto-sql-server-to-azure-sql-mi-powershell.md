---
title: Veritabanı geçiş hizmeti ve PowerShell ile Azure SQL veritabanı yönetilen örneği SQL Server'ı geçirme | Microsoft Docs
description: Azure PowerShell kullanarak şirket içi SQL Server'dan Azure SQL DB yönetilen örneği için geçirmeyi öğrenin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/12/2019
ms.openlocfilehash: ed0d65a0f00bd5ebc3227a249beec6bafd791347
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60532557"
---
# <a name="migrate-sql-server-on-premises-to-azure-sql-database-managed-instance-using-azure-powershell"></a>Şirket içi SQL Server'dan Azure SQL veritabanı yönetilen Azure PowerShell kullanarak örneğine geçirme
Bu makalede, geçiş **Adventureworks2012** şirket içi örneği SQL Server 2005 veya üzeri için Microsoft Azure PowerShell kullanarak Azure SQL veritabanı yönetilen örneği için geri yüklenen veritabanı. Kullanarak Azure SQL veritabanı yönetilen örneği için bir şirket içi SQL Server örneğinden veritabanları geçirebilirsiniz `Az.DataMigration` Microsoft Azure PowerShell modülü.

Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
> * Bir kaynak grubu oluşturun.
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturma.
> * Bir geçiş projesi, Azure veritabanı geçiş hizmeti örneği oluşturun.
> * Geçişi çalıştırma.

## <a name="prerequisites"></a>Önkoşullar
Bu adımları tamamlamak için ihtiyacınız vardır:

- [SQL Server 2016 veya üzeri](https://www.microsoft.com/sql-server/sql-server-downloads) (herhangi bir sürümü)
- SQL Server Express yüklemesi ile varsayılan olarak devre dışıdır TCP/IP Protokolü etkinleştirmek için. Makaleyi izleyerek TCP/IP protokolünü etkinleştirin [etkinleştirmek veya devre dışı sunucu ağ protokolünü](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Yapılandırmak için [veritabanı altyapısı erişimi için Windows Güvenlik Duvarı](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Bir Azure SQL veritabanı örneği. Bir Azure SQL veritabanı örneğine makalesinde ayrıntılı olarak izleyerek oluşturabilirsiniz [Azure portalında bir Azure SQL veritabanı oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- [Data Migration Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 veya üzeri.
- Bir VNET kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı ile Azure veritabanı geçiş hizmeti sağlayan Azure Resource Manager dağıtım modeli kullanarak oluşturmuş olmanız [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Makalesinde açıklandığı gibi veri geçiş Yardımcısı'nı kullanarak şirket içi veritabanı ve şema geçiş değerlendirmesi tamamladınız [bir SQL Server Geçiş değerlendirmesi gerçekleştirme](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem)
- İndirip kullanarak Az.DataMigration modülü PowerShell Galerisi'nden yüklemek için [Install-Module PowerShell cmdlet'i](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1)
- Kaynak SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerini sağlamak için sahip [denetimi sunucusu](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) izni.
- Hedef Azure SQL DB'ye bağlanmak için kullanılan kimlik bilgilerini sağlamak için hedef Azure SQL veritabanı veritabanlarında CONTROL DATABASE izninizin örneği vardır.
- Azure aboneliği. Yoksa, oluşturun bir [ücretsiz](https://azure.microsoft.com/free/) başlamadan önce hesap.

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Microsoft Azure aboneliğiniz için oturum açın
Bu makaledeki yönergeleri kullanın [oturum Azure PowerShell ile oturum açma](https://docs.microsoft.com/powershell/azure/authenticate-azureps) PowerShell kullanarak Azure aboneliğinizde oturum açmak için.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir sanal makine oluşturmadan önce bir kaynak grubu oluşturun.

Kullanarak bir kaynak grubu oluşturma [yeni AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) komutu. 

Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myResourceGroup* içinde *EastUS* bölge.

```powershell
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```
## <a name="create-an-instance-of-the-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti örneği oluşturma 
Yeni Azure veritabanı geçiş hizmeti örneğini kullanarak oluşturabileceğiniz `New-AzDataMigrationService` cmdlet'i. Bu cmdlet, aşağıdaki gerekli parametreler bekliyor:
- *Azure kaynak grubu adı*. Kullanabileceğiniz [yeni AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) daha önce gösterildiği gibi Azure kaynak grubu oluşturun ve adını parametre olarak sağlamak için komutu.
- *Hizmet adı*. İçin Azure veritabanı geçiş hizmeti istenen benzersiz bir hizmet adına karşılık gelen dize 
- *Konum*. Hizmet konumunu belirtir. Batı ABD veya Güneydoğu Asya gibi bir Azure veri merkezi konumu belirtin
- *SKU*. Bu parametre, DMS Sku adına karşılık gelir. Şu anda desteklenen Sku adları *Basic_1vCore*, *Basic_2vCores*, *GeneralPurpose_4vCores*
- *Sanal alt ağ tanımlayıcısı*. Cmdlet'i kullanabilirsiniz [yeni AzVirtualNetworkSubnetConfig](https://docs.microsoft.com//powershell/module/az.network/new-azvirtualnetworksubnetconfig) bir alt ağ oluşturmak için. 

Aşağıdaki örnekte adlı bir hizmet oluşturur *MyDMS* kaynak grubundaki *MyDMSResourceGroup* bulunan *Doğu ABD* bölge adlıbirsanalağkullanma *MyVNET* ve alt *MySubnet*.

```powershell
 $vNet = Get-AzVirtualNetwork -ResourceGroupName MyDMSResourceGroup -Name MyVNET

$vSubNet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vNet -Name MySubnet

$service = New-AzDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId $vSubNet.Id`
```

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma
Azure veritabanı geçiş hizmeti örneği oluşturduktan sonra bir geçiş projesi oluşturun. Bir Azure veritabanı geçiş hizmeti projesi, hem kaynak ve hedef örneklerin yanı projenin bir parçası geçirmek istediğiniz veritabanlarının bir listesi için bağlantı bilgilerini gerektirir.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Kaynak ve hedef bağlantıları için bir veritabanı bağlantı bilgisi nesnesi oluşturun
Bir veritabanı bağlantı bilgisi nesnesi kullanarak oluşturabileceğiniz `New-AzDmsConnInfo` cmdlet'i. Bu cmdlet şu parametreleri bekliyor:
- *ServerType*. Örneğin, SQL, Oracle veya MySQL istendi, veritabanı bağlantısı türü. SQL için SQL Server ve Azure SQL kullanın.
- *Veri kaynağı*. Adı veya IP bir SQL Server örneğine veya Azure SQL veritabanı.
- *AuthType*. SqlAuthentication ya da ServiceCredentials bağlantı için kimlik doğrulaması türü.
- *TrustServerCertificate* parametre kanal güven doğrulamak için sertifika zinciri walking atlayarak sırasında şifrelenmiş olup olmadığını gösteren bir değer ayarlar. Değer true veya false olabilir.

Aşağıdaki örnek, kaynak SQL Server'ın sql kimlik doğrulaması kullanarak MySourceSQLServer adlı bağlantı bilgisi oluşturur: 

```powershell
$sourceConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource MySourceSQLServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

Sonraki örnek, 'sql kimlik doğrulaması kullanarak targetmanagedinstance.database.windows.net' adlı bir Azure SQL veritabanı yönetilen örneği sunucusu için bağlantı bilgileri oluşturulmasını gösterir:

```powershell
$targetConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource "targetmanagedinstance.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Veritabanları için geçiş projesi belirtin
Bir liste oluşturur `AzDataMigrationDatabaseInfo` Azure veritabanı geçişi bir parçası olarak veritabanı projesini belirtir nesneleri sağlanan proje oluşturma için parametre olarak. Cmdlet `New-AzDataMigrationDatabaseInfo` AzDataMigrationDatabaseInfo oluşturmak için kullanılabilir. 

Aşağıdaki örnek, oluşturur `AzDataMigrationDatabaseInfo` için proje **AdventureWorks** veritabanı ve proje oluşturma için parametre olarak sağlanacak listesine ekler.

```powershell
$dbInfo1 = New-AzDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Bir proje oluşturur
Son olarak, adlı Azure veritabanı geçiş projesi oluşturabilirsiniz *MyDMSProject* bulunan *Doğu ABD* kullanarak `New-AzDataMigrationProject` ve önceden oluşturduğunuz kaynak ve hedef bağlantıları ve listesi ekleme Geçirilecek veritabanları.

```powershell
$project = New-AzDataMigrationProject -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName MyDMSProject `
  -Location EastUS `
  -SourceType SQL `
  -TargetType SQLMI `
  -SourceConnection $sourceConnInfo `
  -TargetConnection $targetConnInfo `
  -DatabaseInfo $dbList
```

## <a name="create-and-start-a-migration-task"></a>Bir geçiş görevi oluşturup başlatmak
Son olarak, Azure veritabanı geçiş görevi oluşturup başlatmak. Azure veritabanı geçiş görevi, kaynak ve hedef ve bir önkoşul olarak oluşturulan proje zaten sağlanan bilgilere ek olarak geçirilmesi, veritabanı tablolarını listesi için bağlantı kimlik bilgilerini gerektirir. 

### <a name="create-credential-parameters-for-source-and-target"></a>Kaynak ve hedef için kimlik bilgisi parametreleri oluşturma
Bağlantı güvenlik kimlik bilgileri olarak oluşturulabilir bir [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) nesne. 

Aşağıdaki örnek oluşturulmasını gösterir *PSCredential* parola dizesi değişkenleri olarak sağlayarak hem kaynak hem de hedef bağlantıları için nesneleri *$sourcePassword* ve *$ HedefParolası*. 

```powershell
$secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-backup-fileshare-object"></a>Yedekleme dosya paylaşımını oluşturur.
Şimdi Azure veritabanı geçiş Hizmeti'nin kaynak yeni AzDmsFileShare cmdlet'ini kullanarak veritabanı yedeklemeleri sürebilir yerel SMB ağ paylaşımını temsil eden dosya paylaşımı nesnesi oluşturun.

```powershell
$backupPassword = ConvertTo-SecureString -String $password -AsPlainText -Force
$backupCred = New-Object System.Management.Automation.PSCredential ($backupUserName, $backupPassword)

$backupFileSharePath="\\10.0.0.76\SharedBackup"
$backupFileShare = New-AzDmsFileShare -Path $backupFileSharePath -Credential $backupCred
```

### <a name="create-selected-database-object"></a>Seçilen veritabanı nesnesi oluşturma
Sonraki adım, aşağıdaki örnekte gösterildiği gibi yeni AzDmsSelectedDB cmdlet'ini kullanarak kaynak ve hedef veritabanları seçmektir:

```powershell
$selectedDbs = @()
$selectedDbs += New-AzDmsSelectedDB -MigrateSqlServerSqlDbMi `
  -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -BackupFileShare $backupFileShare `
```

### <a name="sas-uri-for-azure-storage-container"></a>Azure depolama kapsayıcısının SAS URI'si
Azure veritabanı geçiş hizmeti hizmet yedekleme dosyaları karşıya yükler depolama hesabının kapsayıcıya erişimi veren SAS URI'sini içeren bir değişken oluşturun.

```powershell
$blobSasUri="https://mystorage.blob.core.windows.net/test?st=2018-07-13T18%3A10%3A33Z&se=2019-07-14T18%3A10%3A00Z&sp=rwdl&sv=2018-03-28&sr=c&sig=qKlSA512EVtest3xYjvUg139tYSDrasbftY%3D"
```

### <a name="select-logins"></a>Oturum açmaları seçme
Aşağıdaki örnekte gösterildiği gibi geçirilmesi için oturumların listesini oluşturun:  Not, şu anda DMS yalnızca geçirme SQL oturumları destekler. 

```powershell
$selectedLogins = @("user1", "user2")
```

### <a name="select-agent-jobs"></a>Aracı işleri seçin
Aşağıdaki örnekte gösterildiği gibi geçirilmesi Aracısı işleri oluşturun:

>[!NOTE]
>Şu anda yalnızca T-SQL alt sistemi iş adımları işlerle DMS destekler.

```powershell
$selectedAgentJobs = @("agentJob1", "agentJob2")
```

### <a name="create-and-start-a-migration-task"></a>Bir geçiş görevi oluşturup başlatmak

Kullanım `New-AzDataMigrationTask` geçiş görevi oluşturup başlatmak için cmdlet. Bu cmdlet şu parametreleri bekliyor:
- *TaskType*. SQL Server için Azure SQL veritabanı yönetilen örneğine geçiş türüne oluşturmak için geçiş görevinin türü *MigrateSqlServerSqlDbMi* beklenir. 
- *Kaynak grubu adı*. Görev oluşturulacağı Azure kaynak grubunun adı.
- *ServiceName*. Hangi görev oluşturmak Azure veritabanı geçiş hizmeti örneği.
- *ProjectName*. Görev oluşturmak Azure veritabanı geçiş hizmeti projesi adı. 
- *TaskName*. Oluşturulacak görev adı. 
- *KaynakBağlantı*. Kaynak SQL Server bağlantısını temsil eden AzDmsConnInfo nesnesi.
- *TargetConnection*. Hedef Azure SQL veritabanı yönetilen örneği bağlantıyı temsil eden AzDmsConnInfo nesnesi.
- *SourceCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) kaynak sunucuya bağlanmak için nesne.
- *TargetCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) hedef sunucuya bağlanmak için nesne.
- *SelectedDatabase*. Kaynak ve hedef veritabanı eşleme temsil eden AzDataMigrationSelectedDB nesnesi.
- *BackupFileShare*. Azure veritabanı geçiş Hizmeti'nin veritabanı yedeklemeleri oluşturmak için kaynak sürebilir yerel ağ paylaşımı temsil eden dosya paylaşımı nesnesi.
- *BackupBlobSasUri*. Azure veritabanı geçiş Hizmeti'nin hizmeti yedekleme dosyaları karşıya yükler depolama hesabının kapsayıcıya erişimi sağlayan SAS URI'si. Blob kapsayıcısı için SAS URI'sini almayı öğrenin.
- *SelectedLogins*. Geçiş için seçilen oturumları listesi.
- *SelectedAgentJobs*. Geçiş için seçilen Aracısı işleri listesi.

Aşağıdaki örnek, oluşturur ve myDMSTask adlı bir geçiş görevi başlatır:

```powershell
$migTask = New-AzDataMigrationTask -TaskType MigrateSqlServerSqlDbMi `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs `
  -BackupFileShare $backupFileShare `
  -BackupBlobSasUri $blobSasUri `
  -SelectedLogins $selectedLogins `
  -SelectedAgentJobs $selectedJobs `
```

## <a name="monitor-the-migration"></a>Geçişi izleme
Aşağıdaki örnekte gösterildiği gibi görevin durumu özelliği sorgulanarak çalıştıran geçiş görevi izleyebilirsiniz:

```powershell
if (($mytask.ProjectTask.Properties.State -eq "Running") -or ($mytask.ProjectTask.Properties.State -eq "Queued"))
{
  write-host "migration task running"
}
```

## <a name="deleting-the-dms-instance"></a>DMS örneği siliniyor
Geçiş tamamlandıktan sonra Azure DMS örneği silebilirsiniz:

```powershell
Remove-AzDms -ResourceGroupName myResourceGroup -ServiceName MyDMS
```

## <a name="next-steps"></a>Sonraki adımlar
- Microsoft Geçiş Kılavuzu gözden [veritabanı Geçiş Kılavuzu](https://datamigration.microsoft.com/).
