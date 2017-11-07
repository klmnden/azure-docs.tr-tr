---
title: "Hdınsight .NET SDK - Azure kullanarak MapReduce işleri gönderme | Microsoft Docs"
description: "Hdınsight .NET SDK kullanarak Azure Hdınsight Hadoop MapReduce işleri gönderme öğrenin."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: c85e44b0-85fd-4185-ad1c-c34a9fe5ef44
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/06/2017
ms.author: jgao
ms.openlocfilehash: 3028edde4c2c4f3bc268ad9103e3a600379ec295
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>Hdınsight .NET SDK kullanarak MapReduce işleri çalıştırma
[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Hdınsight .NET SDK kullanarak MapReduce işleri gönderme öğrenin. Hdınsight kümeleri gelen jar dosyasını bazı MapReduce örnekleri ile. Jar dosyası */example/jars/hadoop-mapreduce-examples.jar*.  Örnekler, biri *wordcount*. Wordcount işi göndermek için bir C# konsol uygulaması geliştirin.  İş okur */example/data/gutenberg/davinci.txt* dosya ve sonuçları çıkaran */example/data/davinciwordcount*.  Uygulama yeniden çalıştırmak istiyorsanız, çıkış klasörü Temizle gerekir.

> [!NOTE]
> Bu makaledeki adımları bir Windows istemcisinden gerçekleştirilmesi gerekir. Hive ile çalışmak için Linux, OS X veya UNIX istemcisi kullanma hakkında daha fazla bilgi için makalenin üst kısmında gösterilen sekme seçicisini kullanın.
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Bu makaleye başlamadan önce aşağıdaki öğeleri sahip olmanız gerekir:

* **Hdınsight'ta Hadoop kümesi**. Bkz: [Hdınsight'ta Linux tabanlı Hadoop ile çalışmaya başlamak](apache-hadoop-linux-tutorial-get-started.md).
* **Visual Studio 2013/2015/2017**.

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>Hdınsight .NET SDK kullanarak MapReduce işleri gönderme
Hdınsight .NET SDK'sı .NET istemci kitaplıkları, .NET Hdınsight kümeleriyle çalışmak kolaylaştırır sağlar. 

**İşlerini göndermek için**

1. Visual Studio'da bir C# konsol uygulaması oluşturun.
2. Nuget Paket Yöneticisi konsolundan aşağıdaki komutu çalıştırın:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. Aşağıdaki kodu kullanın:
   
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string existingClusterName = "<Your HDInsight Cluster Name>";
                private const string existingClusterUri = existingClusterName + ".azurehdinsight.net";
                private const string existingClusterUsername = "<Cluster Username>";
                private const string existingClusterPassword = "<Cluster User Password>";
   
                private const string defaultStorageAccountName = "<Default Storage Account Name>"; //<StorageAccountName>.blob.core.windows.net
                private const string defaultStorageAccountKey = "<Default Storage Account Key>";
                private const string defaultStorageContainerName = "<Default Blob Container Name>";

                private const string sourceFile = "/example/data/gutenberg/davinci.txt";  
                private const string outputFolder = "/example/data/davinciwordcount";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = existingClusterUsername, Password = existingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(existingClusterUri, clusterCredentials);
   
                    SubmitMRJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitMRJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };
   
                    var paras = new MapReduceJobSubmissionParameters
                    {
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };
   
                    System.Console.WriteLine("Submitting the MR job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitMapReduceJob(paras);
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
                    System.Console.WriteLine("Job output is: ");
                    var storageAccess = new AzureStorageAccess(defaultStorageAccountName, defaultStorageAccountKey,
                        defaultStorageContainerName);
        
                    if (jobDetail.ExitValue == 0)
                    {
                        // Create the storage account object
                        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=" + 
                            defaultStorageAccountName + 
                            ";AccountKey=" + defaultStorageAccountKey);
        
                        // Create the blob client.
                        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
                        // Retrieve reference to a previously created container.
                        CloudBlobContainer container = blobClient.GetContainerReference(defaultStorageContainerName);
        
                        CloudBlockBlob blockBlob = container.GetBlockBlobReference(outputFolder.Substring(1) + "/part-r-00000");
        
                        using (var stream = blockBlob.OpenRead())
                        {
                            using (StreamReader reader = new StreamReader(stream))
                            {
                                while (!reader.EndOfStream)
                                {
                                    System.Console.WriteLine(reader.ReadLine());
                                }
                            }
                        }
                    }
                    else
                    {
                        // fetch stderr output in case of failure
                        var output = _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); 
        
                        using (var reader = new StreamReader(output, Encoding.UTF8))
                        {
                            string value = reader.ReadToEnd();
                            System.Console.WriteLine(value);
                        }
        
                    }
                }
            }
        }
4. Uygulamayı çalıştırmak için **F5**'e basın.

İşi yeniden çalıştırmak için proje çıktı klasör adını değiştirmek aşağıdaki örnekte olduğu "/ data/örnek/davinciwordcount".

İş başarıyla tamamlandığında, uygulama "bölümü-r-00000" çıkış dosyasının içeriği yazdırır.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Hdınsight kümesi oluşturmanın birkaç yolu öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* Hive işi göndermek için bkz: [Hdınsight .NET SDK kullanarak Hive sorgularını çalıştırma](apache-hadoop-use-hive-dotnet-sdk.md).
* Hdınsight kümeleri oluşturmak için bkz: [Hdınsight oluşturma Linux tabanlı Hadoop kümeleri](../hdinsight-hadoop-provision-linux-clusters.md).
* Hdınsight kümeleri için bkz. [Hdınsight'ta Hadoop yönetmek kümeleri](../hdinsight-administer-use-portal-linux.md).
* Hdınsight .NET SDK'sı öğrenmek için bkz: [Hdınsight .NET SDK'sı başvurusu](https://msdn.microsoft.com/library/mt271028.aspx).
* İçin etkileşimli olmayan için Azure kimlik doğrulaması yapmak için bkz: [etkileşimli olmayan kimlik doğrulama .NET Hdınsight uygulamaları oluşturma](../hdinsight-create-non-interactive-authentication-dotnet-applications.md).

