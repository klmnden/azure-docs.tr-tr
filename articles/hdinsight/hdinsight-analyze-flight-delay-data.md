---
title: Uçuş gecikme verilerini hdınsight'ta - Azure Hadoop ile çözümleme | Microsoft Docs
description: Hdınsight kümesi oluşturmak, bir Hive işi çalıştırın, Sqoop işini çalıştırın ve kümeyi silmek için bir Windows PowerShell komut dosyası kullanmayı öğrenin.
services: hdinsight
documentationcenter: ''
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00e26aa9-82fb-4dbe-b87d-ffe8e39a5412
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 6e42cfa666ad6b6523043f4412a321789adad9a1
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Hdınsight'ta Hive kullanarak uçuş gecikme verilerini çözümleme
Hive sağlar Hadoop MapReduce işleri adlı bir SQL benzeri komut dosyası dili ile çalışan bir  *[HiveQL][hadoop-hiveql]*, hangi uygulanabilir özetlemeye doğrultusunda, sorgulama, ve büyük miktarda veriyi analiz etme.

> [!IMPORTANT]
> Bu belgede yer alan adımlar Windows tabanlı Hdınsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). Linux tabanlı bir küme ile çalışma adımları için bkz: [(Linux) hdınsight'ta Hive kullanarak uçuş gecikme verilerini çözümlemek](hdinsight-analyze-flight-delay-data-linux.md).

Azure Hdınsight, büyük avantajlarından biri veri depolama ve işlem ayrılmasıdır. Hdınsight Azure Blob Depolama, veri depolaması için kullanır. Tipik bir işi üç bölümden oluşur:

1. **Verileri Azure Blob Depolama alanında depolar.**  Örneğin, verileri, algılayıcı verilerini, web günlükleri hava durumu ve bu durumda, uçuş gecikme verileri Azure Blob depolama alanına kaydedilir.
2. **İşlerini çalıştırın.** Verileri bir Hdınsight kümesi oluşturmak için bir Windows PowerShell komut dosyası (veya bir istemci uygulaması) çalıştırmadan işleme zamanı geldiğinde işleri çalıştırma ve küme silin. İşlerini çıktı verileri Azure Blob depolama alanına kaydedin. Küme bile silindikten sonra çıktı verileri korunur. Bu şekilde, yalnızca ne, tüketilen için ücret ödersiniz.
3. **Çıktı Azure Blob Depolama'dan alın**, veya Bu öğreticide, verileri bir Azure SQL veritabanına verebilirsiniz.

Aşağıdaki diyagram, senaryo ve bu öğreticinin yapısını gösterir:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

Diyagramdaki sayıları bölüm başlıkları karşılık unutmayın. **M** için ana işlem anlamına gelir. **A** ek içerik için anlamına gelir.

Öğreticinin ana bölümü, bir Windows PowerShell Betiği aşağıdaki görevleri gerçekleştirmek için nasıl kullanılacağını gösterir:

* Hdınsight kümesi oluşturun.
* Ortalama gecikmelerden havaalanları hesaplamak için küme üzerinde bir Hive işi çalıştırın. Uçuş gecikme veriler bir Azure Blob Depolama hesabında depolanır.
* Hive işi çıkışı bir Azure SQL veritabanı için dışarı aktarmak için bir Sqoop işi çalıştırın.
* Hdınsight kümesi silin.

İlişkisini, uçuş gecikme veri yüklemek, Hive sorgu dizesi oluşturma/yükleme ve Azure SQL veritabanı için Sqoop işi hazırlama için yönergeler bulabilirsiniz.

> [!NOTE]
> Bu belgede yer alan adımlar Windows tabanlı Hdınsight kümelerine özeldir. Linux tabanlı bir küme ile çalışma adımları için bkz: [(Linux) hdınsight'ta Hive kullanarak uçuş gecikme verileri analiz](hdinsight-analyze-flight-delay-data-linux.md)

### <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure PowerShell içeren bir iş istasyonu**.

    > [!IMPORTANT]
    > Azure Service Manager kullanılarak HDInsight kaynaklarının yönetilmesi için Azure PowerShell desteği **kullanım dışı bırakılmış** ve 1 Ocak 2017 tarihinde kaldırılmıştır. Bu belgede yer alan adımlar, Azure Resource Manager ile çalışan yeni HDInsight cmdlet'lerini kullanır.
    >
    > Azure PowerShell’in en son sürümünü yüklemek için lütfen [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs)’daki adımları uygulayın. Azure Resource Manager’la çalışan yeni cmdlet’lerle kullanmak için değiştirilmesi gereken komut dosyalarınız varsa, daha fazla bilgi için bkz. [HDInsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçme](hdinsight-hadoop-development-using-azure-resource-manager.md).

**Bu öğreticide kullanılan dosyaları**

Bu öğretici uçak uçuş verileri zamanında performansını kullanır [araştırma ve yenilikçi teknoloji yönetim, taşıma İstatistik Enstitüsü veya RITA][rita-website].
Verilerin bir kopyasını ortak Blob erişim izni olan bir Azure Blob Depolama kapsayıcısını karşıya yüklendi.
PowerShell Betiği parçası Veri kümenizi varsayılan blob kapsayıcısına ortak blob kapsayıcısından kopyalar. HiveQL betiğini de Blob kapsayıcıya kopyalanır.
Get/kendi depolama hesabına veri yükleme etme ve HiveQL komut dosyası oluşturun/karşıya yükleme, bkz bilgi edinmek istiyorsanız [ek A](#appendix-a) ve [ek B](#appendix-b).

Aşağıdaki tabloda, bu öğreticide kullanılan dosyaları listeler:

<table border="1">
<tr><th>Dosyalar</th><th>Açıklama</th></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>Hive işi tarafından kullanılan HiveQL komut dosyası. Bu komut dosyasını bir Azure Blob Depolama hesabına genel erişim ile karşıya yüklendi. <a href="#appendix-b">Ek B</a> yönergeler hazırlama ve bu dosyayı karşıya yüklemeyi kendi Azure Blob storage hesabına sahiptir.</td></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>Hive işi için giriş verileri. Verileri Azure Blob Depolama hesabına genel erişim ile karşıya yüklendi. <a href="#appendix-a">Ek A</a> verileri almak ve veri kendi Azure Blob Depolama hesabına yükleniyor yönergeleri açmıştır.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>Hive işi için çıkış yolu. Varsayılan kapsayıcı, çıktı verilerini depolamak için kullanılır.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>Varsayılan kapsayıcı Hive işi durumu klasör.</td></tr>
</table>

## <a name="create-cluster-and-run-hivesqoop-jobs"></a>Küme oluşturma ve Hive/Sqoop işleri çalıştırma
Hadoop MapReduce toplu işlemesidir. Hive işi çalıştırmak için en uygun maliyetli iş için bir küme oluşturmak ve işi tamamlandıktan sonra işi silmek için yoludur. Aşağıdaki komut dosyası tüm işlem kapsar.
Hdınsight kümesi oluşturma ve Hive işleri çalıştırma hakkında daha fazla bilgi için bkz: [Hdınsight'ta oluşturmak Hadoop kümeleri] [ hdinsight-provision] ve [Hdınsight ile Hive kullanma] [hdinsight-use-hive].

**Azure PowerShell ile Hive sorguları çalıştırmak için**

1. ' Ndaki yönergeleri kullanarak bir Azure SQL veritabanı ve tablo Sqoop iş çıktısı için oluşturma [ek C](#appendix-c).
2. Windows PowerShell ISE açın ve aşağıdaki komut dosyasını çalıştırın:

    ```powershell
    $subscriptionID = "<Azure Subscription ID>"
    $nameToken = "<Enter an Alias>"

    ###########################################
    # You must configure the follwing variables
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
        Login-AzureRmAccount
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
3. SQL veritabanına bağlama ve AvgDelays tablo şehirde ortalama uçuş gecikmelerinden bakın:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]

- - -

## <a id="appendix-a"></a>Ek A - karşıya yükleme uçuş gecikme verileri Azure Blob Depolama
Veri dosyası ve HiveQL komut dosyaları karşıya yükleme (bkz [ek B](#appendix-b)) bazı planlama gerektirir. Veri dosyaları ve Hdınsight kümesi oluşturma ve Hive işi çalıştırma önce HiveQL dosyasını depolamak için kullanılan uygulamadır. İki seçeneğiniz vardır:

* **Varsayılan dosya sistemi olarak Hdınsight küme tarafından kullanılacak aynı Azure depolama hesabı kullanın.** Hdınsight kümesi depolama hesabı erişim tuşu sahip olacağından ek değişiklik gerekmez.
* **Hdınsight küme varsayılan dosya sistemi farklı bir Azure Storage hesabını kullanın.** Bu durumda, Windows PowerShell komut dosyası bulundu oluşturma parçası değiştirmelisiniz [oluşturma Hdınsight kümesi ve çalışma Hive/Sqoop işleri](#runjob) ek depolama alanı hesabı olarak depolama hesabı bağlamak için. Yönergeler için bkz: [Hdınsight'ta oluşturmak Hadoop kümeleri][hdinsight-provision]. Hdınsight kümesi sonra depolama hesabının erişim anahtarı bilir.

> [!NOTE]
> Blob Depolama yolu veri dosyası için HiveQL komut dosyasında kodlanmış zordur. Buna uygun şekilde güncelleştirmeniz gerekir.

**Uçuş veri indirmek için**

1. Gözat [araştırma ve yenilikçi teknoloji yönetim, taşıma İstatistik kuruluşu][rita-website].
2. Sayfasında, aşağıdaki değerleri seçin:

    <table border="1">
    <tr><th>Ad</th><th>Değer</th></tr>
    <tr><td>Filtre yıl</td><td>2013 </td></tr>
    <tr><td>Dönem filtre</td><td>Ocak</td></tr>
    <tr><td>Alanlar</td><td>*Yıl*, *FlightDate*, *UniqueCarrier*, *taşıyıcı*, *FlightNum*, *OriginAirportID*, *Kaynak*, *OriginCityName*, *OriginState*, *DestAirportID*, *taşınmaya*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*,  *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*,  *LateAircraftDelay* (diğer tüm alanlar Temizle)</td></tr>
    </table>
3. Tıklatın **karşıdan**.
4. Dosyanın sıkıştırmasını açın **C:\Tutorials\FlightDelay\2013Data** klasör. Her dosya, bir CSV dosyası ve yaklaşık 60 GB boyutunda.
5. Dosya verilerini içeren ayın adını yeniden adlandırın. Örneğin, Ocak verilerini içeren dosyayı adlı *January.csv*.
6. 2 ve her 12 ay 2013'te bir dosyayı indirmek için 5. adımları yineleyin. Öğretici çalıştırmak için bir dosya en az gerekir.

**Uçuş gecikme verileri Azure Blob depolama alanına yüklemek için**

1. Parametreleri hazırlayın:

    <table border="1">
    <tr><th>Değişken adı</th><th>Notlar</th></tr>
    <tr><td>$storageAccountName</td><td>Verileri karşıya yüklemek istediğiniz Azure depolama hesabı.</td></tr>
    <tr><td>$blobContainerName</td><td>Verileri karşıya yüklemek istediğiniz Blob kapsayıcısı.</td></tr>
    </table>
2. Azure PowerShell ISE açın.
3. Aşağıdaki komut dosyası komut dosyası bölmesine yapıştırın:

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
    catch{Login-AzureRmAccount}
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

Dosyaları yüklemek için farklı bir yöntem kullanmayı tercih ederseniz, lütfen flightdelay/öğreticileri/veri dosya yolu olduğundan emin olun. Dosyalara erişmek için sözdizimi aşağıdaki gibidir:

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

Öğreticiler/flightdelay/veri yolu dosyaları karşıya yüklediğiniz sırada oluşturulan sanal klasörüdür. 12 dosyaları, her ay için bir tane olduğundan emin olun.

> [!NOTE]
> Yeni konumdan okumak için Hive sorgusu güncelleştirmeniz gerekir.
>
> Genel veya Hdınsight kümesi için depolama hesabı bağlamak için kapsayıcı erişim izni ya da yapılandırmanız gerekir. Aksi takdirde, Hive sorgu dizesi veri dosyalarına erişmesini mümkün olmaz.

- - -

## <a id="appendix-b"></a>Ek B - oluşturun ve HiveQL betiğini yükleyin
Azure PowerShell kullanarak, aynı anda birden çok HiveQL ifadelerini bir çalıştırma veya bir komut dosyası HiveQL ifadesine paket. Bu bölümde HiveQL komut dosyası oluşturabilir ve Azure PowerShell kullanarak Azure Blob depolama alanına komut dosyasını karşıya gösterilmektedir. Hive Azure Blob Depolama alanında depolanacak HiveQL betikleri gerektirir.

HiveQL betiğini aşağıdakileri gerçekleştirir:

1. **Delays_raw tablo bırakma**, tablo zaten mevcut durumda.
2. **Delays_raw dış Hive tablosu oluşturmak** uçuş gecikme dosyalarıyla Blob depolama konumuna işaret eden. Bu sorgu alanları tarafından sınırlandırılır belirtir "," ve "tarafından \n" satırları sonlandırılır. Alan değerleri virgül içerdiğinde Hive alan sınırlayıcı virgül ve alan değeri parçası olan bir tane arasında ayrım çünkü bu bir sorun oluşturur (kaynak için alan değerlerini durumda olduğu\_ŞEHİR\_adı ve hedef\_ ŞEHİR\_adı). Bu sorunu çözmek için yanlış sütunlara bölme verileri tutmak için TEMP sütunları sorgu oluşturur.
3. **Gecikmeler tablo bırakma**, tablo zaten mevcut durumda.
4. **Gecikmeler tablosu oluşturma**. Daha fazla işleme önce verileri temizlemek yararlıdır. Bu sorgu yeni bir tablo oluşturur *gecikmeler*, delays_raw tablosundan. (Daha önce belirtildiği gibi) TEMP sütunları kopyalanmadı Not ve **substring** işlevi tırnak işaretleri verileri kaldırmak için kullanılır.
5. **Ortalama hava durumu gecikmesi ve test sonuçlarını gruplar Şehir ada göre işlem.** Ayrıca Blob Depolama sonuçları çıktı. Sorgu kesme verileri kaldırır ve dışlayacak Not satırları değeri **weather_delay** null. Daha sonra Bu öğreticide kullanılan Sqoop, bu değerler varsayılan olarak işleyebilmesini değil çünkü bu gereklidir.

HiveQL komutları tam bir listesi için bkz: [Hive veri tanımlama dili][hadoop-hiveql]. Noktalı virgül Sonlandır her HiveQL komutu gerekir.

**HiveQL komut dosyası oluşturmak için**

1. Parametreleri hazırlayın:

    <table border="1">
    <tr><th>Değişken adı</th><th>Notlar</th></tr>
    <tr><td>$storageAccountName</td><td>HiveQL betiğini karşıya yüklemek istediğiniz Azure depolama hesabı.</td></tr>
    <tr><td>$blobContainerName</td><td>HiveQL betiğini karşıya yüklemek istediğiniz Blob kapsayıcısı.</td></tr>
    </table>
    
2. Azure PowerShell ISE açın.  

3. Kopyalayın ve aşağıdaki komut dosyası komut dosyası bölmesine yapıştırın:  

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
    catch{Login-AzureRmAccount}
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

        $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwirte it? (Y/N)'

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

    Komut dosyasında kullanılan değişkenleri şunlardır:

   * **$hqlLocalFileName** -komut dosyası HiveQL komut dosyası yerel olarak Blob depolama alanına karşıya yüklemeden önce kaydeder. Bu dosya adı değil. Varsayılan değer <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
   * **$hqlBlobName** -Azure Blob depolama alanına kullanılan HiveQL komut dosyası blob adı budur. Tutorials/flightdelay/flightdelays.hql varsayılan değerdir. Dosyayı doğrudan Azure Blob depolama alanına yazılır, var olmadığından bir "/" blob adının başında. Blob depolama alanından dosyaya erişmek istiyorsanız, bir "/" dosya adının başında eklemeniz gerekir.
   * **$srcDataFolder** ve **$dstDataFolder** -"flightdelay/öğreticileri/data" = "flightdelay/öğreticileri/çıktı" =

- - -
## <a id="appendix-c"></a>Ek C - Sqoop iş çıktısı Azure SQL veritabanını hazırlama
**SQL veritabanını hazırlamak için (Sqoop komut dosyasıyla Birleştir)**

1. Parametreleri hazırlayın:

    <table border="1">
    <tr><th>Değişken adı</th><th>Notlar</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>Azure SQL veritabanı sunucusu adı. Yeni bir sunucu oluşturmak için hiçbir şey girin.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>Azure SQL veritabanı sunucusu için oturum açma adı. $SqlDatabaseServerName var olan bir sunucu ise, oturum açma ve oturum açma parolası sunucunun kimliğini doğrulamak için kullanılır. Aksi halde bunlar yeni bir sunucu oluşturmak için kullanılır.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>Azure SQL veritabanı sunucusu için oturum açma parolası.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Yalnızca yeni bir Azure veritabanı sunucusu oluştururken bu değeri kullanılır.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>Sqoop iş AvgDelays tablo oluşturmak için kullanılan SQL veritabanı. Boş bırakarak HDISqoop adlı bir veritabanı oluşturur. AvgDelays Sqoop iş çıktısı için tablo adıdır. </td></tr>
    </table>
    
2. Azure PowerShell ISE açın.

3. Kopyalayın ve aşağıdaki komut dosyası komut dosyası bölmesine yapıştırın:  

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
    [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
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
    catch{Login-AzureRmAccount}
    #EndRegion

    #region - Create and validate Azure resouce group
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
   > Komut dosyasını bir temsili durum aktarımı (REST) hizmeti kullanan http://bot.whatismyipaddress.com, dış IP adresi alınamadı. IP adresi, SQL veritabanı sunucunuz için bir güvenlik duvarı kuralı oluşturmak için kullanılır.

    Komut dosyasında kullanılan bazı değişkenler şunlardır:

   * **$ipAddressRestService** -varsayılan değer http://bot.whatismyipaddress.com. Bir ortak IP adresi, dış IP adresi almak için REST hizmeti değil. İsterseniz diğer hizmetler kullanabilirsiniz. Böylece (bir Windows PowerShell komut dosyası kullanarak) veritabanı istasyonunuzdan erişebilirsiniz hizmet aracılığıyla alınan dış IP adresi, Azure SQL veritabanı sunucusu için bir güvenlik duvarı kuralı oluşturmak için kullanılır.
   * **$fireWallRuleName** -güvenlik duvarı kuralı adı Azure SQL veritabanı sunucusu budur. Varsayılan ad <u>FlightDelay</u>. İstiyorsanız, onu yeniden adlandırabilirsiniz.
   * **$sqlDatabaseMaxSizeGB** -yalnızca yeni bir Azure SQL veritabanı sunucusu oluştururken bu değer kullanılır. Varsayılan değer 10 GB'tır. Bu öğretici için 10GB yeterlidir.
   * **$sqlDatabaseName** -yalnızca yeni bir Azure SQL veritabanı oluştururken bu değer kullanılır. HDISqoop varsayılan değerdir. Adlandırırsanız, Sqoop Windows PowerShell komut dosyasını uygun şekilde güncelleştirmeniz gerekir.
4. Betiği çalıştırmak için **F5**'e basın.
5. Komut dosyası çıkışını doğrulayın. Komut dosyası başarıyla çalıştırıldığını doğrulayın.

## <a id="nextsteps"></a> Sonraki adımlar
Şimdi bir dosyayı Azure Blob depolama alanına yüklemek nasıl, verileri Azure Blob depolama biriminden kullanarak bir Hive tablosu doldurmak nasıl, Hive sorgularını çalıştırma ve Sqoop veri HDFS bir Azure SQL veritabanı için dışarı aktarmak için nasıl kullanılacağını anlayın. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hdınsight kullanmaya başlama][hdinsight-get-started]
* [HDInsight ile Hive kullanma][hdinsight-use-hive]
* [Oozie Hdınsight ile kullanma][hdinsight-use-oozie]
* [Hdınsight ile Sqoop kullanma][hdinsight-use-sqoop]
* [HDInsight ile Pig kullanma][hdinsight-use-pig]
* [Hdınsight için Java MapReduce programlar geliştirmek][hdinsight-develop-mapreduce]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
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
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
