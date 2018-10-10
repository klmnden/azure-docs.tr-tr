---
title: Microsoft Azure PowerShell, SQL Server'ı geçirmek için Azure veritabanı geçiş hizmeti modülünde kullanın şirket içi Azure SQL DB'ye | Microsoft Docs
description: Azure PowerShell kullanarak şirket içi SQL Server'dan Azure SQL geçirmeyi öğrenin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/09/2018
ms.openlocfilehash: ffa4d5f87a722ed3cb95d873d02707ed1c797dc6
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886675"
---
# <a name="migrate-sql-server-on-premises-to-azure-sql-db-using-azure-powershell"></a>Şirket içi SQL Server, Azure PowerShell kullanarak Azure SQL DB'ye geçirme
Bu makalede, geçiş **Adventureworks2012** şirket içi örneği SQL Server 2016 veya üzeri için Microsoft Azure PowerShell kullanarak bir Azure SQL veritabanı'na geri yüklenen veritabanı. Kullanarak Azure SQL veritabanı'na bir şirket içi SQL Server örneğinden veritabanları geçirebilirsiniz `AzureRM.DataMigration` Microsoft Azure PowerShell modülü.

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
- Makalesinde açıklandığı gibi veri geçiş Yardımcısı'nı kullanarak şirket içi veritabanı ve şema geçiş değerlendirmesi tamamladınız [ bir SQL Server Geçiş değerlendirmesi gerçekleştirme](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem)
- İndirip kullanarak AzureRM.DataMigration modülü PowerShell Galerisi'nden yüklemek için [Install-Module PowerShell cmdlet'i](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1); yönetici olarak çalıştır'ı kullanarak powershell komut penceresi açın emin olun.
- Kaynak SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerini sağlamak için sahip [denetimi sunucusu](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) izni.
- Hedef Azure SQL DB'ye bağlanmak için kullanılan kimlik bilgilerini sağlamak için hedef Azure SQL veritabanı veritabanlarında CONTROL DATABASE izninizin örneği vardır.
- Azure aboneliği. Yoksa, oluşturun bir [ücretsiz](https://azure.microsoft.com/free/) başlamadan önce hesap.

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Microsoft Azure aboneliğiniz için oturum açın
Bu makaledeki yönergeleri kullanın [oturum Azure PowerShell ile oturum açma](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-4.4.1) PowerShell kullanarak Azure aboneliğinizde oturum açmak için.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir sanal makine oluşturmadan önce bir kaynak grubu oluşturun.

Kullanarak bir kaynak grubu oluşturma [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1) komutu. 

Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myResourceGroup* içinde *EastUS* bölge.

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```
## <a name="create-an-instance-of-the-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti örneği oluşturma 
Yeni Azure veritabanı geçiş hizmeti örneğini kullanarak oluşturabileceğiniz `New-AzureRmDataMigrationService` cmdlet'i. Bu cmdlet, aşağıdaki gerekli parametreler bekliyor:
- *Azure kaynak grubu adı*. Kullanabileceğiniz [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1) daha önce gösterildiği gibi Azure kaynak grubu oluşturun ve adını parametre olarak sağlamak için komutu.
- *Hizmet adı*. İçin Azure veritabanı geçiş hizmeti istenen benzersiz bir hizmet adına karşılık gelen dize 
- *Konum*. Hizmet konumunu belirtir. Batı ABD veya Güneydoğu Asya gibi bir Azure veri merkezi konumu belirtin
- *SKU*. Bu parametre, DMS Sku adına karşılık gelir. Şu anda desteklenen Sku adları *Basic_1vCore*, *Basic_2vCores*, *GeneralPurpose_4vCores*
- *Sanal alt ağ tanımlayıcısı*. Cmdlet'i kullanabilirsiniz [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?view=azurermps-4.4.1) bir alt ağ oluşturmak için. 

Aşağıdaki örnekte adlı bir hizmet oluşturur *MyDMS* kaynak grubundaki *MyDMSResourceGroup* bulunan *Doğu ABD* bölge adlıbirsanalağkullanma *MyVNET* ve alt *MySubnet*.

```powershell
 $vNet = Get-AzureRmVirtualNetwork -ResourceGroupName MyDMSResourceGroup -Name MyVNET

$vSubNet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vNet -Name MySubnet

$service = New-AzureRmDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId $vSubNet.Id`
```

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma
Azure veritabanı geçiş hizmeti örneği oluşturduktan sonra bir geçiş projesi oluşturun. Bir Azure veritabanı geçiş hizmeti projesi, hem kaynak ve hedef örneklerin yanı projenin bir parçası geçirmek istediğiniz veritabanlarının bir listesi için bağlantı bilgilerini gerektirir.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Kaynak ve hedef bağlantıları için bir veritabanı bağlantı bilgisi nesnesi oluşturun
Bir veritabanı bağlantı bilgisi nesnesi kullanarak oluşturabileceğiniz `New-AzureRmDmsConnInfo` cmdlet'i. Bu cmdlet şu parametreleri bekliyor:
- *ServerType*. Örneğin, SQL, Oracle veya MySQL istendi, veritabanı bağlantısı türü. SQL için SQL Server ve Azure SQL kullanın.
- *Veri kaynağı*. Adı veya IP bir SQL Server örneğine veya Azure SQL veritabanı.
- *AuthType*. SqlAuthentication ya da ServiceCredentials bağlantı için kimlik doğrulaması türü.
- *TrustServerCertificate* parametre kanal güven doğrulamak için sertifika zinciri walking atlayarak sırasında şifrelenmiş olup olmadığını gösteren bir değer ayarlar. Değer true veya false olabilir.

Aşağıdaki örnek, kaynak SQL Server'ın sql kimlik doğrulaması kullanarak MySourceSQLServer adlı bağlantı bilgisi oluşturur: 

```powershell
$sourceConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource MySourceSQLServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

Sonraki örnek, sql kimlik doğrulaması kullanarak SQLAzureTarget adlı bir Azure SQL veritabanı sunucusu için bağlantı bilgileri oluşturulmasını gösterir:

```powershell
$targetConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource "sqlazuretarget.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Veritabanları için geçiş projesi belirtin
Bir liste oluşturur `AzureRmDataMigrationDatabaseInfo` Azure veritabanı geçişi bir parçası olarak veritabanı projesini belirtir nesneleri sağlanan proje oluşturma için parametre olarak. Cmdlet `New-AzureRmDataMigrationDatabaseInfo` AzureRmDataMigrationDatabaseInfo oluşturmak için kullanılabilir. 

Aşağıdaki örnek, oluşturur `AzureRmDataMigrationDatabaseInfo` için proje **AdventureWorks2016** veritabanı ve proje oluşturma için parametre olarak sağlanacak listesine ekler.

```powershell
$dbInfo1 = New-AzureRmDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks2016
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Bir proje oluşturur
Son olarak, adlı Azure veritabanı geçiş projesi oluşturabilirsiniz *MyDMSProject* bulunan *Doğu ABD* kullanarak `New-AzureRmDataMigrationProject` ve önceden oluşturduğunuz kaynak ve hedef bağlantıları ve listesi ekleme Geçirilecek veritabanları.

```powershell
$project = New-AzureRmDataMigrationProject -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName MyDMSProject `
  -Location EastUS `
  -SourceType SQL `
  -TargetType SQLDB `
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

### <a name="create-a-table-map-and-select-source-and-target-parameters-for-migration"></a>Tablo eşleme oluşturup geçiş için kaynak ve hedef parametreleri seçin
Kaynak tablolardan hedef geçirilmesi için geçiş için gerekli olan başka bir parametre eşleme. Geçiş için kaynak ve hedef tablolar arasında bir eşleme sağlayan tablolar sözlüğü oluşturun. Aşağıdaki örnek, İnsan Kaynakları şema 2016 AdventureWorks veritabanı için kaynak ve hedef tablolar arasında eşleme gösterir.

```powershell
$tableMap = New-Object 'system.collections.generic.dictionary[string,string]'
$tableMap.Add("HumanResources.Department", "HumanResources.Department")
$tableMap.Add("HumanResources.Employee","HumanResources.Employee")
$tableMap.Add("HumanResources.EmployeeDepartmentHistory","HumanResources.EmployeeDepartmentHistory")
$tableMap.Add("HumanResources.EmployeePayHistory","HumanResources.EmployeePayHistory")
$tableMap.Add("HumanResources.JobCandidate","HumanResources.JobCandidate")
$tableMap.Add("HumanResources.Shift","HumanResources.Shift")
```

Kaynak ve hedef veritabanlarını seçin ve bir parametre olarak kullanılarak geçirmeye Tablo eşleme sağlamak için sonraki adımdır `New-AzureRmDmsSelectedDB` cmdlet'i, aşağıdaki örnekte gösterildiği gibi:

```powershell
$selectedDbs = New-AzureRmDmsSelectedDB -MigrateSqlServerSqlDb -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -TableMap $tableMap
```

### <a name="create-and-start-a-migration-task"></a>Bir geçiş görevi oluşturup başlatmak

Kullanım `New-AzureRmDataMigrationTask` geçiş görevi oluşturup başlatmak için cmdlet. Bu cmdlet şu parametreleri bekliyor:
- *TaskType*. SQL Server için Azure SQL veritabanı geçiş türü oluşturmak için geçiş görevinin türü *MigrateSqlServerSqlDb* beklenir. 
- *Kaynak grubu adı*. Görev oluşturulacağı Azure kaynak grubunun adı.
- *ServiceName*. Hangi görev oluşturmak Azure veritabanı geçiş hizmeti örneği.
- *ProjectName*. Görev oluşturmak Azure veritabanı geçiş hizmeti projesi adı. 
- *TaskName*. Oluşturulacak görev adı. 
- *KaynakBağlantı*. Kaynak SQL Server bağlantısını temsil eden AzureRmDmsConnInfo nesnesi.
- *TargetConnection*. Hedef Azure SQL veritabanı bağlantısını temsil eden AzureRmDmsConnInfo nesnesi.
- *SourceCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) kaynak sunucuya bağlanmak için nesne.
- *TargetCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) hedef sunucuya bağlanmak için nesne.
- *SelectedDatabase*. Kaynak ve hedef veritabanı eşleme temsil eden AzureRmDataMigrationSelectedDB nesnesi.

Aşağıdaki örnek, oluşturur ve myDMSTask adlı bir geçiş görevi başlatır:

```powershell
$migTask = New-AzureRmDataMigrationTask -TaskType MigrateSqlServerSqlDb `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs`
```

## <a name="monitor-the-migration"></a>Geçişi izleme
Aşağıdaki örnekte gösterildiği gibi görevin durumu özelliği sorgulanarak çalıştıran geçiş görevi izleyebilirsiniz:

```powershell
if (($mytask.ProjectTask.Properties.State -eq "Running") -or ($mytask.ProjectTask.Properties.State -eq "Queued"))
{
  write-host "migration task running"
}
```

## <a name="next-steps"></a>Sonraki adımlar
- Microsoft Geçiş Kılavuzu gözden [veritabanı Geçiş Kılavuzu](https://datamigration.microsoft.com/).