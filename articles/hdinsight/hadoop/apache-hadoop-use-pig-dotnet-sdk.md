---
title: Apache Pig işleri - Azure HDInsight Hadoop için .NET SDK'sı ile çalıştırın.
description: HDInsight üzerinde Hadoop için Pig işleri göndermek için Hadoop için .NET SDK'sını kullanmayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: hrasheed
ms.openlocfilehash: ebf1f2806a6606294c61860a24fb2f02033a4bf4
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62110965"
---
# <a name="run-apache-pig-jobs-using-the-net-sdk-for-apache-hadoop-in-hdinsight"></a>HDInsight, Apache Hadoop için .NET SDK kullanarak Apache Pig işleri çalıştırma

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Hadoop Azure HDInsight üzerinde Apache Pig işleri göndermek için Apache Hadoop için .NET SDK'sını kullanmayı öğrenin.

HDInsight .NET SDK'sı, .NET HDInsight kümeleriyle çalışmayı kolaylaştırır .NET istemci kitaplıkları sağlar. Pig, MapReduce işlemleri bir dizi Veri Dönüşümleri modelleme tarafından oluşturmanıza olanak sağlar. Bu belgede, HDInsight kümesine bir Pig işi göndermek için basit bir C# uygulaması kullanmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için aşağıdakiler gerekir.

* Bir Azure HDInsight (Hadoop HDInsight üzerinde) kümesi (ya da Windows veya Linux tabanlı).

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Visual Studio 2012, 2013, 2015 veya 2017.

## <a name="create-the-application"></a>Uygulama oluşturma

HDInsight .NET SDK'sı .NET istemci kitaplıkları, .NET HDInsight kümeleriyle çalışmak daha kolay hale getiren sağlar.

1. Gelen **dosya** Visual Studio'da seçim menüsünde **yeni** seçip **proje**.

2. Yeni proje için yazın veya aşağıdaki değerleri seçin:

   | Özellik | Değer |
   | ------ | ------ |
   | Kategori | Şablonlar/Visual C#/Windows |
   | Şablon | Konsol Uygulaması |
   | Ad | SubmitPigJob |

3. Projeyi oluşturmak için **Tamam**'a tıklayın.

4. Gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi** veya **NuGet Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.

5. .NET SDK paketleri yüklemek için aşağıdaki komutu kullanın:

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. Çözüm Gezgini'nden çift **Program.cs** açın. Varolan kodu aşağıdakiyle değiştirin.

    ```csharp
    using Microsoft.Azure.Management.HDInsight.Job;
    using Microsoft.Azure.Management.HDInsight.Job.Models;
    using Hyak.Common;

    namespace SubmitPigJob
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

Pig, HDInsight hakkında daha fazla bilgi için bkz: [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma](hdinsight-use-pig.md).

HDInsight üzerinde Hadoop kullanmaya ilişkin daha fazla bilgi için aşağıdaki belgelere bakın:

* [HDInsight üzerinde Apache Hadoop ile Apache Hive'ı kullanma](hdinsight-use-hive.md)
* [HDInsight üzerinde Apache Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
