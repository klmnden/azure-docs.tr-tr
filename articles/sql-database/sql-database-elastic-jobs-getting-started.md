---
title: Elastik veritabanı işleriyle çalışmaya başlama | Microsoft Docs
description: Birden çok veritabanını kapsayan T-SQL betiklerini yürütmek için elastik veritabanı işleri'ni kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 6d794fb14b7f581c9e9b92dc581de97e0a236630
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793768"
---
# <a name="getting-started-with-elastic-database-jobs"></a>Elastik veritabanı işleriyle çalışmaya başlama

Elastik veritabanı işleri (Önizleme) Azure SQL veritabanı için güvenilir bir şekilde otomatik olarak yeniden deneniyor ve son tamamlanma garantileri sağlama sırasında birden çok veritabanını kapsayan T-SQL betiklerini yürütme olanak tanır. Elastik veritabanı iş özelliği hakkında daha fazla bilgi için bkz. [esnek işler](sql-database-elastic-jobs-overview.md).

Bu makalede bulunan örnek genişletir [esnek veritabanı araçları ile çalışmaya başlama](sql-database-elastic-scale-get-started.md). Tamamlandığında, ilişkili veritabanlarından oluşan bir grupta yönetmenize işler oluşturma ve yönetme konusunda bilgi edinin. Esnek işler avantajlarından yararlanmak için esnek ölçeklendirme araçları kullanmak için gerekli değildir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

İndirme ve çalıştırma [esnek veritabanı araçları örnek ile kullanmaya](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Parça eşleme Yöneticisi örnek uygulaması kullanarak oluşturma

Burada Yöneticisi tarafından veri ekleme parçalara ardından birkaç parçalar ile birlikte bir parça eşlemesi oluşturun. Parçalı verileri ayarlama parçalar zaten varsa, aşağıdaki adımları atlayın ve sonraki bölüme Taşı.

1. Derleme ve çalıştırma **esnek veritabanı araçları ile çalışmaya başlama** örnek uygulama. Bölümündeki 7. adım kadar adımları [örnek uygulamasını indirme ve çalıştırma](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app). Adım 7 sonunda, aşağıdaki komut istemi görürsünüz:

   ![Komut İstemi](./media/sql-database-elastic-query-getting-started/cmd-prompt.png)

2. Komut penceresinde "1" yazın ve basın **Enter**. Parça eşleme Yöneticisi oluşturur ve iki parça sunucusuna ekler. Ardından, "3" yazın ve basın **Enter**; dört kez bu eylemi yineleyin. Bu örnek veri satırları, parçalarda ekler.
3. [Azure portalında](https://portal.azure.com) üç yeni veritabanları göstermelidir:

   ![Visual Studio onayı](./media/sql-database-elastic-query-getting-started/portal.png)

   Bu noktada, parça eşlemesi içindeki tüm veritabanlarına yansıtan bir özel bir veritabanı koleksiyonu oluşturun. Bu oluşturma ve parçalar arasında yeni bir tablo ekleyen bir iş yürütme olanak tanır.

Burada biz genellikle bir parça eşlemesi oluşturacak kullanarak hedef **yeni AzureSqlJobTarget** cmdlet'i. Parça eşleme Yöneticisi veritabanını, veritabanı hedef olarak ayarlamanız gerekir ve ardından belirli parça eşlemesi hedef olarak belirtilir. Sunucudaki tüm veritabanları sıralaması ve ana veritabanı dışında yeni özel koleksiyon veritabanları eklemek için bunun yerine, kullanacağız.

## <a name="creates-a-custom-collection-and-add-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Özel bir koleksiyon oluşturur ve ana hariç olmak üzere özel bir koleksiyona hedef sunucuda tüm veritabanları ekleyin

   ```powershell
    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName
    $dbsinserver | %{
    $currentdb = $_.DatabaseName
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target."
        }

        else
        {
            throw $_
        }

    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
   }
   ```

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Veritabanlarında yürütme için bir T-SQL betiği oluşturma

   ```powershell
    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script
   ```

## <a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>Veritabanlarından oluşan özel grupta bir betik yürütmek için iş oluşturma

   ```powershell
    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="execute-the-job"></a>İş yürütme

Aşağıdaki PowerShell Betiği, var olan bir işi yürütmek için kullanılabilir:

Aşağıdaki değişken yürüttünüz için istenen iş adı yansıtacak şekilde güncelleştirin:

   ```powershell
    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="retrieve-the-state-of-a-single-job-execution"></a>Tek iş yürütme durumunu alma

Aynı **Get-AzureSqlJobExecution** cmdlet'iyle **ıncludechildren'ın** parametresi, yani her veritabanında her iş yürütme için belirli durumu olan alt iş yürütmeleri durumunu görüntülemek için İş tarafından hedeflenen.

   ```powershell
    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions
   ```

## <a name="view-the-state-across-multiple-job-executions"></a>Birden çok iş yürütmeleri arasında durumunu görüntüleyin

**Get-AzureSqlJobExecution** cmdlet'i sağlanan parametreler filtre birden çok iş yürütmeleri görüntülemek için kullanılan birden fazla isteğe bağlı parametreye sahiptir. Aşağıdaki Get-AzureSqlJobExecution kullanmak mümkün yollardan bazılarını göstermektedir:

Tüm etkin en üst düzey iş yürütmeleri alın:

   ```powershell
    Get-AzureSqlJobExecution
   ```

Etkin olmayan iş yürütme sayısı dahil olmak üzere tüm üst düzey iş yürütmeleri alın:

   ```powershell
    Get-AzureSqlJobExecution -IncludeInactive
   ```

Tüm alt iş yürütme sayısı, etkin olmayan iş yürütme sayısı dahil olmak üzere belirtilen iş yürütme kimliği Al:

   ```powershell
    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren
   ```

Bir zamanlama kullanılarak oluşturulan tüm iş yürütmeleri almak / etkin olmayan işler de dahil olmak üzere birlikte, iş:

   ```powershell
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive
   ```

Etkin olmayan işler de dahil olmak üzere bir belirtilen parça eşlemesini hedefleyen tüm işleri Al:

   ```powershell
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Etkin olmayan işler de dahil olmak üzere belirtilen özel bir koleksiyonu hedefleyen tüm işleri Al:

   ```powershell
    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Belirli iş yürütme içinde iş görevi yürütme listesini alın:

   ```powershell
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions
   ```

İş görevi yürütme ayrıntılarını alın:

Aşağıdaki PowerShell betiğini yürütme hatalarını hata ayıklama sırasında özellikle yararlı olur. bir iş görevi yürütmede ayrıntılarını görüntülemek için kullanılabilir.

   ```powershell
    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution
   ```

## <a name="retrieve-failures-within-job-task-executions"></a>Görev yürütme iş içindeki hataların alma

JobTaskExecution nesneyi bir özelliği yaşam döngüsü boyunca bir ileti özelliği ile birlikte bir görev içerir. Bir iş görevi yürütmede başarısız olursa, yaşam döngüsü özellik kümesine *başarısız* ve ortaya çıkan özel durum iletisi, yığın için ileti özelliği ayarlanır. Bir işi başarılı olmadıysa için belirli bir işin başarılı olmadı iş görevleri ayrıntılarını görüntülemek önemlidir.

   ```powershell
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions)
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }
   ```

## <a name="waiting-for-a-job-execution-to-complete"></a>Bir iş yürütme tamamlanması bekleniyor

Aşağıdaki PowerShell Betiği, bir işi görevin tamamlanmasını beklemek için kullanılabilir:

   ```powershell
    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="create-a-custom-execution-policy"></a>Bir özel yürütme ilkesi oluşturma

Elastik veritabanı işleri destekler işler başlatılırken uygulanabilen özel yürütme ilkeleri oluşturma.

Şu anda yürütme ilkelerini tanımlamak için izin ver:

* Ad: Yürütme ilkesi için tanımlayıcı.
* İşi zaman aşımı: Elastik veritabanı işleri tarafından bir işi iptal edilmeden önce toplam süresi.
* İlk yeniden deneme aralığı: İlk yeniden denemeden önce beklenecek aralık.
* En fazla yeniden deneme aralığı: Kullanılacak yeniden deneme aralıklarını sınır.
* Yeniden deneme aralığı geri alma katsayısı: Katsayısı, sonraki yeniden deneme aralığını hesaplamak için kullanılır.  Aşağıdaki formülü kullanılır: (İlk yeniden deneme aralığı) * Math.pow ((aralık geri alma katsayısı) (yeniden deneme sayısı) - 2).
* En fazla denemesi: İçinde bir işi gerçekleştirmek en fazla yeniden deneme sayısı çalışır.

Varsayılan yürütme ilkesi aşağıdaki değerleri kullanır:

* Ad: Varsayılan yürütme İlkesi
* İşi zaman aşımı: 1 hafta
* İlk yeniden deneme aralığı:  100 milisaniye
* En fazla yeniden deneme aralığı: 30 dakika
* Yeniden deneme aralığı katsayısı: 2
* En fazla denemesi: 2,147,483,647

İstenen yürütme ilkesi oluşturun:

   ```powershell
    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy
   ```

### <a name="update-a-custom-execution-policy"></a>Bir özel yürütme ilkesini güncelleştirme

Güncelleştirme için istenen yürütme ilkesini güncelleştirin:

   ```powershell
    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
   ```

## <a name="cancel-a-job"></a>Bir işi iptal et

Elastik veritabanı işleri işleri iptal isteklerini destekler.  Elastik veritabanı işleri şu anda yürütülmekte olan iş için bir iptal isteğine algılarsa, işi durdurmak çalışır.

Elastik veritabanı işleri iptal gerçekleştirebilirsiniz iki farklı yolu vardır:

1. Yürütülmekte olan görevleri iptal ediliyor: Görev şu anda çalışırken iptal algılanırsa, bir iptal şu anda yürütülen görev görünümü içinde denenir.  Örneğin: İptal durumunda çalışırken şu anda gerçekleştirilmekte olan uzun süre çalışan bir sorgu varsa, sorgusunu iptal etme girişimi yoktur.
2. Görev iptal yeniden deneme sayısı: Yürütme için bir görev başlatılmadan önce iptal denetimi iş parçacığı tarafından algılanırsa, denetimi iş parçacığı görevi başlatma önler ve istek iptal edildi olarak bildirir.

Üst iş için bir işi iptal istenirse, iptal isteğini üst işin ve tüm alt işlerini için uygulanır.

İptal isteği göndermek için kullanabilir **Stop-AzureSqlJobExecution** cmdlet'i ve **JobExecutionId** parametresi.

   ```powershell
    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>İş adı ve işin geçmişini sil

Elastik veritabanı işleri, zaman uyumsuz işler silinmesini destekler. Bir işi silinmek üzere işaretlenmiş ve tüm iş yürütmeleri için işini tamamladıktan sonra sistem işin ve buna ait iş geçmişi siler. Sistem etkin iş yürütmeleri otomatik olarak iptal etmez.  

Bunun yerine, Stop-AzureSqlJobExecution etkin iş yürütme iptal etmek için çağrılması gerekir.

Proje silme işlemi tetiklemek için kullanmak **Remove-AzureSqlJob** cmdlet'i ve **JobName** parametresi.

   ```powershell
    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
   ```

## <a name="create-a-custom-database-target"></a>Özel veritabanını hedef oluşturun

Özel veritabanını hedefler, doğrudan yürütme için veya içinde bir özel bir veritabanı grubu eklemek için kullanılabilecek, esnek veritabanı işleri içinde tanımlanabilir. Bu yana **elastik havuzlar** doğrudan henüz desteklenmeyen bazı PowerShell API'leri üzerinden, yalnızca özel veritabanı hedef ve havuzdaki tüm veritabanları kapsayan, özel bir veritabanı koleksiyonu hedef oluşturun.

İstenen veritabanı bilgileri yansıtacak şekilde aşağıdaki değişkenleri ayarlayın:

   ```powershell
    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName
   ```

## <a name="create-a-custom-database-collection-target"></a>Özel bir veritabanı koleksiyonu hedef oluşturun

Özel bir veritabanı koleksiyonu hedef arasında birden çok tanımlanmış veritabanı hedef yürütme etkinleştirmek için tanımlanabilir. Bir veritabanı grubu oluşturulduktan sonra özel toplama hedef veritabanları ilişkilendirilebilir.

Özel bir koleksiyona istenen hedef yapılandırmayı yansıtacak şekilde aşağıdaki değişkenleri ayarlayın:

   ```powershell
    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="add-databases-to-a-custom-database-collection-target"></a>Özel bir veritabanı koleksiyonu hedefe veritabanlarını ekleme

Veritabanı hedefleri veritabanlarından oluşan bir grupta oluşturmak için özel bir veritabanı koleksiyonu hedefleri ile ilişkilendirilebilir. Özel bir veritabanı koleksiyonu hedef hedefleyen bir iş oluşturulduğunda, yürütme sırasında Grup ile ilişkili veritabanlarını hedefleyecek şekilde genişletilir.

İstenen veritabanı, belirli özel bir koleksiyona ekleyin:

   ```powershell
    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName
   ```

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Özel bir veritabanı koleksiyonu hedef içinde veritabanlarını gözden geçirin

Kullanım **Get-AzureSqlJobTarget** özel bir veritabanı koleksiyonu hedef içinde alt veritabanlarını almak için cmdlet'i.

   ```powershell
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets
   ```

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Özel bir veritabanı koleksiyonu hedef arasında bir betik yürütmek için bir iş oluşturma

Kullanım **yeni AzureSqlJob** özel bir veritabanı koleksiyonu hedef tarafından tanımlanan veritabanlarından oluşan bir grupta karşı bir iş oluşturmak için cmdlet'i. Elastik veritabanı işleri, her bir veritabanına karşılık gelen özel bir veritabanı koleksiyonu hedefle ilişkili birden çok alt işlerin iş genişletir ve komut dosyası her bir veritabanına karşı yürütülür emin olun. Yeniden betikleri deneme dayanıklı olacak şekilde etkili olması önemlidir.

   ```powershell
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="data-collection-across-databases"></a>Veritabanları arasında veri toplama

**Elastik veritabanı işleri** veritabanlarından oluşan bir grupta Sorguyu yürüten destekler ve belirtilen veritabanının tabloya sonuçları gönderir. Tablo, her veritabanı sorgunun sonuçlarını görmek için çözümledikten sonra sorgulanabilir. Bu, çok sayıda veritabanı arasında sorgu yürütme için zaman uyumsuz bir mekanizma sağlar. Hata koşulları gibi geçici olarak devre dışı olan veritabanlarından birini otomatik olarak yeniden deneme işlenir.

Bunu henüz, döndürülen sonuç kümesi şeması eşleştirme yoksa, belirtilen hedef tablo otomatik olarak oluşturulur. Betik yürütme birden çok sonuç kümesi döndürürse, elastik veritabanı işleri yalnızca gönderir birincisine sağlanan hedef tablo.

Aşağıdaki PowerShell Betiği, belirtilen bir tabloya sonuçlarını toplamak için bir betik yürütmek için kullanılabilir. Bu betik, tek bir sonuç kümesi çıkışı bir T-SQL betiği oluşturuldu ve özel bir veritabanı koleksiyonu hedef oluşturduğunuz varsayılır.

İstenen komut, kimlik bilgilerini ve yürütme hedefi yansıtmak için aşağıdakileri ayarlayın:

   ```powershell
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Oluşturma ve veri koleksiyonu senaryoları için bir iş başlatma

   ```powershell
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Bir işi tetikleyici kullanarak iş yürütme için bir zamanlama oluşturun

Aşağıdaki PowerShell Betiği, yinelenen bir zamanlama oluşturmak için kullanılabilir. Bu betik, bir dakikalık bir aralığı kullanır, ancak yeni AzureSqlJobSchedule - Dayınterval, - HourInterval, - MonthInterval ve - WeekInterval parametreleri de destekler. Yalnızca bir kere yürütülen zamanlamaları oluşturulabilir geçirilerek - OneTime.

Yeni bir zamanlama oluşturur:

   ```powershell
    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime
    Write-Output $schedule
   ```

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Bir zaman planlamada yürütülen bir işi bir iş tetikleyicisi oluşturma

Bir işi tetikleyici, bir zaman zamanlamaya göre çalıştırılan bir iş için tanımlanabilir. Aşağıdaki PowerShell Betiği, bir iş tetikleyicisi oluşturmak için kullanılabilir.

Zamanlama ve istenen iş karşılık gelecek şekilde aşağıdaki değişkenleri ayarlayın:

   ```powershell
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
    Write-Output $jobTrigger
   ```

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>İş, zamanlamaya göre yürütülmesini durdurmak için zamanlanmış bir ilişkilendirmesini Kaldır

Yinelenen iş yürütme işi tetikleyici aracılığıyla kesmek için iş tetikleyici kaldırılabilir.
Bir zamanlamaya göre çalıştırılmasını bir işi durdurmak için bir iş tetikleyiciyi kaldırmak **Remove-AzureSqlJobTrigger** cmdlet'i.

   ```powershell
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
   ```

## <a name="import-elastic-database-query-results-to-excel"></a>Excel için elastik veritabanı sorgusu sonuçlarını Al

 Gelen bir sorgunun sonuçlarının bir Excel dosyasına aktarabilirsiniz.

1. Excel 2013'ün başlatın.
2. Gidin **veri** Şerit.
3. Tıklayın **diğer kaynaklardan** tıklatıp **SQL Server'dan**.

   ![Excel Import diğer kaynaklardan](./media/sql-database-elastic-query-getting-started/exel-sources.png)

4. İçinde **Veri Bağlantı Sihirbazı'nı** sunucu adını ve oturum açma kimlik bilgilerini yazın. Ardından **İleri**'ye tıklayın.
5. İletişim kutusunda **istediğiniz verileri içeren veritabanını seçin**seçin **ElasticDBQuery** veritabanı.
6. Seçin **müşteriler** tıklayın ve liste görünümünde tablo **sonraki**. Ardından **son**.
7. İçinde **verileri içeri aktarma** formunda, altında **bu verileri çalışma kitabınızı görüntüleme istediğiniz şekli seçin**seçin **tablo** tıklatıp **Tamam**.

Tüm satırların **müşteriler** tabloda, farklı parçalarda depolanan Excel sayfası doldurun.

## <a name="next-steps"></a>Sonraki adımlar

Excel'in veri işlevleri artık kullanabilirsiniz. Bağlantı dizesi, BI ve veri tümleştirme araçları esnek sorgu veritabanına bağlanmak için sunucu adı, veritabanı adı ve kimlik bilgilerini kullanın. SQL Server'ın aracınız için bir veri kaynağı olarak desteklendiğinden emin olun. Esnek sorgu veritabanı ve herhangi bir SQL Server veritabanı gibi dış tablolar ve aracınızla bağlanacağı SQL Server tablolarına bakın.

### <a name="cost"></a>Maliyet

Elastik veritabanı sorgusu özelliğini kullanmak için ek ücret yoktur. Ancak, şu anda bu özellik bir uç noktası olarak yalnızca Premium ve iş açısından kritik veritabanları ve elastik havuzlar kullanılabilir, ancak parçalar herhangi bir hizmet katmanı olabilir.

Fiyatlandırma bilgileri için bkz: [SQL veritabanı fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/sql-database/).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->