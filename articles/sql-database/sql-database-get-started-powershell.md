---
title: "PowerShell ile yeni SQL Veritabanı kurulumu | Microsoft Belgeleri"
description: "PowerShell ile SQL veritabanı oluşturmayı hemen öğrenin. Genel veritabanı kurulum görevleri PowerShell cmdlet&quot;leri ile yönetilebilir."
keywords: "yeni sql veritabanı oluşturma,veritabanı kurulumu"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: 7d99869b-cec5-4583-8c1c-4c663f4afd4d
ms.service: sql-database
ms.custom: single databases
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 12/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 3ba16154857f8e7b59a1013b736d6131a4161185
ms.openlocfilehash: d00b7b543f105fd944e91f6ed27e6613366c6716


---

# <a name="get-started-with-azure-sql-database-servers-databases-and-firewall-rules-by-using-azure-powershell"></a>Azure PowerShell aracılığıyla Azure SQL Veritabanı sunucularını, veritabanlarını ve güvenlik duvarı kurallarını kullanmaya başlama

Bu kullanmaya başlama öğreticisinde, PowerShell'i kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

* Yeni bir Azure kaynak grubu oluşturma
* Azure SQL mantıksal sunucusu oluşturma
* Azure SQL sunucusu özelliklerini görüntüleme
* Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma
* AdventureWorksLT örnek veritabanını tek veritabanı olarak oluşturma
* AdventureWorksLT örnek veritabanı özelliklerini görüntüleme
* Boş bir tek veritabanı oluşturma

Bu öğreticide ayrıca şunları yapacaksınız:

* Mantıksal sunucuya ve asıl veritabanına bağlanma
* Asıl veritabanı özelliklerini görüntüleme
* Örnek veritabanına bağlanma
* Kullanıcı veritabanı özelliklerini görüntüleme

Bu öğreticiyi tamamladığınızda; Azure kaynak grubu içinde çalışan ve mantıksal sunucuya bağlı bir örnek veritabanınız ve bir de boş veritabanınız olur. Ayrıca, sunucu düzeyindeki sorumlunun, sunucuda belirtilen IP adresinden (veya IP adresi aralığından) oturum açabilmesini sağlamak için yapılandırılmış bir sunucu düzeyinde güvenlik duvarı kuralına da sahip olacaksınız. 

**Tahmini süre**: Bu öğretici yaklaşık 30 dakika sürer (önkoşulları karşıladığınız varsayılarak).


> [!TIP]
> Aynı görevleri, [Azure portalı](sql-database-get-started.md) aracılığıyla kullanmaya başlama öğreticilerinde de gerçekleştirebilirsiniz.
>


## <a name="prerequisites"></a>Ön koşullar

* Bir Azure hesabınız olmalıdır. [Ücretsiz bir Azure hesabı açabilir](/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abonelik avantajlarını etkinleştirebilirsiniz](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Abonelik sahibi veya katkıda bulunan rolü üyesi olan bir hesap kullanarak oturum açmış olmanız gerekir. Rol tabanlı erişim denetimi (RBAC) ile ilgili daha fazla bilgi için bkz. [Azure portalında erişim yönetimini kullanmaya başlama](../active-directory/role-based-access-control-what-is.md).

* Azure blob depolama alanında AdventureWorksLT örnek veritabanı .bacpac dosyasının bulunması gerekir

### <a name="download-the-adventureworkslt-sample-database-bacpac-file-and-save-it-in-azure-blob-storage"></a>AdventureWorksLT örnek veritabanı .bacpac dosyasını indirin ve Azure blob depolama alanına kaydedin

Bu öğreticide, Azure Depolama kaynağından bir .bacpac dosyası içeri aktarılarak yeni bir AdventureWorksLT veritabanı oluşturulmaktadır. Birinci adım, AdventureWorksLT.bacpac dosyasının bir kopyasını edinmek ve blob depolama alanına yüklemektir.
Aşağıdaki adımlar örnek veritabanını içeri aktarmaya hazır hale getirir:

1. [AdventureWorksLT.bacpac dosyasını indirin](https://sqldbbacpacs.blob.core.windows.net/bacpacs/AdventureWorksLT.bacpac) ve .bacpac dosya uzantısıyla kaydedin.
2. [Depolama hesabı oluşturun](../storage/storage-create-storage-account.md#create-a-storage-account) Depolama hesabını varsayılan ayarlarla oluşturabilirsiniz.
3. Yeni bir **Kapsayıcı** oluşturmak için depolama hesabına gidin, **Blob'lar** öğesini seçin ve ardından **+Kapsayıcı**'ya tıklayın.
4. .bacpac dosyasını depolama hesabınızdaki blob kapsayıcısına yükleyin. Kapsayıcı sayfasının en üstündeki **Yükle** düğmesine tıklayabilir veya [AzCopy kullanabilirsiniz](../storage/storage-use-azcopy.md#blob-upload). 
5. AdventureWorksLT.bacpac dosyasını kaydettikten sonra, kod parçacığını bu öğreticinin sonraki adımlarında içeri aktarmak üzere URL'yi ve depolama hesabı anahtarını not etmeniz gerekir. 
   * bacpac dosyanızı seçin ve URL'yi kopyalayın. Şuna benzer olacaktır: https://{depolama-hesabı-adı}.blob.core.windows.net/{kapsayıcı-adı}/AdventureWorksLT.bacpac. Depolama hesabı sayfasında **Erişim anahtarları**'na tıklayın ve **key1** değerini kopyalayın.


[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]


## <a name="create-a-new-logical-sql-server-using-azure-powershell"></a>Azure PowerShell kullanarak yeni bir mantıksal SQL sunucusu oluşturma

Sunucuyu içerecek yeni bir kaynak grubuna ihtiyacınız olduğundan ilk adım yeni bir kaynak grubu veya sunucu oluşturmak ([New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.3.0/new-azurermresourcegroup), [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqlserver)) veya var olanlara başvurmaktır ([Get-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.3.0/get-azurermresourcegroup), [Get-AzureRmSqlServer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqlserver)).
Aşağıdaki kod parçacıkları, mevcut değilse bir kaynak grubu ve Azure SQL sunucusu oluşturur:

Geçerli Azure konumlarının listesi ve dize biçimi için aşağıdaki [Yardımcı kod parçacıkları](#helper-snippets) bölümüne bakın.
```
# Create new, or get existing resource group
############################################

$resourceGroupName = "{resource-group-name}"
$resourceGroupLocation = "{resource-group-location}"

$myResourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ea SilentlyContinue

if(!$myResourceGroup)
{
   Write-Output "Creating resource group: $resourceGroupName"
   $myResourceGroup = New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else
{
   Write-Output "Resource group $resourceGroupName already exists:"
}
$myResourceGroup



# Create a new, or get existing server
######################################

$serverName = "{server-name}"
$serverVersion = "12.0"
$serverLocation = $resourceGroupLocation
$serverResourceGroupName = $resourceGroupName

$serverAdmin = "{server-admin}"
$serverAdminPassword = "{server-admin-password}"

$securePassword = ConvertTo-SecureString –String $serverAdminPassword –AsPlainText -Force
$serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue
if(!$myServer)
{
   Write-Output "Creating SQL server: $serverName"
   $myServer = New-AzureRmSqlServer -ResourceGroupName $serverResourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
}
else
{
   Write-Output "SQL server $serverName already exists:"
}
$myServer

```


## <a name="view-the-logical-sql-server-properties-using-azure-powershell"></a>Azure PowerShell kullanarak mantıksal SQL sunucusunun özelliklerini görüntüleme

```
#$serverResourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName 

Write-Host "Server name: " $myServer.ServerName
Write-Host "Fully qualified server name: $serverName.database.windows.net"
Write-Host "Server location: " $myServer.Location
Write-Host "Server version: " $myServer.ServerVersion
Write-Host "Server administrator login: " $myServer.SqlAdministratorLogin
```


## <a name="create-a-server-level-firewall-rule-using-azure-powershell"></a>Azure PowerShell kullanarak sunucu düzeyi güvenlik duvarı oluşturma

Güvenlik duvarı kuralını ayarlamak için genel IP adresinizi bilmeniz gerekir. IP adresinizi bulmak için istediğiniz tarayıcıyı kullanabilirsiniz ("IP adresim nedir" aratın). Ayrıntılar için bkz. [güvenlik duvarı kuralları](sql-database-firewall-configure.md).

Aşağıdaki komutlarda başvurmak veya yeni bir kural oluşturmak için [Get-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqlserverfirewallrule) ve [New-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqlserverfirewallrule) cmdlet'leri kullanılmaktadır. Bu kod parçacığında kural zaten varsa yalnızca başvuru oluşturur, başlangıç ve bitiş IP adreslerini güncelleştirmez. Dilediğiniz zaman **else** yan tümcesini değiştirerek [Set-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/set-azurermsqlserverfirewallrule) cmdlet'ini oluşturma veya güncelleştirme işlevini kullanabilirsiniz.

> [!NOTE] 
> Sunucudaki SQL Veritabanı güvenlik duvarını, tek bir IP adresi veya bir adresler aralığının tamamı üzerinde açabilirsiniz. Güvenlik duvarını açmak SQL yönetici ve kullanıcılarının; sunucuda, geçerli kimlik bilgilerine sahip oldukları herhangi bir veritabanında oturum açmasını sağlar.
>

```
#$serverName = "{server-name}"
#$serverResourceGroupName = "{resource-group-name}"
$serverFirewallRuleName = "{server-firewall-rule-name}"
$serverFirewallStartIp = "{server-firewall-rule-startIp}"
$serverFirewallEndIp = "{server-firewall-rule-endIp}"

$myFirewallRule = Get-AzureRmSqlServerFirewallRule -FirewallRuleName $serverFirewallRuleName -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue

if(!$myFirewallRule)
{
   Write-host "Creating server firewall rule: $serverFirewallRuleName"
   $myFirewallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $serverResourceGroupName -ServerName $serverName -FirewallRuleName $serverFirewallRuleName -StartIpAddress $serverFirewallStartIp -EndIpAddress $serverFirewallEndIp
}
else
{
   Write-host "Server firewall rule $serverFirewallRuleName already exists:"
}
$myFirewallRule
```


## <a name="connect-to-sql-server-using-azure-powershell"></a>Azure PowerShell kullanarak SQL sunucusuna bağlanma

Şimdi sunucuyla bağlantı kurabildiğimizi doğrulamak için ana veritabanında kısa bir sorgu çalıştıralım. Aşağıdaki kod parçacığı sunucunun ana veritabanına bağlanmak ve sorgu göndermek için [.NET Framework Provider for SQL Server (System.Data.SqlClient)](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx) kullanır. Önceki kod parçacıklarında kullandığımız değişkenleri içeren bir bağlantı dizesi oluşturur. Yer tutucuları önceki adımlarda sunucuyu oluşturmak için kullandığınız SQL sunucusu yöneticisi ve parolasıyla değiştirin.


```
#$serverName = "{server-name}"
#$serverAdmin = "{server-admin}"
#$serverAdminPassword = "{server-admin-password}"
$databaseName = "master"

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=$serverAdmin;Password=$serverAdminPassword" + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"



$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()


$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()
```


## <a name="create-new-adventureworkslt-sample-database-using-azure-powershell"></a>Azure PowerShell kullanarak yeni AdventureWorksLT örnek veritabanı oluşturma

Aşağıdaki kod parçacığı [New-AzureRmSqlDatabaseImport](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqldatabaseimport) cmdlet'ini kullanarak AdventureWorksLT örnek veritabanının bir bacpac dosyasını içeri aktarır. bacpac dosyası, Azure blob depolama alanında bulunur. İçeri aktarma cmdlet'ini çalıştırdıktan sonra [Get-AzureRmSqlDatabaseImportExportStatus](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqldatabaseimportexportstatus) cmdlet'ini kullanarak içeri aktarma işleminin ilerleme durumunu izleyebilirsiniz.
$storageUri, önceden portala yüklediğiniz bacpac dosyasının URL özelliğidir ve şuna benzer olmalıdır: https://{depolama-hesabı}.blob.core.windows.net/{kapsayıcı}/AdventureWorksLT.bacpac

```
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"

$databaseName = "AdventureWorksLT"
$databaseEdition = "Basic"
$databaseServiceLevel = "Basic"

$storageKeyType = "StorageAccessKey"
$storageUri = "{storage-uri}" # URL of bacpac file you uploaded to your storage account
$storageKey = "{storage-key}" # key1 in the Access keys setting of your storage account

$importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $resourceGroupName –ServerName $serverName –DatabaseName $databaseName –StorageKeytype $storageKeyType –StorageKey $storageKey -StorageUri $storageUri –AdministratorLogin $serverAdmin –AdministratorLoginPassword $securePassword –Edition $databaseEdition –ServiceObjectiveName $databaseServiceLevel -DatabaseMaxSizeBytes 5000000


Do {
     $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
     Write-host "Importing database..." $importStatus.StatusMessage
     Start-Sleep -Seconds 30
     $importStatus.Status
   }
   until ($importStatus.Status -eq "Succeeded")
$importStatus
```



## <a name="view-database-properties-using-azure-powershell"></a>Azure PowerShell kullanarak veritabanı özelliklerini görüntüleme

Veritabanını oluşturduktan sonra bazı özelliklerini görüntüleyin.

```
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
#$databaseName = "{database-name}"

$myDatabase = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName


Write-Host "Database name: " $myDatabase.DatabaseName
Write-Host "Server name: " $myDatabase.ServerName
Write-Host "Creation date: " $myDatabase.CreationDate
Write-Host "Database edition: " $myDatabase.Edition
Write-Host "Database performance level: " $myDatabase.CurrentServiceObjectiveName
Write-Host "Database status: " $myDatabase.Status
```

## <a name="connect-and-query-the-sample-database-using-azure-powershell"></a>Azure PowerShell kullanarak örnek veritabanına bağlanma ve sorgu gönderme

Şimdi bağlantı kurabildiğimizi doğrulamak için AdventureWorksLT veritabanında kısa bir sorgu çalıştıralım. Aşağıdaki kod parçacığı veritabanına bağlanmak ve sorgu göndermek için [.NET Framework Provider for SQL Server (System.Data.SqlClient)](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx) kullanır. Önceki kod parçacıklarında kullandığımız değişkenleri içeren bir bağlantı dizesi oluşturur. Yer tutucusunu SQL sunucusunun yönetici parolasıyla değiştirin.

```
#$serverName = {server-name}
#$serverAdmin = "{server-admin}"
#$serverAdminPassword = "{server-admin-password}"
#$databaseName = {database-name}

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=$serverAdmin;Password=$serverAdminPassword" + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"


$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()


$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()
```

## <a name="create-a-new-blank-database-using-azure-powershell"></a>Azure PowerShell kullanarak yeni boş bir veritabanı oluşturma

```
#$resourceGroupName = {resource-group-name}
#$serverName = {server-name}

$blankDatabaseName = "blankdb"
$blankDatabaseEdition = "Basic"
$blankDatabaseServiceLevel = "Basic"


$myBlankDatabase = New-AzureRmSqlDatabase -DatabaseName $blankDatabaseName -ServerName $serverName -ResourceGroupName $resourceGroupName -Edition $blankDatabaseEdition -RequestedServiceObjectiveName $blankDatabaseServiceLevel
$myBlankDatabase
```


## <a name="complete-azure-powershell-script-to-create-a-server-firewall-rule-and-database"></a>Sunucu, güvenlik duvarı kuralı ve veritabanı oluşturmak için tam Azure PowerShell betiği



```
# Sign in to Azure and set the subscription to work with
########################################################

$SubscriptionId = "{subscription-id}"

Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId

# User variables
################

$myResourceGroupName = "{resource-group-name}"
$myResourceGroupLocation = "{resource-group-location}"

$myServerName = "{server-name}"
$myServerVersion = "12.0"
$myServerLocation = $myResourceGroupLocation
$myServerResourceGroupName = $myResourceGroupName
$myServerAdmin = "{server-admin}"
$myServerAdminPassword = "{server-admin-password}" 

$myServerFirewallRuleName = "{server-firewall-rule-name}"
$myServerFirewallStartIp = "{start-ip}"
$myServerFirewallEndIp = "{end-ip}"

$myDatabaseName = "AdventureWorksLT"
$myDatabaseEdition = "Basic"
$myDatabaseServiceLevel = "Basic"

$myStorageKeyType = "StorageAccessKey"
# Get these values from your Azure storage account:
$myStorageUri = "{http://your-storage-account.blob.core.windows.net/your-container/AdventureWorksLT.bacpac}"
$myStorageKey = "{your-storage-key}"


# Create new, or get existing resource group
############################################

$resourceGroupName = $myResourceGroupName
$resourceGroupLocation = $myResourceGroupLocation

$myResourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ea SilentlyContinue

if(!$myResourceGroup)
{
   Write-host "Creating resource group: $resourceGroupName"
   $myResourceGroup = New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else
{
   Write-host "Resource group $resourceGroupName already exists:"
}
$myResourceGroup


# Create a new, or get existing server
######################################

$serverName = $myServerName
$serverVersion = $myServerVersion
$serverLocation = $myServerLocation
$serverResourceGroupName = $myServerResourceGroupName

$serverAdmin = $myServerAdmin
$serverAdminPassword = $myServerAdminPassword

$securePassword = ConvertTo-SecureString –String $serverAdminPassword –AsPlainText -Force
$serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue
if(!$myServer)
{
   Write-host "Creating SQL server: $serverName"
   $myServer = New-AzureRmSqlServer -ResourceGroupName $serverResourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
}
else
{
   Write-host "SQL server $serverName already exists:"
}
$myServer


# View server properties
##########################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName 

Write-Host "Server name: " $myServer.ServerName
Write-Host "Fully qualified server name: $serverName.database.windows.net"
Write-Host "Server location: " $myServer.Location
Write-Host "Server version: " $myServer.ServerVersion
Write-Host "Server administrator login: " $myServer.SqlAdministratorLogin


# Create or update server firewall rule
#######################################

$serverFirewallRuleName = $myServerFirewallRuleName
$serverFirewallStartIp = $myServerFirewallStartIp
$serverFirewallEndIp = $myServerFirewallEndIp

$myFirewallRule = Get-AzureRmSqlServerFirewallRule -FirewallRuleName $serverFirewallRuleName -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue

if(!$myFirewallRule)
{
   Write-host "Creating server firewall rule: $serverFirewallRuleName"
   $myFirewallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $serverResourceGroupName -ServerName $serverName -FirewallRuleName $serverFirewallRuleName -StartIpAddress $serverFirewallStartIp -EndIpAddress $serverFirewallEndIp
}
else
{
   Write-host "Server firewall rule $serverFirewallRuleName already exists:"
}
$myFirewallRule


# Connect to the server and master database
###########################################
$databaseName = "master"

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=" + $myServer.SqlAdministratorLogin + ";Password=" + $myServerAdminPassword + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"


$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()

$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()


# Create the AdventureWorksLT database from a bacpac
####################################################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName

$databaseName = $myDatabaseName
$databaseEdition = $myDatabaseEdition
$databaseServiceLevel = $myDatabaseServiceLevel

$storageKeyType = $myStorageKeyType
$storageUri = $myStorageUri
$storageKey = $myStorageKey

$importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $resourceGroupName –ServerName $serverName –DatabaseName $databaseName –StorageKeytype $storageKeyType –StorageKey $storageKey -StorageUri $storageUri –AdministratorLogin $serverAdmin –AdministratorLoginPassword $securePassword –Edition $databaseEdition –ServiceObjectiveName $databaseServiceLevel -DatabaseMaxSizeBytes 5000000

Do {
     $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
     Write-host "Importing database..." $importStatus.StatusMessage
     Start-Sleep -Seconds 30
     $importStatus.Status
   }
   until ($importStatus.Status -eq "Succeeded")
$importStatus


# View database properties
##########################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$databaseName = $myDatabaseName

$myDatabase = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName

Write-Host "Database name: " $myDatabase.DatabaseName
Write-Host "Server name: " $myDatabase.ServerName
Write-Host "Creation date: " $myDatabase.CreationDate
Write-Host "Database edition: " $myDatabase.Edition
Write-Host "Database performance level: " $myDatabase.CurrentServiceObjectiveName
Write-Host "Database status: " $myDatabase.Status


# Query the database
####################

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=" + $myServer.SqlAdministratorLogin + ";Password=" + $myServerAdminPassword + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"

$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()

$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()


# Create a blank database
#########################

$blankDatabaseName = "blankdb"
$blankDatabaseEdition = "Basic"
$blankDatabaseServiceLevel = "Basic"


$myBlankDatabase = New-AzureRmSqlDatabase -DatabaseName $blankDatabaseName -ServerName $serverName -ResourceGroupName $resourceGroupName -Edition $blankDatabaseEdition -RequestedServiceObjectiveName $blankDatabaseServiceLevel
$myBlankDatabase
```



> [!TIP]
> Öğrenirken, kullanmadığınız veritabanlarını silerek maliyet tasarrufu yapabilirsiniz. Temel sürüm veritabanlarını 7 gün içinde geri yükleyebilirsiniz. Ancak sunucuyu silmeyin. Bunu yapmanız durumunda, sunucuyu ve silinen veritabanlarının hiçbirini kurtaramazsınız.

## <a name="helper-snippets"></a>Yardımcı kod parçacıkları

```
# Get a list of Azure regions where you can create SQL resources

$sqlRegions = (Get-AzureRmLocation | Where-Object { $_.Providers -eq "Microsoft.Sql" })
foreach ($region in $sqlRegions)
{
   $region.Location
}

# Clean up
# Delete a resource group (and all contained resources)
Remove-AzureRmResourceGroup -Name {resource-group-name}
```

> [!TIP]
> Öğrenirken, kullanmadığınız veritabanlarını silerek maliyet tasarrufu yapabilirsiniz. Temel sürüm veritabanlarını yedi gün içinde geri yükleyebilirsiniz. Ancak sunucuyu silmeyin. Bunu yapmanız durumunda, sunucuyu ve silinen veritabanlarının hiçbirini kurtaramazsınız.
>

## <a name="next-steps"></a>Sonraki adımlar
Bu ilk kullanmaya başlama öğreticisini tamamladığınıza ve örnek veriler içeren bir veritabanı oluşturduğunuza göre, bu öğreticide öğrendiklerinizi geliştirecek ek öğreticilerden birini keşfetmek isteyebilirsiniz. 

* Azure SQL Veritabanı güvenliğini keşfetmeye başlamak isterseniz bkz. [Güvenliğe giriş](sql-database-get-started-security.md).
* Excel kullanmayı biliyorsanız [Excel ile Azure’da SQL veritabanına bağlanma](sql-database-connect-excel.md) işlemini nasıl gerçekleştireceğinizi öğrenin.
* Kodlamaya başlamak için hazırsanız [SQL Veritabanı ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md) konu başlığına göz atarak programlama dilinizi belirleyin.
* Şirket içi SQL Server veritabanlarınızı Azure’a taşımak istiyorsanız, bkz. [Bir veritabanını SQL Veritabanı’na geçirme](sql-database-cloud-migrate.md).
* BCP komut satırı aracını kullanarak yeni bir tabloya bir CSV dosyasından bazı veriler yüklemek istiyorsanız bkz. [BCP kullanarak bir CSV dosyasından SQL Veritabanına veri yükleme](sql-database-load-from-csv-with-bcp.md).
* Tabloları ve diğer nesneleri oluşturmaya başlamak istiyorsanız, [Tablo oluşturma](https://msdn.microsoft.com/library/ms365315.aspx) içindeki “Tablo oluşturmak için” konusuna bakın.

## <a name="additional-resources"></a>Ek kaynaklar
[SQL Database nedir?](sql-database-technical-overview.md)


<!--HONumber=Jan17_HO1-->


