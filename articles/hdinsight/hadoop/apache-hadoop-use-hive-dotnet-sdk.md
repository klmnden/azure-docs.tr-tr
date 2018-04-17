---
title: Hdınsight .NET SDK - Azure kullanarak Hive sorguları çalıştırma | Microsoft Docs
description: Hdınsight .NET SDK kullanarak Azure Hdınsight Hadoop Hadoop işleri gönderme öğrenin.
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: mumian
ms.assetid: 4e291890-f8b4-426c-b5e8-d4fd512ff042
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: jgao
ms.openlocfilehash: 558ea75a74b89776be32095a7230f9c6e97dcea2
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>Hdınsight .NET SDK kullanarak Hive sorguları çalıştırma
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Hdınsight .NET SDK kullanarak Hive sorguları göndermek öğrenin. Hive tablolarını listeleme bir Hive sorgusu göndermek için C# programı yazma ve sonuçları görüntüleyin.

> [!NOTE]
> Bu makaledeki adımları bir Windows istemcisinden gerçekleştirilmesi gerekir. Hive ile çalışmak için Linux, OS X veya UNIX istemcisi kullanma hakkında daha fazla bilgi için makalenin üst kısmında gösterilen sekme seçicisini kullanın.

## <a name="prerequisites"></a>Önkoşullar
Bu makaleye başlamadan önce aşağıdaki öğeleri sahip olmanız gerekir:

* **Hdınsight'ta Hadoop kümesi**. Bkz: [Hdınsight'ta Linux tabanlı Hadoop ile çalışmaya başlamak](apache-hadoop-linux-tutorial-get-started.md).

    > [!WARNING]
    > 15 Eylül 2017 itibariyle Hdınsight .NET SDK'sı yalnızca Azure depolama hesapları arasından döndürmeyi Hive sorgu sonuçları destekler. Azure Data Lake Store birincil depolama kullanan bir Hdınsight kümesi ile bu örnek kullanırsanız, .NET SDK kullanarak arama sonuçlarını alınamıyor.

* **Visual Studio 2013/2015/2017**.

## <a name="run-a-hive-query"></a>Hive sorgusu çalıştırma
Hdınsight .NET SDK'sı .NET istemci kitaplıkları, .NET Hdınsight kümeleriyle çalışmak kolaylaştırır sağlar. 

**İşlerini göndermek için**

1. Visual Studio'da bir C# konsol uygulaması oluşturun.
2. Nuget Paket Yöneticisi konsolundan aşağıdaki komutu çalıştırın:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. Aşağıdaki kodu kullanın:

    ```csharp
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
   
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
                
                // Only Azure Storage accounts are supported by the SDK
                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitHiveJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "tez" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };
   
                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);
   
                    System.Console.WriteLine("Waiting for the job completion ...");
   
                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }
   
                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure
   
                    System.Console.WriteLine("Job output is: ");
   
                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }
    ```
4. Uygulamayı çalıştırmak için **F5**'e basın.

Uygulamanın çıkışı benzer olacaktır:

![Hdınsight Hadoop Hive iş çıktısı](./media/apache-hadoop-use-hive-dotnet-sdk/hdinsight-hadoop-use-hive-net-sdk-output.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Hdınsight kümesi oluşturmanın birkaç yolu öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Hdınsight kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md)
* [Hdınsight'ta Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md)
* [Azure portalını kullanarak hdınsight'ta Hadoop kümelerini yönetme](../hdinsight-administer-use-management-portal.md)
* [Hdınsight .NET SDK'sı başvurusu](https://msdn.microsoft.com/library/mt271028.aspx)
* [HDInsight ile Pig kullanma](hdinsight-use-pig.md)
* [Hdınsight ile Sqoop kullanma](apache-hadoop-use-sqoop-mac-linux.md)
* [Etkileşimli olmayan kimlik doğrulaması .NET HDInsight uygulamaları oluşturma](../hdinsight-create-non-interactive-authentication-dotnet-applications.md)
 


