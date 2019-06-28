---
title: PowerShell kullanarak Azure SQL Veritabanı Elastik İş aracısı oluşturma | Microsoft Docs
description: PowerShell kullanarak Elastik İş aracısı oluşturmayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: tutorial
author: johnpaulkee
ms.author: joke
ms.reviwer: sstein
manager: craigg
ms.date: 03/13/2019
ms.openlocfilehash: 53e10636535c553ac5fa17b5f4aac1000cd138bc
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445380"
---
# <a name="create-an-elastic-job-agent-using-powershell"></a>PowerShell kullanarak Elastik İş aracısı oluşturma

[Elastik işler](sql-database-job-automation-overview.md#elastic-database-jobs), bir veya daha fazla Transact-SQL (T-SQL) betiğinin birden fazla veritabanında paralel olarak çalıştırılmasını sağlar.

Bu öğreticide, birden fazla veritabanında bir sorgu çalıştırmak için gereken adımları öğrenin:

> [!div class="checklist"]
> * Elastik İş aracısı oluşturma
> * İşlerin hedeflerinde betik yürütebilmesi için iş kimlik bilgileri oluşturma
> * İşi çalıştırmak istediğiniz hedefleri (sunucular, elastik havuzlar, parça eşlemeleri) tanımlama
> * Aracının işlere bağlanıp yürütebilmesi için hedef veritabanlarında veritabanlı kapsamlı kimlik bilgileri oluşturma
> * Bir iş oluşturma
> * Bir işe iş adımları ekleme
> * Bir işin yürütülmesini başlatma
> * Bir işi izleme

## <a name="prerequisites"></a>Önkoşullar

Elastik veritabanı işleri yükseltilmiş sürümünü kullanmak için PowerShell cmdlet'leri yeni bir geçiş sırasında sahiptir. Mevcut iş kimlik bilgilerinizi, tüm bu yeni cmdlet'lere aktarım hedefler (veritabanları, sunucular, özel koleksiyonlar), iş tetikleyicileri, iş zamanlamalarını, işi içeriği ve işleri üzerinde yeni bir elastik İş Aracısı.

### <a name="install-the-latest-elastic-jobs-cmdlets"></a>En son esnek işler cmdlet'leri yükleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

Yükleme **Az.Sql** 1.1.1-preview modülünün en son elastik iş cmdlet'lerini edinin. PowerShell'de yönetici erişimiyle aşağıdaki komutları çalıştırın.

```powershell
# Installs the latest PackageManagement powershell package which PowershellGet v1.6.5 is dependent on
Find-Package PackageManagement -RequiredVersion 1.1.7.2 | Install-Package -Force

# Installs the latest PowershellGet module which adds the -AllowPrerelease flag to Install-Module
Find-Package PowerShellGet -RequiredVersion 1.6.5 | Install-Package -Force

# Restart your powershell session with administrative access

# Places Az.Sql preview cmdlets side by side with existing Az.Sql version
Install-Module -Name Az.Sql -RequiredVersion 1.1.1-preview -AllowPrerelease

# Import the Az.Sql module
Import-Module Az.Sql -RequiredVersion 1.1.1

# Confirm if module successfully imported - if the imported version is 1.1.1, then continue
Get-Module Az.Sql
```

- Ek olarak **Az.Sql** 1.1.1-preview modülü, Bu öğretici ayrıca gerektirir *sqlserver* PowerShell modülü. Ayrıntılar için bkz [SQL Server PowerShell modülü yükleme](https://docs.microsoft.com/sql/powershell/download-sql-server-ps-module).


## <a name="create-required-resources"></a>Gerekli kaynakları oluşturma

Elastik İş aracısı oluşturmak için [İş veritabanı](sql-database-job-automation-overview.md#job-database) olarak kullanılacak bir veritabanı (S0 veya üzeri) gerekir. 

*Aşağıdaki betik yeni bir kaynak grubu ve sunucunun yanı sıra İş veritabanı olarak kullanılacak bir veritabanı oluşturur. Aşağıdaki betiği ikinci bir sunucusuna karşı işleri yürütmek için iki boş veritabanı ile de oluşturur.*

Elastik İşlere özel adlandırma gereksinimleri olmadığından [Azure gereksinimlerine](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) uygun olduğu sürece istediğiniz adlandırma kuralını kullanabilirsiniz.

```powershell
# Sign in to your Azure account
Connect-AzAccount

# Create a resource group
Write-Output "Creating a resource group..."
$ResourceGroupName = Read-Host "Please enter a resource group name"
$Location = Read-Host "Please enter an Azure Region"
$Rg = New-AzResourceGroup -Name $ResourceGroupName -Location $Location
$Rg

# Create a server
Write-Output "Creating a server..."
$AgentServerName = Read-Host "Please enter an agent server name"
$AgentServerName = $AgentServerName + "-" + [guid]::NewGuid()
$AdminLogin = Read-Host "Please enter the server admin name"
$AdminPassword = Read-Host "Please enter the server admin password"
$AdminPasswordSecure = ConvertTo-SecureString -String $AdminPassword -AsPlainText -Force
$AdminCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AdminLogin, $AdminPasswordSecure
$AgentServer = New-AzSqlServer -ResourceGroupName $ResourceGroupName -Location $Location -ServerName $AgentServerName -ServerVersion "12.0" -SqlAdministratorCredentials ($AdminCred)

# Set server firewall rules to allow all Azure IPs
Write-Output "Creating a server firewall rule..."
$AgentServer | New-AzSqlServerFirewallRule -AllowAllAzureIPs
$AgentServer

# Create the job database
Write-Output "Creating a blank SQL database to be used as the Job Database..."
$JobDatabaseName = "JobDatabase"
$JobDatabase = New-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $AgentServerName -DatabaseName $JobDatabaseName -RequestedServiceObjectiveName "S0"
$JobDatabase
```

```powershell
# Create a target server and some sample databases - uses the same admin credential as the agent server just for simplicity
Write-Output "Creating target server..."
$TargetServerName = Read-Host "Please enter a target server name"
$TargetServerName = $TargetServerName + "-" + [guid]::NewGuid()
$TargetServer = New-AzSqlServer -ResourceGroupName $ResourceGroupName -Location $Location -ServerName $TargetServerName -ServerVersion "12.0" -SqlAdministratorCredentials ($AdminCred)

# Set target server firewall rules to allow all Azure IPs
$TargetServer | New-AzSqlServerFirewallRule -AllowAllAzureIPs
$TargetServer | New-AzSqlServerFirewallRule -StartIpAddress 0.0.0.0 -EndIpAddress 255.255.255.255 -FirewallRuleName AllowAll
$TargetServer

# Create some sample databases to execute jobs against...
$Db1 = New-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $TargetServerName -DatabaseName "TargetDb1"
$Db1
$Db2 = New-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $TargetServerName -DatabaseName "TargetDb2"
$Db2
```

## <a name="enable-the-elastic-jobs-preview-for-your-subscription"></a>Aboneliğiniz için Elastik İşler önizleme sürümünü etkinleştirme

Esnek işler kullanmak için aşağıdaki komutu çalıştırarak Azure aboneliğinizde özellik kaydedin. Bir kez elastik İş Aracısı sağlamak istediğiniz abonelik için bu komutu çalıştırın. İş hedefleri olan veritabanları yalnızca içeren abonelikler kaydedilmesi gerekmez.

```powershell
Register-AzProviderFeature -FeatureName sqldb-JobAccounts -ProviderNamespace Microsoft.Sql
```

## <a name="create-the-elastic-job-agent"></a>Elastik İş aracısını oluşturma

Elastik İş aracısı; işlerin oluşturulması, çalıştırılması ve yönetilmesi için kullanılan bir Azure kaynağıdır. Aracı işleri belirli bir plana göre veya tek seferlik iş olarak yürütür.

**Yeni AzSqlElasticJobAgent** cmdlet varolduğu için bir Azure SQL veritabanı gerektirir böylece *ResourceGroupName*, *ServerName*, ve  *DatabaseName* parametreleri, mevcut kaynaklar için tüm noktası gerekir.

```powershell
Write-Output "Creating job agent..."
$AgentName = Read-Host "Please enter a name for your new Elastic Job agent"
$JobAgent = $JobDatabase | New-AzSqlElasticJobAgent -Name $AgentName
$JobAgent
```

## <a name="create-job-credentials-so-that-jobs-can-execute-scripts-on-its-targets"></a>İşlerin hedeflerinde betik yürütebilmesi için iş kimlik bilgileri oluşturma

İşler, yürütme sırasında hedef grup tarafından belirtilen hedef veritabanlarına bağlanmak için veritabanı kapsamlı kimlik bilgilerini kullanır. Bu veritabanı kapsamlı kimlik bilgileri aynı zamanda hedef grup üyesi türü olarak kullanıldığında bir sunucu veya elastik havuzdaki tüm veritabanlarını numaralandırma amacıyla asıl veritabanına bağlanmak için kullanılır.

Veritabanı kapsamlı kimlik bilgileri iş veritabanında oluşturulmalıdır.  
İşin başarıyla tamamlanması için hedef veritabanlarının tümünde yeterli izinlere sahip bir oturum açma adı olmalıdır.

![Elastik İşler kimlik bilgileri](media/elastic-jobs-overview/job-credentials.png)

Aşağıdaki betikte görüntüdeki kimlik bilgilerine ek olarak *GRANT* komutlarının kullanıldığına dikkat edin. Bu izinler, bu örnek işte seçtiğimiz betik için geçerlidir. Örnek, hedeflenen veritabanlarında yeni bir tablo oluşturduğundan başarıyla çalışması için hedef veritabanlarında doğru izinlere sahip olması gerekir.

Gereken iş kimlik bilgilerini (iş veritabanında) oluşturmak için aşağıdaki betiği çalıştırın:

```powershell
# In the master database (target server)
# - Create the master user login
# - Create the master user from master user login
# - Create the job user login
$Params = @{
  'Database' = 'master'
  'ServerInstance' =  $TargetServer.ServerName + '.database.windows.net'
  'Username' = $AdminLogin
  'Password' = $AdminPassword
  'OutputSqlErrors' = $true
  'Query' = "CREATE LOGIN masteruser WITH PASSWORD='password!123'"
}
Invoke-SqlCmd @Params
$Params.Query = "CREATE USER masteruser FROM LOGIN masteruser"
Invoke-SqlCmd @Params
$Params.Query = "CREATE LOGIN jobuser WITH PASSWORD='password!123'"
Invoke-SqlCmd @Params

# For each of the target databases
# - Create the jobuser from jobuser login
# - Make sure they have the right permissions for successful script execution
$TargetDatabases = @( $Db1.DatabaseName, $Db2.DatabaseName )
$CreateJobUserScript =  "CREATE USER jobuser FROM LOGIN jobuser"
$GrantAlterSchemaScript = "GRANT ALTER ON SCHEMA::dbo TO jobuser"
$GrantCreateScript = "GRANT CREATE TABLE TO jobuser"

$TargetDatabases | % {
  $Params.Database = $_

  $Params.Query = $CreateJobUserScript
  Invoke-SqlCmd @Params

  $Params.Query = $GrantAlterSchemaScript
  Invoke-SqlCmd @Params

  $Params.Query = $GrantCreateScript
  Invoke-SqlCmd @Params
}

# Create job credential in Job database for master user
Write-Output "Creating job credentials..."
$LoginPasswordSecure = (ConvertTo-SecureString -String "password!123" -AsPlainText -Force)

$MasterCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList "masteruser", $LoginPasswordSecure
$MasterCred = $JobAgent | New-AzSqlElasticJobCredential -Name "masteruser" -Credential $MasterCred

$JobCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList "jobuser", $LoginPasswordSecure
$JobCred = $JobAgent | New-AzSqlElasticJobCredential -Name "jobuser" -Credential $JobCred
```

## <a name="define-the-target-databases-you-want-to-run-the-job-against"></a>İşi çalıştırmak istediğiniz hedef veritabanlarını tanımlama

[Hedef grup](sql-database-job-automation-overview.md#target-group), işin üzerinde çalışacağı veritabanı bir veya daha fazla veritabanından oluşan kümeyi tanımlar. 

Aşağıdaki kod parçacığı, iki hedef grubu oluşturur: *ServerGroup*, ve *ServerGroupExcludingDb2*. *ServerGroup*, sunucuda yürütme anında var olan tüm veritabanlarını hedeflerken *ServerGroupExcludingDb2*, *TargetDb2* hariç sunucudaki tüm veritabanlarını hedefler:

```powershell
Write-Output "Creating test target groups..."
# Create ServerGroup target group
$ServerGroup = $JobAgent | New-AzSqlElasticJobTargetGroup -Name 'ServerGroup'
$ServerGroup | Add-AzSqlElasticJobTarget -ServerName $TargetServerName -RefreshCredentialName $MasterCred.CredentialName

# Create ServerGroup with an exclusion of Db2
$ServerGroupExcludingDb2 = $JobAgent | New-AzSqlElasticJobTargetGroup -Name 'ServerGroupExcludingDb2'
$ServerGroupExcludingDb2 | Add-AzSqlElasticJobTarget -ServerName $TargetServerName -RefreshCredentialName $MasterCred.CredentialName
$ServerGroupExcludingDb2 | Add-AzSqlElasticJobTarget -ServerName $TargetServerName -Database $Db2.DatabaseName -Exclude
```

## <a name="create-a-job"></a>Bir iş oluşturma

```powershell
Write-Output "Creating a new job"
$JobName = "Job1"
$Job = $JobAgent | New-AzSqlElasticJob -Name $JobName -RunOnce
$Job
```

## <a name="create-a-job-step"></a>Bir iş adımı oluşturma

Bu örnek, işin çalışması için iki iş adımı tanımlar. Birinci iş adımı (*step1*), hedef *ServerGroup* grubundaki her veritabanında yeni bir tablo (*Step1Table*) oluşturur. İkinci iş adımı (*step2*), *TargetDb2* haricindeki tüm veritabanlarında yeni bir tablo (*Step2Table*) oluşturur. Bunun nedeni hedef grubun önceden bu veritabanını hariç tutacak şekilde [tanımlanmış](#define-the-target-databases-you-want-to-run-the-job-against) olmasıdır.

```powershell
Write-Output "Creating job steps"
$SqlText1 = "IF NOT EXISTS (SELECT * FROM sys.tables WHERE object_id = object_id('Step1Table')) CREATE TABLE [dbo].[Step1Table]([TestId] [int] NOT NULL);"
$SqlText2 = "IF NOT EXISTS (SELECT * FROM sys.tables WHERE object_id = object_id('Step2Table')) CREATE TABLE [dbo].[Step2Table]([TestId] [int] NOT NULL);"

$Job | Add-AzSqlElasticJobStep -Name "step1" -TargetGroupName $ServerGroup.TargetGroupName -CredentialName $JobCred.CredentialName -CommandText $SqlText1
$Job | Add-AzSqlElasticJobStep -Name "step2" -TargetGroupName $ServerGroupExcludingDb2.TargetGroupName -CredentialName $JobCred.CredentialName -CommandText $SqlText2
```


## <a name="run-the-job"></a>İşi çalıştırma

İşi hemen başlatmak için aşağıdaki komutu çalıştırın:

```powershell
Write-Output "Start a new execution of the job..."
$JobExecution = $Job | Start-AzSqlElasticJob
$JobExecution
```

İşlem başarıyla tamamlandıktan sonra TargetDb1 içinde iki yeni tablo, TargetDb2 içinde ise yalnızca bir yeni tablo görmeniz gerekir:


   ![SSMS'de yeni tablo doğrulaması](media/elastic-jobs-overview/job-execution-verification.png)




## <a name="monitor-status-of-job-executions"></a>İş yürütme durumunu izleme

Aşağıdaki kod parçacıkları, iş yürütme ayrıntılarını almaktadır:

```powershell
# Get the latest 10 executions run
$JobAgent | Get-AzSqlElasticJobExecution -Count 10

# Get the job step execution details
$JobExecution | Get-AzSqlElasticJobStepExecution

# Get the job target execution details
$JobExecution | Get-AzSqlElasticJobTargetExecution -Count 2
```

### <a name="job-execution-states"></a>İş yürütme durumları

Aşağıdaki tabloda, olası iş yürütme durumları listelenmiştir:

|Eyalet|Açıklama|
|:---|:---|
|**Oluşturulan** | İş yürütme yalnızca oluşturuldu ve henüz devam eden değil.|
|**Devam ediyor** | Şu anda iş yürütme işlemi devam ediyor.|
|**WaitingForRetry** | İş yürütme eylemi tamamlaması mümkün değildi ve yeniden denemek için bekliyor.|
|**Başarılı oldu** | İş yürütme başarıyla tamamlandı.|
|**SucceededWithSkipped** | İş yürütme başarıyla tamamlandı, ancak alt bazıları atlandı.|
|**Başarısız** | İş yürütme başarısız olup kendi yeniden deneme bitti.|
|**Zaman aşımına uğradı** | İş yürütme zaman aşımına uğradı.|
|**İptal edildi** | İş yürütme iptal edildi.|
|**Atlandı** | Aynı hedef aynı iş adımı başka bir yürütme zaten çalıştığından, iş yürütme atlandı.|
|**WaitingForChildJobExecutions** | İş yürütme tamamlanması için kendi alt yürütmeleri bekliyor.|

## <a name="schedule-the-job-to-run-later"></a>İşi daha sonra çalışmak üzere zamanlama

Bir işi belirli bir zamanda çalışacak şekilde zamanlamak için aşağıdaki komutu çalıştırın:

```powershell
# Run every hour starting from now
$Job | Set-AzSqlElasticJob -IntervalType Hour -IntervalCount 1 -StartTime (Get-Date) -Enable
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu silerek bu öğreticide oluşturulan Azure kaynaklarını silebilirsiniz.

> [!TIP]
> Bu işlerle çalışmaya devam etmeyi planlıyorsanız bu makalede oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu makalede oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.
>

```powershell
Remove-AzResourceGroup -ResourceGroupName $ResourceGroupName
```


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir veritabanı kümesinde Transact-SQL betiği çalıştırdınız.  Aşağıdaki görevleri nasıl gerçekleştireceğinizi öğrendiniz:

> [!div class="checklist"]
> * Elastik İş aracısı oluşturma
> * İşlerin hedeflerinde betik yürütebilmesi için iş kimlik bilgileri oluşturma
> * İşi çalıştırmak istediğiniz hedefleri (sunucular, elastik havuzlar, parça eşlemeleri) tanımlama
> * Aracının işlere bağlanıp yürütebilmesi için hedef veritabanlarında veritabanlı kapsamlı kimlik bilgileri oluşturma
> * Bir iş oluşturma
> * İşe bir iş adımı ekleme
> * Bir işi yürütmeye başlatma
> * İş izleme

> [!div class="nextstepaction"]
>[Elastik İşleri Transact-SQL kullanarak yönetme](elastic-jobs-tsql.md)
