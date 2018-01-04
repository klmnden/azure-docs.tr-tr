---
title: "SQL Server geçirmek için Microsoft Azure PowerShell'de kullanım Azure veritabanı geçiş hizmeti modülü şirket içi Azure SQL DB'ye | Microsoft Docs"
description: "Şirket içi SQL Server'dan Azure SQL Azure PowerShell kullanarak geçirmek öğrenin."
services: database-migration
author: HJToland3
ms.author: jtoland
manager: 
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 12/13/2017
ms.openlocfilehash: 9eebe8352d6a447df520c194b9906df8c2c9a83f
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="migrate-sql-server-on-premises-to-azure-sql-db-using-azure-powershell"></a>Azure PowerShell kullanarak Azure SQL DB için şirket içi SQL Server geçirme
Bu makalede, geçiş **Adventureworks2012** bir şirket içi örneği SQL Server 2016 veya üstünü, Microsoft Azure PowerShell kullanarak bir Azure SQL veritabanına geri yüklenen veritabanı. Kullanarak Azure SQL veritabanı için bir şirket içi SQL Server örneğinden veritabanları geçirebilirsiniz `AzureRM.DataMigration` Microsoft Azure PowerShell modülü.

Bu makalede, bilgi nasıl yapılır:
> [!div class="checklist"]
> * Bir kaynak grubu oluşturun.
> * Azure veritabanı geçiş hizmeti örneği oluşturun.
> * Bir geçiş projesi Azure veritabanı geçiş hizmeti örneğini oluşturun.
> * Geçiş çalıştırın.

## <a name="prerequisites"></a>Ön koşullar
Bu adımları tamamlamak için gerekir:

- [SQL Server 2016 veya sonraki](https://www.microsoft.com/sql-server/sql-server-downloads) (herhangi bir sürümünü)
- TCP/IP protokolü, SQL Server Express yüklemesi ile varsayılan olarak devre dışıdır. İzleyerek etkinleştirin [bu makaledeki yönergeleri](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Yapılandırmak için [veritabanı altyapısı erişimi için Windows Güvenlik Duvarı](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Bir Azure SQL veritabanı örneği. Makaleyi ayrıntısı izleyerek bir Azure SQL veritabanı örneği oluşturabilirsiniz [Azure portalında bir Azure SQL veritabanı oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- [Veri geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 veya sonraki bir sürümü.
- Azure veritabanı geçiş hizmetini kullanarak, şirket içi kaynak sunucular için siteden siteye bağlantı sağlayan Azure Resource Manager dağıtım modeli kullanılarak oluşturulan bir VNET gerektirir [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [ VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Tamamlanan makalesinde açıklandığı gibi veri geçiş Yardımcısı'nı kullanarak şirket içi veritabanını ve şemayı geçişiniz değerlendirmesini [ bir SQL Server Geçiş değerlendirmesi gerçekleştirme](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem)
- Karşıdan yükleyip AzureRM.DataMigration modül kullanarak PowerShell Galerisi bu [yükleme modülü PowerShell cmdlet'i](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1)
- Kaynak SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerine sahip olmalısınız [denetim SUNUCUSUNA](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) izni.
- Hedef Azure SQL DB örneğine bağlanmak için kullanılan kimlik bilgilerini hedef Azure SQL veritabanı veritabanlarına denetim veritabanı izni olmalıdır.
- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Microsoft Azure aboneliğiniz için oturum açın
Makaledeki yönergeleri kullanın [oturum oturum Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-4.4.1) PowerShell kullanarak Azure aboneliğinizde oturum açmak için.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir sanal makine oluşturmadan önce bir kaynak grubu oluşturun.

Kullanarak bir kaynak grubu oluşturma [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1) komutu. 

Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroup* içinde *EastUS* bölge.

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```
## <a name="create-an-azure-database-migration-service-instance"></a>Bir Azure veritabanı geçiş hizmet örneği oluşturma 
Azure veritabanı geçiş hizmeti yeni bir örneğini kullanarak oluşturabileceğiniz `New-AzureRmDataMigrationService` cmdlet'i. Bu cmdlet'i aşağıdaki gerekli parametreleri bekler:
- *Azure kaynak grubu adı*. Kullanabileceğiniz [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1) daha önce gösterilen Azure kaynak grubu oluşturun ve adını bir parametre olarak sağlamak için komutu.
- *Hizmet adı*. Azure veritabanı geçiş hizmeti için istenen benzersiz bir hizmet adı için karşılık gelen dize 
- *Konum*. Hizmet konumunu belirtir. Batı ABD veya Güneydoğu Asya gibi bir Azure veri merkezi konum belirtin
- *SKU*. Bu parametre DMS Sku adına karşılık gelir. Şu anda desteklenen Sku adları *Basic_1vCore*, *Basic_2vCores*, *GeneralPurpose_4vCores*
- *Sanal alt ağ tanımlayıcısı*. Cmdlet'ini kullanabilirsiniz [yeni AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?view=azurermps-4.4.1) bir alt ağ oluşturmak için. 

Aşağıdaki örnek adlı bir hizmet oluşturduğu *MyDMS* kaynak grubunda *MyDMSResourceGroup*, bulunan *Doğu ABD* bir sanal alt ağ kullanarak bölge adı *MySubnet*.

```powershell
$service = New-AzureRmDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId
$vnet.Id`
```

## <a name="create-a-migration-project"></a>Bir geçiş projesi oluşturma
Azure veritabanı geçiş hizmeti örneğini oluşturduktan sonra bir geçiş projesi oluşturun. Azure veritabanı geçiş hizmeti projesinde, hem kaynak ve hedef örneklerin yanı sıra, projenin bir parçası geçirmek istediğiniz veritabanlarının listesini için bağlantı bilgilerini gerektirir.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Kaynak ve hedef bağlantıları için veritabanı bağlantı bilgisi nesnesi oluşturun
Kullanarak bir veritabanı bağlantı bilgisi nesnesi oluşturabilirsiniz `New-AzureRmDmsConnInfo` cmdlet'i.  Bu cmdlet'i aşağıdaki parametreleri beklemektedir:
- *Sunucu türü*. Veritabanı bağlantı türünü, örneğin, SQL, Oracle veya MySQL istedi. SQL, SQL Server ve SQL Azure için kullanın.
- *Veri kaynağı*. Adı veya IP bir SQL örneğine veya SQL Azure sunucusu.
- *Kimlik*. SqlAuthentication veya WindowsAuthentication bağlantısı için kimlik doğrulama türü.
- *TrustServerCertificate* parametresi kanal güven doğrulamak için sertifika zinciri taramasını atlayarak sırasında şifreli olup olmadığını belirten bir değer ayarlar. Değer true veya false olabilir.

Aşağıdaki örnek, kaynak sql kimlik doğrulaması kullanarak MySQLSourceServer adlı SQL Server için bağlantı bilgileri nesnesi oluşturur. 

```powershell
$sourceConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource MySQLSourceServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

Sonraki örnek sql kimlik doğrulaması kullanarak MySQLAzureTarget adlı SQL Azure veritabanı sunucusu için bağlantı bilgileri oluşturulmasını gösterir

```powershell
$targetConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource "mysqlazuretarget.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Veritabanları için geçiş proje sağlar
Bir liste oluşturur `AzureRmDataMigrationDatabaseInfo` veritabanları Azure veritabanı geçiş işleminin parçası olarak proje belirtir nesneleri sağlanan proje oluşturma için parametre olarak. Cmdlet `New-AzureRmDataMigrationDatabaseInfo` AzureRmDataMigrationDatabaseInfo oluşturmak için kullanılabilir. 

Aşağıdaki örnekte `AzureRmDataMigrationDatabaseInfo` AdventureWorks2016 veritabanı için proje ve proje oluşturma için parametre olarak sağlanacak listesine ekler.

```powershell
$dbInfo1 = New-AzureRmDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks2016
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Bir proje nesnesi oluşturun
Son olarak adlı Azure veritabanı geçiş proje oluşturabilirsiniz *MyDMSProject* bulunan *Doğu ABD* kullanarak `New-AzureRmDataMigrationProject` ve önceden oluşturulmuş kaynak ve hedef bağlantıları ve listesi ekleme geçirmek için veritabanları.

```powershell
$project = New-AzureRmDataMigrationProject -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName MyDMSProject `
  -Location EastUS `
  -SourceType SQL `
  -TargetType SQLDB `
  -SourceConnection $sourceConnInfo `
  -TargetConnection $targetConnInfo `
  -DatabaseInfos $dbList
```

## <a name="create-and-start-a-migration-task"></a>Oluşturma ve bir geçiş görev başlatma
Son olarak, oluşturun ve Azure veritabanı geçiş görevini Başlat. Azure veritabanı geçiş görev, kaynak ve hedef ve bir önkoşul olarak oluşturulan proje ile zaten sağlanan bilgilere ek olarak geçirilecek veritabanı tabloları listesi için bağlantı kimlik bilgileri gerektirir. 

### <a name="create-credential-parameters-for-source-and-target"></a>Kaynak ve hedef için kimlik bilgisi parametrelerini oluşturmak
Bağlantı güvenlik kimlik bilgileri olarak oluşturulabilir bir [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) nesnesi. 

Aşağıdaki örnek oluşturulmasını gösterir *PSCredential* parola dize değişkenleri olarak sağlama hem kaynak hem de hedef bağlantılarında nesneleri *$sourcePassword* ve *$ HedefParolası*. 

```powershell
secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-a-table-map-and-select-source-and-target-parameters-for-migration"></a>Bir tablo eşleme ve geçiş için select kaynak ve hedef parametreleri oluşturmak
Geçirilecek hedef kaynağından tablolara, geçiş için gereken başka bir parametre eşleme. Geçiş için kaynak ve hedef tablolar arasında bir eşleme sağlar sözlük tablo oluşturun. Aşağıdaki örnek, İnsan Kaynakları şema AdventureWorks 2016 veritabanı için kaynak ve hedef tablolar arasında eşleme gösterilmektedir.

```powershell
$tableMap = New-Object 'system.collections.generic.dictionary[string,string]'
$tableMap.Add("HumanResources.Department", "HumanResources.Department")
$tableMap.Add("HumanResources.Employee","HumanResources.Employee")
$tableMap.Add("HumanResources.EmployeeDepartmentHistory","HumanResources.EmployeeDepartmentHistory")
$tableMap.Add("HumanResources.EmployeePayHistory","HumanResources.EmployeePayHistory")
$tableMap.Add("HumanResources.JobCandidate","HumanResources.JobCandidate")
$tableMap.Add("HumanResources.Shift","HumanResources.Shift")
```

Kaynak ve hedef veritabanları seçin ve bir parametre olarak kullanarak geçirmek için tablo eşlemesi sağlamak için sonraki adımdır `New-AzureRmDmsSqlServerSqlDbSelectedDB` aşağıdaki örnekte gösterildiği gibi cmdlet:

```powershell
$selectedDbs = New-AzureRmDmsSqlServerSqlDbSelectedDB -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -TableMap $tableMap
```

### <a name="create-and-start-a-migration-task"></a>Oluşturma ve bir geçiş görev başlatma

Kullanım `New-AzureRmDataMigrationTask` oluşturmak ve bir geçiş görevi başlatmak için cmdlet. Bu cmdlet'i aşağıdaki parametreleri beklemektedir:
- *TaskType*.  SQL Azure geçiş türü için SQL Server için oluşturmak için geçiş görevinin türü *MigrateSqlServerSqlDb* beklenir. 
- *Kaynak grubu adı*. Görevi oluşturmak üzere Azure kaynak grubunun adı.
- *ServiceName*.  Görevi oluşturmak üzere Azure veritabanı geçiş hizmeti örneği.
- *ProjectName*. Görevi oluşturmak üzere Azure veritabanı geçiş projesinin adı. 
- *Görevadı*. Oluşturulacak görev adı. 
- *Kaynak bağlantı*. Kaynak bağlantısı temsil eden AzureRmDmsConnInfo nesnesi.
- *Hedef bağlantı*. Hedef bağlantı temsil eden AzureRmDmsConnInfo nesnesi.
- *SourceCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) kaynak sunucuya bağlanmak için nesne.
- *TargetCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) hedef sunucuya bağlanmak için nesne.
- *SelectedDatabase*. Kaynak ve hedef veritabanı eşleme temsil eden AzureRmDmsSqlServerSqlDbSelectedDB nesnesi.

Aşağıdaki örnek, oluşturur ve myDMSTask adlı bir geçiş görev başlatır:

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

## <a name="monitor-the-migration"></a>Geçiş izleme
Aşağıdaki örnekte gösterildiği gibi görev durumu özelliği sorgulanarak çalıştıran geçiş görevi izleyebilirsiniz:

```powershell
if (($mytask.ProjectTask.Properties.State -eq "Running") -or ($mytask.ProjectTask.Properties.State -eq "Queued"))
{
  write-host "migration task running"
}
```

## <a name="next-steps"></a>Sonraki adımlar
- Microsoft Geçiş Kılavuzu gözden [veritabanı Geçiş Kılavuzu](https://datamigration.microsoft.com/).