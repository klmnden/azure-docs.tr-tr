---
title: ".NET SDK'sı - Azure hdınsight'ta Hadoop kümelerini yönetme | Microsoft Docs"
description: "Hdınsight .NET SDK kullanarak hdınsight'ta Hadoop kümeleri için yönetim görevlerini gerçekleştirmek öğrenin."
services: hdinsight
editor: cgronlun
manager: jhubbard
tags: azure-portal
author: mumian
documentationcenter: 
ms.assetid: fd134765-c2a0-488a-bca6-184d814d78e9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: jgao
ms.openlocfilehash: d881d47e26460d3fff89c01245bba4c608dc8b08
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a>.NET SDK kullanarak hdınsight'ta Hadoop kümelerini yönetme
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Kullanarak Hdınsight kümelerini yönetme öğrenin [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).

**Önkoşullar**

Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="connect-to-azure-hdinsight"></a>Azure Hdınsight Bağlan

Aşağıdaki NuGet paketlerini gerekir:

```
Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
Install-Package Microsoft.Azure.Management.ResourceManager -Pre
Install-Package Microsoft.Azure.Management.HDInsight
```

Aşağıdaki kod örneği, Azure aboneliğinizin altında Hdınsight kümeleri yönetebilmeniz için önce Azure'a bağlanmak nasıl gösterir.

```csharp
using System;
using Microsoft.Azure;
using Microsoft.Azure.Management.HDInsight;
using Microsoft.Azure.Management.HDInsight.Models;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;

namespace HDInsightManagement
{
    class Program
    {
        private static HDInsightManagementClient _hdiManagementClient;
        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId; 
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";

        static void Main(string[] args)
        {
            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            // insert code here

            System.Console.WriteLine("Press ENTER to continue");
            System.Console.ReadLine();
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                ClientId, 
                new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                PromptBehavior.Always, 
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }
    }
}
```

Bu programı çalıştırdığınızda bir istem göreceksiniz.  İstemi görmek istemiyorsanız, bkz: [etkileşimli olmayan kimlik doğrulama .NET Hdınsight uygulamaları oluşturma](hdinsight-create-non-interactive-authentication-dotnet-applications.md).

## <a name="create-clusters"></a>Küme oluşturma
Bkz: [.NET SDK kullanarak Hdınsight oluşturma Linux tabanlı kümelerde](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)

## <a name="list-clusters"></a>Liste kümeleri
Aşağıdaki kod parçacığını kümeleri ve bazı özellikler listelenmektedir:

```csharp
var results = _hdiManagementClient.Clusters.List();
foreach (var name in results.Clusters) {
    Console.WriteLine("Cluster Name: " + name.Name);
    Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
    Console.WriteLine("\t Cluster location: " + name.Location);
    Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
}
```

## <a name="delete-clusters"></a>Küme silme
Aşağıdaki kod parçacığını eşzamanlı veya zaman uyumsuz olarak bir küme silmek için kullanın: 

```csharp
_hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
_hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");
```

## <a name="scale-clusters"></a>Kümeleri ölçeklendirme
Özellik ölçeklendirme küme kümeye yeniden oluşturmak zorunda kalmadan Azure Hdınsight'ta çalıştıran bir küme tarafından kullanılan çalışan düğümü sayısını değiştirmenize izin verir.

> [!NOTE]
> Yalnızca, Hdınsight sürüm 3.1.3 ile kümeleri veya üzeri desteklenir. Kümenizin sürümünü emin değilseniz, Özellikler sayfasını kontrol edebilirsiniz.  Bkz: [listesi ve Göster kümeleri](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
> 
> 

Her tür Hdınsight tarafından desteklenen küme için veri düğüm sayısını değiştirme etkisi:

* Hadoop
  
    Sorunsuz bir şekilde tüm bekleyen veya çalışan işler etkilemeden çalıştıran bir Hadoop kümesinde çalışan düğümü sayısını da artırabilirsiniz. İşlemi devam ederken yeni işleri da gönderilebilir. Kümenin her zaman işlevsel bir durumda bırakılır böylece bir ölçeklendirme işlemi hatalar düzgün bir şekilde ele alınır.
  
    Bir Hadoop kümesine veri düğüm sayısını azaltarak ölçeklendirilir, bazı kümedeki hizmetleri yeniden başlatılır. Bu işleri bekleyen tüm çalışan ve ölçeklendirme işlemi tamamlandığında başarısız olmasına neden olur. İşlemi tamamlandıktan sonra ancak, sunmaları olabilir.
* HBase
  
    Sorunsuz bir şekilde ekleyebilir veya çalışırken, HBase kümesi düğümleri kaldırın. Bölgesel sunucular otomatik olarak ölçeklendirme işlemi tamamladıktan birkaç dakika içinde dengeli. Ancak, küme headnode günlüğe kaydetme ve bir komut istemi penceresinden aşağıdaki komutları çalıştırarak el ile de bölgesel sunucular dengeleyebilirsiniz:
  
    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```
* Storm
  
    Sorunsuz bir şekilde ekleyebilir veya çalışırken Storm kümeniz veri düğümleri kaldırın. Ancak ölçeklendirme işlemi başarıyla tamamlandıktan sonra topoloji yeniden dengelemeniz gerekir.
  
    İki yolla yeniden dengelenmesi gerçekleştirilebilir:
  
  * Storm web kullanıcı Arabirimi
  * Komut satırı arabirimi (CLI) aracı
    
    Lütfen [Apache Storm belgelerine](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) daha fazla ayrıntı için.
    
    Hdınsight kümesinde Storm web kullanıcı Arabirimi kullanılabilir:
    
    ![Hdınsight Storm ölçek yeniden dengeleyin](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)
    
    Storm topolojisini yeniden dengelemeniz CLI komutunu kullanma örneği şöyledir:
    
    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

Aşağıdaki kod parçacığını bir küme eşzamanlı veya zaman uyumsuz olarak yeniden boyutlandırma gösterilmektedir:

```csharp
_hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
_hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   
```

## <a name="grantrevoke-access"></a>GRANT/revoke erişim
Hdınsight kümeleri (Bu hizmetlerin tümü için RESTful uç noktaları vardır) aşağıdaki HTTP web hizmetleri vardır:

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Varsayılan olarak, bu hizmetleri için erişim verilir. İptal etme / erişim izninin. İptal etmek için:

```csharp
var httpParams = new HttpSettingsParameters
{
    HttpUserEnabled = false,
    HttpUsername = "admin",
    HttpPassword = "*******",
};
_hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);
```

Vermek için:

```csharp
var httpParams = new HttpSettingsParameters
{
    HttpUserEnabled = enable,
    HttpUsername = "admin",
    HttpPassword = "*******",
};
_hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);
```

> [!NOTE]
> Verme/erişimi iptal ederek, küme kullanıcı adı ve parola sıfırlanır.
> 
> 

Bu, Portal üzerinden de yapılabilir. Bkz: [yönetmek Azure portalını kullanarak Hdınsight][hdinsight-admin-portal].

## <a name="update-http-user-credentials"></a>HTTP kullanıcı kimlik bilgilerini güncelleştirin
Yordamın aynısını olan [Grant/revoke HTTP erişimi](#grant/revoke-access). Küme HTTP erişim verilmişse, öncelikle iptal gerekir.  Ve ardından yeni HTTP kullanıcı kimlik bilgileriyle erişim verin.

## <a name="find-the-default-storage-account"></a>Varsayılan depolama hesabı bulunamadı
Aşağıdaki kod parçacığını varsayılan depolama hesabı adı ve bir küme için varsayılan depolama hesabı anahtarı alma gösterir.

```csharp
var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
foreach (var key in results.Configuration.Keys)
{
    Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
}
```

## <a name="submit-jobs"></a>İşlerini gönderme
**MapReduce işleri göndermek için**

Bkz: [hdınsight'ta Hadoop MapReduce çalıştırma örnekleri](hadoop/apache-hadoop-run-samples-linux.md).

**Hive işlerini göndermek için** 

Bkz: [.NET SDK kullanarak Hive sorgularını çalıştırma](hadoop/apache-hadoop-use-hive-dotnet-sdk.md).

**Pig işleri göndermek için**

Bkz: [.NET SDK kullanarak çalıştırmak Pig işleri](hadoop/apache-hadoop-use-pig-dotnet-sdk.md).

**Sqoop işlerini göndermek için**

Bkz: [Hdınsight ile Sqoop kullanma](hadoop/apache-hadoop-use-sqoop-dotnet-sdk.md).

**Oozie işlerini göndermek için**

Bkz: [tanımlamak ve Hdınsight'ta bir iş akışını çalıştırmak için Hadoop ile kullanım Oozie](hdinsight-use-oozie-linux-mac.md).

## <a name="upload-data-to-azure-blob-storage"></a>Azure Blob depolama alanına veri yükleme
Bkz. [HDInsight'a veri yükleme][hdinsight-upload-data].

## <a name="see-also"></a>Ayrıca Bkz.
* [Hdınsight .NET SDK'sı başvuru belgeleri](https://msdn.microsoft.com/library/mt271028.aspx)
* [Hdınsight Azure Portalı'nı kullanarak yönetme][hdinsight-admin-portal]
* [Bir komut satırı arabirimi kullanarak Hdınsight yönetme][hdinsight-admin-cli]
* [Hdınsight kümeleri oluşturma][hdinsight-provision]
* [HDInsight'a veri yükleme][hdinsight-upload-data]
* [Azure HDInsight'ı Kullanmaya Başlama][hdinsight-get-started]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]:hadoop/submit-apache-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-portal-linux.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md


