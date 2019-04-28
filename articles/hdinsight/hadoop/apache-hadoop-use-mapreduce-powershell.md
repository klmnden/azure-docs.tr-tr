---
title: Apache Hadoop - Azure HDInsight ile MapReduce ve PowerShell kullanma
description: PowerShell uzaktan Apache Hadoop ile MapReduce işleri HDInsight üzerinde çalıştırmak için kullanmayı öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21b56d32-1785-4d44-8ae8-94467c12cfba
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 05/09/2018
ms.date: 04/15/2019
ms.author: v-yiso
ms.openlocfilehash: 29e23d5919a953566c803f2b7825a75a2993723c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62129034"
---
# <a name="run-mapreduce-jobs-with-apache-hadoop-on-hdinsight-using-powershell"></a>MapReduce işleri PowerShell kullanarak HDInsight üzerinde Apache Hadoop ile çalıştırın.

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]



Bu belgede bir MapReduce işi içinde bir Hadoop HDInsight kümesinde çalıştırmak için Azure PowerShell kullanarak bir örnek sağlar.

## <a id="prereq"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

* **Bir Azure HDInsight (Hadoop HDInsight üzerinde) kümesi**

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Azure PowerShell içeren bir iş istasyonu**.

## <a id="powershell"></a>Bir MapReduce işi çalıştırın

Azure PowerShell sağlar *cmdlet'leri* uzaktan üzerinde HDInsight MapReduce işleri çalıştırmanıza izin verir. Dahili olarak, PowerShell, REST çağrıları yapan [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (eski adıyla templeton da denir) HDInsight kümesinde çalışıyor.

MapReduce işleri çalıştıran bir uzak HDInsight kümesinde aşağıdaki cmdlet'ler kullanılır.

* **Connect AzAccount**: Azure PowerShell, Azure aboneliğinize kimliğini doğrular.

* **Yeni AzHDInsightMapReduceJobDefinition**: Yeni bir oluşturur *iş tanımı* belirtilen MapReduce bilgileri kullanarak.

* **Başlangıç AzHDInsightJob**: HDInsight için iş tanımını gönderir ve bir iş başlatılır. A *iş* nesne döndürülür.

* **Bekleme AzHDInsightJob**: İş nesnesi, iş durumunu denetlemek için kullanır. Bekleme süresi aşılırsa veya iş tamamlanana kadar bekler.

* **Get-AzHDInsightJobOutput**: İşin çıktısını almak için kullanılır.

Aşağıdaki adımlarda HDInsight kümenizdeki bir işi çalıştırmak için bu cmdlet'leri kullanma gösterilmektedir.

1. Bir Düzenleyicisi'ni kullanarak aşağıdaki kodun da Kaydet **mapreducejob.ps1**.

    ```powershell
    # Login to your Azure subscription
    # Is there an active Azure subscription?
    $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
    if(-not($sub))
    {
        Add-AzureRmAccount -EnvironmentName AzureChinaCloud
    }

    # Get cluster info
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    $creds=Get-Credential -Message "Enter the login for the cluster"

    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    #NOTE: This assumes that the storage account is in the same resource
    #      group as the cluster. If it is not, change the
    #      --ResourceGroupName parameter to the group that contains storage.
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
    -ResourceGroupName $resourceGroup)[0].Value

    #Create a storage context
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey

    #Define the MapReduce job
    #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
    # -JarFile = the JAR containing the MapReduce application
    # -ClassName = the class of the application
    # -Arguments = The input file, and the output directory
    $wordCountJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
        -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
        -ClassName "wordcount" `
        -Arguments `
            "/example/data/gutenberg/davinci.txt", `
            "/example/data/WordCountOutput"

    #Submit the job to the cluster
    Write-Host "Start the MapReduce job..." -ForegroundColor Green
    $wordCountJob = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $wordCountJobDefinition `
        -HttpCredential $creds

    #Wait for the job to complete
    Write-Host "Wait for the job to complete..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
        -Blob 'example/data/WordCountOutput/part-r-00000' `
        -Container $container `
        -Destination output.txt `
        -Context $context
    # Print the output of the job.
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds
    ```

2. Yeni bir **Azure PowerShell** komut istemi. Dizinleri konumuna **mapreducejob.ps1** dosya ve betiği çalıştırmak için aşağıdaki komutu kullanın:

        .\mapreducejob.ps1

    Betiği çalıştırmak için HDInsight kümesi ve küme oturum açma adı sorulur. Ayrıca, Azure aboneliğinizin kimlik doğrulaması istenebilir.

3. İş tamamlandığında aşağıdaki metne benzer bir çıktı alırsınız:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Bu çıkış, işin başarıyla tamamlandığını gösterir.

    > [!NOTE]
    > Varsa **ExitCode** değer 0'dan bkz [sorun giderme](#troubleshooting).

    Bu örnek ayrıca indirilen dosyaları depolayan bir **çýktý.txt** betiğini çalıştırdığınız dizindeki dosya.

### <a name="view-output"></a>Görünüm çıkış

Sözcükleri ve sayıları iş tarafından üretilen görmek için **çýktý.txt** dosyasını bir metin düzenleyicisinde.

> [!NOTE]
> Bir MapReduce işi'nın çıktı dosyalarını sabittir. Bu nedenle, bu örnek yeniden, çıkış dosyasının adını değiştirmek gerekir.

## <a id="troubleshooting"></a>Sorun giderme

İş tamamlandığında hiçbir bilgi döndürülürse, iş hataları görüntüleyin. Bu işi hata bilgilerini görüntülemek için aşağıdaki komutu sonuna ekleyin **mapreducejob.ps1** dosyasını kaydedin ve yeniden çalıştırın.

```powershell
# Print the output of the WordCount job.
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Bu cmdlet iş çalışırken STDERR yazılmıştır bilgileri döndürür.

## <a id="summary"></a>Özet

Gördüğünüz gibi Azure PowerShell, bir HDInsight kümesinde MapReduce işleri çalıştırma, iş durumunu izlemek ve çıktısını almak için kolay bir yol sağlar.

## <a id="nextsteps"></a>Sonraki adımlar

HDInsight MapReduce işleri hakkında genel bilgi için:

* [HDInsight üzerinde Hadoop MapReduce kullanma](hdinsight-use-mapreduce.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Apache Hadoop ile Apache Hive'ı kullanma](hdinsight-use-hive.md)
* [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma](hdinsight-use-pig.md)
