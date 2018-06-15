---
title: .NET ve Hdınsight - Azure kullanarak Sqoop işleri çalıştırma | Microsoft Docs
description: Hdınsight .NET SDK'sı Sqoop alma çalıştırın ve Hadoop kümesi ve bir Azure SQL veritabanı arasında dışa aktarmak için nasıl kullanılacağını öğrenin.
keywords: sqoop işi
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: mumian
ms.assetid: 87bacd13-7775-4b71-91da-161cb6224a96
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jgao
ms.openlocfilehash: 818e4aca63249c7a1543abe146e0691e993e9e80
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34200308"
---
# <a name="run-sqoop-jobs-by-using-net-sdk-for-hadoop-in-hdinsight"></a>Hdınsight'ta Hadoop için .NET SDK kullanarak Sqoop işleri çalıştırma
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

İçeri ve dışarı aktarma Hdınsight kümesi ve bir Azure SQL veritabanı veya SQL Server veritabanı arasında hdınsight'ta Sqoop işlerini çalıştırmak için Azure Hdınsight .NET SDK'sını kullanmayı öğrenin.

> [!NOTE]
> Bu makaledeki yordamları bir Windows veya Linux tabanlı Hdınsight kümesi ile ya da kullanabilmenize karşın, yalnızca bir Windows istemcisinden çalışırlar. Diğer yöntemleri seçmek için bu makalenin üst kısmındaki sekme seçicisini kullanın.
> 

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdaki öğesi olması gerekir:

* Hdınsight'ta Hadoop kümesi. Daha fazla bilgi için bkz: [bir küme oluşturup bir SQL veritabanı](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="use-sqoop-on-hdinsight-clusters-with-the-net-sdk"></a>Hdınsight kümelerinde .NET SDK'sı ile Sqoop kullanma
Hdınsight .NET SDK'sı .NET istemci kitaplıkları sunar, böylece .NET Hdınsight kümeleriyle çalışmak daha kolaydır. Bu bölümde, bu öğreticide daha önce oluşturduğunuz Azure SQL veritabanı tablosu hivesampletable verilecek bir C# konsol uygulaması oluşturun.

## <a name="submit-a-sqoop-job"></a>Sqoop işi gönderin

1. Visual Studio'da bir C# konsol uygulaması oluşturun.

2. Visual Studio Paket Yöneticisi Konsolu'ndan, aşağıdaki NuGet komutu çalıştırarak paketini içeri aktarın:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job

3. Aşağıdaki kodu Program.cs dosyasında kullanın:
   
        using System.Collections.Generic;
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
   
                    SubmitSqoopJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
   
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
   
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
   
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
   
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }

4. Programı çalıştırmak için seçin **F5** anahtarı. 

## <a name="limitations"></a>Sınırlamalar
Linux tabanlı Hdınsight aşağıdaki sınırlamalar sunar:

* Toplu dışa aktarma: Microsoft SQL Server veya Azure SQL veritabanı için verileri dışa aktarmak için kullanılan Sqoop bağlayıcı toplu eklemeler şu anda desteklemiyor.

* Toplu işleme: kullanarak `-batch` ne zaman geçiş eklemeleri gerçekleştirir, Sqoop gerçekleştirir INSERT işlemlerine toplu işleme yerine birden çok ekler.

## <a name="next-steps"></a>Sonraki adımlar
Şimdi Sqoop kullanma öğrendiniz. Daha fazla bilgi için bkz:

* [Hdınsight ile Oozie kullanma](../hdinsight-use-oozie.md): Oozie iş akışında kullanmak Sqoop eylem.
* [Hdınsight kullanarak uçuş gecikme verileri analiz](../hdinsight-analyze-flight-delay-data.md): uçuş çözümlemek için kullanmak Hive gecikme veri ve bir Azure SQL veritabanına veri vermek için Sqoop kullanın.
* [Verileri Hdınsight'a yükleme](../hdinsight-upload-data.md): Hdınsight veya Azure Blob depolama alanına veri yüklemek için diğer yöntemler bulun.

