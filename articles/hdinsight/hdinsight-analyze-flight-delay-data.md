---
title: HDInsight - Azure, Apache Hadoop ile uçuş gecikme verilerini çözümleme
description: Bir Windows PowerShell Betiği bir HDInsight kümesi oluşturma, bir Hive işi çalıştırma, Sqoop işi çalıştırın ve kümeyi silmek için kullanmayı öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 311e2ee65b2c24eb1c288a2161bf371732aea452
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55817674"
---
# <a name="analyze-flight-delay-data-by-using-apache-hive-in-hdinsight"></a>Apache Hive, HDInsight'ı kullanarak uçuş gecikme verilerini çözümleme
[Apache Hive](https://hive.apache.org/) çalıştırmanın sağlar [Apache Hadoop MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) işleri SQL benzeri bir betik dilini aracılığıyla adlı *[HiveQL] [ hadoop-hiveql]*, hangi uygulanabilir özetleme, sorgulamaya ve analiz etmeye büyük hacimli verileri doğru.

> [!IMPORTANT]  
> Bu belgedeki adımlarda Windows tabanlı HDInsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). Linux tabanlı bir küme ile çalışmanıza adımlar için bkz: [HDInsight (Linux), Apache Hive'ı kullanarak uçuş gecikme verilerini çözümleme](hdinsight-analyze-flight-delay-data-linux.md).

Azure HDInsight'ın en önemli avantajlarından biri veri depolama ve işlem ayrılmasıdır. HDInsight, veri depolama için Azure Blob Depolama kullanır. Tipik bir iş üç parça içerir:

1. **Verileri Azure Blob Depolama alanında Store.**  Örneğin, verileri, sensör verilerini, web günlükleri hava durumu ve bu durumda, uçuş gecikme verilerini Azure Blob depolama alanına kaydedilir.
2. **İşleri çalıştırın.** Veri işlemeye zaman olduğunda, bir HDInsight kümesi oluşturma, işlerini çalıştırın ve kümeyi silmek için bir Windows PowerShell Betiği (veya bir istemci uygulaması) çalıştırın. İşleri, çıktı verilerini Azure Blob depolama alanına kaydedin. Çıkış veri kümesi bile silindikten sonra korunur. Bu şekilde, yalnızca neler, kullanmışsa için ödeme yaparsınız.
3. **Azure Blob depolama alanından çıkış almak**, bu öğreticide, verileri veya bir Azure SQL veritabanı için dışarı aktarma.

Aşağıdaki diyagram, senaryo ve Bu öğreticide yapısını gösterir:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

Diyagramdaki sayılar için bölüm başlıkları karşılık geldiğini unutmayın. **M** için ana işlem anlamına gelir. **A** ek içeriği için anlamına gelir.

Öğreticinin ana bölümü, bir Windows PowerShell Betiği aşağıdaki görevleri gerçekleştirmek için nasıl kullanılacağını gösterir:

* Bir HDInsight kümesi oluşturun.
* Havaalanları ortalama gecikmelerden hesaplamak için küme üzerinde bir Hive işi çalıştırın. Uçuş gecikme verilerini, bir Azure Blob Depolama hesabında depolanır.
* Hive iş çıktısı, bir Azure SQL veritabanına dışarı aktarmak için Sqoop işi çalıştırın.
* HDInsight küme silin.

Çok uçuş gecikme verilerini karşıya yükleme, oluşturma/Hive sorgu dizesi karşıya yükleme ve Azure SQL veritabanı Sqoop işine hazırlama yönergeleri bulabilirsiniz.

> [!NOTE]  
> Bu belgedeki adımlarda Windows tabanlı HDInsight kümelerine özeldir. Linux tabanlı bir küme ile çalışmanıza adımlar için bkz: [Apache Hive, HDInsight (Linux) kullanarak uçuş gecikme verilerini çözümleme](hdinsight-analyze-flight-delay-data-linux.md).

### <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure PowerShell içeren bir iş istasyonu**.

    > [!IMPORTANT]  
    > Azure Service Manager kullanılarak HDInsight kaynaklarının yönetilmesi için Azure PowerShell desteği **kullanım dışı bırakılmış** ve 1 Ocak 2017 tarihinde kaldırılmıştır. Bu belgede yer alan adımlar, Azure Resource Manager ile çalışan yeni HDInsight cmdlet'lerini kullanır.
    >
    > Azure PowerShell’in en son sürümünü yüklemek için lütfen [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs)’daki adımları uygulayın. Azure Resource Manager’la çalışan yeni cmdlet’lerle kullanmak için değiştirilmesi gereken komut dosyalarınız varsa, daha fazla bilgi için bkz. [HDInsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçme](hdinsight-hadoop-development-using-azure-resource-manager.md).

**Bu öğreticide kullanılan dosyalar**

Bu öğreticide Havayolu uçuş verileri tarihlere ait zamanında kalkış performansı [araştırma ve yenilikçi teknoloji yönetim, nakliye İstatistik Enstitüsü veya RITA][rita-website].
Verilerin bir kopyasını ortak Blob erişim izni olan bir Azure Blob Depolama kapsayıcısına karşıya yüklendi.
PowerShell betiğinizin bir parçası veri kümenizin varsayılan blob kapsayıcısı için ortak bir blob kapsayıcısından kopyalar. HiveQL betiğini de aynı Blob kapsayıcısına kopyalanmıştır.
Get/kendi depolama hesabına veri yükleme konusunda ve HiveQL betik dosyası oluşturma/karşıya yükleme, bkz öğrenmek isterseniz [ek A](#appendix-a) ve [ek B](#appendix-b).

Aşağıdaki tabloda, bu öğreticide kullanılan dosyaları listeler:

<table border="1">
<tr><th>Dosyalar</th><th>Açıklama</th></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>Hive işi tarafından kullanılan HiveQL betik dosyası. Bu betik, ortak erişim ile bir Azure Blob Depolama hesabına yüklendi. <a href="#appendix-b">Ek B</a> yönergeler hazırlama ve bu dosyayı karşıya yüklemek, kendi Azure Blob Depolama hesabına sahiptir.</td></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>Hive iş için giriş verileri. Verilere genel erişim ile bir Azure Blob Depolama hesabına yüklendi. <a href="#appendix-a">Ek A</a> veri almanın ve kendi Azure Blob Depolama hesabına veri yükleme yönergeleri açmıştır.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>Hive işi için çıkış yolu. Varsayılan kapsayıcı, çıktı verilerini depolamak için kullanılır.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>Varsayılan kapsayıcı Hive işi durumu klasörü.</td></tr>
</table>

## <a name="create-cluster-and-run-hivesqoop-jobs"></a>Küme oluşturma ve Hive/Sqoop işleri çalıştırma
Hadoop MapReduce toplu işlem ' dir. Bir Hive işi çalıştırmak için en uygun maliyetli iş için bir küme oluşturmak ve iş tamamlandıktan sonra işi silmek için yoludur. Bu işlem aşağıdaki betiği kapsar.
Bir HDInsight kümesi oluşturmayı ve Hive işlerini çalıştırma hakkında daha fazla bilgi için bkz. [Apache Hadoop kümeleri oluşturma HDInsight] [ hdinsight-provision] ve [HDInsight ile Hive kullanma Apache] [hdinsight-use-hive].

**Azure PowerShell ile Hive sorguları çalıştırmak için**

1. İçindeki yönergeleri kullanarak Azure SQL veritabanı ve tablo Sqoop iş çıktısı için oluşturma [ek C](#appendix-c).
2. Windows PowerShell ISE'yi açın ve aşağıdaki betiği çalıştırın:

    ```powershell
    $subscriptionID = "<Azure Subscription ID>"
    $nameToken = "<Enter an Alias>"

    ###########################################
    # You must configure the following variables
    # for an existing Azure SQL Database
    ###########################################
    $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
    $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
    $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
    $existingSqlDatabaseName = "<Azure SQL Database name>"

    $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files.
    $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different

    ###########################################
    # (Optional) configure the following variables
    ###########################################

    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2"

    $HDInsightClusterName = $namePrefix + "hdi"
    $httpUserName = "admin"
    $httpPassword = "<Enter the Password>"

    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $HDInsightClusterName # use the cluster name

    $existingSqlDatabaseTableName = "AvgDelays"
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"

    $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql"

    $jobStatusFolder = "/tutorials/flightdelays/jobstatus"

    ###########################################
    # Login
    ###########################################
    try{
        $acct = Get-AzureRmSubscription
    }
    catch{
        Connect-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionID $subscriptionID

    ###########################################
    # Create a new HDInsight cluster
    ###########################################

    # Create ARM group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create the default storage account
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS

    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext

    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $existingDefaultBlobContainerName

    ###########################################
    # Prepare the HiveQL script and source data
    ###########################################

    # Create the temp location
    New-Item -Path $localFolder -ItemType Directory -Force

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
    $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
    #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload data to default container

    $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"

    Invoke-Expression -Command:$azcopycmd

    ###########################################
    # Submit the Hive job
    ###########################################
    Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
    $response = Invoke-AzureRmHDInsightHiveJob `
                    -Files $hqlScriptFile `
                    -DefaultContainer $defaultBlobContainerName `
                    -DefaultStorageAccountName $defaultStorageAccountName `
                    -DefaultStorageAccountKey $defaultStorageAccountKey `
                    -StatusFolder $jobStatusFolder

    write-Host $response

    ###########################################
    # Submit the Sqoop job
    ###########################################
    $exportDir = "wasb://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                    -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ResourceGroupName $resourceGroupName `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -HttpCredential $httpCredential `
        -WaitTimeoutInSeconds 3600 `
        -Job $sqoopJob.JobId

    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -DefaultContainer $existingDefaultBlobContainerName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    ###########################################
    # Delete the cluster
    ###########################################
    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName
    ```
3. SQL veritabanı'na bağlanma ve AvgDelays tabloda şehre göre ortalama uçuş gecikmeler görebilirsiniz:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]

- - -

## <a id="appendix-a"></a>Ek A - karşıya yükleme uçuş gecikme verilerini Azure Blob Depolama
Veri dosyasını karşıya yükleme ve [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) komut dosyaları (bkz [ek B](#appendix-b)) bazı planlama yapmak gerekir. Veri dosyaları ve bir HDInsight kümesi oluşturmayı ve Hive işi önce HiveQL dosyasını depolamak için kullanılan uygulamadır. İki seçeneğiniz vardır:

* **Varsayılan dosya sistemi olarak HDInsight küme tarafından kullanılacak olan aynı Azure depolama hesabını kullanın.** HDInsight küme depolama hesabı erişim anahtarı bulunacağından, ek değişiklik gerekmez.
* **HDInsight kümesi varsayılan dosya sisteminden farklı bir Azure depolama hesabını kullanırsınız.** Bu durumda, Windows PowerShell komut dosyası oluşturma HDInsight kümesinde bulunan ve ek bir depolama hesabı depolama hesabı bağlamak için Apache Hive/Sqoop işlerinizi oluşturma bölümünü değiştirmeniz gerekir. Yönergeler için [Apache Hadoop kümeleri oluşturma HDInsight][hdinsight-provision]. HDInsight kümesi, ardından depolama hesabı için erişim anahtarı bilir.

> [!NOTE]  
> Blob Depolama yolu veri dosyası için HiveQL betik dosyasında kodlanmış zordur. Uygun şekilde güncelleştirmeniz gerekir.

**Uçuş verileri indirmek için**

1. [Research and Innovative Technology Administration, Bureau of Transportation Statistics][rita-website] (Araştırma ve Yenilikçi Teknolojiler İdaresi, Ulaşım İstatistikleri Bürosu) sayfasına göz atın.
2. Sayfada aşağıdaki değerleri seçin:

    <table border="1">
    <tr><th>Adı</th><th>Değer</th></tr>
    <tr><td>Yıl Filtresi</td><td>2013 </td></tr>
    <tr><td>Dönem Filtresi</td><td>Ocak</td></tr>
    <tr><td>Alanlar</td><td>*Yıl*, *FlightDate*, *UniqueCarrier*, *taşıyıcı*, *FlightNum*, *OriginAirportID*, *Kaynak*, *OriginCityName*, *OriginState*, *DestAirportID*, *Dest*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*,  *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*,  *LateAircraftDelay* (diğer tüm alanlar Temizle)</td></tr>
    </table>

3. **İndir**’e tıklayın.
4. İçin dosyanın sıkıştırmasını açın **C:\Tutorials\FlightDelay\2013Data** klasör. Her dosya, bir CSV dosyasıdır ve yaklaşık 60 GB boyutunda.
5. Dosya verilerini içeren ayın adını yeniden adlandırın. Örneğin, Ocak veriler içeren bir dosya adlı *January.csv*.
6. 2. ve 5 her 12 aylık 2013'te bir dosya yüklemek için adımları yineleyin. Öğreticiyi çalıştırmak için bir dosya en az gerekir.

**Uçuş gecikme verilerini Azure Blob depolama alanına yüklemek için**

1. Parametreleri hazırlayın:

    <table border="1">
    <tr><th>Değişken adı</th><th>Notlar</th></tr>
    <tr><td>$storageAccountName</td><td>Verileri karşıya yüklemek için istediğiniz Azure depolama hesabı.</td></tr>
    <tr><td>$blobContainerName</td><td>Verileri karşıya yüklemek istediğiniz yeri Blob kapsayıcısı.</td></tr>
    </table>
    
2. Open Azure PowerShell ISE.
3. Aşağıdaki komut dosyası, betik bölmesine yapıştırın:

    ```powershell
    [CmdletBinding()]
    Param(

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #Region - Variables
    $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
    $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
    #EndRegion

    #Region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Connect-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
    # Validate the Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate the container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #Region - Copy the file from local workstation to Azure Blob storage
    if (test-path -Path $localFolder)
    {
        foreach ($item in Get-ChildItem -Path $localFolder){
            $fileName = "$localFolder\$item"
            $blobName = "$destFolder/$item"

            Write-Host "Copying $fileName to $blobName" -ForegroundColor Green

            Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
        }
    }
    else
    {
        Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
    }

    # List the uploaded files on HDInsight
    Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
    #EndRegion
    ```
4. Betiği çalıştırmak için **F5**'e basın.

Dosyaları karşıya yükleme için farklı bir yöntem kullanmayı seçerseniz, dosya yolu flightdelay/öğreticiler/veri olmasını sağlayın. Dosyalara erişmek için sözdizimi aşağıdaki gibidir:

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

Öğreticiler/flightdelay/veri yolu, dosyaları karşıya yüklediğiniz sırada oluşturulan sanal klasörüdür. Her ay için 12 dosyaları olduğunu doğrulayın.

> [!NOTE]  
> Yeni konumdan okunamıyor Hive sorgusu güncelleştirmeniz gerekir.
>
> Ayrıca, kapsayıcı erişim iznini genel veya HDInsight kümesi için depolama hesabına bağlama ya da yapılandırmanız gerekir. Aksi takdirde, Hive sorgu dizesi veri dosyalarına erişmek mümkün olmayacaktır.

- - -

## <a id="appendix-b"></a>Ek B - oluşturma ve HiveQL betiğini karşıya yükleyin
Azure PowerShell kullanarak, birden çok çalıştırabileceğiniz [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) deyimleri bir zaman veya paket bir betik dosyasına HiveQL deyimi. Bu bölümde, HiveQL betiğini oluşturma ve betik, Azure PowerShell kullanarak Azure Blob depolama alanına karşıya yükleme işlemini göstermektedir. Hive, Azure Blob Depolama alanında depolanacak HiveQL betikleri gerektirir.

HiveQL betiğini aşağıdakileri gerçekleştirin:

1. **Delays_raw tabloyu bırakmak**, durumda tablo zaten mevcut.
2. **Delays_raw dış Hive tablosu oluşturmak** uçuş gecikme dosyaları Blob depolama konumuna işaret eden. Bu sorgu, alanları tarafından sınırlandırılır belirtir "," ve satırları "\n" tarafından sonlandırılır. Hive alan sınırlayıcı bir virgül ve alan değeri parçası olan bir tane arasında ayrım olmadığından alan değerleri virgül içeriyorsa, bu bir sorun oluşturur (kaynak için alan değerlerini durumda olduğu\_ŞEHİR\_adı ve hedef\_ ŞEHİR\_adı). Bunu ele almak için sorgu yanlış sütuna bölünür verileri tutmak için geçici sütunları oluşturur.
3. **Gecikmeler tabloyu bırakmak**, durumda tablo zaten mevcut.
4. **Gecikmeler tablosu oluşturma**. Daha fazla işleme önce verileri temizlemek yararlıdır. Bu sorgu yeni bir tablo oluşturur *gecikmeleri*, delays_raw tablosundan. Not (daha önce belirtildiği gibi) TEMP sütunları kopyalanmaz ve **substring** işlevi, tırnak işaretleri verileri kaldırmak için kullanılır.
5. **Test sonuçlarını gruplar ve ortalama hava durumu gecikme şehir adı tarafından işlem.** Ayrıca Blob depolama alanına sonuçları çıktı olarak sunar. Sorgu kesme verilerini kaldırır ve dışladığı Not satırları değeri **weather_delay** null. Bunun gerekli olmasının nedeni, Sqoop, daha sonra Bu öğreticide kullanılan değerler düzgün bir şekilde varsayılan olarak işlemiyor.

HiveQL komutların tam listesi için bkz. [Apache Hive veri tanımlama dili][hadoop-hiveql]. Her [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) gerekir Sonlandır noktalı virgül ile komutu.

**HiveQL komut dosyası oluşturmak için**

1. Parametreleri hazırlayın:

    <table border="1">
    <tr><th>Değişken adı</th><th>Notlar</th></tr>
    <tr><td>$storageAccountName</td><td>Azure depolama hesabı için HiveQL betiğini karşıya yüklemek istediğiniz yeri.</td></tr>
    <tr><td>$blobContainerName</td><td>Blob kapsayıcısı için HiveQL betiğini karşıya yüklemek istediğiniz yeri.</td></tr>
    </table>
    
2. Open Azure PowerShell ISE.  

3. Kopyalayın ve aşağıdaki betiği betik bölmesine yapıştırın:  

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Blob storage variables
        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #region - Define variables
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    # The HiveQL script file is exported as this file before it's uploaded to Blob storage
    $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"

    # The HiveQL script file will be uploaded to Blob storage as this blob name
    $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"

    # These two constants are used by the HiveQL script file
    #$srcDataFolder = "tutorials/flightdelay/data"
    $dstDataFolder = "/tutorials/flightdelay/output"
    #endregion

    #Region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Connect-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
    # Validate the Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate the container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #region - Validate the file and file path

    # Check if a file with the same file name already exists on the workstation
    Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
    if (test-path $hqlLocalFileName){

        $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwrite it? (Y/N)'

        if ($isDelete.ToLower() -ne "y")
        {
            Exit
        }
    }

    # Create the folder if it doesn't exist
    $folder = split-path $hqlLocalFileName
    if (-not (test-path $folder))
    {
        Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green

        new-item $folder -ItemType directory
    }
    #end region

    #region - Write the Hive script into a local file
    Write-Host "`nWriting the Hive script into a file on your workstation ..." `
                -ForegroundColor Green

    $hqlDropDelaysRaw = "DROP TABLE delays_raw;"

    $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
            "YEAR string, " +
            "FL_DATE string, " +
            "UNIQUE_CARRIER string, " +
            "CARRIER string, " +
            "FL_NUM string, " +
            "ORIGIN_AIRPORT_ID string, " +
            "ORIGIN string, " +
            "ORIGIN_CITY_NAME string, " +
            "ORIGIN_CITY_NAME_TEMP string, " +
            "ORIGIN_STATE_ABR string, " +
            "DEST_AIRPORT_ID string, " +
            "DEST string, " +
            "DEST_CITY_NAME string, " +
            "DEST_CITY_NAME_TEMP string, " +
            "DEST_STATE_ABR string, " +
            "DEP_DELAY_NEW float, " +
            "ARR_DELAY_NEW float, " +
            "CARRIER_DELAY float, " +
            "WEATHER_DELAY float, " +
            "NAS_DELAY float, " +
            "SECURITY_DELAY float, " +
            "LATE_AIRCRAFT_DELAY float) " +
        "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
        "LINES TERMINATED BY '\n' " +
        "STORED AS TEXTFILE " +
        "LOCATION 'wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"

    $hqlDropDelays = "DROP TABLE delays;"

    $hqlCreateDelays = "CREATE TABLE delays AS " +
        "SELECT YEAR AS year, " +
            "FL_DATE AS flight_date, " +
            "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
            "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
            "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
            "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
            "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
            "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
            "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
            "DEST_AIRPORT_ID AS dest_airport_id, " +
            "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
            "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
            "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
            "DEP_DELAY_NEW AS dep_delay_new, " +
            "ARR_DELAY_NEW AS arr_delay_new, " +
            "CARRIER_DELAY AS carrier_delay, " +
            "WEATHER_DELAY AS weather_delay, " +
            "NAS_DELAY AS nas_delay, " +
            "SECURITY_DELAY AS security_delay, " +
            "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
        "FROM delays_raw;"

    $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
        "SELECT regexp_replace(origin_city_name, '''', ''), " +
            "avg(weather_delay) " +
        "FROM delays " +
        "WHERE weather_delay IS NOT NULL " +
        "GROUP BY origin_city_name;"

    $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal

    $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
    #endregion

    #region - Upload the Hive script to the default Blob container
    Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green

    # Create a storage context object
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Upload the file from local workstation to Blob storage
    Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
    #endregion

    Write-host "`nEnd of the PowerShell script" -ForegroundColor Green
    ```

    Betikte kullanılan değişkenleri şunlardır:

   * **$hqlLocalFileName** -betik HiveQL betiği yerel olarak Blob depolama alanına karşıya yüklemeden önce kaydeder. Bu dosya adıdır. Varsayılan değer <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
   * **$hqlBlobName** -Azure Blob Depolama alanında kullanılan HiveQL betik dosyası blob adı budur. Tutorials/flightdelay/flightdelays.hql varsayılan değerdir. Dosyayı doğrudan Azure Blob depolama alanına yazılacak olduğundan değil blob adı başındaki "/". Blob depolama alanından dosyaya erişmek istiyorsanız, dosya adının başındaki "/" eklemeniz gerekir.
   * **$srcDataFolder** ve **$dstDataFolder** -"öğreticiler/flightdelay/verileri" = "öğreticiler/flightdelay/çıkış" =

- - -
## <a id="appendix-c"></a>Ek C - Sqoop iş çıkışı bir Azure SQL veritabanını hazırlamak
**SQL veritabanını hazırlamak için (Sqoop komut dosyasıyla Birleştir)**

1. Parametreleri hazırlayın:

    <table border="1">
    <tr><th>Değişken adı</th><th>Notlar</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>Azure SQL veritabanı sunucu adı. Yeni bir sunucu oluşturmak için hiçbir şey girin.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>Azure SQL veritabanı sunucusu için oturum açma adı. Mevcut bir sunucu $sqlDatabaseServerName ise oturum açma ve oturum açma parolası sunucunun kimliğini doğrulamak için kullanılır. Aksi takdirde bunlar yeni bir sunucu oluşturmak için kullanılır.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>Azure SQL veritabanı sunucusu için oturum açma parolası.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Yalnızca yeni bir Azure veritabanı sunucusunu oluştururken bu değeri kullanılır.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>Sqoop işin AvgDelays tablo oluşturmak için kullanılan SQL veritabanı. Boş bırakılırsa HDISqoop adlı bir veritabanı oluşturur. AvgDelays Sqoop iş çıktısı için tablo adıdır. </td></tr>
    </table>
    
2. Open Azure PowerShell ISE.

3. Kopyalayın ve aşağıdaki betiği betik bölmesine yapıştırın:  

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Resource group variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
        [String]$resourceGroupName,

        # SQL database server variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseServer,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure SQL Database admin user.")]
        [String]$sqlDatabaseLogin,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure SQL Database admin user password.")]
        [String]$sqlDatabasePassword,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter the region to create the Database in.")]
        [String]$sqlDatabaseLocation,   #For example, West US.

        # SQL database variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
    )

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Constants and variables

    # IP address REST service used for retrieving external IP address and creating firewall rules
    [String]$ipAddressRestService = "https://bot.whatismyipaddress.com"
    [String]$fireWallRuleName = "FlightDelay"

    # SQL database variables
    [String]$sqlDatabaseMaxSizeGB = 10

    #SQL query string for creating AvgDelays table
    [String]$sqlDatabaseTableName = "AvgDelays"
    [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [origin_city_name] [nvarchar](50) NOT NULL,
                [weather_delay] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
                [origin_city_name] ASC
            )
            )"
    #endregion

    #Region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Connect-AzureRmAccount}
    #EndRegion

    #region - Create and validate Azure resource group
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
    }

    #EndRegion

    #region - Create and validate Azure SQL database server
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress

        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
    }

    #endregion

    #region - Create and validate Azure SQL database

    try {
        Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
    }

    #endregion

    #region -  Execute an SQL command to create the AvgDelays table

    Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = $sqlCreateAvgDelaysTable
    $cmd.executenonquery()

    $conn.close()

    Write-host "`nEnd of the PowerShell script" -ForegroundColor Green
    ```

   > [!NOTE]  
   > Betik bir temsili durum aktarımı (REST) hizmeti kullanır https://bot.whatismyipaddress.com, dış IP adresiniz alınacak. IP adresi, SQL veritabanı sunucunuz için bir güvenlik duvarı kuralı oluşturmak için kullanılır.

    Betikte kullanılan bazı değişkenleri şunlardır:

   * **$ipAddressRestService** -varsayılan değer https://bot.whatismyipaddress.com. Genel bir IP adresi, dış IP adresini almak için REST hizmeti var. İsterseniz, diğer hizmetleri kullanabilirsiniz. (Bir Windows PowerShell Betiği kullanılarak) veritabanı istasyonunuzdan erişebilmeleri hizmet aracılığıyla alınan dış IP adresi, Azure SQL veritabanı sunucunuz için bir güvenlik duvarı kuralı oluşturmak için kullanılır.
   * **$fireWallRuleName** -Azure SQL veritabanı sunucusu için güvenlik duvarı kuralı adı budur. Varsayılan ad <u>FlightDelay</u>. İsterseniz yeniden adlandırabilirsiniz.
   * **$sqlDatabaseMaxSizeGB** -yalnızca yeni bir Azure SQL veritabanı sunucusunu oluştururken bu değeri kullanılır. Varsayılan değer 10 GB'tır. 10GB, Bu öğretici için yeterlidir.
   * **$sqlDatabaseName** -yalnızca yeni bir Azure SQL veritabanı oluştururken bu değeri kullanılır. HDISqoop varsayılan değerdir. Yeniden adlandırmak, Sqoop Windows PowerShell komut dosyasını uygun şekilde güncelleştirmeniz gerekir.
4. Betiği çalıştırmak için **F5**'e basın.
5. Betik çıktısı doğrulayın. Betik başarıyla çalıştırdınız emin olun.

## <a id="nextsteps"></a> Sonraki adımlar
Artık Azure Blob depolama alanına bir dosya karşıya yükleme, Azure Blob depolama alanındaki verilerin kullanarak bir Apache Hive tablosu doldurmak nasıl, Hive sorgularının nasıl çalıştırılacağını ve verileri dışarı aktarmak için Sqoop kullanma anladığınıza göre [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html) için bir Azure SQL Veritabanı. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight ile çalışmaya başlama][hdinsight-get-started]
* [Apache Hive, HDInsight ile kullanma][hdinsight-use-hive]
* [HDInsight ile Apache Oozie kullanma][hdinsight-use-oozie]
* [HDInsight ile Apache Sqoop'u kullanma][hdinsight-use-sqoop]
* [Apache Pig, HDInsight ile kullanma][hdinsight-use-pig]
* [HDInsight için Java MapReduce programları geliştirme][hdinsight-develop-mapreduce]

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[rita-website]: https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
[hdinsight-develop-mapreduce]:hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: https://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: https://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
