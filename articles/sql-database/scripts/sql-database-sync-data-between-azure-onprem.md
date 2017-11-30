---
title: "Şirket içi PowerShell örnek-eşitleme SQL Database ve SQL Server arasında | Microsoft Docs"
description: "Bir Azure SQL Database ve SQL Server içi veritabanı arasında eşitleme için azure PowerShell örnek betiği"
services: sql-database
documentationcenter: sql-database
author: jognanay
manager: craigg
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 07/31/2017
ms.author: jognanay
ms.reviewer: douglasl
ms.openlocfilehash: 524193835bd5d350d6ac312fcc997f6c135abea1
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="use-powershell-to-sync-between-a-sql-database-and-a-sql-server-on-premises-database"></a>SQL Database ve SQL Server içi veritabanı arasında eşitlemek için PowerShell kullanma

Bu PowerShell örnek bir Azure SQL Database ve SQL Server içi veritabanı arasında eşitlemek için veri eşitleme yapılandırır. 

Bu örnek, Azure PowerShell modülü 4.2 veya sonraki bir sürümü gerektiriyor. Çalıştırma `Get-Module -ListAvailable AzureRM` yüklü olan sürümü bulunamıyor. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).
 
Çalıştırma `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için.

SQL veri eşitleme genel bakış için bkz: [verileri Eşitle birden çok Bulut ve şirket içi veritabanları arasında Azure SQL veri eşitleme (Önizleme) ile](../sql-database-sync-data.md).

## <a name="sample-script"></a>Örnek komut dosyası

```powershell
# prerequisites: 
# 1. Create an Azure Database from AdventureWorksLT sample database as hub database
# 2. Create an Azure Database in the same region as sync database
# 3. Create an on premises SQL Server Database as member database
# 4. Update the parameters below before running the sample
#
using namespace Microsoft.Azure.Commands.Sql.DataSync.Model
using namespace System.Collections.Generic

# Hub database info
# Subscription id for hub database
$SubscriptionId = "subscription_guid"
# Resource group name for hub database
$ResourceGroupName = "ResourceGroupName"
# Server name for hub database
$ServerName = "ServerName"
# Database name for hub database
$DatabaseName = "TestHubDatabase"

# Sync database info
# Resource group name for sync database
$SyncDatabaseResourceGroupName = "ResourceGroupName"
# Server name for sync database
$SyncDatabaseServerName = "ServerName"
# Sync database name
$SyncDatabaseName = "SyncDatabaseName"

# Sync group info
# Sync group name
$SyncGroupName = "TestSyncGroup"
# Conflict resolution Policy. Value can be HubWin or MemberWin
$ConflictResolutionPolicy = "HubWin"
# Sync interval in seconds. Value must be no less than 300
$IntervalInSeconds = 300

# Member database info
# Member name
$SyncMemberName = "member"
# Member server name
$MemberServerName = "OnPremiseServer"
# Member database name
$MemberDatabaseName = "MemberDatabaseTest"
# Member database type. Value can be AzureSqlDatabase or SqlServerDatabase
$MemberDatabaseType = "SqlServerDatabase"
# Sync direction. Value can be Bidirectional, Onewaymembertohub, Onewayhubtomember
$SyncDirection = "Bidirectional"

#Sync Agent Info
$SyncAgentName = "TestSyncAgent"
$SyncAgentResourceGroupName = "ResourceGroupName"
$SyncAgentServerName = "ServerName"

# Other info
# Temp file to save the sync schema
$TempFile = $env:TEMP+"\syncSchema.json"

# List of included columns and tables in quoted name
$IncludedColumnsAndTables =  "[SalesLT].[Address].[AddressID]",
                             "[SalesLT].[Address].[AddressLine2]",
                             "[SalesLT].[Address].[rowguid]",
                             "[SalesLT].[Address].[PostalCode]",
                             "[SalesLT].[ProductDescription]"
$MetadataList = [System.Collections.ArrayList]::new($IncludedColumnsAndTables)


add-azurermaccount 
select-azurermsubscription -SubscriptionId $SubscriptionId

# Use this section if it is safe to show password in the script.
# Otherwise, use the PromptForCredential
# $User = "username"
# $PWord = ConvertTo-SecureString -String "password" -AsPlainText -Force
# $Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $User, $PWord

$Credential = $Host.ui.PromptForCredential("Need credential", 
              "Please enter your user name and password for server "+$ServerName+".database.windows.net", 
              "", 
              "")

 #Create a new Sync Agent
 Write-Host "Creating new Sync Agent "
 New-AzureRmSqlSyncAgent -ResourceGroupName $ResourceGroupName  `
                               -ServerName  $ServerName  `
                               -SyncDatabaseName $SyncDatabaseName `
                              -SyncAgentName $SyncAgentName


#Generate Agent Key
Write-Host "Generating Agent Key"
$AgentKey = New-AzureRmSqlSyncAgentKey -ResourceGroupName $ResourceGroupName   `
                              -ServerName  $ServerName  `
                              -SyncAgentName $SyncAgentName
 Write-Host "Use your agent key to configure the sync agent. Do this before proceeding"
$agentkey
                  
#DO THE FOLLOWING BEFORE THE NEXT STEP
#Install the on-premises sync agent on your machine and register the sync agent using the agent key generated above to bring the sync agent online.
#Add the SQL server database information including server name, database name, user name, password on the configuration tool within the sync agent.  


# Create a new sync group
Write-Host "Creating Sync Group"$SyncGroupName
New-AzureRmSqlSyncGroup   -ResourceGroupName $ResourceGroupName `
                            -ServerName $ServerName `
                            -DatabaseName $DatabaseName `
                            -Name $SyncGroupName `
                            -SyncDatabaseName $SyncDatabaseName `
                            -SyncDatabaseServerName $SyncDatabaseServerName `
                            -SyncDatabaseResourceGroupName $SyncDatabaseResourceGroupName `
                            -ConflictResolutionPolicy $ConflictResolutionPolicy `
                            -DatabaseCredential $Credential

# Use this section if it is safe to show password in the script.
#$User = "username"
#$Password = ConvertTo-SecureString -String "password" -AsPlainText -Force
#$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $User, $Password

$Credential = $Host.ui.PromptForCredential("Need credential", 
              "Please enter your user name and password for server "+$MemberServerName, 
              "", 
              "")

#Get Information from sync Agent 
#Confirm that your SQL Server was configured
#Note the Database ID, you will use this as SqlServerDatabaseID for the next step.
$SyncAgentInfo = Get-AzureRmSqlSyncAgentLinkedDatabase -ResourceGroupName $ResourceGroupName   `
                              -ServerName  $ServerName  `
                              -SyncAgentName $SyncAgentName

# Add a new sync member
Write-Host "Adding member"$SyncMemberName" to the sync group"



New-AzureRmSqlSyncMember -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -SyncGroupName $SyncGroupName -Name $SyncMemberName -MemberDatabaseType $MemberDatabaseType -SyncAgentResourceGroupName SyncAgentResourceGroupName -SyncAgentServerName $SyncAgentServerName -SyncAgentName $SyncAgentName  -SyncDirection $SyncDirection -SqlServerDatabaseID  $SyncAgentInfo.DatabaseId

# Refresh database schema from hub database
# Specify the -SyncMemberName parameter if you want to refresh schema from the member database
Write-Host "Refreshing database schema from hub database"
$StartTime= Get-Date
Update-AzureRmSqlSyncSchema   -ResourceGroupName $ResourceGroupName `
                              -ServerName $ServerName `
                              -DatabaseName $DatabaseName `
                              -SyncGroupName $SyncGroupName


#Waiting for successful refresh

$StartTime=$StartTime.ToUniversalTime()
$timer=0
$timeout=90
# Check the log and see if refresh has gone through
Write-Host "Check for successful refresh"
$IsSucceeded = $false
While ($IsSucceeded -eq $false)
{
    Start-Sleep -s 10
    $timer=$timer+10
    $Details = Get-AzureRmSqlSyncSchema -SyncGroupName $SyncGroupName -ServerName $ServerName -DatabaseName $DatabaseName -ResourceGroupName $ResourceGroupName
    if ($Details.LastUpdateTime -gt $StartTime)
      {
        Write-Host "Refresh was successful"
        $IsSucceeded = $true
      }
    if ($timer -eq $timeout) 
      {
              Write-Host "Refresh timed out"
        break;
      }
}


# Get the database schema 
Write-Host "Adding tables and columns to the sync schema"
$databaseSchema = Get-AzureRmSqlSyncSchema   -ResourceGroupName $ResourceGroupName `
                                             -ServerName $ServerName `
                                             -DatabaseName $DatabaseName `
                                             -SyncGroupName $SyncGroupName `

$databaseSchema | ConvertTo-Json -depth 5 -Compress | Out-File "C:\Users\OnPremiseServer\AppData\Local\Temp\syncSchema.json"     
$newSchema = [AzureSqlSyncGroupSchemaModel]::new()
$newSchema.Tables = [List[AzureSqlSyncGroupSchemaTableModel]]::new();

# Add columns and tables to the sync schema
foreach ($tableSchema in $databaseSchema.Tables)
{
    $newTableSchema = [AzureSqlSyncGroupSchemaTableModel]::new()
    $newTableSchema.QuotedName = $tableSchema.QuotedName
    $newTableSchema.Columns = [List[AzureSqlSyncGroupSchemaColumnModel]]::new();
    $addAllColumns = $false
    if ($MetadataList.Contains($tableSchema.QuotedName))
    {
        if ($tableSchema.HasError)
        {
            $fullTableName = $tableSchema.QuotedName
            Write-Host "Can't add table $fullTableName to the sync schema" -foregroundcolor "Red"
            Write-Host $tableSchema.ErrorId -foregroundcolor "Red"
            continue;
        }
        else
        {
            $addAllColumns = $true
        }
    }
    foreach($columnSchema in $tableSchema.Columns)
    {
        $fullColumnName = $tableSchema.QuotedName + "." + $columnSchema.QuotedName
        if ($addAllColumns -or $MetadataList.Contains($fullColumnName))
        {
            if ((-not $addAllColumns) -and $tableSchema.HasError)
            {
                Write-Host "Can't add column $fullColumnName to the sync schema" -foregroundcolor "Red"
                Write-Host $tableSchema.ErrorId -foregroundcolor "Red"
            }
            elseif ((-not $addAllColumns) -and $columnSchema.HasError)
            {
                Write-Host "Can't add column $fullColumnName to the sync schema" -foregroundcolor "Red"
                Write-Host $columnSchema.ErrorId -foregroundcolor "Red"
            }
            else
            {
                Write-Host "Adding"$fullColumnName" to the sync schema"
                $newColumnSchema = [AzureSqlSyncGroupSchemaColumnModel]::new()
                $newColumnSchema.QuotedName = $columnSchema.QuotedName
                $newColumnSchema.DataSize = $columnSchema.DataSize
                $newColumnSchema.DataType = $columnSchema.DataType
                $newTableSchema.Columns.Add($newColumnSchema)
            }
        }
    }
    if ($newTableSchema.Columns.Count -gt 0)
    {
        $newSchema.Tables.Add($newTableSchema)
    }
}

# Convert sync schema to Json format
$schemaString = $newSchema | ConvertTo-Json -depth 5 -Compress

# workaround a powershell bug
$schemaString = $schemaString.Replace('"Tables"', '"tables"').Replace('"Columns"', '"columns"').Replace('"QuotedName"', '"quotedName"').Replace('"MasterSyncMemberName"','"masterSyncMemberName"')

# Save the sync schema to a temp file
$schemaString | Out-File $TempFile

# Update sync schema
Write-Host "Updating the sync schema"
Update-AzureRmSqlSyncGroup  -ResourceGroupName $ResourceGroupName `
                            -ServerName $ServerName `
                            -DatabaseName $DatabaseName `
                            -Name $SyncGroupName `
                            -Schema $TempFile

$SyncLogStartTime = Get-Date

# Trigger sync manually
Write-Host "Trigger sync manually"
Start-AzureRmSqlSyncGroupSync  -ResourceGroupName $ResourceGroupName `
                               -ServerName $ServerName `
                               -DatabaseName $DatabaseName `
                               -SyncGroupName $SyncGroupName

# Check the sync log and wait until the first sync succeeded
Write-Host "Check the sync log"
$IsSucceeded = $false
For ($i = 0; ($i -lt 300) -and (-not $IsSucceeded); $i = $i + 10)
{
    Start-Sleep -s 10
    $SyncLogEndTime = Get-Date
    $SyncLogList = Get-AzureRmSqlSyncGroupLog  -ResourceGroupName $ResourceGroupName `
                                           -ServerName $ServerName `
                                           -DatabaseName $DatabaseName `
                                           -SyncGroupName $SyncGroupName `
                                           -StartTime $SyncLogStartTime.ToUniversalTime() `
                                           -EndTime $SyncLogEndTime.ToUniversalTime()
    if ($SynclogList.Length -gt 0)
    {
        foreach ($SyncLog in $SyncLogList)
        {
            if ($SyncLog.Details.Contains("Sync completed successfully"))
            {
                Write-Host $SyncLog.TimeStamp : $SyncLog.Details
                $IsSucceeded = $true
            }
        }
    }
}

if ($IsSucceeded)
{
    # Enable scheduled sync
    Write-Host "Enable the scheduled sync with 300 seconds interval"
    Update-AzureRmSqlSyncGroup  -ResourceGroupName $ResourceGroupName `
                                -ServerName $ServerName `
                                -DatabaseName $DatabaseName `
                                -Name $SyncGroupName `
                                -IntervalInSeconds $IntervalInSeconds
}
else
{
    # Output all log if sync doesn't succeed in 300 seconds
    $SyncLogEndTime = Get-Date
    $SyncLogList = Get-AzureRmSqlSyncGroupLog  -ResourceGroupName $ResourceGroupName `
                                           -ServerName $ServerName `
                                           -DatabaseName $DatabaseName `
                                           -SyncGroupName $SyncGroupName `
                                           -StartTime $SyncLogStartTime.ToUniversalTime() `
                                           -EndTime $SyncLogEndTime.ToUniversalTime()
    if ($SynclogList.Length -gt 0)
    {
        foreach ($SyncLog in $SyncLogList)
        {
            Write-Host $SyncLog.TimeStamp : $SyncLog.Details
        }
    }
}
# Clean up deployment 
# Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
# Remove-AzureRmResourceGroup -ResourceGroupName $SyncDatabaseResourceGroupName

```

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Örnek betiği çalıştırma sonra kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırabilirsiniz.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $ResourceGroupName
Remove-AzureRmResourceGroup -ResourceGroupName $SyncDatabaseResourceGroupName
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut özgü belgelere Tablo bağlantıları her komut.

| Komut | Notlar |
|---|---|
| [AzureRmSqlSyncAgent yeni](/powershell/module/azurerm.sql/New-AzureRmSqlSyncAgent) |  Yeni bir eşitleme aracı oluşturur |
| [AzureRmSqlSyncAgentKey yeni](/powershell/module/azurerm.sql/New-AzureRmSqlSyncAgentKey) |  Eşitleme Aracı ile ilişkili Aracısı anahtar üretir. |
| [Get-AzureRmSqlSyncAgentLinkedDatabase](/powershell/module/azurerm.sql/Get-AzureRmSqlSyncAgentLinkedDatabase) |  Eşitleme aracısı için tüm bilgileri alın |
| [AzureRmSqlSyncMember yeni](/powershell/module/azurerm.sql/New-AzureRmSqlSyncMember) |  Yeni bir üye eşitleme grubuna Ekle |
| [Güncelleştirme AzureRmSqlSyncSchema](/powershell/module/azurerm.sql/Update-AzureRmSqlSyncSchema) |  Veritabanı şema bilgileri yeniler |
| [Get-AzureRmSqlSyncSchema](/powershell/module/azurerm.sql/Get-AzureRmSqlSyncSchem) |  Veritabanı şema bilgileri alma |
| [Güncelleştirme AzureRmSqlSyncGroup](/powershell/module/azurerm.sql/Update-AzureRmSqlSyncGroup) |  Eşitleme grubunu güncelleştirir |
| [Start-AzureRmSqlSyncGroupSync](/powershell/module/azurerm.sql/Start-AzureRmSqlSyncGroupSync) | Bir eşitleme tetikler |
| [Get-AzureRmSqlSyncGroupLog](/powershell/module/azurerm.sql/Get-AzureRmSqlSyncGroupLog) |  Eşitleme günlüğü denetler |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek SQL veritabanı PowerShell Betiği örnekleri bulunabilir [Azure SQL veritabanı PowerShell komut dosyalarını](../sql-database-powershell-samples.md).

SQL veri eşitleme hakkında daha fazla bilgi için bkz:

-   [Eşitleme verilerle birden çok Bulut ve şirket içi veritabanları arasında Azure SQL veri eşitleme](../sql-database-sync-data.md)
-   [Azure SQL veri eşitleme ayarı](../sql-database-get-started-sql-data-sync.md)
-   [Azure SQL veri eşitleme için en iyi yöntemler](../sql-database-best-practices-data-sync.md)
-   [OMS günlük analizi ile İzleyici Azure SQL veri eşitleme](../sql-database-sync-monitor-oms.md)
-   [Azure SQL veri eşitleme ile ilgili sorunları giderme](../sql-database-troubleshoot-data-sync.md)

-   SQL veri eşitleme yapılandırmayı gösterir PowerShell örnekleri tamamlayın:
    -   [Birden çok Azure SQL veritabanları arasında eşitlemek için PowerShell kullanma](sql-database-sync-data-between-sql-databases.md)

-   [SQL veri eşitleme REST API belgelerini indirebilirsiniz](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

SQL veritabanı hakkında daha fazla bilgi için bkz:

-   [SQL veritabanı genel bakış](../sql-database-technical-overview.md)
-   [Veritabanı yaşam döngüsü yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
