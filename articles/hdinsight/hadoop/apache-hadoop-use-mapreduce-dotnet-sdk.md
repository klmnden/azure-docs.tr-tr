---
title: HDInsight .NET SDK - Azure kullanarak MapReduce işlerini gönderme
description: HDInsight .NET SDK kullanarak Azure HDInsight Apache hadoop MapReduce işlerini gönderme hakkında bilgi edinin.
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 1ac2dda20ba1219c9f62e834b5cd2cfba8a50086
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62124704"
---
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>HDInsight .NET SDK kullanarak MapReduce işleri çalıştırma
[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

HDInsight .NET SDK kullanarak MapReduce işleri göndermeyi öğrenin. HDInsight kümeleri bazı MapReduce örnekleri içeren bir jar dosyası ile birlikte geldiğinden. Jar dosyası */example/jars/hadoop-mapreduce-examples.jar*.  Örneklerden biridir *wordcount*. Wordcount işi göndermek için bir C# konsol uygulaması geliştirme.  İş okur */example/data/gutenberg/davinci.txt* dosya ve sonuçları çıkarır */example/data/davinciwordcount*.  Uygulamayı yeniden çalıştırmak isterseniz, çıktı klasörü temizlemek gerekir.

> [!NOTE]  
> Bu makaledeki adımlarda, bir Windows istemcisinden gerçekleştirilmelidir. Hive ile çalışmak için Linux, OS X veya UNIX istemcisi kullanma hakkında daha fazla bilgi için gösterilen makalenin üst kısmındaki sekme seçicisini kullanın.
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bu makaleye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Bir HDInsight Hadoop kümesinde**. Bkz: [Linux tabanlı Apache Hadoop HDInsight kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md).
* **Visual Studio 2013/2015/2017**.

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>HDInsight .NET SDK kullanarak MapReduce işlerini gönderme
HDInsight .NET SDK'sı .NET istemci kitaplıkları, .NET HDInsight kümeleriyle çalışmak daha kolay hale getiren sağlar. 

**İşleri göndermek için**

1. Visual Studio'da C# konsol uygulaması oluşturun.
2. NuGet Paket Yöneticisi konsolundan aşağıdaki komutu çalıştırın:

    ```   
    Install-Package Microsoft.Azure.Management.HDInsight.Job
    ```
3. Aşağıdaki kodu kullanın:

    ```csharp
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
    ```

4. Uygulamayı çalıştırmak için **F5**'e basın.

İşi yeniden çalıştırmak için iş çıktısı klasörü adı değiştirme olduğu aşağıdaki örnekte, "/ data/örnek/davinciwordcount".

İş başarıyla tamamlandığında uygulamanın "bölümü-r-00000" çıkış dosyasının içeriğini yazdırır.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir HDInsight kümesi oluşturmanın birkaç yolu öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* Bir Hive işi göndermek için bkz: [HDInsight .NET SDK kullanarak Apache Hive sorgularını çalıştırma](apache-hadoop-use-hive-dotnet-sdk.md).
* HDInsight kümeleri oluşturmak için bkz: [oluşturma Linux tabanlı Apache Hadoop HDInsight kümeleri](../hdinsight-hadoop-provision-linux-clusters.md).
* HDInsight kümelerini yönetmek için bkz: [yönetme Apache Hadoop HDInsight kümeleri](../hdinsight-administer-use-portal-linux.md).
* HDInsight .NET SDK'sı öğrenmek için bkz: [HDInsight .NET SDK başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight).
* İçin etkileşimli olmayan Azure'a kimliğini doğrulamak için bkz: [etkileşimli olmayan kimlik doğrulaması .NET HDInsight uygulamaları oluşturma](../hdinsight-create-non-interactive-authentication-dotnet-applications.md).

