---
title: PowerShell kullanarak elastik işler oluşturma ve yönetme | Microsoft Docs
description: Azure SQL veritabanı havuzlarını yönetmek için kullanılan PowerShell
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: pwershell
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 06/14/2018
ms.openlocfilehash: 36b03794f4b55af3de89f96ecee02f5542f40f01
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52972108"
---
# <a name="create-and-manage-sql-database-elastic-jobs-using-powershell-preview"></a>PowerShell (Önizleme) kullanarak SQL veritabanı esnek işler oluşturma ve yönetme


[!INCLUDE [elastic-database-jobs-deprecation](../../includes/sql-database-elastic-jobs-deprecate.md)]


İçin PowerShell API'lerini **elastik veritabanı işleri** (Önizleme), komut dosyaları çalıştırılacağı karşı veritabanlarından oluşan bir grupta tanımlamanıza olanak sağlar. Bu makale oluşturmak ve yönetmek nasıl **elastik veritabanı işleri** PowerShell cmdlet'lerini kullanarak. Bkz: [esnek işler genel bakış](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Önkoşullar
* Azure aboneliği. Ücretsiz deneme için bkz: [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Esnek veritabanı araçları ile oluşturulmuş bir veritabanları kümesi. Bkz: [esnek veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md).
* Azure PowerShell. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/overview).
* **Elastik veritabanı işleri** PowerShell paketi: bkz [yükleme elastik veritabanı işleri](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Azure aboneliğinizi seçin
Aboneliği seçmek için abonelik kimliği gerekir. (**- Subscriptionıd**) veya abonelik adı (**- SubscriptionName**). Birden fazla aboneliğiniz varsa çalıştırabileceğiniz **Get-AzureRmSubscription** cmdlet ve kopyalama sonuç istenen abonelik bilgilerini ayarlayın. Abonelik bilgilerinizi aldıktan sonra Bu abonelik, işleri oluşturmaya ve yönetmeye için hedef özelliği varsayılan olarak ayarlamak için aşağıdaki cmdlet'i çalıştırın:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

[PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) geliştirin ve elastik veritabanı işleri PowerShell betiklerini yürütmek kullanım için tavsiye edilir.

## <a name="elastic-database-jobs-objects"></a>Elastik veritabanı işleri nesneleri
Aşağıdaki tabloda tüm nesne türlerini **elastik veritabanı işleri** açıklaması ve ilgili PowerShell API'lerini birlikte.

<table style="width:100%">
  <tr>
    <th>Nesne Türü</th>
    <th>Açıklama</th>
    <th>İlgili PowerShell API'leri</th>
  </tr>
  <tr>
    <td>Kimlik Bilgisi</td>
    <td>Kullanıcı adı ve parola betiklerinin çalıştırılmasını veya DACPACs uygulamasının veritabanlarına bağlanırken kullanılacak. <p>Parola gönderme ve elastik veritabanı işleri veritabanında saklamanın önce şifrelenir.  Parola kimlik bilgisi oluşturulur ve karşıya yükleme betiği aracılığıyla elastik veritabanı işleri hizmeti tarafından şifresi çözülür.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>New-AzureSqlJobCredential</p><p>Set-AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Betik</td>
    <td>Veritabanlarında çalıştırması için kullanılacak, transact-SQL betiği.  Hizmet hataları sırasında betik yürütme işlemi yeniden deneyin bu yana kere etkili olacak şekilde betiği yeniden yazılması.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>New-AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Veri katmanı uygulaması </a> paket veritabanlarında uygulanacak.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>New-AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Hedef veritabanı</td>
    <td>Bir Azure SQL veritabanı'na işaret eden veritabanı ve sunucu adı.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>New-AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Parça eşleme hedef</td>
    <td>Bir veritabanı hedef ve bir esnek veritabanı parça eşlemesi içinde depolanan bilgileri belirlemek için kullanılacak kimlik bilgisini birleşimi.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>New-AzureSqlJobTarget</p>
    <p>Set-AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Özel bir koleksiyona hedef</td>
    <td>Toplu yürütme için kullanılacak veritabanı tanımlanmış grubu.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>New-AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Özel bir koleksiyona alt hedef</td>
    <td>Özel bir koleksiyondan başvurulan veritabanı hedefi.</td>
    <td>
    <p>Add-AzureSqlJobChildTarget</p>
    <p>Remove-AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>İş</td>
    <td>
    <p>Yürütmeyi tetiklemek için veya bir zamanlama yerine getirmek için kullanılabilecek bir işin parametrelerini tanımı.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>New-AzureSqlJob</p>
    <p>Set-AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>İş yürütme</td>
    <td>
    <p>Bir komut dosyası çalıştırma veya bir DACPAC hataları olan veritabanı bağlantıları için kimlik bilgilerini kullanarak bir hedef uygulama yerine getirmek için gerekli görevlerin kapsayıcı bir yürütme İlkesi anlaşmalara uygun şekilde işlenir.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start-AzureSqlJobExecution</p>
    <p>Stop-AzureSqlJobExecution</p>
    <p>Wait-AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>İş görevi yürütme</td>
    <td>
    <p>Bir işi karşılamak için tek iş birimi.</p>
    <p>İş görevi başarıyla için yükleyememesi durumunda yürütün, ortaya çıkan özel durum iletisi kaydedilir ve yeni eşleşen iş görevi oluşturulur ve belirtilen yürütme İlkesi anlaşmalara uygun şekilde yürütüldü.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start-AzureSqlJobExecution</p>
    <p>Stop-AzureSqlJobExecution</p>
    <p>Wait-AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>İş yürütme İlkesi</td>
    <td>
    <p>Denetimler, yürütme zaman aşımı, yeniden limitleri ve aralıklarla yeniden denemeler arasındaki iş.</p>
    <p>Elastik veritabanı işleri arasında her yeniden deneme aralıkları üstel geri alma ile işi görev hataları temelde sonsuz yeniden denenme neden varsayılan iş yürütme ilkesini içerir.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>New-AzureSqlJobExecutionPolicy</p>
    <p>Set-AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Zamanlama</td>
    <td>
    <p>Belirtimi için yinelenen bir aralık veya tek bir zaman gerçekleşmesi yürütme zamanı tabanlı.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>New-AzureSqlJobSchedule</p>
    <p>Set-AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>İşi tetikler</td>
    <td>
    <p>Bir iş ve zamanlamaya göre tetikleme iş yürütme için bir zamanlama arasında bir eşleme.</p>
    </td>
    <td>
    <p>New-AzureSqlJobTrigger</p>
    <p>Remove-AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Desteklenen esnek veritabanı işleri türlerini gruplama
İş, veritabanlarından oluşan bir grupta Transact-SQL (T-SQL) betikleri veya DACPACs uygulamasının yürütür. Bir işi veritabanlarından oluşan bir grupta yürütülecek gönderildiğinde iş "genişletir" burada her gerçekleştiren tek bir veritabanında istenen yürütme grubunda alt işlere. 

Grupların oluşturabileceğiniz iki tür vardır: 

* [Parça eşlemesi](sql-database-elastic-scale-shard-map-management.md) Grup: bir parça eşlemesi hedeflemek için bir iş gönderildiğinde iş parçalar geçerli kümesini belirlemek için parça eşlemesi sorgular ve daha sonra alt işleri her parça parça eşlemesinde oluşturur.
* Özel Toplama grubu: özel bir veritabanları kümesi tanımlanmış. Bir işi özel bir koleksiyona hedeflediğinde, alt işleri her veritabanı için şu anda özel koleksiyon oluşturur.

## <a name="to-set-the-elastic-database-jobs-connection"></a>Elastik veritabanı için bağlantı işler.
Bir bağlantı işleri için ayarlanması gerekir *denetimi veritabanı* işleri API'leri kullanarak önce. Bu cmdlet'in çalıştırılması, bir kimlik bilgisi kullanıcı adını ve parolayı elastik veritabanı işleri yüklenirken oluşturulan isteyen yukarı açılan pencereye tetikler. Bu konu içinde sağlanan tüm örneklerde, bu ilk adım zaten gerçekleştirildi varsayılır.

Elastik veritabanı işleri bir bağlantı açılamadı:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a>Elastik veritabanı işleri içinde şifrelenmiş kimlik bilgileri
Veritabanı kimlik bilgileri işlere eklenebilir *denetimi veritabanı* şifreli, parola ile. İşleri daha sonraki bir zamanda yürütülecek (iş zamanlamaları kullanarak) etkinleştirmek için kimlik bilgilerini saklamak gereklidir.

Yükleme betiğinin bir parçası oluşturulan bir sertifika aracılığıyla şifreleme çalışır. Yükleme betiğini oluşturur ve sertifika depolanan şifrelenmiş parolaları şifresinin çözülmesi için Azure bulut hizmetine yükler. Azure bulut hizmeti projeleri içinde ortak anahtarı daha sonra depolar *denetimi veritabanı* sertifikanın yerel olarak yüklenmesini gerektirmeden sağlanan parola şifrelemek PowerShell API'si veya Azure portal arabirimi sağlar .

Kimlik bilgisi parolalar şifrelenmiş ve elastik veritabanı işleri nesnelere salt okunur erişimi olan kullanıcıların güvenli. Ancak bir parola ayıklanacak kötü niyetli bir kullanıcı için elastik veritabanı işleri nesneler için okuma-yazma erişimiyle mümkündür. Kimlik bilgileri, iş yürütmeleri arasında yeniden kullanılabilmeleri için tasarlanmıştır. Kimlik bilgilerini hedef veritabanlarına bağlantı kurarken geçirilir. Şu anda her kimlik bilgisi için kullanılan hedef veritabanlarına hiçbir kısıtlama vardır, kötü niyetli bir kullanıcının denetimi altında bir veritabanı için veritabanı hedef kötü niyetli bir kullanıcı ekleyebilirsiniz. Kullanıcı, parola kimlik bilgisi elde etmek için bu veritabanını hedefleyen bir işi daha sonra başlatabilir.

Elastik veritabanı işleri için en iyi güvenlik uygulamaları şunlardır:

* API’lerin kullanımını güvenilir kişilerle sınırlayın.
* Kimlik bilgileri, iş görevi gerçekleştirmek gerekli en az ayrıcalığa sahip olmalıdır.  Daha fazla bilgi bu içinde görülebilir [yetkilendirme ve izinler](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN makalesi.

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a>Veritabanları arasında iş yürütme için şifrelenmiş bir kimlik bilgisi oluşturmak için
Bir yeni şifrelenmiş kimlik bilgisi oluşturmak için [ **Get-Credential cmdlet** ](/powershell/module/microsoft.powershell.security/get-credential) isteyen bir kullanıcı adı ve geçirilebilir parola [ **AzureSqlJobCredential yeni cmdlet** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a>Kimlik bilgilerini güncelleştirmek için
Parolaları değiştirme, kullanın [ **kümesi AzureSqlJobCredential cmdlet'i** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) ayarlayıp **CredentialName** parametresi.

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a>Bir esnek veritabanı parça eşlemesi hedef tanımlamak için
Bir parça kümesindeki tüm veritabanlarında bir iş çalıştırmak için (kullanılarak oluşturulan [elastik veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md)), bir parça eşlemesi veritabanı hedefi olarak kullanın. Bu örnek, elastik veritabanı istemci kitaplığı kullanılarak oluşturulan bir parçalı uygulama gerektirir. Bkz: [esnek veritabanı araçları örnek ile kullanmaya](sql-database-elastic-scale-get-started.md).

Parça eşleme Yöneticisi veritabanını, veritabanı hedef olarak ayarlamanız gerekir ve ardından belirli parça eşlemesi hedef olarak belirtilmelidir.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Veritabanlarında yürütme için bir T-SQL betiği oluşturma
T-SQL betiklerini yürütme için oluştururken, bunları olmasını oluşturmak önerilir [ıdempotent](https://en.wikipedia.org/wiki/Idempotence) ve arızalarına karşı dayanıklı. Elastik veritabanı işleri, yürütme hatası sınıflandırmasını bağımsız olarak bir hata karşılaştığında bir betik yürütme işlemi yeniden deneyecek.

Kullanım [ **AzureSqlJobContent yeni cmdlet** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) oluşturmak ve bir betik için yürütme ve kümesi kaydetmek için **- ContentName** ve **- CommandText** Parametreler.

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Bir dosyadan yeni bir komut dosyası oluşturma
T-SQL betiği dosyası içinde tanımlanmış olması durumunda, bu komut dosyasını içeri aktarmak için kullanın:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a>T-SQL betiği yürütme için veritabanlarında güncelleştirmek için
Bu PowerShell Betiği, var olan bir komut dosyası için T-SQL komut metni güncelleştirir.

Aşağıdaki değişkenleri ayarlamak için istenen komut tanımı yansıtacak şekilde ayarlayın:

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="to-update-the-definition-to-an-existing-script"></a>Var olan bir komut dosyasına tanımını güncelleştirmek için
    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a>Bir parça eşlemesi bir betik yürütmek için bir iş oluşturmak için
Bu PowerShell Betiği, esnek ölçeklendirme parça eşlemesindeki her parça arasında bir betik yürütme işlemi için bir iş başlatır.

İstenen komut dosyası ve hedef yansıtacak şekilde aşağıdaki değişkenleri ayarlayın:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a>Bir işin yürütülmesi için
Bu PowerShell Betiği, var olan bir işi yürütür:

Aşağıdaki değişken yürüttünüz için istenen iş adı yansıtacak şekilde güncelleştirin:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution

## <a name="to-retrieve-the-state-of-a-single-job-execution"></a>Tek iş yürütme durumunu almak için
Kullanım [ **cmdlet Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) ve **JobExecutionId** iş yürütme durumunu görüntülemek için parametre.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Aynı **Get-AzureSqlJobExecution** cmdlet'iyle **ıncludechildren'ın** parametresi, yani her veritabanında her iş yürütme için belirli durumu olan alt iş yürütmeleri durumunu görüntülemek için İş tarafından hedeflenen.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a>Birden çok iş yürütmeleri arasında durumunu görüntülemek için
[ **Cmdlet Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) sağlanan parametreler filtre birden çok iş yürütmeleri görüntülemek için kullanılan birden fazla isteğe bağlı parametrelere sahip. Aşağıdaki Get-AzureSqlJobExecution kullanmak mümkün yollardan bazılarını göstermektedir:

Tüm etkin üst düzey iş yürütmeleri alın:

    Get-AzureSqlJobExecution

Etkin olmayan iş yürütme sayısı dahil olmak üzere tüm üst düzey iş yürütmeleri alın:

    Get-AzureSqlJobExecution -IncludeInactive

Tüm alt iş yürütme sayısı, etkin olmayan iş yürütme sayısı dahil olmak üzere belirtilen iş yürütme kimliği Al:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren

Bir zamanlama kullanılarak oluşturulan tüm iş yürütmeleri almak / etkin olmayan işler de dahil olmak üzere birlikte, iş:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Etkin olmayan işler de dahil olmak üzere bir belirtilen parça eşlemesini hedefleyen tüm işleri Al:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

Etkin olmayan işler de dahil olmak üzere belirtilen özel bir koleksiyonu hedefleyen tüm işleri Al:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

Belirli iş yürütme içinde iş görevi yürütme listesini alın:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

İş görevi yürütme ayrıntılarını alın:

Aşağıdaki PowerShell betiğini yürütme hatalarını hata ayıklama sırasında özellikle yararlı olur. bir iş görevi yürütmede ayrıntılarını görüntülemek için kullanılabilir.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="to-retrieve-failures-within-job-task-executions"></a>Görev yürütme iş içindeki hataların almak için
**JobTaskExecution nesne** yaşam döngüsünü görev ileti özelliği ile birlikte bir özelliği içerir. Bir iş görevi yürütmede başarısız olursa, yaşam döngüsü özelliği ayarlanacak *başarısız* ve ortaya çıkan özel durum iletisi ve kendi yığınını ileti özelliği ayarlanır. Bir işi başarılı olmadıysa için belirli bir işin başarılı olmadı iş görevleri ayrıntılarını görüntülemek önemlidir.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a>Tamamlamak iş yürütme için beklenecek
Aşağıdaki PowerShell Betiği, bir işi görevin tamamlanmasını beklemek için kullanılabilir:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Bir özel yürütme ilkesi oluşturma
Elastik veritabanı işleri destekler işler başlatılırken uygulanabilen özel yürütme ilkeleri oluşturma.

Şu anda yürütme ilkelerini tanımlamak için izin ver:

* Adı: Yürütme İlkesi tanımlayıcısı.
* İşi zaman aşımı: bir iş tarafından elastik veritabanı işleri iptal edilecek önce toplam süresi.
* İlk yeniden deneme aralığı: ilk yeniden denemeden önce beklenecek aralık.
* En fazla yeniden deneme aralığı: kullanılacak uç yeniden deneme aralıkları.
* Yeniden deneme aralığı geri alma katsayısı: Katsayısı sonraki yeniden deneme aralığını hesaplamak için kullanılır.  Aşağıdaki formülü kullanılır: (ilk yeniden deneme aralığı) * Math.pow ((aralık geri alma katsayısı) (yeniden deneme sayısı) - 2). 
* En fazla denemesi: Yeniden deneme sayısı içinde bir işi gerçekleştirmek çalışır.

Varsayılan yürütme ilkesi aşağıdaki değerleri kullanır:

* Adı: Varsayılan yürütme İlkesi
* İşi zaman aşımı: 1 hafta
* İlk yeniden deneme aralığı: 100 milisaniye
* En fazla yeniden deneme aralığı: 30 dakika
* Yeniden deneme aralığı katsayısı: 2
* En fazla denemesi: 2.147.483.647

İstenen yürütme ilkesi oluşturun:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Bir özel yürütme ilkesini güncelleştirme
Güncelleştirme için istenen yürütme ilkesini güncelleştirin:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy

## <a name="cancel-a-job"></a>Bir işi iptal et
Elastik veritabanı işleri işlerinin iptal isteklerini destekler.  Elastik veritabanı işleri şu anda yürütülmekte olan iş için bir iptal isteğine algılarsa, işi durdurmak dener.

Elastik veritabanı işleri iptal gerçekleştirebilirsiniz iki farklı yolu vardır:

1. Yürütülmekte olan görevleri iptal: şu anda yürütülen görev görünümü içinde bir görev şu anda çalışırken iptal algılanırsa, bir iptal denenecek.  Örneğin: iptal çalışırken şu anda gerçekleştirilmekte olan uzun süre çalışan bir sorgu varsa, sorgusunu iptal etme girişimi olur.
2. Görev yeniden deneme iptal ediliyor: yürütme için bir görev başlatılmadan önce iptal denetimi iş parçacığı tarafından algılanırsa, denetimi iş parçacığı görevi başlatmaktan kaçınmak ve istek iptal edildi olarak bildirin.

Üst iş için bir işi iptal istenirse, üst işin ve tüm alt işlerini iptal isteğini getirilmez.

İptal isteği göndermek için kullanabilir [ **Stop-AzureSqlJobExecution cmdlet'i** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) ayarlayıp **JobExecutionId** parametresi.

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a>Bir işi silin ve geçmiş zaman uyumsuz olarak iş için
Elastik veritabanı işleri, zaman uyumsuz işler silinmesini destekler. Bir işi silinmek üzere işaretlenmiş ve tüm iş yürütmeleri için işini tamamladıktan sonra sistem iş ve buna ait iş geçmişi silinir. Sistem etkin iş yürütmeleri otomatik olarak iptal.  

Çağırma [ **Stop-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) etkin iş yürütme iptal etmek için.

Proje silme işlemi tetiklemek için kullanın [ **Remove-AzureSqlJob cmdlet'i** ](/powershell/module/elasticdatabasejobs/remove-azuresqljob) ayarlayıp **JobName** parametresi.

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName

## <a name="to-create-a-custom-database-target"></a>Özel veritabanını hedef oluşturmak için
Özel veritabanını hedefler doğrudan yürütme için veya içinde bir özel bir veritabanı grubu eklemek için tanımlayabilirsiniz. Örneğin, çünkü **elastik havuzlar** olan henüz PowerShell API'lerini kullanarak doğrudan destek, özel veritabanı hedef ve havuzdaki tüm veritabanları kapsayan, özel bir veritabanı koleksiyonu hedef oluşturabilirsiniz.

İstenen veritabanı bilgileri yansıtacak şekilde aşağıdaki değişkenleri ayarlayın:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a>Özel bir veritabanı koleksiyonu hedefi oluşturmak için
Kullanım [ **yeni AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet'ini birden çok tanımlanmış veritabanı hedeflerden yürütme etkinleştirmek için özel bir veritabanı koleksiyonu hedef tanımlayın. Bir veritabanı grubu oluşturduktan sonra veritabanları özel koleksiyon hedefle ilişkili olabilir.

Özel bir koleksiyona istenen hedef yapılandırmayı yansıtacak şekilde aşağıdaki değişkenleri ayarlayın:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a>Özel bir veritabanı koleksiyonu hedef veritabanı eklemek için
Belirli özel koleksiyon için bir veritabanı ekleme [ **Ekle AzureSqlJobChildTarget** ](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) cmdlet'i.

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Özel bir veritabanı koleksiyonu hedef içinde veritabanlarını gözden geçirin
Kullanım [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) özel bir veritabanı koleksiyonu hedef içinde alt veritabanlarını almak için cmdlet'i. 

    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Özel bir veritabanı koleksiyonu hedef arasında bir betik yürütmek için bir iş oluşturma
Kullanım [ **yeni AzureSqlJob** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) özel bir veritabanı koleksiyonu hedef tarafından tanımlanan veritabanlarından oluşan bir grupta karşı bir iş oluşturmak için cmdlet'i. Elastik veritabanı işleri her bir veritabanına karşılık gelen özel bir veritabanı koleksiyonu hedefle ilişkili birden çok alt işlere işi'ni genişletin ve komut dosyası her bir veritabanına karşı yürütülür emin olun. Yeniden betikleri deneme dayanıklı olacak şekilde etkili olması önemlidir.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Veritabanları arasında veri toplama
Bir iş, veritabanlarından oluşan bir grupta bir sorgu yürütmek ve sonuçları belirli bir tabloya göndermek için kullanabilirsiniz. Tablo, her veritabanı sorgunun sonuçlarını görmek için çözümledikten sonra sorgulanabilir. Bu, çok sayıda veritabanı arasında bir sorgu yürütmek için zaman uyumsuz bir yöntem sağlar. Başarısız oturum açma girişimlerinin otomatik olarak yeniden deneme işlenir.

Henüz yoksa, belirtilen hedef tablo otomatik olarak oluşturulur. Yeni Tablo döndürülen sonuç kümesi şeması eşleşir. Bir komut dosyası birden çok sonuç kümesi döndürürse, elastik veritabanı işleri, yalnızca ilk hedef tabloya gönderir.

Aşağıdaki PowerShell betiğini bir betik yürütür ve belirtilen bir tabloya, sonuçları toplar. Bu betik, tek bir sonuç kümesi çıkışı bir T-SQL betiği oluşturuldu ve özel bir veritabanı koleksiyonu hedef oluşturulduğunu varsayar.

Bu betiği kullanan [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet'i. Betik, kimlik bilgilerini ve yürütme hedefi parametrelerini ayarla:

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

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a>Oluşturma ve veri koleksiyonu senaryoları için bir proje başlatmak için
Bu betiği kullanan [ **başlangıç AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) cmdlet'i.

    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="to-schedule-a-job-execution-trigger"></a>İş yürütme tetikleyici zamanlamak için
Aşağıdaki PowerShell Betiği, yinelenen bir zamanlama oluşturmak için kullanılabilir. Bu betik, bir dakikalık bir aralığı kullanır ancak [ **yeni AzureSqlJobSchedule** ](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) - Dayınterval, - HourInterval, - MonthInterval ve - WeekInterval parametreleri de destekler. Yalnızca bir kere yürütülen zamanlamaları oluşturulabilir geçirilerek - OneTime.

Yeni bir zamanlama oluşturur:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a>Bir zaman planlamada yürütülen bir iş tetiklemek için
Bir işi tetikleyici, bir zaman zamanlamaya göre çalıştırılan bir iş için tanımlanabilir. Aşağıdaki PowerShell Betiği, bir iş tetikleyicisi oluşturmak için kullanılabilir.

Kullanım [yeni AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) ve zamanlama ve istenen iş karşılık gelecek şekilde aşağıdaki değişkenleri ayarlayın:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    -JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>İş, zamanlamaya göre yürütülmesini durdurmak için zamanlanmış bir ilişkilendirmesini kaldırmak için
Yinelenen iş yürütme işi tetikleyici aracılığıyla kesmek için iş tetikleyici kaldırılabilir. Bir zamanlamaya göre çalıştırılmasını bir işi durdurmak için bir iş tetikleyiciyi kaldırmak [ **Remove-AzureSqlJobTrigger cmdlet'i**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a>Bir zaman zamanlamaya bağlı iş Tetikleyicileri alma
Aşağıdaki PowerShell betiğini elde edip belirli bir zamanlama kayıtlı iş Tetikleyiciler görüntüleyecek şekilde kullanılabilir.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a>İş Tetikleyicileri almak için bir projeye bağlı.
Kullanım [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) alıp içeren kayıtlı bir iş zamanlamaları görüntüler.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a>Yürütme için veri katmanı uygulaması (DACPAC) veritabanlarında oluşturmak için
DACPAC oluşturmak için bkz [veri katmanı uygulamaları](https://msdn.microsoft.com/library/ee210546.aspx). DACPAC dağıtmak için [AzureSqlJobContent yeni cmdlet](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent). DACPAC hizmete erişilebilir olmalıdır. Oluşturulan DACPAC Azure Depolama'ya yükler ve oluşturmak için önerilen bir [paylaşılan erişim imzası](../storage/common/storage-dotnet-shared-access-signature-part-1.md) DACPAC için.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a>Yürütme için veri katmanı uygulaması (DACPAC) veritabanlarında güncelleştirmek için
Elastik veritabanı işleri içinde kayıtlı mevcut DACPACs için yeni bir URI'leri işaret edecek şekilde güncelleştirilebilir. Kullanım [ **kümesi AzureSqlJobContentDefinition cmdlet'i** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) DACPAC kayıtlı var olan bir DACPAC URI güncelleştirmek için:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a>Veri katmanı uygulaması (DACPAC) veritabanlarında uygulamak için bir iş oluşturmak için
Elastik veritabanı işleri içinde bir DACPAC oluşturulduktan sonra bir işi DACPAC veritabanlarından oluşan bir grupta uygulamak için oluşturulabilir. Aşağıdaki PowerShell Betiği, bir DACPAC iş veritabanları arasında özel koleksiyonunu oluşturmak için kullanılabilir:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
