---
title: Hdınsight'ta Hadoop Oozie kullanın | Microsoft Docs
description: Hadoop Oozie Hdınsight, büyük veri hizmeti kullanın. Oozie iş akışı tanımlamak ve Oozie işi göndermek öğrenin.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 870098f0-f416-4491-9719-78994bf4a369
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: c0558432c0d74e2c9fcec108182a4dbafa332904
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-in-hdinsight"></a>Oozie Hadoop ile tanımlamak ve Hdınsight'ta bir iş akışını çalıştırmak için kullanın.
[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Apache Oozie bir iş akışı tanımlayın ve üzerinde Hdınsight iş akışını çalıştırmak için nasıl kullanılacağını öğrenin. Oozie Düzenleyicisi hakkında bilgi edinmek için [Hdınsight ile zamana dayalı Oozie Düzenleyicisi Hadoop kullanma][hdinsight-oozie-coordinator-time]. Azure Data Factory öğrenmek için bkz: [kullanım Pig ve Hive Data Factory ile][azure-data-factory-pig-hive].

Apache Oozie, Hadoop işlerini yöneten bir iş akışı/koordinasyon sistemidir. Bu Hadoop yığını ile tümleşiktir ve Apache MapReduce, Apache Pig, Apache Hive ve Apache Sqoop için Hadoop işlerini destekler. Ayrıca, Java programları veya kabuk betikleri gibi sisteme özel işleri planlamak için de kullanılabilir.

Bu öğreticide yönergeleri izleyerek uygulama iş akışı iki eylemleri içerir:

![İş akışı diyagramı][img-workflow-diagram]

1. Hive eylem log4j dosyasını her günlük düzeyi türünün oluşumları saymak için HiveQL betiğini çalıştırır. Her log4j dosyasını türünün ve önem örneğin gösterir [günlük düzeyi] alan içeren bir dizi alanlarının oluşur:
   
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
   
    Hive betiği çıkış benzer:
   
        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4
   
    Hive hakkında daha fazla bilgi için bkz. [HDInsight ile Hive kullanma][hdinsight-use-hive].
2. Sqoop eylem HiveQL çıktı Azure SQL veritabanındaki bir tablo dışa aktarır. Sqoop hakkında daha fazla bilgi için bkz: [Hdınsight ile kullanım Hadoop Sqoop][hdinsight-use-sqoop].

> [!NOTE]
> Hdınsight kümelerinde desteklenen Oozie sürümleri için bkz: [Hdınsight tarafından sağlanan Hadoop küme sürümlerindeki yenilikler nelerdir?] [hdinsight-versions].
> 
> 

### <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdaki öğesi olması gerekir:

* **Azure PowerShell içeren bir iş istasyonu**. 
  

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]
  

## <a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Oozie iş akışı ve ilgili HiveQL betiğini tanımlayın
Oozie iş akışı tanımları hPDL (XML işlem tanım dili) yazılır. Varsayılan iş akışı dosya adı *workflow.xml*. Bu öğreticide kullandığınız iş akışı dosyası verilmiştir.

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

İş akışında tanımlanan iki eylemler vardır. Başlangıç için eylem *RunHiveScript*. Eylem başarıyla çalışırsa, bir sonraki eylem olan *RunSqoopExport*.

RunHiveScript birkaç değişkeni yok. Azure PowerShell kullanarak Oozie iş istasyonunuzdan gönderdiğinizde değerlerini geçirin.

<table border = "1">
<tr><th>İş akışı değişkenleri</th><th>Açıklama</th></tr>
<tr><td>${Jobtracker'a}</td><td>Hadoop işi İzleyicisi URL'sini belirtir. Kullanım <strong>jobtrackerhost:9010</strong> Hdınsight sürüm 3.0 ve 2.1 içinde.</td></tr>
<tr><td>${İş}</td><td>Hadoop adı düğümü URL'sini belirtir. Örneğin, varsayılan dosya sistemi adresi kullanın <i>wasb: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
<tr><td>${queueName}</td><td>İş için gönderildiğinde sıra adı belirtir. Kullanım <strong>varsayılan</strong>.</td></tr>
</table>

<table border = "1">
<tr><th>Eylem değişkeni yığını</th><th>Açıklama</th></tr>
<tr><td>${hiveDataFolder}</td><td>Create Table Hive komutu için kaynak dizini belirtir.</td></tr>
<tr><td>${hiveOutputFolder}</td><td>INSERT üzerine deyimi için çıkış klasörünü belirtir.</td></tr>
<tr><td>${hiveTableName}</td><td>Log4j veri dosyalarına başvuran Hive tablosu adını belirtir.</td></tr>
</table>

<table border = "1">
<tr><th>Sqoop eylem değişkeni</th><th>Açıklama</th></tr>
<tr><td>${sqlDatabaseConnectionString}</td><td>Azure SQL veritabanı bağlantı dizesi belirtir.</td></tr>
<tr><td>${sqlDatabaseTableName}</td><td>Veriler için dışa Azure SQL veritabanı tablosu belirtir.</td></tr>
<tr><td>${hiveOutputFolder}</td><td>Hive Ekle üzerine deyimi için çıkış klasörünü belirtir. Bu Sqoop verme (verme-dir) için aynı klasörüdür.</td></tr>
</table>

Oozie iş akışı ve iş akışı eylemlerinin kullanma hakkında daha fazla bilgi için bkz: [Apache Oozie 4.0 belgelerine] [ apache-oozie-400] (Hdınsight sürüm 3.0 için) veya [Apache Oozie 3.3.2 belgelerine] [ apache-oozie-332] (Hdınsight sürüm 2.1 için).

İş akışı Hive eylemde HiveQL komut dosyasını çağırır. Bu komut dosyasını üç HiveQL ifadelerini içerir:

    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

1. **DROP TABLE deyimi** varsa log4j Hive tablosu siler.
2. **CREATE TABLE deyimi** log4j günlük dosyası konumuna işaret log4j Hive dış tablo oluşturur. Alan sınırlayıcı ",". Varsayılan satır ayırıcı "\n" dir. Hive dış tablo birden çok kez Oozie iş akışını çalıştırmak istiyorsanız, özgün konumundan kaldırılmakta olan veri dosyası önlemek için kullanılır.
3. **INSERT üzerine deyimi** log4j Hive tablosu her günlük düzeyi türünden oluşumlarını sayar ve çıktı Azure storage'da bir blob kaydeder.

Komut dosyasında kullanılan üç değişkenleri şunlardır:

* ${hiveTableName}
* ${hiveDataFolder}
* ${hiveOutputFolder}

İş akışı tanımı dosyası (Bu öğreticide workflow.xml) bu değerleri bu HiveQL betiğini çalışma zamanında geçirir.

İş akışı dosyası ve HiveQL dosya bir blob kapsayıcısında depolanır.  Daha sonra Bu öğreticide kullandığınız PowerShell Betiği hem dosyaları için varsayılan depolama hesabı kopyalar. 

## <a name="submit-oozie-jobs-using-powershell"></a>PowerShell kullanarak Oozie işlerini gönderme
Azure PowerShell cmdlet'lerin Oozie işleri tanımlamak için şu anda sağlamaz. Kullanabileceğiniz **Invoke-RestMethod** cmdlet'ini Oozie web hizmetlerini çağırır. Oozie web hizmetleri API'si bir HTTP REST JSON API'dir. Oozie web hizmetleri API'si hakkında daha fazla bilgi için bkz: [Apache Oozie 4.0 belgelerine] [ apache-oozie-400] (Hdınsight sürüm 3.0 için) veya [Apache Oozie 3.3.2 belgelerine] [ apache-oozie-332] (Hdınsight sürüm 2.1 için).

Bu bölümdeki PowerShell Betiği aşağıdaki adımları gerçekleştirir:

1. Azure'a bağlayın.
2. Bir Azure kaynak grubu oluşturun. Daha fazla bilgi için bkz: [kullanım Azure PowerShell'i Azure Resource Manager ile](../powershell-azure-resource-manager.md).
3. Bir Azure SQL veritabanı sunucusu, Azure SQL veritabanına ve iki tablo oluşturun. Bunlar, Sqoop eylem iş akışı tarafından kullanılır.
   
    Tablo adı *log4jLogCount*.
4. Oozie işlerini çalıştırmak için kullanılan Hdınsight kümesi oluşturun.
   
    Küme incelemek için Azure portalında veya Azure PowerShell'i kullanabilirsiniz.
5. Oozie iş akışı dosyası ve HiveQL komut dosyası için varsayılan dosya sistemi kopyalayın.
   
    Her iki dosyaları bir ortak Blob kapsayıcısında depolanır.
   
   * HiveQL betiğini (useoozie.hql) (wasb:///tutorials/useoozie/useoozie.hql) Azure depolama alanına kopyalayın.
   * Workflow.XML için wasb:///tutorials/useoozie/workflow.xml kopyalayın.
   * Veri dosyasını kopyalayın (/ example/data/sample.log) wasb:///tutorials/useoozie/data/sample.log için.
6. Oozie işi gönderin.
   
    OOzie iş sonuçları incelemek için Azure SQL veritabanına bağlanmak için Visual Studio veya diğer Araçlar'ı kullanın.

Komut dosyası aşağıda verilmiştir.  Windows PowerShell ISE komut dosyasını çalıştırabilirsiniz. Yalnızca ilk 7 değişkenleri yapılandırmanız gerekir.

    #region - provide the following values

    $subscriptionID = "<Enter your Azure subscription ID>"

    # SQL Database server login credentials used for creating and connecting
    $sqlDatabaseLogin = "<Enter SQL Database Login Name>"
    $sqlDatabasePassword = "<Enter SQL Database Login Password>"

    # HDInsight cluster HTTP user credential used for creating and connectin
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
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
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
    try{Get-AzureRmContext}
    catch{
        Connect-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
    }
    #endregion

    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion

    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $sqlLoginCredentials = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $sqlLoginCredentials `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress

        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
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
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        New-AzureRMSqlDatabase `
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
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS

    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 

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
        -DefaultStorageContainer $defaultBlobContainerName 

    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion

    #region - copy Oozie workflow and HiveQL files

    Write-Host "Copy workflow definition and HiveQL script file ..." -ForegroundColor Green

    # Both files are stored in a public Blob
    $publicBlobContext = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous

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
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/workflow.xml

    Get-AzureStorageBlob `
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
    Get-AzureStorageBlob `
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
    $oozieServerSatus = $jsonResponse[0].("systemMode")
    Write-Host "Oozie server status is $oozieServerSatus."

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


**Öğreticiyi yeniden çalıştırmak için**

İş akışını yeniden çalıştırmak için aşağıdaki öğeleri silmeniz gerekir:

* Hive betiği çıkış dosyası
* Log4jLogsCount tablosundaki verileri

Kullanabileceğiniz bir örnek PowerShell komut dosyasını şöyledir:

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
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $defaultBlobContainerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Oozie iş akışı tanımlama ve PowerShell kullanarak bir Oozie işi çalıştırmak öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hdınsight ile zamana dayalı Oozie düzenleyicisi kullanın][hdinsight-oozie-coordinator-time]
* [Hadoop ile hdınsight'ta Hive mobil ahize kullanımını çözümleme için kullanmaya başlama][hdinsight-get-started]
* [Hdınsight ile Azure Blob storage kullanma][hdinsight-storage]
* [PowerShell kullanarak Hdınsight yönetme][hdinsight-admin-powershell]
* [Hdınsight'ta Hadoop işleri için verileri karşıya yükleme][hdinsight-upload-data]
* [Hdınsight'ta Hadoop ile Sqoop kullanma][hdinsight-use-sqoop]
* [Hdınsight'ta Hadoop ile Hive kullanma][hdinsight-use-hive]
* [Hdınsight'ta Hadoop ile pig kullanma][hdinsight-use-pig]
* [Hdınsight için Java MapReduce programlar geliştirmek][hdinsight-develop-mapreduce]

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/transform-data.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-develop-mapreduce]:hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: ../sql-database-create-configure.md
[sqldatabase-get-started]: ../sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
