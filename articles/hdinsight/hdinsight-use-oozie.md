---
title: HDInsight, Hadoop Oozie kullanma
description: HDInsight, büyük veri hizmeti Hadoop Oozie kullanma. Bir Oozie iş akışının tanımlayın ve Oozie işi gönderme hakkında bilgi edinin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/25/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 25694a44b1474cccaf62ed5005566cdb5230b15c
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58360869"
---
# <a name="use-apache-oozie-with-apache-hadoop-to-define-and-run-a-workflow-in-hdinsight"></a>Tanımlamak ve iş akışı çalıştırma HDInsight için Apache Hadoop ile Apache Oozie kullanma
[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Bir iş akışını tanımlayabilir ve iş akışını HDInsight üzerinde çalıştırmak için Apache Oozie kullanmayı öğrenin. Oozie Düzenleyicisi hakkında bilgi edinmek için [HDInsight ile zamana dayalı Apache Oozie Düzenleyici kullanma][hdinsight-oozie-coordinator-time]. Azure Data Factory bilgi edinmek için [kullanım Apache Pig ve Apache Hive ile veri fabrikası][azure-data-factory-pig-hive].

Apache Oozie, Hadoop işlerini yöneten bir iş akışı/koordinasyon sistemidir. Bu Hadoop yığını ile tümleştirilir ve Apache MapReduce, Apache Pig, Apache Hive ve Apache Sqoop için Hadoop işlerini destekler. Ayrıca, Java programları veya kabuk betikleri gibi sisteme özel işleri planlamak için de kullanılabilir.

Bu öğreticide yönergeleri takip ederek uygulama iş akışı iki eylemleri içerir:

![İş akışı diyagramı][img-workflow-diagram]

1. Bir Hive eylemi bir Apache Log4j dosyasını her günlük düzeyi türünün sayısını için HiveQL betiğini çalıştırır. Her log4j dosyasını türünün ve önem gösterir, örneğin bir [günlük düzeyi] alan içeren bir dizi alanlarının oluşur:
   
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
   
    Benzer şekilde Hive betik çıktısı:
   
        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4
   
    Hive hakkında daha fazla bilgi için bkz. [HDInsight ile Hive kullanma Apache][hdinsight-use-hive].
2. Sqoop eylem HiveQL çıktı Azure SQL veritabanındaki tabloya dışarı aktarır. Sqoop hakkında daha fazla bilgi için bkz: [HDInsight ile Apache Sqoop'u kullanma][hdinsight-use-sqoop].

> [!NOTE]  
> HDInsight kümelerinde desteklenen Oozie sürümleri için bkz: [HDInsight tarafından sağlanan Apache Hadoop küme sürümlerindeki yenilikler nelerdir?] [hdinsight-versions].
> 
> 

### <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiye başlamadan önce aşağıdaki öğe olmalıdır:

* **Azure PowerShell içeren bir iş istasyonu**. 
  

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]
  

## <a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Oozie iş akışının ve ilgili HiveQL betiğini tanımlayın
Oozie iş akışı tanımları hPDL (XML işlem tanımı dili) yazılır. Varsayılan iş akışı dosya adı *workflow.xml*. Bu öğreticide kullandığınız iş akışı dosyası verilmiştir.
```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>

        <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.job.queue.name</name>
                        <value>${queueName}</value>
                    </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
        </action>

        <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.compress.map.output</name>
                        <value>true</value>
                    </property>
                </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
            </sqoop>
            <ok to="end"/>
            <error to="fail"/>
        </action>

        <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>

        <end name="end"/>
    </workflow-app>
```
İş akışında tanımlanan iki eylemler vardır. Başlangıç için eylem *RunHiveScript*. Eylem başarıyla çalışırsa, sonraki eylem ise *RunSqoopExport*.

RunHiveScript birkaç değişkeni yok. Azure PowerShell kullanarak Oozie iş istasyonunuzdan gönderdiğinizde değerlerini geçirirsiniz.

|İş akışı değişkenleri|Açıklama|
|---|---|
|${jobTracker}|Hadoop işi İzleyicisi URL'sini belirtir. Kullanım **jobtrackerhost:9010** HDInsight sürüm 3.0 ve 2.1 içinde.|
|${nameNode}|Hadoop adı düğüm URL'sini belirtir. Örneğin, varsayılan dosya sistemi adresi kullanın *wasb: / /&lt;containerName&gt;\@&lt;storageAccountName&gt;. blob.core.windows.net*.|
|${queueName}|İşin gönderildiği kuyruk adı belirtir. Kullanım **varsayılan**.|

|Hive eylem değişkeni|Açıklama|
|---|---|
|${hiveDataFolder}|Hive tablosu oluşturma komutu için kaynak dizini belirtir.|
|${hiveOutputFolder}|ÜZERİNE INSERT deyimi için çıkış klasörü belirtir.|
|${hiveTableName}|Log4j veri dosyalarına başvuran bir Hive tablosu adını belirtir.|

|Sqoop eylem değişkeni|Açıklama|
|---|---|
|${sqlDatabaseConnectionString}|Azure SQL veritabanı bağlantı dizesini belirtir.|
|${sqlDatabaseTableName}|Veriler için dışa Azure SQL veritabanı tablosunu belirtir.|
|${hiveOutputFolder}|Hive Ekle üzerine deyimi için çıkış klasörü belirtir. Sqoop dışa aktar (dışarı aktarma-dir) aynı klasörde budur.|

Oozie iş akışının ve iş akışı eylemlerini kullanma hakkında daha fazla bilgi için bkz. [Apache Oozie 4.0 belgeleri] [ apache-oozie-400] (HDInsight sürüm 3.0 için) veya [ApacheOozie3.3.2belgeleri] [ apache-oozie-332] (için HDInsight sürüm 2.1).

Hive iş akışı eylemi HiveQL komut dosyasını çağırır. Bu komut dosyası üç HiveQL ifadelerini içerir:

    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

1. **DROP TABLE deyimi** log4j Hive tablosu varsa siler.
2. **CREATE TABLE deyimi** log4j günlük dosyasının konumuna işaret log4j Hive dış tablo oluşturur. Alan sınırlayıcı ",". Varsayılan satır sınırlayıcı "\n" dir. Hive dış tablodaki veri dosyasındaki Oozie iş akışının birden çok kez çalıştırmak istiyorsanız özgün konumundan kaldırılmasını önlemek için kullanılır.
3. **INSERT üzerine deyimi** log4j Hive tablosundaki her günlük düzeyi türünün sayar ve çıktısını Azure Depolama'daki bir blob kaydeder.

Betikte kullanılan üç değişkenler vardır:

* ${hiveTableName}
* ${hiveDataFolder}
* ${hiveOutputFolder}

İş akışı tanımı dosyası (Bu öğreticide workflow.xml) bu değerleri çalışma zamanında bu HiveQL betiğini geçirir.

İş akışı dosyasını hem de HiveQL dosya bir blob kapsayıcısında depolanır.  Daha sonra Bu öğreticide kullandığınız PowerShell Betiği iki dosyayı varsayılan depolama hesabına kopyalar. 

## <a name="submit-oozie-jobs-using-powershell"></a>PowerShell kullanarak Oozie işlerini gönderme
Azure PowerShell, şu anda Oozie işleri tanımlamak için tüm cmdlet'leri sağlamaz. Kullanabileceğiniz **Invoke-RestMethod** cmdlet'ini Oozie web hizmetlerini çağır. Oozie web servisleri API bir HTTP REST JSON API'dir. Oozie web hizmetleri API'si hakkında daha fazla bilgi için bkz. [Apache Oozie 4.0 belgeleri] [ apache-oozie-400] (HDInsight sürüm 3.0 için) veya [Apache Oozie 3.3.2 belgeleri] [ apache-oozie-332] (için HDInsight sürüm 2.1).

Bu bölümdeki PowerShell Betiği aşağıdaki adımları gerçekleştirir:

1. Azure'a bağlanın.
2. Bir Azure kaynak grubu oluşturun. Daha fazla bilgi için [Azure PowerShell kullanarak Azure Resource Manager ile](../powershell-azure-resource-manager.md).
3. Azure SQL veritabanı sunucusu, bir Azure SQL veritabanı ve iki tablo oluşturun. Bunlar, iş akışı Sqoop eylemi tarafından kullanılır.
   
    Tablo adı *log4jLogCount*.
4. Oozie işlerini çalıştırmak için kullanılan bir HDInsight kümesi oluşturun.
   
    Küme incelemek için Azure portal veya Azure PowerShell kullanabilirsiniz.
5. Oozie iş akışı dosyası ve HiveQL komut dosyası için varsayılan dosya sistemi kopyalayın.
   
    Her iki dosyaları ortak bir Blob kapsayıcısında depolanır.
   
   * HiveQL betiğini (useoozie.hql), Azure Depolama'ya (wasb:///tutorials/useoozie/useoozie.hql) kopyalayın.
   * Workflow.XML wasb:///tutorials/useoozie/workflow.xml için kopyalayın.
   * Veri dosyasını kopyalayın (/ example/data/sample.log) wasb:///tutorials/useoozie/data/sample.log için.
6. Oozie işi gönderin.
   
    OOzie iş sonuçlarını incelemek için Azure SQL veritabanı'na bağlanmak için Visual Studio ya da başka Araçlar'ı kullanın.

Komut dosyası aşağıda verilmiştir.  Windows PowerShell ISE'den betiği çalıştırabilirsiniz. Yalnızca ilk 7 değişkenleri yapılandırmanız gerekir.
```powershell
    #region - provide the following values

    $subscriptionID = "<Enter your Azure subscription ID>"

    # SQL Database server login credentials used for creating and connecting
    $sqlDatabaseLogin = "<Enter SQL Database Login Name>"
    $sqlDatabasePassword = "<Enter SQL Database Login Password>"

    # HDInsight cluster HTTP user credential used for creating and connecting
    $httpUserName = "admin"  # The default name is "admin"
    $httpPassword = "<Enter HDInsight Cluster HTTP User Password>"

    # Used for creating Azure service names
    $nameToken = "<Enter an Alias>"
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion

    #region - variables

    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial

    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10

    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "https://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"

    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzContext}
    catch{
        Connect-AzAccount
        Select-AzSubscription -SubscriptionId $subscriptionID
    }
    #endregion

    #region - Create Azure resource group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion

    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $sqlLoginCredentials = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServerName = (New-AzSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $sqlLoginCredentials `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress

        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    #endregion

    #region - Create and validate Azure SQL database
    Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green

    try {
        Get-AzSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        New-AzSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    #endregion

    #region - Create SQL database tables
    Write-Host "Creating the log4jlogs table  ..." -ForegroundColor Green

    $sqlDatabaseTableName = "log4jLogsCount"
    $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
            [Level] [nvarchar](10) NOT NULL,
            [Total] float,
        CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
        (
        [Level] ASC
        )
        )"

    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()

    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jCountTable
    $cmd.ExecuteNonQuery()

    $conn.close()
    #endregion

    #region - Create HDInsight cluster

    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green

    # Create the default storage account
    New-AzStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS

    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 

    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    New-AzHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 

    # Validate the cluster
    Get-AzHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion

    #region - copy Oozie workflow and HiveQL files

    Write-Host "Copy workflow definition and HiveQL script file ..." -ForegroundColor Green

    # Both files are stored in a public Blob
    $publicBlobContext = New-AzStorageContext -StorageAccountName "hditutorialdata" -Anonymous

    # WASB folder for storing the Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use the long path here

    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "useooziewf.hql"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/useooziewf.hql" `
        -Force

    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "workflow.xml"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/workflow.xml" `
        -Force

    #validate the copy
    Get-AzStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/workflow.xml

    Get-AzStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/useooziewf.hql

    #endregion

    #region - copy the sample.log file

    Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green

    Start-CopyAzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -SrcContainer $defaultBlobContainerName `
        -SrcBlob "example/data/sample.log"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -destBlob "$destFolder/data/sample.log" 

    #validate the copy
    Get-AzStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/data/sample.log

    #endregion

    #region - submit Oozie job

    $storageUri="wasb://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net"

    $oozieJobName = $namePrefix + "OozieJob"

    #Oozie WF variables
    $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
    $waitTimeBetweenOozieJobStatusCheck=10

    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"

    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

    <property>
        <name>nameNode</name>
        <value>$storageUrI</value>
    </property>

    <property>
        <name>jobTracker</name>
        <value>jobtrackerhost:9010</value>
    </property>

    <property>
        <name>queueName</name>
        <value>default</value>
    </property>

    <property>
        <name>oozie.use.system.libpath</name>
        <value>true</value>
    </property>

    <property>
        <name>hiveScript</name>
        <value>$hiveScript</value>
    </property>

    <property>
        <name>hiveTableName</name>
        <value>$hiveTableName</value>
    </property>

    <property>
        <name>hiveDataFolder</name>
        <value>$hiveDataFolder</value>
    </property>

    <property>
        <name>hiveOutputFolder</name>
        <value>$hiveOutputFolder</value>
    </property>

    <property>
        <name>sqlDatabaseConnectionString</name>
        <value>&quot;$sqlDatabaseConnectionString&quot;</value>
    </property>

    <property>
        <name>sqlDatabaseTableName</name>
        <value>$SQLDatabaseTableName</value>
    </property>

    <property>
        <name>user.name</name>
        <value>$httpUserName</value>
    </property>

    <property>
        <name>oozie.wf.application.path</name>
        <value>$oozieWFPath</value>
    </property>

    </configuration>
    "@

    Write-Host "Checking Oozie server status..." -ForegroundColor Green
    $clusterUriStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/admin/status"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $httpCredential -OutVariable $OozieServerStatus

    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieServerStatus = $jsonResponse[0].("systemMode")
    Write-Host "Oozie server status is $oozieServerStatus."

    # create Oozie job
    Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
    Write-Host "`n--------`n$OoziePayload`n--------"
    $clusterUriCreateJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/jobs"
    $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $httpCredential -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName #-debug

    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieJobId = $jsonResponse[0].("id")
    Write-Host "Oozie job id is $oozieJobId..."

    # start Oozie job
    Write-Host "Starting the Oozie job $oozieJobId..." -ForegroundColor Green
    $clusterUriStartJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=start"
    $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $httpCredential | Format-Table -HideTableHeaders #-debug

    # get job status
    Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
    Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

    Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
    $clusterUriGetJobStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $JobStatus = $jsonResponse[0].("status")

    while($JobStatus -notmatch "SUCCEEDED|KILLED")
    {
        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")
        $jobStatus
    }

    Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!" -ForegroundColor Green

    #endregion
```

**Öğreticiyi yeniden çalıştırmak için**

İş akışını yeniden çalıştırmak için aşağıdaki öğeleri silmeniz gerekir:

* Hive betik çıkış dosyası
* Log4jLogsCount tablodaki verileri

Aşağıda, kullanabileceğiniz örnek bir PowerShell Betiği verilmiştir:
```powershell
    $resourceGroupName = "<AzureResourceGroupName>"

    $defaultStorageAccountName = "<AzureStorageAccountName>"
    $defaultBlobContainerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServerName = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabasePassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
    $destContext = New-AzStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    Remove-AzStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $defaultBlobContainerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()
```

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir Apache Oozie iş akışının tanımlayın ve PowerShell kullanarak bir Oozie işi çalıştırmak nasıl öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight ile zamana dayalı Apache Oozie düzenleyicisini kullanma][hdinsight-oozie-coordinator-time]
* [Apache Hadoop mobil ahize kullanımını analiz etme Apache Hive, HDInsight kullanmaya başlama][hdinsight-get-started]
* [HDInsight ile Azure Blob Depolama kullanma][hdinsight-storage]
* [HDInsight PowerShell kullanarak yönetme][hdinsight-admin-powershell]
* [HDInsight, Apache Hadoop işleri için veri yükleme][hdinsight-upload-data]
* [HDInsight, Apache Hadoop ile Apache Sqoop'u kullanma][hdinsight-use-sqoop]
* [HDInsight üzerinde Apache Hadoop ile Apache Hive'ı kullanma][hdinsight-use-hive]
* [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma][hdinsight-use-pig]
* [HDInsight için Java MapReduce programları geliştirme][hdinsight-develop-mapreduce]

[hdinsight-cmdlets-download]: https://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/transform-data.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md


[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-develop-mapreduce]:hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md

[sqldatabase-get-started]: ../sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: https://hadoop.apache.org/
[apache-oozie-400]: https://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: https://oozie.apache.org/docs/3.3.2/

[powershell-download]: https://azure.microsoft.com/downloads/
[powershell-about-profiles]: https://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: https://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/library/ee176961.aspx

[cindygross-hive-tables]: https://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: https://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
