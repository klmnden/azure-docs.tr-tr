---
title: .NET ve HDInsight - Azure'ı kullanarak Apache Sqoop işleri çalıştırma
description: Apache Sqoop alma ve bir Apache Hadoop kümesi ile bir Azure SQL veritabanı arasında için HDInsight .NET SDK'sını kullanmayı öğrenin.
keywords: sqoop işi
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: ac0890be0abccb316bffc4d9bdd6868a80173e18
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64710124"
---
# <a name="run-apache-sqoop-jobs-by-using-net-sdk-for-apache-hadoop-in-hdinsight"></a>HDInsight, Apache Hadoop için .NET SDK kullanarak Apache Sqoop işleri çalıştırma
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

İçeri aktarma ve bir HDInsight kümesi ve bir Azure SQL veritabanı veya SQL Server veritabanı arasında dışa aktarmak için HDInsight, Apache Sqoop işlerini çalıştırmak için Azure HDInsight .NET SDK'sını kullanmayı öğrenin.

> [!NOTE]
> Bu makalede yer alan yordamları bir Windows tabanlı veya Linux tabanlı HDInsight kümesi ile birlikte kullanabilirsiniz, ancak yalnızca bir Windows istemcisinden çalışırlar. Diğer yöntemleri seçmek için bu makalenin başında sekme seçicisini kullanın.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdaki öğe olmalıdır:

* HDInsight, Apache Hadoop kümesi. Daha fazla bilgi için [bir küme ve SQL veritabanı oluşturma](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="use-sqoop-on-hdinsight-clusters-with-the-net-sdk"></a>.NET SDK ile HDInsight kümelerinde Sqoop'u kullanma
Böylece .NET HDInsight kümeleriyle çalışmak daha kolay HDInsight .NET SDK'sı .NET istemci kitaplıkları sağlar. Bu bölümde hivesampletable, bu öğreticide daha önce oluşturduğunuz Azure SQL veritabanı tablosuna dışarı aktarmak için bir C# konsol uygulaması oluşturun.

## <a name="submit-a-sqoop-job"></a>Sqoop işi Gönder

1. Visual Studio'da C# konsol uygulaması oluşturun.

2. Visual Studio Paket Yöneticisi konsolundan aşağıdaki NuGet komutunu çalıştırarak paketin içeri aktarın:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job

3. Program.cs dosyasına aşağıdaki kodu kullanın:
   
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
Linux tabanlı HDInsight aşağıdaki sınırlamalar sunar:

* Toplu dışarı aktarın: Microsoft SQL Server veya Azure SQL veritabanı için verileri dışarı aktarmak için kullanılan Sqoop bağlayıcı toplu ekleme şu anda desteklemiyor.

* Toplu işleme: Kullanarak `-batch` anahtarı, Sqoop, toplu INSERT işlemler yerine birden çok ekler gerçekleştirir.

## <a name="next-steps"></a>Sonraki adımlar
Artık Sqoop kullanmayı öğrendiniz. Daha fazla bilgi için bkz:

* [HDInsight ile Apache Oozie kullanma](../hdinsight-use-oozie-linux-mac.md): İçinde bir Oozie iş akışının Sqoop eylemini kullanın.
* [HDInsight için verileri karşıya](../hdinsight-upload-data.md): HDInsight veya Azure Blob depolamaya veri yüklemek için diğer yöntemler bulun.

