---
title: Azure Data Lake Analytics'i Azure PowerShell'i kullanarak yönetme
description: Bu makale, Data Lake Analytics hesaplarını, veri kaynakları, kullanıcılar ve işlerini yönetmek için Azure PowerShell kullanmayı açıklar.
services: data-lake-analytics
ms.service: data-lake-analytics
author: matt1883
ms.author: mahi
ms.reviewer: jasonwhowell
ms.assetid: ad14d53c-fed4-478d-ab4b-6d2e14ff2097
ms.topic: conceptual
ms.date: 06/29/2018
ms.openlocfilehash: 4273828c9c2bdb75fcbc1de45da55c5a03dd615f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66156414"
---
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Azure Data Lake Analytics'i Azure PowerShell'i kullanarak yönetme
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Bu makalede, Azure PowerShell kullanarak Azure Data Lake Analytics hesaplarını, veri kaynakları, kullanıcılar ve işlerini yönetmek açıklar.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell ile Data Lake Analytics kullanmak için şu bilgilere Topla: 

* **Abonelik kimliği**: Data Lake Analytics hesabınızı içeren Azure abonelik kimliği.
* **Kaynak grubu**: Data Lake Analytics hesabınızı içeren Azure kaynak grubu adı.
* **Data Lake Analytics hesap adı**: Data Lake Analytics hesabınızın adı.
* **Varsayılan Data Lake Store hesap adını**: Her Data Lake Analytics hesabı bir varsayılan Data Lake Store hesabına sahiptir.
* **Konum**: Data Lake Analytics hesabınızı "Doğu ABD 2" ya da diğer gibi konumunu konumları desteklenir.

Bu öğreticideki PowerShell kod parçacıkları, bu bilgileri depolamak için bu değişkenleri kullanır

```powershell
$subId = "<SubscriptionId>"
$rg = "<ResourceGroupName>"
$adla = "<DataLakeAnalyticsAccountName>"
$adls = "<DataLakeStoreAccountName>"
$location = "<Location>"
```

## <a name="log-in-to-azure"></a>Azure'da oturum açma

### <a name="log-in-using-interactive-user-authentication"></a>Etkileşimli kullanıcı kimlik doğrulaması kullanarak oturum açın

Abonelik adını veya abonelik kimliği kullanarak oturum

```powershell
# Using subscription id
Connect-AzAccount -SubscriptionId $subId

# Using subscription name
Connect-AzAccount -SubscriptionName $subname 
```

## <a name="saving-authentication-context"></a>Kimlik doğrulaması bağlamı kaydediliyor

`Connect-AzAccount` Cmdlet her zaman için kimlik bilgilerini ister. Aşağıdaki cmdlet'leri kullanarak istenmesini önlemek:

```powershell
# Save login session information
Save-AzAccounts -Path D:\profile.json  

# Load login session information
Select-AzAccounts -Path D:\profile.json 
```

### <a name="log-in-using-a-service-principal-identity-spi"></a>Bir hizmet sorumlusu kimlik (SPI) kullanarak oturum açın

```powershell
$tenantid = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"  
$spi_appname = "appname" 
$spi_appid = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" 
$spi_secret = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" 

$pscredential = New-Object System.Management.Automation.PSCredential ($spi_appid, (ConvertTo-SecureString $spi_secret -AsPlainText -Force))
Login-AzAccount -ServicePrincipal -TenantId $tenantid -Credential $pscredential -Subscription $subid
```

## <a name="manage-accounts"></a>Hesapları yönetme


### <a name="list-accounts"></a>Hesapları Listele

```powershell
# List Data Lake Analytics accounts within the current subscription.
Get-AdlAnalyticsAccount

# List Data Lake Analytics accounts within a specific resource group.
Get-AdlAnalyticsAccount -ResourceGroupName $rg
```

### <a name="create-an-account"></a>Hesap oluşturma

Her Data Lake Analytics hesabı, günlükleri depolamak için kullandığı varsayılan bir Data Lake Store hesabı gerektirir. Var olan bir hesabı yeniden kullanabilir veya bir hesap oluşturun. 

```powershell
# Create a data lake store if needed, or you can re-use an existing one
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

### <a name="get-account-information"></a>Hesap bilgilerini alma

Bir hesap ayrıntılarını alın.

```powershell
Get-AdlAnalyticsAccount -Name $adla
```

### <a name="check-if-an-account-exists"></a>Hesabınız mevcut olup olmadığını denetleyin

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

## <a name="manage-data-sources"></a>Veri kaynaklarını yönetme
Azure Data Lake Analytics, şu anda aşağıdaki veri kaynaklarını destekler:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Depolama](../storage/common/storage-introduction.md)

Her Data Lake Analytics hesabı bir varsayılan Data Lake Store hesabına sahiptir. Varsayılan Data Lake Store hesabı iş meta verileri ve iş denetim günlüklerini depolamak için kullanılır. 

### <a name="find-the-default-data-lake-store-account"></a>Varsayılan Data Lake Store hesabı bulunamadı

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

Varsayılan Data Lake Store hesabı tarafından veri kaynakları listesini filtreleyerek bulabileceğinizi `IsDefault` özelliği:

```powershell
Get-AdlAnalyticsDataSource -Account $adla  | ? { $_.IsDefault } 
```

### <a name="add-a-data-source"></a>Veri Kaynağı Ekle

```powershell

# Add an additional Storage (Blob) account.
$AzureStorageAccountName = "<AzureStorageAccountName>"
$AzureStorageAccountKey = "<AzureStorageAccountKey>"
Add-AdlAnalyticsDataSource -Account $adla -Blob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

# Add an additional Data Lake Store account.
$AzureDataLakeStoreName = "<AzureDataLakeStoreAccountName"
Add-AdlAnalyticsDataSource -Account $adla -DataLakeStore $AzureDataLakeStoreName 
```

### <a name="list-data-sources"></a>Veri kaynakları listesi

```powershell
# List all the data sources
Get-AdlAnalyticsDataSource -Account $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Account $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Account $adla | where -Property Type -EQ "Blob"
```

## <a name="submit-u-sql-jobs"></a>U-SQL işlerini gönderme

### <a name="submit-a-string-as-a-u-sql-job"></a>Bir dize bir U-SQL işi gönderme

```powershell
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"@

$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 

Submit-AdlJob -AccountName $adla -Script $script -Name "Demo"
```

### <a name="submit-a-file-as-a-u-sql-job"></a>Bir dosya bir U-SQL işi gönderme

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

### <a name="list-jobs"></a>İşleri listele

Çıktı o anda çalışan işleri ve yakın zamanda tamamlanan işleri içerir.

```powershell
Get-AdlJob -Account $adla
```

### <a name="list-the-top-n-jobs"></a>İlk N işleri listele

Gönderme zamanı varsayılan üzerinde işlerin listesini sıralar. Bu nedenle en son gönderilen işlerin ilk görünür. Varsayılan olarak, ADLA hesabı işleri 180 gün boyunca hatırlar. ancak Get-AdlJob cmdlet'ini varsayılan olarak yalnızca ilk 500 döndürür. Kullanın - belirli sayıda işleri listelemek için ilk parametre.

```powershell
$jobs = Get-AdlJob -Account $adla -Top 10
```

### <a name="list-jobs-by-job-state"></a>İş durumu tarafından işleri listele

Kullanarak `-State` parametresi. Bu değerlerden herhangi birini birleştirebilirsiniz:

* `Accepted`
* `Compiling`
* `Ended`
* `New`
* `Paused`
* `Queued`
* `Running`
* `Scheduling`
* `Start`

```powershell
# List the running jobs
Get-AdlJob -Account $adla -State Running

# List the jobs that have completed
Get-AdlJob -Account $adla -State Ended

# List the jobs that have not started yet
Get-AdlJob -Account $adla -State Accepted,Compiling,New,Paused,Scheduling,Start
```

### <a name="list-jobs-by-job-result"></a>İş sonucu tarafından işleri listele

Kullanım `-Result` bitişi işler başarıyla tamamlanıp tamamlanmadığını belirlemek için parametre. Bu değerler içerir:

* İptal Edildi
* Başarısız
* None
* Başarılı oldu

``` powershell
# List Successful jobs.
Get-AdlJob -Account $adla -State Ended -Result Succeeded

# List Failed jobs.
Get-AdlJob -Account $adla -State Ended -Result Failed
```

### <a name="list-jobs-by-job-submitter"></a>İşi gönderen tarafından işleri listele

`-Submitter` Parametresi olan bir işi gönderilen belirlemenize yardımcı olur.

```powershell
Get-AdlJob -Account $adla -Submitter "joe@contoso.com"
```

### <a name="list-jobs-by-submission-time"></a>Gönderme saati tarafından işleri listele

`-SubmittedAfter` Filtreleme yapmak bir zaman aralığı için kullanışlıdır.


```powershell
# List  jobs submitted in the last day.
$d = [DateTime]::Now.AddDays(-1)
Get-AdlJob -Account $adla -SubmittedAfter $d

# List  jobs submitted in the last seven day.
$d = [DateTime]::Now.AddDays(-7)
Get-AdlJob -Account $adla -SubmittedAfter $d
```

### <a name="get-job-status"></a>İş durumunu Al

Belirli bir işin durumunu alın.

```powershell
Get-AdlJob -AccountName $adla -JobId $job.JobId
```


### <a name="cancel-a-job"></a>Bir işi iptal et

```powershell
Stop-AdlJob -Account $adla -JobID $jobID
```

### <a name="wait-for-a-job-to-finish"></a>İşin tamamlanmasını bekleyin

Yinelenen yerine `Get-AdlAnalyticsJob` bir iş tamamlanana kadar kullanabileceğiniz `Wait-AdlJob` cmdlet'ini sonlandırmak işin tamamlanmasını bekleyin.

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

## <a name="analyzing-job-history"></a>İş geçmişi analiz etme

Data Lake analytics'te çalıştırılan işler geçmişini analiz etmek için Azure PowerShell kullanarak güçlü bir tekniktir. Kullanım ve maliyet hakkında Öngörüler elde etmek için kullanabilirsiniz. Daha fazla bilgi bakarak [iş geçmişi analizi örnek depo](https://github.com/Azure-Samples/data-lake-analytics-powershell-job-history-analysis)  

## <a name="list-job-pipelines-and-recurrences"></a>Liste İş işlem hatları ve tekrarlar

Kullanım `Get-AdlJobPipeline` önceden gönderilmiş işler hakkında işlem hatları bilgilerini görmek için cmdlet.

```powershell
$pipelines = Get-AdlJobPipeline -Account $adla
$pipeline = Get-AdlJobPipeline -Account $adla -PipelineId "<pipeline ID>"
```

Kullanım `Get-AdlJobRecurrence` gönderilmiş işler hakkında tekrar bilgilerini daha önce görmek için cmdlet.

```powershell
$recurrences = Get-AdlJobRecurrence -Account $adla

$recurrence = Get-AdlJobRecurrence -Account $adla -RecurrenceId "<recurrence ID>"
```


## <a name="manage-compute-policies"></a>İşlem ilkelerini yönetme

### <a name="list-existing-compute-policies"></a>Mevcut işlem ilkeleri listesi

`Get-AdlAnalyticsComputePolicy` Cmdlet'i, bir Data Lake Analytics hesabı için işlem ilkeleri hakkında bilgi alır.

```powershell
$policies = Get-AdlAnalyticsComputePolicy -Account $adla
```

### <a name="create-a-compute-policy"></a>İşlem ilkesi oluşturma

`New-AdlAnalyticsComputePolicy` Cmdlet'i, bir Data Lake Analytics hesabı için yeni bir işlem ilkesi oluşturur. Bu örnek, 50 belirtilen kullanıcıya ve 250 en düşük iş önceliği için kullanılabilen maksimum AUS değerini ayarlar.

```powershell
$userObjectId = (Get-AzAdUser -SearchString "garymcdaniel@contoso.com").Id

New-AdlAnalyticsComputePolicy -Account $adla -Name "GaryMcDaniel" -ObjectId $objectId -ObjectType User -MaxDegreeOfParallelismPerJob 50 -MinPriorityPerJob 250
```
## <a name="manage-files"></a>Dosyaları yönetme

### <a name="check-for-the-existence-of-a-file"></a>Bir dosyanın varlığını denetleyin.

```powershell
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

### <a name="uploading-and-downloading"></a>Karşıya yükleme ve indirme

Bir dosyayı karşıya yükleyin.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

Bir klasörün tamamını yinelemeli olarak karşıya yükleyin.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\myData\" -Destination "/myData/" -Recurse
```

Bir dosya indirin.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "c:\data.csv"
```

Bir klasörün tamamını yinelemeli olarak indirin.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/" -Destination "c:\myData\" -Recurse
```

> [!NOTE]
> Karşıya yükleme ve indirme işlemini kesintiye uğrarsa, yeniden cmdlet'ini çalıştırarak işlemi sürdürmeyi deneyebilirsiniz ``-Resume`` bayrağı.

## <a name="manage-the-u-sql-catalog"></a>U-SQL kataloğunu yönetme

U-SQL Kataloğu, veri ve kod U-SQL betikleri tarafından paylaşılacak şekilde yapılandırmak için kullanılır. Kataloğu, verileri Azure Data Lake ile olası en yüksek performans sağlar. Daha fazla bilgi için bkz. [U-SQL kataloğunu kullanma](data-lake-analytics-use-u-sql-catalog.md).

### <a name="list-items-in-the-u-sql-catalog"></a>U-SQL Kataloğu liste öğeleri

```powershell
# List U-SQL databases
Get-AdlCatalogItem -Account $adla -ItemType Database 

# List tables within a database
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database"

# List tables within a schema.
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database.schema"
```

### <a name="list-all-the-assemblies-the-u-sql-catalog"></a>Derlemeleri U-SQL Kataloğu listesi

```powershell
$dbs = Get-AdlCatalogItem -Account $adla -ItemType Database

foreach ($db in $dbs)
{
    $asms = Get-AdlCatalogItem -Account $adla -ItemType Assembly -Path $db.Name

    foreach ($asm in $asms)
    {
        $asmname = "[" + $db.Name + "].[" + $asm.Name + "]"
        Write-Host $asmname
    }
}
```

### <a name="get-details-about-a-catalog-item"></a>Katalog öğesi hakkında ayrıntılı bilgi edinin

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="store-credentials-in-the-catalog"></a>Kimlik bilgileri kataloğa Store

İçindeki bir U-SQL veritabanı, Azure'da barındırılan bir veritabanı için bir kimlik bilgisi nesnesi oluşturun. Şu anda, U-SQL kimlik bilgileri yalnızca PowerShell üzerinden oluşturabilirsiniz katalog öğesi türüdür.

```powershell
$dbName = "master"
$credentialName = "ContosoDbCreds"
$dbUri = "https://contoso.database.windows.net:8080"

New-AdlCatalogCredential -AccountName $adla `
          -DatabaseName $db `
          -CredentialName $credentialName `
          -Credential (Get-Credential) `
          -Uri $dbUri
```

## <a name="manage-firewall-rules"></a>Güvenlik duvarı kurallarını yönetme

### <a name="list-firewall-rules"></a>Güvenlik duvarı kurallarını Listele

```powershell
Get-AdlAnalyticsFirewallRule -Account $adla
```

### <a name="add-a-firewall-rule"></a>Bir güvenlik duvarı kuralı ekleyin

```powershell
$ruleName = "Allow access from on-prem server"
$startIpAddress = "<start IP address>"
$endIpAddress = "<end IP address>"

Add-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

### <a name="modify-a-firewall-rule"></a>Bir güvenlik duvarı kuralını değiştirme

```powershell
Set-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

### <a name="remove-a-firewall-rule"></a>Bir güvenlik duvarı kuralını Kaldır

```powershell
Remove-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName
```

### <a name="allow-azure-ip-addresses"></a>Azure IP adreslerine izin ver

```powershell
Set-AdlAnalyticsAccount -Name $adla -AllowAzureIpState Enabled
```

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Enabled
Set-AdlAnalyticsAccount -Name $adla -FirewallState Disabled
```


## <a name="working-with-azure"></a>Azure ile çalışma

### <a name="get-error-details"></a>Hata ayrıntılarını Al

```powershell
Resolve-AzError -Last
```

### <a name="verify-if-you-are-running-as-an-administrator-on-your-windows-machine"></a>Windows makinenizde bir yönetici olarak çalıştırıp çalıştırmadığınızı doğrulayın

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a>Bir Tenantıd bulun

Abonelik adı:

```powershell
function Get-TenantIdFromSubscriptionName( [string] $subname )
{
    $sub = (Get-AzSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubscriptionName "ADLTrainingMS"
```

Bir abonelik kimliği:

```powershell
function Get-TenantIdFromSubscriptionId( [string] $subid )
{
    $sub = (Get-AzSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubscriptionId $subid
```

"Contoso.com" gibi bir etki alanı adresi

```powershell
function Get-TenantIdFromDomain( $domain )
{
    $url = "https://login.windows.net/" + $domain + "/.well-known/openid-configuration"
    return (Invoke-WebRequest $url|ConvertFrom-Json).token_endpoint.Split('/')[3]
}

$domain = "contoso.com"
Get-TenantIdFromDomain $domain
```

### <a name="list-all-your-subscriptions-and-tenant-ids"></a>Tüm abonelikleri listeleme ve Kiracı kimlikleri

```powershell
$subs = Get-AzSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a>Bir şablonu kullanarak Data Lake Analytics hesabı oluşturma

Aşağıdaki örneği kullanarak bir Azure kaynak grubu şablonu da kullanabilirsiniz: [Bir şablonu kullanarak Data Lake Analytics hesabı oluşturma](https://github.com/Azure-Samples/data-lake-analytics-create-account-with-arm-template)

## <a name="next-steps"></a>Sonraki adımlar
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* Kullanarak Data Lake Analytics ile çalışmaya başlama [Azure portalında](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [Azure CLI](data-lake-analytics-get-started-cli.md)
* Azure Data Lake Analytics'i kullanarak yönetme [Azure portalında](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md) 
