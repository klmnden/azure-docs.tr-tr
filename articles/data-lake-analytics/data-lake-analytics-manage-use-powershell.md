---
title: Azure Data Lake Analytics'i Azure PowerShell'i kullanarak yönetme
description: Bu makalede, Data Lake Analytics hesaplarını, veri kaynakları, kullanıcılar ve işleri yönetmek için Azure PowerShell kullanmayı açıklar.
services: data-lake-analytics
ms.service: data-lake-analytics
author: matt1883
ms.author: mahi
manager: kfile
editor: jasonwhowell
ms.assetid: ad14d53c-fed4-478d-ab4b-6d2e14ff2097
ms.topic: conceptual
ms.date: 06/02/2018
ms.openlocfilehash: 560f36dc64480fd6aceaa50226b191ee40d2486f
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36959857"
---
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Azure Data Lake Analytics'i Azure PowerShell'i kullanarak yönetme
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Bu makalede, Azure PowerShell kullanarak Azure Data Lake Analytics hesaplarını, veri kaynakları, kullanıcılar ve işleri Yönet açıklar.

## <a name="prerequisites"></a>Önkoşullar

PowerShell Data Lake Analytics ile kullanmak için şu bilgileri toplayın: 

* **Abonelik kimliği**: Data Lake Analytics hesabınızı içeren Azure abonelik kimliği.
* **Kaynak grubu**: Data Lake Analytics hesabınızı içeren Azure kaynak grubunun adı.
* **Data Lake Analytics hesap adı**: Data Lake Analytics hesabınızın adını.
* **Varsayılan Data Lake Store hesabı adı**: her Data Lake Analytics hesabı varsayılan Data Lake Store hesabına sahiptir.
* **Konum**: "Doğu ABD 2" veya diğer gibi Data Lake Analytics hesabınızı konumunu desteklenen konumlar.

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

Bir abonelik kimliği kullanarak veya abonelik adı oturum

```powershell
# Using subscription id
Connect-AzureRmAccount -SubscriptionId $subId

# Using subscription name
Connect-AzureRmAccount -SubscriptionName $subname 
```

## <a name="saving-authenticaiton-context"></a>Kimlik doğrulaması bağlamı kaydetme

`Connect-AzureRmAccount` Cmdlet her zaman için kimlik bilgilerini ister. Aşağıdaki cmdlet'leri kullanarak sorulmasını önlemek:

```powershell
# Save login session information
Save-AzureRmProfile -Path D:\profile.json  

# Load login session information
Select-AzureRmProfile -Path D:\profile.json 
```

### <a name="log-in-using-a-service-principal-identity-spi"></a>Bir hizmet asıl kimlik (SPI) kullanarak oturum açın

```powershell
$tenantid = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"  
$spi_appname = "appname" 
$spi_appid = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" 
$spi_secret = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" 

$pscredential = New-Object System.Management.Automation.PSCredential ($spi_appid, (ConvertTo-SecureString $spi_secret -AsPlainText -Force))
Login-AzureRmAccount -ServicePrincipal -TenantId $tenantid -Credential $pscredential -Subscription $subid
```

## <a name="manage-accounts"></a>Hesapları yönetme


### <a name="list-accounts"></a>Hesapları listeleme

```powershell
# List Data Lake Analytics accounts within the current subscription.
Get-AdlAnalyticsAccount

# List Data Lake Analytics accounts within a specific resource group.
Get-AdlAnalyticsAccount -ResourceGroupName $rg
```

### <a name="create-an-account"></a>Hesap oluşturma

Her Data Lake Analytics hesabı, günlükleri depolamak için kullandığı varsayılan bir Data Lake Store hesabı gerektirir. Hesabınız yeniden ya da bir hesap oluşturun. 

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

### <a name="check-if-an-account-exists"></a>Bir hesap var olup olmadığını denetleyin

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

## <a name="manage-data-sources"></a>Veri kaynaklarını yönet
Azure Data Lake Analytics şu anda aşağıdaki veri kaynaklarını destekler:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Depolama](../storage/common/storage-introduction.md)

Her Data Lake Analytics hesabı bir varsayılan Data Lake Store hesabı vardır. Varsayılan Data Lake Store hesabı iş meta verileri ve iş denetim günlüklerini depolamak için kullanılır. 

### <a name="find-the-default-data-lake-store-account"></a>Varsayılan Data Lake Store hesabı bulunamadı

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

Varsayılan Data Lake Store hesabına veri kaynakları tarafından listesi filtreleyerek bulabileceğiniz `IsDefault` özelliği:

```powershell
Get-AdlAnalyticsDataSource -Account $adla  | ? { $_.IsDefault } 
```

### <a name="add-a-data-source"></a>Veri kaynağı ekleme

```powershell

# Add an additional Storage (Blob) account.
$AzureStorageAccountName = "<AzureStorageAccountName>"
$AzureStorageAccountKey = "<AzureStorageAccountKey>"
Add-AdlAnalyticsDataSource -Account $adla -Blob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

# Add an additional Data Lake Store account.
$AzureDataLakeStoreName = "<AzureDataLakeStoreAccountName"
Add-AdlAnalyticsDataSource -Account $adla -DataLakeStore $AzureDataLakeStoreName 
```

### <a name="list-data-sources"></a>Veri kaynaklarını listele

```powershell
# List all the data sources
Get-AdlAnalyticsDataSource -Name $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "Blob"
```

## <a name="submit-u-sql-jobs"></a>U-SQL işlerini gönderme

### <a name="submit-a-string-as-a-u-sql-job"></a>U-SQL iş olarak bir dize gönderme

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

### <a name="submit-a-file-as-a-u-sql-job"></a>U-SQL iş olarak bir dosya gönderin

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

### <a name="list-jobs"></a>Liste işleri

Çıktı o anda çalışan işleri ve yakın zamanda tamamlanan işleri içerir.

```powershell
Get-AdlJob -Account $adla
```

### <a name="list-the-top-n-jobs"></a>İlk N işleri listelemek

İşlerin listesini üzerinde sıralanır varsayılan saat gönderin. Bu nedenle en son gönderilen işler ilk görünür. Varsayılan olarak, 180 gün için işleri ADLA hesap hatırlıyor ancak Get-AdlJob cmdlet'i varsayılan olarak yalnızca ilk 500 döndürür. Kullanın - işleri belirli sayıda listelemek için üst parametre.

```powershell
$jobs = Get-AdlJob -Account $adla -Top 10
```

### <a name="list-jobs-by-job-state"></a>İş durumuna göre listesi işleri

Kullanarak `-State` parametresi. Bu değerleri birleştirebilirsiniz:

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

### <a name="list-jobs-by-job-result"></a>İş sonuç listesi işleri

Kullanım `-Result` sona erdi işler başarıyla tamamlandığında olup olmadığını algılamak için parametre. Bu değer vardır:

* İptal edildi
* Başarısız
* None
* Başarılı oldu

``` powershell
# List Successful jobs.
Get-AdlJob -Account $adla -State Ended -Result Succeeded

# List Failed jobs.
Get-AdlJob -Account $adla -State Ended -Result Failed
```

### <a name="list-jobs-by-job-submitter"></a>Liste işleri iş gönderen tarafından

`-Submitter` Parametre kullanan bir işin gönderildiği belirlemenize yardımcı olur.

```powershell
Get-AdlJob -Account $adla -Submitter "joe@contoso.com"
```

### <a name="list-jobs-by-submission-time"></a>Liste işleri gönderme zamanı tarafından

`-SubmittedAfter` Bir zaman aralığı için filtreleme yararlıdır.


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


### <a name="cancel-a-job"></a>Bir işi iptal etme

```powershell
Stop-AdlJob -Account $adla -JobID $jobID
```

### <a name="wait-for-a-job-to-finish"></a>İşin tamamlanmasını bekleyin

Yinelenen yerine `Get-AdlAnalyticsJob` bir işi tamamlanana kadar kullanabileceğiniz `Wait-AdlJob` cmdlet işi sona erdirmek bekleyin.

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

## <a name="analyzing-job-history"></a>İş geçmişi analiz etme

Data Lake analytics çalıştıran işleri geçmişini çözümlemek için Azure PowerShell kullanarak güçlü bir tekniktir. Kullanım ve maliyet Öngörüler elde etmek için kullanabilirsiniz. Bakarak tarafından daha fazla bilgi edinebilirsiniz [iş geçmişi analiz örnek depo](https://github.com/Azure-Samples/data-lake-analytics-powershell-job-history-analysis)  

## <a name="list-job-pipelines-and-recurrences"></a>Liste İş ardışık düzen ve tekrarları

Kullanım `Get-AdlJobPipeline` ardışık düzen bilgileri görmek için cmdlet daha önce gönderilen işler.

```powershell
$pipelines = Get-AdlJobPipeline -Account $adla
$pipeline = Get-AdlJobPipeline -Account $adla -PipelineId "<pipeline ID>"
```

Kullanım `Get-AdlJobRecurrence` yineleme bilgileri önceden görmek için cmdlet gönderilen işler.

```powershell
$recurrences = Get-AdlJobRecurrence -Account $adla

$recurrence = Get-AdlJobRecurrence -Account $adla -RecurrenceId "<recurrence ID>"
```


## <a name="manage-compute-policies"></a>İşlem ilkelerini yönetme

### <a name="list-existing-compute-policies"></a>Var olan bilgi işlem ilkeleri listesi

`Get-AdlAnalyticsComputePolicy` Cmdlet'i bir Data Lake Analytics hesabı için bilgi işlem ilkeleri hakkında bilgi alır.

```powershell
$policies = Get-AdlAnalyticsComputePolicy -Account $adla
```

### <a name="create-a-compute-policy"></a>Bir işlem ilkesi oluşturma

`New-AdlAnalyticsComputePolicy` Cmdlet'i, bir Data Lake Analytics hesabı için yeni bir işlem ilkesi oluşturur. Bu örnekte kullanılabilen en yüksek Avustralya 50 belirtilen kullanıcıya ve 250 en düşük iş önceliği ayarlar.

```powershell
$userObjectId = (Get-AzureRmAdUser -SearchString "garymcdaniel@contoso.com").Id

New-AdlAnalyticsComputePolicy -Account $adla -Name "GaryMcDaniel" -ObjectId $objectId -ObjectType User -MaxDegreeOfParallelismPerJob 50 -MinPriorityPerJob 250
```
## <a name="manage-files"></a>Dosyalarını yönetme

### <a name="check-for-the-existence-of-a-file"></a>Bir dosyanın varlığını denetleyin.

```powershell
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

### <a name="uploading-and-downloading"></a>Karşıya yükleme ve indirme

Bir dosyayı karşıya yükleyin.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

Tüm klasörü yinelemeli olarak karşıya yükleyin.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\myData\" -Destination "/myData/" -Recurse
```

Dosya indirme.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "c:\data.csv"
```

Tüm klasörü yinelemeli olarak indirin.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/" -Destination "c:\myData\" -Recurse
```

> [!NOTE]
> Karşıya yükleme veya indirme işlemi kesintiye uğrarsa, yeniden cmdlet'ini çalıştırarak işlemini sürdürmek deneyebilirsiniz ``-Resume`` bayrağı.

## <a name="manage-the-u-sql-catalog"></a>U-SQL kataloğunu yönetme

U-SQL kataloğunu, U-SQL komut dosyaları tarafından paylaşılabilmesi veri ve kod yapısı için kullanılır. Katalog Azure Data Lake verilerle olası en yüksek performans sağlar. Daha fazla bilgi için bkz. [U-SQL kataloğunu kullanma](data-lake-analytics-use-u-sql-catalog.md).

### <a name="list-items-in-the-u-sql-catalog"></a>U-SQL kataloğunda liste öğeleri

```powershell
# List U-SQL databases
Get-AdlCatalogItem -Account $adla -ItemType Database 

# List tables within a database
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database"

# List tables within a schema.
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database.schema"
```

### <a name="list-all-the-assemblies-the-u-sql-catalog"></a>Tüm derlemeleri U-SQL kataloğunu listesi

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

### <a name="get-details-about-a-catalog-item"></a>Bir katalog öğesi hakkında bilgi almak

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="store-credentials-in-the-catalog"></a>Katalogda kimlik bilgilerini depolamak

U-SQL veritabanı içinde Azure üzerinde barındırılan bir veritabanı için bir kimlik bilgisi nesnesi oluşturun. Şu anda, U-SQL kimlik bilgileri yalnızca PowerShell aracılığıyla oluşturabilirsiniz katalog öğesi türüdür.

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

## <a name="manage-firewall-rules"></a>Güvenlik duvarı kurallarını yönet

### <a name="list-firewall-rules"></a>Liste güvenlik duvarı kuralları

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

### <a name="allow-azure-ip-addresses"></a>Azure IP adreslerini izin ver

```powershell
Set-AdlAnalyticsAccount -Name $adla -AllowAzureIpState Enabled
```

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Enabled
Set-AdlAnalyticsAccount -Name $adla -FirewallState Disabled
```


## <a name="working-with-azure"></a>Azure ile çalışma

### <a name="get-details-of-azurerm-errors"></a>AzureRm hataların ayrıntılarını alma

```powershell
Resolve-AzureRmError -Last
```

### <a name="verify-if-you-are-running-as-an-administrator-on-your-windows-machine"></a>Windows makinenizde yönetici olarak çalıştırıyorsanız doğrulayın

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a>Bir Tenantıd Bul

Abonelik adı:

```powershell
function Get-TenantIdFromSubcriptionName( [string] $subname )
{
    $sub = (Get-AzureRmSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubcriptionName "ADLTrainingMS"
```

Bir abonelik kimliği:

```powershell
function Get-TenantIdFromSubcriptionId( [string] $subid )
{
    $sub = (Get-AzureRmSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubcriptionId $subid
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

### <a name="list-all-your-subscriptions-and-tenant-ids"></a>Tüm aboneliklerinizi listelemek ve kimlikler Kiracı

```powershell
$subs = Get-AzureRmSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a>Bir şablon kullanarak Data Lake Analytics hesabı oluşturma

Aşağıdaki örneği kullanarak bir Azure kaynak grubu şablonu de kullanabilirsiniz: [bir şablon kullanarak Data Lake Analytics hesabı oluşturma](https://github.com/Azure-Samples/data-lake-analytics-create-account-with-arm-template)

## <a name="next-steps"></a>Sonraki adımlar
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* Kullanarak Data Lake Analytics ile çalışmaya başlama [Azure portal](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI 2.0](data-lake-analytics-get-started-cli2.md)
* Kullanarak Azure Data Lake Analytics yönetmek [Azure portal](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md) 
