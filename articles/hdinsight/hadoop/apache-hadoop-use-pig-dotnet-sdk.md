---
title: "Apache Pig işleri Hadoop - Azure Hdınsight .NET SDK'sı ile çalıştırma | Microsoft Docs"
description: "Hadoop için .NET SDK'sı hdınsight'ta Hadoop Pig iş göndermek için nasıl kullanılacağını öğrenin."
services: hdinsight
documentationcenter: .net
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: fa11d49a-328c-47e7-b16d-e7ed2a453195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/08/2017
ms.author: larryfr
ms.openlocfilehash: c828a7b63e70669ed38ecea898442a3978e67ba7
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Hdınsight'ta Hadoop için .NET SDK kullanarak Pig işleri çalıştırma

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Hadoop için .NET SDK'sı Azure hdınsight'ta Hadoop Apache Pig iş göndermek için nasıl kullanılacağını öğrenin.

Hdınsight .NET SDK'sı .NET gelen Hdınsight kümeleri ile çalışmayı kolaylaştırır .NET istemci kitaplıkları sağlar. Pig, bir dizi Veri Dönüşümleri modelleme tarafından MapReduce işlemleri oluşturmanıza olanak sağlar. Bu belgede, temel bir C# uygulaması Hdınsight kümesi için Pig işi göndermek için nasıl kullanılacağını öğrenin.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamak için aşağıdakiler gerekir.

* Azure Hdınsight (Hadoop hdınsight) kümesi (ya da Windows veya Linux tabanlı).

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Visual Studio 2012 2013, 2015 veya 2017.

## <a name="create-the-application"></a>Uygulama oluşturma

Hdınsight .NET SDK'sı .NET istemci kitaplıkları, .NET Hdınsight kümeleriyle çalışmak kolaylaştırır sağlar.

1. Gelen **dosya** menü Visual Studio'da seçin **yeni** ve ardından **proje**.

2. Yeni proje için yazın veya aşağıdaki değerleri seçin:

   | Özellik | Değer |
   | ------ | ------ |
   | Kategori | Şablonlar/Visual C#/Windows |
   | Şablon | Konsol Uygulaması |
   | Ad | SubmitPigJob |

3. Projeyi oluşturmak için **Tamam**'a tıklayın.

4. Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi** veya **NuGet Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.

5. .NET SDK'sı paketleri yüklemek için aşağıdaki komutu kullanın:

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. Çözüm Gezgini'nde, çift **Program.cs** açın. Var olan kodu aşağıdakilerle değiştirin.

    ```csharp
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

            static void Main(string[] args)
            {
                System.Console.WriteLine("The application is running ...");

                var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                SubmitPigJob();

                System.Console.WriteLine("Press ENTER to continue ...");
                System.Console.ReadLine();
            }

            private static void SubmitPigJob()
            {
                var parameters = new PigJobSubmissionParameters
                {
                    Query = @"LOGS = LOAD '/example/data/sample.log';
                                LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                RESULT = order FREQUENCIES by COUNT desc;
                                DUMP RESULT;"
                };

                System.Console.WriteLine("Submitting the Pig job to the cluster...");
                var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                System.Console.WriteLine("Validating that the response is as expected...");
                System.Console.WriteLine("Response status code is " + response.StatusCode);
                System.Console.WriteLine("Validating the response object...");
                System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
            }
        }
    }
    ```

7. Uygulamayı başlatmak için basın **F5**.

8. Uygulamadan çıkmak için basın **ENTER**.

## <a name="next-steps"></a>Sonraki adımlar

Hdınsight'ta Pig hakkında daha fazla bilgi için bkz: [hdınsight'ta Hadoop ile Pig kullanma](hdinsight-use-pig.md).

Hdınsight'ta Hadoop kullanma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
