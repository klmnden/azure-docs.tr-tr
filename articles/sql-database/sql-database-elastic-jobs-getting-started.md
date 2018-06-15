---
title: Esnek veritabanı işlerine Başlarken | Microsoft Docs
description: Birden çok veritabanı span T-SQL betikleri çalıştırmak için esnek veritabanı işlerini kullanın.
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 4f12c3353ca4949b3c1c031420ec5a0b8fdb2dbf
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34649161"
---
# <a name="getting-started-with-elastic-database-jobs"></a>Esnek veritabanı işlerine Başlarken
Esnek veritabanı iş (Önizleme) Azure SQL veritabanı için güvenilir bir şekilde birden fazla veritabanı otomatik olarak yeniden deneniyor ve nihai tamamlama garanti sağlama sırasında span T-SQL betikleri çalıştırmak sağlar. Esnek veritabanı iş özelliği hakkında daha fazla bilgi için bkz: [esnek iş](sql-database-elastic-jobs-overview.md).

Bu makalede bulunan örnek genişletir [esnek veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md). Tamamlandığında, bir grup ilişkili veritabanlarını yönetmek işleri oluşturmak ve yönetmek nasıl öğrenin. Esnek iş avantajlarından yararlanmak için esnek ölçek araçlarını kullanmak için gerekli değildir.

## <a name="prerequisites"></a>Önkoşullar
İndirme ve çalıştırma [esnek veritabanı araçlarını örneği ile çalışmaya başlama](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Harita manager örnek uygulamasını kullanarak bir parça oluşturma
Burada birkaç parça parça veri ekleme tarafından izlenen, birlikte Yöneticisi bir parça eşleme oluşturun. Parçalı veriler bunlara ayarlayın parça zaten varsa, aşağıdaki adımları atlayın ve sonraki bölüme taşıyın.

1. Derleme ve çalıştırma **esnek veritabanı araçlarını kullanmaya başlama** örnek uygulama. Adım 7 bölümünde kadar adımları [örnek uygulamasını indirme ve çalıştırma](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app). Adım 7 sonunda, aşağıdaki komut istemi görürsünüz:

   ![Komut İstemi](./media/sql-database-elastic-query-getting-started/cmd-prompt.png)

2. Komut penceresinde "1" yazın ve tuşuna basın **Enter**. Bu parça eşleme Yöneticisi oluşturur ve iki parça sunucusuna ekler. Daha sonra "3" yazın ve basın **Enter**; Bu eylem dört kez yineler. Bu örnek verileri satır, parça ekler.
3. [Azure portal](https://portal.azure.com) üç yeni veritabanları göstermesi gerekir:

   ![Visual Studio onayı](./media/sql-database-elastic-query-getting-started/portal.png)

   Bu noktada, parça eşlemindeki tüm veritabanları yansıtan özel veritabanını koleksiyonu oluşturun. Bu oluşturma ve yeni bir tablo arasında parça ekleyen bir iş yürütme olanak tanır.

Burada size genellikle bir parça eşleme oluşturacak kullanarak hedef **yeni AzureSqlJobTarget** cmdlet'i. Parça eşleme manager veritabanı, veritabanı hedefi olarak ayarlamanız gerekir ve ardından belirli parça eşleme hedef olarak belirtilir. Bunun yerine, biz sunucudaki tüm veritabanları numaralandırır ve veritabanlarını yeni özel koleksiyon ana veritabanı dışında eklemek adımıdır.

## <a name="creates-a-custom-collection-and-add-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Özel bir koleksiyon oluşturur ve tüm veritabanları ana hariç olmak üzere özel koleksiyon hedef sunucu ekleyin.
   ```
    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName
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
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Veritabanları arasında yürütme için T-SQL komut dosyası oluşturma
   ```
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

## <a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>Veritabanları özel grup arasında bir betik yürütmek için proje oluşturma

   ```
    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="execute-the-job"></a>İş yürütme
Aşağıdaki PowerShell betiğini, varolan bir projeyi yürütmek için kullanılabilir:

Aşağıdaki değişkeni istenen iş yürütülmesini yansıtacak şekilde güncelleştirin:

   ```
    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="retrieve-the-state-of-a-single-job-execution"></a>Tek iş yürütme durumunu alır
Aynı **Get-AzureSqlJobExecution** cmdlet'iyle **ıncludechildren'ın** öğesine her iş yürütme iş tarafından hedeflenen her bir veritabanına karşı özel durumu olan alt iş yürütmeleri durumunu görüntülemek için parametre.

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions
   ```

## <a name="view-the-state-across-multiple-job-executions"></a>Birden çok iş yürütmeleri arasında durumunu görüntüleyin
**Get-AzureSqlJobExecution** cmdlet'i aracılığıyla sağlanan parametreleri filtre birden çok iş yürütmeleri görüntülemek için kullanılan birden fazla isteğe bağlı parametreler vardır. Get-AzureSqlJobExecution kullanmak için olası yollardan bazılarını şunlar gösterilmektedir:

Tüm etkin en üst düzey iş yürütmeleri Al:

   ```
    Get-AzureSqlJobExecution
   ```

Etkin olmayan iş yürütmeleri dahil olmak üzere tüm üst düzey iş yürütmeleri Al:

   ```
    Get-AzureSqlJobExecution -IncludeInactive
   ```

Etkin olmayan iş yürütmeleri dahil olmak üzere sağlanan iş yürütme kimliği, tüm alt iş yürütmeleri Al:

   ```
    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren
   ```

Bir zamanlama kullanılarak oluşturulan tüm iş yürütmeleri almak / birleşimi etkin olmayan işler de dahil olmak üzere, iş:

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive
   ```

Etkin olmayan işler de dahil olmak üzere bir belirtilen parça eşleme hedefleme tüm işleri Al:

   ```
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Etkin olmayan işler de dahil olmak üzere belirtilen özel bir koleksiyonu hedefleyen tüm işleri Al:

   ```
    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Belirli iş yürütme içinde iş görev yürütmeleri listesini al:

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions
   ```

İş görev yürütme ayrıntıları alın:

Aşağıdaki PowerShell betiğini yürütme hatalarını ayıklama özellikle yararlıdır iş görevi yürütmede ayrıntılarını görüntülemek için kullanılabilir.
   ```
    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution
   ```

## <a name="retrieve-failures-within-job-task-executions"></a>İş görevi yürütmeleri içinde hataları alma
JobTaskExecution nesnesi, görev bir ileti özelliği birlikte yaşam döngüsü için bir özellik içerir. Bir iş görevi yürütmede başarısız olduysa, yaşam döngüsü özellik kümesine *başarısız* ve sonuçta elde edilen özel durum iletisi, yığın için ileti özelliği ayarlanır. Bir işi başarısız oldu, belirli bir iş için başarılı olmadı iş görevleri ayrıntılarını görüntülemek önemlidir.

   ```
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

## <a name="waiting-for-a-job-execution-to-complete"></a>Bir iş yürütmenin tamamlanması bekleniyor
Aşağıdaki PowerShell betiğini bir iş görevinin tamamlanmasını beklemek için kullanılabilir:

   ```
    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="create-a-custom-execution-policy"></a>Özel yürütme ilkesi oluşturma
Esnek veritabanı iş destekler işleri başlatırken uygulanan özel yürütme ilkelerini oluşturma.

Yürütme ilkelerini tanımlamak için şu anda izin ver:

* Adı: Yürütme İlkesi tanımlayıcısı.
* İş zaman aşımı: bir iş tarafından esnek veritabanı iş iptal etmeden önce toplam süre.
* İlk yeniden deneme aralığı: ilk yeniden denemeden önce beklenecek aralığı.
* En fazla yeniden deneme aralığı: kullanılacak Cap yeniden deneme aralıkları.
* Aralık geri Çekilme katsayısı yeniden deneme: Katsayısı sonraki yeniden deneme aralığını hesaplamak için kullanılır.  Aşağıdaki formül kullanılır: (ilk yeniden deneme aralığı) * Math.pow ((aralığı geri Çekilme katsayısı) (yeniden deneme sayısı) - 2).
* En fazla deneme: En fazla yeniden deneme sayısını içinde bir işi gerçekleştirmek çalışır.

Varsayılan yürütme ilkesi aşağıdaki değerleri kullanır:

* Ad: Varsayılan yürütme İlkesi
* İş zaman aşımı: 1 hafta
* İlk yeniden deneme aralığı: 100 milisaniyede
* En fazla yeniden deneme aralığı: 30 dakika
* Yeniden deneme aralığı katsayısı: 2
* En fazla deneme: 2.147.483.647

İstenen yürütme ilkesi oluşturun:

   ```
    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy
   ```

### <a name="update-a-custom-execution-policy"></a>Özel yürütme ilkesini güncelleştirin
Güncelleştirmek için istenen yürütme İlkesi güncelleştirmesi:

   ```
    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
   ```

## <a name="cancel-a-job"></a>Bir işi iptal etme
Esnek veritabanı iş işleri iptal isteklerini destekler.  Esnek veritabanı iş şu anda yürütülmekte olan bir işi iptal isteği algılarsa, işi durdurma dener.

Esnek veritabanı iş iptal gerçekleştirebilirsiniz iki farklı yolu vardır:

1. Şu anda yürütülmekte olan iptal etme görevleri: bir görevi şu anda çalışırken iptal algılanırsa, görevi şu anda yürütülen en boy içinde iptal denenir.  Örneğin: iptal çalışırken şu anda gerçekleştirilen uzun süre çalışan bir sorgusu ise, sorguyu iptal etmek için bir girişim yoktur.
2. İptal etme görev yeniden deneme: iptal denetimi iş parçacığı tarafından algılanırsa, bir görev için yürütme başlatılmadan önce denetimi iş parçacığı görev başlatmasını önler ve istek iptal edildi olarak bildirin.

İş iptali için üst iş istediyseniz iptal isteğini üst iş ve tüm alt işlerini'de de dikkate alınır.

İptal isteği göndermek için kullanmak **Stop-AzureSqlJobExecution** cmdlet'i ve **JobExecutionId** parametresi.

   ```
    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>İş adı ve iş geçmişini sil
Esnek veritabanı iş işleri zaman uyumsuz silinmesini destekler. Bir işi silinmek üzere işaretlenmiş ve tüm iş yürütmeleri için işini tamamladıktan sonra sistem iş ve tüm iş geçmişini siler. Sistem etkin iş yürütmeleri otomatik olarak iptal edilmez.  

Bunun yerine, Dur AzureSqlJobExecution etkin iş yürütmeleri iptal etmek için çağrılması gerekir.

İş silme tetiklemek için kullanabileceğiniz **Kaldır AzureSqlJob** cmdlet'i ve ayarlayın **JobName** parametresi.

   ```
    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
   ```

## <a name="create-a-custom-database-target"></a>Özel veritabanını hedef oluşturma
Özel veritabanı hedefleri yürütme doğrudan veya bir özel veritabanı grubundaki eklemek için kullanılabilen esnek veritabanı işlerinde tanımlanabilir. Bu yana **esnek havuzlar** henüz doğrudan desteklenmeyen PowerShell API'leri, yalnızca bir özel veritabanı hedef ve havuzdaki tüm veritabanları kapsayan özel veritabanı koleksiyon hedef oluşturun.

İstenen veritabanı bilgilerini yansıtmak için aşağıdaki değişkenleri ayarlayın:

   ```
    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName
   ```

## <a name="create-a-custom-database-collection-target"></a>Özel veritabanı koleksiyon hedef oluşturma
Özel veritabanı koleksiyon hedef yürütme arasında birden çok tanımlanmış veritabanı hedefleri etkinleştirmek için tanımlanabilir. Bir veritabanı grubu oluşturulduktan sonra özel toplama hedef veritabanları ilişkilendirilebilir.

İstenen özel koleksiyon hedef yapılandırmayı yansıtacak şekilde aşağıdaki değişkenleri ayarlayın:

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="add-databases-to-a-custom-database-collection-target"></a>Özel veritabanını koleksiyonu hedefe veritabanları ekleme
Veritabanı hedefleri veritabanlarının bir grup oluşturmak için özel veritabanı koleksiyon hedefleri ile ilişkili olabilir. Özel veritabanı koleksiyon hedef hedefleyen bir iş oluşturulduğunda, yürütme sırasında grubu için ilişkili veritabanlarını hedeflemek için genişletilir.

İstenen veritabanı belirli özel bir koleksiyona ekleyin:

   ```
    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName
   ```

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Özel veritabanı koleksiyon hedef içinde veritabanları gözden geçirin
Kullanım **Get-AzureSqlJobTarget** özel veritabanı koleksiyon hedef içinde alt veritabanları almak üzere.

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets
   ```

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Özel veritabanı koleksiyon hedef arasında bir betik yürütmek için bir proje oluşturun
Kullanım **yeni AzureSqlJob** özel veritabanı koleksiyon hedef tarafından tanımlanan veritabanlarının bir gruba göre bir iş oluşturmak için cmdlet'i. Esnek veritabanı iş her bir veritabanına karşılık gelen özel veritabanı koleksiyon hedefle ilişkili birden çok alt işlere iş genişletir ve komut dosyası her veritabanına karşı yürütülür emin olun. Yeniden, komut dosyaları, deneme dayanıklı olmasını ıdempotent olduğunu önemlidir.

   ```
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="data-collection-across-databases"></a>Veritabanları arasında veri toplama
**Esnek veritabanı iş** grubu bir sorgu yürütülürken destekler ve sonuçları belirtilen veritabanının tabloya gönderir. Tablonun her veritabanından sorgunun sonuçlarını görmek için Olgu sonra sorgulanabilir. Bu, birçok veritabanı arasında bir sorguyu yürütmek için zaman uyumsuz bir mekanizma sağlar. Geçici olarak devre dışı bırakılıyor veritabanlarından birini gibi hata durumları otomatik olarak yeniden deneme işlenir.

Bunu henüz, döndürülen sonuç kümesi şemasını eşleştirme yoksa, belirtilen hedef tablo otomatik olarak oluşturulur. Bir komut dosyası yürütme birden çok sonuç kümesi döndürürse, esnek veritabanı işleri yalnızca gönderir birinciye sağlanan hedef tablo.

Aşağıdaki PowerShell komut dosyası sonuçlarını belirtilen tabloya toplama bir betik yürütmek için kullanılabilir. Bu komut dosyasını bir T-SQL betiği, tek bir sonuç kümesi çıkarır oluşturulup oluşturulmadığını ve bir özel veritabanı koleksiyon hedef oluşturulan varsayar.

İstenen komut, kimlik bilgileri ve yürütme hedef yansıtmak için aşağıdakileri ayarlayın:

   ```
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

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Oluşturma ve veri toplama senaryoları için bir işi başlatma
   ```
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Bir iş tetikleyici kullanarak iş yürütme için bir zamanlama oluşturmak
Aşağıdaki PowerShell betiğini yeniden bir zamanlama oluşturmak için kullanılabilir. Bu komut dosyasını bir dakikalık bir zaman aralığı kullanır, ancak yeni AzureSqlJobSchedule - DayInterval, - HourInterval, - MonthInterval ve - WeekInterval parametreleri de destekler. Yalnızca bir kez yürütme zamanlamaları oluşturulabilir göre geçirme - kez.

Yeni bir zamanlama oluşturun:
   ```
    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime
    Write-Output $schedule
   ```

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Bir zaman zamanlamaya göre çalıştırılan bir iş için bir iş Tetikleyici oluşturma
Bir iş tetikleyici bir saat zamanlamaya göre çalıştırılan bir iş için tanımlanabilir. Aşağıdaki PowerShell betiğini bir işi tetikleyici oluşturmak için kullanılabilir.

İstenen iş ve zamanlama karşılık gelen için aşağıdaki değişkenleri ayarlayın:

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
    Write-Output $jobTrigger
   ```

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Zamanlamaya göre yürütülmesini işini durdurmak için zamanlanmış bir ilişkilendirmesini Kaldır
Bir iş tetikleyici aracılığıyla yeniden iş yürütme kesmek için iş tetikleyici kaldırılabilir.
Bir zamanlamaya göre çalıştırılmasını bir işi durdurmak için bir iş tetikleyiciyi kaldırmak **Kaldır AzureSqlJobTrigger** cmdlet'i.

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
   ```

## <a name="import-elastic-database-query-results-to-excel"></a>Esnek veritabanı sorgu sonuçları Excel'e Al
 Gelen bir sorgunun sonuçlarını bir Excel dosyasını içeri aktarabilirsiniz.

1. Excel 2013'ü başlatın.
2. Gidin **veri** Şerit.
3. Tıklatın **diğer kaynaklardan** tıklatıp **SQL Server'dan**.

   ![Excel Import diğer kaynaklardan](./media/sql-database-elastic-query-getting-started/exel-sources.png)

4. İçinde **Veri Bağlantı Sihirbazı** sunucu adını ve oturum açma kimlik bilgilerini yazın. Ardından **İleri**'ye tıklayın.
5. İletişim kutusunda **istediğiniz verileri içeren bir veritabanı seçin**seçin **ElasticDBQuery** veritabanı.
6. Seçin **müşteriler** Tablo liste görünümünde ve tıklayın **sonraki**. Ardından **son**.
7. İçinde **veri içeri aktarma** formunda, altında **nasıl çalışma kitabınızı bu verileri görüntülemek istediğinizi seçin**seçin **tablo** tıklatıp **Tamam**.

Tüm satırların **müşteriler** tablo, farklı parça içinde depolanan doldurmak Excel sayfası.

## <a name="next-steps"></a>Sonraki adımlar
Artık, Excel'in veri işlevleri de kullanabilirsiniz. Bağlantı dizesi, BI ve veri tümleştirme araçları esnek sorgu veritabanına bağlanmak için sunucu adı, veritabanı adının ve kimlik bilgilerini kullanın. SQL Server, aracı için bir veri kaynağı olarak desteklendiğinden emin olun. Herhangi bir SQL Server veritabanı gibi dış tablolar ve aracı ile bağlanacağı SQL Server tablolarını ve esnek sorgu veritabanı bakın.

### <a name="cost"></a>Maliyet
Esnek veritabanı sorgu özelliğini kullanmak için ek ücret yoktur. Ancak, şu anda bu özellik yalnızca Premium ve iş kritik (Önizleme) üzerinde kullanılabilir veritabanları ve esnek havuzlar bir uç noktası, ancak parça olarak herhangi bir hizmet katmanı olabilir.

Fiyatlandırma bilgileri için bkz: [SQL veritabanı fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/sql-database/).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
