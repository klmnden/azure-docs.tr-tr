---
title: "PowerShell ve Azure Hdınsight kullanarak Sqoop işleri çalıştırma | Microsoft Docs"
description: "Azure PowerShell bir iş istasyonundan Sqoop alma çalıştırın ve Hadoop kümesi ve bir Azure SQL veritabanı arasında dışa aktarmak için nasıl kullanılacağını öğrenin."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: bbb6f53a-e019-4d01-92bd-92c208c760b6
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2017
ms.author: jgao
ms.openlocfilehash: 1eaf55ea8e46598e261be43c646b5dbd48ac730b
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="run-sqoop-jobs-by-using-azure-powershell-for-hadoop-in-hdinsight"></a>Hdınsight'ta Hadoop için Azure PowerShell kullanarak Sqoop işleri çalıştırma
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Hdınsight kümesi ve bir Azure SQL veritabanı veya SQL Server veritabanı arasında vermek ve almak için Azure hdınsight'ta Sqoop işlerini çalıştırmak için Azure PowerShell kullanmayı öğrenin.

> [!NOTE]
> Bu makaledeki yordamları bir Windows veya Linux tabanlı Hdınsight kümesi ile ya da kullanabilmenize karşın, yalnızca bir Windows istemcisinden çalışırlar. Diğer yöntemleri seçmek için bu makalenin üst kısmındaki sekme seçicisini kullanın. 
> 
> 

### <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* Bir iş istasyonu Azure PowerShell ile.
* Hdınsight'ta Hadoop kümesi. Daha fazla bilgi için bkz: [küme ve SQL veritabanı oluşturma](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="run-sqoop-by-using-powershell"></a>PowerShell kullanarak Sqoop çalıştırın
Aşağıdaki PowerShell betiğini kaynak dosyasını önceden işler ve bunu bir Azure SQL veritabanına aktarır:

    $resourceGroupName = "<AzureResourceGroupName>"
    $hdinsightClusterName = "<HDInsightClusterName>"

    $httpUserName = "admin"
    $httpPassword = "<Password>"

    $defaultStorageAccountName = $hdinsightClusterName + "store"
    $defaultBlobContainerName = $hdinsightClusterName


    $sqlDatabaseServerName = $hdinsightClusterName + "dbserver"
    $sqlDatabaseName = $hdinsightClusterName + "db"
    $sqlDatabaseLogin = "sqluser"
    $sqlDatabasePassword = "<Password>"

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - pre-process the source file

    Write-Host "`nPreprocessing the source file ..." -ForegroundColor Green

    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"

    # Define the connection string
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    # Create block blob objects referencing the source and destination blob.
    $storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $defaultStorageAccountName
    $storageContainer = ($storageAccount |Get-AzureStorageContainer -Name $defaultBlobContainerName).CloudBlobContainer
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)

    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)

    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")

        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)

            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }

        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }

    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    #endregion

    #region - export the log file from the cluster to the SQL database

    Write-Host "Exporting the log file ..." -ForegroundColor Green

    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"

    $tableName_log4j = "log4jlogs"
    $exportDir_log4j = "/tutorials/usesqoop/data"
    $sqljdbcdriver = "/user/oozie/share/lib/sqoop/sqljdbc41.jar"

    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1" `
        -Files $sqljdbcdriver

    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    #endregion

## <a name="limitations"></a>Sınırlamalar
Linux tabanlı Hdınsight aşağıdaki sınırlamalar sunar:

* Toplu dışa aktarma: Microsoft SQL Server veya Azure SQL veritabanı için verileri dışa aktarmak için kullanılan Sqoop bağlayıcı toplu eklemeler şu anda desteklemiyor.

* Toplu işleme: kullanarak `-batch` ne zaman geçiş eklemeleri gerçekleştirir, Sqoop gerçekleştirir INSERT işlemlerine toplu işleme yerine birden çok ekler. 

## <a name="next-steps"></a>Sonraki adımlar
Şimdi Sqoop kullanma öğrendiniz. Daha fazla bilgi için bkz:

* [Hdınsight ile Oozie kullanma](../hdinsight-use-oozie.md): Oozie iş akışında kullanmak Sqoop eylem.
* [Hdınsight kullanarak uçuş gecikme verilerini çözümlemek](../hdinsight-analyze-flight-delay-data.md): uçuş çözümlemek için kullanmak Hive gecikme veri ve bir Azure SQL veritabanına veri vermek için Sqoop kullanın.
* [Verileri Hdınsight'a yükleme](../hdinsight-upload-data.md): Hdınsight veya Azure Blob depolama alanına veri yüklemek için diğer yöntemler bulun.

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
