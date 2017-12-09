---
title: "PowerShell kullanarak esnek işleri oluşturmak ve yönetmek | Microsoft Docs"
description: "Azure SQL veritabanı havuzlarını yönetmek için kullanılan PowerShell"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 737d8d13-5632-4e18-9cb0-4d3b8a19e495
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: f9bdc28349c540ee68b421b7643e4bed099c9fdd
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="create-and-manage-sql-database-elastic-jobs-using-powershell-preview"></a>PowerShell (Önizleme) kullanarak SQL Database esnek işleri oluşturmak ve yönetmek

PowerShell API'ler **esnek veritabanı işleri** (Önizleme) içinde göre betikleri yürütülecek grubu tanımlamanıza olanak sağlar. Bu makalede oluşturmak ve yönetmek nasıl gösterilmektedir **esnek veritabanı işleri** PowerShell cmdlet'lerini kullanarak. Bkz: [esnek iş genel bakış](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Ön koşullar
* Azure aboneliği. Ücretsiz deneme için bkz: [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Esnek veritabanı araçları ile oluşturulmuş bir veritabanları kümesi. Bkz: [esnek veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md).
* Azure PowerShell. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/overview).
* **Esnek veritabanı iş** PowerShell paketi: bkz [esnek veritabanı yükleme işleri](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Azure aboneliğinizi seçin
Aboneliği seçmek için abonelik kimliği gerekir (**- Subscriptionıd**) veya abonelik adı (**varlığıyla - SubscriptionName**). Birden çok aboneliğiniz varsa, çalıştırabilirsiniz **Get-AzureRmSubscription** cmdlet'i ve kopyalama sonuç istenen abonelik bilgilerini ayarlayın. Abonelik bilgilerinizi sahip olduğunda, işleri oluşturmaya ve yönetmeye için hedef özelliği varsayılan olarak bu aboneliği ayarlamak için aşağıdaki komutunu çalıştırın:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

[PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) geliştirmek ve esnek veritabanı işleri karşı PowerShell betikleri çalıştırmak kullanım için tavsiye edilir.

## <a name="elastic-database-jobs-objects"></a>Esnek veritabanı işleri nesneleri
Aşağıdaki tabloda tüm nesne türlerini listeler **esnek veritabanı işleri** açıklaması ve ilgili PowerShell API'lerinden birlikte.

<table style="width:100%">
  <tr>
    <th>Nesne türü</th>
    <th>Açıklama</th>
    <th>İlgili PowerShell API'leri</th>
  </tr>
  <tr>
    <td>Kimlik Bilgisi</td>
    <td>Kullanıcı adı ve parola betiklerinin yürütülmesi veya DACPACs uygulamasının veritabanlarına bağlanırken kullanılacak. <p>Parola gönderme ve esnek veritabanı iş veritabanında saklamak önce şifrelenir.  Esnek veritabanı iş hizmeti aracılığıyla oluşturulan ve yükleme komut dosyasını karşıya kimlik bilgisi tarafından şifresi parola.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>AzureSqlJobCredential yeni</p><p>Set-AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Betik</td>
    <td>Veritabanları arasında yürütme için kullanılacak transact-SQL betiği.  Komut dosyası hizmeti hataları bağlı betik yürütme işlemi yeniden deneyecek beri ıdempotent olmasını yazılan.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>AzureSqlJobContent yeni</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Veri katmanı uygulaması </a> veritabanları arasında uygulanacak paket.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>AzureSqlJobContent yeni</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Veritabanı hedef</td>
    <td>Bir Azure SQL veritabanına işaret eden veritabanı ve sunucu adı.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>AzureSqlJobTarget yeni</p>
    </td>
  </tr>
  <tr>
    <td>Parça eşleme hedef</td>
    <td>Bir veritabanı hedef ve bir esnek veritabanı parça eşlemesi içinde depolanan bilgileri belirlemek için kullanılacak bir kimlik bilgisi birleşimi.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>AzureSqlJobTarget yeni</p>
    <p>Set-AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Özel Toplama hedef</td>
    <td>Tanımlanan grubu veritabanlarının topluca yürütme için kullanın.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>AzureSqlJobTarget yeni</p>
    </td>
  </tr>
<tr>
    <td>Özel Toplama alt hedef</td>
    <td>Özel bir koleksiyondan başvurulan veritabanı hedefi.</td>
    <td>
    <p>Ekleme AzureSqlJobChildTarget</p>
    <p>Remove-AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>İş</td>
    <td>
    <p>Yürütme tetiklemek için veya bir zamanlama yerine getirmek için kullanılabilir bir işin parametrelerini tanımı.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>AzureSqlJob yeni</p>
    <p>Set-AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>İş yürütme</td>
    <td>
    <p>Uygun bir yürütme ilkesi için bir komut dosyası yürütme ya da bir DACPAC hataları olan veritabanı bağlantıları için kimlik bilgilerini kullanarak bir hedef uygulama karşılamak için gerekli görevleri kapsayıcının işlenir.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start-AzureSqlJobExecution</p>
    <p>Stop-AzureSqlJobExecution</p>
    <p>Bekleme AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>İş görevi yürütmede</td>
    <td>
    <p>Bir işi karşılamak üzere tek iş birimi.</p>
    <p>İş görevi başarıyla için mümkün değilse, yürütme, elde edilen özel durum iletisi günlüğe kaydedilir ve yeni eşleşen iş görevi oluşturulur ve belirtilen yürütme İlkesi uygun yürütülür.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start-AzureSqlJobExecution</p>
    <p>Stop-AzureSqlJobExecution</p>
    <p>Bekleme AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>İş yürütme İlkesi</td>
    <td>
    <p>Yürütme zaman aşımı, yeniden deneme sınırları ve aralıklarla yeniden denemeler arasında denetimleri işi.</p>
    <p>Esnek veritabanı iş denemeler arasındaki aralığı üstel geri alma ile temelde sonsuz yeniden deneme iş görevi başarısızlık neden varsayılan iş yürütme İlkesi içerir.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>AzureSqlJobExecutionPolicy yeni</p>
    <p>Set-AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Zamanlama</td>
    <td>
    <p>Zaman belirtimi yürütme yeniden aralığı veya tek bir seferde gerçekleşmesi temel.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>AzureSqlJobSchedule yeni</p>
    <p>Set-AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>İş Tetikleyicileri</td>
    <td>
    <p>Bir iş ve zamanlamaya göre tetikleyici iş yürütme için bir zamanlama arasında bir eşleme.</p>
    </td>
    <td>
    <p>AzureSqlJobTrigger yeni</p>
    <p>Remove-AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Desteklenen esnek veritabanı işleri türleri Grup
İş Transact-SQL (T-SQL) komut dosyaları veya DACPACs uygulama grubu yürütür. Bir iş grubu yürütülecek gönderildiğinde iş "genişletir" burada her gerçekleştirir tek bir veritabanına karşı istenen yürütme grubunda alt işlere. 

İki tür oluşturabileceğiniz Grup vardır: 

* [Parça eşleme](sql-database-elastic-scale-shard-map-management.md) Grup: bir iş, bir parça eşleme hedeflemek için gönderildiğinde, iş parça parça geçerli kendi belirlemek eşlemek sorgular ve ardından alt işleri her parça parça eşlemesinde oluşturur.
* Özel Toplama grubu: özel bir veritabanları kümesi tanımlı. Bir işi özel bir koleksiyona hedeflediğinde, bu alt işleri her veritabanı için şu anda özel koleksiyonu oluşturur.

## <a name="to-set-the-elastic-database-jobs-connection"></a>Esnek veritabanı ayarlamak için bağlantı işleri
İşlerini ayarlamak bir bağlantı gerekiyor *denetim veritabanı* işleri API'leri kullanılarak önce. Bu cmdlet çalıştıran kullanıcı adı ve parola esnek veritabanı işleri yüklenirken oluşturulan isteyen yukarı pop için bir kimlik bilgisi pencere tetikler. Bu konu içinde sağlanan tüm örnekleri bu ilk adımı zaten gerçekleştirilen olduğunu varsayalım.

Esnek veritabanı işleri bağlantı açın:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a>Esnek veritabanı işleri içinde şifrelenmiş kimlik bilgileri
Veritabanı kimlik bilgileri işlere eklenebilir *denetim veritabanı* şifreli, parola ile. İşlerini daha sonraki bir zamanda yürütülecek (iş zamanlamalarını kullanarak) etkinleştirmek için kimlik bilgilerini depolamak gereklidir.

Şifreleme yükleme komut dosyasının bir parçası oluşturulan bir sertifika ile çalışır. Yükleme komut dosyası oluşturur ve sertifika depolanan şifrelenmiş parolalar verilerin şifresini çözmek için Azure bulut hizmeti içine yükler. Azure bulut hizmeti daha sonra ortak anahtarı işleri içinde depolar *denetim veritabanı* sertifika yerel olarak yüklenmesini gerektirmeden sağlanan parola şifrelemek PowerShell API'si veya Klasik Azure portalı arabirimi sağlar.

Şifrelenmiş ve salt okunur erişimi olan kullanıcılar esnek veritabanı işleri nesneler için güvenli kimlik bilgileri parolalar. Ancak bir parola ayıklamak kötü amaçlı bir kullanıcı için esnek veritabanı iş nesnelere okuma-yazma erişimi mümkündür. Kimlik bilgileri iş yürütmeleri arasında yeniden tasarlanmıştır. Kimlik bilgileri hedef veritabanlarına bağlantı kurulurken aktarılır. Şu anda her kimlik bilgisi için kullanılan hedef veritabanı üzerinde bir kısıtlama yoktur, kötü amaçlı kullanıcı kötü niyetli bir kullanıcının denetimindeki bir veritabanı için veritabanı hedef ekleyebilirsiniz. Kullanıcı kimlik bilgisi parola kazanmak için bu veritabanını hedefleyen bir işi daha sonra başlayamadı.

Esnek veritabanı işleri için en iyi yöntemler şunlardır:

* Güvenilen Kişiler için API'lerin kullanımını sınırlayın.
* Kimlik bilgileri, iş görevi gerçekleştirmek için gerekli en düşük ayrıcalık olmalıdır.  Daha fazla bilgi bu içinde görülebilir [yetkilendirme ve izinleri](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN makalesi.

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a>Veritabanları arasında iş yürütme için şifrelenmiş bir kimlik bilgisi oluşturmak için
Bir yeni şifreli kimlik bilgisi oluşturmak için [ **Get-Credential cmdlet** ](https://technet.microsoft.com/library/hh849815.aspx) ister bir kullanıcı adı ve parola için geçirilen [ **yeni AzureSqlJobCredential cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a>Kimlik bilgilerini güncelleştirmek için
Parolaları değiştirdiğinizde [ **Set-AzureSqlJobCredential cmdlet'i** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) ve **CredentialName** parametresi.

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a>Bir esnek veritabanı parça eşleme hedefi tanımlamak için
Bir işin tüm veritabanlarında parça kümesinde yürütmek için (kullanılarak oluşturulan [esnek veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md)), bir parça eşleme veritabanı hedefi olarak kullanın. Bu örnek, esnek veritabanı istemci kitaplığı kullanılarak oluşturulmuş bir parçalı uygulaması gerektirir. Bkz: [esnek veritabanı araçlarını örneği ile çalışmaya başlama](sql-database-elastic-scale-get-started.md).

Parça eşleme Yöneticisi veritabanı bir veritabanı hedefi olarak ayarlanmalı ve belirli parça eşleme hedefi olarak belirtilen.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Veritabanları arasında yürütme için T-SQL komut dosyası oluşturma
Yürütme için T-SQL komut dosyalarını oluştururken, bunları olarak oluşturmak için önerilir [ıdempotent](https://en.wikipedia.org/wiki/Idempotence) ve hatalarına karşı dayanıklı. Esnek veritabanı iş yürütme hatası sınıflandırmasını bağımsız olarak bir hata karşılaştığında bir betik yürütme işlemi yeniden deneyecek.

Kullanım [ **yeni AzureSqlJobContent cmdlet** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) oluşturma ve yürütme ve kümesi için komut dosyasını kaydetmek için **- ContentName** ve **- CommandText** parametreleri.

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
T-SQL komut dosyası içinde tanımlanmış olması durumunda, bu komut dosyasını içeri aktarmak için kullanın:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a>Bir T-SQL komut dosyası yürütme için veritabanları arasında güncelleştirmek için
Bu PowerShell komut dosyasını varolan bir komut dosyasını T-SQL komut metni güncelleştirir.

Ayarlanacak istenen komut tanımı yansıtmak için aşağıdaki değişkenleri ayarlayın:

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

### <a name="to-update-the-definition-to-an-existing-script"></a>Varolan bir komut dosyasını tanımı güncelleştirmek için
    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a>Parça eşleme arasında bir betik yürütmek için bir iş oluşturmak için
Bu PowerShell Betiği esnek ölçek parça eşlemesindeki her parça genelinde bir betik yürütme işlemi için bir iş başlatır.

İstenen komut dosyası ve hedef yansıtmak için aşağıdaki değişkenleri ayarlayın:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a>Bir işi çalıştırmak için
Bu PowerShell komut dosyası var olan bir işi çalıştırır:

Aşağıdaki değişkeni istenen iş yürütülmesini yansıtacak şekilde güncelleştirin:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution

## <a name="to-retrieve-the-state-of-a-single-job-execution"></a>Tek iş yürütme durumunu almak için
Kullanım [ **Get-AzureSqlJobExecution cmdlet'i** ](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) ve **JobExecutionId** iş yürütme durumunu görüntülemek için parametre.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Aynı **Get-AzureSqlJobExecution** cmdlet'iyle **ıncludechildren'ın** öğesine her iş yürütme iş tarafından hedeflenen her bir veritabanına karşı özel durumu olan alt iş yürütmeleri durumunu görüntülemek için parametre.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a>Birden çok iş yürütmeleri arasında durumunu görüntülemek için
[ **Get-AzureSqlJobExecution cmdlet'i** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) aracılığıyla sağlanan parametreleri filtre birden çok iş yürütmeleri görüntülemek için kullanılan birden fazla isteğe bağlı parametre yok. Get-AzureSqlJobExecution kullanmak için olası yollardan bazılarını şunlar gösterilmektedir:

Tüm etkin üst düzey iş yürütmeleri Al:

    Get-AzureSqlJobExecution

Etkin olmayan iş yürütmeleri dahil olmak üzere tüm üst düzey iş yürütmeleri Al:

    Get-AzureSqlJobExecution -IncludeInactive

Etkin olmayan iş yürütmeleri dahil olmak üzere sağlanan iş yürütme kimliği, tüm alt iş yürütmeleri Al:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren

Bir zamanlama kullanılarak oluşturulan tüm iş yürütmeleri almak / birleşimi etkin olmayan işler de dahil olmak üzere, iş:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Etkin olmayan işler de dahil olmak üzere bir belirtilen parça eşleme hedefleme tüm işleri Al:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

Etkin olmayan işler de dahil olmak üzere belirtilen özel bir koleksiyonu hedefleyen tüm işleri Al:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

Belirli iş yürütme içinde iş görev yürütmeleri listesini al:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

İş görev yürütme ayrıntıları alın:

Aşağıdaki PowerShell betiğini yürütme hatalarını ayıklama özellikle yararlıdır iş görevi yürütmede ayrıntılarını görüntülemek için kullanılabilir.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="to-retrieve-failures-within-job-task-executions"></a>İş görevi yürütmeleri içinde hataları almak için
**JobTaskExecution nesne** kullanım ömrü boyunca bir ileti özelliği birlikte görev için bir özellik içerir. Bir iş görevi yürütmede başarısız olduysa, yaşam döngüsü özelliği olarak ayarlanacaktır *başarısız* ve sonuçta elde edilen özel durum iletisi ve kendi yığını ileti özelliği ayarlanır. Bir işi başarısız oldu, belirli bir iş için başarılı olmadı iş görevleri ayrıntılarını görüntülemek önemlidir.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a>Bir iş yürütme tamamlamak beklenecek
Aşağıdaki PowerShell betiğini bir iş görevinin tamamlanmasını beklemek için kullanılabilir:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Özel yürütme ilkesi oluşturma
Esnek veritabanı iş destekler işleri başlatırken uygulanan özel yürütme ilkelerini oluşturma.

Yürütme ilkelerini tanımlamak için şu anda izin ver:

* Adı: Yürütme İlkesi tanımlayıcısı.
* İş zaman aşımı: bir iş tarafından esnek veritabanı işi iptal edilecek önce toplam süre.
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

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Özel yürütme ilkesini güncelleştirin
Güncelleştirmek için istenen yürütme İlkesi güncelleştirmesi:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy

## <a name="cancel-a-job"></a>Bir işi iptal etme
Esnek veritabanı iş işlerin iptal isteklerini destekler.  Esnek veritabanı iş şu anda yürütülmekte olan bir işi iptal isteği algılarsa, işi durdurmayı deneyecek.

Esnek veritabanı iş iptal gerçekleştirebilirsiniz iki farklı yolu vardır:

1. Şu anda yürütülmekte olan görevleri iptal: bir görevi şu anda çalışırken iptal algılanırsa, görevi şu anda yürütülen en boy içinde iptal denenir.  Örneğin: iptal çalışırken şu anda gerçekleştirilen uzun süre çalışan bir sorgusu ise, sorguyu iptal etme girişimi olacaktır.
2. Görev yeniden deneme iptal etme: iptal denetimi iş parçacığı tarafından algılanırsa, bir görev için yürütme başlatılmadan önce denetimi iş parçacığı görev başlatma önlemek ve istek iptal edildi olarak bildirin.

İş iptali için üst iş istediyseniz üst iş ve tüm alt işlerini iptal isteğini uyulacaktır.

İptal isteği göndermek için kullanmak [ **Stop-AzureSqlJobExecution cmdlet'i** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) ve **JobExecutionId** parametresi.

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a>Bir işi silin ve geçmiş zaman uyumsuz olarak iş için
Esnek veritabanı iş işleri zaman uyumsuz silinmesini destekler. Bir işi silinmek üzere işaretlenmiş ve tüm iş yürütmeleri işi tamamlandıktan sonra sistem iş ve tüm iş geçmişi silinir. Sistem etkin iş yürütmeleri otomatik olarak iptal etmek değil.  

Çağırma [ **Stop-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) etkin iş yürütmeleri iptal etmek için.

İş silme tetiklemek için kullanabileceğiniz [ **Kaldır AzureSqlJob cmdlet** ](/powershell/module/elasticdatabasejobs/remove-azuresqljob) ve ayarlayın **JobName** parametresi.

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName

## <a name="to-create-a-custom-database-target"></a>Özel veritabanını hedef oluşturmak için
Özel veritabanı hedefleri doğrudan yürütme veya eklenmek üzere bir özel veritabanı grubu içinde tanımlayabilirsiniz. Örneğin, çünkü **esnek havuzlar** olan henüz PowerShell API'lerini kullanarak doğrudan desteklenen, bir özel veritabanı hedef ve havuzdaki tüm veritabanları kapsayan özel veritabanı koleksiyon hedef oluşturabilirsiniz.

İstenen veritabanı bilgilerini yansıtmak için aşağıdaki değişkenleri ayarlayın:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a>Özel veritabanı koleksiyon hedef oluşturmak için
Kullanım [ **yeni AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) arasında birden çok tanımlanmış veritabanı hedefleri yürütme etkinleştirmek için bir özel veritabanı koleksiyon hedefi tanımlamak için cmdlet. Bir veritabanı grubu oluşturduktan sonra veritabanları özel koleksiyon hedefle ilişkili olabilir.

İstenen özel koleksiyon hedef yapılandırmayı yansıtacak şekilde aşağıdaki değişkenleri ayarlayın:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a>Özel veritabanını koleksiyonu hedefe veritabanı eklemek için
Belirli özel toplama kullanımı için bir veritabanı eklemek için [ **Ekle AzureSqlJobChildTarget** ](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) cmdlet'i.

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Özel veritabanı koleksiyon hedef içinde veritabanları gözden geçirin
Kullanım [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) özel veritabanı koleksiyon hedef içinde alt veritabanları almak üzere. 

    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Özel veritabanı koleksiyon hedef arasında bir betik yürütmek için bir proje oluşturun
Kullanım [ **yeni AzureSqlJob** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) özel veritabanı koleksiyon hedef tarafından tanımlanan veritabanlarının bir gruba göre bir iş oluşturmak için cmdlet'i. Esnek veritabanı iş her bir veritabanına karşılık gelen özel veritabanı koleksiyon hedefle ilişkili birden çok alt işlere işi'ni genişletin ve komut dosyası her veritabanına karşı yürütülür emin olun. Yeniden, komut dosyaları, deneme dayanıklı olmasını ıdempotent olduğunu önemlidir.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Veritabanları arasında veri toplama
Bir iş grubu bir sorgu çalıştırmak ve sonuçları belirli bir tabloya göndermek için kullanabilirsiniz. Tablonun her veritabanından sorgunun sonuçlarını görmek için Olgu sonra sorgulanabilir. Bu, birçok veritabanı arasında bir sorguyu yürütmek için zaman uyumsuz bir yöntem sağlar. Başarısız denemeleri otomatik yeniden denemeler işlenir.

Henüz yoksa, belirtilen hedef tablonun otomatik olarak oluşturulur. Yeni Tablo döndürülen sonuç kümesi şeması eşleşir. Bir komut dosyası birden çok sonuç kümesi döndürürse, esnek veritabanı işlerini ilk hedef tablonun yalnızca gönderir.

Aşağıdaki PowerShell betiğini bir komut dosyası çalıştırır ve sonuçları belirli bir tabloya toplar. Bu komut dosyasını bir T-SQL betiği, tek bir sonuç kümesi çıkarır oluşturuldu ve bir özel veritabanı koleksiyon hedef oluşturulduğunu varsayar.

Bu komut dosyası kullanan [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet'i. Komut dosyası, kimlik bilgileri ve yürütme hedef parametrelerini ayarlayın:

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

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a>Oluşturma ve veri toplama senaryoları için bir işi başlatmak için
Bu komut dosyası kullanan [ **başlangıç AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) cmdlet'i.

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
Aşağıdaki PowerShell betiğini yinelenen bir zamanlama oluşturmak için kullanılabilir. Bu komut dosyasını bir dakika aralığı kullanır ancak [ **yeni AzureSqlJobSchedule** ](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) - DayInterval, - HourInterval, - MonthInterval ve - WeekInterval parametreleri de destekler. Yalnızca bir kez yürütme zamanlamaları oluşturulabilir göre geçirme - kez.

Yeni bir zamanlama oluşturun:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a>Bir zaman zamanlamaya göre çalıştırılan bir iş tetiklemek için
Bir iş tetikleyici bir saat zamanlamaya göre çalıştırılan bir iş için tanımlanabilir. Aşağıdaki PowerShell betiğini bir işi tetikleyici oluşturmak için kullanılabilir.

Kullanım [yeni AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) ve istenen iş ve zamanlama karşılık gelen için aşağıdaki değişkenleri ayarlayın:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    -JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Zamanlamaya göre yürütülmesini işini durdurmak için zamanlanmış bir ilişkilendirmesini kaldırmak için
Bir iş tetikleyici aracılığıyla yeniden iş yürütme kesmek için iş tetikleyici kaldırılabilir. Bir zamanlamaya göre çalıştırılmasını bir işi durdurmak için bir iş tetikleyiciyi kaldırmak [ **Kaldır AzureSqlJobTrigger cmdlet**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a>Bir zaman çizelgesi bağlı iş Tetikleyicileri alma
Aşağıdaki PowerShell betiğini elde ve belirli bir zamanlama kayıtlı iş Tetikleyicileri görüntülemek için kullanılabilir.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a>İş Tetikleyicileri almak için bir projeye bağlı
Kullanım [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) elde edilir ve kayıtlı bir işi içeren zamanlamaları görüntüler.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a>Veritabanları arasında yürütme için veri katmanı uygulaması (DACPAC) oluşturmak için
Bir DACPAC oluşturmak için bkz: [veri katmanı uygulamaları](https://msdn.microsoft.com/library/ee210546.aspx). Bir DACPAC dağıtmak için kullandığınız [yeni AzureSqlJobContent cmdlet](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent). DACPAC hizmete erişilebilir olması gerekir. Oluşturulan DACPAC Azure Storage'a yükler ve oluşturmak için önerilen bir [paylaşılan erişim imzası](../storage/common/storage-dotnet-shared-access-signature-part-1.md) DACPAC için.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a>Yürütme için veri katmanı uygulaması (DACPAC) veritabanları arasında güncelleştirmek için
Esnek veritabanı iş içinde kayıtlı varolan DACPACs yeni URI'ler işaret edecek şekilde güncelleştirilebilir. Kullanım [ **Set-AzureSqlJobContentDefinition cmdlet'i** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) DACPAC kayıtlı varolan üzerinde DACPAC URI güncelleştirmek için:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a>Veri katmanı uygulaması (DACPAC) veritabanları arasında uygulamak için bir iş oluşturmak için
Esnek veritabanı iş bir DACPAC oluşturulduktan sonra bir iş grubu DACPAC uygulamak için oluşturulabilir. Aşağıdaki PowerShell betiğini bir DACPAC işi veritabanları arasında özel bir koleksiyon oluşturmak için kullanılabilir:

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
