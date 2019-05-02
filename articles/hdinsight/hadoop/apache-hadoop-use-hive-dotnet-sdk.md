---
title: HDInsight .NET SDK - Azure kullanarak Apache Hive sorguları çalıştırma
description: HDInsight .NET SDK kullanarak Azure HDInsight Apache hadoop, Apache Hadoop işlerini gönderme hakkında bilgi edinin.
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 947e797842000e4da2f9e22077bc32c24d6c6a74
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64718846"
---
# <a name="run-apache-hive-queries-using-hdinsight-net-sdk"></a>HDInsight .NET SDK kullanarak Apache Hive sorguları çalıştırma
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

HDInsight .NET SDK kullanarak Apache Hive sorguları göndermek öğrenin. Hive tablolarını listelemek için bir Hive sorgusu göndermek için bir C# programı yazma ve sonuçları görüntüler.

> [!NOTE]  
> Bu makaledeki adımlarda, bir Windows istemcisinden gerçekleştirilmelidir. Hive ile çalışmak için Linux, OS X veya UNIX istemcisi kullanma hakkında daha fazla bilgi için gösterilen makalenin üst kısmındaki sekme seçicisini kullanın.

## <a name="prerequisites"></a>Önkoşullar
Bu makaleye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **HDInsight, Apache Hadoop kümesi**. Bkz: [HDInsight içinde Linux tabanlı Hadoop kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md).

    > [!WARNING]  
    > 15 Eylül 2017'den itibaren HDInsight .NET SDK'sı, Azure depolama hesaplarından döndüren Hive sorgu sonuçları yalnızca destekler. Bu örnekte, birincil depolama olarak Azure Data Lake depolama kullanan bir HDInsight kümesi ile kullanırsanız, .NET SDK kullanarak arama sonuçlarını alınamıyor.

* **Visual Studio 2013/2015/2017**.

## <a name="run-a-hive-query"></a>Bir Hive sorgusu çalıştırma
HDInsight .NET SDK'sı .NET istemci kitaplıkları, .NET HDInsight kümeleriyle çalışmak daha kolay hale getiren sağlar. 

**İşleri göndermek için**

1. Visual Studio'da C# konsol uygulaması oluşturun.
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

Uygulama çıktısı benzer olacaktır:

![HDInsight Hadoop Hive iş çıktısı](./media/apache-hadoop-use-hive-dotnet-sdk/hdinsight-hadoop-use-hive-net-sdk-output.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir HDInsight kümesi oluşturmanın birkaç yolu öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure HDInsight ile çalışmaya başlama](apache-hadoop-linux-tutorial-get-started.md)
* [HDInsight Apache Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md)
* [HDInsight .NET SDK başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight)
* [Apache Pig, HDInsight ile kullanma](hdinsight-use-pig.md)
* [HDInsight ile Apache Sqoop'u kullanma](apache-hadoop-use-sqoop-mac-linux.md)
* [Etkileşimli olmayan kimlik doğrulaması .NET HDInsight uygulamaları oluşturma](../hdinsight-create-non-interactive-authentication-dotnet-applications.md)
 


